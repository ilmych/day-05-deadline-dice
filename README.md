# Deadline Dice

> Shake your phone to turn vague deadlines into concrete plans.

Built as part of [30 Days of Vibe Code](../README.md).

## What it does

"I'll do it sometime this week" is a lie you tell yourself. Vague deadlines don't get done.

This app forces a decision. Enter your task, pick a timeframe, shake your phone—you get a specific deadline. Wednesday at 2pm. Thursday at 11am. Now it's real.

All deadlines are during business hours (9am-6pm) on weekdays. No 3am Sunday deadlines.

## Demo

![Deadline Dice Screenshot](content/screenshot.png)

## Try it

This is a native iOS app. To run it:

```bash
# Clone the repo
git clone https://github.com/ilmych/day-05-deadline-dice
cd day-05-deadline-dice

# Install dependencies
npm install

# Sync to iOS
npx cap sync ios

# Open in Xcode
npx cap open ios

# Run on device or simulator (Cmd+R in Xcode)
```

**Note**: Shake detection works best on a real device. The simulator doesn't have accelerometer input.

## How I built it

- **Time**: ~2 hours (including debugging iOS quirks)
- **Stack**: Capacitor, HTML/CSS/JS, iOS
- **AI assist**: Claude Code

### The Process

1. Discussed the concept—shake to roll dice for deadlines
2. Built the UI with dice animation and timeframe selector
3. Implemented shake detection via DeviceMotion API
4. Added haptic feedback using Capacitor Haptics plugin
5. Fixed iOS permission flow for motion access
6. Fixed touch event handling for iOS
7. Refined date logic to respect calendar boundaries
8. Generated app icon using Gemini AI

### The Debugging Journey

Building for iOS revealed several platform-specific issues:

**Touch not working**: iOS needs explicit CSS for touch handling:
```css
-webkit-user-select: none;
touch-action: manipulation;
```
Plus proper event listeners for both `click` and `touchend`.

**Shake not triggering**: iOS 13+ requires motion permission requested during user interaction. Had to request permission on first tap, not on page load.

**Date boundaries**: "Today" was sometimes giving tomorrow's date. Fixed by respecting midnight as the boundary, not a 24-hour window.

### Prompts Used

- "what's day 5" (Deadline Dice was on the roadmap)
- "yes let's build it! i love this one. can we do a mobile app where shaking your phone rolls the dice?"
- "tapping doesn't work" (led to iOS touch handling fix)
- "cool, the shake doesn't work though" (led to motion permission fix)
- "ok so i chose today, but it gives me deadlines that are tomorrow. we need a midnight cutoff instead of 24h"
- "week should also be until midnight sunday, and month should be until midnight last day of current month"

## What I learned

**Capacitor is great for simple mobile apps**. If your app is mostly UI with a few native features (haptics, motion), you don't need React Native or Flutter. Write a web app, wrap it with Capacitor, ship to iOS.

**iOS has quirks**:
- Motion API requires permission on user interaction (iOS 13+)
- Touch events need specific CSS and event handling
- Safari's behavior differs from Chrome

**Date math is tricky**. "This week" sounds simple until you consider: what day is today? What if it's Saturday? What about weekends? What timezone? Every edge case matters.

## Features

| Feature | Description |
|---------|-------------|
| Shake to roll | DeviceMotion API detects phone shake |
| Tap fallback | Tap the dice area if shake isn't available |
| Today | Random time today (business hours) |
| This Week | Random weekday through Sunday midnight |
| This Month | Random weekday through month end |
| Business hours | All deadlines are 9am-6pm |
| Weekdays only | No weekend deadlines |
| Haptic feedback | Vibration on roll |
| History | Recent rolls saved locally |

## Technical Details

### Shake Detection

```javascript
window.addEventListener('devicemotion', (e) => {
    const acc = e.accelerationIncludingGravity;
    const total = Math.abs(acc.x) + Math.abs(acc.y) + Math.abs(acc.z);
    if (total > 25) {
        rollDice();
    }
});
```

### iOS Motion Permission

```javascript
// Must be called during user interaction (tap, click)
if (typeof DeviceMotionEvent.requestPermission === 'function') {
    const permission = await DeviceMotionEvent.requestPermission();
    if (permission === 'granted') {
        enableMotion();
    }
}
```

## File Structure

```
day-05-deadline-dice/
├── www/
│   └── index.html      # Complete app (single file)
├── ios/                # Capacitor iOS project
├── capacitor.config.json
├── package.json
└── content/
    └── twitter-thread.md
```

## Requirements

- Node.js
- Xcode (for iOS)
- iOS device or simulator
- Capacitor CLI (`npm install -g @capacitor/cli`)
