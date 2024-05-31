# Prerequisites

We're here to openly build, distribute, and deploy software! To do that, we'll
need a few things.

## Sign Up

I recommend signing up for the following services, which generally all have a
free tier:

- [GitHub](https://github.com/)
- [Cloudflare](https://www.cloudflare.com/)
- [Fly.io](https://fly.io/)
- [Sonatype Central Repository](https://central.sonatype.org/)

GitHub is where we'll host our code, issues, and CI/CD. Cloudflare is where
we'll buy a domain and manage DNS. Fly.io is where we'll deploy our
applications. Sonatype is where we'll publish our libraries (artifacts).

Artifacts are deployed as reversed domain names, so you'll need a domain name
you control! If you do not wish to buy/manage a domain, you can use the
`io.github.<username>` namespace from your GitHub account.

Fly.io deployments will have their own domain as well, so using CNAME records
from your own domain there is _cool_, but also optional.

## IDE

For this book, I recommend using
[Visual Studio Code](https://code.visualstudio.com/) with the
[Metals](https://scalameta.org/metals/) plugin for Scala development. Metals is
a language server that provides IDE features like autocomplete, go to
definition, and more.
