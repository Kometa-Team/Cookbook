I am starting with a demo library and nothing in Radarr.

First step will be to put all these things into Radarr with a base tag:

Using this config:
```
libraries:
  Kometa-Demo-Movies:
    operations:
      radarr_add_all: true
...
radarr:                              # Can be individually specified per library as well
  url: http://192.168.1.11:7878
...
  tag: radarr_global
...
```

Kometa added 197 movies and applied that tag:

![image](https://github.com/Kometa-Team/Cookbook/assets/3865541/22135e21-df46-46da-8b41-33579e27c0c7)

## Use with custom collections:

Now let's play with adding and syncing tags in Radarr.

I have a test.yml containing:
```yaml
collections:

  720p:
    plex_search:
      all:
        resolution: 720p
    item_radarr_tag: item_radarr_tag_720p

  1080p:
    plex_search:
      all:
        resolution: 1080p
    item_radarr_tag.sync: item_radarr_tag_sync_1080p

  4K:
    plex_search:
      all:
        resolution: 4K
    item_radarr_tag.remove: radarr_global

  80s:
    plex_search:
      all:
        release.after: 01/01/1980
        release.before: 01/01/1990
    item_radarr_tag: item_radarr_tag_1980s

  90s:
    plex_search:
      all:
        release.after: 01/01/1990
        release.before: 01/01/2000
    item_radarr_tag.sync: item_radarr_tag_1990s
```

Which I add to the config and run against the demo library.

A movie which is 720p in Plex gets a tag **added** by `item_radarr_tag`:

![image](https://github.com/Kometa-Team/Cookbook/assets/3865541/d8661353-a6f4-434c-b66c-57d755fa39e2)

A movie which is 1080p in Plex gets the existing label **replaced** by `item_radarr_tag.sync`:

![image](https://github.com/Kometa-Team/Cookbook/assets/3865541/a5b6b067-5a1e-40b0-994d-0eb7768b8697)

A movie which is 4K in Plex gets the existing label **removed** by `item_radarr_tag.remove`: 

![image](https://github.com/Kometa-Team/Cookbook/assets/3865541/bf65a5f7-0bc9-4890-b5bb-e3f9b7e1a07e)

A movie which is 1080p in Plex and is from the 80s gets a tag added by `item_radarr_tag` [if those two collections were in the collection file in the other order this movie would have **only** the `item_radarr_tag_sync_1080p` tag]:

![image](https://github.com/Kometa-Team/Cookbook/assets/3865541/1f839518-c1f3-42c3-b84f-315e564ea495)

A movie which is 720p in Plex and is from the 80s gets a tag added by `item_radarr_tag`:

![image](https://github.com/Kometa-Team/Cookbook/assets/3865541/7dabc828-382f-4fc5-9fd1-c6f8eef08065)


A movie which is from the 90s gets **only** `item_radarr_tag_1990s` due to `item_radarr_tag.sync`:

![image](https://github.com/Kometa-Team/Cookbook/assets/3865541/26b07633-018f-4050-9fce-4ad7d3130f6f)


## Tagging with the defaults:

`radarr_tag` is only useful with the defaults, so let's add this to the config:

`radarr_tag` and variants will only apply to items which are newly added to Radarr, they will not affect things which are already in Radarr.

`item_radarr_tag` and variants will tag items which are already in Radarr.

So we will add this to the config and run it:

```yaml
libraries:
  Kometa-Demo-Movies:
    collection_files:
      - file: config/tag-test.yml
      - default: imdb
        template_variables:
          radarr_add_missing_lowest: true
          radarr_tag: radarr_tag_imdb_default
          radarr_tag_lowest: radarr_tag_imdb_lowest
          item_radarr_tag: item_radarr_tag_imdb_default
          item_radarr_tag_lowest: item_radarr_tag_imdb_lowest
```

This movie was already in Radarr and does not appear on the "Lowest Rated" list, so it got `item_radarr_tag_default` applied to it:

![image](https://github.com/Kometa-Team/Cookbook/assets/3865541/7bd2eeb5-b5ee-4adf-b36b-0a0f71cc8924)

This movie was already in Radarr and **does appear** on the "Lowest Rated" list, so it got `item_radarr_tag_imdb_lowest` and `item_radarr_tag_default` applied to it. 

![image](https://github.com/Kometa-Team/Cookbook/assets/3865541/a8dfe4d1-5094-486f-abc8-e14176d002e8)

This movie was added to Radarr on this run, and came from the "Lowest Rated" list:

![image](https://github.com/Kometa-Team/Cookbook/assets/3865541/f319cc79-494d-49f9-bf32-4248902dde7a)
