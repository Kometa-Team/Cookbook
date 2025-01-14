# How long do episode overlays take?

Here are a set of timings to use as a *relative* guide when deciding whether you want to enable overlays at a season or episode level.

These experiments were performed on a library containing two shows, one with 27 seasons and 562 total episodes, the other with 6 seasons and 69 total episodes.

Of course this is a tiny example, but this is only meant as an example to show relative times.  There are only two shows so that these experiments run in a reasonable time.

Don't pay any attention to the specific times here.  Pay attention to the relative times.

## Setup

The config in use:

```yaml
libraries:
  Kometa-One-Show:
    overlay_files:
      - default: resolution
        template_variables:
          use_dvhdrplus: false
          use_dvhdr: false
          use_plus: false
          use_dv: false
          use_hdr: false
      - default: resolution
        template_variables:
          builder_level: season
          use_dvhdrplus: false
          use_dvhdr: false
          use_plus: false
          use_dv: false
          use_hdr: false
      - default: resolution
        template_variables:
          builder_level: episode
          use_dvhdrplus: false
          use_dvhdr: false
          use_plus: false
          use_dv: false
          use_hdr: false
```

Show, Season, Episodes; none of the different video types.

I have three copies; one with all three levels, one with Show and Season only, one with Show only.  I also have one with `remove_overlays: true` to get back to a known starting point.

The script which runs the test:
```bash
#!/bin/bash

python kometa.py --run --config config/levels-3.yml && cp config/logs/meta.log level-3-1-meta.log
python kometa.py --run --config config/levels-3.yml && cp config/logs/meta.log level-3-2-meta.log
python kometa.py --run --config config/levels-remove.yml && cp config/logs/meta.log level-3-remove-meta.log
python kometa.py --run --config config/levels-2.yml && cp config/logs/meta.log level-2-1-meta.log
python kometa.py --run --config config/levels-2.yml && cp config/logs/meta.log level-2-2-meta.log
python kometa.py --run --config config/levels-remove.yml && cp config/logs/meta.log level-2-remove-meta.log
python kometa.py --run --config config/levels-1.yml && cp config/logs/meta.log level-1-1-meta.log
python kometa.py --run --config config/levels-1.yml && cp config/logs/meta.log level-1-2-meta.log
python kometa.py --run --config config/levels-remove.yml && cp config/logs/meta.log level-1-remove-meta.log

```
For each level:
1. run all the overlays for the first time, which will filter and then apply all overlays on everything, which will show the maximum time this process will take.
2. run the same config again, which will filter and then leave all the overlays alone, showing the time involved in just the filtering.
3. remove all overlays so the next level is from the same standing start.

## Timings

### Shows

Show only, applying the two overlays for the first time:
```
|      Start Time: 16:57:27 2025-01-14     Finished: 16:57:55 2025-01-14     Run Time: 0:00:27       |
```
Only 27 seconds.

Show only, no overlay changes, so only the filtering:
```
|      Start Time: 16:57:56 2025-01-14     Finished: 16:58:23 2025-01-14     Run Time: 0:00:26       |
```
Saved a second, which makes sense since applying two overlays doesn't take long.

Removing the two overlays takes 6 seconds:
```
|      Start Time: 16:58:24 2025-01-14     Finished: 16:58:31 2025-01-14     Run Time: 0:00:06       |
```

### Shows + Seasons

Shows and Seasons, applying the two show overlays and the 33 season overlays for the first time:
```
|      Start Time: 16:54:13 2025-01-14     Finished: 16:55:43 2025-01-14     Run Time: 0:01:30       |
```
90 seconds; three times the two shows.  Not too bad since we're applying 35 overlays vs 2 overlays.

Shows and Seasons, no overlay changes, so only the filtering:
```
|      Start Time: 16:55:44 2025-01-14     Finished: 16:56:59 2025-01-14     Run Time: 0:01:14       |
```
74 seconds; 20% reduction relative to the full application.

Removing these 35 overlays:
```
|      Start Time: 16:57:00 2025-01-14     Finished: 16:57:26 2025-01-14     Run Time: 0:00:26       |
```
26 seconds.

### Shows + Seasons + Episodes

Shows, Seasons, and Episodes, applying 2 show, 33 season, and 631 episode overlays for the first time:
```
|      Start Time: 16:22:04 2025-01-14     Finished: 16:36:56 2025-01-14     Run Time: 0:14:52       |
```
Nearly 15 minutes, about ten times as long as the shows + seasons.  Keep in mind this is one overlay.

Shows, Seasons, and Episodes, no overlay changes, so only the filtering:
```
|      Start Time: 16:36:57 2025-01-14     Finished: 16:47:17 2025-01-14     Run Time: 0:10:19       |
```
Still ten minutes to do the filtering, but not actually applying overlays reduces the the time taken by about 4 minutes, or 30-odd percent.

Removing these 666 overlays:
```
|      Start Time: 16:47:18 2025-01-14     Finished: 16:54:12 2025-01-14     Run Time: 0:06:53       |
```
Nearly 7 minutes, up from 26 seconds to remove the 35 Shows and Season overlays.

![image](https://github.com/user-attachments/assets/0de211a4-bcbb-4b9a-9d77-e03f4aca7ab0)

While this is a small example, it should be clear that adding episode-level overlays is very expensive compared to show and season-level.

Don't pay any attention to the specific times here.  You cannot count on your library taking 15 minutes.

When I ran this same example against my typical Kometa test library of 92 shows, 613 seasons, and 11513 episodes, the all-three levels run took nearly five hours.  That's for one overlay.

The takeaway here should be the dramatic jump once episodes get involved.

Only you can decide whether putting overlays on episode images is worth the resources and time involved in doing so.
