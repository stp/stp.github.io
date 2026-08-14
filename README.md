# stp.github.io

This repository is a set of redirect stubs. **The website lives in
[stp/stp](https://github.com/stp/stp) under `web/`** and is published, together
with the Sphinx manual in `docs/`, to <https://stp.github.io/stp/>.

| old address | redirects to |
| --- | --- |
| <https://stp.github.io/> | <https://stp.github.io/stp/> |
| <https://stp.github.io/docs/> | <https://stp.github.io/stp/docs/> |
| <https://stp.github.io/live/> | <https://stp.github.io/stp/live/> |

## Why this repo still exists

GitHub reserves the repository name `stp.github.io` for the organisation's
Pages URL. That address cannot be served from `stp/stp`, so it survives here as
forwarding stubs while the sources live next to the code they document.

The redirects are HTML `meta refresh` plus a `canonical` link, because GitHub
Pages serves static files only and cannot issue a real 301.

## Editing

Don't edit the site here — changes belong in `stp/stp`. The only reason to
touch this repo is to add a stub for a new top-level path, in which case add
the directory and list it in the `cp` step of
`.github/workflows/pages.yml`.
