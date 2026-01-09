## 2026-01-09 (Docs)
- Updated the documentation comment in static/language.js to reflect new multi-language structure and usage instructions.
# Changelog


## [Unreleased]
- General improvements and bugfixes.
- Added settings.example.json as a template for configuration.
- Anonymized EIC nicknames in settings.example.json for safe sharing.
- run.sh now exits with an error if settings.json does not exist, instead of creating a default file.

## 2026-01-09 (UI/Language)
- Refactored static text in index.html to use a language.js file for all UI strings.
- Added Estonian language support (LANGUAGES.et) in language.js.
- Made language switching user-friendly: use setLanguage('en') or setLanguage('et') and getLang() for current language.

## 2026-01-09
- Switched Docker base image to python:3.11-alpine, reducing image size from 169MB to 66MB.
- Aggressively slimmed Docker image with multi-stage build and removal of build dependencies.
- Only copy required files into Docker image for security and size.
- Added multi-arch build.sh script (local build only, no push).
- run.sh now deletes old image before building, tags as :latest, and uses --restart always.
- run.sh and Dockerfile now default to WSGI (Waitress) for production.
- All template CSS moved to static/custom.css.
- UI: 'Data fetched' changed to 'Data updated', right-aligned, and reduced top margin.
- Plotly controls (zoom, screenshot, etc.) removed from chart.
- Today's value excluded from min calculation in summary, shown as today_kwh.
- README: added screenshot, API credential instructions, and manual refresh info.

## 2026-01-08
- Initial project setup: Flask app, Plotly frontend, caching, settings.json, Docker, .gitignore, README, and basic CI scripts.
- Added support for Elering/Estfeed API with OAuth2 credentials.
- Added color and nickname support for EICs in settings.json.
- Added Docker and run.sh for easy deployment.
