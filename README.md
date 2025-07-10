# Persistent Playlist Manager (PPM)

Create robust, space-efficient music playlists using hard links. With PPM, every track you add to an album is a true, independent file entry: if you update the metadata or tags of the original, your playlist track reflects those changes instantly. Even if you move or delete the original file, your playlist copy remains playable—so your albums are safe from accidental file moves or deletions. Thanks to hard links, there’s no duplication or wasted disk space: each track exists only once on disk, no matter how many playlists it appears in. Playlists are organized as self-contained folders with standard M3U8 files, compatible with most media players, and your original files are never modified or moved by the script.


## Installation

1. Download the script:
```bash
# Place the 'ppm' script somewhere in your PATH, e.g. ~/bin/ppm
```

2. Make it executable:
```bash
chmod +x ppm
```

3. Run it:
```bash
ppm help
```

## Usage

### Basic Commands

```bash
# Show help
ppm help

# List all playlists
ppm list

# List songs in a playlist
ppm tracks "Playlist Name"

# Create a new playlist
ppm create "Playlist Name"

# Add songs to a playlist
ppm add "Playlist Name" /path/to/song1.mp3 /path/to/song2.mp3

# Remove songs from a playlist
ppm remove-songs "Playlist Name" song1.mp3 song2.mp3

# Delete a playlist
ppm delete-playlist "Playlist Name"

# Play a playlist (with mpv by default)
ppm play "Playlist Name"
```

### Examples

#### Creating and Populating a Playlist

```bash
# Create a new playlist
ppm create "My Rock Collection"

# Add songs to the playlist
ppm add "My Rock Collection" \
    /home/user/Music/rock_song1.mp3 \
    /home/user/Music/rock_song2.mp3 \
    /home/user/Music/rock_song3.mp3

# List the songs in the playlist
ppm tracks "My Rock Collection"
```

#### Managing Multiple Playlists

```bash
# Create different genre playlists
ppm create "Jazz Classics" "Electronic Beats" "Classical Masterpieces"

# Add songs to each playlist
ppm add "Jazz Classics" /path/to/jazz/*.mp3
ppm add "Electronic Beats" /path/to/electronic/*.mp3
ppm add "Classical Masterpieces" /path/to/classical/*.mp3

# List all playlists
ppm list
```

#### Removing Songs

```bash
# Remove specific songs from a playlist
ppm remove-songs "My Rock Collection" rock_song1.mp3 rock_song2.mp3

# Check the updated playlist
ppm tracks "My Rock Collection"
```

#### Using stdin for batch operations

```bash
# Create playlists from a list
cat playlists.txt | ppm create

# Add songs from a list
cat songs.txt | ppm add "My Rock Collection"
```

## File Structure

The script creates the following directory structure in your home directory:

```
~/Music/PPM/
└── playlists/
    ├── Playlist1/
    │   ├── Playlist1.m3u8
    │   ├── song1.mp3 (hard link)
    │   ├── song2.mp3 (hard link)
    │   └── ...
    ├── Playlist2/
    │   ├── Playlist2.m3u8
    │   └── ...
    └── ...
```

### Play tracks in Vim

With this keybind, you can play tracks straight from vim/neovim
with mpv. You can change mpv with your own program.

```lua
vim.keymap.set('n', '<F6>', function()
  local line = vim.fn.getline('.')
  vim.fn.jobstart({'mpv', line}, {detach = true})
end, { noremap = true, silent = true })
```
