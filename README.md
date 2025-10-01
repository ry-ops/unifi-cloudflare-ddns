# 🌩️ Cloudflare DDNS for UniFi OS

[![CodeQL](https://github.com/willswire/unifi-ddns/actions/workflows/github-code-scanning/codeql/badge.svg)](https://github.com/willswire/unifi-ddns/actions/workflows/github-code-scanning/codeql)
[![Code Coverage](https://github.com/willswire/unifi-ddns/actions/workflows/coverage.yml/badge.svg)](https://github.com/willswire/unifi-ddns/actions/workflows/coverage.yml)
[![Dependabot Updates](https://github.com/willswire/unifi-ddns/actions/workflows/dependabot/dependabot-updates/badge.svg)](https://github.com/willswire/unifi-ddns/actions/workflows/dependabot/dependabot-updates)
[![Deploy](https://github.com/willswire/unifi-ddns/actions/workflows/deploy.yml/badge.svg)](https://github.com/willswire/unifi-ddns/actions/workflows/deploy.yml)

A Cloudflare Worker script that enables UniFi devices (e.g., UDM-Pro, USG) to dynamically update DNS A/AAAA records on Cloudflare.

> **Note:** This is a fork of [willswire/unifi-ddns](https://github.com/willswire/unifi-ddns). Check out the original project for updates and community support.

## Why Use This?

UniFi devices do not natively support Cloudflare as a DDNS provider. This script bridges that gap, allowing your UniFi device to keep your DNS records updated with your public IP address.

## 🚀 **Setup Overview**

### 1. **Deploy the Cloudflare Worker**

#### **Option 1: Click to Deploy**

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/ry-ops/unifi-ddns)

1. Click the button above.
2. Complete the deployment.
3. Note the `*.workers.dev` route.

#### **Option 2: Deploy with Wrangler CLI**

1. Clone this repository:
   ```sh
   git clone https://github.com/ry-ops/unifi-ddns.git
   cd unifi-ddns
   ```
2. Install [Wrangler CLI](https://developers.cloudflare.com/workers/wrangler/install-and-update/).
3. Run:
   ```sh
   npm i
   wrangler login
   wrangler deploy
   ```
4. Note the `*.workers.dev` route.

### 2. **Generate a Cloudflare API Token**

1. Go to the [Cloudflare Dashboard](https://dash.cloudflare.com/).
2. Navigate to **Profile > API Tokens**
3. Create a token using the **Edit zone DNS** template.
4. Scope the token to **one** specific zone.
5. Save the token securely.

### 3. **Configure UniFi OS**

1. Log in to your [UniFi OS Controller](https://unifi.ui.com/).
2. Go to **Settings > Internet > WAN > Dynamic DNS**.
3. Create New Dynamic DNS with the following information:
   - **Service:** `custom`
   - **Hostname:** `subdomain.example.com` or `example.com`
   - **Username:** Cloudflare Account Email Address (e.g., `you@example.com`)
   - **Password:** Cloudflare User API Token *(not an Account API Token)*
   - **Server:** `<worker-name>.<worker-subdomain>.workers.dev/update?ip=%i&hostname=%h`
     *(Omit `https://`)*

## 🛠️ **Testing & Troubleshooting**

### Verify It's Working

1. Check the DDNS status in UniFi (Settings > Internet > WAN)
2. Verify the DNS record in Cloudflare matches your public IP
3. Monitor the worker in Cloudflare Dashboard (Workers & Pages > your worker > Logs)

### Common Issues

Using this script with various Ubiquiti devices and different UniFi software versions can introduce unique challenges. If you encounter issues:

- Check that you omitted `https://` from the Server field
- Verify your API token has correct permissions (Edit zone DNS)
- Ensure the hostname exactly matches what you want in Cloudflare
- Confirm `%i` and `%h` placeholders are in the Server URL

For more help, refer to the [original project's FAQ](https://github.com/willswire/unifi-ddns/blob/main/docs/faq.md) or [discussions](https://github.com/willswire/unifi-ddns/discussions).

## 📝 **License**

This project maintains the same license as the original [willswire/unifi-ddns](https://github.com/willswire/unifi-ddns) project.

## 🙏 **Credits**

Original project by [willswire](https://github.com/willswire). This fork is maintained by [ry-ops](https://github.com/ry-ops) for personal use.
