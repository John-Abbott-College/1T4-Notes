These notes will help you complete assignment 4/5, where I will require you to
encrypt/digitally sign some of your submissions.

# GPG Key Creation and Usage Guide

I have adapted this guide from a variety of online resources
([see here](https://www.digitalocean.com/community/tutorials/how-to-use-gpg-to-encrypt-and-sign-messages)
for example).

You should be able to follow the steps of this guide from scratch, without any
existing keys, and end with a functioning public/private key pair that can
encrypt/decrypt/sign/verify documents. You will have practise doing each of
these actions in the steps below.

You should also be able to jump to any section and perform commands that you
wish without issue, if you do have a valid public/private key available to use.

> **NOTE:** The keys that we are making in class are for educational purposes
> only. There are a few steps we will take that I do not recommend in general.
> If you are interested in using GPG keys after this course, I would recommend
> starting fresh on your own computer and configuring a secure place to back up
> your keys.
> [Here](https://riseup.net/en/security/message-security/openpgp/best-practices) > [are](https://github.com/drduh/YubiKey-Guide) > [some](https://www.gnupg.org/gph/en/manual/book1.html) > [example](https://wiki.debian.org/Subkeys) > [references](https://wclarke.net/posts/2022-01-05--how-to-share-private-gpg-keys-securely.html)
> I have used for this myself (each word is a separate link).

## Setup

GPG is pre-installed on most Linux distributions; these days, it is also
pre-installed on Windows via **Git Bash**. If it is not installed on the machine
you want to use it on, it is generally straightforward to install. See here for
some examples:
[Windows](https://www.educative.io/answers/how-to-install-git-bash-in-windows)
[Mac](https://formulae.brew.sh/formula/gnupg)
[Linux](https://gnupg.org/download/). On our lab computers, we will use Git
Bash.

Open Git Bash (press the windows key and search). Check the gpg version to
ensure that it is indeed installed:

```
gpg --version
```

You should see some output indicating that gpg is installed (likely version

> 2.2).

## Creating a GPG key

Let's create a key:

```
gpg --gen-key
```

Answer the questions asked as follows: key RSA, keysize 4096, key valid for 1y,
realname (your real name), email address (your real email address), leave
comment empty, O for okay, enter a password that you will remember two weeks
from now (hint: try bitwarden or your browser password manager).

Test that your key was generated correctly by running the following commands:

```
gpg --list-keys
gpg --list-secret-keys
```

## Creating a Revocation Certificate

GPG public keys are designed to be shared with others, building a "web of trust"
that the digital key truly represents the user behind it. Whenever keys are
lost/destroyed/rotated, a **revocation certificate** is used to publically
declare that the key is no longer valid.

If your key is ever made publically available, it is usually impossible to
permanently "deleted" that key. If your key is compromised or should no longer
be trusted, the only way to communicate this with the world is to _revoke_ your
key. Even if we do not intend to share our key, it is "better safe than sorry"
to just create the cert before forgetting about it in case we do end up needing
it.

```
gpg --output ~/revocation.crt --gen-revoke your_email@address.com
```

This will create the file **revocation.crt** at the path specifed (in the
example, ~/, which is your home directory).

Your key will not be revoked when the certificate is created (phew!) -- all we
have done is created the ability to revoke our key later if we need. This will
be covered in the future.

# Managing GPG Keys

Our GPG keyring is essentially a directory of files on your lab machine (so, if
you create a key on one machine, it is not teleported to other machines
automatically).

In order to use _other people's public keys_, or in order to use _our own
public/private keys on a new machine_, we need to be able to export and import
GPG keys. These instructions follow below.

## Exporting your keys to use on another machine

Run the following commands to export your keys to files, which we can transfer
to OneDrive or another machine that you have access to.

Replace **user-id** with the email address you used to create your key:

```
gpg --armor --export user-id > ~/pubkey.asc
gpg --export-secret-keys --armor user-id > ~/privkey.asc
```

This will create the file **pubkey.asc** at the path specifed (in the example,
~/, which is your home directory).

**For the keys we use in class**, I recommend storing your pubkey.asc,
privkey.asc, and revocation.crt files in **OneDrive**, so you can easily find
them to be used on other machines.

> NOTE: Storing private keys in plaintext on a server we do not control is NOT a
> good practise. I recommend we do so anyway to make sure they don't get lost;
> it is okay if these keys are compromised since we are just using them to
> learn. At the end of the semester, we will be revoking these keys.

In order to use your keys on another machine, you will also need to import both
keys to the gpg application on that other machine. Instructions for this follow.

## Importing GPG keys from a file

Any key (private or public) can be imported from a file using the
`gpg --import /path/to/key` command.

For example, to import the public and private keys you created before (replace
with example paths with your actual path):

```
gpg --import /path/to/public-key
gpg --import /path/to/private-key
```

To import the public key your friend has shared with you (so you can send them
encrypted messages or verify their signatures):

```
gpg --import /path/to/friends/public-key
```

We can double check that our public and private keys were imported correctly by
running the following two commands respectively:

```
gpg --list-keys
gpg --list-secret-keys
```

## Importing public GPG keys from a keyserver

If the public key you are intersted in is uploaded to a key server, you can
import it from key server.

To specify a particular key server, us the --keyserver option like below.

Try the following command with the user-id **michael.haaf@johnabbott.qc.ca**.
That's my public key!

```
gpg --search user-id

gpg --keyserver hkps://keys.openpgp.org --search user-id
```

Verify that my key was imported. The key should have my name/email address, be
of type _ed25519_, created **2022-11-16**, with a fingerprint that starts with
**1ED3E9**.

```
gpg --list-keys
```

If you already have my key, you can make sure it is up to date with the
following command:

```
gpg --refresh-keys
```

> **Note:** I do not recommend uploading the keys we have created in class to a
> public server. I have done so to make my key easier to acquire, and because I
> am prepared to ensure that the private keys are secure. Having a compromised
> public key can be a hassle, we might as well avoid it.

# Encrypting and Decrypting files with GPG keys

## Encrypting with a Public Key

Once you have imported someone else's public key, you can encrypt any file using
the following command: (replace person @email.com and name_of_file)

```
gpg --encrypt --sign --armor -r person@email.com name_of_file
```

A file `name_of_file.asc` will be created. Take a look at it in notepad or any
other text-viewing application.

The only person who can decrypt this file is the owner of the corresponding
private key. The steps for decryption follow.

## Decrypting with your GPG Private key

When you receive a message encrypted with your public key, simply call GPG on
the encryped file. The software will prompt you as necessary. **You will need
your key password in order to use your private key**. Passwords are the last
line of defence in case your private key is made available to someone else.

```
gpg file_name.asc
```

The original file `file_name` should be created and contain the original
message.

# Digital signatures and verification with GPG keys

In the previous sections, we have focused on using GPG to encrypt data: that is,
we ensure that only the owner of a private key is **authorized** to view the
contents of a file encrypted with the corresponding public key, giving us the
ability to share data with end to end asymmetric encryption.

Another way we can use GPG, and asymmetric cryptography more generally, is to
**authenticate** our communication. This is useful even for non-secret
communication: when you download a file from the internet and it is written to
your storage, can you guarantee that the file has remained in its original
state? In fact, there is no guarantee: there are both natural (bit rot, service
outage, electrical interference) and artificial (software bugs, intentional
attacks) events that can occur which cause data to be corrupted or changed
before reaching its intended recipient. Is it possible to detect whether a file
has been changed from its original state?

> Note: in Tech Support, think about how frustrating it has been that sometimes
> the Windows/CentOS ISOs we use don't seem to work. These ISO files do come
> with digital signatures that can be verified -- next time your ISO isn't
> working, you can follow this guide to verify that the file hasn't been
> corrupted.

In the real world, we often make people prove that their documentation is
**authentic** by having them sign a unique signature on the documentation.
Digitally, we can do the same thing by using our **private key** to **sign**
documents, while having recipients check those documents using our **public
key** to check that the document they have received is **authentic**.

This means that with a tool like GPG we can authenticate both (1) that the
sender of the document is who we think they are and (2) that the data they sent
has not been corrupted/modified/tampered. For both reasons, digital signatures
are used in both day-to-day data sharing, as well as for signing off on software
releases/commits/etc.

> Knowledge check: what is the difference between **authorization** and
> **authentication**? See [here]() to read more.

This tutorial is based on the
[GNU Privacy Handbook documentation for producing digital signatures](https://www.gnupg.org/gph/en/manual/x135.html).
It's a nice short tutorial and explains how GPG signatures are used
professionally by developers.

## Signing a file with our private key

Previously, we saw _public keys_ used to encrypt data which only _private keys_
could decrypt.

Digital signatures are the opposite: key owners want to make sure they are the
only people who can sign their files, so they use their password-protected
_private key_ to generate signatures. There are a couple ways to do this:

### Default behavior: sign & compress a file for efficient sharing

The `--sign` parameter provides digital signature functionality. Like
decrypting, **our private key password will be required to give a signature**.

```
gpg --sign name_of_file
```

The document is compressed before being signed, and the output
`name_of_file.gpg` is in binary format (take a look at it in a text editor: it's
not human readable).

We can verify that our signature worked using either of the following commands:

```
gpg --verify name_of_file.gpg     # Verifies the signature
gpg --decrypt name_of_file.gpg    # Verifies sig and extracts compressed content
```

Try editing `name_of_file.gpg` by making random changes and saving them. Once
you've done this, run the verify script again. What output do you get now?

### Alternative 1: embed a plain-text signature in a file

The default case where signed documents are automatically compressed is useful
for software, which often does not need to be human readable to do what it is
supposed to do (.ISO files, applications, etc). We can simply verify the
signature before using the file.

For text documents and messages, however, this extra compression step can be
redundant and confusing. To simply sign a message in plaintext (while still
preserving the ability to verify the signature/contents), use the `clear-sign`
parameter instead (once again, replace `name_of_file` with the file you want to
use):

```
gpg --clearsign name_of_file
```

Take a look at the `name_of_file.asc` file that has been generated. You can
still read the original contents, but there should also be
`PGP Signature/Signed Message` headings in the file as well. We can check that
the file has been signed correctly using the same `--verify` parameter as
before.

```
gpg --verify name_of_file.asc
```

Try editing the content of `name_of_file.asc` and running the verification
again. What output do you see?

### Alternative 2: Signing in a separate file

There are many useful file formats for which Alternative 1 will not work (can
you think of why?). There is another option: we can create a separate signature
file that is sent alongside the original file, while leaving the original file
unmodified.

Use either of the following two commands:

```
gpg --detach-sig name_of_file             # creates a binary signature file
gpg --detach-sig --armor name_of_file     # creates a ASCII plaintext signature file
```

These commands will create a file called `name_of_file.sig`(binary) or
`name_of_file.asc`(ascii) respectively. Try both commands and view both created
files in a text editor to make sure you understand the difference.

To verify the signature, we use the `--verify` option like before, but we
require that **both** the original document and the signature are present:

```
gpg --verify name_of_file.sig name_of_file    # for binary signature
gpg --verify name_of_file.asc name_of_file    # for ascii signature
```

Try editing the content of `name_of_file` and running the verification. What do
you see?

## Verifying a signed document using a public key

In the previous sections, we were verifying our own signatures immediately after
making them. While this does demonstrate how the tools work, it's not actually
very useful -- what we really want is to send our signed documents to others,
and have them verify the signature in case those documents were tampered with
(and vice versa).

We can use all the same commands as in the previous section, with the `--verify`
and `--decrypt` parameters working the same as before. All we need to do is to
specify which _signature_ we want to verify by specifying which _public key_ to
use for verification.

This is done using the `-u` parameter. For example, to verify a document signed
by me, using my public key (assuming it is in your key ring):

```
gpg -u michael.haaf@johnabbott.qc.ca --verify name_of_file
```
