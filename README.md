# GrdlHub - Secure PWA Hub with Minimalist Design

[![Built with Vite](https://img.shields.io/badge/Built%20with-Vite-646CFF?logo=vite&logoColor=white)](https://vitejs.dev/)
[![Firebase](https://img.shields.io/badge/Firebase-FFCA28?logo=firebase&logoColor=black)](https://firebase.google.com/)
[![PWA Ready](https://img.shields.io/badge/PWA-Ready-success)](https://web.dev/progressive-web-apps/)
[![Security First](https://img.shields.io/badge/Security-First-red)](https://owasp.org/)

A modern, **invite-only Progressive Web App (PWA)** built with security-first architecture and minimalist design principles inspired by jwevent.org. Features separated authentication, role-based access control, and a clean, professional interface.

## ✨ Features

### 🔐 Security-First Architecture
- **Isolated Authentication**: Completely separate auth module from main app
- **Invite-Only Access**: Secure invitation system with time-limited tokens
- **Role-Based Permissions**: Granular access control for different user roles
- **Content Security Policy**: Strict CSP headers for XSS protection
- **Secure Headers**: Anti-clickjacking, MIME sniffing prevention

### 🎨 Minimalist Design System
- **JW.org-Inspired**: Clean, professional aesthetics
- **Responsive Design**: Mobile-first approach with touch-friendly interface
- **Accessibility**: WCAG 2.1 AA compliant design
- **Consistent Theming**: Design tokens and color system

### 🚀 Modern Tech Stack
- **Frontend**: Vite + Vanilla JavaScript (ES6+)
- **Authentication**: Firebase Auth with email verification
- **Database**: Firestore for real-time data management
- **PWA**: Service Worker, offline capabilities, app installation
- **Offline Storage**: IndexedDB via idb library for local caching

### 📱 Progressive Web App
- **Installable**: Add to home screen on mobile and desktop
- **Offline Capable**: Works without internet connection
- **Fast Loading**: Code splitting and lazy loading
- **Real-time Updates**: Live data synchronization

## Tech Stack

- **Frontend**: Vite + Vanilla JavaScript
- **Authentication**: Firebase Auth
- **Database**: Firestore
- **PWA**: Vite PWA Plugin with Workbox
- **Styling**: Modern CSS with CSS Grid/Flexbox
- **Offline Storage**: IndexedDB via idb library

## Project Structure

```
src/
├── main.js                 # Main application entry point
├── style.css              # Minimalistic styles
├── auth.js                # Firebase authentication
├── accessControl.js       # Role-based access control
├── ui.js                  # UI components and modals
├── utils/
│   └── notifications.js   # Notification system
└── pages/
    ├── users.js           # User management
    ├── pages.js           # Static page management
    ├── content.js         # Dynamic content management
    └── settings.js        # App configuration
```

## Getting Started

### Prerequisites

- Node.js (v16 or higher)
- Firebase project with Auth and Firestore enabled

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd GrdlHub
```

2. Install dependencies:
```bash
npm install
```

3. Configure Firebase:
   - Update `src/auth.js` with your Firebase configuration
   - Set up Firestore security rules
   - Enable Authentication providers in Firebase Console

4. Start development server:
```bash
npm run dev
```

5. Build for production:
```bash
npm run build
```

6. Preview production build:
```bash
npm run preview
```

## Firebase Setup

### 1. Firestore Database

Create the following collections:

#### Users Collection (`users`)
```javascript
{
  email: "user@example.com",
  name: "User Name",
  role: "admin" | "user",
  permissions: ["home", "users", "pages", "content", "settings"],
  status: "active" | "invited" | "disabled",
  createdAt: Timestamp,
  lastLogin: Timestamp
}
```

### 2. Security Rules

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can read their own data, admins can read all
    match /users/{userId} {
      allow read: if request.auth != null && 
        (request.auth.uid == userId || 
         isAdmin(request.auth.uid));
      allow write: if request.auth != null && 
        isAdmin(request.auth.uid);
    }
    
    function isAdmin(userId) {
      return get(/databases/$(database)/documents/users/$(userId)).data.role == 'admin';
    }
  }
}
```

### 3. Authentication

Enable Email/Password authentication in the Firebase Console.

## User Roles & Permissions

### Admin
- Full access to all features
- User management capabilities
- System configuration
- Default permissions: `["home", "users", "pages", "content", "settings"]`

### User
- Limited access based on assigned permissions
- Cannot manage other users
- Default permissions: `["home", "content"]`

## Authentication Flow

1. **Landing Page**: Non-authorized users see welcome page with "Request Access"
2. **Email Check**: User enters email to verify if they've been invited
3. **Invite System**: 
   - If user is invited but hasn't set up account → sends invite link
   - If user is active → shows they can sign in with existing credentials
   - If user not found → shows contact administrator message
4. **Account Setup**: Users follow invite link to create password and activate account
5. **Role-Based Access**: Navigation filtered by user permissions

### Invite Link Flow
- Admin invites user via email in Users Management
- System generates secure invite link (24-hour expiration)
- User clicks link and completes account setup
- No password required in auth form - everything is invite-based

## PWA Features

- **Offline Functionality**: Key features work without internet
- **App Installation**: Users can install as native app
- **Push Notifications**: Real-time updates
- **Background Sync**: Sync data when connection restored

## Development

### Available Scripts

- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build
- `npm run lint` - Run linting (if configured)

### Adding New Pages

1. Create page module in `src/pages/`
2. Add page section to `index.html`
3. Import and initialize in `main.js`
4. Update permissions in `accessControl.js`
5. Add navigation link to header

### Customization

- **Colors**: Update CSS custom properties in `style.css`
- **Fonts**: Modify font imports in `index.html`
- **Firebase Config**: Update `src/auth.js`
- **PWA Settings**: Modify `vite.config.js`

## Deployment

### Firebase Hosting

1. Install Firebase CLI:
```bash
npm install -g firebase-tools
```

2. Login and initialize:
```bash
firebase login
firebase init hosting
```

3. Build and deploy:
```bash
npm run build
firebase deploy
```

### Other Platforms

The app can be deployed to any static hosting service (Netlify, Vercel, etc.). Just run `npm run build` and deploy the `dist` folder.

## Contributing

1. Fork the repository
2. Create feature branch
3. Make changes
4. Test thoroughly
5. Submit pull request

## License

MIT License - see LICENSE file for details.

## Support

For support, please open an issue in the repository or contact the development team.
