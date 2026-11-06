# Google Dorks Cheat Sheet

![Version](https://img.shields.io/badge/version-2.3.0-blue)

A one-page reference for Google search operators and the dorks people actually use.

I put this together because most dork lists online are years out of date. They still tell you to use `cache:` and `link:`, which Google removed a while ago. Everything here has been checked against Google's own docs, and the operators are grouped by whether they still work.

If you want the interactive version, with a dork builder and a searchable list of 200+ dorks, it's all at [google-dorks.com](https://google-dorks.com/).

**Contents:** [The basics](#the-basics) · [Operators](#operators) · [Combining them](#combining-operators) · [Dorks](#dorks) · [If you find something](#if-you-find-something-on-your-own-site) · [Things to know](#things-to-know)

---

## The basics

A Google dork is just a search that uses operators to narrow down results. It doesn't hack anything. Every result is a page Google has already indexed and will show to anyone who searches for it.

A few things catch people out:

- Don't put a space after the colon. `site:example.com` works, `site: example.com` doesn't.
- `OR` has to be in capitals, otherwise Google treats it as a normal word.
- No space after the minus sign either: `-draft`, not `- draft`.
- The `+` sign doesn't do anything anymore. Use quotes for exact matches.
- Use brackets to group things: `(admin OR login)`.

---

## Operators

### Documented by Google

These are the six Google officially supports. They're the safest to rely on.

| Operator | What it does | Example |
|---|---|---|
| `"..."` | Exact phrase | `"internal use only"` |
| `site:` | Search one site, subdomain or folder | `site:example.com/blog` |
| `-` | Leave out a word or site | `-site:www.example.com` |
| `filetype:` | Only one type of file | `filetype:pdf` |
| `after:` | Updated after a date | `after:2024-01-01` |
| `before:` | Updated before a date | `before:2025` |

### Undocumented, but they work

Google doesn't officially support these, so they could change at any time. For now they work fine.

| Operator | What it does | Example |
|---|---|---|
| `intitle:` | Word in the page title | `intitle:"index of"` |
| `allintitle:` | All words in the title | `allintitle:admin login` |
| `inurl:` | Word in the URL | `inurl:admin` |
| `allinurl:` | All words in the URL | `allinurl:wp-content uploads` |
| `intext:` | Word in the page body | `intext:"powered by"` |
| `allintext:` | All words in the body | `allintext:username password` |
| `ext:` | Same as `filetype:` | `ext:log` |
| `OR` or `\|` | Either one | `ext:sql \| ext:bak` |
| `*` | Wildcard inside quotes | `"default * password"` |
| `..` | Number range | `"report" 2020..2024` |

### Hit and miss

- `AROUND(n)` was never documented and works sometimes.
- `info:` has been unreliable since 2017.
- `related:` has been on its way out since July 2023.

### Gone

| Operator | Removed | Use this instead |
|---|---|---|
| `cache:` | September 2024 | The [Wayback Machine](https://web.archive.org/) |
| `link:` | 2017 | Search Console or a backlink tool |
| `+` | October 2011 | Quotes |

---

## Combining operators

The real power comes from stacking operators. Here's a typical one:

```
site:example.com filetype:pdf "annual report" -draft after:2024
```

That reads as: PDFs on example.com, containing the exact phrase "annual report", but not the word "draft", updated since 2024.

Some patterns worth remembering:

| You want | Search for |
|---|---|
| A certain file type on a site | `site:example.com filetype:pdf` |
| A few file types at once | `site:example.com (ext:xls OR ext:csv)` |
| Subdomains other than www | `site:*.example.com -site:www.example.com` |
| Mentions of a site on other sites | `"example.com" -site:example.com` |
| Only recent pages | `site:example.com "keyword" after:2024-01-01` |

---

## Dorks

Swap `example.com` for your own domain, or one you have permission to test.

**Documents**
```
site:example.com filetype:pdf
site:example.com (filetype:xls OR filetype:xlsx OR filetype:csv)
site:example.com (filetype:doc OR filetype:docx)
site:example.com filetype:pdf "internal use only"
site:example.com filetype:pdf "confidential" after:2024
```

**Open directories**
```
site:example.com intitle:"index of"
site:example.com intitle:"index of" (backup OR bak OR old)
site:example.com intitle:"index of" "parent directory"
```

**Config files, logs and backups**
```
site:example.com filetype:log
site:example.com (ext:conf OR ext:cfg OR ext:ini)
site:example.com (ext:sql OR ext:db OR ext:bak)
site:example.com ext:env
```

**Login pages**
```
site:example.com inurl:login
site:example.com (inurl:admin OR inurl:dashboard)
site:example.com inurl:wp-admin
```

**Subdomains and dev sites**
```
site:*.example.com -site:www.example.com
site:example.com (inurl:dev OR inurl:staging OR inurl:test)
site:example.com (inurl:swagger OR inurl:api-docs)
site:example.com ("stack trace" OR "fatal error")
```

**OSINT**
```
"name@example.com" -site:example.com
site:pastebin.com "example.com"
site:github.com "example.com"
site:linkedin.com/in "Example Ltd"
```

**Bug bounty**
```
inurl:/.well-known/security.txt
"vulnerability disclosure" inurl:security
intext:"bug bounty" inurl:security
```

---

## If you find something on your own site

1. Delete the file, put it behind a login, or turn off directory listing.
2. Add a `noindex` tag (`<meta name="robots" content="noindex">`) or send an `X-Robots-Tag: noindex` header.
3. Ask Google to remove it through [Search Console](https://search.google.com/search-console).

One common mistake: adding the page to `robots.txt` does **not** remove it from Google. It just stops Google crawling it, which means Google never sees your `noindex` tag. The page can sit in search results indefinitely.

---

## Things to know

- `filetype:` gives you a sample of files, not all of them.
- `before:` and `after:` rely on Google's guess of when a page was updated, and it's often wrong.
- The result count on a `site:` search is a rough estimate.
- Bing and DuckDuckGo understand `site:` and `filetype:`, but not much else. It's worth checking more than one search engine.

---

## Use it sensibly

Running a search is fine. You're only looking at what Google has already indexed. What you do with the results is a different matter. Stick to sites you own or have permission to test. Personal data is still covered by GDPR even if it's publicly visible.

If you come across exposed data by accident, stop there, don't download anything, and let the site owner know. Most sites list a security contact at `/.well-known/security.txt`.

---

## Contributing

Pull requests are welcome. If you're adding a dork, please include what it finds and how to fix it. If an operator has changed, link to where you saw that.

---

Not affiliated with Google. This is for learning and auditing your own sites, not legal advice.
