General stuff that maybe will be moved to a better home.

### I want to take action on some things without building a collection

For example, you want to add a genre to things but don't want the collection hanging around.

```yaml
collections:
  Add-live-action-genre:
    plex_search:
      all:
        genre.not: Animation  # find everything in the library that doesn't have Animation as a genre
    item_genre: Live Action   # add the live action genre to all those things
    build_collection: false   # and don't create a collection
```

