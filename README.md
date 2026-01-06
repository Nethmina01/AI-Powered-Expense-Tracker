# Expense Tracker App

A modern React Native expense tracking application built with Expo, Firebase, and TypeScript. This app helps users manage their finances by tracking income, expenses, wallets, and providing detailed statistics.

## 📱 Features

- **User Authentication**: Secure login and registration with Firebase Authentication
- **Wallet Management**: Create and manage multiple wallets with balance tracking
- **Transaction Tracking**: Add, edit, and delete income/expense transactions
- **Real-time Updates**: Live data synchronization using Firestore real-time listeners
- **Statistics Dashboard**: View weekly, monthly, and yearly financial statistics with charts
- **Image Upload**: Attach images to transactions and wallets using Cloudinary
- **Search Functionality**: Search through transactions
- **Profile Management**: Update user profile with name and image

## 🏗️ Project Structure

```
expense-tracker/
├── app/                          # Expo Router file-based routing
│   ├── (auth)/                   # Authentication screens
│   │   ├── welcome.tsx           # Welcome/landing screen
│   │   ├── login.tsx             # Login screen
│   │   └── register.tsx          # Registration screen
│   ├── (tabs)/                   # Main app tabs (bottom navigation)
│   │   ├── _layout.tsx           # Tab navigation layout
│   │   ├── index.tsx             # Home screen (transactions list)
│   │   ├── statistics.tsx        # Statistics/charts screen
│   │   ├── wallet.tsx            # Wallet management screen
│   │   └── profile.tsx            # User profile screen
│   ├── (modals)/                 # Modal screens
│   │   ├── transactionModal.tsx  # Add/edit transaction modal
│   │   ├── walletModal.tsx       # Add/edit wallet modal
│   │   ├── categoryModal.tsx     # Category selection modal
│   │   ├── profileModal.tsx      # Profile edit modal
│   │   └── searchModal.tsx       # Search transactions modal
│   ├── _layout.tsx               # Root layout with AuthProvider
│   └── index.tsx                 # Entry point
├── components/                    # Reusable UI components
│   ├── BackButton.tsx            # Navigation back button
│   ├── Button.tsx                # Custom button component
│   ├── CustomTabs.tsx            # Custom tab bar component
│   ├── Header.tsx                 # Screen header component
│   ├── HomeCard.tsx               # Home screen summary card
│   ├── ImageUpload.tsx            # Image picker/upload component
│   ├── Input.tsx                  # Custom input field
│   ├── Loading.tsx                # Loading spinner component
│   ├── ModalWrapper.tsx           # Modal container wrapper
│   ├── ScreenWrapper.tsx          # Screen container wrapper
│   ├── TransactionList.tsx        # Transaction list component
│   ├── Typo.tsx                   # Typography component
│   └── WalletListItem.tsx         # Wallet list item component
├── config/                        # Configuration files
│   └── firebase.ts                # Firebase initialization
├── constants/                     # App constants
│   ├── data.ts                    # Static data (categories, etc.)
│   ├── index.ts                   # Constants exports
│   └── theme.ts                   # Theme colors, spacing, radius
├── contexts/                      # React Context providers
│   └── authContext.tsx            # Authentication context
├── hooks/                         # Custom React hooks
│   └── useFetchData.ts            # Firestore real-time data fetching hook
├── services/                      # Business logic services
│   ├── imageService.ts            # Cloudinary image upload service
│   ├── transactionService.ts      # Transaction CRUD operations
│   ├── userService.ts             # User profile management
│   └── walletService.ts           # Wallet CRUD operations
├── types.ts                       # TypeScript type definitions
├── utils/                         # Utility functions
│   ├── common.ts                  # Common helper functions
│   └── styling.ts                 # Styling utilities (scale, verticalScale)
└── assets/                        # Static assets
    ├── fonts/                     # Custom fonts
    └── images/                    # Image assets
```

## 🔄 Data Flow

### Authentication Flow

