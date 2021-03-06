# rules_nixpkgs

[![CircleCI](https://circleci.com/gh/tweag/rules_nixpkgs.svg?style=svg)](https://circleci.com/gh/tweag/rules_nixpkgs)

Rules for importing Nixpkgs packages into Bazel.

## Rules

* [nixpkgs_git_repository](#nixpkgs_git_repository)
* [nixpkgs_package](#nixpkgs_package)

## Setup

Add the following to your `WORKSPACE` file, and select a `$COMMIT` accordingly.

```bzl
http_archive(
    name = "io_tweag_rules_nixpkgs",
    strip_prefix = "rules_nixpkgs-$COMMIT",
    urls = ["https://github.com/tweag/rules_nixpkgs/archive/$COMMIT.tar.gz"],
)

load("@io_tweag_rules_nixpkgs//nixpkgs:nixpkgs.bzl", "nixpkgs_git_repository", "nixpkgs_package")
```

## Example

```bzl
nixpkgs_git_repository(
    name = "nixpkgs",
    revision = "17.09", # Any tag or commit hash
    sha256 = "" # optional sha to verify package integrity!
)

nixpkgs_package(
    name = "hello",
    repository = "@nixpkgs"
)
```

## Rules

### nixpkgs_git_repository

Name a specific revision of Nixpkgs on GitHub or a local checkout.

```bzl
nixpkgs_git_repository(name, revision, sha256)
```

<table class="table table-condensed table-bordered table-params">
  <colgroup>
    <col class="col-param" />
    <col class="param-description" />
  </colgroup>
  <thead>
    <tr>
      <th colspan="2">Attributes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>name</code></td>
      <td>
        <p><code>Name; required</code></p>
        <p>A unique name for this target</p>
      </td>
    </tr>
    <tr>
      <td><code>revision</code></td>
      <td>
        <p><code>String; optional</code></p>
        <p>Git commit hash or tag identifying the version of Nixpkgs
           to use.</p>
      </td>
    </tr>
    <tr>
      <td><code>remote</code></td>
      <td>
        <p><code>String; optional</code></p>
        <p>The URI of the remote Git repository. This must be a HTTP
           URL. There is currently no support for authentication.</p>
      </td>
    </tr>
    <tr>
      <td><code>sha256</code></td>
      <td>
        <p><code>String; optional</code></p>
        <p>The SHA256 used to verify the integrity of the repository</p>
      </td>
    </tr>
  </tbody>
</table>

### nixpkgs_package

Make the content of a Nixpkgs package available in the Bazel workspace.

```bzl
nixpkgs_package(
    name, attribute_path, nix_file, nix_file_deps, nix_file_content,
    path, repository, build_file, build_file_content,
)
```

If neither `repository` or `path` are specified, you must provide a
nixpkgs clone in `nix_file` or `nix_file_content`.

<table class="table table-condensed table-bordered table-params">
  <colgroup>
    <col class="col-param" />
    <col class="param-description" />
  </colgroup>
  <thead>
    <tr>
      <th colspan="2">Attributes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>name</code></td>
      <td>
        <p><code>Name; required</code></p>
        <p>A unique name for this target</p>
      </td>
    </tr>
    <tr>
      <td><code>attribute_path</code></td>
      <td>
        <p><code>String; optional</code></p>
        <p>Select an attribute from the top-level Nix expression being
           evaluated. The attribute path is a sequence of attribute
           names separated by dots.</p>
      </td>
    </tr>
    <tr>
      <td><code>nix_file</code></td>
      <td>
        <p><code>String; optional</code></p>
        <p>A file containing an expression for a Nix derivation.</p>
      </td>
    </tr>
    <tr>
      <td><code>nix_file_deps</code></td>
      <td>
        <p><code>List of labels; optional</code></p>
        <p>Dependencies of `nix_file` if any.</p>
      </td>
    </tr>
    <tr>
      <td><code>nix_file_content</code></td>
      <td>
        <p><code>String; optional</code></p>
        <p>An expression for a Nix derivation.</p>
      </td>
    </tr>
    <tr>
      <td><code>repository</code></td>
      <td>
        <p><code>Label; optional</code></p>
        <p>A Nixpkgs repository label. Specify one of `path` or
		   `repository`.</p>
      </td>
    </tr>
    <tr>
      <td><code>path</code></td>
      <td>
        <p><code>String; optional</code></p>
        <p>The path to the directory containing Nixpkgs, as
           interpreted by `nix-build`. Specify one of `path` or
		   `repository`.</p>
      </td>
    </tr>
    <tr>
      <td><code>build_file</code></td>
      <td>
        <p><code>String; optional</code></p>
        <p>The file to use as the BUILD file for this repository. This
           attribute is a label relative to the main workspace. The
           file does not need to be named BUILD, but can be.</p>
      </td>
    </tr>
    <tr>
      <td><code>build_file_content</code></td>
      <td>
        <p><code>String; optional</code></p>
        <p>The content for the BUILD file for this repository.</p>
      </td>
    </tr>
  </tbody>
</table>
