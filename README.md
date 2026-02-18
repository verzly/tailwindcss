# TailwindCSS Standalone executables (via Mise) for Linux, macOS, and Windows

This guide focuses on managing **TailwindCSS Standalone binaries** across Linux, macOS, and Windows using [mise-en-place](https://github.com/jdx/mise). This method allows you to maintain multiple versions (e.g., v3 and v4) simultaneously on your machine **without needing Node.js or npm**.

## Why use Mise for Tailwind?

While Tailwind Labs provides the executables, managing them manually (downloading, renaming, moving to PATH) is tedious. Using `mise` with the `github` backend automates this:

* **No Node.js required**: Runs the pre-compiled native binaries.
* **Side-by-side versions**: Use Tailwind v3 for legacy projects and v4 for new ones in the same terminal.
* **Auto-switching**: `mise` switches the `tailwindcss` command version automatically based on your current directory.

---

## Installation via `mise` (Recommended)

The most reliable way to manage Tailwind versions is using the `github:` backend. This fetches releases directly from the official [tailwindlabs/tailwindcss](https://github.com/tailwindlabs/tailwindcss/releases) repository.

### 1. Set up a shorthand alias (Optional but Recommended)

To avoid typing the full GitHub path every time, set an alias:

```bash
mise config set tool_alias.tailwindcss github:tailwindlabs/tailwindcss
```

Without alias, you can't use `tailwindcss` name:
```bash
mise use github:tailwindlabs/tailwindcss@4
```

### 2. Installing specific versions

You can install and use multiple major versions globally or per project.

```bash
# Install the absolute latest version
mise use tailwindcss@latest

# Install/Use the latest stable v4
mise use tailwindcss@4

# Install/Use the latest stable v3 (for compatibility)
mise use tailwindcss@3

# Install a specific patch version
mise use tailwindcss@3.4.15
```

### 3. Usage Examples

**Check available versions**

```bash
mise ls tailwindcss
```

**Switching versions globally**

```bash
# Make v4 the default system-wide
mise use -g tailwindcss@4
```

**Running the compiler**
Regardless of the version, the command remains the same:

```bash
tailwindcss -i input.css -o output.css --watch
```

---

## Automatic updates & version pinning

One of the most powerful features of `mise` is how it handles version resolutions. Instead of hardcoding a specific release, you can reference **major versions**.

### Why use Major Versions (`@3`, `@4`)?

By referencing only the major version, you ensure that you always have the latest features and security patches within that release cycle, without manually downloading new binaries.

* **`tailwindcss@4`**: Points to the latest stable **v4.x.x**.
* **`tailwindcss@3`**: Points to the latest stable **v3.x.x**.

This is particularly useful because Tailwind v4 is a significant architectural shift. You can keep your v3 projects stable while exploring v4 features, and `mise` will manage the "latest" for both simultaneously.

### How `mise` Upgrades Packages

`mise` doesn't just "install and forget." It allows you to keep your binaries fresh with simple commands:

#### 1. Checking for updates

To see if a newer version is available for your pinned major version:

```bash
mise outdated
```

#### 2. Upgrading to the latest patch

If you are using `@4` and Tailwind releases `4.0.1`, you can upgrade simply by running:

```bash
# Upgrades all managed tools to their latest resolved version
mise upgrade

# Upgrade only TailwindCSS
mise upgrade tailwindcss
```

#### 3. Fresh Installs

If you want to ensure you are starting a project with the absolute latest patch of a major version:

```bash
# This will fetch the newest 4.x.x even if you have an older 4.0.0 cached
mise use tailwindcss@4
```

---

## Advanced Configuration

### Per-Project Versioning

If you have an older project that requires TailwindCSS v3, simply run this inside the project folder:

```bash
mise use tailwindcss@3
```

This creates a `.mise.toml` file. Every time you enter this folder, `mise` will ensure the `tailwindcss` command points to v3, while the rest of your system stays on v4.

---

## Troubleshooting

### Permissions

On Linux and macOS, `mise` automatically sets the executable bit. If you manually downloaded a binary from the releases page, remember to run `chmod +x <filename>`.

### Version Conflicts

If you previously installed Tailwind via `npm`, the npm version might take precedence in your PATH. Run `which tailwindcss` to ensure it points to the `~/.local/share/mise` path.

---

### Missing a Version?

If a new release is out but not appearing, you can force `mise` to clear its cache:

```bash
mise rescan
```

For official issues regarding the binaries themselves, please visit the [Tailwind Labs issue tracker](https://github.com/tailwindlabs/tailwindcss/issues).
