# Signed Commits - a case study

Signing commits drastically hardens your repository against attempts to commit malicious code, e.g. via commit spoofing.Commit signing in addition to some features of GitHub, such as [Vigilante mode](https://docs.github.com/en/authentication/managing-commit-signature-verification/displaying-verification-statuses-for-all-of-your-commits), can help you make sure to only accept verified commits by contributors you know and trust. 

This repository attempts to serve as a demo of the "look 'n feel" of commit signing on GitHub, and the different possibilities to do so.

## 1. Commit - signed by a valid user

The first commit is an authentic one:

- it was drafted by the **repository's owner** (`cloudnativeclutter`) from his **verified email address** (`github@cloudnativeclutter.com`)
- it has been signed by a GPG key which has been **linked** to the author's account.

You can have a look at the **commit history** of a repository at all times by navigating to the `Commits` tab in the top of the repo, or [following this link](https://github.com/cloudnativeclutter/signed-commits-case-study/commits/main).

## 2. Commit - signed with a non-verified GPG key

The second commit is a bit weird:

- it seems to originate from `cloudnativeclutter`
- **but** the GPG key is not verified?

Maybe the user just messed up his key management, or he forgot to **upload his public gpg signing key** for this key. Mmaybe it's a quite intricate attempt of getting malicious code in - in general it's better do decline commits like this!
