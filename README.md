# dyad
Useful scripts to go in ~/bin (but they aren't binary).

Defaults are for my system. Change them if you don't like them.

## clarinet
`clarinet <pipepath>`  
Create a named pipe, send its contents to stdout forever. Supplied pipe path
must not exist. Warning: supplied pipe path will be deleted after a keyboard
interrupt.

## clipyt
`clipyt`  
Runs `youtube` below, with the url given by your X primary selection.

## edrename
`edrename [FILE] ...`  
Opens your preferred text editor (vim), with the names/paths of the above files
listed for you to edit. After the editor closes (successfully), the files are
renamed.

## urxvtsudo
`urxvtsudo <command>`  
Starts `urxvt`, running `sudo -k <command>`. Replacement for `gksudo`, in four
lines of `zsh`.

## youtube
`youtube <url>`  
Stream the given youtube video in mpv, using both `bestaudio` and `bestvideo`
options. By default, limits to 1080p, but you can remove that restriction. Note
that running `youtube-dl <url> -o - | mpv -` does not do the same thing - this
will be limited to the best stream containing both audio and video, which will
only be 720p30.
