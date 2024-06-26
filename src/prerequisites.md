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

## Other Software

You should get [Docker Desktop](https://www.docker.com/products/docker-desktop)
and [docker-compose](https://github.com/docker/compose) for local development.
Docker is a container runtime that allows you to run applications in isolated
environments. We will use it to bring up our dependencies such as Postgres, and
Redis.

The Scala ecosystem also encompasses two other projects/platforms we will gear
up for.

### ScalaJs

[ScalaJS](https://www.scala-js.org/) is a Scala to JavaScript compiler. You will
need to install [NodeJS](https://nodejs.org/) to run the JavaScript code that
ScalaJS produces, if you wish to follow any ScalaJS sections of this book.

### Scala Native

[Scala Native](https://scala-native.org/) is an optimizing ahead-of-time
compiler and lightweight managed runtime designed specifically for Scala. It
requires Clang, which is part of the LLVM toolchain. Please follow their
[documentation](https://scala-native.org/en/latest/user/setup.html#installing-clang-and-runtime-dependencies)
to get that set up, if you wish to follow any Scala Native sections of this
book.

## Kubernetes

Just kidding! We won't be using Kubernetes in this book. If you come to drips
with Docker, then you are well on your way to understanding Kubernetes to the
extent of deploying/managing your applications. It's a powerful tool, but the
cost-to-benefit ratio is not worth it for the context of this book.

## Scala

The most important part! Setting up Scala, via scala-cli, is covered in the
first section of the next chapter on getting set up.
