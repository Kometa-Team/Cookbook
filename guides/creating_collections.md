# WORK IN PROGRESS Creating a collection

This article will take you through various ways of creating a particular colelction, depending on your needs and requirements.

## Decide what you want

The first step in creating a collection is going to be deciding what you want the collection to contain.  In this case, I'll take a recent request from the Discord and look at different ways to accomplish the various thing sthat this description could mean.

"What are my options for a 'Top 50 Comedy' movies or something like it?"

That could mean many things:

 - The 50 highest rated comedy movies in my library.
 - The 50 highest rated comedy movies on Trakt, IMDB, etc
 - This one guy's top 50 comedy movies from soem website
 - The top 50 of this list of 1000 cpmedy movies on Trakt.

And so on.

## Structure of a collection

A Kometa collection has two required parts, a name and a "builder".

The name is just that, the name of the collection as displayed in Plex.

The "builder" is the bit that produces the list of movies that will go into the collection.  The Trakt list, or the search, or the list of TMDB IDs, etc.

Those are the two required parts:

 - I want the collection to be named this
 - I want the collection to contain these things

In the collection definition YAML, these things are expressed in this way:
```yaml
collections:
  Name of Collection:
    BUILDER GOES HERE
```

There are other things that a collection *can* contain like sort modes and artwork and so forth that wil will discuss later, but the two required elements are:

 - I want the collection to be named this
 - I want the collection to contain these things

### The 50 highest rated comedy movies in my library.

If you want to build a collection purely based on what you have in your library, you can use a `plex_search`, `smart_filter`, or regular `filter`.

`plex_search` - Kometa tells Plex to run a search, then uses the resulting list of movies to create the collection; Kometa will haev to run to update this collection.

`smart_filter` - Kometa creates a collection in Plex as a filter that Plex evaluates when the collection is viewed, so it updates automatically as new movies arrive in your library.

regular `filter` - Kometa applies a filter to all the elements returned byt a builder, which for now will be a special plex 'search' that returns everything in the library.  Kometa has to run to update this type of collection.

#### `plex_search`

<https://kometa.wiki/en/nightly/files/builders/plex/#plex-search>

There's an example of searching by genre on that page that we can use as a starting point; it's the very first example:
```yaml
collections:
  Documentaries:
    plex_search:
      all:
        genre: Documentary
```

We want comedies:
```
collections:
  Comedies:
    plex_search:
      all:
        genre: Comedy
```

We only want 50:
```
collections:
  Comedies:
    plex_search:
      all:
        genre: Comedy
      limit: 50
```

And we want them sorted by rating [we'll use the audience rating, but use whichever plex rating [critic/audience/user] you wish here:
```yaml
collections:
  Comedies:
    plex_search:
      all:
        genre: Comedy
      sort_by: audience_rating.desc
      limit: 50
```
That wil create a collection named "Comedies" that contains the 50 highest-rated comedy movies in your library at the time Kometa created it.

#### `smart_filter`

You can use a smart filter to let that update automatically as the movies in your library change.

<https://kometa.wiki/en/nightly/files/builders/smart/#smart-filter>

The smart filter is basically the same as the search we just created, just with a little different syntx:

Again, there's an example at the bottom of the page we can start with:
```yaml
collections:
  Documentaries:
    smart_filter:
      all:
        genre: Documentary
```
We want comedy, so:
```yaml
collections:
  Comedies:
    smart_filter:
      all:
        genre: Comedy
```
And only 50:
```yaml
collections:
  Comedies:
    smart_filter:
      all:
        genre: Comedy
      limit: 50
```
And sorted by rating [same note as above]:
```yaml
collections:
  Comedies:
    smart_filter:
      all:
        genre: Comedy
      limit: 50
      sort_by: audience_rating.desc
```
This collection will autoupdate in Plex without you having to run Kometa to update it.

You can see that's basically the same as the `plex_search` in this case.

#### regular `filter`


