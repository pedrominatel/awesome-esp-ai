# ESP-IDF Migration Paths to 6.0

Resolve the exact target patch release first. Use official documentation matching
that release and treat a local checkout only as supporting context.

Use `eim list` to discover installed versions and absolute paths, then verify the
target with:

```sh
eim run "idf.py --version" <TARGET_IDF_VERSION>
```

If EIM is unavailable, inspect the active `idf.py --version` and `IDF_PATH`. Ask
the user when the exact source or target still cannot be identified.

## Rule

If the application starts before 5.5, review every intermediate migration guide through 5.5 before applying the 5.5 to 6.0 guide.

## Version-Matched Guide URLs

Replace `<TARGET_IDF_VERSION>` with the exact target, for example `v6.0.2`:

```text
https://docs.espressif.com/projects/esp-idf/en/<TARGET_IDF_VERSION>/esp32/migration-guides/release-5.x/5.0/index.html
https://docs.espressif.com/projects/esp-idf/en/<TARGET_IDF_VERSION>/esp32/migration-guides/release-5.x/5.1/index.html
https://docs.espressif.com/projects/esp-idf/en/<TARGET_IDF_VERSION>/esp32/migration-guides/release-5.x/5.2/index.html
https://docs.espressif.com/projects/esp-idf/en/<TARGET_IDF_VERSION>/esp32/migration-guides/release-5.x/5.3/index.html
https://docs.espressif.com/projects/esp-idf/en/<TARGET_IDF_VERSION>/esp32/migration-guides/release-5.x/5.4/index.html
https://docs.espressif.com/projects/esp-idf/en/<TARGET_IDF_VERSION>/esp32/migration-guides/release-5.x/5.5/index.html
https://docs.espressif.com/projects/esp-idf/en/<TARGET_IDF_VERSION>/esp32/migration-guides/release-6.x/6.0/index.html
```

## Guide Chain by Starting Version

- 5.5: 6.0
- 5.4: 5.5, 6.0
- 5.3: 5.4, 5.5, 6.0
- 5.2: 5.3, 5.4, 5.5, 6.0
- 5.1: 5.2, 5.3, 5.4, 5.5, 6.0
- 5.0: 5.1, 5.2, 5.3, 5.4, 5.5, 6.0
- 4.4: 5.0, 5.1, 5.2, 5.3, 5.4, 5.5, 6.0
- Older than 4.4: define and approve a staged migration plan before editing.
