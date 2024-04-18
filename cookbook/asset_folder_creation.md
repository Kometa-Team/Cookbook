# 12 Runs exercising assets

Relevant settings to start:

```
libraries:
  TV Shows:
    collection_files:
        - file: config/testing.yml
    settings:
        asset_directory: config/assets/TV

    operations:
        assets_for_all: false
#         mass_poster_update:
#             source: lock
#             seasons: false
#             episodes: false

settings:
  asset_directory: config/hampus-assets
  asset_folders: false
  create_asset_folders: false
  download_url_assets: false
```

`testing.yml`:
```
templates:
  some_template:
    tvdb_show: <<show>>
    url_poster: <<poster>>

collections:
  poster YES:
    template: {name: some_template, show: "337907, 364080", poster: https://theposterdb.com/api/assets/203792}
    
  poster NO:
    template: {name: some_template, show: "277092, 332747"}
```

Library has no collections.

I've created `config/assets`

Every run is this command:
```
python kometa.py --run --config config/test-config.yml -dc
```

## RUN 1
---
### Run 01 changes:

None

### Run 01 result:

nothing downloaded, no folders created

## RUN 2
---
### Run 02 changes:

changed:
```
  download_url_assets: true
```
### Run 02 result:

nothing downloaded

## RUN 3
---
### Run 03 changes:

changed:
```
  asset_folders: true
```
### Run 03 result:

nothing downloaded

## RUN 4
---
### Run 04 changes:

changed:
```
  create_asset_folders: true
```
### Run 04 result:

1. created `config/hampus-assets/poster NO`
1. created `config/hampus-assets/poster YES`
1. downloaded `https://theposterdb.com/api/assets/203792` to `config/hampus-assets/poster YES/poster.jpg`

## RUN 5
---
### Run 05 changes:

1. delete `poster.jpg`
1. change:
    ```
      create_asset_folders: false
    ```

### Run 05 result:

downloaded `https://theposterdb.com/api/assets/203792` to `config/hampus-assets/poster YES/poster.jpg`

## RUN 6
---
### Run 06 changes:

1. delete `poster.jpg`
1. change:
    ```
      asset_folders: false
    ```
### Run 06 result:

nothing downloaded

So there's something we've learned:
```
 download_url_assets: true
```
REQUIRES `asset_folders: true`

## RUN 7
---
### Run 07 changes:

change:
```
        assets_for_all: true
```
### Run 07 result:

1. nothing downloaded
1. no folders created

## RUN 8
---
### Run 08 changes:

change:
```
  asset_folders: true
```
### Run 08 result:

1. nothing downloaded
1. no folders created

## RUN 9
---
### Run 09 changes:

changed:
```
  asset_directory:
```
### Run 09 result:

nothing created or downloaded

## RUN 10
---
### Run 10 changes:

manually created: `config/hampus-assets/TV`

### Run 10 result:

1. created `config/hampus-assets/TV/poster NO`
1. created `config/hampus-assets/TV/poster YES`
1. downloaded `https://theposterdb.com/api/assets/203792` to `config/hampus-assets/TV/poster YES/poster.jpg`

So there's something we've learned:

Kometa will not create asset directories, only directories WITHIN asset directories 

## RUN 11
---
### Run 11 changes:

uncomment:
```
        mass_poster_update:
            source: lock
            seasons: false
            episodes: false
```
### Run 11 result:

3500 folders created [one for every show in the library]

So there's something we've learned:

running a mass poster update, even if it doesn't set the art, triggers the "look for an asset and create the folder if needed and enabled" logic

## RUN 12
---
### Run 12 changes:

1. delete all 3500 movie asset folders
1. change:
    ```
        assets_for_all: true
    ```
### Run 12 result:

1. 3500 folders created [one for every show in the library]

So there's something we've learned:

Mass poster reset creating asset folders does not depend on `assets_for_all`

Summary:

1. Kometa will only download `url_poster` if `asset_folders` is enabled
1. Kometa will create all missing asset folders if `create_asset_folders` is enabled, not just those that it is downloading
1. If `create_asset_folders` is disabled and the asset folder does not exist, Kometa will not download a `url_poster`
1. Performing a mass poster update will create asset folders for everything in the library if `create_asset_folders` is enabled, regardless of `assets_for_all` setting.
