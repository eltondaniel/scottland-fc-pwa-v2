# Scottland FC Mobile App

## Overview

This is a React Native/Expo mobile application for Scottland Football Club that serves as a WebView wrapper for the club's website (scottlandfc.club). The app provides native mobile navigation while loading web content, giving fans a seamless app experience for accessing club information, fixtures, news, team details, and gallery content.

The app features a custom animated splash screen with parallax effects, a 6-tab bottom navigation structure, and handles offline states gracefully. Authentication is handled entirely by the website - the app itself requires no native auth implementation.

**Version**: 1.0
**Designed by**: Elton Daniel

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Framework**: React Native with Expo SDK 54
- Uses Expo's managed workflow with new architecture enabled
- React 19.1.0 with React Compiler experimental feature
- TypeScript for type safety throughout

**Navigation Structure**:
- `RootStackNavigator`: Top-level native stack navigator
- `MainTabNavigator`: Bottom tab navigator with 6 tabs (Home, Fixtures, News, Team, Gallery, More)
- Each tab loads a different URL from scottlandfc.club within a WebView

**Core Screen Pattern**:
- `WebViewScreen`: Reusable component that wraps react-native-webview with loading states, error handling, offline detection, and pull-to-refresh
- `MoreScreen`: Native screen with menu items that open web browser for Login, Contact, Membership, and Videos pages

**Theming System**:
- Dual theme support (light/dark) with automatic system preference detection
- Primary colors: Forest green (#1B4D3E) and gold (#C9A227)
- Centralized theme constants in `client/constants/theme.ts`
- `useTheme` hook provides theme object and dark mode boolean

**Animation & Gestures**:
- React Native Reanimated for performant animations
- React Native Gesture Handler for gesture support
- Custom `AnimatedSplash` component with parallax background effect (1.5s duration)

**State Management**:
- TanStack React Query for server state management
- Local component state with React hooks
- No global state management library needed (app is primarily WebView-based)

### Backend Architecture

**Server**: Express.js with TypeScript
- Minimal server setup primarily for development proxy and serving static assets
- CORS configured for Replit domains
- HTTP server created through `registerRoutes` function

**API Pattern**:
- Routes prefixed with `/api`
- Currently minimal API surface (app relies on external website)
- `apiRequest` helper function for making authenticated requests

### Data Storage

**Database**: PostgreSQL with Drizzle ORM
- Schema defined in `shared/schema.ts`
- Currently contains a basic `users` table (username, password)
- Drizzle-zod integration for schema validation
- In-memory storage fallback available (`MemStorage` class)

**Note**: The database is provisioned but minimally used since the app is a WebView wrapper. The existing schema serves as a foundation if native features requiring data persistence are added later.

### Path Aliases

- `@/` → `./client/` (client-side code)
- `@shared/` → `./shared/` (shared code between client and server)

## Project Structure

```
client/
├── App.tsx                    # Main app entry with splash screen logic
├── components/
│   ├── AnimatedSplash.tsx     # Parallax splash screen animation
│   ├── ErrorBoundary.tsx      # App crash handler
│   ├── ErrorScreen.tsx        # Offline/error UI
│   ├── HeaderTitle.tsx        # Custom header with logo
│   ├── ShimmerLoader.tsx      # Loading skeleton animation
│   └── ...                    # Other reusable components
├── screens/
│   ├── WebViewScreen.tsx      # WebView wrapper with loading/error states
│   └── MoreScreen.tsx         # Native menu screen
├── navigation/
│   ├── RootStackNavigator.tsx
│   └── MainTabNavigator.tsx   # 6-tab bottom navigation
├── constants/
│   └── theme.ts               # Colors, spacing, typography
└── hooks/
    ├── useTheme.ts
    └── useScreenOptions.ts

assets/images/
├── icon.png                   # App icon
├── splash-icon.png            # Splash screen logo
└── splash-background.jpg      # Parallax background image

server/
├── index.ts                   # Express server setup
├── routes.ts                  # API routes
└── storage.ts                 # Database operations
```

## External Dependencies

### Third-Party Services

**Primary External Service**:
- scottlandfc.club - All content is loaded from this website via WebView
- The app is fundamentally a native wrapper for this web property

### Key Libraries

**UI & Navigation**:
- `@react-navigation/native` + `@react-navigation/bottom-tabs` + `@react-navigation/native-stack`
- `expo-blur` for blur effects on tab bar
- `expo-glass-effect` for iOS liquid glass styling
- `react-native-webview` for embedding website content

**Network & Data**:
- `@react-native-community/netinfo` for connectivity detection
- `@tanstack/react-query` for data fetching patterns
- `drizzle-orm` + `pg` for database operations

**Animations**:
- `react-native-reanimated` for declarative animations
- `react-native-gesture-handler` for gesture recognition

**Utilities**:
- `expo-web-browser` for opening external links
- `expo-haptics` for haptic feedback
- `expo-image` for optimized image loading
- `zod` for runtime validation

### Database

- PostgreSQL (connection via `DATABASE_URL` environment variable)
- Drizzle ORM for type-safe database queries
- Migrations stored in `./migrations` directory

## Development

### Running the App

```bash
npm run all:dev
```

This starts both the Expo development server (port 8081) and the Express backend (port 5000).

### Testing on Device

Scan the QR code from Replit's URL bar menu to test on a physical device via Expo Go.

### Building for Production

The app uses Expo's static deployment system for production builds. See Expo documentation for EAS Build setup.

## Features

### MVP (Completed)
- Parallax splash screen with animated logo
- 6-tab bottom navigation (Home, Fixtures, News, Team, Gallery, More)
- WebView loading content from scottlandfc.club
- Pull-to-refresh functionality
- Shimmer skeleton loading animations
- Offline/error handling screens
- File upload/download support in WebView
- Local caching for fast navigation

### Future Enhancements
- Push notifications for match updates
- In-app share functionality
- Favorites/bookmarks feature
- Haptic feedback on navigation
