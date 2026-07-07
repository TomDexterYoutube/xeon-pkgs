# Creating a Rubidium Package

Publishing a package is intentionally simple. Every package lives inside this repository and consists of a small folder containing your source code and package information.

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
* The **minor** and **patch** numbers must each be a single digit (`0-9`).
* The **major** number may contain multiple digits.

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

```
json_parser/
```

---

## Step 3 - Add `pkg.rub`

Inside your package folder, create a file named:

```
pkg.rub
```

This is the package's main entry point.

Example:

```
json_parser/
├── pkg.rub
```

Your package may also contain additional `.rub` files.

For example:

```
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

```
pkg.info
```

This file contains a description of the package.

Example:

```text
A fast and lightweight JSON parser written in Rubidium.
```

---

## Final Structure

A complete package should look like this:

```
json_parser/
├── pkg.rub
├── lexer.rub
├── parser.rub
├── utils.rub
└── pkg.info
```

And the corresponding entry in `pkg-list`:

```text
json_parser|1.0.0
```

That's it. No manifests, dependency files, build scripts, or forty-seven layers of configuration that somehow require another package manager to install the package manager. Just register the package, add the folder, include `pkg.rub`, and write a short description in `pkg.info`.
