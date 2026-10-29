# Creating a Rubidium Package

Publishing a package is intentionally simple. Every package lives inside this repository and consists of a folder containing its available versions, source code, package information, and ownership details.

---

## Step 1 - Add your package to `pkg-list`

Open the `pkg-list` file and add a new entry using the following format:

```text
package-name|0.0.1
```

The version listed here must always be the **latest stable version** of the package.

### Package Name Rules

- Only lowercase letters (`a-z`) and hyphens (`-`) are allowed.
- Every package name must be unique.

Examples:

```text
math|1.0.0
fast-math|2.1.3
json-parser|0.5.8
```

### Version Rules

Versions must always contain **three numbers** separated by dots.

Format:

```text
major.minor.patch
```

Examples:

```text
0.0.1
1.2.0
21.9.8
```

Rules:

- Exactly three numbers are required.
- Major, minor, and patch versions may contain multiple digits.
- Version numbers must be non-negative integers.
- Leading zeros are not allowed, except for the number `0`.
- Every published version must have its own directory.
- Previously published versions must not be modified or overwritten.
- The version listed in `pkg-list` must exist in the package's `versions` file.

Valid:

```text
0.0.0
1.4.7
12.13.29
123.0.5
1.10.100
```

Invalid:

```text
1.0
1.0.0.0
1.-2.0
1.02.3
v1.0.0
```

---

## Step 2 - Create your package folder

Create a folder with exactly the same name as your package.

Example:

```text
json-parser/
```

Inside this folder, create a file named:

```text
versions
```

This file contains **all available versions** of the package, with one version per line.

Example:

```text
0.0.1
0.1.0
0.5.8
1.0.0
```

Versions should be listed from oldest to newest.

Every version listed must have a corresponding directory inside the package folder.

Example:

```text
json-parser/
├── versions
├── 0.0.1/
├── 0.1.0/
├── 0.5.8/
└── 1.0.0/
```

When publishing a new version:

1. Create a new directory named after the version.
2. Add the package files inside that directory.
3. Add the new version to the `versions` file.
4. Update `pkg-list` if the new version is the latest stable release.

Do not delete or modify older versions.

---

## Step 3 - Add `owner`

Every package must include a file named:

```text
owner
```

This file must be located in the package's main directory, alongside the `versions` file.

It contains the **GitHub username of the package owner**.

Example:

```text
TomDexterYoutube
```

### Ownership Rules

- Every package must have exactly one owner.
- The owner must be a valid GitHub account.
- Only the package owner may publish new versions or update package metadata.
- Published versions must not be modified or overwritten, including by the owner.
- Package ownership cannot be transferred without approval from the repository administrator.
- Unauthorized users must not modify the `owner` file.
- New package ownership must be verified before the first publication.
- The package owner is responsible for maintaining the package and its releases.

### Ownership Verification

When publishing a package through Xeon, the publishing system must verify that the authenticated GitHub account matches the username in the `owner` file.

For example:

```text
Package: json-parser
Owner: TomDexterYoutube
Publisher: TomDexterYoutube

Publication authorized.
```

If the publisher does not match:

```text
Package: json-parser
Owner: TomDexterYoutube
Publisher: AnotherDeveloper

Error: You do not own this package.
```

The `owner` file identifies the authorized developer, but permissions must be enforced by the repository's publishing and validation system.

The `owner` file itself must not be trusted as proof of ownership when submitted as part of an update. Ownership must be checked against the existing trusted registry record.

---

## Step 4 - Add `pkg.rub`

Inside each version directory, create a file named:

```text
pkg.rub
```

This is the package's main entry point.

Your package may also contain additional `.rub` files.

Example:

```text
json-parser/
├── owner
├── versions
└── 1.0.0/
    ├── pkg.rub
    ├── lexer.rub
    ├── parser.rub
    └── utils.rub
```

These files can be imported normally using Rubidium's `import` system.

Each version has its own independent source files.

---

## Step 5 - Add `pkg.info`

Every version must include a file named:

```text
pkg.info
```

This file contains a short description of the package.

Example:

```text
A fast and lightweight JSON parser written in Rubidium.
```

---

## Step 6 - Add `pkg.ver`

Every version must also include a file named:

```text
pkg.ver
```

This file contains **only** the package version.

Example:

```text
1.0.0
```

The version inside `pkg.ver` must match the name of its containing version directory and an entry in the package's `versions` file.

For example:

```text
json-parser/1.0.0/pkg.ver
```

Must contain:

```text
1.0.0
```

