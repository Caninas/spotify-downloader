# spotify-downloader

Spotify song downloader using API created by [lucida.to](https://lucida.to/) _(Source code can be found [here](https://git.gay/lucida/lucida))_

The script `src/spotify_dl.py` can be run in interactive mode or CLI mode.

## Interactive mode

When run without any arguments, interactive mode is used.  The user is prompted for URLs of songs or playlists.  If a playlist is given, the user has the option to download individual songs from that playlist or all of them as well as the ability to see the songs in the playlist prior to making a decision.  The default download directory is the user's `Downloads/` directory, e.g. `C:\Users\[USER]\Downloads\`.  The user is prompted if they want to change the directory prior to downloading the songs.

## CLI mode

```shell
usage: spotify_dl.py [-h] [-u URLS [URLS ...]] [-f FILENAME] [-d {lucida,spotifydown}] [-t {mp3-320,mp3-256,mp3-128,ogg-320,ogg-256,ogg-128,original}] [-o OUTPUT] [-c] [-s]
                     [-k CONFIG_FILE] [--retry-failed-downloads RETRY_FAILED_DOWNLOADS] [--cfg-file CFG_FILE] [--debug]

optional arguments:
  -h, --help            show this help message and exit
  -u URLS [URLS ...], --urls URLS [URLS ...]
                        URL(s) of Sptofy songs or playlists to download. If a playlist is given, append "|[TRACK NUMBERS]" to URL to specify which tracks to download. Example:
                        'https://open.spotify.com/playlist/mYpl4YLi5T|1,4,15-' to download the first, fourth, and fifteenth to the end. If not specified, all tracks are downloaded.    
  -f FILENAME_TEMPLATE, --filename-template FILENAME_TEMPLATE
                        Specify custom filename template using variables '{title}', '{artist}', and '{track_num}'.
  -d {lucida,spotifydown}, --downloader {lucida,spotifydown}
                        Specify download server to use.
  -t {mp3-320,mp3-256,mp3-128,ogg-320,ogg-256,ogg-128,original}, --file-type {mp3-320,mp3-256,mp3-128,ogg-320,ogg-256,ogg-128,original}
                        Specify audio file format to download. Must be one of mp3-320, mp3-256, mp3-128, ogg-320, ogg-256, ogg-128, original.
  -o OUTPUT, --output OUTPUT
                        Path to directory where tracks should be downloaded to
  -c, --create-dir      Create the output directory if it does not exist.
  -s, --skip-duplicate-downloads
                        Don't download a song if the file already exists in the output directory.
  -k CONFIG_FILE, --config-file CONFIG_FILE
                        Path to JSON containing download instructions.
  --retry-failed-downloads RETRY_FAILED_DOWNLOADS
                        Number of times to retry failed downloads.
  --cfg-file CFG_FILE
                        Path to .cfg file used for user default settings if not using `$HOME/.spotify_dl.cfg`.
  --debug               Debug mode.
```

### Cfg file

Not to be confused with the "config" file below (bad name, I know. I'll change it at some point), The user can define a file named `.spotify_dl.cfg` in their home directory (in Windows, it's `C:\Users\[Your user]\`) to define settings that will always be used by this tool.  The `Settings` section **must** be defined.

Example:

```
[Settings]
default_download_location="C:\Users\me\Desktop\folder"
default_filename_template="{artist} - {title}"
```

The following values are supported:
* `default_download_location`: Path to directory/folder to download tracks to.
* `default_downloader`: Downloader to use.  Either `lucida` or `spotifydown`.
* `default_file_type`: Audio file type to download _(Only applicable when using Lucida)_.
* `default_filename_template`: Filename format template to use when naming downloads.
* `default_retry_downloads_attempts`: Number of attempts to retry downloading tracks that failed to download.


### Config file

Example of JSON file used to contain download instructions:

```json
[
  {
    "url": "https://open.spotify.com/playlist/mYpl4YLi5T"
  },
  {
    "url": "https://open.spotify.com/playlist/mYpl4YLi5T2",
    "output_dir": "/path/to/dir/",
    "create_dir": true,
    "filename_template": "{id} - {artist} - {title}"
  },
  {
    "url": "https://open.spotify.com/playlist/mYpl4YLi5T3",
    "skip_duplicate_downloads": true
  }
]
```

The following arguments can be specified in entries within the config JSON:
* `url` (Required)
* `output_dir`
* `create_dir`
* `skip_duplicate_downloads`
* `filename_template`
* `file_type`
There types and use match those of the CLI arguments.  

_The arguments `--retry-failed-downloads` and `--cfg-file` do not need to be put into the config JSON.  It should be run when using the respective args when executing the tool, e.g., `spotify_dl --config-file path/to/config.json --retry-failed-downloads 3`_
