This is a tutorial for anyone who is new to GPG and wants or needs to use it quickly.

### Installing GPG
#### Windows
Install Gpg4win (https://www.gpg4win.org/get-gpg4win.html)

#### Linux
``` 
# For Debian based distributions
sudo apt install gpg

# For RHEL based distributions
sudo dnf install gpg
```

#### Mac
``` brew install gpg ```

### Creating your keypair
1. Type this into the command line: ``` gpg --full-generate-key ```
2. Select the default - *(1)RSA and RSA*
3. For the key size, enter 4096
4. For the expiration date, enter 0 unless you're a target and need to rotate keys. If you would like to set an expiration date
	- Enter a number followed by nothing if you mean X amount of days (e.g. ```5```)
	- Enter a number followed by <b>w</b> if you mean X amount of weeks (e.g. ```5w```)
	- Enter a number followed by <b>m</b> if you mean X amount of weeks (e.g. ```5m```)
	- Enter a number followed by <b>y</b> if you mean X amount of years (e.g. ```5y```)
5. When asked, the real name and comment can be left blank, although I would enter the email address used when asked.
6. Enter O and hit enter.
7. Enter the password that you would like to use. Please make this strong and unique (in other words, not used elsewhere). If your private key is stolen, a strong password is your second line of defense. If it can be easily brute forced or guessed, all previous GPG-encrypted communication can be decrypted by the adversary.
8. If it says "We need to generate a lot of random bytes...", move your mouse around until it completes.
9. There should be a line that says "gpg: revocation certificate stored as...". Read that file and get familiar with it. There are instructions in there to be aware of in case your private key is lost or compromised. If you don't see this section, you can generate a revocation certificate using the following command: ``` gpg --gen-revoke paste_long_string_here ``` (you will be prompted for your password). Store that revocation certificate somewhere safe.
10. Now that it's been created, type the following and make note of your fingerprint: ```gpg --list-keys```
The fingerprint is the long string of numbers and letters under "pub". Copy all of that for the next step.

### Exporting your keypair
1. Export your public key by typing the following: ``` gpg --export --armor paste_long_string_here > ~/public_key.asc ```
2. Export your private key (you will be prompted for your password) by typing the following: ``` gpg --export-secret-keys --armor paste_long_string_here > ~/private_key.asc ```
3. Be very careful about where you keep that private key. This is the linchpin  linchpin that holds together the security of the encryption process and your privacy.

### Importing and verifying someone else's keypair
1. You can import someone else's public key using the following command: ``` gpg --import other_public_key.asc ```
2. If a fingerprint is provided by the owner ahead of time, you can verify it using the following command: ``` gpg --fingerprint <email_address> ```

### Encrypting a message
1. To send an encrypted message, type the following command: ``` gpg --encrypt --recipient <email_address> -o encrypted_message.gpg << EOF ```
2. On the new line, start writing your message. Once you're finished, on a new line, type ``` EOF ``` and hit enter. This will generate a file named *encrypted_message.gpg*. That's your encrypted message. Send that to the intended recipient.

NOTE: You can also encrypt files like images, documents, etc using the following command:
``` gpg --encrypt --recipient <email_address> -o encrypted_file.gpg file_to_encrypt.jpg ```


### Decrypting a message
1. Once you've downloaded the encrypted file, use the following command pointing to the location of the file: ``` gpg --decrypt encrypted_file.gpg ```

### Revoking you keypair
1. If the original key generation process created a revocation certificate for you, it will have some very interesting text inside:
```
This is a revocation certificate for the OpenPGP key:

pub   ---
      ---
uid          ---@---.---

A revocation certificate is a kind of "kill switch" to publicly
declare that a key shall not anymore be used.  It is not possible
to retract such a revocation certificate once it has been published.

Use it to revoke this key in case of a compromise or loss of
the secret key.  However, if the secret key is still accessible,
it is better to generate a new revocation certificate and give
a reason for the revocation.  For details see the description of
of the gpg command "--generate-revocation" in the GnuPG manual.

To avoid an accidental use of this file, a colon has been inserted
before the 5 dashes below.  Remove this colon with a text editor
before importing and publishing this revocation certificate.

:-----BEGIN PGP PUBLIC KEY BLOCK-----
Comment: This is a revocation certificate

...

-----END PGP PUBLIC KEY BLOCK-----

```

If this wasn't created, there were steps above for generating your own. To revoke your keypair, use the following command with the filename of the revocation certificate:
``` gpg --import revocation_certificate.asc ```
Remember, replace the filename with that of the revocation certificate.

### Conclusion
That's it. Obviously there are a lot more options and capbilities that weren't mentioned here (see https://www.gnupg.org/documentation/). This was just meant as a quick and relatively painless intro. Be sure to take time to learn more about best practices. A few things to consider are
- Signing public keys
- How to securely store your private key
- Setting up a secure environment to encrypt and decrypt messages in
- Know your threat landscape and attack surface.

