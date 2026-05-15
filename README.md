# starfleetmedical_oauth_landing

Static GitHub-Pages site that serves as the OAuth 2.0 redirect URI for personal-vault data pullers (Withings, Strava, anything else that requires a publicly-reachable callback URL).

## Why this exists

Several OAuth providers (notably Withings) validate at app-registration time that the callback URL is reachable from their servers. `http://localhost:...` is rejected, and auth-protected URLs behind Authentik / Cloudflare Access return 401/302 instead of 200, also rejected. This repo hosts a static `index.html` that returns a plain 200 from any path — satisfying the reachability check while letting the puller scripts capture the `code` parameter from the browser's URL bar after redirect.

## Usage

Register `https://yadavravi.github.io/starfleetmedical_oauth_landing/` (or any sub-path like `.../withings/callback`) as the redirect URI in the OAuth provider's developer dashboard.

After the user authorizes, the provider redirects the browser here with `?code=...&state=...` in the URL. The page parses those parameters via client-side JS and displays the authorization code with a "Copy" button. The code stays in the browser — no telemetry, no third-party requests.

## Privacy

- No analytics, no trackers, no third-party scripts.
- Query-string parameters (including OAuth codes) are read in client-side JS only and never sent anywhere.
- View the source — it's one HTML file.

## License

MIT — see [LICENSE](LICENSE) if present, otherwise consider this notice the license.
