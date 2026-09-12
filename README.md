# Spotify Widget for OBS Studio

A fully customizable Spotify widget that works with OBS Browser Source. Hosts on GitHub Pages with zero backend required.

## Features

- Real-time Spotify "now playing" display with album art
- Live progress bar synced to track position
- Animated now-playing indicator
- Full color customization (title, artist, progress bar, background)
- Adjustable art size, radius, and font sizes
- Spotify OAuth via PKCE (Authorization Code flow) — secure, no client secret needed
- Works directly on GitHub Pages — no server required

## Quick Setup

### 1. Get Spotify Client ID
1. Go to https://developer.spotify.com/dashboard
2. Create a new app (or use existing)
3. Copy the **Client ID**

### 2. Configure Redirect URI
In your Spotify app settings, add this redirect URI (already configured in the widget):
```
https://thelonewanderer2694.github.io/Vault-26-Music-Player/
```

### 3. Deploy to GitHub Pages
1. Push `spotify.html` to your repo `Vault-26-Music-Player`
2. Enable GitHub Pages in repo settings (source: main branch, root folder)
3. Your widget will be available at: `https://thelonewanderer2694.github.io/Vault-26-Music-Player/`

### 4. Use in OBS
1. In OBS, add a **Browser Source**
2. Paste your GitHub Pages URL
3. Set width/height as desired (e.g., 400x80)
4. The widget will show Spotify playback status

### 5. Authenticate
1. Open the widget in your browser
2. Click the widget or the gear icon → enter your Client ID → Save
3. You'll be redirected to Spotify to authorize
4. After authorization, you'll return to the widget showing your current track

### 6. Customize
Click the **gear icon** in the top-right to:
- Enter your Spotify Client ID
- Change colors (title, artist, progress, background)
- Adjust art size, radius, font sizes
- Toggle now-playing indicator
- Set poll interval

## Customization via URL Parameters

You can also customize the widget directly via URL query parameters:

```
https://YOUR_GITHUB_USERNAME.github.io/?artSize=64&artRadius=12&titleColor=#ff0000&artistColor=#00ff00&progressColor=#1db954&bgColor=#000000
```

| Parameter | Description | Default |
|-----------|-------------|---------|
| `artSize` | Album art size in px | `56` |
| `artRadius` | Album art corner radius | `8` |
| `titleSize` | Track title font size | `15` |
| `artistSize` | Artist font size | `13` |
| `titleColor` | Track title color | `#ffffff` |
| `artistColor` | Artist name color | `#b3b3b3` |
| `progressColor` | Progress bar color | `#1db954` |
| `bgColor` | Background color | `#121212` |
| `showIndicator` | Show animated indicator | `true` |
| `pollInterval` | Polling interval in ms | `5000` |

## How It Works

- Uses **Authorization Code Flow with PKCE** — secure OAuth without needing a client secret
- Generates a `code_verifier` and `code_challenge` locally, stores verifier in `localStorage`
- Redirects to Spotify with `response_type=code`, receives an auth code back
- Exchanges the code for an access token via POST to Spotify's token endpoint
- Polls Spotify Web API `/me/player/currently-playing` endpoint
- Album art fades in smoothly when track changes
- Progress bar updates in real-time based on track position

## Troubleshooting

**"Invalid redirect URI" error:**
- Ensure your GitHub Pages URL matches exactly what's in Spotify Dashboard
- Must be `https://YOUR_USERNAME.github.io/` (with trailing slash)

**"Client ID not found" or "Auth failed":**
- Open the config panel (gear icon)
- Enter your Spotify Client ID
- Click "Save & Reload"

**Not showing current track:**
- Make sure Spotify is playing on your account
- Check that you granted the required permissions
- Ensure you're using the same Spotify account that authorized the widget

**Token expired:**
- Click the widget to re-authenticate
- Or use the "Clear Auth" button in settings, then re-authenticate

## Security Notes

- This widget uses PKCE, which is the recommended OAuth flow for public clients
- No client secret is needed or stored
- Access tokens are stored in `localStorage` (acceptable for a single-user widget)
- Code verifiers are generated fresh on each authentication attempt

## License

MIT