1. **App Initialization** (`app/_layout.tsx`)
   - Wraps app with `AuthProvider` context
   - Sets up navigation stack with modals

2. **Auth Context** (`contexts/authContext.tsx`)
   - Manages user authentication state
   - Listens to Firebase auth state changes via `onAuthStateChanged`
   - Automatically redirects:
     - Authenticated users → `/(tabs)` (main app)
     - Unauthenticated users → `/(auth)/welcome`
   - Provides methods:
     - `login(email, password)`: Sign in with Firebase Auth
     - `register(email, password, name)`: Create account and user document
     - `updateUserData(uid)`: Fetch and sync user data from Firestore

3. **User Data Storage**
   - Firebase Authentication: User credentials (email, password)
   - Firestore Collection `users`: User profile data (name, image, email, uid)

### Transaction Flow

1. **Creating/Updating Transactions** (`services/transactionService.ts`)
   ```
   User Input → transactionModal.tsx
   ↓
   createOrUpdateTransaction()
   ↓
   [If new transaction]
   → updateWalletForNewTransaction() → Updates wallet balance & totals
   ↓
   [If updating transaction]
   → revertAndUpdateWallets() → Reverts old transaction, applies new one
   ↓
   [If image provided]
   → uploadFileToCloudinary() → Uploads to Cloudinary
   ↓
   Firestore: Create/Update transaction document
   ```

2. **Transaction Data Structure**
   - Collection: `transactions`
   - Fields: `id`, `type` (income/expense), `amount`, `category`, `date`, `description`, `image`, `uid`, `walletId`

3. **Real-time Transaction List** (`app/(tabs)/index.tsx`)
   - Uses `useFetchData` hook with Firestore `onSnapshot`
   - Automatically updates when transactions change
   - Filters by `uid` and orders by `date` descending
   - Limits to 30 most recent transactions

4. **Wallet Balance Updates**
   - When transaction is created:
     - Income: `wallet.amount += amount`, `wallet.totalIncome += amount`
     - Expense: `wallet.amount -= amount`, `wallet.totalExpenses += amount`
   - When transaction is updated:
     - Reverts old transaction's effect on wallet
     - Applies new transaction's effect
   - When transaction is deleted:
     - Reverts transaction's effect on wallet balance

### Wallet Flow

1. **Wallet Management** (`services/walletService.ts`)
   ```
   User Input → walletModal.tsx
   ↓
   createOrUpdateWallet()
   ↓
   [If image provided]
   → uploadFileToCloudinary() → Uploads to Cloudinary
   ↓
   Firestore: Create/Update wallet document
   ```

2. **Wallet Data Structure**
   - Collection: `wallets`
   - Fields: `id`, `name`, `amount`, `totalIncome`, `totalExpenses`, `image`, `uid`, `created`

3. **Real-time Wallet List** (`app/(tabs)/wallet.tsx`)
   - Uses `useFetchData` hook
   - Filters by `uid` and orders by `created` descending
   - Calculates total balance across all wallets

4. **Wallet Deletion**
   - Deletes wallet document
   - Cascades deletion to all associated transactions

### Statistics Flow

1. **Statistics Data Fetching** (`services/transactionService.ts`)
   - `fetchWeeklyStats(uid)`: Last 7 days data
   - `fetchMonthlyStats(uid)`: Last 12 months data
   - `fetchYearlyStats(uid)`: All years data

2. **Statistics Screen** (`app/(tabs)/statistics.tsx`)
   - User selects time period (Weekly/Monthly/Yearly)
   - Fetches transactions for selected period
   - Aggregates income/expense by time period
   - Displays bar chart using `react-native-gifted-charts`
   - Shows transaction list for selected period

### Image Upload Flow

1. **Image Service** (`services/imageService.ts`)
   ```
   User selects image → ImageUpload component
   ↓
   uploadFileToCloudinary(file, folderName)
   ↓
   Uploads to Cloudinary via REST API
   ↓
   Returns secure URL
   ↓
   URL stored in Firestore document
   ```

