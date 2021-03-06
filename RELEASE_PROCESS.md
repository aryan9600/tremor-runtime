# Release Process

* Update version in all Cargo.toml files in the repository
  - ./Cargo.toml
  - ./tremor-api/Cargo.toml
  - ./tremor-cli/Cargo.toml
  - ./tremor-common/Cargo.toml
  - ./tremor-influx/Cargo.toml (if we have changes compared to the previous release)
  - ./tremor-pipeline/Cargo.toml
  - ./tremor-script/Cargo.toml (don't forget the reference to the other tremor packages)
  - ./tremor-value/Cargo.toml (if we have changes compared to the previous release)
* Update version in `./tremor-cli/src/cli.yaml` (minor version update only)
* Update version in `Dockerfile.learn`
* Update CHANGELOG.md
  - Change unreleased section to have the new version for the upcoming release
* Update tests in tremor-cli/tests/ that match against the current version number
* `git tag -a -m"Release v<MAJOR>.<MINOR>.<BUGFIX>" <COMMIT>`
* `git push origin --tag`
* Draft a new release on github
  - This will trigger the docker image build jobs
  - Add a catchy title
  - Add the relevant changelog entries for this release in the description
* Release crates to crates.io:
  - Make sure you are an owner of the crates to publish
  - Execute `cargo publish` in these folders in the following order:
    - ./tremor-common
    - ./tremor-value
    - ./tremor-script
* Release https://github.com/tremor-rs/tremor-language-server
  - Bump version and update dependency `tremor-script` to the new version.
  - Execute `cargo publish` from the language server repository root.
  - Verify new language server installation via `cargo install tremor-language-server`
* Wait for the docker image to build and publish
  - Verify docker image with some usage examples
  - Tag the published dockerhub image as latest: 

  ```sh
  export VERSION=<version>
  docker rmi --force tremorproject/tremor:$VERSION && \
    docker pull tremorproject/tremor:$VERSION && \
    docker tag tremorproject/tremor:$VERSION tremorproject/tremor:latest && \
    docker push tremorproject/tremor:latest
  ```

* If syntax changed: Update the highlighers:
  - https://github.com/tremor-rs/tremor-vim
  - https://github.com/tremor-rs/highlightjs-tremor
  - https://github.com/tremor-rs/tremor-mkdocs-lexer
  - https://github.com/tremor-rs/tremor-vscode
* Go to bed.
