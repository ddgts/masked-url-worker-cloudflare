# masked-url-worker

Cloudflare Worker for URL masking (reverse proxy) with safe client splitting:

- **Browsers**: masked URL stays visible (e.g. `movie1.yourdomain.com/...`)
- **IPTV apps / Smart STB / MAG boxes / Tivimate / iSTB / MyTVOnline+**: **instant redirect** to the upstream original URL (most stable)
- **Bots / link previews / non-browser clients**: redirect to upstream
- **Fail-open**: if the upstream returns a WAF/Cloudflare block (403/429/503), the Worker redirects the browser to the original upstream instead of showing a block page.

This repo is designed so you only edit:
- `ORIGIN_MAP` in `src/worker.js`
- optionally: add User-Agent patterns for new apps in `isIptvOrStbClient()`

## Quick start

1. Create a Cloudflare Worker and paste the code from:
   - `src/worker.js`
2. Add DNS records for your subdomains (proxied / orange cloud).
3. Add a Worker route:
   - `*.yourdomain.com/*` (or only specific subdomains)
4. Edit `ORIGIN_MAP` to point each vanity host to an upstream origin.

Detailed steps: see **SETUP.md**.

## Safety notes

- This project **does not** attempt to bypass upstream security controls.
- If an upstream blocks masked browsing, the Worker redirects users directly to the upstream.

## Example vanity hosts

Examples included for 20+ subdomains using `yourdomain.com`.  
See `examples/origin-map.example.json`.

## License

MIT (see LICENSE).
