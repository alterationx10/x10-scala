# Publishing Artifacts

This chapter will cover the process of publishing artifacts to Sontaype.

## Sonatype OSSRH

Sonatype OSSRH (OSS Repository Hosting) uses Sonatype Nexus Repository Manager
to provide repository hosting service for open source project binaries.

The initial setup for your OSSRH repository requires some manual steps and human
review, after which your deployment process is typically modified to get
components into OSSRH.

Sonatype is moving to a new way of publishing artifacts, and this is now
considered "Legacy", thought the way most tooling is geared towards (e.g.
sbt-ci-release). It still works, as the new way is (open) "early-access", but it
is recommended to move to the new way if possible.

Publishing via the existing workflow and the new workflow to the same namespace
is not possible, but migrating back to the OSSRH workflow is supported while the
Portal workflow is in Early Access.

### Links

- [https://central.sonatype.org/publish/publish-guide/#introduction](https://central.sonatype.org/publish/publish-guide/#introduction)

## Sonatype Central Portal

The Central Portal is the new way of publishing artifacts to Sonatype. The
process itself is straight forward, and fairly well
[documented](https://central.sonatype.org/publish-ea/publish-ea-guide/).

### Technical Difficulties

Here are a few issues that I encountered while trying to publish artifacts to
the Central Portal.

1. scala-cli doesn't directly support it yet.

The publish command still seems to be geared towards the old way of publishing.

2. scala-cli publish local publishes ivy2

We can piggyback off of the publish local command to bundle our files, but it
publishes to the local ivy2 repository, and the archive we need to upload needs
to be in maven format.

3. archive format

The documentation indicates that `.zip`, `tar.gz`, and `jar` are supported, but
when I uploaded a `.tar.gz` file, it would not pass validation. I switched to a
`.zip` file, and it worked. I came across a helpful comment on the internet
where someone mentioned they had a similar issue with `.zip`, and they resolved
it by using zip via command line, vs some gui built-in archiver.

### Technical Solutions

Here is an example of how I resolved the issues I encountered, via a Makefile.
This example is from my custom scala-cli toolkit repo
[x10-toolkit](https://github.com/alterationx10/x10-toolkit), if you want to see
the full code. Note this bundles/publishes two artifacts at once, the toolkit
and the toolkit-test.

There is the `x10-bundle` target, which uses scala-cli to published signed
artifacts to the local ivy2 repository, and then copies the artifacts to a
directory, and zips it up after adjusting it to a maven layout. The
`x10-publish` target uses curl to upload the zip file to the Central Portal.
Note the `publishingType=AUTOMATIC` query parameter, which will automatically
publish the artifacts after they are uploaded and pass validation. remove this
query parameter if you want to manually release the artifacts.

If you plan to use a `Makefile` in a GitHub action, be sure you run it with
`make -s`, so that the expanded variables aren't echoed back to the console. (-s
is for silent).

```make
.PHONY: x10-bundle x10-publish local clean

clean:
	rm -rf x10-bundle

x10-bundle:
	mkdir -p x10-bundle/dev/alteration/x10/toolkit_3/${TOOLKIT_VERSION}
	mkdir -p x10-bundle/dev/alteration/x10/toolkit-test_3/${TOOLKIT_VERSION}
	scala-cli --power publish local X10Toolkit.scala publish-conf.scala --gpg-key ${PGP_KEY_ID} --gpg-option --pinentry-mode --gpg-option loopback --gpg-option --passphrase --gpg-option ${PGP_PASSPHRASE}
	scala-cli --power publish local --dependency "dev.alteration.x10::toolkit::${TOOLKIT_VERSION}" X10ToolkitTest.scala publish-conf.scala --gpg-key ${PGP_KEY_ID} --gpg-option --pinentry-mode --gpg-option loopback --gpg-option --passphrase --gpg-option ${PGP_PASSPHRASE}
	for DIR in srcs docs poms jars; do \
		cp  ~/.ivy2/local/dev.alteration.x10/toolkit_3/${TOOLKIT_VERSION}/$$DIR/* x10-bundle/dev/alteration/x10/toolkit_3/${TOOLKIT_VERSION}; \
		cp  ~/.ivy2/local/dev.alteration.x10/toolkit-test_3/${TOOLKIT_VERSION}/$$DIR/* x10-bundle/dev/alteration/x10/toolkit-test_3/${TOOLKIT_VERSION}; \
	done
	cd x10-bundle/dev/alteration/x10/toolkit_3/${TOOLKIT_VERSION} && \
		rename 's/toolkit_3/toolkit_3-${TOOLKIT_VERSION}/' *
	cd x10-bundle/dev/alteration/x10/toolkit-test_3/${TOOLKIT_VERSION} && \
		rename 's/toolkit-test_3/toolkit-test_3-${TOOLKIT_VERSION}/' *
	cd x10-bundle && \
		zip -r x10-toolkit-${TOOLKIT_VERSION}.zip .

x10-publish:
	curl \
            --request POST \
            --header 'Authorization: Bearer ${CENTRAL_TOKEN}' \
            --form bundle=@x10-bundle/x10-toolkit-${TOOLKIT_VERSION}.zip \
            https://central.sonatype.com/api/v1/publisher/upload?publishingType=AUTOMATIC

```

### Links

- [https://central.sonatype.org/publish-ea/publish-ea-guide/](https://central.sonatype.org/publish-ea/publish-ea-guide/)
- [https://central.sonatype.org/publish/publish-portal-api/](https://central.sonatype.org/publish/publish-portal-api/)
