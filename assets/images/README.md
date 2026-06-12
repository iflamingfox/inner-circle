# Image folders

Where to put images for the site. Drop files in the right folder (or just hand
them to Claude and say which event they're for).

```
assets/images/
├── events/
│   └── <event-slug>/              ← one folder per concert (e.g. rimpa-siva)
│       ├── header.jpg             ← EVENT HEADER  — wide banner across the top of the concert page
│       ├── listing.jpg            ← EVENT LISTING — the thumbnail shown in lists, the home page & archive
│       ├── artists/               ← ADDITIONAL ARTISTS — drop one photo per performer
│       │   ├── uday-bhawalkar.jpg
│       │   └── pratap-awad.jpg
│       └── instruments/           ← INSTRUMENTS — drop one photo per instrument
│           ├── pakhawaj.jpg
│           └── tanpura.jpg
├── house/                         ← venue / "The House" photography
└── logo.png
```

## Notes

- **event-slug** is the concert's filename, e.g. the file
  `content/concerts/rimpa-siva.md` uses the folder `events/rimpa-siva/`.
- **header.jpg** — looks best **wide / landscape** (it becomes a full-width band).
- **listing.jpg** — looks best **square or portrait** (it's cropped to a tile).
  If you don't provide one, the header image is used instead.
- **artists/** and **instruments/** — any images you drop here appear
  automatically in a gallery on that concert's page. The file name (minus the
  extension, dashes → spaces) is used as the caption, so name them well:
  `uday-bhawalkar.jpg` → "Uday Bhawalkar".
- Formats: `.jpg`, `.jpeg`, `.png`, or `.webp`. Hugo resizes/optimises them, so
  bigger originals are fine — don't shrink them yourself.
- The empty `.gitkeep` files just keep empty folders tracked in git; ignore them.
```
