Here's a walkthrough example of the entire process from start to finish, covering all the files and settings needed.

This is an example.  You do not need to use a collection file to use assets.  This example does so to have a clean starting point with no pre-defined poster.

I have a library that contains only the 6 "Original Crew" Star Trek movies:

<img width="702" height="861" alt="image" src="https://github.com/user-attachments/assets/5f2dd1f6-90a9-4162-8f02-84b4eab3b722" />

The filesystem behind that library:
```
/opt/plex-test-libraries/just_trek
├── Star Trek III The Search for Spock (1984) {imdb-tt0088170} {tmdb-157}
│   └── Star Trek III The Search for Spock (1984) {imdb-tt0088170} {tmdb-157} [Remux-1080p][H264][Vorbis 6.0]-BINGBANG.mkv
├── Star Trek II The Wrath of Khan (1982) {imdb-tt0084726} {tmdb-154}
│   └── Star Trek II The Wrath of Khan (1982) {imdb-tt0084726} {tmdb-154} [WEBRIP-2160p][H264][DTS 4.0]-BINGBANG.mkv
├── Star Trek IV The Voyage Home (1986) {imdb-tt0092007} {tmdb-168}
│   └── Star Trek IV The Voyage Home (1986) {imdb-tt0092007} {tmdb-168} [WEBDL-2160p][H264][AAC 12.0]-BINGBANG.mkv
├── Star Trek The Motion Picture (1979) {imdb-tt0079945} {tmdb-152}
│   └── Star Trek The Motion Picture (1979) {imdb-tt0079945} {tmdb-152} [Bluray-720p][H264][DTS HD MA 5 1]-BINGBANG.mkv
├── Star Trek VI The Undiscovered Country (1991) {imdb-tt0102975} {tmdb-174}
│   └── Star Trek VI The Undiscovered Country (1991) {imdb-tt0102975} {tmdb-174} [WEBRIP-2160p][H264][DTS 4.0]-BINGBANG.mkv
└── Star Trek V The Final Frontier (1989) {imdb-tt0098382} {tmdb-172}
    └── Star Trek V The Final Frontier (1989) {imdb-tt0098382} {tmdb-172} [WEBRIP-576p][H264][Opus 5.1]-BINGBANG.mkv
```

`config.yml`:
```yaml
libraries:
  Just Trek:
    collection_files:
      - file: config/example-collection.yml

settings:
  asset_directory:
    movies:
    - config/assets/example
  asset_folders: true
  create_asset_folders: true
```

`config/example-collection.yml`:
```yaml
collections:
  Star Trek Stuff:
    tmdb_collection: 
      - 151
```
There is nothing special about this; it's just a simple collection with no predefined poster.  You can use assets to override posters in collections you do not control, such as the Kometa defaults, but that's out of scope here.

I am using `asset_folders` here because it simplifies the process by adding automation oppportunities.
`create_asset_folders` is one of those automation opportunities.  Kometa is going to automatically create the asset folders it needs.

At the moment, `config/assets/example` exists but is empty.
<img width="513" height="110" alt="image" src="https://github.com/user-attachments/assets/b18afd44-a13c-4872-848e-4e9e00a3f4b4" />

I run Kometa and the collection is created:

```
|=============================== Adding to Star Trek Stuff Collection
|
| 1/6 | Star Trek Stuff Collection | + | Star Trek: The Motion Picture (1979)
| 2/6 | Star Trek Stuff Collection | + | Star Trek II: The Wrath of Khan (1982)
| 3/6 | Star Trek Stuff Collection | + | Star Trek III: The Search for Spock (1984)
| 4/6 | Star Trek Stuff Collection | + | Star Trek IV: The Voyage Home (1986)
| 5/6 | Star Trek Stuff Collection | + | Star Trek V: The Final Frontier (1989)
| 6/6 | Star Trek Stuff Collection | + | Star Trek VI: The Undiscovered Country (1991)
|
| 6 Movies Processed 6 Movies Added
|
|========================= Updating Metadata of Star Trek Stuff Collection
|
| Collection Metadata Edits
| Label | +Kometa
| Metadata: Update Completed
|
| Asset Warning: Asset Directory Not Found and Created: config/assets/example/Star Trek Stuff
```

