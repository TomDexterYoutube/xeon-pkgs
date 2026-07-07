# Creating a Rubidium Package

Publishing a package is intentionally simple. Every package lives inside this repository and consists of a small folder containing your source code and package information.

---

## Step 1 - Add your package to `pkg-list`

Open the `pkg-list` file and add a new entry using the following format:

```text
package_name|0.0.1
```

### Package Name Rules

* Only lowercase letters (`a-z`) and underscores (`_`) are allowed.
* Every package name must be unique.

Examples:

```text
math|1.0.0
fast_math|2.1.3
json_parser|0.5.8
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

* Exactly three numbers are required.
* The **major** version may contain multiple digits.
* The **minor** and **patch** versions must each be a single digit (`0-9`).

Valid:

```text
0.0.0
1.4.7
12.3.9
123.0.5
```

Invalid:

```text
1.10.0
1.0
1.0.0.0
```

---

## Step 2 - Create your package folder

Create a folder with exactly the same name as your package.

Example:

```text
json_parser/
```

---

## Step 3 - Add `pkg.rub`

Inside your package folder, create a file named:

```text
pkg.rub
```

This is the package's main entry point.

Your package may also contain additional `.rub` files.

Example:

```text
json_parser/
├── pkg.rub
├── lexer.rub
├── parser.rub
├── utils.rub
```

These files can be imported normally using Rubidium's `import` system.

---

## Step 4 - Add `pkg.info`

Every package must include a file named:

```text
pkg.info
```

This file contains a short description of the package.

Example:

```text
A fast and lightweight JSON parser written in Rubidium.
```

---

## Step 5 - Add `pkg.ver`

Every package must also include a file named:

```text
pkg.ver
```

This file contains **only** the package version.

Example:

```text
1.0.0
```

The version inside `pkg.ver` must match the version listed in `pkg-list`.

---

## Final Structure

A complete package should look like this:

```text
json_parser/
├── pkg.rub
├── lexer.rub
├── parser.rub
├── utils.rub
├── pkg.info
└── pkg.ver
```

With the corresponding entry in `pkg-list`:

```text
json_parser|1.0.0
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
xeon pkg pull <package_name>
```

Downloads and installs the requested package.

Example:

```text
xeon pkg pull json_parser
```

---

### Remove a Package

```text
xeon pkg purge <package_name>
```

Removes an installed package.

Example:

```text
xeon pkg purge json_parser
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
4. Downloads and replaces any package that has a newer version available.
5. Leaves packages that are already up to date unchanged.

It is recommended to fetch the latest package list before upgrading:

```text
xeon pkg fetch
xeon pkg upgrade
```

---

That's all there is to it.

No manifests.

No dependency graphs.

No lock files.

No build scripts.

No twenty-seven configuration files arguing about whose turn it is to break the build.

Just register the package, create its folder, include `pkg.rub`, `pkg.info`, and `pkg.ver`, and it's ready to be installed through Xeon.
