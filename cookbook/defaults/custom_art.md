# Using Custom Art with Universe defaults

NOTE: This article is using the universe defaults, but most all default collections can be customized in similar ways.

Starting with basic config:

```yaml
libraries:
  Kometa-Demo-Movies:
    collection_files:
      - default: universe
```

Produces:
![image](https://github.com/user-attachments/assets/c838fc3e-c45a-4b74-a3a4-8e28f4087540)

This article will demonstrate various ways to replace the images on these collections with your own images.

Those default images are all coming from this github repo:
<https://github.com/Kometa-Team/Default-Images/tree/master/universe>

I've created a new set of custom images located at:
```
config/images/custom/universe/
```

All named the same as the repo above. except that they are saved as PNG, for example:
```
config/images/custom/universe/avp.png
config/images/custom/universe/dca.png
config/images/custom/universe/dcu.png
...
```
These images all have text added reading "CUSTOM IMAGE", for example:

![image](https://github.com/user-attachments/assets/b781d9c7-8c5c-4642-a485-d31f77e3e1f3)

## Replacing images via template variables
 
### Individually

First, we will replace a couple individual posters using template variables:

```yaml
libraries:
  Kometa-Demo-Movies:
    collection_files:
      - default: universe
        template_variables:
          file_poster_avp: config/images/custom/universe/avp.png
          file_poster_dca: config/images/custom/universe/dca.png
          file_poster_dcu: config/images/custom/universe/dcu.png
```

Then running Kometa produces:

![image](https://github.com/user-attachments/assets/f72563ef-7647-440b-8745-b7f4388c4a00)

Note that the three collections that are specified in the template variables have been changed.

### All at once

Say you want to replace them all while adding the minimum information in the config.

Use a single template variable with a placeholder for Kometa to replace:

```yaml
libraries:
  Kometa-Demo-Movies:
    collection_files:
      - default: universe
        template_variables:
          file_poster: config/images/custom/universe/<<key>>.png
```

In this case, Kometa will fill in that path with the key for each universe in turn.

The "key" can be found in the table at the top of each default's wiki page.  In this case, the keys are `avp`, `dca`, `dcu`, etc.

Now running Kometa produces:

![image](https://github.com/user-attachments/assets/2c41a831-a15e-4958-b9b1-332161e9ce8d)

Note that all posters are changed to the custom images.

Kometa took this:
```
config/images/custom/universe/<<key>>.png
```
And replaced `<<key>>` in turn with each key, resulting in:
```
config/images/custom/universe/avp.png
config/images/custom/universe/dca.png
config/images/custom/universe/dcu.png
...
```
Which you will note are the same paths as we used just above when specifying these individually

Those examples both use file paths, but you can do the same thing with URLs, though with URLs you may need to specify them individually unless you can control the URLs such that they can be made all the same except for the key.

For this test I've created versions of these images with the text "URL IMAGE" on them, and uploaded them to [ImgBB](https://imgbb.com/).

I add all the individual URLs to the config [note that because of the unique part of each URL, these cannot be consolidated into a single template variable as above]:

```yaml
libraries:
  Kometa-Demo-Movies:
    collection_files:
      - default: universe
        template_variables:
          url_poster_askew: https://i.ibb.co/JRdcfHp/askew.png
          url_poster_avp: https://i.ibb.co/fnz2Z2Y/avp.png
          url_poster_dca: https://i.ibb.co/StrtT45/dca.png
          url_poster_dcu: https://i.ibb.co/6ryLN6h/dcu.png
          url_poster_fast: https://i.ibb.co/8xTmnMv/fast.png
          url_poster_marvel: https://i.ibb.co/2kFPTNB/marvel.png
          url_poster_mcu: https://i.ibb.co/KNW1KL2/mcu.png
          url_poster_middle: https://i.ibb.co/9VZ4XYz/middle.png
          url_poster_mummy: https://i.ibb.co/9GVNRMV/mummy.png
          url_poster_rocky: https://i.ibb.co/RSnzvfr/rocky.png
          url_poster_star: https://i.ibb.co/4TFMnDK/star.png
          url_poster_trek: https://i.ibb.co/sbHBQhg/trek.png
          url_poster_wizard: https://i.ibb.co/FzjrfYD/wizard.png
          url_poster_xmen: https://i.ibb.co/HVv7prv/xmen.png
```

And then run Kometa, which produces:

![image](https://github.com/user-attachments/assets/3b00ecec-8046-4e6b-9d30-c271280a6c7c)

Note that all the posters reflect the URL-sourced images.

You can also combine the two:

```yaml
libraries:
  Kometa-Demo-Movies:
    collection_files:
      - default: universe
        template_variables:
          file_poster: config/images/custom/universe/<<key>>.png
          url_poster_star: https://i.ibb.co/4TFMnDK/star.png
          url_poster_trek: https://i.ibb.co/sbHBQhg/trek.png
          url_poster_wizard: https://i.ibb.co/FzjrfYD/wizard.png
          url_poster_xmen: https://i.ibb.co/HVv7prv/xmen.png
```

Which produces:

![image](https://github.com/user-attachments/assets/1442d04f-c0b2-4205-919d-3c83cd3491c7)

Note that all the posters reflect the file path images, except for the four that use the URL-sourced posters.

## Replacing images via asset directory
 
I'm going to let Kometa create the required asset directories:

```yaml
settings:
  asset_directory: config/universe-assets/
  asset_folders: true
  create_asset_folders: true
```
```
|     1 | Asset Warning: Asset Directory Not Found and Created: config/universe-assets/Alien  Predator |
|     1 | Asset Warning: Asset Directory Not Found and Created: config/universe-assets/View Askewniverse |
|     1 | Asset Warning: Asset Directory Not Found and Created: config/universe-assets/DC Animated Universe |
|     1 | Asset Warning: Asset Directory Not Found and Created: config/universe-assets/DC Extended Universe |
|     1 | Asset Warning: Asset Directory Not Found and Created: config/universe-assets/Fast & Furious |
|     1 | Asset Warning: Asset Directory Not Found and Created: config/universe-assets/In Association With Marvel |
|     1 | Trakt Error: No TVDb ID found for Marvel's Agents of S.H.I.E.L.D.: Slingshot (2016)        |
|     1 | Asset Warning: Asset Directory Not Found and Created: config/universe-assets/Marvel Cinematic Universe |
|     1 | Asset Warning: Asset Directory Not Found and Created: config/universe-assets/Middle Earth  |
|     1 | Asset Warning: Asset Directory Not Found and Created: config/universe-assets/Rocky  Creed  |
|     1 | Asset Warning: Asset Directory Not Found and Created: config/universe-assets/Star Trek     |
|     1 | Asset Warning: Asset Directory Not Found and Created: config/universe-assets/Star Wars Universe |
|     1 | Asset Warning: Asset Directory Not Found and Created: config/universe-assets/The Mummy Universe |
|     1 | Asset Warning: Asset Directory Not Found and Created: config/universe-assets/Wizarding World |
|     1 | Asset Warning: Asset Directory Not Found and Created: config/universe-assets/X-Men Universe |
```

Now we create asset images and put them in those folders.

![image](https://github.com/user-attachments/assets/333f1358-5224-4d5e-89bb-c6dbb7ab11e9)

![image](https://github.com/user-attachments/assets/0aea2b99-ea33-4a0d-aa2a-e3fe3b79dd08)

I'm also going to leverage Kometa's ability to rename those asset images as required.

```yaml
settings:
  dimensional_asset_rename: true
```

I've removed all template variables:
```yaml
libraries:
  Kometa-Demo-Movies:
    collection_files:
      - default: universe
```

And since We want Kometa to use those assets we need to tell it to prioritize them:
```yaml
settings:
  prioritize_assets: true
```

Now run Kometa, which produces:

![image](https://github.com/user-attachments/assets/98d81562-b4ed-4479-a34c-86fe67496c16)

Note that all these collections are now using the asset-sourced posters.

Also, Kometa has renamed the asset images:

![image](https://github.com/user-attachments/assets/720c592b-b96e-4de5-bb41-eb9c373b60fb)
