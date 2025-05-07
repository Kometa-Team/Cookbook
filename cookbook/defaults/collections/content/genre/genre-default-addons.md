# Using addons in Genre Defaults

My test library had 36 movies with the "Family" genre assigned.  I changed 18 of those to "Familie".

![image](https://github.com/user-attachments/assets/204e97e5-efc8-4048-a6ae-0d7a74f875c5)

![image](https://github.com/user-attachments/assets/0fbb7909-e1a9-4094-a41d-34453dd0b69b)

Now we'll run the genre default:

```yaml
libraries:
  Movies:
    collection_files:
    - default: genre
```

That produces "Familie" and "Family" collections, each containing 18 items, as expected:

![image](https://github.com/user-attachments/assets/26303a57-4084-4968-9d1e-396bd57fe07c)

We want only a single "Family" collection, so we add an "`add-on`" to do that:

```yaml
libraries:
  Movies:
    collection_files:
    - default: genre
      template_variables:
        append_addons:
          Family:
          - "Familie"
```
And we run Kometa again.

Now the "Family" collection contains 36 items, as desired.  However, the "Familie" collection is still present:

![image](https://github.com/user-attachments/assets/45033667-e859-41d2-a1b2-7253765e683a)

This is because Kometa typically doesn't delete leftover collections after you disable them in the config.

The log shows that the "Familie" collection isn't touched as part of this new run:

```
| Crime Movies           |     0 |     0 |     0 |  0:00:00 | Unchanged                              |
| Drama Movies           |     0 |     0 |     0 |  0:00:00 | Unchanged                              |
| Family Movies          |     0 |     0 |     0 |  0:00:00 | Unchanged                              |
| Fantasy Movies         |     0 |     0 |     0 |  0:00:00 | Unchanged                              |
| Food Movies            |     0 |     0 |     0 |  0:00:00 | Unchanged                              |
```

One way to get rid of it would be to explicitly tell Kometa to do that:

```yaml
libraries:
  Movies:
    collection_files:
    - default: genre
      template_variables:
        append_addons:
          Family:
          - "Familie"
    operations:
      delete_collections:
        configured: false  # delete collections that aren't in the config
```

Which results in:

```
|====================================== Collection Operations =======================================|
|                                                                                                    |
|                                                                                                    |
|======================================= Deleting Collections =======================================|
|                                                                                                    |
| Familie Movies Deleted                                                                             |
```

And:

![image](https://github.com/user-attachments/assets/0663eb3f-3fa3-47b9-8e65-e9d7d995f6ea)

That's a blunt instrument, though, and may delete more than you intend.

The simplest thing would be to delete them manually in the UI, since you're jsut doing a one-time cleanup on the consolidated genres.