<img width="279" height="427" alt="image" src="https://github.com/user-attachments/assets/ba17aada-b398-4a13-99ec-e50a6bf7f4f3" />

We can see there that Kometa automatically created an asset folder:
```
| Asset Warning: Asset Directory Not Found and Created: config/assets/example/Star Trek Stuff
```

<img width="493" height="88" alt="image" src="https://github.com/user-attachments/assets/2fa64f6f-e2b0-47c7-8f44-d485e505937c" />

Now let's use an asset to apply a poster to the collection.  I am going to add a setting to make this simpler; another of the "automation opportunities":
```yaml
libraries:
  Movies:
    collection_files:
      - file: config/example-collection.yml

settings:
  asset_directory:
    movies:
    - config/assets/example
  asset_folders: true
  create_asset_folders: true
  dimensional_asset_rename: true
```

`dimensional_asset_rename` will automatically rename the first image Kometa finds in that folder that matches the correct aspect ratio (1:1.5) to `poster.ext`
This ONLY works with `asset_folders: true` since that's the only way for Kometa to know what movie `some-random-poster-qpb123.webbp` belongs to.

I'll drag that poster in:

<img width="555" height="111" alt="image" src="https://github.com/user-attachments/assets/eda0b8fb-c129-464b-88f7-de2a2ad12ecc" />

And then run Kometa again:
```
|========================= Updating Metadata of Star Trek Stuff Collection
|
| Collection Metadata Edits
| Metadata: asset_directory updated Star Trek Stuff's poster to [File] /home/chaz/dev/Kometa-Team/Kometa/config/assets/example/Star Trek Stuff/poster.png |
```

Note that the file is now named `poster.png` thanks to `dimensional_asset_rename`:

<img width="548" height="114" alt="image" src="https://github.com/user-attachments/assets/d3ad146e-2b87-48a2-b5e4-19670cdb6a80" />

That set the art on the collection only:

<img width="1377" height="786" alt="image" src="https://github.com/user-attachments/assets/516d4a99-7d66-41ac-926f-f5355ee9f4a2" />

Now I want to apply assets to the movies.

The `assets_for_all` operation applies assets to individual items (movies/shows/seasons/episodes)
```yaml
libraries:
  Movies:
    collection_files:
      - file: config/chaz-TechD87.yml
    operations:
      assets_for_all: true

settings:
  asset_directory:
    movies:
    - config/assets/TechD87
  asset_folders: true
  create_asset_folders: true
```

And run Kometa:
```
| (1/6) Star Trek: The Motion Picture
| Asset Warning: Asset Directory Not Found and Created: config/assets/example/Star Trek The Motion Picture (1979) {imdb-tt0079945} {tmdb-152} |
| No Item Edits
|
| (2/6) Star Trek II: The Wrath of Khan
| Asset Warning: Asset Directory Not Found and Created: config/assets/example/Star Trek II The Wrath of Khan (1982) {imdb-tt0084726} {tmdb-154} |
| No Item Edits
|
| (3/6) Star Trek III: The Search for Spock
| Asset Warning: Asset Directory Not Found and Created: config/assets/example/Star Trek III The Search for Spock (1984) {imdb-tt0088170} {tmdb-157} |
| No Item Edits
|
| (4/6) Star Trek IV: The Voyage Home
| Asset Warning: Asset Directory Not Found and Created: config/assets/example/Star Trek IV The Voyage Home (1986) {imdb-tt0092007} {tmdb-168} |
| No Item Edits
|
| (5/6) Star Trek V: The Final Frontier
| Asset Warning: Asset Directory Not Found and Created: config/assets/example/Star Trek V The Final Frontier (1989) {imdb-tt0098382} {tmdb-172} |
| No Item Edits
|
| (6/6) Star Trek VI: The Undiscovered Country
| Asset Warning: Asset Directory Not Found and Created: config/assets/example/Star Trek VI The Undiscovered Country (1991) {imdb-tt0102975} {tmdb-174}
| No Item Edits
```
Since `assets_for_all` is enabled, Kometa tried to find assets for the individual movies, and since the folders didn't exist, `create_asset_folders` created them.

