# SySHIP Releases

This repository hosts the public distribution and update-check metadata for SySHIP. It does not contain application source code.

## Download

Get the latest installer from the [Releases](https://github.com/Domiki/SySHIP/releases) page.

## latest.json

`latest.json` is read by the app on startup to check for a newer version. It is updated automatically by [.github/workflows/update-latest-json.yml](.github/workflows/update-latest-json.yml) whenever a new GitHub Release is published — no manual edits needed.