The latest stable version must also match the version listed in `pkg-list`.

---

## Final Structure

A complete package containing multiple versions should look like this:

```text
xeon-pkgs/
├── pkg-list
├── json-parser/
│   ├── owner
│   ├── versions
│   ├── 0.5.8/
│   │   ├── pkg.rub
│   │   ├── pkg.info
│   │   └── pkg.ver
│   └── 1.0.0/
│       ├── pkg.rub
│       ├── lexer.rub
│       ├── parser.rub
│       ├── utils.rub
│       ├── pkg.info
│       └── pkg.ver
└── math/
    ├── owner
    ├── versions
    └── 1.0.0/
        ├── pkg.rub
        ├── pkg.info
        └── pkg.ver
```

The `owner` file for `json-parser`:

```text
TomDexterYoutube
```

The `versions` file:

```text
0.5.8
1.0.0
```

With the corresponding entries in `pkg-list`:

```text
json-parser|1.0.0
math|1.0.0
```

---

# Installing Packages

Rubidium packages are managed using **Xeon**.

All package commands begin with:

```text
xeon pkg
```

## Available Commands

### Show Help

```text
xeon pkg help
```

Displays information about the package manager and all available commands.

---

### Fetch Package List

```text
xeon pkg fetch
```

Downloads the latest `pkg-list` from this repository.

This updates the local package index but does **not** install or update any packages.

---

### Install a Package

```text
xeon pkg pull <package-name>
```

Downloads and installs the latest stable version of the requested package.

Example:

```text
xeon pkg pull json-parser
```

Xeon reads the version listed in `pkg-list` and downloads the package files from the corresponding version directory.

For example:

```text
json-parser/1.0.0/
```

---

### Install a Specific Version

```text
xeon pkg pull <package-name>@<version>
```

Downloads and installs a specific version of a package.

Example:

```text
xeon pkg pull json-parser@0.5.8
```

Xeon checks the package's `versions` file to confirm that the requested version exists, then downloads the files from that version's directory.

---

### View Available Versions

```text
xeon pkg versions <package-name>
```

Displays all available versions of a package by reading its `versions` file.

Example:

```text
xeon pkg versions json-parser
```

Example output:

```text
Available versions for json-parser:

  0.5.8
  1.0.0 (latest)
```

---

### Remove a Package

```text
xeon pkg purge <package-name>
```

Removes an installed package.

Example:

```text
xeon pkg purge json-parser
```

---

### Upgrade Packages

```text
xeon pkg upgrade
```

Checks every installed package for updates.

For each installed package, Xeon:

1. Reads the package's local `pkg.ver`.
2. Looks up the latest version in the downloaded `pkg-list`.
3. Compares the versions.
4. Downloads the newer version from its corresponding version directory.
5. Replaces the installed package with the newer version.
6. Leaves packages that are already up to date unchanged.

It is recommended to fetch the latest package list before upgrading:

```text
xeon pkg fetch
xeon pkg upgrade
```

---

# Publishing and Maintaining Packages

Package owners are responsible for publishing and maintaining their own packages.

## Publishing a New Package

When publishing a package for the first time:

1. Choose a unique package name.
2. Create a package directory.
3. Add an `owner` file containing your GitHub username.
4. Create a `versions` file containing the initial version.
5. Create the initial version directory.
6. Add `pkg.rub`, `pkg.info`, and `pkg.ver`.
7. Register the package in `pkg-list`.
8. Submit the package for ownership verification and publication.

Once approved, the package name belongs to the verified owner.

## Publishing a New Version

When publishing an update:

1. Authenticate with the GitHub account registered in `owner`.
2. Create a new version directory.
3. Place the updated package files inside it.
4. Add the new version to `versions`.
5. Update `pkg-list` to point to the newest stable version.
6. Submit the update for ownership verification.

Existing versions must remain unchanged.

## Transferring Ownership

Package ownership may only be transferred with repository administrator approval.

The current owner must request the transfer, and the new owner's GitHub account must be verified.

After approval, the `owner` file may be updated.

Changing the `owner` file without an approved transfer does not grant package ownership.

---

That's all there is to it.

No manifests.

No dependency graphs.

No lock files.

No build scripts.

No twenty-seven configuration files arguing about whose turn it is to break the build.

Just register the package, add an `owner` and `versions` file, and put `pkg.rub`, `pkg.info`, and `pkg.ver` inside each version directory.

Old versions remain available, new versions can be published independently, and package ownership can be verified through GitHub.

**Simple package management, proper versioning, and verified ownership.**
