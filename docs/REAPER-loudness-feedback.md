# Feedback to Cockos / REAPER: Offline Loudness Export Discrepancies

**Product:** REAPER (Batch Converter / Statistics / loudness export columns)  
**Date:** 2026-08-12  
**Severity:** High (affects QC, delivery, and speech/SFX loudness decisions)  
**Reporter context:** Cross-checked against ITU-R BS.1770 / EBU R128 reference behaviour (ffmpeg `ebur128` / libebur128)

---

## Summary

When exporting or listing loudness for files (especially **short mono** speech / SFX), REAPER’s **LUFS-I** can read **~2.6–5.5 LU higher** than a standards-conformant integrated loudness measurement of the same PCM.

That offset is large enough to reverse loudness QC decisions (pass/fail against −23 / −16 / −14 LUFS targets, relative matching between takes, etc.).

We also observe that exported **LUFS-M** / **LUFS-S** are **end-of-scan meter snapshots**, not whole-file statistics — which is easy to misread as “file loudness” in a table next to LUFS-I.

---

## Why this matters (speech / dialogue / SFX workflows)

In speech editing and sample-library QC, people commonly:

1. Batch-export loudness from REAPER  
2. Sort / filter / normalize / deliver based on **LUFS-I**  
3. Compare REAPER numbers to other meters (ffmpeg, libebur128, dedicated loudness tools, DAW competitors)

If REAPER’s LUFS-I is systematically hotter on mono shorts:

- Takes may be rejected or approved incorrectly  
- Dialogue stems may be leveled to the wrong target  
- Cross-tool handoff (“REAPER said −18.7, the other tool says −21.3”) destroys trust and burns time  

For **short mono one-shots** (typical game / Foley / VO punch-ins under ~1 s), the error is especially large and **file-dependent** (not a fixed calibration offset), so it cannot be corrected by a single global trim.

---

## Reproducible observation

### Test material

- Mono, 24-bit, 48 kHz WAV one-shots (weapon / body hits), duration ~0.55–1.13 s  
- Same files measured in:
  - **REAPER** loudness / batch statistics export (columns Peak, LUFS-M, LUFS-S, LUFS-I)
  - **ffmpeg** `ebur128` (libebur128):  
    `ffmpeg -i file.wav -filter_complex ebur128=peak=true -f null -`
  - Independent BS.1770 reimplementation matching ffmpeg within ≤0.05 LU on LUFS-I

### Example (same file)

| File | ffmpeg / BS.1770 LUFS-I | REAPER LUFS-I | Δ |
|---|---:|---:|---:|
| `…Axe_Hit_HitBody_01.wav` | −25.6 | −21.1 | **+4.5 LU** |
| `…Bow_Hit_HitBody_01.wav` | −21.3 | −18.7 | **+2.6 LU** |
| `…Bullet_Hit_HitBody_01.wav` | −25.0 | −20.7 | **+4.3 LU** |
| `…Spear_Hit_HitBody_01.wav` | −26.9 | −23.2 | **+3.7 LU** |

ffmpeg also reports the absolute/relative gate threshold consistently with the reference implementation (e.g. Axe_01: `I −25.6`, `Threshold −39.2`).

---

## Root-cause analysis (two independent effects)

Both effects were reproduced numerically; together they match REAPER’s LUFS-I within ~0.3 LU average / ~0.6 LU max on a 20-file sample.

### Issue A — Mono treated as dual-mono (+3.01 LU fixed)

These files are **1-channel**.

BS.1770 integrated loudness for mono uses **one** channel with weight 1.0.

REAPER’s export path appears to measure the file as if the same mono signal is present on **both** sides of a stereo bus (dual-mono). K-weighted power doubles → exactly:

\[
10\log_{10}(2) \approx +3.01\ \mathrm{LU}
\]

**Evidence:** REAPER’s exported **LUFS-M** and **LUFS-S** match a standards momentary/short-term calculation **after** adding +3.01 LU (errors ~0.05–0.13 LU across the sample). That strongly indicates a stereo-path / dual-mono weighting, not a K-filter coefficient bug.

**Impact:** Every mono file is reported **~3 LU too loud** relative to file-as-stored BS.1770, even before Issue B.

### Issue B — Partial / zero-padded gating blocks inflate LUFS-I (+0 … ~2.5 LU, content-dependent)

BS.1770 integrated loudness is based on **complete 400 ms** gating blocks (typically 75% overlap / 100 ms hop), then absolute (−70 LUFS) and relative (−10 LU) gating.

REAPER’s offline table values behave like a **streaming meter dump**:

- Blocks are emitted from ~100 ms onward  
- Incomplete windows are **zero-padded** and still normalized by a full 400 ms denominator  
- For one-shots whose **transient sits at sample 0**, those early padded windows each contain nearly the **entire transient** at almost full momentary level  
- Those windows pass the relative gate and are averaged into integrated loudness, **re-counting the same transient multiple times**

