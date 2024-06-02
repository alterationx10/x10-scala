# Scala CLI

[Scala CLI](https://scala-cli.virtuslab.org/) is an ambitious new tool for
Scala, that aims to be a batteries-included powerhouse for getting up and
running with the language. It lets you write simple scripts/programs with ease,
and can even manage a decent sized project as a build tool, despite them not
billing it as a build tool. You can easily publish your own libraries -
including GPG signing your artifacts. The list goes on...

Generally, only single module projects are supported at the moment, but we'll
take that as a challenge and see just how much we can build with it!

## Installation

Head over to
[https://scala-cli.virtuslab.org/install#advanced-installation](https://scala-cli.virtuslab.org/install#advanced-installation)
to quickly get up and running with Scala CLI. It will likely already be
selected, but pick your OS and install method, and follow the instructions. (I
use homebrew for MacOS).

As a reminder you will need to install [NodeJS](https://nodejs.org/) to run the
JavaScript code that ScalaJS produces, and
[Clang/LLVM Toolchain](https://scala-native.org/en/latest/user/setup.html#installing-clang-and-runtime-dependencies)
for native interop, if you wish to follow any ScalaJS or Scala Native sections
of this book.

As an alternative, you can follow Scala's
[official getting started guide](https://docs.scala-lang.org/getting-started/index.html),
and use a tool called [coursier](https://get-coursier.io/docs/cli-overview) to
run `cs setup`, which installs a few useful things to bootstrap your
environment, including `scala-cli`.

### Scala CLI vs Scala

This will be a bit confusing at first, but if you were to install Scala via the
[official getting started guide](https://docs.scala-lang.org/getting-started/index.html)
and run `cs setup`, it would install a few useful things to bootstrap your
environment, including `scala` and `scala-cli` commands.

In that guide, the `scala` command is referred to as a _runner_, and `scala-cli`
is referred to as an _interactive toolkit_. However, as of
[SIP-46](https://github.com/scala/improvement-proposals/pull/46), the newest
`scala` runner is based on scala-cli!

What this means for you, in the context of this book, is that we haven't
necessarily installed the `scala` command, so in order to manage our code, or
drop into a REPL, we will use the `scala-cli` command. `scala-cli` can manage
the dependencies for use, and run our code, so it's a good all-in-one tool for
our purposes.

For a longer discussion, see the
[What is the difference between Scala runner and Scala CLI](https://virtuslab.com/blog/technology/scala-cli-the-new-scala-runner/)
from VirtusLab.
