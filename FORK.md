nchat (media fork)
==================

Unofficial fork of [d99kris/nchat](https://github.com/d99kris/nchat) focused
on media inside the terminal. Upstream is in maintenance mode
([doc/SCOPE.md](/doc/SCOPE.md)) and does not accept features of this size, so
they live here.

Roadmap
-------
WhatsApp first (`lib/wmchat`). Telegram and Signal later, since the rendering
side lives in the UI (`src/`) and applies to all protocols.

| # | Feature | Approach | Status |
|---|---------|----------|--------|
| 1 | Inline audio | `Ctrl+V` on an audio message plays it in the background with `mpv`; again to stop | todo |
| 2 | Inline images | kitty graphics / sixel, `chafa` Unicode blocks as fallback | todo |
| 3 | Stickers | convert `.webp` (WhatsApp) and `.tgs`/`.webm` (Telegram); first frame to start | todo |
| 4 | Video | inline thumbnail, `Enter` opens in `mpv` | todo |

Core challenge for 2–4: nchat draws with ncurses, which knows nothing about
images. They must be written outside ncurses, over the message's cells, and
repositioned on every scroll/redraw.

Development setup (Ubuntu 24.04)
--------------------------------
System dependencies:

    sudo apt install ccache cmake build-essential gperf help2man libreadline-dev \
      libssl-dev libncurses-dev libncursesw5-dev zlib1g-dev libsqlite3-dev \
      libmagic-dev wl-clipboard mpv chafa

Go: `whatsmeow` requires Go >= 1.26 and Ubuntu 24.04 apt only ships 1.22. Use
the official tarball in `~/.local/go`:

    curl -fsSLO https://go.dev/dl/go1.27.2.linux-amd64.tar.gz
    tar -C ~/.local -xzf go1.27.2.linux-amd64.tar.gz
    export PATH="$HOME/.local/go/bin:$PATH"   # add to your shell rc

Build:

    ./make.sh build        # full build (includes tdlib, slow the first time)
    ./build/bin/nchat      # run without installing

While the focus is WhatsApp only, skip tdlib (the slowest step):

    mkdir -p build && cd build
    cmake -DHAS_TELEGRAM=OFF -DHAS_SIGNAL=OFF .. && make -j"$(nproc)"

To keep it apart from a regular install, use a separate config directory:
`./build/bin/nchat -d ~/.nchat-dev`.

Git workflow
------------
- `origin` = this fork, `upstream` = d99kris/nchat.
- `master` tracks upstream; each feature on its own branch (`feat/audio-inline`).
- Update: `git fetch upstream && git rebase upstream/master` on the branch.

Contributors
------------
- [@jaopdc11](https://github.com/jaopdc11)
- [@gabriel-pinotti](https://github.com/gabriel-pinotti)
