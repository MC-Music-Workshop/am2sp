# am2sp

Turn an Apple Music link (or the song playing in Music) into a Spotify link for the same song or album. The link goes to your clipboard and a notification shows what was matched.

## Install

```sh
brew install mc-music-workshop/tap/am2sp
```

Then add the Shortcut, which puts **Copy Spotify Link** in the Services menu (including Music's) and the menu bar:

```sh
open "$(brew --prefix am2sp)/share/am2sp/Copy Spotify Link.shortcut"
```

1. Click **Add Shortcut**.
2. In Shortcuts → Settings → Advanced, turn on **Allow Running Scripts**.

## Use

- **In Music:** right-click a song → Share → Copy Link, then Music → Services → **Copy Spotify Link**.
- **Song that's playing:** with no Apple Music link on the clipboard, **Copy Spotify Link** uses Music's current track.
- **Terminal:**

  ```sh
  am2sp 'https://music.apple.com/us/album/exponent/1553732538?i=1553732948'
  am2sp   # reads the clipboard, then falls back to Music's current track
  ```

## How it matches

The result is a Spotify search link that lands on the exact song or album, for example `https://open.spotify.com/search/isrc%3ADED832001385`.

1. **Apple Music link:** [song.link](https://song.link) maps the Apple Music ID to a Deezer ID. Deezer's public API returns the song's ISRC or the album's UPC, and Spotify's search finds the item with that code.
2. **Song that's playing:** Music doesn't expose the Apple Music ID, so `am2sp` searches Deezer for the title, artist and album and takes the ISRC from a result with the same title and artist.
3. **No exact match:** you get a Spotify search for the title and artist instead. The notification says which kind of link you got.

It needs no accounts or API keys, and uses only tools built into macOS (`curl`, `plutil`, `osascript`, `pbcopy`).

> **Note:** this depends on song.link's page layout and Deezer's public API. If either changes, `am2sp` quietly falls back to the title + artist search.

## Develop

`shortcut.plist` is the source for `Copy Spotify Link.shortcut`. Rebuild it with:

```sh
plutil -convert binary1 -o /tmp/u.shortcut shortcut.plist
shortcuts sign -m anyone -i /tmp/u.shortcut -o "Copy Spotify Link.shortcut"
```

The Shortcut runs `am2sp` from `~/bin`, `/opt/homebrew/bin` or `/usr/local/bin`, so a symlink in `~/bin` overrides the Homebrew install while you work on it.

## License

[MIT](LICENSE)
