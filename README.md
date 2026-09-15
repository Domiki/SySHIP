# SySHIP Releases

[![Documentation Status](https://readthedocs.org/projects/syship/badge/?version=latest)](https://syship.readthedocs.io/en/latest/?badge=latest)

This repository hosts the public distribution and update-check metadata for SySHIP. It does not contain application source code.

## Download

Get the latest installer from the [Releases](https://github.com/Domiki/SySHIP/releases) page.

## Documentation

Full usage docs: **[syship.readthedocs.io](https://syship.readthedocs.io)**

## latest.json

`latest.json` is read by the app on startup to check for a newer version. It is updated automatically by [.github/workflows/update-latest-json.yml](.github/workflows/update-latest-json.yml) whenever a new GitHub Release is published — no manual edits needed.
