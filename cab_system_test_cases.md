# CAB SYSTEM - COMPREHENSIVE TEST CASE SUITE

This document contains the complete test case repository for the **CAB SYSTEM** project (`minhquan123170/23667011_TranMinhQuan_CABSYSTEM`).

Every test scenario is systematically covered across 5 critical testing dimensions:
- **Positive** (Valid inputs & success flows)
- **Negative** (Invalid inputs & exception handling)
- **Boundary** (Edge/limit values)
- **Empty / Null** (Missing required fields)
- **Format / Security** (Invalid data types, special characters, and code injections)

---

## Table of Contents

1. [Module 1: Authentication & User Access](#module-1-authentication--user-access)
2. [Module 2: Booking Management](#module-2-booking-management)
3. [Module 3: Driver & Dispatch Management](#module-3-driver--dispatch-management)
4. [Module 4: Payment & Wallet System](#module-4-payment--wallet-system)
5. [Module 5: Rating & Review System](#module-5-rating--review-system)
6. [Module 6: Promotions & Discount System](#module-6-promotions--discount-system)
7. [Module 7: Admin Panel & Configuration](#module-7-admin-panel--configuration)

---

## Module 1: Authentication & User Access

### Scenario: User Login & Session Management

| Test Case ID | Category | Test Case Description | Preconditions | Test Steps | Test Data | Expected Result | Priority |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-AUTH-001** | Positive | Login successfully with valid Phone Number and OTP | User registered | 1. Enter valid Phone Number<br>2. Click 'Send OTP'<br>3. Enter valid OTP<br>4. Submit | Phone: `0901234567`<br>OTP: `123456` | Login success, session token generated, redirected to Home screen. | High |
| **TC-AUTH-002** | Positive | Login successfully via Biometric Authentication | Biometrics enabled | 1. Open app<br>2. Scan fingerprint / FaceID | Biometric: `Valid` | Login success immediately without password entry. | High |
| **TC-AUTH-003** | Positive | Session restore on app re-open | Active session | 1. Open app with valid session token | Token: `Valid` | User remains logged in without prompting credentials. | Medium |
| **TC-AUTH-004** | Positive | Logout and re-login flow | Logged in | 1. Tap Logout<br>2. Enter credentials<br>3. Submit | Phone: `0901234567` | Session invalidated on logout; new valid session created on login. | Medium |
| **TC-AUTH-005** | Negative | Login failure with incorrect OTP | OTP requested | 1. Enter valid Phone<br>2. Enter incorrect OTP | OTP: `000000` | Error: "Invalid OTP code". Login denied. | High |
| **TC-AUTH-006** | Negative | Login failure with unregistered Phone Number | Not registered | 1. Enter unregistered Phone<br>2. Click Send OTP | Phone: `0999999999` | Error: "Phone number not registered on the system". | High |
| **TC-AUTH-007** | Negative | Temporary account lock after 5 consecutive failed attempts | User registered | 1. Enter incorrect OTP 5 times consecutively | Attempts: `5` | Account temporarily locked for 15 minutes with countdown timer. | High |
| **TC-AUTH-008** | Negative | Login attempt on suspended/locked account | Account suspended | 1. Enter credentials for locked account | Status: `Locked` | Error: "Your account is locked. Please contact support". | High |
| **TC-AUTH-009** | Negative | Login attempt with expired OTP (> 60 seconds) | OTP sent | 1. Wait 61 seconds after receiving OTP<br>2. Enter OTP | Time elapsed: `61s` | Error: "OTP code has expired. Please request a new code". | Medium |
| **TC-AUTH-010** | Negative | Network connection loss during login request | Offline | 1. Turn off Wifi/Data<br>2. Click Submit | Network: `Disconnected` | Error: "Network connection lost. Please check your internet". | Medium |
| **TC-AUTH-011** | Boundary | Phone number with minimum valid length (9 digits) | Valid format | 1. Enter 9-digit phone number<br>2. Request OTP | Phone: `912345678` | Accepted and OTP sent successfully. | Medium |
| **TC-AUTH-012** | Boundary | Phone number shorter than minimum length (8 digits) | Valid format | 1. Enter 8-digit phone number<br>2. Request OTP | Phone: `91234567` | Error: "Phone number must be between 9 and 11 digits". | Medium |
| **TC-AUTH-013** | Boundary | Phone number with maximum valid length (11 digits) | Valid format | 1. Enter 11-digit phone number<br>2. Request OTP | Phone: `01234567890` | Accepted and OTP sent successfully. | Medium |
| **TC-AUTH-014** | Boundary | Phone number exceeding maximum length (12 digits) | Valid format | 1. Enter 12-digit phone number | Phone: `012345678901` | Input restricted at 11th digit or validation error shown. | Medium |
| **TC-AUTH-015** | Empty/Null | Submit login with empty Phone Number | Blank field | 1. Leave Phone empty<br>2. Click Submit | Phone: `""` | Error: "Phone number is required". | High |
| **TC-AUTH-016** | Empty/Null | Submit login with empty OTP field | Blank field | 1. Enter Phone<br>2. Leave OTP empty<br>3. Submit | OTP: `""` | Error: "OTP code is required". | High |
| **TC-AUTH-017** | Empty/Null | Submit phone number consisting only of whitespaces | Blank spaces | 1. Enter "   " in Phone field<br>2. Submit | Phone: `"   "` | Input trimmed, validation error: "Phone number is required". | Medium |
| **TC-AUTH-018** | Format | Enter letters and special characters in Phone Number | Invalid characters | 1. Enter `abc@#$123` in Phone field | Phone: `abc@#$123` | Keyboard blocks non-numeric input or error: "Numeric characters only". | Low |
| **TC-AUTH-019** | Format | XSS Script injection in Phone field | Security test | 1. Enter `<script>alert(1)</script>` | Phone: `<script>...` | Input sanitized safely, request blocked gracefully. | High |
| **TC-AUTH-020** | Format | SQL Injection attempt in OTP field | Security test | 1. Enter `' OR '1'='1` in OTP field | OTP: `' OR '1'='1` | Parameterized query prevents SQLi, error: "Invalid OTP". | High |

---

## Module 2: Booking Management

### Scenario: Immediate Ride Booking

| Test Case ID | Category | Test Case Description | Preconditions | Test Steps | Test Data | Expected Result | Priority |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-BOOK-001** | Positive | Book immediate ride with valid pickup and dropoff | User logged in | 1. Select Pickup<br>2. Select Dropoff<br>3. Choose Car type<br>4. Tap 'Book Now' | Pickup: `284 Cong Quynh`<br>Dropoff: `Ben Thanh Market` | Booking created (`PENDING`), system starts searching for nearby drivers. | High |
| **TC-BOOK-002** | Positive | Book ride with payment via Linked Wallet | Wallet funded | 1. Set routes<br>2. Select Wallet payment<br>3. Tap 'Book Now' | Payment: `Wallet`<br>Fare: `50,000 VND` | Booking confirmed, fare authorized from wallet balance. | High |
| **TC-BOOK-003** | Positive | Re-booking a ride from History | Ride completed | 1. Go to History<br>2. Select past ride<br>3. Tap 'Book Again' | Ride ID: `R1001` | Pickup and Dropoff pre-filled automatically on booking screen. | Medium |
| **TC-BOOK-004** | Positive | Real-time fare calculation update on route change | Booking screen | 1. Change dropoff location | New Dropoff: `Landmark 81` | Distance and estimated fare re-calculated instantly. | Medium |
| **TC-BOOK-005** | Negative | Booking failed when pickup and dropoff locations are identical | Location screen | 1. Set Pickup: Location A<br>2. Set Dropoff: Location A | Location: `12 Nguyen Hue` | Error: "Pickup and destination points cannot be identical". | High |
| **TC-BOOK-006** | Negative | Booking failed when no drivers are available nearby | System high demand | 1. Set routes<br>2. Tap 'Book Now' | Radius: `No driver` | Notification: "No drivers available in your area. Please try again later". | High |
| **TC-BOOK-007** | Negative | Booking failed due to outstanding unpaid debt | Account has debt | 1. Set routes<br>2. Tap 'Book Now' | Debt: `120,000 VND` | Error: "Please pay your previous unpaid ride before booking a new one". | High |
| **TC-BOOK-008** | Negative | Booking blocked during active ongoing ride | Active ride | 1. Attempt to book a second ride simultaneously | Active Ride: `In-progress` | Error: "You currently have an active trip in progress". | High |
| **TC-BOOK-009** | Negative | Rapid double-clicking on 'Book Now' button | Slow connection | 1. Tap 'Book Now' twice rapidly | Action: `Double-click` | Button disabled immediately after first tap; only 1 booking request sent. | Medium |
| **TC-BOOK-010** | Negative | Booking timeout when driver matching fails after 3 minutes | Searching state | 1. Wait 180 seconds in searching screen | Timer: `180s` | Status changed to `EXPIRED`, prompt user to retry or change vehicle type. | Medium |
| **TC-BOOK-011** | Boundary | Minimum distance booking (Biên dưới: < 100m) | Location screen | 1. Pick 2 points 50m apart<br>2. Tap 'Book Now' | Distance: `~50m` | Base fare (Minimum fare) applied; booking proceeds normally. | Medium |
| **TC-BOOK-012** | Boundary | Distance just below minimum threshold (0 meters) | Location screen | 1. Pick exactly same GPS point | Distance: `0m` | System prompts user to select a valid destination. | Medium |
| **TC-BOOK-013** | Boundary | Maximum distance booking (Biên trên: 300 km) | Location screen | 1. Select destination 300 km away | Distance: `300km` | Booking allowed, long-distance surcharge applied. | Medium |
| **TC-BOOK-014** | Boundary | Exceeding maximum allowed distance (> 300 km) | Location screen | 1. Select destination 301 km away | Distance: `301km` | Error: "Trip distance exceeds maximum limit of 300 km". | Medium |
| **TC-BOOK-015** | Empty/Null | Submit booking without selecting Dropoff location | Location screen | 1. Set Pickup<br>2. Leave Dropoff blank<br>3. Tap Book | Dropoff: `""` | Error: "Please enter a destination". | High |
| **TC-BOOK-016** | Empty/Null | Submit booking without Pickup location | GPS disabled | 1. Leave Pickup blank<br>2. Set Dropoff<br>3. Tap Book | Pickup: `""` | Error: "Please specify your pickup location". | High |
| **TC-BOOK-017** | Empty/Null | Submit booking with blank spaces in address search | Search bar | 1. Enter "   " in address search | Search: `"   "` | Search results remain empty; no crash or invalid state. | Low |
| **TC-BOOK-018** | Format | Enter special characters in address note to driver | Note field | 1. Enter `@#$%^&*()` in note | Note: `@#$%^&*()` | Characters sanitized or displayed as plain text safely. | Low |
| **TC-BOOK-019** | Format | HTML script injection in Pickup note field | Security test | 1. Enter `<b>Test</b>` in note field | Note: `<b>Test</b>` | Rendered as literal text `<b>Test</b>`, no HTML execution. | Medium |
| **TC-BOOK-020** | Format | SQL Injection payload in destination search field | Security test | 1. Enter `1' UNION SELECT NULL--` | Search: `1' UNION...` | Query sanitized safely; returns "No location found". | High |

---

## Module 3: Driver & Dispatch Management

### Scenario: Driver Online/Offline Status & Dispatching

| Test Case ID | Category | Test Case Description | Preconditions | Test Steps | Test Data | Expected Result | Priority |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-DRV-001** | Positive | Driver switches status to Online successfully | Driver app open | 1. Toggle 'Go Online' switch | GPS: `ON`<br>Status: `OFFLINE` | Driver status becomes `ONLINE`, location updates sent to server every 5s. | High |
| **TC-DRV-002** | Positive | Driver accepts incoming trip dispatch within time limit | Driver Online | 1. Incoming trip pops up<br>2. Tap 'Accept' within 10s | Action time: `4s` | Trip accepted (`ACCEPTED`), app navigates to passenger pickup route. | High |
| **TC-DRV-003** | Positive | Driver switches status to Offline when idle | Driver Online | 1. Toggle 'Go Offline' switch | Status: `ONLINE` | Status becomes `OFFLINE`, driver stops receiving trip dispatches. | High |
| **TC-DRV-004** | Positive | Driver declines dispatch request manually | Dispatch popup | 1. Incoming trip pops up<br>2. Tap 'Decline' | Action: `Decline` | Trip removed from driver screen; dispatch rerouted to next nearest driver. | Medium |
| **TC-DRV-005** | Negative | Driver cannot go Online with GPS location disabled | Device GPS off | 1. Turn off GPS<br>2. Toggle 'Go Online' | GPS: `OFF` | Error: "Please enable GPS location services to go online". | High |
| **TC-DRV-006** | Negative | Driver cannot go Online with expired driver license | License expired | 1. Attempt to toggle Online | License: `Expired` | Error: "Your driver license has expired. Please update document". | High |
| **TC-DRV-007** | Negative | Accept trip failed if passenger canceled trip beforehand | Passenger canceled | 1. Incoming trip pops up<br>2. Passenger cancels<br>3. Driver taps Accept | State: `CANCELED` | Notification: "Passenger has canceled this booking". Return to idle map. | High |
| **TC-DRV-008** | Negative | Accept trip failed when internet connection drops during tap | Connection drops | 1. Tap 'Accept'<br>2. Disconnect network | Network: `Lost` | Error: "Connection failed. Trip reassigned". Driver status set to reconnecting. | Medium |
| **TC-DRV-009** | Negative | Driver attempts to go Offline during an active trip | Trip in progress | 1. Trip status: `IN_PROGRESS`<br>2. Toggle Offline | Active trip: `Yes` | Error: "Cannot go offline while a trip is in progress". | High |
| **TC-DRV-010** | Negative | Auto-decline when dispatch request response times out | Dispatch screen | 1. Incoming trip pops up<br>2. Ignore for 15 seconds | Action time: `16s` | Request expires, trip status `MISSED`, assigned to another driver. | Medium |
| **TC-DRV-011** | Boundary | Response at exact boundary limit (15.0 seconds) | Dispatch screen | 1. Tap 'Accept' at exactly 15.0s mark | Action time: `15.0s` | System accepts request successfully. | Medium |
| **TC-DRV-012** | Boundary | Response just past limit (15.1 seconds) | Dispatch screen | 1. Tap 'Accept' at 15.1s mark | Action time: `15.1s` | System treats request as expired / missed. | Medium |
| **TC-DRV-013** | Boundary | Dispatch distance at maximum search radius (5.0 km) | Searching driver | 1. Driver is exactly 5.0 km from pickup | Radius: `5.0km` | Driver receives dispatch notification. | Medium |
| **TC-DRV-014** | Boundary | Dispatch distance outside search radius (5.1 km) | Searching driver | 1. Driver is 5.1 km away | Radius: `5.1km` | Driver does not receive this dispatch notification. | Medium |
| **TC-DRV-015** | Empty/Null | Reject trip without selecting mandatory cancellation reason | Cancel trip | 1. Driver cancels accepted trip<br>2. Leave reason blank | Reason: `""` | Error: "Please select a reason for canceling the trip". | High |
| **TC-DRV-016** | Empty/Null | Send chat message to passenger with empty text | In-app chat | 1. Leave text box blank<br>2. Tap Send | Message: `""` | Send button disabled or no action taken. | Low |
| **TC-DRV-017** | Empty/Null | Send chat message consisting of spaces only | In-app chat | 1. Type "   "<br>2. Tap Send | Message: `"   "` | Text trimmed, send action prevented. | Low |
| **TC-DRV-018** | Format | Upload vehicle document with invalid file format (.exe) | Profile edit | 1. Upload `license.exe` as vehicle registration | File: `license.exe` | Error: "Invalid file format. Please upload JPG, PNG, or PDF". | High |
| **TC-DRV-019** | Format | Upload vehicle image exceeding max size limit (> 10MB) | Profile edit | 1. Upload 12MB image | Size: `12MB` | Error: "File size exceeds 10MB maximum limit". | Medium |
| **TC-DRV-020** | Format | XSS payload in Driver cancellation note field | Cancel trip | 1. Enter `<img src=x onerror=alert(1)>` in note | Note: `<img...>` | Input sanitized, displayed safely as text. | High |

---

## Module 4: Payment & Wallet System

### Scenario: Wallet Top-Up & Trip Payments

| Test Case ID | Category | Test Case Description | Preconditions | Test Steps | Test Data | Expected Result | Priority |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-PAY-001** | Positive | Wallet top-up via linked Banking app successfully | Wallet open | 1. Enter amount<br>2. Select Bank<br>3. Confirm OTP | Amount: `100,000 VND` | Balance updated immediately (+100,000 VND), transaction history logged. | High |
| **TC-PAY-002** | Positive | Automatic payment deduction for completed trip | Ride completed | 1. Driver completes trip | Fare: `45,000 VND`<br>Wallet: `100,000 VND` | Wallet deducted 45,000 VND (New balance: 55,000 VND), receipt issued. | High |
| **TC-PAY-003** | Positive | Payment via Cash method chosen by passenger | Ride ended | 1. Passenger selects Cash<br>2. Pay driver cash | Fare: `50,000 VND` | Driver marks "Cash Received", trip completed (`COMPLETED`). | High |
| **TC-PAY-004** | Positive | Wallet withdrawal to linked bank account by driver | Driver Wallet | 1. Enter amount<br>2. Enter PIN<br>3. Submit | Amount: `200,000 VND` | Withdrawal request processed, wallet deducted instantly. | High |
| **TC-PAY-005** | Negative | Payment failure due to insufficient wallet balance | Low balance | 1. Complete ride using Wallet payment | Fare: `50,000 VND`<br>Wallet: `10,000 VND` | Payment fails, status set to `UNPAID_DEBT`, prompt passenger to pay cash. | High |
| **TC-PAY-006** | Negative | Top-up failure due to bank account authorization rejection | Bank account | 1. Top-up request sent | Bank status: `REJECTED` | Error: "Bank transaction rejected. Please check your bank account". | High |
| **TC-PAY-007** | Negative | Driver withdrawal failure due to incorrect PIN 3 times | Driver Wallet | 1. Enter incorrect PIN 3 times | PIN: `0000` | Wallet withdrawal feature locked for 24 hours. | High |
| **TC-PAY-008** | Negative | Double charge prevention on system network retry | Network glitch | 1. Submit payment request twice due to lag | Request: `Repeated` | Idempotency key prevents duplicate transaction; single charge applied. | High |
| **TC-PAY-009** | Negative | Payment with expired credit card | Saved card | 1. Select credit card payment | Expiry: `01/22` | Error: "Selected card is expired. Please select another method". | Medium |
| **TC-PAY-010** | Negative | Refund request for trip older than policy limit (> 7 days) | History | 1. Request refund for trip completed 10 days ago | Trip date: `-10 days` | Error: "Refund request period (7 days) has expired for this trip". | Medium |
| **TC-PAY-011** | Boundary | Minimum allowed top-up amount (Biên dưới: 10,000 VND) | Top-up screen | 1. Enter 10,000 VND<br>2. Submit | Amount: `10,000 VND` | Transaction processed successfully. | Medium |
| **TC-PAY-012** | Boundary | Top-up amount below minimum limit (9,999 VND) | Top-up screen | 1. Enter 9,999 VND<br>2. Submit | Amount: `9,999 VND` | Error: "Minimum top-up amount is 10,000 VND". | Medium |
| **TC-PAY-013** | Boundary | Maximum allowed single top-up amount (5,000,000 VND) | Top-up screen | 1. Enter 5,000,000 VND<br>2. Submit | Amount: `5,000,000 VND` | Transaction processed successfully. | Medium |
| **TC-PAY-014** | Boundary | Top-up amount exceeding single transaction limit (5,000,001 VND) | Top-up screen | 1. Enter 5,000,001 VND<br>2. Submit | Amount: `5,000,001 VND` | Error: "Maximum single transaction limit is 5,000,000 VND". | Medium |
| **TC-PAY-015** | Empty/Null | Submit top-up with empty amount field | Top-up screen | 1. Leave amount blank<br>2. Tap Continue | Amount: `""` | Error: "Please enter top-up amount". | High |
| **TC-PAY-016** | Empty/Null | Driver withdrawal with empty Wallet PIN | Withdrawal | 1. Enter amount<br>2. Leave PIN blank | PIN: `""` | Error: "Please enter your wallet PIN". | High |
| **TC-PAY-017** | Empty/Null | Submit top-up containing whitespaces only | Top-up screen | 1. Enter "   "<br>2. Tap Continue | Amount: `"   "` | Validation error: "Please enter a valid amount". | Medium |
| **TC-PAY-018** | Format | Enter negative numbers in top-up field | Top-up screen | 1. Enter `-50000` | Amount: `-50000` | Input blocked or error: "Amount must be a positive number". | High |
| **TC-PAY-019** | Format | Enter non-numeric text in top-up amount field | Top-up screen | 1. Enter `FiftyThousand` | Amount: `FiftyThousand` | Keyboard restricts input to digits only. | Medium |
| **TC-PAY-020** | Format | SQL Injection in transaction search bar | Transaction history | 1. Enter `' OR 1=1 --` | Search: `' OR 1=1 --` | Search executed safely without exposing database records. | High |

---

## Module 5: Rating & Review System

### Scenario: Trip Rating & Feedback Submission

| Test Case ID | Category | Test Case Description | Preconditions | Test Steps | Test Data | Expected Result | Priority |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-RAT-001** | Positive | Submit 5-star rating with constructive comment | Ride completed | 1. Select 5 Stars<br>2. Enter comment<br>3. Tap Submit | Rating: `5 Stars`<br>Comment: `Great service!` | Rating saved, driver average rating recalculated. | High |
| **TC-RAT-002** | Positive | Driver rates passenger after ride completion | Ride completed | 1. Select 5 Stars<br>2. Tap Submit | Rating: `5 Stars` | Passenger score updated successfully. | Medium |
| **TC-RAT-003** | Positive | Select preset feedback tags (e.g., "Clean Car", "Polite") | Rating screen | 1. Select 5 Stars<br>2. Tap tags: Clean Car<br>3. Submit | Tags: `["Clean Car"]` | Feedback tags saved to driver stats. | Medium |
| **TC-RAT-004** | Positive | Low rating triggers automatic feedback category selector | Rating screen | 1. Select 1 Star | Rating: `1 Star` | Complaint options automatically appear (e.g., Unsafe driving, Late pickup). | High |
| **TC-RAT-005** | Negative | Submit rating for a canceled trip | Trip canceled | 1. Attempt to open rating modal for canceled trip | Trip status: `CANCELED` | Rating modal not accessible for canceled trips. | Medium |
| **TC-RAT-006** | Negative | Resubmit rating for an already rated trip | Trip rated | 1. Re-open completed trip rating dialog | Trip status: `RATED` | Screen displays submitted rating in read-only mode. | Medium |
| **TC-RAT-007** | Negative | Submit rating when trip rating grace period (> 7 days) expired | History | 1. Attempt rating 8 days post-trip | Time: `+8 days` | Notification: "The rating period for this trip has ended". | Low |
| **TC-RAT-008** | Negative | Automatic driver-passenger matching block on 1-star rating | Ride rated | 1. Passenger rates driver 1 star | Rating: `1 Star` | System flags pair; future automatic dispatches between them blocked. | High |
| **TC-RAT-009** | Negative | Rating update fails during internet drop | Rating screen | 1. Rate trip<br>2. Cut internet<br>3. Tap Submit | Network: `Offline` | Error: "Failed to submit. Please check connection and retry". | Medium |
| **TC-RAT-010** | Negative | Duplicate rating submission via rapid tapping | Rating screen | 1. Tap Submit rapidly 3 times | Action: `Multi-tap` | Only 1 rating entry recorded in system database. | Medium |
| **TC-RAT-011** | Boundary | Comment length at maximum allowed limit (500 characters) | Rating screen | 1. Enter exactly 500 characters<br>2. Submit | Length: `500 chars` | Comment saved completely. | Medium |
| **TC-RAT-012** | Boundary | Comment length exceeding maximum limit (501 characters) | Rating screen | 1. Enter 501 characters | Length: `501 chars` | Input truncated at 500th char or error: "Max 500 characters allowed". | Medium |
| **TC-RAT-013** | Boundary | Star rating minimum boundary (1 Star) | Rating screen | 1. Select 1 Star<br>2. Submit | Rating: `1 Star` | System records score of 1.0. | High |
| **TC-RAT-014** | Boundary | Star rating maximum boundary (5 Stars) | Rating screen | 1. Select 5 Stars<br>2. Submit | Rating: `5 Stars` | System records score of 5.0. | High |
| **TC-RAT-015** | Empty/Null | Submit rating with Star selected but empty comment | Rating screen | 1. Select 4 Stars<br>2. Leave comment empty<br>3. Submit | Comment: `""` | Rating saved successfully (comment optional). | Medium |
| **TC-RAT-016** | Empty/Null | Submit feedback without selecting any Star rating | Rating screen | 1. Do not tap any star<br>2. Tap Submit | Stars: `0` | Error: "Please select a star rating". | High |
| **TC-RAT-017** | Empty/Null | Comment containing whitespaces only | Rating screen | 1. Select 5 Stars<br>2. Enter "   " in comment | Comment: `"   "` | Rating saved; comment trimmed to empty string. | Low |
| **TC-RAT-018** | Format | Comment containing emojis and non-Latin characters | Rating screen | 1. Enter `Taxi rất tốt 🚕⭐👍` | Comment: `Emojis` | Emojis stored and rendered correctly in database/UI. | Low |
| **TC-RAT-019** | Format | XSS Script injection in review comment field | Security test | 1. Enter `<script>document.cookie=""</script>` | Comment: `<script>` | HTML/JS sanitized; rendered safely as text. | High |
| **TC-RAT-020** | Format | SQL Injection attempt in feedback category parameter | Security test | 1. Inject `' OR 1=1--` into category selection | Category: `SQLi` | Backend handles parameterized input safely. | High |

---

## Module 6: Promotions & Discount System

### Scenario: Promo Code Application & Validation

| Test Case ID | Category | Test Case Description | Preconditions | Test Steps | Test Data | Expected Result | Priority |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-PROMO-001** | Positive | Apply valid active promo code successfully | Valid promo code | 1. Enter code `HE2026`<br>2. Tap Apply | Code: `HE2026`<br>Fare: `100,000 VND` | Discount applied (-20%), total fare updated to 80,000 VND. | High |
| **TC-PROMO-002** | Positive | Auto-suggestion of best available promo code | Codes available | 1. Open promo drawer | Available: `2 codes` | System highlights code offering highest discount. | Medium |
| **TC-PROMO-003** | Positive | Remove applied promo code before booking | Code applied | 1. Tap 'Remove Promo' button | Action: `Remove` | Fare reverts to original amount before discount. | Medium |
| **TC-PROMO-004** | Positive | Promo code usage limit decremented after completed ride | Ride completed | 1. Finish ride with promo | Code: `HE2026` | Code usage counter increased by 1 for user account. | High |
| **TC-PROMO-005** | Negative | Apply expired promo code | Code expired | 1. Enter `SUMMER2025`<br>2. Tap Apply | Code: `SUMMER2025` | Error: "Promo code has expired". | High |
| **TC-PROMO-006** | Negative | Apply promo code whose total usage limit has been reached | Usage maxed | 1. Enter `LIMITED100`<br>2. Tap Apply | Code: `LIMITED100` | Error: "This promo code has reached its total usage limit". | High |
| **TC-PROMO-007** | Negative | Apply promo code restricted to new users on old account | Old user | 1. Enter `NEWUSER`<br>2. Tap Apply | Account: `10 rides` | Error: "This promo code is valid for new users only". | High |
| **TC-PROMO-008** | Negative | Apply non-existent promo code | Code invalid | 1. Enter `INVALID999`<br>2. Tap Apply | Code: `INVALID999` | Error: "Promo code does not exist". | High |
| **TC-PROMO-009** | Negative | Apply promo code restricted to specific vehicle type | Bike promo on Car | 1. Select Car<br>2. Enter Bike code `BIKE20` | Vehicle: `Car` | Error: "Promo code not applicable for this vehicle type". | Medium |
| **TC-PROMO-010** | Negative | Re-use single-use promo code on same account | Code used | 1. Enter previously used single-use code | Code: `ONETIME` | Error: "You have already used this promo code". | High |
| **TC-PROMO-011** | Boundary | Trip fare exactly equals minimum required fare (50,000 VND) | Minimum fare code | 1. Fare = 50,000 VND<br>2. Apply code `MIN50` | Fare: `50,000 VND` | Code applied successfully. | Medium |
| **TC-PROMO-012** | Boundary | Trip fare just below minimum required fare (49,999 VND) | Minimum fare code | 1. Fare = 49,999 VND<br>2. Apply code `MIN50` | Fare: `49,999 VND` | Error: "Minimum order value of 50,000 VND required to use this code". | Medium |
| **TC-PROMO-013** | Boundary | Discount capped at maximum limit (Cap: 30,000 VND) | Max cap code | 1. 50% discount on 100,000 VND fare | Fare: `100,000 VND` | Discount capped at 30,000 VND (New fare: 70,000 VND). | Medium |
| **TC-PROMO-014** | Boundary | Fare equal to exact discount amount (Fare = 20k, Discount = 20k) | Fixed discount | 1. Fare = 20,000 VND<br>2. Apply 20k code | Fare: `20,000 VND` | Final fare becomes 0 VND (or minimum base charge per system rule). | Medium |
| **TC-PROMO-015** | Empty/Null | Tap Apply with empty promo code field | Booking screen | 1. Leave code field blank<br>2. Tap Apply | Code: `""` | Error: "Please enter a promo code". | High |
| **TC-PROMO-016** | Empty/Null | Enter spaces only in promo code input | Booking screen | 1. Enter "   "<br>2. Tap Apply | Code: `"   "` | Validation error: "Please enter a promo code". | Medium |
| **TC-PROMO-017** | Empty/Null | Select promo code when user account has no saved vouchers | Voucher wallet | 1. Open Voucher list | Wallet: `Empty` | Displays "No promo codes available". | Low |
| **TC-PROMO-018** | Format | Promo code entered in lowercase characters (`he2026`) | Lowercase input | 1. Enter `he2026`<br>2. Tap Apply | Code: `he2026` | Auto-converted to uppercase `HE2026` and applied successfully. | Medium |
| **TC-PROMO-019** | Format | Promo code input containing special characters | Special chars | 1. Enter `HE@2026#` | Code: `HE@2026#` | Error: "Promo code contains invalid characters". | Low |
| **TC-PROMO-020** | Format | SQL Injection in promo code input field | Security test | 1. Enter `' OR '1'='1` | Code: `' OR '1'='1` | Sanitized safely, returns "Promo code does not exist". | High |

---

## Module 7: Admin Panel & Configuration

### Scenario: User Management & System Pricing Configuration

| Test Case ID | Category | Test Case Description | Preconditions | Test Steps | Test Data | Expected Result | Priority |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **TC-ADM-001** | Positive | Admin locks user account due to violation | Admin logged in | 1. Search User ID<br>2. Set Status: `Locked`<br>3. Save | User: `USR1001`<br>Status: `Locked` | Account status updated to `Locked`; active sessions forcefully terminated. | High |
| **TC-ADM-002** | Positive | Admin approves new driver registration application | Driver pending | 1. Review docs<br>2. Click 'Approve Application' | Driver: `DRV2002` | Driver status changed to `APPROVED`; notification sent to driver. | High |
| **TC-ADM-003** | Positive | Admin updates Base Fare for 4-seater cars | Price settings | 1. Update Base Fare to 15,000 VND<br>2. Save | Fare: `15,000 VND` | Updated pricing configuration broadcasted to calculation engine. | High |
| **TC-ADM-004** | Positive | Export system trip reports to Excel format | Admin Dashboard | 1. Select date range<br>2. Click 'Export XLSX' | Range: `01/01 - 31/01` | Excel report file generated and downloaded successfully. | Medium |
| **TC-ADM-005** | Negative | Admin attempts to lock another Admin account without super-admin rights | Sub-admin role | 1. Select Admin user<br>2. Tap Lock | Target: `Admin User` | Error: "Insufficient privileges to perform this operation". | High |
| **TC-ADM-006** | Negative | System rejects negative pricing input for per-km rate | Price settings | 1. Set Rate/km = -10,000 VND<br>2. Save | Rate: `-10,000 VND` | Error: "Price per km must be a positive number". | High |
| **TC-ADM-007** | Negative | Approve driver with unverified/missing documents | Pending driver | 1. Open incomplete profile<br>2. Click Approve | Docs: `Missing` | System blocks approval: "Cannot approve driver with unverified documents". | High |
| **TC-ADM-008** | Negative | Concurrent edit conflict when two admins update price simultaneously | Pricing page | 1. Admin A and B edit pricing<br>2. Both tap Save | Conflict: `Simultaneous` | Optimistic locking handles conflict; second admin prompted to refresh. | Medium |
| **TC-ADM-009** | Negative | Session timeout for inactive Admin panel session | Idle screen | 1. Inactive for 30 minutes | Time: `30 mins` | Admin auto-logged out; redirected to Login screen. | High |
| **TC-ADM-010** | Negative | Reject driver application without specifying rejection reason | Review page | 1. Click 'Reject'<br>2. Leave reason blank | Reason: `""` | Error: "Please specify reason for rejecting driver application". | Medium |
| **TC-ADM-011** | Boundary | Set surge pricing multiplier at minimum limit (1.0x) | Peak pricing | 1. Set Surge = 1.0x<br>2. Save | Surge: `1.0x` | Normal rates applied without surge markup. | Medium |
| **TC-ADM-012** | Boundary | Set surge pricing multiplier below minimum (0.9x) | Peak pricing | 1. Set Surge = 0.9x<br>2. Save | Surge: `0.9x` | Error: "Surge multiplier cannot be less than 1.0x". | Medium |
| **TC-ADM-013** | Boundary | Set surge pricing multiplier at maximum limit (5.0x) | Peak pricing | 1. Set Surge = 5.0x<br>2. Save | Surge: `5.0x` | Surge cap set to 5.0x successfully. | Medium |
| **TC-ADM-014** | Boundary | Set surge pricing multiplier exceeding limit (5.1x) | Peak pricing | 1. Set Surge = 5.1x<br>2. Save | Surge: `5.1x` | Error: "Surge multiplier cannot exceed maximum cap of 5.0x". | Medium |
| **TC-ADM-015** | Empty/Null | Save pricing form with empty Base Fare field | Price settings | 1. Clear Base Fare<br>2. Tap Save | Base Fare: `""` | Error: "Base Fare field cannot be left blank". | High |
| **TC-ADM-016** | Empty/Null | Search users with empty query string | User management | 1. Leave search bar blank<br>2. Tap Search | Query: `""` | Complete default paginated user list returned. | Low |
| **TC-ADM-017** | Empty/Null | Submit lock account form without entering lock reason | Lock modal | 1. Select Lock<br>2. Leave reason empty | Reason: `""` | Error: "Please enter a reason for locking this account". | Medium |
| **TC-ADM-018** | Format | Enter textual input into Base Fare field | Price settings | 1. Enter "TenThousand" in fare | Base Fare: `Text` | Validation error: "Field must contain numeric values only". | Medium |
| **TC-ADM-019** | Format | XSS Script injection in System Announcement title | Broadcast message | 1. Enter `<script>alert('admin')</script>` | Title: `<script>` | Title sanitized, rendered harmlessly as plain text. | High |
| **TC-ADM-020** | Format | SQL Injection in Admin User Search bar | User management | 1. Enter `' OR '1'='1` | Search: `' OR '1'='1` | SQL query parameterized safely; returns no invalid records. | High |