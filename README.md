# RustDesk — Unofficial APT Repository

Unofficial Debian/Ubuntu repository that provides `.deb` packages of [RustDesk](https://rustdesk.com/) via the `apt` package manager.

Packages are automatically fetched from the official RustDesk project *releases*, repackaged, and signed with a custom GPG key.

> ⚠️ **IMPORTANT DISCLAIMER**
>
> This repository is **NOT affiliated with, endorsed by, or maintained by** the official RustDesk team.
> This is a personal/community project. Use it at your own risk.
> For the official version, please visit [rustdesk.com](https://rustdesk.com/).

---

## Features

- 🔄 **Automatic daily updates** via GitHub Actions.
- 🔐 **GPG-signed packages** — ensures integrity and authenticity.
- 📦 **Installation via `apt`** — simple update management.
- 🏗️ **Supported architecture:** `amd64` (x86_64).

---

## Installation

### 1. Import the repository GPG key

```bash
sudo mkdir -p /usr/share/keyrings
wget -qO - https://<YOUR-USERNAME>.github.io/<REPO-NAME>/KEY.gpg \
  | sudo gpg --dearmor -o /usr/share/keyrings/rustdesk-unofficial.gpg
sudo chmod 644 /usr/share/keyrings/rustdesk-unofficial.gpg
```

### 2. Add the repository to your sources list

```bash
echo "deb [signed-by=/usr/share/keyrings/rustdesk-unofficial.gpg] \
https://<YOUR-USERNAME>.github.io/<REPO-NAME>/ stable main" \
  | sudo tee /etc/apt/sources.list.d/rustdesk-unofficial.list
```

### 3. Update and install

```bash
sudo apt update
sudo apt install rustdesk
```

---

## Updating

Whenever you want to update to the latest version:

```bash
sudo apt update
sudo apt upgrade rustdesk
```

The GitHub workflow runs daily (`cron: 0 0 * * *`) and on every repository change, ensuring the available package matches the latest stable release published by the original authors.

---

## Signature Verification

To confirm the package was signed by this repository:

```bash
apt-cache policy rustdesk
```

The `Release` field should reference this repository's URL and the imported GPG key.

---

## Uninstallation

### Remove the package

```bash
sudo apt remove --purge rustdesk
```

### Remove the repository and key

```bash
sudo rm /etc/apt/sources.list.d/rustdesk-unofficial.list
sudo rm /usr/share/keyrings/rustdesk-unofficial.gpg
sudo apt update
```

---

## Troubleshooting

| Issue | Solution |
|---|---|
| `The following signatures couldn't be verified` | Re-import the GPG key (installation step 1). |
| `404 Not Found` when running `apt update` | Verify the repository URL is correct in the `.list` file. |
| Package doesn't appear after `apt update` | Wait a few hours — the daily update may be in progress. |
| Architecture error | Make sure you're using an `amd64` system: `dpkg --print-architecture`. |

---

## Local Build (for contributors)

This repository is managed entirely via GitHub Actions. The workflow:

1. Downloads the latest official RustDesk `.deb`.
2. Normalizes the filename to Debian format.
3. Generates `Packages`, `Packages.gz`, and `Release` metadata.
4. Signs the files with the private GPG key (stored in *Secrets*).
5. Publishes the result to GitHub Pages.

For technical details, see `.github/workflows/build-apt.yml`.

---

## License and Credits

- **RustDesk** — © RustDesk, licensed under [AGPL-3.0](https://github.com/rustdesk/rustdesk/blob/master/LICENSE).
- **Packaging scripts and repository** — provided "as is", without any warranty.

This project exists solely to facilitate distribution via `apt`. All rights to the RustDesk software belong to their respective authors.
