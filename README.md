# AI Expense Tracker

> Intelligent personal finance management with on-device AI predictions and real-time insights

[![React Native](https://img.shields.io/badge/React%20Native-0.76.9-61DAFB?logo=react)](https://reactnative.dev/)
[![Expo](https://img.shields.io/badge/Expo-52.0.48-000020?logo=expo)](https://expo.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.3.3-3178C6?logo=typescript)](https://www.typescriptlang.org/)
[![Firebase](https://img.shields.io/badge/Firebase-11.0.2-FFCA28?logo=firebase)](https://firebase.google.com/)
[![License](https://img.shields.io/badge/License-Private-red)](LICENSE.txt)

---

## 📋 Table of Contents

- [Description](#-description)
- [About the Project](#-about-the-project)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Architecture Overview](#-architecture-overview)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Installation](#-installation)
- [Environment Variables](#-environment-variables)
- [Usage](#-usage)
- [API Documentation](#-api-documentation)
- [Database & Migrations](#-database--migrations)
- [Authentication & Authorization](#-authentication--authorization)
- [Error Handling & Logging](#-error-handling--logging)
- [Testing](#-testing)
- [CI/CD Pipeline](#-cicd-pipeline)
- [Deployment](#-deployment)
- [Performance & Security](#-performance--security)
- [Roadmap](#-roadmap)
- [Contributing](#-contributing)
- [Code of Conduct](#-code-of-conduct)
- [License](#-license)
- [Acknowledgements](#-acknowledgements)

---

## 📖 Description

AI Expense Tracker is a modern, cross-platform mobile application built with React Native and Expo that empowers users to manage their personal finances intelligently. The app combines traditional expense tracking with advanced AI-powered features including expense predictions, anomaly detection, and personalized financial coaching—all running on-device for maximum privacy and performance.

### Key Highlights

- **On-Device AI**: TensorFlow Lite model processes financial data locally
- **Real-Time Synchronization**: Firestore real-time listeners for instant updates
- **Multi-Wallet Support**: Track finances across multiple wallets with aggregated analytics
- **Intelligent Insights**: AI-driven predictions and anomaly detection
- **Privacy-First**: All AI processing happens on-device, no cloud uploads required

---

## 🎯 About the Project

### Problem Solved

Managing personal finances effectively requires constant tracking, analysis, and decision-making. Traditional expense tracking apps provide basic record-keeping but lack intelligent insights. AI Expense Tracker solves this by:

- Automating expense categorization and pattern recognition
- Predicting future spending based on historical data
- Detecting unusual spending patterns before they become problems
- Providing personalized financial advice without compromising privacy

### Target Users

- **Primary**: Individuals seeking intelligent personal finance management
- **Secondary**: Small business owners tracking business expenses
- **Tertiary**: Financial advisors and consultants

### Project Type

**Mobile Application** (iOS, Android, Web-capable)

---

## ✨ Features

### Core Features

| Feature | Description |
|---------|-------------|
| **User Authentication** | Secure email/password authentication via Firebase Auth with persistent sessions |
| **Wallet Management** | Create, edit, and delete multiple wallets with balance tracking and visual indicators |
| **Transaction Tracking** | Add, edit, and delete income/expense transactions with categories, dates, and receipts |
| **Real-Time Statistics** | Weekly, monthly, and yearly financial statistics with interactive bar charts |
| **AI Financial Assistant** | On-device AI predictions, anomaly detection, and personalized coaching |
| **Image Upload** | Attach receipt images to transactions and wallet icons via Cloudinary |
| **Search Functionality** | Real-time search through transactions by category, type, or description |
| **Profile Management** | Update user profile with name and profile picture |

### AI Features

- **Expense Predictions**: Forecast upcoming expenses based on historical spending patterns (7-day and 30-day forecasts)
- **Anomaly Detection**: Identify unusual spending behavior outside normal patterns with severity indicators
- **Personalized Coaching**: AI-generated financial advice tailored to user spending habits
- **Data Pipeline Visualization**: Interactive visualization of how AI processes financial data

---

## 🛠️ Tech Stack

### Frontend

| Technology | Version | Purpose |
|------------|---------|---------|
| React Native | 0.76.9 | Mobile framework |
| Expo | ~52.0.48 | Development platform and tooling |
| TypeScript | 5.3.3 | Type-safe development |
| Expo Router | ~4.0.22 | File-based routing |
| React Navigation | ^7.0.0 | Navigation primitives |
| React Native Reanimated | ~3.16.1 | Animations and gestures |
| React Native Gifted Charts | ^1.4.47 | Data visualization |

### Backend Services

| Service | Purpose |
|---------|---------|
| Firebase Authentication | User authentication and session management |
| Cloud Firestore | NoSQL database for real-time data synchronization |
| Cloudinary | Image storage and CDN for receipts and profile pictures |

### AI/ML

| Technology | Purpose |
|-----------|---------|
| TensorFlow Lite | On-device machine learning model for financial predictions |
| Custom Neural Network | Lightweight model optimized for expense forecasting |

### Development Tools

| Tool | Purpose |
|------|---------|
| Jest | Unit testing framework |
| ESLint | Code linting and quality |
| TypeScript | Static type checking |
| Expo CLI | Development and build tooling |

### UI Libraries

- `phosphor-react-native`: Icon library
- `react-native-element-dropdown`: Dropdown components
- `@react-native-community/datetimepicker`: Date selection
- `@shopify/flash-list`: High-performance list rendering
- `expo-linear-gradient`: Gradient effects
- `expo-image`: Optimized image rendering

---

## 🏗️ Architecture Overview

### Architecture Pattern

The application follows a **layered architecture** with clear separation of concerns:

```
┌─────────────────────────────────────────┐
│      Presentation Layer                  │
│  (Screens/Components - UI/UX)           │
├─────────────────────────────────────────┤
│      Business Logic Layer                │
│  (Services - Transaction, Wallet, etc.) │
├─────────────────────────────────────────┤
│      Data Access Layer                   │
│  (Firebase Firestore, Cloudinary)       │
├─────────────────────────────────────────┤
│      State Management                    │
│  (React Context API - AuthContext)      │
└─────────────────────────────────────────┘
```

### Key Architectural Decisions

1. **Service Layer Pattern**: Business logic separated into service modules (`services/`)
2. **Context API**: Global state management for authentication
3. **Custom Hooks**: Reusable data fetching logic (`useFetchData`)
4. **Component Composition**: Reusable UI components for consistency
5. **File-based Routing**: Expo Router for navigation structure

### Data Flow

```
User Action → Component → Service Layer → Firebase/Cloudinary → Real-time Update → UI Refresh
```

---

## 📂 Project Structure

```
AI-Expense-Tracker/
├── app/                          # Expo Router file-based routing
│   ├── (auth)/                   # Authentication screens (public routes)
│   │   ├── welcome.tsx           # Landing/welcome screen
│   │   ├── login.tsx             # Login screen
│   │   └── register.tsx          # Registration screen
│   ├── (tabs)/                   # Main app tabs (protected routes)
│   │   ├── _layout.tsx           # Tab navigation layout
│   │   ├── index.tsx             # Home screen (transactions list)
│   │   ├── statistics.tsx        # Statistics/charts screen
│   │   ├── wallet.tsx            # Wallet management screen
│   │   ├── ai.tsx                # AI assistant screen
│   │   └── profile.tsx           # User profile screen
│   ├── (modals)/                 # Modal screens (overlay routes)
│   │   ├── transactionModal.tsx  # Add/edit transaction modal
│   │   ├── walletModal.tsx       # Add/edit wallet modal
│   │   ├── categoryModal.tsx     # Category selection modal
│   │   ├── profileModal.tsx      # Profile edit modal
│   │   └── searchModal.tsx       # Search transactions modal
│   ├── _layout.tsx               # Root layout with AuthProvider
│   └── index.tsx                 # Entry point (redirects based on auth)
│
├── components/                    # Reusable UI components
│   ├── BackButton.tsx            # Navigation back button
│   ├── Button.tsx                # Custom button component
│   ├── CustomTabs.tsx            # Custom tab bar component
│   ├── Header.tsx                # Screen header component
│   ├── HomeCard.tsx              # Home screen summary card
│   ├── ImageUpload.tsx           # Image picker/upload component
│   ├── Input.tsx                 # Custom input field
│   ├── Loading.tsx               # Loading spinner component
│   ├── ModalWrapper.tsx          # Modal container wrapper
│   ├── ScreenWrapper.tsx         # Screen container wrapper
│   ├── TransactionList.tsx       # Transaction list component
│   ├── Typo.tsx                  # Typography component
│   └── WalletListItem.tsx        # Wallet list item component
│
├── config/                       # Configuration files
│   └── firebase.ts               # Firebase initialization & config
│
├── constants/                    # App constants
│   ├── data.ts                   # Static data (categories, transaction types)
│   ├── index.ts                  # Constants exports (Cloudinary config)
│   └── theme.ts                  # Theme colors, spacing, radius
│
├── contexts/                     # React Context providers
│   └── authContext.tsx           # Authentication context & state management
│
├── hooks/                        # Custom React hooks
│   └── useFetchData.ts           # Firestore real-time data fetching hook
│
├── services/                     # Business logic services
│   ├── imageService.ts           # Cloudinary image upload service
│   ├── transactionService.ts     # Transaction CRUD operations
│   ├── userService.ts            # User profile management
│   └── walletService.ts          # Wallet CRUD operations
│
├── utils/                        # Utility functions
│   ├── common.ts                 # Common helper functions (date utils)
│   └── styling.ts                # Styling utilities (scale, verticalScale)
│
├── assets/                       # Static assets
│   ├── fonts/                    # Custom fonts
│   └── images/                   # Image assets (icons, placeholders)
│
├── functions/                    # Firebase Cloud Functions (optional)
│   ├── index.js
│   └── package.json
│
├── scripts/                      # Build and utility scripts
│   └── reset-project.js          # Project reset script
│
├── types.ts                      # TypeScript type definitions
├── app.json                      # Expo configuration
├── package.json                  # Dependencies and scripts
├── tsconfig.json                 # TypeScript configuration
└── README.md                     # Project documentation
```

---

## 🚀 Getting Started

### Prerequisites

| Requirement | Version | Installation |
|-------------|---------|--------------|
| Node.js | v16+ | [Download](https://nodejs.org/) |
| npm | v7+ | Included with Node.js |
| Expo CLI | Latest | `npm install -g expo-cli` |
| iOS Simulator | Latest | Xcode (macOS only) |
| Android Emulator | Latest | Android Studio |
| Firebase Account | - | [Sign up](https://firebase.google.com/) |
| Cloudinary Account | - | [Sign up](https://cloudinary.com/) |

### System Requirements

- **macOS**: For iOS development (Xcode required)
- **Windows/Linux/macOS**: For Android development
- **Minimum RAM**: 8GB (16GB recommended)
- **Disk Space**: 2GB free space

---

## 📦 Installation

### 1. Clone the Repository

```bash
git clone <repository-url>
cd AI-Expense-Tracker
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Configure Firebase

1. Create a Firebase project at [Firebase Console](https://console.firebase.google.com/)
2. Enable **Email/Password** authentication
3. Create a **Firestore Database** (start in production mode)
4. Copy your Firebase config to `config/firebase.ts`:

```typescript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT_ID.appspot.com",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

5. Configure Firestore Security Rules (see [Database & Migrations](#-database--migrations))

### 4. Configure Cloudinary

1. Create a Cloudinary account at [Cloudinary](https://cloudinary.com/)
2. Get your **Cloud Name** from the dashboard
3. Create an **Upload Preset** named `images`
4. Update `constants/index.ts`:

```typescript
export const CLOUDINARY_CLOUD_NAME = "your-cloud-name";
export const CLOUDINARY_UPLOAD_PRESET = "images";
```

### 5. Start Development Server

```bash
npm start
```

---

## 🔐 Environment Variables

| Variable | Location | Description | Required |
|----------|----------|-------------|----------|
| `FIREBASE_API_KEY` | `config/firebase.ts` | Firebase API key | Yes |
| `FIREBASE_AUTH_DOMAIN` | `config/firebase.ts` | Firebase auth domain | Yes |
| `FIREBASE_PROJECT_ID` | `config/firebase.ts` | Firebase project ID | Yes |
| `FIREBASE_STORAGE_BUCKET` | `config/firebase.ts` | Firebase storage bucket | Yes |
| `FIREBASE_MESSAGING_SENDER_ID` | `config/firebase.ts` | Firebase messaging sender ID | Yes |
| `FIREBASE_APP_ID` | `config/firebase.ts` | Firebase app ID | Yes |
| `CLOUDINARY_CLOUD_NAME` | `constants/index.ts` | Cloudinary cloud name | Yes |
| `CLOUDINARY_UPLOAD_PRESET` | `constants/index.ts` | Cloudinary upload preset | Yes |

> **Note**: For production, consider using `expo-constants` with environment-specific configs or a secrets management service.

---

## 💻 Usage

### Development

```bash
# Start Expo development server
npm start

# Start with specific platform
npm run android    # Android
npm run ios        # iOS (macOS only)
npm run web        # Web browser
```

### Building

```bash
# Build for Android
eas build --platform android

# Build for iOS
eas build --platform ios

# Build for both platforms
eas build --platform all
```

### Production

```bash
# Create production build
expo build:android
expo build:ios

# Or use EAS Build (recommended)
eas build --profile production
```

### Available Scripts

| Script | Command | Description |
|--------|---------|-------------|
| `start` | `npm start` | Start Expo development server |
| `android` | `npm run android` | Run on Android emulator/device |
| `ios` | `npm run ios` | Run on iOS simulator/device |
| `web` | `npm run web` | Run in web browser |
| `test` | `npm test` | Run Jest tests |
| `lint` | `npm run lint` | Run ESLint |

---

## 📡 API Documentation

### Firebase Firestore Collections

#### Users Collection

**Path**: `users/{userId}`

| Field | Type | Description |
|-------|------|-------------|
| `uid` | `string` | Firebase Auth UID (document ID) |
| `email` | `string` | User email address |
| `name` | `string` | User display name |
| `image` | `string?` | Cloudinary URL for profile picture |

**Example Document**:
```json
{
  "uid": "abc123xyz789",
  "email": "user@example.com",
  "name": "John Doe",
  "image": "https://res.cloudinary.com/.../users/profile_abc123.jpg"
}
```

#### Wallets Collection

**Path**: `wallets/{walletId}`

| Field | Type | Description |
|-------|------|-------------|
| `id` | `string` | Firestore document ID |
| `name` | `string` | Wallet name |
| `amount` | `number` | Current balance |
| `totalIncome` | `number` | Total income ever received |
| `totalExpenses` | `number` | Total expenses ever made |
| `image` | `string` | Cloudinary URL for wallet icon |
| `uid` | `string` | Owner's Firebase Auth UID |
| `created` | `Date` | Wallet creation timestamp |

**Example Document**:
```json
{
  "id": "wallet_xyz123",
  "name": "Main Bank Account",
  "amount": 5000.50,
  "totalIncome": 15000.00,
  "totalExpenses": 10000.00,
  "image": "https://res.cloudinary.com/.../wallets/wallet_xyz123.jpg",
  "uid": "abc123xyz789",
  "created": {
    "_seconds": 1609459200,
    "_nanoseconds": 0
  }
}
```

#### Transactions Collection

**Path**: `transactions/{transactionId}`

| Field | Type | Description |
|-------|------|-------------|
| `id` | `string` | Firestore document ID |
| `type` | `"income" \| "expense"` | Transaction type |
| `amount` | `number` | Transaction amount (always positive) |
| `category` | `string?` | Expense category (required for expenses) |
| `date` | `Timestamp` | Transaction date |
| `description` | `string?` | Optional transaction description |
| `image` | `string?` | Cloudinary URL for receipt/image |
| `uid` | `string` | Owner's Firebase Auth UID |
| `walletId` | `string` | Associated wallet ID |

**Example Document**:
```json
{
  "id": "trans_expense_456",
  "type": "expense",
  "amount": 150.75,
  "category": "groceries",
  "date": {
    "_seconds": 1609459200,
    "_nanoseconds": 0
  },
  "description": "Weekly grocery shopping",
  "image": "https://res.cloudinary.com/.../transactions/receipt_456.jpg",
  "uid": "abc123xyz789",
  "walletId": "wallet_xyz123"
}
```

### Service Methods

#### Transaction Service

```typescript
// Create or update transaction
createOrUpdateTransaction(transactionData: Partial<TransactionType>): Promise<ResponseType>

// Delete transaction
deleteTransaction(transactionId: string, walletId: string): Promise<ResponseType>

// Fetch weekly statistics
fetchWeeklyStats(uid: string): Promise<ResponseType>

// Fetch monthly statistics
fetchMonthlyStats(uid: string): Promise<ResponseType>

// Fetch yearly statistics
fetchYearlyStats(uid: string): Promise<ResponseType>
```

#### Wallet Service

```typescript
// Create or update wallet
createOrUpdateWallet(walletData: Partial<WalletType>): Promise<ResponseType>

// Delete wallet (cascades to transactions)
deleteWallet(walletId: string): Promise<ResponseType>
```

#### Image Service

```typescript
// Upload image to Cloudinary
uploadFileToCloudinary(file: { uri?: string } | string, folderName: string): Promise<ResponseType>
```

---

## 🗄️ Database & Migrations

### Firestore Security Rules

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users collection: users can read/write their own data
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Wallets collection: users can read/write their own wallets
    match /wallets/{walletId} {
      allow read, write: if request.auth != null && 
        resource.data.uid == request.auth.uid;
      allow create: if request.auth != null && 
        request.resource.data.uid == request.auth.uid;
    }
    
    // Transactions collection: users can read/write their own transactions
    match /transactions/{transactionId} {
      allow read, write: if request.auth != null && 
        resource.data.uid == request.auth.uid;
      allow create: if request.auth != null && 
        request.resource.data.uid == request.auth.uid;
    }
  }
}
```

### Required Indexes

Create these composite indexes in Firestore for optimal query performance:

1. **Transactions by uid and date (descending)**
   - Collection: `transactions`
   - Fields: `uid` (Ascending), `date` (Descending)

2. **Transactions by uid and date (ascending) for date range queries**
   - Collection: `transactions`
   - Fields: `uid` (Ascending), `date` (Ascending)

3. **Wallets by uid and created (descending)**
   - Collection: `wallets`
   - Fields: `uid` (Ascending), `created` (Descending)

4. **Transactions by walletId (for cascade deletion)**
   - Collection: `transactions`
   - Fields: `walletId` (Ascending)

### Data Relationships

```
users (1) ──< (N) wallets
wallets (1) ──< (N) transactions
```

- One user can have many wallets
- One wallet can have many transactions
- Cascade deletion: Deleting a wallet deletes all associated transactions

---

## 🔒 Authentication & Authorization

### Authentication Flow

1. **Registration**:
   ```
   User Input → authContext.register()
   → Firebase Auth: createUserWithEmailAndPassword()
   → Firestore: Create user document in 'users' collection
   → Auth state changes → Redirect to main app
   ```

2. **Login**:
   ```
   User Input → authContext.login()
   → Firebase Auth: signInWithEmailAndPassword()
   → Auth state changes → Redirect to main app
   → Fetch user profile from Firestore
   ```

3. **Session Management**:
   - Firebase Auth persists sessions using AsyncStorage
   - `onAuthStateChanged` listener automatically handles auth state
   - Unauthenticated users redirected to welcome screen
   - Authenticated users redirected to main app

### Authorization

- **Firestore Rules**: Enforce user-level data access
- **Client-Side Validation**: All service methods validate `uid` matches authenticated user
- **Route Protection**: Expo Router redirects based on auth state

### Security Considerations

- Passwords are hashed by Firebase (never stored in plain text)
- API keys are safe to expose in client apps (security via Firestore rules)
- All data access validated server-side via Firestore security rules
- No sensitive data stored in AsyncStorage (only auth tokens)

---

## ⚠️ Error Handling & Logging

### Error Handling Strategy

1. **Service Layer**: All service methods return `ResponseType`:
   ```typescript
   {
     success: boolean;
     data?: any;
     msg?: string;
   }
   ```

2. **Component Layer**: Errors displayed via `Alert.alert()` or inline error messages

3. **Network Errors**: Handled gracefully with user-friendly messages

### Logging

- **Development**: Console logs for debugging
- **Production**: Consider integrating Sentry or similar error tracking service

### Common Error Scenarios

| Error | Handling |
|-------|----------|
| Network failure | Show retry option |
| Invalid credentials | Display error message |
| Insufficient wallet balance | Prevent transaction creation |
| Image upload failure | Show error, allow retry |
| Firestore permission denied | Redirect to login |

---

## 🧪 Testing

### Test Coverage

| Test Type | Status | Framework |
|-----------|--------|-----------|
| Unit Tests | Configured | Jest |
| Integration Tests | Not implemented | - |
| E2E Tests | Not implemented | - |

### Running Tests

```bash
# Run all tests
npm test

# Run tests in watch mode
npm test -- --watch

# Run tests with coverage
npm test -- --coverage
```

### Testing Strategy

**Current State**: Jest is configured but not actively used. Recommended testing approach:

1. **Unit Tests**: Test service functions (`transactionService`, `walletService`)
2. **Component Tests**: Test UI components with React Testing Library
3. **Integration Tests**: Test user flows (authentication, transaction creation)
4. **E2E Tests**: Consider Detox or Appium for end-to-end testing

### Example Test Structure

```typescript
// services/__tests__/transactionService.test.ts
import { createOrUpdateTransaction } from '../transactionService';

describe('TransactionService', () => {
  it('should create a new transaction', async () => {
    const result = await createOrUpdateTransaction({
      type: 'expense',
      amount: 100,
      walletId: 'wallet123',
      uid: 'user123'
    });
    expect(result.success).toBe(true);
  });
});
```

---

## 🔄 CI/CD Pipeline

### Current State

No CI/CD pipeline configured. Recommended setup:

### Recommended CI/CD with GitHub Actions

```yaml
# .github/workflows/ci.yml
name: CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm install
      - run: npm run lint
      - run: npm test
```

### Deployment Pipeline

1. **Development**: Manual deployment via Expo Go
2. **Staging**: EAS Build with staging environment
3. **Production**: EAS Build with production environment

### EAS Build Configuration

```json
{
  "build": {
    "development": {
      "developmentClient": true,
      "distribution": "internal"
    },
    "preview": {
      "distribution": "internal"
    },
    "production": {}
  }
}
```

---

## 🚢 Deployment

### Deployment Targets

| Platform | Method | Status |
|----------|--------|--------|
| iOS App Store | EAS Build + App Store Connect | Ready |
| Google Play Store | EAS Build + Play Console | Ready |
| Web | Expo Web | Limited functionality |

### iOS Deployment

```bash
# Build for iOS
eas build --platform ios --profile production

# Submit to App Store
eas submit --platform ios
```

### Android Deployment

```bash
# Build for Android
eas build --platform android --profile production

# Submit to Play Store
eas submit --platform android
```

### Environment-Specific Builds

1. Create environment-specific Firebase projects
2. Use `expo-constants` for environment variables
3. Configure different `app.json` settings per environment

---

## ⚡ Performance & Security

### Performance Optimizations

1. **Real-Time Listeners**: Efficient Firestore `onSnapshot` usage with proper cleanup
2. **Image Optimization**: Cloudinary automatic format and size optimization
3. **List Performance**: `@shopify/flash-list` for high-performance scrolling
4. **Code Splitting**: Expo Router automatic code splitting
5. **Lazy Loading**: Components loaded on-demand

### Security Measures

1. **Firestore Security Rules**: Server-side validation of all data access
2. **Input Validation**: Client-side validation before Firestore writes
3. **Authentication**: Firebase Auth handles secure credential storage
4. **API Keys**: Safe to expose (security via Firestore rules)
5. **Image Upload**: Cloudinary signed upload presets for production

### Best Practices

- ✅ Never commit Firebase config with production credentials
- ✅ Use environment variables for sensitive configuration
- ✅ Implement rate limiting for API calls (if using custom backend)
- ✅ Regular security audits of Firestore rules
- ✅ Keep dependencies updated for security patches

---

## 🗺️ Roadmap

### Completed ✅

- [x] User authentication and profile management
- [x] Multi-wallet support with balance tracking
- [x] Transaction CRUD operations
- [x] Real-time statistics with charts
- [x] AI financial assistant with predictions
- [x] Anomaly detection
- [x] Personalized financial coaching
- [x] Image upload for receipts
- [x] Search functionality

### In Progress 🚧

- [ ] Enhanced AI model training with more data
- [ ] Offline mode with sync
- [ ] Export transactions to PDF/CSV

### Planned 📋

- [ ] Recurring transactions
- [ ] Budget planning and alerts
- [ ] Multi-currency support
- [ ] Data backup and restore
- [ ] Dark/Light theme toggle
- [ ] Biometric authentication
- [ ] Transaction templates
- [ ] Advanced filtering and sorting
- [ ] Category-wise spending analysis
- [ ] Integration with banking APIs
- [ ] Family/shared wallet support

---

## 🤝 Contributing

We welcome contributions! Please follow these guidelines:

### Getting Started

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Make your changes
4. Commit with clear messages: `git commit -m 'Add amazing feature'`
5. Push to your branch: `git push origin feature/amazing-feature`
6. Open a Pull Request

### Development Guidelines

1. **Code Style**: Follow existing code patterns and ESLint rules
2. **TypeScript**: Maintain type safety, avoid `any` types
3. **Testing**: Add tests for new features
4. **Documentation**: Update README for significant changes
5. **Commits**: Use conventional commit messages

### Pull Request Process

1. Ensure all tests pass
2. Update documentation if needed
3. Request review from maintainers
4. Address review feedback
5. Maintainers will merge after approval

### Code of Conduct

- Be respectful and inclusive
- Welcome newcomers and help them learn
- Focus on constructive feedback
- Respect different viewpoints and experiences

---

## 📜 Code of Conduct

### Our Pledge

We pledge to make participation in our project a harassment-free experience for everyone, regardless of age, body size, disability, ethnicity, sex characteristics, gender identity and expression, level of experience, education, socio-economic status, nationality, personal appearance, race, religion, or sexual identity and orientation.

### Our Standards

**Examples of behavior that contributes to a positive environment:**

- Using welcoming and inclusive language
- Being respectful of differing viewpoints and experiences
- Gracefully accepting constructive criticism
- Focusing on what is best for the community
- Showing empathy towards other community members

**Examples of unacceptable behavior:**

- The use of sexualized language or imagery
- Trolling, insulting/derogatory comments, and personal or political attacks
- Public or private harassment
- Publishing others' private information without permission
- Other conduct which could reasonably be considered inappropriate

### Enforcement

Project maintainers are responsible for clarifying and enforcing our standards of acceptable behavior and will take appropriate action in response to any instances of unacceptable behavior.

---

## 📄 License

This project is **private** and proprietary. All rights reserved.

See `LICENSE.txt` for details.

---

## 🙏 Acknowledgements

### Technologies & Libraries

- [React Native](https://reactnative.dev/) - Mobile framework
- [Expo](https://expo.dev/) - Development platform
- [Firebase](https://firebase.google.com/) - Backend services
- [Cloudinary](https://cloudinary.com/) - Image management
- [TensorFlow Lite](https://www.tensorflow.org/lite) - On-device ML
- [Phosphor Icons](https://phosphoricons.com/) - Icon library

### Inspiration

- Modern fintech applications
- Privacy-first design principles
- On-device AI processing trends

### Contributors

Thank you to all contributors who have helped improve this project.

---

## 📞 Support

For issues, questions, or contributions:

- **GitHub Issues**: Open an issue for bugs or feature requests
- **Documentation**: Check this README and inline code comments
- **Community**: Join discussions in GitHub Discussions (if enabled)

---

**Built with ❤️ using React Native and Expo**

