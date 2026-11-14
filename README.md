# BEAD

**BEAD** — **Bulk & Batch Email Attachment Downloader** for IMAP. Stop
downloading attachments one email at a time: filter, tick across pages, and
queue the whole batch once.

English | [简体中文](README.zh-CN.md)

![Selecting mails in the Inbox](docs/screenshots/inbox-multi-select.jpg)

> **This repository hosts the release builds only.** There is no application
> source code here — the binaries in
> [Releases](https://github.com/nuyoah18/BEAD/releases) are the whole content
> of this repo.

## Download

| Platform | Requirements | File |
| --- | --- | --- |
| macOS | macOS 12 or later | `BEAD-macos-universal.dmg` (Apple Silicon + Intel) |
| | | `BEAD-macos-arm64.dmg` / `BEAD-macos-x86_64.dmg` for a smaller single-arch build |
| Windows | Windows 10 / 11 · 64-bit | `BEAD-setup.exe` (installer) |
| | | `BEAD-windows-x64-live.zip` (portable, no install) |

Download from the
**[releases page](https://github.com/nuyoah18/BEAD/releases)** — each release
lists its assets and version notes. A release marked **Pre-release** is not
shown as GitHub's "Latest"; pick the highest stable tag unless you
deliberately want the newest build.

## How it works

BEAD speaks standard IMAP and connects **directly to your mail server** — no
proxy, nothing of ours in between. No scripts, no browser extension. Sign-in
uses the client authorization code / app-specific password your mailbox
issues for third-party apps, not your web login password.

Tested with Gmail, iCloud, Outlook, Yahoo, AOL, QQ Mail (including
`@foxmail.com` aliases), NetEase 163 / 126 / yeah.net, NetEase and Tencent
enterprise mailboxes (`imap.qiye.163.com`, `imap.exmail.qq.com`), Zoho,
Yandex, and any custom IMAP server.

## Features

- **Filter out the batch you need** — a filter panel combines a sent-date
  range with *unread only*, *only with attachments*, *starred only*, and text
  matching on sender address, sender name, subject and attachment name — all
  conditions apply together. Filters run on the local cache, so they are
  instant, and the active count shows on the **Filtered** chip.
- **Accumulate across pages, queue once** — check three on this page, twenty
  on the next; selections are kept per folder, and mails from several folders
  can join the same task. The toolbar always tells you what you are about to
  do (`16 emails selected · 20 attachments`).
- **Tidy files on disk** — flat, by sender, by month, or by email: pick one
  of the four filing modes per task. Duplicate file names get a suffix, and
  email dates can go into the file names.
- **One failure doesn't drag the batch** — failures are logged attachment by
  attachment while the rest keep downloading; retry just the failed ones.
- **Close the app, come back later** — tasks and per-attachment details live
  on this machine; after quitting or rebooting, hit **Resume**.
- **Safe writes** — attachments are written to temp files first and renamed
  only when complete, so a half-written file never exists.
- **See attachments before downloading** — subject, sender, read/answered
  state and every attachment's name and size come from `BODYSTRUCTURE` alone.
  Click a mail to read it as real HTML/CSS in the system WebView, with
  scripts disabled and remote images loaded only when you ask for it.
- **Works while you keep browsing** — downloads lease their own IMAP
  connection, so paging, reading or switching folders never waits for the
  queue.
- **Sync on your terms** — **Get mail** fetches only new mail, **Sync
  latest** refreshes the newest N, **Full sync** backfills the older ones,
  and **Clear cache** rebuilds the local index. The header line reports
  exactly where you stand (`1000 emails loaded · 1522 emails on the server ·
  522 older not stored`).
- **Local caching built in** — mail headers, flags and unread counts are
  cached in a per-account SQLite database, so paging, re-sorting and changing
  page size never hit the server.
- **Read-only, changes nothing** — reading never marks messages as read,
  deletes them or adds flags.
- **UI in 11 languages** — follows the system language or a manual override
  in settings.

### Filtering

![Filter panel](docs/screenshots/filter-panel.jpg)

Date range, flags and text criteria stack together; the resulting list stays
pageable and sortable. *Reset* empties the form, *Apply* runs it.

### Selecting across pages

![Selection kept while paging](docs/screenshots/cross-page-selection.jpg)

The selection counter splits the total from what the current page contributes
(`3 emails selected · 3 attachments (1 on this page · …)`), so you always
know whether a batch is complete before pressing **Bulk download**.

### Task center

![Task center with per-attachment progress](docs/screenshots/task-center.jpg)

Every task keeps its own record — mails, attachments, sizes, target folder
and filing mode — and can be re-downloaded or opened in the file manager when
finished.

## Who uses it

- **Finance & audit** — years of invoices and statements from vendors, filed
  whole by sender with email dates in the file names.
- **Design & engineering** — big design files, CAD drawings and archives that
  only arrive as attachments, pulled down in one task.
- **Legal & admin** — contracts and official documents archived by month.
- **Operations & support** — reports and media with intact names: RFC 2047,
  RFC 2231 continuation names and raw GBK byte names are decoded
  automatically, so no more garbled file names.

## Pay once, use forever

Browsing, searching, filtering and reading mail are **free forever**. Only
bulk downloading is metered:

- **No license** — each download task can include the first **3** mails. It
  is not a total quota; the next task gets its own 3.
- **Licensed** — full batches, retries and re-downloads are unlocked.

**$19 buys a lifetime license** — for one computer, Mac or Windows. No
subscription. Unlimited accounts, folders and download tasks; all four filing
modes; all UI languages.

Payment and activation happen inside the app through Dodo Payments. A
license key activates a limited number of devices — you can deactivate an old
machine and activate a new one. Buying for a team? Multi-license checkout is
available (e.g. 2 licenses · $38).

## Install

### macOS

1. Open the `.dmg` and drag **BEAD** into `/Applications`. Use the universal
   build unless you specifically want the smaller arch-specific one.
2. The build is signed ad-hoc (not notarized by Apple), so the first launch is
   blocked by Gatekeeper. One of these gets past it:
   - Right-click the app in Finder → **Open** → confirm **Open**; or
   - `xattr -dr com.apple.quarantine /Applications/BEAD.app`

### Windows

1. Run `BEAD-setup.exe` and follow the installer, or extract
   `BEAD-windows-x64-live.zip` and run `BEAD.exe` from the extracted folder.
   Do not run it from inside a compressed archive viewer.
2. No separate runtime is needed — the VC++ runtime DLLs are bundled. Reading
   a mail body requires the **WebView2 runtime** — preinstalled on Windows 11
   and on most Windows 10 machines with Edge; if it is missing, the reader
   pane offers a download link.

## Quick start

1. In your mailbox's web settings, enable IMAP and generate a **client
   authorization code / app-specific password** (NetEase: POP3/SMTP/IMAP
   settings; QQ Mail: Settings → Account; Gmail, Yahoo, etc.: app passwords;
   enterprise mailboxes: IMAP is usually enabled by the admin).
2. Launch the app, pick the mailbox **type** first, then enter your email
   address and the authorization code.
3. Choose a folder in the sidebar, tick the mails you need (optionally
   narrow the list with **Filter** first), and press **Bulk download**.
   Confirm the target folder and filing mode, and the task starts.
4. Follow progress in the **Task Center**, which opens from the sidebar.

## FAQ

**How do I bulk-download attachments?**
Connect your mailbox, filter or search the list, tick messages across as many
pages as you like, and press **Bulk download**. Selection accumulates across
pages and folders until you queue it.

**Is there a cap on batch size?**
No cap. Tens of thousands of emails as one task is normal use. Within a task,
attachments run one at a time so the mail server is not overwhelmed; the UI
stays responsive.

**Will downloading change my mailbox?**
No. BEAD is read-only — it never marks messages as read, deletes them or adds
flags on the server.

**What is an app password / authorization code?**
A password your mailbox issues specifically for third-party apps, so your
web login password is never handed to a desktop client.

**What if an attachment fails?**
The rest keep downloading; the failure is logged attachment by attachment and
you can retry just those. Files are written to temp files first and renamed
only when complete, so you never get a truncated download.

**Are garbled attachment names fixed?**
Yes — RFC 2047, RFC 2231 continuation names and raw GBK byte names are
decoded automatically.

## Privacy and local data

- The app talks to your IMAP server and the license service only — no
  analytics, no telemetry.
- **Credentials never touch the disk.** The authorization code is kept in
  memory only and is cleared on sign-out.
- Account settings, list preferences, the local mail cache and the license
  file live under the app's Application Support directory
  (`~/Library/Containers/com.nuyoah.bead/Data/Library/Application Support/com.nuyoah.bead/`
  on macOS). Deleting them resets the app to a fresh state.

## Known limitations

- Attachments are read into memory before being written to disk; very large
  single attachments (hundreds of MB) need comparable free RAM.
- Downloads within a task run one attachment at a time.
- Only the newest mails of a folder are cached by default — older ones are
  fetched on demand, or after **Full sync**.

## Feedback and links

- Website: [nuyoah.fun](https://nuyoah.fun)
- Releases: [github.com/nuyoah18/BEAD/releases](https://github.com/nuyoah18/BEAD/releases)
- Issues: [github.com/nuyoah18/BEAD/issues](https://github.com/nuyoah18/BEAD/issues) —
  please include your platform, the version from the About window, and the
  steps to reproduce.
- Gitee mirror: [gitee.com/nuyoah18/bead](https://gitee.com/nuyoah18/bead)
- Email: nuyoah@nuyoah.fun

Each release is tagged (`v1.0.5`, …) and the tag's assets are the builds for
that version; see the releases page for what changed in each build.
