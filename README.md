# proxmox-perfect-woocommerce

![Proxmox VE 8.x](https://img.shields.io/badge/Proxmox_VE-8.x-E57000?logo=proxmox&logoColor=white)
![Ubuntu 24.04](https://img.shields.io/badge/Ubuntu-24.04_LTS-E95420?logo=ubuntu&logoColor=white)
![Debian 13](https://img.shields.io/badge/Debian-13_Trixie-A81D33?logo=debian&logoColor=white)
![License MIT](https://img.shields.io/badge/License-MIT-green)
[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20me%20a%20coffee-djanzin-FFDD00?logo=buy-me-a-coffee&logoColor=black)](https://buymeacoffee.com/djanzin)

One script to create a Proxmox LXC container and install a production-ready WooCommerce shop — fully automated.

---

## English

### What it does

A single bash script that runs on your **Proxmox VE host**, creates an unprivileged LXC container (Ubuntu 24.04 LTS or Debian 13 Trixie), and automatically installs a hardened, high-performance WooCommerce shop inside it using [djanzin/perfect-woocommerce](https://github.com/djanzin/perfect-woocommerce).

Everything is configured interactively — no manual editing required.

### Requirements

- Proxmox VE 8+
- Internet access on the Proxmox host
- A domain name pointing to the server (required for SSL)

### One-line install

Run this **on your Proxmox host**:

```bash
curl -fsSL https://raw.githubusercontent.com/djanzin/proxmox-perfect-woocommerce/main/create-woocommerce-lxc.sh -o /tmp/create-wc-lxc.sh && bash /tmp/create-wc-lxc.sh
```

### What the script asks for

**LXC Container:**
- Operating system: Ubuntu 24.04 LTS or Debian 13 (Trixie)
- Container ID (e.g. `100`)
- Hostname
- Root password (with confirmation)
- CPU cores, RAM (MB), Disk size (GB)
- Proxmox storage (e.g. `local-lvm`)
- Network bridge (e.g. `vmbr0`)
- MAC address (optional — auto-generated if left empty)
- Network: DHCP or static IP / Gateway / DNS

**WooCommerce:**
- Domain & admin e-mail
- Site title & admin username
- PHP version (8.1 / 8.2 / **8.3** / 8.4 / 8.5)
- PHP memory limit (128M / 256M / **512M** / 1024M)
- WordPress language & timezone
- SSL via Let's Encrypt (yes/no)
- Reverse proxy mode (NPM / Traefik / Cloudflare)
- phpMyAdmin (yes/no)
- FileBrowser (yes/no)
- Shop currency (EUR / USD / GBP / CHF)
- WooCommerce base country (e.g. `DE`, `AT`, `US:CA`)

### What gets created

| Component | Details |
|-----------|---------|
| LXC type | Unprivileged, nesting enabled |
| OS | Ubuntu 24.04 LTS or Debian 13 (Trixie) |
| Web server | Nginx + FastCGI Cache + Brotli |
| PHP | PHP-FPM (selected version) + OPcache JIT |
| Database | MariaDB (512 MB InnoDB, query cache disabled) |
| Object cache | Redis (256 MB LRU) |
| Security | UFW + Fail2ban (SSH, Nginx, WP login, WooCommerce carding jail) |
| WooCommerce | Installed, activated, configured (currency, country, pages) |
| Scheduler | Action Scheduler via system cron (every 2 min) |
| Backups | Daily MariaDB dumps, 7-day rotation |

### After installation

The script copies the credentials file from the container to the Proxmox host:

```
/root/.wc_lxc<CT_ID>_credentials_<domain>.txt
```

If the copy fails, retrieve it manually:

```bash
pct pull <CT_ID> /root/.wp_install_credentials_<domain>.txt ./credentials.txt
```

Access your shop:
- **Shop:** `https://yourdomain.com/shop`
- **WooCommerce Admin:** `https://yourdomain.com/wp-admin/admin.php?page=wc-admin`

### Based on

This script uses [djanzin/perfect-woocommerce](https://github.com/djanzin/perfect-woocommerce) for the WooCommerce installation inside the container.

<a href="https://www.buymeacoffee.com/djanzin"><img src="https://img.buymeacoffee.com/button-api/?text=Buy%20me%20a%20coffee&emoji=%E2%98%95&slug=djanzin&button_colour=00354d&font_colour=ffffff&font_family=Cookie&outline_colour=ffffff&coffee_colour=FFDD00" /></a>

---

## Deutsch

### Was es macht

Ein einzelnes Bash-Script, das auf dem **Proxmox VE Host** ausgeführt wird, einen unprivilegierten LXC Container (Ubuntu 24.04 LTS oder Debian 13 Trixie) erstellt und darin automatisch einen abgesicherten, leistungsstarken WooCommerce-Shop installiert — basierend auf [djanzin/perfect-woocommerce](https://github.com/djanzin/perfect-woocommerce).

Alles wird interaktiv konfiguriert — kein manuelles Editieren nötig.

### Voraussetzungen

- Proxmox VE 8+
- Internetzugang auf dem Proxmox-Host
- Ein Domainname der auf den Server zeigt (erforderlich für SSL)

### Ein-Befehl-Installation

Diesen Befehl **auf dem Proxmox-Host** ausführen:

```bash
curl -fsSL https://raw.githubusercontent.com/djanzin/proxmox-perfect-woocommerce/main/create-woocommerce-lxc.sh -o /tmp/create-wc-lxc.sh && bash /tmp/create-wc-lxc.sh
```

### Was das Script abfragt

**LXC Container:**
- Betriebssystem: Ubuntu 24.04 LTS oder Debian 13 (Trixie)
- Container-ID (z.B. `100`)
- Hostname
- Root-Passwort (mit Bestätigung)
- CPU-Kerne, RAM (MB), Disk-Größe (GB)
- Proxmox Storage (z.B. `local-lvm`)
- Netzwerk-Bridge (z.B. `vmbr0`)
- MAC-Adresse (optional — wird automatisch generiert wenn leer)
- Netzwerk: DHCP oder statische IP / Gateway / DNS

**WooCommerce:**
- Domain & Admin-E-Mail
- Site-Titel & Admin-Benutzername
- PHP-Version (8.1 / 8.2 / **8.3** / 8.4 / 8.5)
- PHP Memory Limit (128M / 256M / **512M** / 1024M)
- WordPress-Sprache & Zeitzone
- SSL via Let's Encrypt (ja/nein)
- Reverse Proxy Modus (NPM / Traefik / Cloudflare)
- phpMyAdmin (ja/nein)
- FileBrowser (ja/nein)
- Shop-Währung (EUR / USD / GBP / CHF)
- WooCommerce Basisland (z.B. `DE`, `AT`, `US:CA`)

### Was erstellt wird

| Komponente | Details |
|-----------|---------|
| LXC Typ | Unprivilegiert, Nesting aktiviert |
| OS | Ubuntu 24.04 LTS oder Debian 13 (Trixie) |
| Webserver | Nginx + FastCGI Cache + Brotli |
| PHP | PHP-FPM (gewählte Version) + OPcache JIT |
| Datenbank | MariaDB (512 MB InnoDB, Query Cache deaktiviert) |
| Object Cache | Redis (256 MB LRU) |
| Sicherheit | UFW + Fail2ban (SSH, Nginx, WP-Login, WooCommerce Carding Jail) |
| WooCommerce | Installiert, aktiviert, konfiguriert (Währung, Land, Seiten) |
| Scheduler | Action Scheduler via System-Cron (alle 2 Min.) |
| Backups | Tägliche MariaDB-Dumps, 7-Tage-Rotation |

### Nach der Installation

Das Script kopiert die Zugangsdaten-Datei vom Container auf den Proxmox-Host:

```
/root/.wc_lxc<CT_ID>_credentials_<domain>.txt
```

Falls das Kopieren fehlschlägt, manuell abrufen:

```bash
pct pull <CT_ID> /root/.wp_install_credentials_<domain>.txt ./credentials.txt
```

Shop aufrufen:
- **Shop:** `https://deindomain.de/shop`
- **WooCommerce Admin:** `https://deindomain.de/wp-admin/admin.php?page=wc-admin`

### Basiert auf

Dieses Script verwendet [djanzin/perfect-woocommerce](https://github.com/djanzin/perfect-woocommerce) für die WooCommerce-Installation im Container.

<a href="https://www.buymeacoffee.com/djanzin"><img src="https://img.buymeacoffee.com/button-api/?text=Buy%20me%20a%20coffee&emoji=%E2%98%95&slug=djanzin&button_colour=00354d&font_colour=ffffff&font_family=Cookie&outline_colour=ffffff&coffee_colour=FFDD00" /></a>

---

## License

MIT
