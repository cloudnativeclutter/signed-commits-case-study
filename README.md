# Signed Commits - a case study

Signing commits drastically hardens your repository against attempts to commit malicious code, e.g. via commit spoofing. Commit signing in addition to some features of GitHub, such as [Vigilante mode](https://docs.github.com/en/authentication/managing-commit-signature-verification/displaying-verification-statuses-for-all-of-your-commits), can help you make sure to only accept verified commits by contributors you know and trust. 

This repository attempts to serve as a demo of the "look 'n feel" of commit signing on GitHub, and the different possibilities to do so.

We will look at three ways to sign commits - using `GPG` keys, `SSH` keys, and `gitsign` - and what the results look like on GitHub.

## Using GPG keys

### 1. Commit - signed by a valid user

The first commit is an authentic one:

- it was drafted by the **repository's owner** (`cloudnativeclutter`) from his **verified email address** (`github@cloudnativeclutter.com`)
- it has been signed by a GPG key which has been **linked** to the author's account.

You can have a look at the **commit history** of a repository at all times by navigating to the `Commits` tab in the top of the repo, or [following this link](https://github.com/cloudnativeclutter/signed-commits-case-study/commits/main).

### 2. Commit - signed with a non-verified GPG key

The second commit is a bit weird:

- it seems to originate from `cloudnativeclutter`
- **but** the GPG key is `unverified`?

Maybe the user just messed up his key management, or he forgot to **upload his public gpg signing key** for this key. Maybe it's a quite intricate attempt of getting malicious code in - in general it's better do decline commits like this!

### 3. Commit - Who's mocdaniel?!

The third commit is *definitely* weird:

- despite being created by the *same* account as the previous two commits, it shows **Daniel Bodky** (my main account) as author
- again, the commit is `unverified`, this time because I got **vigilante mode** configured for my account

Spoofing commits is as easy as changing the `user.email` field of your git config, thus changing the email address **GitHub** sees upon committing. Since GitHub is a **collaborative coding platform**, it will try to resolve this email address and display the author's information - totally fooling everyone who doesn't look twice!

## Using SSH keys

### 4. Commit - signed with a verified SSH key

The fourth commit is very similar to the first one:

- it comes from the repository's owner
- it is `verified`

The only difference is a tiny detail: Instead of a **GPG Key ID** we see a **SSH Key Fingerprint** when clicking on the `verified` badge now.

### 5. Commit - signed with an unverified SSH key

Let's go ahead and sign our fifth commit with a non-verified SSH key, similar to commit 2:

- the author is correct
- ...but the signature isn't

Once again, GitHub warns uns that it couldn't verify whether the stated author actually *is* the author, due to the signature not being verified. That's good!

### 6. Commit - We've been blessed by Octocat!

The sixth commit seems off again. Octocat has contributed to this humble guide?

- this time, it's worse - Octocat doesn't seem to use commit signatures!
- the commit seems authentic, and without a `(un)verified` badge, it's hard to tell who wrote this commit.

## Using `gitsign`

The third method I want to show is `gitsign`, a way to do keyless commit signing! If you don't feel like fiddling with  GPG or SSH keys, this might be for you. More information about gitsign as a tool can be found on [the project's repository](https://github.com/sigstore/gitsign).

`gitsign` signs your commits using your very own **GitHub** or **OIDC** entity. All you need to do is to configure your local repository (or global settings). Once configured, upon (to be signed) commit, a browser window will opeb, and you will have to confirm your identity with the configured OIDC service provider - currently, GitHub, Microsoft, and Google are supported.

### 7. A valid signed commit using gitsign

What's that? The heading says it's supposed to be a **valid** commit signature, yet GitHub displays it as `unverified`? This has a reason - `gitsign` takes a different approach to signing commits than the previous methods by using **short-lived** certificates and a signature database called `Rekor` which persists signatures and can be queried in the future to validate existing signatures.

While this is not optimal for GitHub's UI, having a remote authority guaranteeing the authenticity of a signature can be helpful for checks in pipelines, scheduled checks, and audits, as the checking entity doesn't need to know whether a used SSH/GPG key is actually valid in the context of the identity used for the commit.

Nonetheless, the sigstore team is working together with GitHub to eventually be able to display a shiny `verified` badge on your commit histories, too!
