# Haven Property Portal

Haven helps people discover homes in Yangon, Mandalay, and Bago. This workspace contains the responsive web client (`app/`), API and SQLite service (`api/`), and Expo mobile client (`mobile/`). Each project is maintained in its own GitHub repository and linked here as a Git submodule.

## Requirements

Install Node.js 20+ and pnpm. npm can also be used with the included lockfiles.

## Run web and API locally

1. In `api/`, install dependencies, copy `.env.example` to `.env`, then run `pnpm seed` and `pnpm dev`.
2. In `app/`, install dependencies, then run `pnpm dev`.
3. Open `http://localhost:5173`. The API listens at `http://localhost:4000`.

The web app reads `VITE_API_URL` from `.env.local` and defaults to `http://localhost:4000/api/v1`. API settings are listed in `api/.env.example`.

## Run mobile locally

In `mobile/`, install dependencies, copy `.env.example` to `.env`, set `EXPO_PUBLIC_API_URL`, then run `pnpm start`. On a physical phone, use your development computer's LAN address instead of `localhost`.

## Local demo accounts

- Member: `demo@example.test` / `demo-password-change-me`
- Administrator: `admin@example.test` / `admin-password-change-me`

These accounts are for local development only. Do not use them in a shared environment.

The web app and mobile client show illustrative listings when the API cannot supply results. API seed listings use Unsplash image URLs; see the [Unsplash License](https://unsplash.com/license) and review assets before using them beyond local development.
