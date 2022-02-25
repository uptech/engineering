+++
title = "Git Commit and Tag Signing"
date = 2017-08-04T09:32:01-08:00
updated = 2017-08-04T09:32:01-08:00
draft = false
template = "blog/page.html"

[extra]
authors = ["drewdeponte"]
+++

I believe that there is a large gap in current development practices in terms of association of identity and source code. This is something that a large percentage of people have been ignoring for quite some time. I was one of those people until recently. I started asking myself the question: How hard would it be for someone to write code and claim me as the author, basically forging code in my name? I realized that it was as simple as someone setting their authorship email and name in their `.gitconfig` to the values of my email address and name.

The more I thought about this, the more it concerned me, and the more it seemed there must be a solution to this problem. This especially becomes a problem when you start talking about regulations around software development and the legal responsibility involved with software that is tightly coupled to human life and well-being, for example software in self-driving cars.

## The Solution

Coming from a security background, I knew that a digital signature could solve this problem. So, being familar with [GPG], an encryption and digital signing tool that is often used to digitally sign and encrypt emails, I decided to search and see if [Git] supported integration with [GPG]. Sure enough it does, specifically for signing commits and tags. Knowing this, the next step was to setup and configure my machine, which is running macOS Sierra Version 10.12.6 (16G29).

## How I Did It

The following is a breakdown of the final set of steps I took to get [Git] and [GPG] functioning together. It took me a number of tries to get to this point, as numerous articles didn't seem to work for me with version 2.1.22 of [GPG], which is what I am using. It is also probably worth mentionining that I am using [Git] version 2.13.3

### Install Dependencies

The first thing is to make sure that you have the necessary dependencies installed. I did this via [Homebrew], and recommend the same.

```
brew install gnupg gpg-agent pinentry-mac
```

The above installs `gnupg` (a.k.a. `gpg`) the tool set for digital signing, `gpg-agent` a support tool for `gnugp` to help securely manage your `gnugp` key's password while using it, and `pinentry-mac`, a tool to securely prompt and manage password/pin entry.

### GnuPG Key Pair Generation

Once you have the dependencies installed, assuming you don't have a [GPG] key pair already, you need to create one. This is done by running the following command and following the on screen instructions.

```
gpg --gen-key
```

Next, I recommend backing up your key pair. This is important, as you probably won't want to create a new identity if you get a new computer. I recommend backing it up in a physically secured space, like a safe. Given the depth and size of debate around proper key management, I will simply point you to the [NIST Recommendation for Key Management](http://csrc.nist.gov/publications/nistpubs/800-57/sp800-57_part1_rev3_general.pdf).

The crucial thing is that you **never ever** share your private key, otherwise all this effort is pointless, as they would be able to forge your commits and tags.

### Get your GPG Key Identifier

Once you have your key, the next step is getting your key identifer. This is needed to configure [Git] properly later.

The key id is actually a component of the fingerprent of the public key certificate, the thing holding all your subkeys and meta data that are used for signing and encryption. Specifically, the "Key ID" (a.k.a. Long Key ID) is the last 16 characters of the fingerprint. You can get it by running the following:

```
gpg --fingerprint <email you tied to the key during generation>
```

The output should look something like the following:

```
pub   rsa2048 2017-07-28 [SC] [expires: 2019-07-28]
            678A A67F A701 CFA7 D666  BCE8 41C5 D2C6 E5AF 944C
uid           [ultimate] Andrew De Ponte <cyphactor@gmail.com>
sub   rsa2048 2017-07-28 [E] [expires: 2019-07-28]
```

In the above output the following is the fingerprint:

```
678A A67F A701 CFA7 D666  BCE8 41C5 D2C6 E5AF 944C
```

The last 16 characters are:

```
41C5 D2C6 E5AF 944C
```

If you remove the spaces you have the Key ID, which you will need very shortly:

```
41C5D2C6E5AF944C
```

**Note:** It is ok to expose your fingerprint. It is part of the public key certificate and is something that you would share with someone to prove that the public key they have for you is actually your public key.

### Tell Git about your GPG Key

Next you need to tell [Git] that you have a [GPG] key and to use it to sign your commits and tags. This is done by running the following:

```
git config --global user.signingkey <your gpg key id>
git config --global commit.gpgsign true
git config --global tag.gpgsign true
```

### Configure GPG

Then you need to configure [GPG] appropriately to work on macOS. In order to do this you need to create the `~/.gnupg/gpg-agent.conf` file, with the following content:

```
pinentry-program /usr/local/bin/pinentry-mac
```

The the above config tells [GPG] to work with the `pinentry-mac` program we installed at the beginning, allowing it to properly prompt us for the password when needed.

### Testing It Out

Now that everything should be set up, you need to test it out and verify that everything is working properly. Below are the steps I took to create a test repository with an initial signed commit.

