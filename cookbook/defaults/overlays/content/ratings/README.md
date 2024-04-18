## Ratings

### I want two ratings in the bottom corners of the poster

![image](https://github.com/chazlarson/Plex-Meta-Manager-Cookbook/assets/3865541/a341736e-de7e-45c5-b4fa-791ae922885e)

```yaml
libraries:
  Some-Shows:
    overlay_files:
    - pmm: ratings
      template_variables:
         rating1: critic
         rating1_image: imdb

         rating_alignment: horizontal
         vertical_position: bottom
    - pmm: ratings
      template_variables:
         rating2: user
         rating2_image: tmdb

         horizontal_position: right
         rating_alignment: horizontal
         vertical_position: bottom
    operations:
      mass_critic_rating_update: imdb
      mass_user_rating_update: tmdb
```

