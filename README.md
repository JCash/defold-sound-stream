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


