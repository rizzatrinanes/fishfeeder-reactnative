# Fish Feeding Intuitive Smart Heightened System (FFISHS)

FFISHS is a NativeScript application built with React and TypeScript for
monitoring an ESP32-based fish feeder.

The current application provides a dashboard for aquarium sensor
readings and a separate screen for managing daily feeding schedules.

## Features

-   ESP32 connection status
-   Temperature monitoring
-   pH monitoring
-   Ammonia monitoring
-   Manual ESP32 connection attempt
-   Automatic sensor polling
-   Feeding schedule creation
-   Enable / disable feeding schedules
-   Remove feeding schedules
-   Next feeding time display
-   Local notification scheduling
-   Local schedule persistence
-   Dashboard and schedule navigation

## Technologies

-   NativeScript
-   React
-   TypeScript
-   React NativeScript
-   NativeScript Core
-   NativeScript Local Notifications
-   Tailwind CSS
-   Android

## ESP32 Communication

The application communicates with the ESP32 over HTTP.

The default ESP32 address in the current source is:

``` text
http://192.168.0.100
```

Sensor data is requested from:

``` text
GET /sensors
```

The application expects a JSON response containing:

``` json
{
  "temperature": 0,
  "ph": 0,
  "ammonia": 0
}
```

The ESP32 service checks the device every 10 seconds.

The ESP32 address can also be changed through the connection service.

## Dashboard

The dashboard displays three sensor values:

-   Temperature in °C
-   pH level
-   Ammonia in ppm

It also shows the current ESP32 connection state and the next scheduled
feeding time.

The current gauge configuration uses:

### Temperature

``` text
Range: 18–30 °C
Warning: 24 °C
Danger: 28 °C
```

### pH

``` text
Range: 6.0–9.0
Low warning: 6.5
Low danger: 6.0
Warning: 7.8
Danger: 8.5
```

### Ammonia

``` text
Range: 0–8 ppm
Warning: 0.25 ppm
Danger: 1.0 ppm
```

These values are UI thresholds used by the current application.

## Feeding Schedules

A feeding schedule contains:

``` text
hours
minutes
enabled
```

Schedules are stored using NativeScript `ApplicationSettings`.

The application can:

-   Add a feeding time
-   Enable or disable a schedule
-   Remove a schedule
-   Calculate the next enabled feeding time

## Notifications

The app uses `@nativescript/local-notifications` to create local
notifications for enabled feeding schedules.

The notification currently uses:

``` text
Title: Fish Feeder
Message: The fish has been fed.
```

Notification permission is requested when the notification service is
initialized.

## Time Synchronization

The schedule service attempts to obtain UTC time from:

``` text
worldtimeapi.org
```

If the request fails, it falls back to the device's local time.

## Project Structure

``` text
fishfeeder-reactnative-main/
├── src/
│   ├── components/
│   │   ├── AppTabs.tsx
│   │   ├── ESP32Connection.tsx
│   │   ├── FeedingScheduleItem.tsx
│   │   ├── GaugeChart.tsx
│   │   ├── MainStack.tsx
│   │   ├── NextFeedingSchedule.tsx
│   │   └── TimePicker.tsx
│   ├── models/
│   │   └── FeedingSchedule.ts
│   ├── screens/
│   │   ├── Dashboard.tsx
│   │   └── Schedule.tsx
│   ├── services/
│   │   ├── ESP32Service.ts
│   │   ├── NotificationService.ts
│   │   ├── ScheduleService.ts
│   │   └── TimeService.ts
│   └── utils/
│       └── getNextFeedingTime.ts
├── package.json
├── nativescript.config.ts
├── tailwind.config.js
└── tsconfig.json
```

## Requirements

-   Node.js
-   npm
-   NativeScript CLI / project tooling
-   Android development environment for Android builds
-   An ESP32 running a compatible HTTP `/sensors` endpoint for real
    sensor data

## Installation

Clone the repository:

``` bash
git clone https://github.com/yourusername/fishfeeder-reactnative.git
cd fishfeeder-reactnative
```

Install dependencies:

``` bash
npm install
```

The current project includes an Android build target through
NativeScript.

## Development

The package currently includes a TypeScript type-check script:

``` bash
npm run type-check
```

Android build configuration is defined in `project.json`.

## Current Scope

This repository contains the application side of the fish feeder
project.

The current source communicates with an ESP32 for sensor readings, but
the repository does not contain ESP32 firmware for the `/sensors`
endpoint or a direct feeding/servo command implementation.

The feeding schedules in this version primarily manage local schedule
data and local notifications.

## Project Status

Prototype / Academic Project
