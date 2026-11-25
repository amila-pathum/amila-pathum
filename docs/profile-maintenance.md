# Profile maintenance

## Design and content

Section order: hero (local SVG + typing strip + icon links) → About me (with an editable "current focus" block) → Tech stack → GitHub statistics → Contribution activity (streak, activity graph, snake) → Engineering mindset → Currently exploring → Connect → footer. The Featured/Selected Projects section was removed on purpose. The only project mention left is one SwiftDrop link and a repositories link in About me.

Palette: background `#0d1117`, violet `#a78bfa`/`#7c3aed`, blue `#60a5fa`, cyan `#22d3ee`/`#67e8f9`. Local SVGs and stats-card colour parameters all use it.

### Editing guide

- **Typing strip** (`assets/typing.svg`): one `<text class="line">` per message, `animation-delay` = index × 4s. If you change the number of lines, set the cycle to N × 4s and the visible keyframe window to 100/N %.
- **Current focus**: the three-line `text` block in About me.
- **Tech stack**: one table row per category, one `<img>` per technology. Skillicons IDs are listed at https://skillicons.dev. Skillicons has no DigitalOcean or ESP32 icon, and none for "REST APIs", so local tiles in `assets/icons/` copy its 256×256, `#242938`, rx 60 dark-tile style. The DigitalOcean glyph comes from Simple Icons (CC0). The ESP32 tile is a hand-drawn chip with the brand red. Logos remain trademarks of their owners.
- **Currently exploring**: a Markdown table. Change the Stage column (`practising` / `learning` / `next`).
- **Engineering loop** (`assets/engineering-loop.svg`): pure SVG/CSS, with no animation when reduced motion is preferred.

### Evidence for the stack

Only technologies supported by the previous README, public repositories or information I supplied are listed. Python: `dailydev-journal`. HTML: `restaurant`. React/Next.js: `portfolio`. Java/Spring Boot/PostgreSQL/MySQL/Docker: SwiftDrop and Library REST API (see below). Node.js is labelled "fundamentals" in its tooltip. **Express is not listed** because no public repository uses it. Add `<img src="https://skillicons.dev/icons?i=express" …>` to the Backend row once it is backed by a project. ESP32/Arduino/embedded systems come from my own hardware work, which has no public repository yet.

Verified public sources:

- [SwiftDrop README](https://github.com/amila-pathum/swiftdrop/blob/develop/README.md): university delivery project, microservices, Spring Boot, React, PostgreSQL and Docker Compose. No production readiness claim is made.
- [StyleCart README](https://github.com/amila-pathum/swiftdrop/blob/develop/stylecart/README.md): store inside the same repository, orders, shipment booking and delivery updates; no real payment processing.
- [MemberConnect backend](https://github.com/amila-pathum/member-connect-backend) and [frontend](https://github.com/amila-pathum/MemberConnectFrontend): forks; Maven/package manifests confirm Spring Boot, PostgreSQL, Next.js and Tailwind. These links do not imply sole authorship. (Kept as evidence; the README no longer has a project section.)
- [Library REST API](https://github.com/amila-pathum/library-rest-api): BookController and Maven dependencies confirm Spring Web MVC, JPA, validation and MySQL.

No verified public repository was found for the compost or environmental monitoring projects, so they were omitted. No live demo URLs were invented. Add those projects later when public source or demo links are available.

## Contribution automation

`.github/workflows/contribution-snake.yml` uses [Platane/snk](https://github.com/Platane/snk) to generate both palettes under `assets/`. It runs Mondays at **03:23 UTC / 08:53 Asia/Colombo**, or manually. GitHub schedules are best-effort and may be delayed.

The checked-in light/dark SVGs are explicitly labeled **first-run placeholders**, not fabricated contribution graphs. The first successful workflow run replaces both with actual animations. The README uses a picture element to select the reader's color scheme. No personal token is needed to initialize the assets locally.

Both actions are pinned to verified full commit SHAs (checkout v4.4.0, snk v3.5.0; re-verified 2026-11-25). The workflow has read access by default and grants only `contents: write` to its single publishing job. Checkout does not persist credentials. Only the two snake files are staged; an unchanged staged diff exits without a commit. There is no push trigger, so generated commits cannot loop this workflow. See [GitHub's token-trigger behavior](https://docs.github.com/en/actions/how-tos/write-workflows/choose-when-workflows-run/trigger-a-workflow).

Runs are serialized, limited to ten minutes and restricted to this repository's default branch. A competing human push causes a safe non-fast-forward failure; rerun the workflow to regenerate from the new branch head. It never force-pushes or bypasses branch protection.

## Enable and test after review

1. Review these local changes, then commit and push them to the profile repository's default branch when ready. No commit or push was made during implementation.
2. In **Settings → Actions → General**, enable Actions and allow `actions/checkout` and `Platane/snk` (including SHA-pinned references). Repository/organization policy must permit the job's `contents: write` grant. The workflow requests this explicitly; no broad default write permission or PAT is required.
3. Ensure branch rules allow the workflow token to push generated assets. Required PRs or signed commits may block this approach; do not disable protections casually. If needed, adapt publication to an allowed output branch and update README references.
4. Open **Actions → Update contribution snake → Run workflow**, select the default branch and run it. This action **will commit and push** the generated SVGs when changed.
5. Verify both SVGs contain the animated contribution grid, the bot commit changes only those files, and the README displays the appropriate image in light/dark mode.
6. Run again without contribution changes: expect “Contribution animations are unchanged.” and no new commit if the generated bytes are identical.

No custom secrets or repository variables are required: GitHub supplies `GITHUB_TOKEN`. The workflow must exist on the default branch to be dispatched. Scheduled workflows in inactive public repositories may be disabled after 60 days; re-enable from Actions if needed. See [GitHub's schedule documentation](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#schedule).

## External services and limitations

The hero and typing strip are repository-local SVGs. The snake is served from checked-in assets after generation, so it stays visible between runs even if the generator/API is unavailable.

Live cards, all public community services, all for user `amila-pathum`:

| Card | Service | Shows |
| :--- | :--- | :--- |
| Stats | [GitHub Readme Stats](https://github.com/anuraghazra/github-readme-stats) | stars, commits (last year), PRs, issues, contributed-to, rank |
| Top languages | GitHub Readme Stats | language share across public repositories (size, not skill) |
| Streak | [GitHub Readme Streak Stats](https://github.com/DenverCoder1/github-readme-streak-stats) | total contributions, active period, current and longest streak |
| Activity graph | [GitHub Readme Activity Graph](https://github.com/Ashutosh00710/github-readme-activity-graph) | daily contributions over the last month |

All four returned HTTP 200 with real SVG data on 2026-11-25. They can still be cached, rate-limited or go offline. Each card is wrapped in a link to the matching GitHub page and has descriptive alt text, so a failed card degrades to a working link. A fallback line under the contribution section points to the profile overview. No statistic is hand-entered or snapshotted. GitHub Profile Trophy was evaluated and rejected (HTTP 402). If the Vercel instances become unreliable, self-host github-readme-stats on Vercel with your own token and swap the host in the URLs.

skillicons.dev serves all tech and social icons except the three local tiles. If it is down, alt text shows the technology names.

GitHub controls page colors and HTML sanitization; the README cannot force the entire profile into dark mode. Local SVGs provide the dark visual accents. A GitHub-rendered mobile/desktop review and the first authenticated Actions run remain publishing-time checks.
