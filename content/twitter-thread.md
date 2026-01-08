# Day 5 Twitter Thread

## Tweet 1 (Hook)

Day 5/30: Deadline Dice

Shake your phone to get a random deadline. Turns "sometime this week" into "Wednesday at 2pm."

First mobile app of the challenge. Built in ~2 hours.

## Tweet 2 (The Problem)

The problem:

Vague deadlines don't get done. "I'll do it this week" means never.

But picking a specific time feels arbitrary. Why Wednesday? Why 2pm?

Solution: Let the dice decide. Now you have a deadline AND an excuse.

## Tweet 3 (The Solution)

What it does:

1. Enter your task
2. Pick a timeframe: Today, This Week, This Month
3. Shake your phone (or tap)
4. Get a specific deadline
5. Haptic buzz confirms the roll

The deadline is always during business hours (9-6) on a weekday. No 3am Sunday deadlines.

[screenshot of the app]

## Tweet 4 (The Technical Bits)

How shake detection works:

DeviceMotion API gives you accelerometer data. Sum up the acceleration in all directions. If it exceeds a threshold (~25), that's a shake.

```javascript
const total = Math.abs(x) + Math.abs(y) + Math.abs(z);
if (total > 25) rollDice();
```

iOS 13+ requires explicit permission on first use.

## Tweet 5 (The Stack)

The mobile stack:

- **Capacitor**: Wraps web app as native iOS
- **Haptics plugin**: Native vibration feedback
- **DeviceMotion API**: Shake detection
- **Single HTML file**: All UI/logic in one file

No React Native, no Flutter, no Swift. Just HTML/CSS/JS wrapped for iOS.

## Tweet 6 (The Iteration)

The bugs I hit (and fixed):

1. **Touch not working on iOS**: Needed `-webkit-user-select: none` and proper event listeners
2. **Shake not triggering**: iOS 13+ requires permission request on user interaction
3. **"Today" giving tomorrow**: Fixed date logic to respect midnight boundary

Mobile dev has quirks. Each platform has its gotchas.

## Tweet 7 (What I Learned)

Lesson from today:

**Capacitor is underrated** for simple mobile apps. If your app is mostly UI with a few native features, you don't need React Native or Flutter.

Write it as a web app. Add Capacitor. Ship to iOS. Done.

## Tweet 8 (CTA + Links)

Code: https://github.com/ilmych/day-05-deadline-dice

To run on your phone:
1. Clone the repo
2. `npm install && npx cap sync ios`
3. Open in Xcode, run on device

#30DaysOfVibeCode #BuildInPublic

---

# Short Version (if needed)

Day 5/30: Deadline Dice

Shake your phone to get a random deadline. Turns vague "this week" into specific "Wednesday 2pm."

- Capacitor iOS app
- DeviceMotion API for shake
- Haptic feedback
- ~2 hours to build

Code: https://github.com/ilmych/day-05-deadline-dice

#30DaysOfVibeCode
