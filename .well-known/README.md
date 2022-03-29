# `/.well-known/`

## `openpgpkey/` - Web Key Directory (WKD)

> Web Key Directories provide an easy way to discover public keys through HTTPS. They provide an important piece to the infrastructure to improve the user experience for exchanging secure emails and files.
>
> A Web Key Directory keeps email addresses private (as you need to know an address already to ask for a public key). And it is an authoritative source for a public key for its domain.

```bash
mkdir -p openpgpkey/hu
touch openpgpkey/policy
gpg --list-keys --with-wkd-hash security@sebastianforst.de
# Take hash output in front of domain as destination
# filename in next statement:
gpg --no-armor --export security@sebastianforst.de > openpgpkey/hu/t5s8ztdbon8yzntexy6oz5y48etqsnbb
```

See:
- https://wiki.gnupg.org/WKD
- https://shibumi.dev/posts/how-to-setup-your-own-wkd-server/
- https://metacode.biz/openpgp/web-key-directory
- https://www.uriports.com/blog/setting-up-openpgp-web-key-directory/
- (German) https://www.kuketz-blog.de/gnupg-web-key-directory-wkd-einrichten/

## `mta-sts.txt` - SMTP Mail Transfer Agent Strict Transport Security

See:
- https://www.digitalocean.com/community/tutorials/how-to-configure-mta-sts-and-tls-reporting-for-your-domain-using-apache-on-ubuntu-18-04
- https://esmtp.email/tools/mta-sts/
- https://starttls-everywhere.org

## `security.txt`

> A File Format to Aid in Security Vulnerability Disclosure
>
> When security vulnerabilities are discovered by researchers, proper
> reporting channels are often lacking.  As a result, vulnerabilities
> may be left unreported.  This document defines a format
> ("security.txt") to help organizations describe their vulnerability
> disclosure practices to make it easier for researchers to report
> vulnerabilities.

See:
- https://securitytxt.org/
- https://tools.ietf.org/html/draft-foudil-securitytxt

## create my signed `security.txt`

    gpg --local-user security@sebastianforst.de \
      --output security.txt --clearsign unsigned_security.txt

## verify `security.txt` signature

     gpg --locate-key security@sebastianforst.de

requires `curl` v7.52.0 or above for TLS 1.3 support:

    curl -s https://sebastianforst.de/.well-known/security.txt | gpg --verify
