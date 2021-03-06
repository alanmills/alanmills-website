# Encrypting and Decrypting files using PGP with symmetric password

**Author:** [Alan Mills]
**Date:** [6 March 2021 22:11]
**Tags:** [PGP]
**Status**: Draft

From time to time, you will need to send someone sensitive data, like Production security credentials. 

## Encrypting a file with PGP

You will need two things:

1. A file
2. A password

### File - secret.txt

```text
A secret: 56xYEqdShv2D7uA6
```

### Password

Do not use any special characters as this will lead to the error message **gpg: public key decryption failed: Bad passphrase**.

If you use a password generator, like the [Secure Password Generator](https://passwordsgenerator.net), do not include *symbols* and exclude *ambiguous characters*.

```bash
gpg -a --symmetric --cipher-algo AES256 secret.txt
```

When prompted, provide a password, and PGP will generate the file **secret.txt.asc** with the contents.

```bash
-----BEGIN PGP MESSAGE-----

jA0ECQMCHKgfbOfOyh/r0lYBZyV1vOVZo4Y45MjG3OxcQ3VA6aIICcFWgkaeX0cS
DH/kNYvY1OTgTU27J6pVsSApdvXSrGAXfrG3jDwVv231IFPvf9OBWYUhIPexy5GR
Z7ClhVjFww==
=e/LL
-----END PGP MESSAGE----
```

## Decrypting a PGP encrypted file

Now you can use a vulnerable communication tool, like email, to send the **secret.txt.asc** file.  At a minimum, send the **password** in a separate email but ideally send that using another communication tool like *Slack*.

```bash
gpg -a --output secret.txt --decrypt secret.txt.asc
```

## References
* [How to use symmetric (password) encryption with GPG](https://medium.com/@retprogramisto/how-to-use-symmetric-password-encryption-with-gpg-af0d9734d08c)
* [Secure Password Generator](https://passwordsgenerator.net)
* [How to solve “gpg: public key decryption failed: Bad passphrase” in a batch file](https://everyuseful.com/how-to-solve-gpg-public-key-decryption-failed-bad-passphrase-in-batch-file/)