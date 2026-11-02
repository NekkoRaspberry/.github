<div align="center">

# 🍓 NekkoRaspberry

<br>

**A home lab that fits in your hand.**<br>
One Raspberry Pi 4, running a small Kubernetes cluster, its own CI/CD runners and shared services for every app.

<br>

![Raspberry Pi 4](https://img.shields.io/badge/Raspberry_Pi_4-8_GB-C51A4A?style=for-the-badge&logo=raspberrypi&logoColor=white)
![K3s](https://img.shields.io/badge/K3s-Kubernetes-FFC61C?style=for-the-badge&logo=k3s&logoColor=black)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-Self--hosted-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-17-336791?style=for-the-badge&logo=postgresql&logoColor=white)

</div>

---

> [!NOTE]
> Everything here runs on **one small, silent, low-power computer** at home. No cloud servers, no monthly bills.
> The goal is a server that is **lightweight, secure and reliable enough to run 24/7**.

## 🖥️ The machine

| | |
|---|---|
| 🍓 **Hardware** | Raspberry Pi 4 Model B, 8 GB RAM |
| 💽 **Storage** | 128 GB |
| 🐧 **System** | Raspberry Pi OS Lite 64-bit (Debian 13), no desktop |
| ☸️ **Platform** | K3s, a lightweight Kubernetes |
| 🔌 **Access** | Headless, managed remotely from VS Code |
| 🌡️ **Footprint** | A few watts, around a third of its memory in use |

## 🧭 How it fits together

```mermaid
flowchart LR
    GH["🐙 GitHub<br/>repositories"] -- "push" --> ARC

    subgraph PI["🍓 Raspberry Pi 4 · K3s"]
        ARC["🏃 Actions runners<br/>(ARC)"]
        PG[("🐘 PostgreSQL<br/>shared database")]
        MP["📬 Mailpit<br/>shared email catcher"]
        PT["🧭 Portainer<br/>dashboard"]
        APPS["📦 Your apps"]
        ARC -- "tests" --> PG
        APPS -- "own database" --> PG
        APPS -- "own email login" --> MP
    end
```

**One shared service, one private space per app.** Every app gets its own database and its own email login. Apps can't see each other's data, and each one can be added or removed on its own.

## 📦 Projects

| Project | What it does | Status |
|---|---|---|
| 🏃 [**ARC-SelfHosted-K3S**](https://github.com/NekkoRaspberry/ARC-SelfHosted-K3S) | GitHub Actions runners that run **on the Pi itself**, created on demand and removed after each job | [![ARC Runner Test](https://github.com/NekkoRaspberry/ARC-SelfHosted-K3S/actions/workflows/arc-test.yml/badge.svg)](https://github.com/NekkoRaspberry/ARC-SelfHosted-K3S/actions/workflows/arc-test.yml) |
| 🐘 [**Postgres-K3s**](https://github.com/NekkoRaspberry/Postgres-K3s) | One PostgreSQL for all apps, with **one private database per app**, nightly backups and restore | [![Postgres Tests](https://github.com/NekkoRaspberry/Postgres-K3s/actions/workflows/tests.yml/badge.svg)](https://github.com/NekkoRaspberry/Postgres-K3s/actions/workflows/tests.yml) |
| 📬 [**Mailpit-K3s**](https://github.com/NekkoRaspberry/Mailpit-K3s) | Catches every email apps send and shows it in a web page, **tagged per app**. Nothing reaches real inboxes | [![Mailpit Checks](https://github.com/NekkoRaspberry/Mailpit-K3s/actions/workflows/tests.yml/badge.svg)](https://github.com/NekkoRaspberry/Mailpit-K3s/actions/workflows/tests.yml) |

## ✨ How we build

<table>
<tr>
<td width="50%" valign="top">

### 🍃 Keep it light
Small images, memory limits on every service, and nothing running when it isn't needed. CI runners only exist while a job runs.

### 🔐 Security first
Services are reachable only inside the cluster. Passwords are generated at random and never stored in git. Each CI runner gets only the permissions its tests need.

</td>
<td width="50%" valign="top">

### 🧪 Tested, not assumed
Every project ships tests for health, scripts, deployment and cleanup, and the tests check that they leave nothing behind.

### 💾 Protect the data
Daily backups with automatic catch-up after downtime, one per day, kept for a week. Fewer disk writes to make the storage last longer.

</td>
</tr>
</table>

## 🗺️ Roadmap

- [x] Raspberry Pi OS Lite 64-bit
- [x] K3s, Helm and Portainer
- [x] Self-hosted GitHub Actions runners (ARC)
- [x] Shared PostgreSQL with per-app databases and backups
- [x] Shared Mailpit with per-app email logins
- [ ] 🛡️ Pi-hole: block ads for the whole home network
- [ ] 🔒 WireGuard: secure access from anywhere
- [ ] ☁️ Nextcloud: a private cloud for files and photos
- [ ] 🚀 Build images and deploy apps automatically from GitHub

<details>
<summary><b>🔍 Under the hood</b></summary>
<br>

| Layer | Tools |
|---|---|
| Kubernetes | K3s, with Traefik and local storage built in |
| Packages | Helm |
| CI/CD | GitHub Actions + Actions Runner Controller (ARC) |
| Shared services | PostgreSQL 17, Mailpit |
| Management | Portainer, `kubectl`, VS Code over SSH |

Each project has its own README with install steps, commands and tests.

</details>

---

<div align="center">

<sub>Built and run at home on a 🍓 Raspberry Pi · Lightweight · Secure · Reliable</sub>

</div>
