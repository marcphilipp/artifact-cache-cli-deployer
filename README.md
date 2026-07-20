# artifact-cache-cli-deployer

A GitHub Action that downloads the [Develocity Artifact Cache CLI](https://docs.develocity.ai/artifact-cache/1.4/#download)
and republishes it to the current repository's **GitHub Packages** Maven registry.

For a given `version` it:

1. Downloads the jar and its `.asc` PGP signature from `docs.develocity.ai`.
2. Verifies the signature against the pinned Gradle Inc. key
   (`develocity-signing-key.asc`, fingerprint `7B79 ADD1 1F8A 779F E90F D3D0 893A 0284 7555 7671`).
3. Publishes it via `mvn deploy:deploy-file` as
   **`com.gradle:develocity-artifact-cache-cli:<version>`** (jar + `.jar.asc`).

## Publish a version

Run the **Publish CLI** workflow (`workflow_dispatch`) and enter a version, e.g. `1.4.0`.
The artifact appears under the repository's *Packages*.

## Use as a reusable action

```yaml
permissions:
  contents: read
  packages: write
steps:
  - uses: actions/checkout@v4
  - uses: junit-team/artifact-cache-cli-deployer@main
    with:
      version: 1.4.0
```

## Consume the published artifact

```xml
<dependency>
  <groupId>com.gradle</groupId>
  <artifactId>develocity-artifact-cache-cli</artifactId>
  <version>1.4.0</version>
</dependency>
```

...with `https://maven.pkg.github.com/{org}/{repo}` configured as a
repository (GitHub Packages requires authentication for reads).
