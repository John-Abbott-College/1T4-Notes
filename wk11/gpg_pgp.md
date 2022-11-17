These notes will help you complete assignment 4/5, where I will require you to encrypt/digitally sign some of your submissions.

# Creating a GPG key pair

Much of this tutorial is (in part) based off the DigitalOcean tutorial linked in last week's slides. [See here](https://www.digitalocean.com/community/tutorials/how-to-use-gpg-to-encrypt-and-sign-messages). Have a read through it if you are ever stuck and to get the big picture of what we're doing.

GPG is pre-installed on most Linux distributions; these days, it is also pre-installed on Windows via **Git Bash**. 

Open Git Bash (press the windows key and search). Check the gpg version to ensure that it is indeed installed:

```
gpg --version
```

You should see some output indicating that gpg is installed (likely version >2.2). Let's create a key:

```
gpg --gen-key
```

Answer the questions asked as follows: key RSA, keysize 4096, key valid for 1y, realname (your real name), email address (your real email address), leave comment empty, O for okay, enter a password that you will remember two weeks from now (hint: try bitwarden or your browser password manager).

Test that your key was generated correctly by running the following commands:

```
gpg --list-keys
gpg --list-secret-keys
```

# Creating a Revocation Certificate

GPG public keys are designed to be shared with others, building a "web of trust" that the digital key truly represents the user behind it. Whenever keys are lost/destroyed/rotated, a **revocation certificate** is used to publically declare that the key is no longer valid.

We will practise revoking keys in assignment 5. However, it is always a good idea to immediately generate a revocation key at the same time as creating your key, in case you need it earlier than you expect. Run the following command:

```
gpg --output ~/revocation.crt --gen-revoke your_email@address.com
```

This will create the file **revocation.crt** at the path specifed (in the example, ~/, which is your home directory)

# Exporting your keys to use on another machine

Run the following commands to export your keys to files, which we can transfer to OneDrive or another machine that you have access to.

Replace **user-id** with the email address you used to create your key:
```
gpg --armor --export user-id > ~/pubkey.asc
gpg --export-secret-keys --armor user-id > ~/privkey.asc
```

This will create the file **pubkey.asc** at the path specifed (in the example, ~/, which is your home directory).

**I recommend storing your pubkey.asc, privkey.asc, and revocation.crt files in OneDrive, so you can use them on other machines.** 

Note, in order to use your keys on another machine, you will also need to import both keys to the gpg application on that other machine. Instructions for this follow.

# Importing a GPG key

## Importing a public key from a file
To import a public key that someone has shared with you into your GPG key ring, run the following command:



## Importing a public key from a keyserver

If the public key you are intersted in is uploaded to a key server, you can import it from key server. Use the following command to search public keys on the keyserver. User ID is the recipient’s email address.

To specify a particular key server, us the --keyserver option like below.

Try the following command with the user-id **michael.haaf@johnabbott.qc.ca**. That's my public key!


```
gpg --search user-id

gpg --keyserver hkps://keys.openpgp.org --search user-id
```

# Encrypting a file with a GPG public key
To 
```
gpg --encrypt --sign --armor -r person@email.com name_of_file
```


# Decrypting a file with your GPG private key
When you receive a message encrypted with your public key, simply call GPG on the encryped file. The software will prompt you as necessary. **You will need your key password in order to use your private key**.

```
gpg file_name.asc
```
