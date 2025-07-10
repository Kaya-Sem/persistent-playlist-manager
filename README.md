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

## How It Works

1. **Data Storage**: All playlist data is stored in `~/Music/PPM/playlists/`
2. **Hard Links**: When you add a song to a playlist, the script creates a hard link to the original file in the playlist directory
3. **M3U8 Format**: Each playlist is stored as an M3U8 playlist file with proper headers
4. **Duplicate Handling**: If multiple songs have the same filename, the script automatically renames them (e.g., `song.mp3`, `song_1.mp3`, `song_2.mp3`)

## Supported Audio Formats

The script recognizes these common audio file extensions:
- MP3 (.mp3)
- M4A (.m4a)
- FLAC (.flac)
- WAV (.wav)
- OGG (.ogg)
- AAC (.aac)
- WMA (.wma)

Other file types will work but may show a warning.

## Benefits

- **No Duplication**: Hard links ensure no disk space is wasted
- **Persistent**: Playlists survive system reboots and script updates
- **Compatible**: M3U8 format works with most media players
- **Safe**: Original files are never modified or moved
- **Organized**: Each playlist is a self-contained folder

## Troubleshooting

### Common Issues

1. **Permission Denied**: Make sure the script is executable:
   ```bash
   chmod +x ppm
   ```

2. **Song Not Found**: Ensure the file path is correct and the file exists:
   ```bash
   ls -la /path/to/your/song.mp3
   ```

3. **Playlist Not Found**: Check if the playlist exists:
   ```bash
   ./ppm list
   ```

### Data Recovery

If you need to access your playlists directly:
- Playlist folders: `~/Music/PPM/playlists/`
- Each folder contains the `.m3u8` file and hard links to songs

### Vim Integration

With this keybind, you can play tracks straight from vim/neovim
with mpv. You can change mpv with your own program.

```lua
vim.keymap.set('n', '<F6>', function()
  local line = vim.fn.getline('.')
  vim.fn.jobstart({'mpv', line}, {detach = true})
end, { noremap = true, silent = true })
```

## Technical Details

- **Shell**: Bash 4.0+
- **Dependencies**: Standard Unix tools (grep, sed, ln, etc.)
- **File System**: Requires a filesystem that supports hard links
- **Permissions**: Requires read access to source files and write access to `~/Music/PPM/`

## License

This script is provided as-is for personal use. Feel free to modify and distribute as needed. 
