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