2. **Image Storage**
   - Cloudinary: Image hosting
   - Folders: `transactions`, `wallets`, `users`
   - Firestore: Stores Cloudinary URLs as strings

### Real-time Data Synchronization

1. **useFetchData Hook** (`hooks/useFetchData.ts`)
   - Uses Firestore `onSnapshot` for real-time updates
   - Automatically re-renders components when data changes
   - Manages loading and error states
   - Used by:
     - Transaction lists
     - Wallet lists
     - Any component needing real-time Firestore data

2. **Data Flow Pattern**
   ```
   Component mounts
   ↓
   useFetchData(collectionName, constraints)
   ↓
   Firestore onSnapshot listener
   ↓
   Data changes in Firestore
   ↓
   onSnapshot callback fires
   ↓
   Component state updates
   ↓
   Component re-renders with new data
   ```

## 🛠️ Technology Stack

- **Framework**: React Native with Expo (~52.0.36)
- **Routing**: Expo Router (file-based routing)
- **Language**: TypeScript
- **Backend**: Firebase
  - Authentication: Firebase Auth
  - Database: Cloud Firestore
  - Storage: Cloudinary (for images)
- **State Management**: React Context API
- **UI Libraries**:
  - `react-native-gifted-charts`: Charts and graphs
  - `phosphor-react-native`: Icons
  - `react-native-element-dropdown`: Dropdown components
- **Navigation**: Expo Router with custom tab bar
- **Styling**: React Native StyleSheet with custom theme system

## 📦 Installation

1. Install dependencies:
   ```bash
   npm install
   ```

2. Configure Firebase:
   - Create a Firebase project at [Firebase Console](https://console.firebase.google.com/)
   - Copy your Firebase config to `config/firebase.ts`
   - Enable Authentication (Email/Password)
   - Create Firestore database
   - Set up Firestore security rules

3. Configure Cloudinary:
   - Create a Cloudinary account
   - Get your cloud name and upload preset
   - Add to `constants/index.ts`:
     ```typescript
     export const CLOUDINARY_CLOUD_NAME = "your-cloud-name";
     export const CLOUDINARY_UPLOAD_PRESET = "your-upload-preset";
     ```

4. Start the app:
   ```bash
   npx expo start
   ```

## 🗄️ Database Schema

### Firestore Collections

#### `users`
```typescript
{
  uid: string;
  email: string;
  name: string;
  image?: string; // Cloudinary URL
}
```

#### `wallets`
```typescript
{
  id: string;
  name: string;
  amount: number;
  totalIncome: number;
  totalExpenses: number;
  image: string; // Cloudinary URL
  uid: string;
  created: Date;
}
```

#### `transactions`
```typescript
{
  id: string;
  type: "income" | "expense";
  amount: number;
  category?: string;
  date: Timestamp;
  description?: string;
  image?: string; // Cloudinary URL
  uid: string;
  walletId: string;
}
```

## 🔐 Security Considerations

- Firebase Authentication handles user authentication securely
- Firestore security rules should be configured to:
  - Allow users to read/write only their own data
  - Validate data structure
  - Prevent unauthorized access
- Cloudinary upload preset should be unsigned or have appropriate restrictions

## 📝 Key Features Implementation

### Transaction Validation
- Prevents expenses exceeding wallet balance
- Validates transaction data before creation
- Handles wallet balance updates atomically

### Real-time Updates
- All lists update automatically when data changes
- No manual refresh needed
- Optimistic UI updates

### Image Handling
- Supports local images and Cloudinary URLs
- Automatic upload on transaction/wallet creation
- Fallback to default images when needed

## 🚀 Development

- **Start development server**: `npm start`
- **Run on Android**: `npm run android`
- **Run on iOS**: `npm run ios`
- **Run on Web**: `npm run web`
- **Lint**: `npm run lint`

## 📄 License

See LICENSE.txt for details.
