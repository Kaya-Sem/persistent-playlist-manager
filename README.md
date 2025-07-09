# Persistent Album Creator

A bash script for managing persistent music albums using M3U8 files and hard links. This tool allows you to create, manage, and organize your music collection into albums that persist across sessions.

## Features

- **Persistent Storage**: Albums are stored in your home directory (`~/.album-manager/`)
- **Hard Link Management**: Songs are hard-linked to avoid duplication while maintaining file integrity
- **M3U8 Format**: Uses standard M3U8 playlist format for compatibility with media players
- **Color-coded Output**: Easy-to-read colored terminal output
- **Duplicate Handling**: Automatically handles duplicate filenames
- **Audio File Validation**: Basic validation for common audio formats

## Installation

1. Download the script:
```bash
# Clone or download the album-manager.sh file
```

2. Make it executable:
```bash
chmod +x album-manager.sh
```

3. Run it:
```bash
./album-manager.sh help
```

## Usage

### Basic Commands

```bash
# Show help
./album-manager.sh help

# List all albums
./album-manager.sh list-albums

# List songs in an album
./album-manager.sh list-songs "Album Name"

# Create a new album
./album-manager.sh create-album "Album Name"

# Add songs to an album
./album-manager.sh add-songs "Album Name" /path/to/song1.mp3 /path/to/song2.mp3

# Remove songs from an album
./album-manager.sh remove-songs "Album Name" song1.mp3 song2.mp3

# Delete an album
./album-manager.sh delete-album "Album Name"
```

### Examples

#### Creating and Populating an Album

```bash
# Create a new album
./album-manager.sh create-album "My Rock Collection"

# Add songs to the album
./album-manager.sh add-songs "My Rock Collection" \
    /home/user/Music/rock_song1.mp3 \
    /home/user/Music/rock_song2.mp3 \
    /home/user/Music/rock_song3.mp3

# List the songs in the album
./album-manager.sh list-songs "My Rock Collection"
```

#### Managing Multiple Albums

```bash
# Create different genre albums
./album-manager.sh create-album "Jazz Classics"
./album-manager.sh create-album "Electronic Beats"
./album-manager.sh create-album "Classical Masterpieces"

# Add songs to each album
./album-manager.sh add-songs "Jazz Classics" /path/to/jazz/*.mp3
./album-manager.sh add-songs "Electronic Beats" /path/to/electronic/*.mp3
./album-manager.sh add-songs "Classical Masterpieces" /path/to/classical/*.mp3

# List all albums
./album-manager.sh list-albums
```

#### Removing Songs

```bash
# Remove specific songs from an album
./album-manager.sh remove-songs "My Rock Collection" rock_song1.mp3 rock_song2.mp3

# Check the updated album
./album-manager.sh list-songs "My Rock Collection"
```

## File Structure

The script creates the following directory structure in your home directory:

```
~/.album-manager/
├── albums/          # M3U8 playlist files
│   ├── Album1.m3u8
│   ├── Album2.m3u8
│   └── ...
└── songs/           # Hard-linked song files
    ├── song1.mp3
    ├── song2.mp3
    └── ...
```

## How It Works

1. **Data Storage**: All album data is stored in `~/.album-manager/`
2. **Hard Links**: When you add a song to an album, the script creates a hard link to the original file in the songs directory
3. **M3U8 Format**: Each album is stored as an M3U8 playlist file with proper headers
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
- **Persistent**: Albums survive system reboots and script updates
- **Compatible**: M3U8 format works with most media players
- **Safe**: Original files are never modified or moved
- **Organized**: Clear separation between albums and song storage

## Troubleshooting

### Common Issues

1. **Permission Denied**: Make sure the script is executable:
   ```bash
   chmod +x album-manager.sh
   ```

2. **Song Not Found**: Ensure the file path is correct and the file exists:
   ```bash
   ls -la /path/to/your/song.mp3
   ```

3. **Album Not Found**: Check if the album exists:
   ```bash
   ./album-manager.sh list-albums
   ```

### Data Recovery

If you need to access your albums directly:
- Album files: `~/.album-manager/albums/`
- Song links: `~/.album-manager/songs/`

## Technical Details

- **Shell**: Bash 4.0+
- **Dependencies**: Standard Unix tools (grep, sed, ln, etc.)
- **File System**: Requires a filesystem that supports hard links
- **Permissions**: Requires read access to source files and write access to `~/.album-manager/`

## License

This script is provided as-is for personal use. Feel free to modify and distribute as needed. 