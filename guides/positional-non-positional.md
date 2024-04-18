## Positional and Non-positional overlay use in Plex Meta Manager

Starting with two overlay images:

non-pos.png is 1000x1500, transparent except for the word “overlay”.

![Non-positional source](images/pos-nonpos/non-pos.png?raw=true "Non-positional source")

pos.png is 336 x 83 and again transparent except for the text:

![Positional source](images/pos-nonpos/pos.png?raw=true "positional source")

These are stored at:

```
/opt/kometa/config/overlays/non-pos.png
/opt/kometa/config/overlays/pos.png
```

`/opt/kometa/config` is the configured kometa config directory.

I'm going to create a `test-overlays.yml` in the config directory and link to it in my config:

```yaml
library:
  One-Movie:
    metadata_path:
      - file: config/test-overlays.yml
```

Non-positional overlay:

[this is the content of `test-overlays.yml`]

```yaml
overlays:
  non-pos:
    plex_search:
      all:
        resolution: 4K
```

This is going to find the one 4K movie in this library, and lay the 1000x1500 non-pos.png on top of it.  Ends up like this:

What does what in that YAML?

```yaml
overlays:               # tells kometa to expect overlay definitions
  non-pos:              # overlay "name"; this is the name of the image kometa will look for
                        # if not otherwise specified
    plex_search:        # from here down is the builder that tells kometa what to apply
      all:              # the overlay _to_.  Any builders can be used here, this is
        resolution: 4K  # a plain plex search
```

![result 01](images/pos-nonpos/result-01.png?raw=true "result 01")

Now let’s swap that for the smaller image but change nothing else:

[this is the content of `test-overlays.yml`]

```yaml
overlays:
  pos:                  # now kometa is looking for config/overlays/pos.png
    plex_search:
      all:
        resolution: 4K
```

That gives me:

![result 02](images/pos-nonpos/result-02.png?raw=true "result 02")

Since there’s no positioning info, kometa just scales the small image to fill the poster as if it were a non-positional overlay, just like it did the other image but since this image isn’t 1000x1500 it gets spread all over the poster.

Let’s add some positioning info to prevent *that*, which requires a couple other lines:

[this is the content of `test-overlays.yml`]

```yaml
overlays:
  pos:
    plex_search:
      all:
        resolution: 4K
    overlay:                   # we need this "overlay" atribute to specify positioning
      name: pos                # since we have an "overlay" attribute, we need to specify the file name here
                               # The "pos" up above is now ununsed and completely arbitrary
      horizontal_align: right  # where to align the overlay horizontally on the poster
      horizontal_offset: 0     # offset this many pixels _in_ from the alignment edge
                               # if alignment is "left" or "center", this is moving the image to the right
                               # and the opposite for "right"
      vertical_align: bottom   # where to align the overlay vertically on the poster
      vertical_offset: 0       # offset this many pixels _in_ from the alignment edge
                               # if alignment is "top" or "center", this is moving the image down
                               # and the opposite for "bottom"
```

![result 03](images/pos-nonpos/result-03.png?raw=true "result 03")

Let’s move it around a little bit:

[this is the content of `test-overlays.yml`]

```yaml
overlays:
  pos:
    plex_search:
      all:
        resolution: 4K
    overlay:
      name: pos
      horizontal_align: right
      horizontal_offset: 100  # 100 pixels in from the right
      vertical_align: bottom
      vertical_offset: 300    # 300 pixels up from the bottom
```

![result 04](images/pos-nonpos/result-04.png?raw=true "result 04")

It’s moved 100 pixels relative to the horizontal alignment [so to the left] and 300 vertical.

If we change the alignment:

```yaml
overlays:
  pos:
    plex_search:
      all:
        resolution: 4K
    overlay:
      name: pos
      horizontal_align: left
      horizontal_offset: 100  # now 100 pixels in from the _left_
      vertical_align: bottom
      vertical_offset: 300
```

We can see that it’s 100 pixels in from the left:

![result 05](images/pos-nonpos/result-05.png?raw=true "result 05")

Hopefully this gives you a start on working with creating your own overlays.