Example reconstruction for `Axe_Hit_HitBody_01` (dual-mono–aligned display):

```
Complete blocks only (BS.1770-style):
  0..400 ms   -19.3  KEEP
  100..500    -23.9  KEEP
  200..600    -30.3  KEEP
  → LUFS-I ≈ -22.55 (dual-mono) / -25.56 (true mono)

Streaming + leading zero-pad (REAPER-like):
  -300..100   -21.1  KEEP   ← padded; still full transient
  -200..200   -19.7  KEEP
  -100..300   -19.3  KEEP
  0..400      -19.3  KEEP
  …
  → LUFS-I ≈ -21.08   (matches REAPER -21.1)
```

**Impact:** Extra inflation depends on where energy sits in the file. Attack-at-start SFX/VO punches are worst-case; material with later onsets may show a smaller Issue B contribution. Combined with Issue A, total error is **not a constant** → dangerous for batch QC.

### Related UX issue — LUFS-M / LUFS-S columns on long files

For a **3-minute** piece, exported LUFS-M / LUFS-S necessarily reflect only the **last ~400 ms / ~3 s** of the scan (sliding windows), not the programme as a whole.

Placing them beside LUFS-I in an export table strongly implies “file metrics,” but M/S are **end-of-file meter snapshots**. For speech reels this can be especially misleading (e.g. quiet room tone or fade at the end vs loud dialogue earlier).

**Suggestion:** Label clearly (e.g. “LUFS-M (last)”) or omit M/S from whole-file export unless the user requests max/mean/histogram statistics.

---

## Expected behaviour (standards)

For **file loudness export** of a mono WAV:

1. **LUFS-I** should match mono BS.1770 / EBU R128 integrated loudness of the **stored channel layout** (1 channel), agreeing with libebur128 / ffmpeg `ebur128` within normal numerical tolerance.  
2. If REAPER intentionally measures through a stereo monitoring path, the UI/export should state that explicitly (e.g. “dual-mono / stereo bus LUFS”), and ideally offer a **“file layout / BS.1770 file loudness”** mode for offline QC.  
3. Integrated loudness should use **complete** 400 ms gating blocks (or an equivalent method proven to match the standard), not leading zero-padded partial windows that re-weight onsets.  
4. Column names should not present end-of-scan M/S as if they were whole-file descriptors.

---

## Requested fixes / clarification

1. **Confirm** whether offline LUFS-I is intentionally dual-mono / master-bus weighted for mono files.  
2. If yes: add a **File / BS.1770** measurement mode for Batch Converter / statistics that matches libebur128 on mono and multi-channel layouts.  
3. Fix or document gating for short files so LUFS-I does not include leading zero-padded partial blocks that bias onset-heavy content.  
4. Clarify export column semantics for LUFS-M / LUFS-S (last window vs whole-file aggregate).  
5. Optionally document Peak vs True Peak (dBTP) naming if the Peak column is sample peak rather than BS.1770 true peak.

---

## Minimal reproduction steps

1. Take a short **mono** WAV with a strong transient at the start (or use any of the files above).  
2. Measure with:  
   `ffmpeg -hide_banner -i input.wav -filter_complex ebur128=peak=true -f null -`  
   Note **Integrated loudness I**.  
3. Run the same file through REAPER Batch Converter / loudness statistics export.  
4. Compare **LUFS-I**. Expect REAPER ≈ ffmpeg + ~3 LU, often more on attack-at-start shorts.  
5. Optional: convert the same file to **stereo identical L/R** and re-measure in a strict mono-aware meter vs REAPER to isolate dual-mono weighting.

---

## Environment notes

- OS: Windows 10/11 x64  
- Files: 48 kHz, 24-bit PCM, 1 channel  
- Reference: ffmpeg ebur128 / libebur128 (Integrated loudness + threshold)  
- Independent reimplementation of K-weighting + gating matched ffmpeg to ≤0.05 LU on I

Happy to provide the WAV set and a CSV of side-by-side numbers if useful.

---

## Chinese summary（给报告者自己用）

**问题：** REAPER 离线导出的 **LUFS-I** 对短 **单声道** 语音/音效会比标准 BS.1770（ffmpeg ebur128）偏高约 **2.6–5.5 LU**，足以影响响度质检和交付判断。

**原因 1：** 单声道被按立体声双单声道测量 → 固定 **+3.01 LU**。  
**原因 2：** 流式仪表把开头补零的不完整 400 ms 门控块也计入 integrated，瞬态在文件开头时会被重复计入 → 再抬高 **0–2.5 LU**（随内容变化）。  

**附带：** 导出表里的 LUFS-M/S 对长节目只是扫完时最后窗口的快照，不是整曲响度，容易误读。

**诉求：** 请确认/修复口径，或提供与 libebur128 一致的「按文件声道布局」的离线 LUFS-I；并明确 M/S 列含义。
