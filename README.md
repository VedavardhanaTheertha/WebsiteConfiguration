# WebsiteConfiguration
Configuration for the Main Website UI. Refer README from https://github.com/VedavardhanaTheertha/MainWebsiteUI for more information

`ui_elements.json` defines shared UI settings:

- `collections.initialVisible` and `collections.loadIncrement`: positive integer counts
  for the initial results and each "Show more" batch in searchable tile collections.
- `autoScroll.pixelsPerSecond`: a positive number controlling the forward speed of all
  auto-scrolling rows (default: 36 pixels per second). Higher values scroll faster.
- `autoScroll.returnDurationMs`: a positive integer controlling the animated return
  to the start (default: 2200 milliseconds). Lower values return faster.

Run `npm run settings:build` in the UI repository to regenerate settings while the local
server is running, or restart `npm run dev:local`. Builds regenerate them automatically.
