🔍 What Is User Activity Detection?

User activity detection is the process of identifying what the user is currently doing based on sensor data. Common activities include:

🚶‍♂️ Walking

🏃 Running

🚴 Cycling

🚗 In a Vehicle (Driving)

🧍 Still (Not Moving)

📱 Tilting (e.g., picking up the phone)

❓ Unknown


These are useful for apps like fitness trackers, driving apps, or productivity tools that adjust behavior based on context.


---

🔧 How It Works

1. Sensors Involved

The Activity Recognition API mainly uses:

Accelerometer

Gyroscope

Magnetometer

Location (optionally)


2. Google's Machine Learning

Google's backend uses trained models to analyze sensor patterns and determine probable activities.


---

🧑‍💻 Implementation Overview

To use this in your Android app, you'll typically follow these steps:

✅ 1. Add Dependencies

Add Google Play Services in your build.gradle:

implementation 'com.google.android.gms:play-services-location:21.0.1'

✅ 2. Request Permissions

In your AndroidManifest.xml:

<uses-permission android:name="android.permission.ACTIVITY_RECOGNITION"/>

For Android 10+ (API 29+), you need to request this permission at runtime.

✅ 3. Use the Activity Recognition Client

Here’s a simple example:

ActivityRecognitionClient activityRecognitionClient = new ActivityRecognitionClient(context);

Intent intent = new Intent(context, YourBroadcastReceiver.class);
PendingIntent pendingIntent = PendingIntent.getBroadcast(context, 0, intent, PendingIntent.FLAG_UPDATE_CURRENT | PendingIntent.FLAG_IMMUTABLE);

// Request updates every 5 seconds
activityRecognitionClient.requestActivityUpdates(
        5000,  // detection interval in ms
        pendingIntent
);

✅ 4. Receive Activity Updates

Create a BroadcastReceiver:

public class YourBroadcastReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        if (ActivityRecognitionResult.hasResult(intent)) {
            ActivityRecognitionResult result = ActivityRecognitionResult.extractResult(intent);
            DetectedActivity mostProbableActivity = result.getMostProbableActivity();

            int activityType = mostProbableActivity.getType();
            int confidence = mostProbableActivity.getConfidence();

            switch (activityType) {
                case DetectedActivity.WALKING:
                    Log.d("Activity", "Walking - " + confidence + "%");
                    break;
                case DetectedActivity.IN_VEHICLE:
                    Log.d("Activity", "Driving - " + confidence + "%");
                    break;
                // Add other cases...
            }
        }
    }
}


---

✅ Best Practices

Always check confidence levels before acting on activity results.

Use JobIntentService or WorkManager if you're handling updates in background.

Minimize polling frequency to save battery.



---

📦 Alternative: Third-Party Tracking Apps

If you’re not developing your own app, various third-party tracking apps already use this technology. Examples include:

Google Fit

Strava

Moves (discontinued but was popular)

Fitness tracking features in Samsung Health, Fitbit, etc.



---

🔐 Privacy & Battery Considerations

Request only the permissions you need.

Let the user know why you’re accessing their activity.

Avoid high-frequency updates unless necessary.


