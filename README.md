# TailwindCSS Standalone executables (via Mise) for Linux, macOS, and Windows

This guide focuses on managing **TailwindCSS Standalone binaries** across Linux, macOS, and Windows using [mise-en-place](https://github.com/jdx/mise). This method allows you to maintain multiple versions (e.g., v3 and v4) simultaneously on your machine **without needing Node.js or npm**.

## Why use Mise for Tailwind?

While Tailwind Labs provides the executables, managing them manually (downloading, renaming, moving to PATH) is tedious. Using `mise` with the `github` backend automates this:

* **No Node.js required**: Runs the pre-compiled native binaries.
* **Side-by-side versions**: Use Tailwind v3 for legacy projects and v4 for new ones in the same terminal.
* **Auto-switching**: `mise` switches the `tailwindcss` command version automatically based on your current directory.

---

## Installation with mise-en-place via `github` backend (Recommended)

The most reliable way to manage Tailwind versions is using the `github:` backend. This fetches releases directly from the official [tailwindlabs/tailwindcss](https://github.com/tailwindlabs/tailwindcss/releases) repository.

Install it as a TailwindCSS alias to avoid constantly using the long backend reference. It's important to set the alias before installing the first TailwindCSS version; otherwise, `mise ls` will list your TailwindCSS versions in two separate entries (it won't handle the original name and the alias together).

```bash
mise plugin rm tailwindcss
mise config set tool_alias.tailwindcss github:tailwindlabs/tailwindcss
```

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

# Check installed TailwindCSS versions
mise ls tailwindcss

# Change globally selected TailwindCSS version
mise use -g tailwindcss@4
```

Regardless of the version, the command remains the same:

```bash
# For development (runs until you stop it)
tailwindcss -i input.css -o output.css --watch

# For production (runs once)
tailwindcss -i input.css -o output.css
```

---

## Automatic updates & version pinning

One of the most powerful features of `mise` is how it handles version resolutions. Instead of hardcoding a specific (patch) release, you can reference **major versions**. By referencing only the major version, you ensure that you always have the latest features and security patches within that release cycle, without manually downloading new binaries.

* **`tailwindcss@4`**: Points to the latest stable **v4.x.x**.
* **`tailwindcss@3`**: Points to the latest stable **v3.x.x**.

This is particularly useful because Tailwind v4 is a significant architectural shift. You can keep your v3 projects stable while exploring v4 features, and `mise` will manage the "latest" for both simultaneously.

### How `mise` upgrades packages

`mise` doesn't just "install and forget". It allows you to keep your binaries fresh with simple commands. To see if a newer version is available for your pinned major version:

```bash
mise outdated
```

If you are using `@4` and Tailwind releases `4.0.1`, you can upgrade simply by running:

```bash
# Upgrades all managed tools to their latest resolved version
mise upgrade

# Upgrade only TailwindCSS
mise upgrade tailwindcss
```

---

## Advanced configuration

### Per-project versioning

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

## Running into issues?

While I actively follow TailwindCSS development and can answer related questions here, I recommend reaching out via Stack Overflow or GitHub Discussions for general TailwindCSS support.
* https://stackoverflow.com/questions/tagged/tailwind-css
* https://github.com/tailwindlabs/tailwindcss/issues
* https://github.com/tailwindlabs/tailwindcss/discussions

If you encounter issues specifically with mise-en-place, you can find a more active community for troubleshooting in mise-en-place Discussions.
* https://github.com/jdx/mise/discussions

## About Me

I use mise-en-place primarily as a user, but I have deep, professional-level expertise in TailwindCSS. I have contributed answers to numerous Tailwind-related questions across Stack Overflow, GitHub Discussions, and the official Discord servers.

* [Stack Overflow answers](https://stackoverflow.com/search?tab=votes&q=user:15167500%20[tailwind-css]&searchOn=3)

