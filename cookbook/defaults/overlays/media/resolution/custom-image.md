# BASICS

1. create your new images
   
   For this test, I just copied the stock images to `config/overlays/replacement-images/resolution` and then ran an imagemagick command to flip them all horizontally.

   ```
   config/overlays/replacement-images/
   └── resolution
       ├── 1080pdvhdrplus.png
       ├── 1080pdvhdr.png
       ├── 1080pdv.png
       ├── 1080phdr.png
       ...
   ```
   for example:
   
   ![1080p](https://github.com/user-attachments/assets/ec1be556-d572-4313-a32c-c0391a619b67)

2. Set template variable to point at new images:

   ```
   libraries:
     Kometa-Demo-Movies:
       overlay_files:
         - default: resolution
           template_variables:
             file: config/overlays/replacement-images/resolution/<<key>>.png
   ```

3. Run Kometa, which produces:

   ![image](https://github.com/user-attachments/assets/adedd696-acba-4fc9-bb54-fa732d5f7587)


## OTHER CONSIDERATIONS

Doing it this way means you have to copy *all* the stock images, including the editions if you use those, even if you aren't changing them, since you are changing the path for *all* images.

If you want to change only one or two, you can specify paths for individual overlays:

```yaml
libraries:
  Kometa-Demo-Movies:
    overlay_files:
      - default: resolution
        template_variables:
          file_480p: config/overlays/replacement-images/resolution/480p.png
          file_1080p: config/overlays/replacement-images/resolution/1080p.png
```

In this case I didn't have to adjust the backdrop, since I didn't change the size of the image.

If I change the size of the image, for example:

![1080p](https://github.com/user-attachments/assets/afaefac8-bd50-4030-aed0-4fd9b5389767)

Then the example above produces this:

![image](https://github.com/user-attachments/assets/3761e843-638a-4e07-8aa2-9cac1f721f29)

Unless we shrink the backdrop:

```yaml
libraries:
  Kometa-Demo-Movies:
    overlay_files:
      - default: resolution
        template_variables:
          file: config/overlays/replacement-images/resolution/<<key>>.png
          back_width: 153
```

Which produces:

![image](https://github.com/user-attachments/assets/1fd8f40e-12c3-4ed0-abb8-14c5abd0eaf8)