<img width="548" height="241" alt="image" src="https://github.com/user-attachments/assets/4e389a3d-34ee-40ba-8676-8068a9412d5c" />

I put posters in those folders, again leveraging `dimensional_asset_rename`:

<img width="547" height="355" alt="image" src="https://github.com/user-attachments/assets/12eaad83-c5ba-4c3b-8d97-e3b778259fab" />

Then run Kometa again:
```
| (1/6) Star Trek: The Motion Picture
| Metadata: asset_directory updated Star Trek: The Motion Picture's poster to [File] /home/chaz/dev/Kometa-Team/Kometa/config/assets/example/Star Trek The Motion Picture (1979){imdb-tt0079945} {tmdb-152}/poster.png
| No Item Edits
|
| (2/6) Star Trek II: The Wrath of Khan
| Metadata: asset_directory updated Star Trek II: The Wrath of Khan's poster to [File] /home/chaz/dev/Kometa-Team/Kometa/config/assets/example/Star Trek II The Wrath of Khan (1982) {imdb-tt0084726} {tmdb-154}/poster.png
| No Item Edits
|
| (3/6) Star Trek III: The Search for Spock
| Metadata: asset_directory updated Star Trek III: The Search for Spock's poster to [File] /home/chaz/dev/Kometa-Team/Kometa/config/assets/example/Star Trek III The Search for Spock (1984) {imdb-tt0088170} {tmdb-157}/poster.png
| No Item Edits
|
| (4/6) Star Trek IV: The Voyage Home
| Metadata: asset_directory updated Star Trek IV: The Voyage Home's poster to [File] /home/chaz/dev/Kometa-Team/Kometa/config/assets/example/Star Trek IV The Voyage Home (1986){imdb-tt0092007} {tmdb-168}/poster.png
| No Item Edits
|
| (5/6) Star Trek V: The Final Frontier
| Metadata: asset_directory updated Star Trek V: The Final Frontier's poster to [File] /home/chaz/dev/Kometa-Team/Kometa/config/assets/example/Star Trek V The Final Frontier (1989) {imdb-tt0098382} {tmdb-172}/poster.png
| No Item Edits
|
| (6/6) Star Trek VI: The Undiscovered Country
| Metadata: asset_directory updated Star Trek VI: The Undiscovered Country's poster to [File] /home/chaz/dev/Kometa-Team/Kometa/config/assets/example/Star Trek VI The Undiscovered Country (1991) {imdb-tt0102975} {tmdb-174}/poster.png
| No Item Edits
|
``` 

<img width="1374" height="791" alt="image" src="https://github.com/user-attachments/assets/d0a974b9-0575-4de6-995d-570488f96923" />

And Kometa renamed the posters:

<img width="548" height="352" alt="image" src="https://github.com/user-attachments/assets/0e54a5a4-d8c1-4394-97c2-aefb63720013" />

Note that the assets apply to the movies in the library as well:

<img width="1403" height="491" alt="image" src="https://github.com/user-attachments/assets/be679f0b-2a22-4be2-822e-5f80aa28874e" />

The same thing with `asset_folders: false` would end up like this:

```yaml
libraries:
  Movies:
    collection_files:
      - file: config/example-collection.yml

settings:
  asset_directory:
    movies:
    - config/assets/example-no-folders
  asset_folders: false
```
Neither `create_asset_folders` nor `dimensional_asset_rename` are available in this case, so the user will have to create and name these images manually.

<img width="567" height="216" alt="image" src="https://github.com/user-attachments/assets/10cca418-2b91-4e7f-8d66-565403c35196" />


