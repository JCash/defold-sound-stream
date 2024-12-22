# Sound streaming

Sound streaming is useful to keep memory usage down to a minimum.
It is also convenient for streaming audio over the http protocol.


## Test project

The project tests streaming sounds by playing a looping sound.

* The current sound formats are .ogg, or .wav.

* The streaming source may come from:
    * The game files (Using the Defold archive or file provider)
    * Web url (Using the Defold http provider)


## Configuration

There are two ways to configure sound streaming.

### Enabling the streaming

Currently, while it's an experimental feature, you need to enable it manually by editing `game.project`:

```ini
[sound]
stream_enabled = 1
```

### Setting the preload size

The preload size is the number of bytes loaded as the first, partial chunk of a sound resource:

```ini
[sound]
stream_preload_size = 16384
```

### Setting the sound chunk size

The streaming sounds are loaded in chunks (default 16k).
This can be configured by the setting `sound.stream_chunk_size`

```ini
[sound]
stream_chunk_size = 16384
```

### Setting the sound cache size

The sound chunks are stored in a cache.
When the cache is full, old chunks are evicted based on a LRU scheme (least recently used).
The size of the cache can be configured by the setting `sound.stream_cache_size`. Default is 2MB.

```ini
[sound]
stream_cache_size = 2097152
```


## Creating sound data at runtime

The example also shows how to create a new resource from scratch, by downloading initial bytes over http.
Using this initial chunk of data, it's possible to register a new resource as a streaming resource.

By mounting a `http` file provider to the `liveupdate` system, it is possible to continue streaming from the specified URL.
Note that the server as the URL has to support `ranged requests`


# Testing

You can build & run the project from the Defold editor, but currently, there is no gui setup.
So we recommend using the command line to test the various features.

## Build

No special commands are needed:

```bash
java -jar ~/work/defold/tmp/dynamo_home/share/java/bob.jar clean build
```

## Run

* .ogg
* Loading from the file system

```bash
DM_QUIT_ON_ESC=1 ./path/to/dmengine --config=test.sound_type=Ogg
```

* .wav
* Loading from the file system

```bash
DM_QUIT_ON_ESC=1 ./path/to/dmengine --config=test.sound_type=Wav
```


* Specify url to file server
* Specify relative path to files on file server

Start a server at `http://localhost:8000`
```bash
pip3 install RangeHTTPServer
(cd path/to/project && python -m RangeHTTPServer)
```

Run the example, using the `--config` options:
```bash
DM_QUIT_ON_ESC=1 ./path/to/dmengine --config=test.sound_type=Web --config=web.url=http://localhost:8000 --config=web.filename=web/mary-had-a-little-lamb-loop.wav
```
