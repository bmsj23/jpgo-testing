# QA Guide: JeepneyGo

> **For QA Testers**

---

## Table of Contents

- [What You're Testing](#what-youre-testing)
- [What You Need](#what-you-need)
- [Step 1: Install Expo Go](#step-1-install-expo-go)
- [Step 2: Connect to the App](#step-2-connect-to-the-app)
- [Step 3: Create Test Accounts](#step-3-create-test-accounts)
- [Step 4: Testing the Commuter App](#step-4-testing-the-commuter-app)
- [Step 5: Testing the Driver App](#step-5-testing-the-driver-app)
- [Step 6: Testing Both Apps Together (Real-Time Sync)](#step-6-testing-both-apps-together-real-time-sync)
- [Known Limitations (Expo Go)](#known-limitations-expo-go)
- [Troubleshooting](#troubleshooting)

---

## What You're Testing

JeepneyGo has **two mobile apps**:

| App | Who It's For | What It Does |
|-----|-------------|--------------|
| **JeepneyGo** (Commuter) |  Passengers | Find jeepneys, plan trips, check fares, save favorite stops |
| **JeepneyGo Driver** | Drivers | Broadcast live location, manage trips, track passengers and earnings |

Both apps connect to the same backend. When a driver starts a trip, commuters can see that jeepney moving on their map in real-time.

---

## What You Need

| Item | Details |
|------|---------|
| **iPhone** | iOS 16 or later |
| **Expo Go app** | yes |
| **WiFi connection** | or data |

---

## Step 1: Install Expo Go

1. Open the **App Store** on your iPhone
2. Search for **"Expo Go"**
3. Install the app by **Expo** (it's free)
4. Open Expo Go once to confirm it installed correctly

> Direct link: [Expo Go on App Store](https://apps.apple.com/app/expo-go/id982107779)

---

## Step 2: Connect to the App

The developer running the project will provide you a **QR code**. Here's how to connect:

### If You're on a Different Network (Remote Testing)

The developer will start the app in **tunnel mode** and share the QR code with you:

1. The developer sends you a screenshot of the QR code (via chat, email, etc.)
2. Open **Expo Go** on your iPhone
3. Tap **"Scan QR Code"** inside Expo Go
4. Scan the QR code from the screenshot
5. The app loads on your phone

> **First load** may take long (due to Codespaces). Subsequent loads should be faster.

---

## Step 3: Create Test Accounts

### Commuter Account

1. Open the **commuter app** via Expo Go
2. Tap **"Register"** on the login screen
3. Enter your **display name**
4. Enter your **email address**
5. Enter and confirm your **password**
6. Tap **Register**
7. You're logged in as a commuter!

### Driver Account

Driver accounts require **admin approval** before they can start trips:

1. Open the **driver app** via Expo Go
2. Tap **"Register"** on the login screen
3. Enter your **display name**, **email address**, and **password**
4. Tap **Register**
5. You'll see a **"Pending Approval"** screen. This is expected
6. **Tell the developer** the email you used so they can approve you
7. The developer will run an approval script on the backend
8. Once approved, sasabihin sa inyo then **close and reopen** the app. You can now access the driver dashboard


### Logging In Again

If you've already registered, just:
1. Enter your email address on the login screen
2. Enter your password
3. Tap **"Sign In"**
4. You're back in!

---

## Step 4: Testing the Commuter App

### App Navigation

The commuter app has **4 tabs** at the bottom:

| Tab | What It Does |
|-----|-------------|
| **Map** | Live map with jeepney markers, search, and trip planning |
| **Routes** | Browse all available routes and their details |
| **Favorites** | Your saved stops for quick access |
| **Profile** | Account settings, notifications, support |

### Key Things to Test

#### Map Screen
- [ ] Map loads without crashing
- [ ] Your current location appears (allow location permission when asked)
- [ ] Route chips at the top let you filter by route
- [ ] Bottom sheet shows nearby jeepneys (if any are active)
- [ ] Tapping a jeepney marker opens its details

#### Search & Trip Planning
- [ ] Tap the search bar and type a destination (e.g., "SM Lipa")
- [ ] Results appear and keyboard dismisses when you select one
- [ ] A destination pin appears on the map
- [ ] "Routes Serving This Destination" shows matching routes
- [ ] Trip planner (From → To) finds direct routes with fare estimates

#### Route Details
- [ ] Open any route from the Routes tab
- [ ] Stops are listed in order with landmarks
- [ ] "View Map" shows the full route polyline on a map
- [ ] Fare estimator calculates fare between two stops
- [ ] Fare matrix shows regular and discounted prices

#### Favorites
- [ ] Save a stop from a route detail screen
- [ ] It appears in your Favorites tab
- [ ] Opening a saved stop shows live jeepney data
- [ ] You can remove a saved stop

#### Profile & Notifications
- [ ] Edit profile (change display name, upload avatar)
- [ ] Notification settings work (toggle preferences)
- [ ] Notification inbox shows received notifications

---

## Step 5: Testing the Driver App

### App Navigation

The driver app has **4 sections** accessible from the home screen and bottom tabs:

| Section | What It Does |
|---------|-------------|
| **Home** | Dashboard with today's earnings and stats |
| **Drive** | Start/manage trips with live map |
| **History** | Past trip records and earnings |
| **Profile** | Account, vehicle info, notifications |

### Key Things to Test

#### Home Screen
- [ ] Dashboard loads with today's stats
- [ ] "Start Driving" button is visible
- [ ] Vehicle card shows assigned vehicle details
- [ ] Pull-to-refresh updates stats

#### Starting a Trip
1. Tap **"Start Driving"** or go to the **Drive** tab
2. Select your **assigned route** from the picker
3. Tap **"Start Trip"**
4. Allow **location permission** when prompted
5. Your location should appear on the map with the route polyline

#### During a Trip
- [ ] Map shows your current location moving (walk around to test)
- [ ] Passenger count controls work (board/alight buttons)
- [ ] Boarding events are recorded
- [ ] Spacing alert appears if another driver is nearby on the same route

#### Ending a Trip
1. Tap **"End Trip"**
2. Trip summary should appear (duration, passengers, distance)
3. Enter fare/earnings if prompted
4. Trip appears in **History**

#### Trip History & Earnings
- [ ] History tab shows completed trips
- [ ] Tap a trip to see details (route, passengers, timing, GPS path)
- [ ] Earnings screen shows aggregated data
- [ ] "Add Missing Earnings" modal works

#### Profile & Vehicle
- [ ] Edit profile works (name, avatar)
- [ ] Vehicle info screen shows plate number and capacity
- [ ] Notification settings and inbox work

---

## Step 6: Testing Both Apps Together (Real-Time Sync)

### Setup
- **Phone A**: Running the **Driver App** (logged in as approved driver)
- **Phone B**: Running the **Commuter App** (logged in as commuter)

### Sample Test Steps

1. **Driver (Phone A):** Start a trip on a route (e.g., BC-LIPA)
2. **Commuter (Phone B):** Open the Map tab
3. **Commuter:** You should see a jeepney marker appear on the map for the driver's route
4. **Driver:** Walk around (this simulates driving). Your location updates every few seconds
5. **Commuter:** The jeepney marker should move on the map, reflecting the driver's movement
6. **Commuter:** Tap the jeepney marker. You should see trip details (route, passenger count, ETA)
7. **Driver:** Tap "Board" to add a passenger
8. **Commuter:** Passenger count should update on the jeepney card
9. **Driver:** End the trip
10. **Commuter:** The jeepney marker should disappear from the map

### What to Check
- Jeepney appears on the commuter map within a few seconds of the driver starting a trip
- Location updates are reflected on the commuter map (may have 3–10 second delay)
- Passenger count changes propagate to the commuter view
- Ending a trip removes the jeepney from the commuter map

---

## Known Limitations (Expo Go)

Since we're testing via Expo Go (not a production build), there are a few limitations:

| Limitation | Impact | Workaround |
|-----------|--------|-----------|
| **Background location** | If the driver minimizes the app, location updates pause | Keep the driver app in the foreground while testing trips |
| **Push notification delay** | Push notifications may arrive with a slight delay | Use the notification inbox to verify notifications were received |
| **Notification tapping** | Tapping a notification may not navigate to a specific screen | Open the app manually and check the notification inbox |
| **First load time** | Initial app load takes 30–60 seconds | Subsequent loads are faster; be patient on first launch |
| **JavaScript bundle** | App reloads if Expo Go is backgrounded for too long | Just re-scan the QR code if this happens |

---

## Troubleshooting

### App Won't Load / Blank Screen
1. Restart the app
2. Close Expo Go completely (swipe up from app switcher)
3. Re-scan the QR code
4. If it still fails, ask the developer to check terminal logs

### "Something went wrong" Error
1. Shake your phone to open the Expo developer menu
2. Tap **"Reload"**
3. If the error persists, note the error message and report it

### Can't Sign In
1. Double-check the email address you used during registration
2. Make sure your password is correct
3. If this is a driver account, confirm the account has already been approved
4. If needed, you can just create new accounts

### Can't See Jeepneys on Commuter Map
- Make sure a driver has an **active trip** running on the driver app
- Check that you're looking at the **correct route area** on the map (Batangas/Lipa)
- Try pulling down to refresh or switching route chip filters
