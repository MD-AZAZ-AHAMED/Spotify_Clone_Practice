# Spotify_Clone_Practice

# Spotify Clone 🎵

A full-stack web application that replicates the core features of Spotify. Users can browse, play music, create playlists, and manage their music library. Admin users have the ability to upload and manage songs and albums.

## 📚 Table of Contents

- [Overview](#overview)
- [Tech Stack](#tech-stack)
- [Features](#features)
- [Project Structure](#project-structure)
- [Backend Architecture](#backend-architecture)
- [Frontend Architecture](#frontend-architecture)
- [Installation & Setup](#installation--setup)
- [Environment Variables](#environment-variables)
- [API Endpoints](#api-endpoints)
- [Component Details](#component-details)
- [How It Works](#how-it-works)

---

## Overview

This is a full-stack MERN (MongoDB, Express, React, Node.js) application that allows users to:
- Register and authenticate securely using JWT tokens
- Browse and play songs organized by albums
- Save songs to personal playlists
- Stream audio with playback controls
- Manage song libraries (admin users only)
- Upload album thumbnails and audio files to cloud storage (Cloudinary)

---

## Tech Stack

### Backend
- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MongoDB with Mongoose ODM
- **Authentication**: JWT (JSON Web Tokens) + bcrypt
- **Cloud Storage**: Cloudinary (for images and audio files)
- **File Handling**: Multer (for file uploads)
- **Environment**: dotenv (for configuration)
- **Dev Tool**: Nodemon (for auto-reload)

### Frontend
- **Framework**: React 18
- **Build Tool**: Vite
- **Styling**: Tailwind CSS + PostCSS
- **Routing**: React Router DOM v6
- **HTTP Client**: Axios
- **Notifications**: React Hot Toast
- **Icons**: React Icons
- **State Management**: React Context API

---

## Features

### User Features
✅ **Authentication**
- Register with name, email, and password
- Secure login with JWT tokens
- Password hashing using bcrypt
- Session management with HTTP-only cookies

✅ **Music Browsing**
- Browse all available songs and albums
- View album details with song listings
- Search and filter capabilities

✅ **Music Playback**
- Play, pause, resume music
- Next/Previous song navigation
- Progress bar with seek functionality
- Volume control
- Display currently playing song info (title, artist, duration)

✅ **Playlist Management**
- Save songs to personal playlist
- View saved playlist
- Remove songs from playlist
- Persistent playlist storage in user profile

✅ **User Profile**
- View personal profile information
- Manage playlist
- Logout functionality

### Admin Features
✅ **Album Management**
- Create new albums with title, description, and thumbnail
- View all albums
- Edit album information

✅ **Song Management**
- Add songs to albums with audio files
- Specify song title, description, singer name
- Upload audio files and thumbnails
- Add/Update song thumbnails
- Delete songs from library
- View all uploaded songs with details

---

## Project Structure

```
spotify-master/
├── backend/
│   ├── index.js                          # Server entry point
│   ├── controllers/
│   │   ├── songControllers.js            # Song & Album logic
│   │   └── userControllers.js            # User auth & playlist logic
│   ├── routes/
│   │   ├── songRoutes.js                 # Song & Album endpoints
│   │   └── userRoutes.js                 # User endpoints
│   ├── models/
│   │   ├── User.js                       # User schema with playlist
│   │   ├── Song.js                       # Song schema with audio & thumbnail
│   │   └── Album.js                      # Album schema
│   ├── middlewares/
│   │   ├── isAuth.js                     # JWT verification middleware
│   │   └── multer.js                     # File upload configuration
│   ├── database/
│   │   └── db.js                         # MongoDB connection
│   ├── utils/
│   │   ├── generateToken.js              # JWT token generation
│   │   ├── TryCatch.js                   # Error handling wrapper
│   │   └── urlGenerator.js               # Data URI conversion for uploads
│   └── package.json
├── frontend/
│   ├── src/
│   │   ├── main.jsx                      # React app entry point
│   │   ├── App.jsx                       # Main app routing
│   │   ├── index.css                     # Global styles
│   │   ├── context/
│   │   │   ├── User.jsx                  # User auth context
│   │   │   └── Song.jsx                  # Song & Album context
│   │   ├── pages/
│   │   │   ├── Home.jsx                  # Albums & songs display
│   │   │   ├── Login.jsx                 # Login form
│   │   │   ├── Register.jsx              # Registration form
│   │   │   ├── PlayList.jsx              # User's saved songs
│   │   │   ├── Album.jsx                 # Album detail page
│   │   │   └── Admin.jsx                 # Admin dashboard
│   │   ├── components/
│   │   │   ├── Layout.jsx                # Main layout wrapper
│   │   │   ├── Navbar.jsx                # Top navigation bar
│   │   │   ├── Sidebar.jsx               # Left navigation panel
│   │   │   ├── Player.jsx                # Music player controls
│   │   │   ├── SongItem.jsx              # Song card component
│   │   │   ├── AlbumItem.jsx             # Album card component
│   │   │   ├── PlayListCard.jsx          # Playlist preview card
│   │   │   └── Loading.jsx               # Loading spinner
│   │   ├── assets/
│   │   │   └── assets.js                 # Images & static assets
│   ├── package.json
│   ├── vite.config.js                    # Vite build configuration
│   ├── tailwind.config.js                # Tailwind CSS setup
│   └── postcss.config.js
└── package.json                          # Root package.json
```

---

## Backend Architecture

### Database Models

#### User Model
```javascript
{
  name: String (required),
  email: String (required, unique),
  password: String (hashed, required),
  role: String (default: "user", can be "admin"),
  playlist: [String] (array of song IDs),
  timestamps: true
}
```

#### Song Model
```javascript
{
  title: String (required),
  description: String (required),
  singer: String (required),
  album: String (required, album ID),
  thumbnail: {
    id: String (Cloudinary public ID),
    url: String (Cloudinary URL)
  },
  audio: {
    id: String (Cloudinary public ID),
    url: String (Cloudinary URL)
  },
  timestamps: true
}
```

#### Album Model
```javascript
{
  title: String (required),
  description: String (required),
  thumbnail: {
    id: String (Cloudinary public ID),
    url: String (Cloudinary URL)
  },
  timestamps: true
}
```

### Controllers

#### userControllers.js
- **registerUser**: Validates input, hashes password, creates user, sets JWT cookie
- **loginUser**: Verifies email/password, generates JWT token
- **myProfile**: Fetches authenticated user's profile data
- **logoutUser**: Clears authentication cookie
- **saveToPlaylist**: Adds/removes song from user's playlist

#### songControllers.js
- **createAlbum**: Admin-only, uploads thumbnail to Cloudinary, creates album
- **getAllAlbums**: Fetches all albums from database
- **addSong**: Admin-only, uploads audio to Cloudinary, creates song record
- **addThumbnail**: Admin-only, uploads/updates song thumbnail
- **getAllSongs**: Fetches all songs
- **getAllSongsByAlbum**: Gets album details and all songs in that album
- **getSingleSong**: Fetches single song by ID
- **deleteSong**: Admin-only, removes song from database

### Middleware

#### isAuth.js
- Verifies JWT token from cookies
- Extracts user ID from token
- Fetches user data and attaches to request object
- Throws 403 error if token missing or invalid

#### multer.js
- Configures memory storage for file uploads
- Accepts single file upload via "file" field
- No file size restrictions (should be limited in production)

### Utilities

#### generateToken.js
- Creates JWT token with 15-day expiration
- Sets HTTP-only, Secure, SameSite cookie
- Token payload contains user ID

#### TryCatch.js
- Higher-order function that wraps async handlers
- Catches errors and returns JSON error response
- Prevents unhandled promise rejections

#### urlGenerator.js
- Converts file buffer to Data URI format
- Uses DataURI parser
- Extracts file extension for proper formatting

---

## Frontend Architecture

### Context Providers

#### User Context (User.jsx)
**State**:
- `user`: Current user object
- `isAuth`: Authentication status
- `loading`: Initial load state
- `btnLoading`: Button loading state

**Methods**:
- `registerUser(name, email, password, navigate, fetchSongs, fetchAlbums)`: Creates new user account
- `loginUser(email, password, navigate, fetchSongs, fetchAlbums)`: Authenticates user
- `fetchUser()`: Retrieves current user data
- `logoutUser()`: Logs out and refreshes page
- `addToPlaylist(id)`: Toggles song in/out of playlist

#### Song Context (Song.jsx)
**State**:
- `songs`: Array of all songs
- `albums`: Array of all albums
- `selectedSong`: Currently selected song ID
- `song`: Current song details
- `isPlaying`: Playback status
- `albumSong`: Songs in selected album
- `albumData`: Album details
- `loading`: Operation loading state
- `index`: Current song index in playlist

**Methods**:
- `fetchSongs()`: Loads all songs
- `fetchAlbums()`: Loads all albums
- `fetchSingleSong()`: Gets current song details
- `fetchAlbumSong(id)`: Gets songs by album ID
- `addAlbum(formData, ...)`: Creates album (admin)
- `addSong(formData, ...)`: Adds song (admin)
- `addThumbnail(id, formData, ...)`: Updates song thumbnail (admin)
- `deleteSong(id)`: Removes song (admin)
- `nextMusic()`: Plays next song
- `prevMusic()`: Plays previous song

### Pages

#### Home.jsx
- Displays featured albums and songs
- Uses horizontal scrolling for album/song lists
- Links to album details page
- Allows quick play from song items

#### Login.jsx
- Email/password authentication form
- Links to registration page
- Shows loading state during login
- Redirects to home on successful login

#### Register.jsx
- New user registration form
- Name, email, password fields
- Links to login page
- Handles validation and error messages

#### PlayList.jsx
- Shows all songs in user's playlist
- Displays playlist name as "{UserName} PlayList"
- Shows song details (artist, description)
- Bookmark/play controls for each song
- Empty state with placeholder image

#### Album.jsx
- Displays songs in selected album
- Shows album cover and details
- Formatted table view with song listings
- Play and bookmark controls
- Dynamic route using album ID

#### Admin.jsx
- **Album Management**:
  - Form to add albums with title, description, thumbnail
  - Lists all albums with thumbnails
- **Song Management**:
  - Form to add songs (title, description, singer, album, audio file)
  - Displays all added songs with thumbnails
  - Add thumbnail button if missing
  - Delete button with confirmation
- Admin-only access verified by user role

### Components

#### Layout.jsx
- Wraps main content with sidebar, navbar, and player
- Three-section layout: sidebar, main content area, player
- Responsive design (sidebar hidden on mobile)

#### Navbar.jsx
- Top navigation bar
- Navigation arrows (back/forward)
- "Explore Premium" and "Install App" buttons
- Logout functionality
- Filter buttons (All, Music, Podcasts)
- Playlist shortcut on mobile

#### Sidebar.jsx
- Left navigation panel (desktop only)
- Home and search options
- "Your Library" section
- Playlist card
- Podcast suggestions
- Admin dashboard link (if user is admin)

#### Player.jsx
- Audio player controls at bottom
- Play/pause button
- Previous/next track navigation
- Current song info (thumbnail, title, duration)
- Progress bar with seek functionality
- Volume control
- Uses HTML5 audio element
- Auto-loads song based on selectedSong

#### SongItem.jsx
- Card component for individual songs
- Shows song image/thumbnail
- Displays title and description
- Hover effects with play and bookmark buttons
- Color-coded bookmark (filled if saved)
- Play control triggers song selection

#### AlbumItem.jsx
- Card component for albums
- Shows album thumbnail
- Displays truncated title and description
- Navigates to album detail page on click
- Hover effects

#### PlayListCard.jsx
- Displays user's playlist in sidebar
- Shows music icon and playlist name
- Shows creator (user) name
- Clickable navigation to playlist page

#### Loading.jsx
- Full-screen loading spinner
- Animated spinning circle
- Green color theme matching Spotify

### Routing Structure

```
/ → Home (if authenticated) / Login (if not)
/login → Login page
/register → Registration page
/playlist → User's saved songs
/album/:id → Album detail page
/admin → Admin dashboard (admin only)
```

---

## Installation & Setup

### Prerequisites
- Node.js (v14+)
- MongoDB (local or Atlas)
- Cloudinary account
- npm or yarn

### Backend Setup

1. **Navigate to backend**:
   ```bash
   cd backend
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Create `.env` file** in backend directory:
   ```
   PORT=5000
   MONGO_URL=mongodb+srv://username:password@cluster.mongodb.net/?retryWrites=true&w=majority
   Cloud_Name=your_cloudinary_cloud_name
   Cloud_Api=your_cloudinary_api_key
   Cloud_Secret=your_cloudinary_api_secret
   Jwt_secret=your_jwt_secret_key
   ```

4. **Start backend**:
   ```bash
   npm run dev
   ```

### Frontend Setup

1. **Navigate to frontend**:
   ```bash
   cd frontend
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Start development server**:
   ```bash
   npm run dev
   ```

The frontend will typically run on `http://localhost:5173`

### Production Build

1. **Build frontend**:
   ```bash
   npm run build
   ```

2. **Build entire project** (from root):
   ```bash
   npm run build
   ```

This compiles the React app and serves it from the Express backend.

---

## Environment Variables

### Backend (.env)
```
PORT=5000                                    # Express server port
MONGO_URL=                                   # MongoDB connection string
Cloud_Name=                                  # Cloudinary cloud name
Cloud_Api=                                   # Cloudinary API key
Cloud_Secret=                                # Cloudinary API secret
Jwt_secret=                                  # JWT signing secret
```

### Frontend
- No `.env` file needed
- Communicates with backend via relative paths (/api/*)

---

## API Endpoints

### User Routes (`/api/user`)

**POST /register**
- Create new user account
- Body: `{ name, email, password }`
- Returns: User object + authentication cookie

**POST /login**
- Authenticate user
- Body: `{ email, password }`
- Returns: User object + authentication cookie

**GET /me**
- Fetch current user profile
- Auth: Required (JWT)
- Returns: User object

**GET /logout**
- Clear authentication cookie
- Auth: Required (JWT)
- Returns: Success message

**POST /song/:id**
- Add/remove song from playlist
- Auth: Required (JWT)
- Params: Song ID
- Returns: Success/removal message + user data

### Song Routes (`/api/song`)

**POST /album/new**
- Create new album
- Auth: Required (JWT), Admin only
- Body: FormData with `title`, `description`, `file` (thumbnail)
- Returns: Success message

**GET /album/all**
- Fetch all albums
- Auth: Required (JWT)
- Returns: Array of albums

**POST /new**
- Add new song
- Auth: Required (JWT), Admin only
- Body: FormData with `title`, `description`, `singer`, `album`, `file` (audio)
- Returns: Success message

**POST /:id**
- Update song thumbnail
- Auth: Required (JWT), Admin only
- Params: Song ID
- Body: FormData with `file` (thumbnail)
- Returns: Success message

**GET /single/:id**
- Get single song details
- Auth: Required (JWT)
- Params: Song ID
- Returns: Song object with audio/thumbnail URLs

**GET /all**
- Fetch all songs
- Auth: Required (JWT)
- Returns: Array of songs

**GET /album/:id**
- Get album with its songs
- Auth: Required (JWT)
- Params: Album ID
- Returns: `{ album, songs }` objects

**DELETE /:id**
- Delete song
- Auth: Required (JWT), Admin only
- Params: Song ID
- Returns: Success message

---

## Component Details

### State Flow Diagram
```
User Context (Authentication)
├── user (object)
├── isAuth (boolean)
├── loading (boolean)
└── Methods: register, login, logout, addToPlaylist

        ↓ Provides user data to components

Song Context (Music Management)
├── songs (array)
├── albums (array)
├── selectedSong (string ID)
├── isPlaying (boolean)
└── Methods: fetchSongs, playSong, pauseSong, nextMusic, prevMusic

        ↓ Provides music data to components

Layout Component (Main UI)
├── Sidebar (navigation)
├── Navbar (top bar)
├── Main Content Area (pages/children)
└── Player (audio controls)
```

### Data Flow for Playing a Song
1. User clicks play button on SongItem or AlbumItem
2. `setSelectedSong(id)` and `setIsPlaying(true)` called
3. Song Context updates `selectedSong` state
4. Player component's `useEffect` detects change
5. `fetchSingleSong()` retrieves song with audio URL
6. Audio element plays the song
7. Progress and volume controls update audio state

### Data Flow for Saving Song to Playlist
1. User clicks bookmark on SongItem
2. `addToPlaylist(id)` called in User Context
3. Backend updates user's playlist array
4. Frontend updates local user state
5. SongItem re-renders with updated bookmark status

---

## How It Works

### Authentication Flow
1. **Register**: User submits name/email/password → Backend hashes password → Saves user → Issues JWT token → Sets HTTP-only cookie
2. **Login**: User submits email/password → Backend verifies → Issues JWT token → Sets HTTP-only cookie
3. **Authenticated Requests**: Frontend sends requests with JWT cookie → isAuth middleware verifies token → Attaches user to request
4. **Logout**: User clicks logout → Backend clears cookie → Frontend reloads page

### Music Playback Flow
1. **Load Songs**: On app load, Song Context fetches all songs/albums from database
2. **Select Song**: User clicks play on song → `selectedSong` state updates
3. **Fetch Details**: Player detects change → Fetches full song object with audio URL
4. **Play Audio**: HTML5 audio element loads URL from Cloudinary → Plays audio
5. **Controls**: Play/pause/volume/seek all manipulate audio element directly

### Admin Operations Flow
1. **Create Album**: Admin fills form → Uploads thumbnail → Multer converts to Data URI → Cloudinary stores → Creates album record in MongoDB
2. **Add Song**: Admin fills form → Uploads audio file → Cloudinary stores → Creates song record with URLs
3. **Update Thumbnail**: Admin uploads image → Cloudinary stores → Updates existing song record
4. **Delete Song**: Admin confirms → Backend deletes from MongoDB → Cloudinary media auto-cleaned (optional)

### File Upload Process
1. User selects file in form
2. File stored in React state
3. FormData object created with file
4. POST request sent to backend
5. Multer middleware stores in memory
6. Data URI parser converts buffer to proper format
7. Cloudinary uploads file and returns URLs
8. URLs stored in MongoDB document

---

## Key Features Explained

### JWT Authentication
- Tokens expire after 15 days
- Stored in HTTP-only cookies (CSRF protection)
- SameSite=strict prevents cross-site cookie access
- User ID encoded in token payload

### Role-Based Access Control
- User.role field determines permissions
- Only "admin" users can:
  - Create albums
  - Add songs
  - Delete songs
  - Update thumbnails
- Regular users can only consume content

### Cloudinary Integration
- Media files never stored on server
- Reduces backend storage costs
- Provides CDN distribution for fast delivery
- Returns secure URLs for files

### Playlist Management
- User.playlist stores array of song IDs
- Not full song objects (reduces database size)
- Songs filtered on frontend using filter() method
- Toggle behavior: same ID added twice removes it

### Responsive Design
- Sidebar hidden on mobile (lg:flex)
- Tailwind responsive utilities (md:, sm:)
- Player always visible
- Grid layouts adjust columns based on screen size

---

## Notes for Development

### Error Handling
- TryCatch utility wraps all async handlers
- Returns 500 status with error message
- Frontend toast notifications display errors
- Console logs available for debugging

### Loading States
- `loading` indicates admin operation in progress
- `btnLoading` shows form button state
- `songLoading` tracks song data loading
- Prevents duplicate requests during operations

### Optimization Tips
- Implement pagination for large song/album lists
- Add search functionality to filter songs
- Cache album data to reduce API calls
- Implement song shuffle and repeat modes
- Add queue management for song playback

### Security Considerations
- Passwords hashed with bcrypt (cost factor 10)
- JWT tokens have expiration
- Admin routes checked on backend
- Cloudinary URLs can be restricted by domain
- Implement rate limiting on sensitive endpoints

---

## Future Enhancements

- [ ] User profiles with avatar upload
- [ ] Social features (follow users, share playlists)
- [ ] Search and filtering across all content
- [ ] Recommendation algorithm
- [ ] Mobile app using React Native
- [ ] Offline mode with service workers
- [ ] Dark/light theme toggle
- [ ] Queue management and shuffle/repeat
- [ ] User activity history and analytics
- [ ] Collaborative playlists
- [ ] Song lyrics display
- [ ] Payment integration for premium features

---

## Troubleshooting

### Common Issues

**"Please Login" error on all requests**
- JWT token not being sent
- Check if cookies are enabled in browser
- Verify `.env` JWT_SECRET matches frontend origin
- Clear browser cookies and try again

**Audio files won't play**
- Check Cloudinary API credentials
- Verify audio URL is publicly accessible
- Check browser console for CORS errors
- Ensure audio file format is supported

**Images/thumbnails not loading**
- Verify Cloudinary cloud name is correct
- Check if image URLs are valid
- Images must be uploaded before songs

**Admin features not appearing**
- Ensure user role is "admin" in MongoDB
- Check admin condition in component rendering
- Verify auth middleware attaches user to request

**Database connection fails**
- Verify MongoDB connection string
- Check if IP is whitelisted in MongoDB Atlas
- Ensure database name matches (.env vs connection string)



**Last Updated**: May 2026
**Version**: 1.0.0
