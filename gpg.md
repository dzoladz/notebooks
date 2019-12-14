GPG - Verified Git Commits
==========================

1. Get `gpg` command
```bash
brew install gnupg
```

2. Generate GPG key
```bash
gpg --full-generate-key
```

3. List GPG keys
```bash
gpg --list-secret-keys
```

4. Copy the GPG key ID that starts with `sec`. In the following example,
   `17369B171FFA76349D07488652B7FB6A368AAF3B`:
```bash
/Users/derek/.gnupg/pubring.kbx
-------------------------------
sec   rsa4096 2019-12-14 [SC]
      17369B171FFA76349D07488652B7FB6A368AAF3B
uid           [ultimate] Derek Zoladz (None) <derek@derekzoladz.com>
ssb   rsa4096 2019-12-14 [E]
```

5. Export the public key of that ID
```bash
gpg --armor --export 17369B171FFA76349D07488652B7FB6A368AAF3B
```

6. Copy the public GPG key and it to both GitLab and GitHub accounts

7. Tell Git to use your GPG key to sign commits
```bash
git config --global user.signingkey 17369B171FFA76349D07488652B7FB6A368AAF3B
 ```

8. Tell Git to sign your commits automatically, to avoid using the `-S` flag with every
commit
```bash
git config --global commit.gpgsign true
```
