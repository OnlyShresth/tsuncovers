# Tsun — Manga 3×3 Grid Builder

A manga archive and 3×3 grid builder with a distinctive tsundere personality. Browse manga by genre or color, build custom 3×3 grids, and save them for later.

## ✨ Features

### 🏠 Main Page (index.html)
- **Genre Carousels**: Browse manga by Action, Romance, Fantasy, Horror, Comedy, Drama, Mystery, and Sci-Fi
- **Color Filtering**: Filter manga covers by dominant color (red, blue, green, yellow, purple, pink, orange, brown, gray, black, white)
- **Search**: Real-time autocomplete search for manga
- **Theme Toggle**: Switch between dark and light modes
- **Google Login**: Sign in to save grids across devices (requires backend setup)

### 🎨 3×3 Builder (builder.html)
- **Grid Building**: Search and add any manga to create 3×3 grids
- **Persistent Storage**: Grids are saved to localStorage and persist across sessions
- **Download**: Export grids as high-quality PNG images
- **Save & Manage**: Save multiple grids with custom names
- **Preview**: View and download previously saved grids
- **Direct Adding**: Click manga from main page to add directly to builder

## 🚀 Setup Instructions

### 1. Basic Setup

Simply open `index.html` in a web browser. The site works entirely client-side with no server required for basic functionality.

### 2. Google OAuth Setup (Optional)

To enable Google login for syncing grids across devices:

1. **Get Google OAuth Credentials**:
   - Go to [Google Cloud Console](https://console.cloud.google.com/)
   - Create a new project or select existing one
   - Enable "Google+ API"
   - Go to "Credentials" → "Create Credentials" → "OAuth client ID"
   - Select "Web application"
   - Add authorized JavaScript origins: `http://localhost`, `https://yourdomain.com`
   - Copy your Client ID

2. **Update the Code**:
   In both `index.html` and `builder.html`, replace:
   ```javascript
   const GOOGLE_CLIENT_ID = 'YOUR_GOOGLE_CLIENT_ID.apps.googleusercontent.com';
   ```
   With your actual Client ID.

3. **Backend Integration** (Required for full Google login functionality):
   The frontend is ready for backend integration. You'll need to:
   
   - Create an API endpoint to save grids: `POST /api/grids`
   - Create an API endpoint to fetch grids: `GET /api/grids?userId=XXX`
   - Store user data and grids in a database (MongoDB, PostgreSQL, etc.)
   
   Example backend structure:
   ```javascript
   // Grid Schema
   {
     id: Number,
     userId: String,
     name: String,
     manga: Array<{id, title, image}>,
     createdAt: Date
   }
   ```

   Update the `syncToBackend()` function in `builder.html` with your API endpoint.

## 📁 File Structure

```
tsun-manga/
├── index.html          # Main browsing page
├── builder.html        # 3×3 grid builder
└── README.md          # This file
```

## 🎨 Features Explained

### Color Filtering System
The color filter works by matching manga titles and descriptions against color keywords:
- **Red**: "red", "crimson", "scarlet", "blood", "ruby"
- **Blue**: "blue", "azure", "ocean", "sky", "sapphire"
- **Green**: "green", "emerald", "forest", "jade"
- And more...

This is a simple keyword-based approach. For better results, you could:
1. Use a color extraction API (e.g., Color Thief)
2. Pre-process covers and store dominant colors
3. Use machine learning color classification

### Persistent Storage
- **Current Grid**: Saved to `localStorage` as `tsun_current_grid`
- **Saved Grids**: Saved to `localStorage` as `tsun_saved_grids`
- **User Data**: Saved to `localStorage` as `tsun_user`
- **Theme**: Saved to `localStorage` as `tsun_theme`

All data persists even after closing the browser.

### Carousel Loading Improvements
The genre carousels now:
- Use increased delay between API calls (600ms vs 350ms) to avoid rate limiting
- Implement exponential backoff retry logic
- Better error handling with user-friendly messages
- Load 20 items initially to increase chances of finding color matches

## 🔧 Customization

### Adding More Genres
Edit the `GENRE_IDS` object in `index.html`:
```javascript
const GENRE_IDS = { 
  'YourGenre': GENRE_ID, // Find IDs at https://docs.api.jikan.moe/
};
```

### Changing Colors
Edit CSS variables in `:root`:
```css
:root {
  --accent: #c9a96e;  /* Main accent color */
  --blush: #c97070;   /* Tsundere personality color */
  /* etc... */
}
```

### Customizing Grid Size
Current default is 3×3. To change:
1. Update grid template: `grid-template-columns: repeat(3, 1fr)`
2. Update `currentGrid` array size: `Array(9).fill(null)`
3. Update canvas generation logic for different dimensions

## 🐛 Troubleshooting

### Carousels Not Loading
- **Issue**: Some genre carousels fail to load
- **Cause**: Jikan API rate limiting (3 requests/second)
- **Solution**: The code now includes automatic retry with backoff. If issues persist, increase the delay in `loadGenres()` function

### Google Login Not Working
- **Issue**: "Google SDK not loaded" error
- **Cause**: Google's GSI script blocked or failed to load
- **Solution**: Check your network connection and browser console for errors

### Images Not Loading in Downloaded Grids
- **Issue**: Blank spaces in downloaded PNG
- **Cause**: CORS restrictions on image URLs
- **Solution**: Images are loaded with `crossOrigin = 'anonymous'`. If issues persist, the images may need to be proxied through your own server

## 🎭 Personality Features

The site has a tsundere personality with:
- Dismissive but helpful UI copy
- Sarcastic notifications
- "Reluctant" feature descriptions
- Playful tsun-tsun comments throughout

Example lines:
- "It's not like I archived these for you"
- "b-baka! i just had extra server space..."
- "Whatever. Come back when you feel like it."

## 📝 API Credits

This site uses the [Jikan API](https://jikan.moe/) - an unofficial MyAnimeList API.
- Free and open source
- Rate limit: 3 requests/second, 60/minute
- No authentication required for basic usage

## 🚧 Future Enhancements

Potential features to add:
- [ ] Backend API for cloud sync
- [ ] User profiles and public grid sharing
- [ ] Advanced color extraction using image analysis
- [ ] Grid templates (2×2, 4×4, etc.)
- [ ] Social sharing (Twitter, Reddit)
- [ ] Drag-and-drop grid reordering
- [ ] Grid themes/filters
- [ ] Collections and folders for grids

## 📄 License

This project is provided as-is for personal use. The Jikan API has its own terms of service.

---

Built with ♥ and a bit of tsundere attitude
