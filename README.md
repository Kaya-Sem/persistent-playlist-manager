# Persistent Playlist Manager (PPM)

A bash script for managing persistent music playlists (albums) using M3U8 files and hard links. This tool allows you to create, manage, and organize your music collection into stable playlists. Moving or deleting the original file does not break the playlist!

## Features

- **Hard Link Management**: Songs are hard-linked to avoid duplication while maintaining file integrity
- **M3U8 Format**: Uses standard M3U8 playlist format for compatibility with media players
- **Color-coded Output**: Easy-to-read colored terminal output
- **Duplicate Handling**: Automatically handles duplicate filenames
- **Audio File Validation**: Basic validation for common audio formats
- **Flexible Input**: Supports adding/creating via arguments or stdin
- **Shuffle Play**: Uses `mpv --shuffle` by default (configurable)

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
