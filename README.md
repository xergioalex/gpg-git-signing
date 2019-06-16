# GPG git signing

Instructions that explain how to generate GPG RSA keys to allow sing git commits.

### 1. Verify gpg instalation:

```shell
which gpg
```

If it doesn't exist, we install it with the following command:

```shell
sudo apt-get install gnupg
```

### 2. Generate GPG RSA Keys

```shell
gpg --gen-key
```

Just follow the instructions provided  by the shell menu:
```
gpg (GnuPG) 1.4.20; Copyright (C) 2015 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
Your selection? 1
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048)
Requested keysize is 2048 bits
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 1y
Key expires at Sun Jun 14 12:17:54 2020 -05
Is this correct? (y/N) y

You need a user ID to identify your key; the software constructs the user ID
from the Real Name, Comment and Email Address in this form:
    "Heinrich Heine (Der Dichter) <heinrichh@duesseldorf.de>"

Real name: Heinrich Heine
Email address: heinrichh@duesseldorf.de
Comment: Der Dichter
You selected this USER-ID:
    "Heinrich Heine (Der Dichter) <heinrichh@duesseldorf.de>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
You need a Passphrase to protect your secret key.
```

Finally the key must be generated with an output like:

```shell
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
......+++++

Not enough random bytes available.  Please do some other work to give
the OS a chance to collect more entropy! (Need 114 more bytes)
.............+++++
gpg: key 7110CE00 marked as ultimately trusted
public and secret key created and signed.

gpg: checking the trustdb
gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
gpg: next trustdb check due at 2020-06-14
pub   2048R/3423CE33 2019-06-15 [expires: 2020-06-14]
      Key fingerprint = 432B A5B0 5423 5467 CE00 3333  FCE2 7F45 593F 7110
uid                  Heinrich Heine (Der Dichter) <heinrichh@duesseldorf.de>
sub   2048R/43564534 2019-06-15 [expires: 2020-06-14]
```

### 3. Problems generating the keys?

It's posible get the following error the firts time that you tried to generate a GPG key:
```shell
..+++++
.+++++
gpg: no writable public keyring found: eof
Key generation failed: eof
```

The above error is due to a permissions problem, to fix it just exec following command and try again:
```shell
sudo chown -R myuser:myuser ~/.gnupg
```

#### 4. Get public key

You will need the public key to store it on github or gitlab if you want sing your commits.

Get gpg keys:
```shell
gpg --list-keys
```

Output:
```
/home/myuser/.gnupg/pubring.gpg
-----------------------------------
pub   2048R/PUBKEYXX 2019-06-15 [expires: 2020-06-14]
uid                  Heinrich Heine (Der Dichter) <heinrichh@duesseldorf.de>
sub   2048R/233XXXXX 2019-06-15 [expires: 2020-06-14]
```

Get public key:
```
gpg --armor --export PUBKEYXX
```

Output:
```
-----BEGIN PGP PUBLIC KEY BLOCK-----
Version: GnuPG v1

6IEdhbGVhbm8gKEdQRyBLZXkgZm9yIFhlcmdpb0FsZVggRGV2T3Bzbyb5laXguDf
6IEdhbGVhbm8gKEdQRyBLZXkgZm9yIFhlcmdpb0FsZVggRGV2T3Bzbyb5laXguDf
6IEdhbGVhbm8gKEdQRyBLZXkgZm9yIFhlcmdpb0FsZVggRGV2T3Bzbyb5laXguDf
...
6IEdhbGVhbm8gKEdQRyBLZXkgZm9yIFhlcmdpb0FsZVggRGV2T3Bzbyb5laXguDf
6IEdhbGVhbm8gKEdQRyBLZXkgZm9yIFhlcmdpb0FsZVggRGV2T3Bzbyb5laXguDf
6IEdhbGVhbm8gKEdQRyBLZXkgZm9yIFhlcmdpb0FsZVggRGV2T3Bzbyb5laXguDf
9w==
=qtS2
-----END PGP PUBLIC KEY BLOCK-----
```

#### 5. Store GPG Key on Github && GitLab

* Go to [GitHub Settings](https://github.com/settings), then click on `SSH and GPG keys` option at side bar menu. Finally add your key clicking on **New GPG key** button.
* Go to [GitLab Settings](https://gitlab.com/profile), then click on `GPG Keys` option at side bar menu. Finally add your key clicking on `Add Key`.



#### 6. Sign git commmits

Finally it's posible sign your commit using **-S** option:
```
git commit -S -am 'signed commit'
```