# Fix GitHub Contributors display

GitHub only lists people whose **commit email** matches a verified address on the account.

Past Sonora site commits used `jianchenyang@hypergryph.com`. If that address is not verified on **JANFNASY**, the account may not appear. Cursor may also show up when commits include a `Co-authored-by: Cursor` trailer.

## What you should do (once)

1. Open https://github.com/settings/emails
2. Either:
   - **Add & verify** `jianchenyang@hypergryph.com`, **or**
   - Keep using GitHub’s private noreply address (recommended for public repos)
3. Optional: enable **Block command line pushes that expose my email**

Private noreply form (from your user id):

`226231481+JANFNASY@users.noreply.github.com`

After the email is linked, older commits authored with that address will attribute to you. New commits in this repo are authored as **JANFNASY** with the noreply address.
