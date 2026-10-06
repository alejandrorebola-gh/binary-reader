# Universal Binary Inspector

A standalone, browser-based tool for inspecting unknown binary files (`.bin` and other formats).

## Features

- Runs entirely in the browser; selected files are not uploaded.
- Recognizes common file signatures such as PNG, JPEG, PDF, ZIP, GZIP, TIFF, WAV, SQLite, HDF5, FITS, ELF, and PE.
- Detects likely text headers followed by binary payloads.
- Computes byte entropy and printable/NUL-byte statistics.
- Searches for plausible fixed-size record structures.
- Tests candidate record widths and monotonic 64-bit fields.
- Highlights low-cardinality fields that may represent channels, flags, or record types.
- Hex + ASCII viewer with arbitrary offsets.
- Printable-string extraction.
- Numeric decoding as signed/unsigned integers and 32/64-bit floating point, with selectable endianness.
- Includes recognition for the DGX16 list-mode format used during development.

## Run

There are no dependencies and no build step.

1. Download or clone this repository.
2. Open `index.html` in a modern browser.
3. Choose a binary file to inspect.

You can also host `index.html` as a static site, including with GitHub Pages.

## Privacy

The application uses the browser's local File API. Binary files selected by the user are processed locally and are not sent to a server by this application.

## Important limitation

Automatic binary-format detection is heuristic. The app can often identify structural properties such as record width, byte order, counters, timestamps, and low-cardinality fields, but it cannot reliably infer the physical or semantic meaning of proprietary fields without documentation or contextual information.

## Example

For a DGX16 list-mode export, the detector can identify a likely structure consisting of:

```text
512-byte text/NUL-padded header
12-byte records:
  uint64 little-endian timestamp
  uint16 little-endian value
  uint8 channel
  uint8 flags
```

The tool reports this as a hypothesis and keeps the raw evidence visible for verification.