# Staff Check-in Portal - Frontend Handoff

## Overview
This repository contains the static frontend assets (HTML/CSS) for the Staff Check-in Portal. The UI is fully responsive, mobile-first, and includes an automatic Dark Mode based on the user's system preferences.

The core functionality requires staff to authenticate, capture a live selfie, and log their GPS coordinates for their daily check-in.

## File Structure
* `index.html` - The login page.
* `signup.html` - The account creation page.
* `checkin.html` - The main dashboard for capturing the selfie and location.
* `styles.css` - Global stylesheet (includes CSS variables and `@media (prefers-color-scheme: dark)`).

---

## Pages & Backend Requirements

### 1. Sign Up (`signup.html`)
Standard registration form. The `<form>` action is currently set to `#`.
**Fields to capture:**
* `full_name` (Text)
* `email` (Email)
* `password` (Password)
* `confirm_password` (Password)

### 2. Sign In (`index.html`)
Standard login form. The `<form>` action is currently set to `#`.
**Fields to capture:**
* `email` (Email)
* `password` (Password)

### 3. Daily Check-in (`checkin.html`)
This is the core application view. It requires JavaScript to handle device hardware (Camera & GPS) and API calls to check the user's daily status.

**UI Elements to hook into:**
* **`.date-display`**: Needs to be dynamically populated with today's date.
* **`.status-badge`**: Toggle classes between `.status-pending` (yellow) and `.status-success` (green) based on whether the backend confirms they have checked in today.
* **`.camera-container`**: Currently holds a placeholder icon (`📸`). Needs to be replaced with a `<video>` element streaming from `navigator.mediaDevices.getUserMedia()`.
* **`.location-badge`**: Needs to be updated with coordinates via `navigator.geolocation.getCurrentPosition()`.
* **`.btn` (Capture Button)**: Needs an event listener to draw the video frame to a `<canvas>`, convert to a base64 image or Blob, and POST to the check-in endpoint along with the lat/long coordinates.

---

## Required API Endpoints (Suggested)

To make this frontend functional, the following backend endpoints will likely need to be created:

1. **`POST /api/auth/register`** - Handles `signup.html` submission.
2. **`POST /api/auth/login`** - Handles `index.html` submission and sets session/JWT.
3. **`GET /api/checkin/status`** - Returns whether the logged-in user has already checked in today (to update the UI badge upon loading `checkin.html`).
4. **`POST /api/checkin`** - Accepts the multipart/form-data or JSON payload containing:
   * User ID / Auth Token
   * Image file (Selfie)
   * Latitude
   * Longitude
   * Timestamp

## Styling Notes
* **Dark Mode**: Handled entirely via CSS variables in `styles.css`. Please avoid hardcoding colors in inline styles or JS so the theme toggling doesn't break.
* **Forms**: All forms use standard HTML5 validation (`required`, `type="email"`).
