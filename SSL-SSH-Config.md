Approach
========

The DPN approach for secure communication covers the following two use
cases:

1.  Inter-node AMQP messaging
2.  Inter-node content transfer

DPN uses SSL and SSH connections to ensure channel security. \
In the DPN context, in order for SSL communication to be established,
the client and server must both have X509 certificates. Similarly with
SSH, rsa/dsa certificates must be held. \
Since DPN is a closed network, there is no additional value in having a
CA verify DPN certificates.\
At a future point, it may be arguable that having an internal DPN CA is
more secure and practical.

#### DPN Certificate Process

1.  All DPN nodes
    create X509 certificates and rsa/dsa certificates for SSH

2.  The nodes
    configure  **RabbitMQ** , and their 
    **webserver** 
    and/or  **rsync**  with\
    1.  their own signed certificates, and
    2.  the public certificates of the other DPN nodes

#### SSH Keys

For transfers with rsync, DPN will use SSH as a means of authentication
and encryption between nodes. It should be noted that while SSH uses
public key cryptography, it only uses the private key to connect to
clients. From the [Ubuntu
wiki](https://help.ubuntu.com/community/SSH/OpenSSH/Keys#Public_and_Private_Keys):
" The private key is kept on the computer you log in from, while the
public key is stored on the **.ssh/authorized\_keys** file on all the
computers you want to log in to."

Set up steps are as follows:

1.  Generate a key with "ssh-keygen -t rsa"\
    1.  You can specify the key length with the -b flag, but the keygen
        defaults to 2048 which is fine

2.  Send your public key (located in \~/.ssh/id\_rsa.pub by default) to
    the other nodes\
    1.  We can't use ssh-copy-id as it will prompt for a password, which
        we don't have
    2.  We can encrypt with GPG, but since we're sending public keys it
        isn't necessary

3.  Have the other nodes append your public key to their authorized key
    file, located at \~/.ssh/authorized\_keys by default\
    1.  For those interested, the format for the public keys are
        algorithm key user@host
    2.  For the lazy, "cat id\_rsa.pub &gt;&gt;
        \~/.ssh/authorized\_keys"

4.  Assuming sshd is running and making a connection is possible, you
    should now be able to connect and transfer content.

Resources
=========

1.  General SSL
    certificate [setup](http://www.tldp.org/HOWTO/SSL-Certificates-HOWTO/index.html)
2.  RabbitMQ SSL [setup](http://www.rabbitmq.com/ssl.html)
    [](http://www.tldp.org/HOWTO/SSL-Certificates-HOWTO/index.html)
3.  Apache2.4 SSL [setup](http://httpd.apache.org/docs/2.4/ssl/)
4.  Rsync SSL setup?? Rsync uses SSH, a different certificate than
    X.509.\
    1.  For linux machines use ssh-keygen 
    2.  <http://linux.die.net/man/1/ssh-keygen>, 
    3.   <http://www.cyberciti.biz/faq/howto-linux-unix-bsd-appleosx-generate-ssh-hostkeys/>  

5.  A good reference to SSL setup and self signed
    certificates: <http://www.zytrax.com/tech/survival/ssl.html>, <http://www.hosting.com/support/ssl/generate-a-self-signed-ssl-in-linux>

 


#### [James Simon]

I do not suggest we use a CA as we only have 5 nodes. We can use
self-signed certs. The question is how we exchange the certs securely
and how often we update. We will need to do this for both ssl and ssh.
If we have more than 5 or 10 nodes perhaps a CA, but not with 5 total,
the amount of effort is not worth it.


#### [James Simon]
From Wiki-pedia, they said it better than me -

------------------------------------------------

CAs are third parties and require both parties to trust the CA. (CAs are
typically large, impersonal enterprises and a high value target for
compromise.) If the parties know each other, trust each other to protect
their private keys, and can confirm transfer public keys (e.g. compare
the [hash](http://en.wikipedia.org/wiki/Hash "Hash"){.external-link} out
of band), then self-signed certificates may decrease overall risk.
Self-signed certificate transactions may also present a far smaller
[attack
surface](http://en.wikipedia.org/wiki/Attack_surface "Attack surface"){.external-link}.

Self-signed certificates cannot (by nature) be
revoked,^[\[1\]](http://en.wikipedia.org/wiki/Self-signed_certificate#cite_note-1){.external-link}^
which may allow an attacker who has already gained access to monitor and
inject data into a connection to spoof an identity if a private key has
been compromised. CAs on the other hand have the ability to revoke a
compromised certificate if alerted, which prevents its further use.

Some CA's can verify the identity of the person to whom they issue a
certificate; for example the US military issues their [Common Access
Cards](http://en.wikipedia.org/wiki/Common_Access_Cards "Common Access Cards"){.external-link}
in person, with multiple forms of other ID, and only when a higher
authority requires the issue.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Cost** Self-signed certificates can be created for free using a wide
variety of tools including Java's keytool, Adobe Reader, and Apple's
Keychain. Certificates bought from major CAs range from tens to
thousands of dollars per year.^\[*[<span
title="This claim needs references to reliable sources from December 2011">citation\\ needed](http://en.wikipedia.org/wiki/Wikipedia:Citation_needed "Wikipedia:Citation needed"){.external-link}*\]^

**Speed to Deploy** Self-signed certificates require the two parties to
interact (e.g. to securely trade public keys). Using a CA requires only
the CA and the certificate holder to interact; the holder of the public
key can validate its authenticity with the CA's root certificate.

**Customization** Self-signed certificates are easier to customize, for
example a larger key size, contained data, metadata, etc.


#### [Sebastien Korner]

I agree with using self-signed SSL certs to secure our message and
content transfer protocols (see, for example, the direction the SAML
federations are all going wrt PKI), and properly signed and verified GPG
keys to securely, yet easily, exchange the SSL public keys amongst
ourselves.

