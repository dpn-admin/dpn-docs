Guidelines
==========

When generating your GPG key:

1.  Use a 4096 bit RSA key with SHA512 hash\
    1.  JJS - I'd recommend <span>RSA with SHA-256, or use DSA. We are
        using this encryption for exchanging public keys, when we
        exchange we should verify on the phone. </span>

2.  Use your real name, preferred email address, and "DPN KEY" as
    the comment.
3.  Use a strong password to protect your key

Once generated, you should:

-   Keep your private key file on a safe, secure computer, and make sure
    you have a secure backup.

Generate a GPG Key
==================

Carefully follow the instructions
[here](http://www.apache.org/dev/openpgp.html#generate-key) to generate
your key and check that SHA1 is avoided.

Tip: Popular binaries for GnuPG 2.x can be found here:

-   [Linux](http://www.gnupg.org/download/index.en.html#gnupg2)
-   [Mac OS X](https://gpgtools.org/macgpg2/index.html)
-   [Windows](http://www.gpg4win.org/download.html)

Note: After initially generating your key with GnuPG 2.x (gpg2), you can
work with it using the more commonly-available 1.4.9 release (gpg).

Note: On linux it is easy to build the latest version of gnuPG and
install. Once that is done, also easy, it integrates well with
Thunderbird.

Publish Public GPG Key
======================

To enable people to find your public key, you should publish it to a
well-known keyserver. This is a simple command with gpg:

gpg --send-key \[yourKeyID\]

<span style="font-size: 10.0pt;line-height: 13.0pt;">...where yourKeyID
is the last 8 digits of your public key fingerprint.</span>

This will upload your public key to a well-known keyserver, which will
then trigger other connected keyservers to get a copy. Afterward, you
can verify the general availability of your public key by searching for
your name in one of the [keyservers](http://pgp.mit.edu/). 

Add GPG Fingerprint to DPN wiki
===============================

Add your fingerprint to the DPN Contacts  page.

Sign Other Node Keys
====================

For each fingerprint on the DPN Nodes  page:

-   Download the key via:

    gpg --recv-keys \[fingerprint\]

<!-- -->

-   Sign it via:

    gpg -u yourKeyID --sign-key \[fingerprint\]

<!-- -->

-   Upload the signature via:

    gpg --send-key \[fingerprint\]

Ask Other Nodes to Sign GPG Key

Email the other DPN node representatives notifying them that you have
signed their key and uploaded the signature, and they should run:

gpg --refresh-keys

...then ask them to sign your key as indicated above.\
After they
have had a chance to sign your key and upload the signature, you should
also do a --refresh-keys so your local web of trust is up to
date.
