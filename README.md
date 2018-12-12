# react-native-samsung-health

React Native bridge module for interacting with Samsung Health

## Installing it as a library in your main project

There are many ways to do this:

1. Do `npm install --save git+https://github.com/cellihealth/react-native-samsung-health.git` in your main project.
2. Link the library:

   - Add the following to `android/settings.gradle`:

     ```
     include ':react-native-samsung-health'
     project(':react-native-samsung-health').projectDir = new File(settingsDir, '../node_modules/react-native-samsung-health/android')
     ```

   - Add the following to `android/app/build.gradle`:

     ```xml
     ...

     dependencies {
         ...
         compile project(':react-native-samsung-health')
     }
     ```

   - Add the following to `android/app/src/main/java/**/MainApplication.java`:

     ```java
     package com.example;

     import com.reactnative.samsunghealth.SamsungHealthPackage;  // add this for react-native-samsung-health

     public class MainApplication extends Application implements ReactApplication {

         @Override
         protected List<ReactPackage> getPackages() {
             return Arrays.<ReactPackage>asList(
                 new MainReactPackage(),
                 new SamsungHealthPackage(BuildConfig.APPLICATION_ID)     // add this for react-native-samsung-health
             );
         }
     }
     ```

   - Add permission in `android/app/src/main/AndroidManifest.xml`:

     ```xml
     <application

     <meta-data
       android:name="com.samsung.android.health.permission.read"
       android:value="com.samsung.health.exercise;com.samsung.health.weight;com.samsung.health.step_count;com.samsung.shealth.height;com.samsung.shealth.step_daily_trend;" />
     ```

3. Simply `import/require` it by the name defined in your library's `package.json`:

   ```javascript
   import SamsungHealth from 'react-native-samsung-health';

   SamsungHealth.authorize([SamsungHealth.STEP_COUNT], (err, res) => {
     if (res) {
       let opt = {};
       SamsungHealth.getDailyStepCountSamples(opt, (err, res) => {
         if (err) console.log(err);
         if (res) console.log(res);
       });
     } else console.log(err);
   });
   ```

## Methods

### getExerciseSamples

```
SamsungHealth.getExerciseSamples({}, (err, res) => { console.log(res) });
```

```
[{
    calories: 119
    date: "2018-12-12"
    distance: 1000
    duration: 1800
    exerciseTypeCode: 11007
    exerciseTypeName: "Cycling"
    startTime: "2018-12-12T11:50:00.000Z"
},
{
    calories: 50
    date: "2018-12-12"
    distance: 500
    duration: 90
    exerciseTypeCode: 11007
    exerciseTypeName: "Cycling"
    startTime: "2018-12-12T10:50:00.000Z"
}]
```

### getWeightSamples

```
SamsungHealth.getWeightSamples({}, (err, res) => { console.log(res) });
```

```
[{
    time:1544610309815 // UTC timestamp
    weight: 65 // kilos
}]
```

### getStepsDailyTrend

Aggregated de-duplicated step counts from all possible sources. Use this unless you know you want duplicate data from all devices via `getDailyStepCountSamples`.

```
SamsungHealth.getStepsDailyTrend({}, (err, res) => { console.log(res) });
```

```
[{
    count:207 // steps
    day_time:1544572800000 // UTC timestamp of the start of the day
}]
```

## Modify/Build the Project in Android Studio

- Start `Android Studio` and select `File -> New -> Import Project` and select the **android** folder of this package.
- If you get a `Plugin with id 'android-library' not found` Error, install `android support repository`.
- If you get asked to upgrade _gradle_ to a new version, you can skip it.

## Access Data without S Health partner app certification

If you want to test your app on your device but you don't have the Samsung app certification you'll see an error that looks like

```
java.lang.SecurityException: Denied to access by whitelist
```

this error is because to work with S Health you app must be certified, to prevent this error for local development you can enable the developer mode on the Samsun Healt app, go to

> Settings -> About Samsung Health

and tap 10 times on the app version, the name of the version will then change to
`*(Developer Mode)*` and you'll be able to access S Health data
