# Free Proxy List ⚡

[![Updated every 30 minutes](https://img.shields.io/github/last-commit/proxy-free/free-proxy-list?label=last%20update)](https://github.com/proxy-free/free-proxy-list/commits/main)
[![Proxies](https://img.shields.io/badge/proxies-2%2C600%2B-blue)](all.txt)
[![Countries](https://img.shields.io/badge/countries-78-blue)](proxies.json)
[![License](https://img.shields.io/badge/data-free%20to%20use-green)](#disclaimer)

Free, regularly tested **HTTP, SOCKS4 and SOCKS5** proxies. Every proxy here was
connected to and checked for speed, uptime and anonymity before it was committed,
and the whole list is refreshed every 30 minutes by a scheduled GitHub Action.

No signup, no key, no rate limit. The files in this repository are served by
GitHub, so pull them as often as you like.

👉 Searchable version with country, protocol, anonymity and speed filters:
**https://proxy-free.com/proxy-list/**

## What is in here

| File | Proxies | What it holds |
|------|--------:|---------------|
| [all.txt](all.txt) | ~2,670 | every protocol, one `ip:port` per line |
| [socks5.txt](socks5.txt) | ~1,975 | SOCKS5 only |
| [http.txt](http.txt) | ~531 | HTTP only |
| [socks4.txt](socks4.txt) | ~163 | SOCKS4 only |
| [https.txt](https.txt) | ~1 | see the note below |
| [proxies.json](proxies.json) | ~2,670 | the same list with country, anonymity, uptime and check time |

Counts move with every update. The numbers above are a recent snapshot.

**About https.txt.** It is nearly always close to empty and that is not a bug.
Most public proxies cannot tunnel HTTPS through `CONNECT`, so very few ever pass
the check. If you need to fetch HTTPS URLs, reach for the SOCKS5 list instead,
which carries the bulk of what is here and handles any TCP connection.

## Quick start

Grab the list:

```bash
curl -sL https://raw.githubusercontent.com/proxy-free/free-proxy-list/main/socks5.txt
```

Use one with `curl`:

```bash
curl -x socks5://IP:PORT https://api.ipify.org
curl -x http://IP:PORT   http://ip-api.com/json
```

Python, picking a random proxy and checking that it really relays:

```python
import random, requests

url = "https://raw.githubusercontent.com/proxy-free/free-proxy-list/main/socks5.txt"
proxies = requests.get(url, timeout=10).text.split()

for candidate in random.sample(proxies, 10):
    p = {"http": f"socks5://{candidate}", "https": f"socks5://{candidate}"}
    try:
        r = requests.get("http://ip-api.com/json", proxies=p, timeout=8)
        if r.status_code == 200 and r.json().get("query"):
            print(candidate, "->", r.json()["query"], r.json()["country"])
            break
    except requests.RequestException:
        continue
```

Node, filtering the JSON down to one country:

```javascript
const url = "https://raw.githubusercontent.com/proxy-free/free-proxy-list/main/proxies.json";
const { proxies } = await (await fetch(url)).json();

const german = proxies.filter(p => p.country_code === "DE" && p.anonymity === "Elite");
console.log(german.slice(0, 5));
```

## proxies.json

```json
{
  "ip": "95.3.69.222",
  "port": 8080,
  "protocol": "HTTP",
  "country": "Turkey",
  "country_code": "TR",
  "anonymity": "Anonymous",
  "uptime": 100.0,
  "last_checked": "2026-09-18T14:40:44Z"
}
```

| Field | Meaning |
|-------|---------|
| `protocol` | `HTTP`, `HTTPS`, `SOCKS4` or `SOCKS5` |
| `anonymity` | `Elite` if no headers gave away the proxy, `Anonymous` if the destination could tell one was in the path, `Transparent` if your address was passed along |
| `uptime` | percentage of recent checks this proxy answered |
| `last_checked` | UTC timestamp of the connection attempt that put it on this list |

## Proxies by country

78 countries are represented in the current list. A few of the larger ones:

- https://proxy-free.com/free-proxy-list/united-states/
- https://proxy-free.com/free-proxy-list/germany/
- https://proxy-free.com/free-proxy-list/india/
- https://proxy-free.com/free-proxy-list/netherlands/
- https://proxy-free.com/free-proxy-list/vietnam/
- https://proxy-free.com/free-proxy-list/canada/

The rest are at https://proxy-free.com/proxy-list/

## Free API

If you would rather call an endpoint than parse a file, there is a free API that
needs no key. It returns 50 proxies per call and takes one parameter, `format`,
which is either `json` or `txt`:

```bash
curl "https://proxy-free.com/free-api/proxies/?format=json"
curl "https://proxy-free.com/free-api/proxies/?format=txt"
```

It allows 10 requests per day per address. For anything heavier, or for
filtering by country and protocol, use the files in this repository instead:
they are the same data, updated just as often, with no limit. Details at
**https://proxy-free.com/free-proxy-api/**

## Check your own proxies

Paste a list into https://proxy-free.com/proxy-checker/ to get speed, country and
anonymity for each one.

## How it updates

A scheduled GitHub Action pulls the latest verified proxies from
[proxy-free.com](https://proxy-free.com) every 30 minutes and commits them here.
Every commit in this repository is one of those runs, so the history doubles as a
record of what was live at any point.

## Disclaimer

These are free, public proxies provided as is, for testing and educational use.
Whoever operates a public proxy can see the traffic you send through it, so do
not put logins, payments or anything private through one. Availability and speed
vary from one update to the next. For stable, private proxies see
https://proxy-free.com/pricing/.

---

Data source: **[proxy-free.com](https://proxy-free.com)**, updated every 30 minutes.
