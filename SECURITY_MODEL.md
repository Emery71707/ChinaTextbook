# Security model: Emery71707/ChinaTextbook

*Priorities and attackers below were confirmed by the repository owner (sections marked **[owner]**). The other sections describe the repository contents. Security scans start from this file.*

## System overview
- Public GitHub **fork of `TapXWorld/ChinaTextbook`**. The fork adds no content of its own and only syncs from upstream.
- **No source code, CI, workflows or deployment.** There is no `.github/` directory and nothing runs server-side. The repo is a static file distribution served by GitHub.
- Contents: about 1,900 textbook PDFs under `小学/`, `初中/`, `高中/`, `大学/`, `*（五•四学制）/`, `学数学最重要的刷习题在这里/`. `*.pdf_merge_folder/` directories hold PDFs split into `*.pdf.1`, `*.pdf.2`, … (GitHub file size limit). There is also one `.djvu` (`大学/离散数学/…djvu`).
- **Committed executable:** `高中/英语/重庆大学版-重庆大学出版社/mergePDFs.exe` (about 1.9 MB, Windows PE).
- `README.md` tells users to download `mergePDFs-windows-amd64.exe` from `https://github.com/TapXWorld/ChinaTextbook-tools/releases` and double-click it inside the folder of split PDFs. It also links Telegram, `tchMaterial-parser`, `dxsx.net` (plain http), and shows an Alipay donation QR (`.cache/support-alipay.png`).
- No infrastructure repository exists.

## Trust boundaries
1. **Upstream → fork:** content synced from `TapXWorld/ChinaTextbook` is trusted as-is. Nothing verifies it (no checksums, no signatures).
2. **Repo → downloader's machine:** users open the PDFs in local readers and run the `.exe` on Windows. The repo's only security impact lands here.
3. **README → third-party sites:** links send users to external releases, Telegram, and http sites.

## Components
| Name | Covers |
|---|---|
| **Textbook files** | all `*.pdf`, `*.pdf.N` split parts, `*.djvu` under the subject directories |
| **Merge tool** | `高中/英语/重庆大学版-重庆大学出版社/mergePDFs.exe` and the README's merge-tool download and run instructions |
| **README & links** | `README.md` (external links, Telegram, instructions) |
| **Donation QR** | `.cache/support-alipay.png` and its README embed |

## Entry points and who can reach them
- Read and download: anyone on the internet (public repo).
- Write: the fork owner. Upstream maintainers and contributors reach it indirectly when the owner syncs.

## Assets **[owner]**
- **Most important:** integrity of the committed `mergePDFs.exe` and the PDFs (including split parts) as received by downloaders. A trojaned executable, or a PDF/DjVu carrying an exploit payload, is the outcome that matters.
- Secondary: README links and the donation QR.
- No secrets, credentials, user data or servers.

## Attacker profiles **[owner]**
- **In scope:** a compromised or malicious upstream maintainer or contributor who slips tampered content into `TapXWorld/ChinaTextbook`, which the fork then syncs.
- Someone with write access to the fork itself is also covered by the same integrity concern.
- Anonymous readers have no write path.

## Existing controls
- GitHub access control on the fork. Nothing else: no checksums or signatures for the `.exe` or PDFs, and no CI.

## Out of scope / accepted risk
- Copyright and licensing of the textbooks. The educational content itself. (Assumed; not yet confirmed by the owner.)

## Finding tags **[owner]**
- `Component` only (the names in the Components table).
