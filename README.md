# unstop-cli

Unofficial command-line interface for [Unstop.com](https://unstop.com) — browse hackathons, internships, jobs, competitions, quizzes, workshops and scholarships from your terminal, log in, and even register into events.

Built to be **agent-friendly**: every flow works non-interactively (`--json`, `--otp`, `-y`), and there's a browser login bridge for headless machines.

```text
$ unstop hackathons -k ai
$ unstop closing -d 7
$ unstop login --expose          # headless: owner completes login in their browser
$ unstop register 1743077 -g M -y
```

## Features

- **List opportunities** — hackathons, internships, jobs, competitions, quizzes, workshops, scholarships (server-side keyword filter + pagination)
- **Login, every way Unstop allows** — OTP (email or mobile), email+password, refresh-token exchange, pasted access token, a **browser login bridge** (`login --expose`) for headless machines where the owner completes login from any device, and a **social bridge** (`login-social`) for Google/Facebook/LinkedIn provider tokens. All sessions are saved server-side; tokens never touch the browser.
- **Account commands** — `me`, `whoami`, `status`, `logout`
- **Your activity** — `registrations`, `my-competitions`, `certificates`
- **Register into events** — `register <id>` with eligibility pre-check; guided mobile-verification path when an event requires it
- **Deadline tracking** — `closing -d 7`; local watchlist with countdowns (`watch add/list/rm`)
- **Global search**, event details, top-college stats, organisations, platform stats, blog reader
- **JSON output** everywhere via `--json`

## Install

```bash
git clone https://github.com/mwmQi/unstop-cli.git
cd unstop-cli
sudo cp unstop /usr/local/bin/        # or: cp unstop ~/bin/ if ~/bin is on PATH
unstop types                          # sanity check
```

Requirements: Python 3.8+ (stdlib only). Optional: the `cryptography` package speeds up encrypted-response handling; without it an `openssl` fallback is used automatically.

## Usage

### Browsing (no login needed)

```text
unstop <type>                 list live items (hackathons/internships/jobs/
                              competitions/quizzes/workshops/scholarships)
unstop <type> -k <keyword>    server-side keyword filter
unstop <type> -p 2 -n 20      page / page size
unstop closing -d 7           everything closing within 7 days
unstop search "ai hackathon"  global search across categories
unstop detail <id>            full details for one opportunity
unstop stats <id>             top colleges registered in an event
unstop orgs                   browse organisations
unstop platform               platform-wide stats
unstop open <id>              print/open the opportunity URL
unstop blogs [keyword]        Unstop blog posts (optional keyword filter)
unstop blog <slug|id>         full text of one post
unstop cats                   blog category ids
unstop tags                   platform tags
unstop types                  supported opportunity types
unstop --json <cmd>           raw JSON output for scripting
```

### Login

```text
unstop login                      interactive OTP login (asks email/mobile + code)
unstop login me@mail.com --otp 123456     fully non-interactive
unstop login 9876543210           mobile login (OTP by SMS)
unstop login-password me@mail.com [-p PASS]   classic email+password accounts
unstop login-refresh [TOKEN]      exchange a refresh_token for a fresh session
unstop login-social               Google / Facebook / LinkedIn bridge:
                                  serves a page where you paste a provider
                                  token (or the LinkedIn ?code=) — the CLI
                                  exchanges it server-side; includes a
                                  one-click LinkedIn authorize link
unstop login --expose             AGENTS/HEADLESS: serve an email/mobile OTP
                                  page on 0.0.0.0:8777 — owner completes login
                                  in their browser; session saved server-side
unstop login --expose --port 9000 --public-ip myhost.example.com
unstop login-qr                   print a scannable QR of the expose URL
unstop login-token [TOKEN]        paste an existing access token instead
unstop whoami / status / logout   session info & teardown
```

The session is stored at `~/.config/unstop/auth.json` with `0600` permissions.
Tokens last ~28 days; when one expires, just `unstop login` again.

### Your account (after login)

```text
unstop me                     profile summary
unstop registrations          opportunities you registered in
unstop my-competitions        competitions dashboard data
unstop certificates           your certificates + download links
```

### Registering into events

```text
unstop register <id>                      interactive (asks gender + confirm)
unstop register <id> -g M -y              non-interactive
unstop register <id> -g M --name "Name"   override display name
unstop mobile add +91XXXXXXXXXX           add & verify mobile via OTP
                                          (some events require it)
```

Gender codes: `F` `M` `T` `I` `NB` `O`. Registration creates an individual team of one, same as registering alone on the website.

### Watchlist

```text
unstop watch add <id>         track an opportunity locally
unstop watch list             tracked items + deadline countdown
unstop watch rm <id>          stop tracking
```

## Notes & limitations

- Browsing needs no account. Login adds account commands and registration.
- Credentials never leave your machine except toward Unstop itself; only the issued session token is stored locally.
- Events with paid entry, extra form fields, or team minimums may need the website.
- Be respectful: keep request rates around ~1 req/sec.
- This is an unofficial tool, not affiliated with or endorsed by Unstop.com.

## License

MIT — see [LICENSE](LICENSE).