```
$ mkdir -p /tmp/test-git-signing
$ cd /tmp/test-git-signing
$ git init .
$ echo "Hello" > README.md
$ git add README.md
$ git commit -m "Initial signed commit"
```

You will probably be prompted by `pinentry-mac` to enter your [GPG] key password. **Note:** You can check the box to have it store and manage your password in Apple Keychain. This will make it so you don't have to enter your password all the time.

Now you can check out your signature in all it's glory by doing the following:

```
$ git log --show-signature
```

The output should look something like this:

```
commit 1a5fed0757e2c8600318595f1681cf508033e72f (HEAD -> master)
gpg: Signature made Sat Aug  5 01:30:43 2017 PDT
gpg:                using RSA key 678AA67FA701CFA7D666BCE841C5D2C6E5AF944C
gpg: Good signature from "Andrew De Ponte <cyphactor@gmail.com>" [ultimate]
Author: Andrew De Ponte <cyphactor@gmail.com>
Date:   Sat Aug 5 01:30:43 2017 -0700

        Initial signed commit
```

The above shows you that the commit is signed and that the signature is **verified**. If it was a commit by someone else and wasn't verified, maybe because you didn't have their verified public key in your [GPG] key ring, it would look something like the following:

```
gpg: Signature made Wed Sep 13 02:08:25 2006 PDT using DSA key ID F3119B9A
gpg: Can't check signature: public key not found
error: could not verify the tag 'v1.4.2.1'
```

### Great. So how does this help?

This makes it possible for people to verify via cryptography that you are actually the person that authored the commit or tag. If they are signing their commits and tags it also allows you to do the same. In fact, [Git] even allows you to have it reject merges and pulls if the commits are **not** signed by a verified person. To see more details on this, check out the [Git Tools Signing Your Work in the Git SCM Book](https://git-scm.com/book/en/v2/Git-Tools-Signing-Your-Work). 

To really understand exactly how that is done you have to understand that you have two keys, a public key and a private key, and that they are related. You share, and generally publish, your public key. This is because the public key is used by others to do two things. First, to verify signatures you have made with your private key, and second, to encrypt information to make sure you are the only one that can access/decrypt it using your private key.

#### Publish Your Public Key

You need to make sure that you have given people your public key, that you have the other collaborators' public keys, and that everyone has verified everyone's public keys. To facilitate sharing public keys there are key servers you can publish your keys to. Beyond that, [GitHub](https://github.com) also provides a mechanism for publishing your [GPG] public keys as well. Just go to your Settings, SSH & GPG Keys, and add your GPG Public Key. You can also publish your public key to a key server using [GPG] directly. The following is an example:

```
$ gpg --send-keys 41C5D2C6E5AF944C
gpg: sending key 41C5D2C6E5AF944C to hkps://hkps.pool.sks-keyservers.net
```

**Note:** The major key servers share and sync the public keys so you shouldn't have to worry about dealing with multiple key servers.

#### Verify and Sign

Once you have published your key, you need to work with people to verify that the key they have actually belongs to you. This is often done at a [Key Signing Party](https://en.wikipedia.org/wiki/Key_signing_party) to be the most secure. However, I have known many to verify their keys over the phone, as long as the people can answer questions proving they are who they say they are.

You can search for someone's public keys via their email address on the key servers using the following:

```
$ gpg --search-keys <their email address>
```

After you find and import their public key, you need to verify it and sign it. To verify it you can have your own little signing party, or do it via phone if you are comfortable with that. Either way, the process of verification is the same. First ask the person for the fingerprint of their public key. Next verify that it matches the fingerprint of the public key you have for them. Remember, you can see the fingerprint of their key by doing, `gpg --fingerprint <their email address>`. Once you have verified they match, you can define your level of trust for them using the following:

```
$ gpg --edit-keys <their email address>
> trust
```

For details around the different levels of trust and what they mean you can refer to the [Trust in a key's owner](https://www.gnupg.org/gph/en/manual/x334.html). You can also fully sign a public key by doing the following:

```
$ gpg --edit-keys <their email address>
> sign
```

You can find out more about validating keys in the [GPG Manual - Validating other keys on your public keyring](https://www.gnupg.org/gph/en/manual/x334.html). More information is also available on exchanging keys and signing keys in the [GPG Manual - Exchanging keys](https://www.gnupg.org/gph/en/manual/x56.html).

#### Confusion

I know this topic can be a little confusing at times. A few parts are still confusing for me when I haven't dealt with them in a while. So, if you are confused with terms, or want a deeper understanding of the entities involved and their attributes with [GPG], I recommend checking out the article [Anatomy of a GPG Key](https://davesteele.github.io/gpg/2014/09/20/anatomy-of-a-gpg-key/). If you are confused about [GPG] itself, they have decent [documentation](https://gnupg.org/documentation/).

Once all of the above has been done you should be able to cryptographically verify and manage your source code with true identity representation and trust.

[GPG]: https://gnupg.org
[Git]: https://git-scm.com
[Homebrew]: https://brew.sh
