![vampire_xs](https://github.com/vicktor/vampire/assets/382114/90813c29-7c22-4fa4-80b2-1f667fb9d7d9)

# vampire | Delighted to suck your glucoses
###### _Give me back my **fucking data**_

## How it works

The application works like a **vampire**, **sucking glucose** data from the permanent notifications that apps have to show the patient their data. 

It will only work with apps that have this notification and update it regularly with the glucose value.

So, what can it do?

Read glucose from notifications and **share** it with **anyone** through internal WebServer, BroadCast to another apps, Health Connect and ContentProvider.
Optionally you can write all information in Google [Health Connect](https://developer.android.com/guide/health-and-fitness/health-connect).

Based on the great [xDrip+](https://github.com/NightscoutFoundation/xDrip) project without which, **vampire** would not exist.

## Screenshots

![vampire](https://github.com/vicktor/vampire/assets/382114/89195d2d-d139-4fdb-bf2f-853af0eb1479)
![health](https://github.com/vicktor/vampire/assets/382114/d45ef88a-7a39-4852-9783-e4594ee4e766)
![glucosa](https://github.com/vicktor/vampire/assets/382114/6ae4663f-42fa-449b-814f-31e19d158a74)

## Configuration

App needs special permission to read your notifications, you can inspect the code for your safety and peace of mind.

Go to Settings -> Accessibility -> Notifications and allow **vampire** to access your notifications, allow all
Go to Settings -> Batery ->  Select **vampire**, Battery Saving and select **No Restrictions**


## CGM supported

From xDrip+
```
    coOptedPackages.add("com.dexcom.g6")
    coOptedPackages.add("com.dexcom.g6.region1.mmol")
    coOptedPackages.add("com.dexcom.g6.region3.mgdl")
    coOptedPackages.add("com.dexcom.dexcomone")
    coOptedPackages.add("com.dexcom.g7")
    coOptedPackages.add("com.camdiab.fx_alert.mmoll")
    coOptedPackages.add("com.camdiab.fx_alert.mgdl")
    coOptedPackages.add("com.camdiab.fx_alert.hx.mmoll")
    coOptedPackages.add("com.camdiab.fx_alert.hx.mgdl")
    coOptedPackages.add("com.medtronic.diabetes.guardian")
    coOptedPackages.add("com.medtronic.diabetes.minimedmobile.eu")
    coOptedPackages.add("com.medtronic.diabetes.minimedmobile.us")
```

## Reading via Intent BroadCast

Declare something similar in your AndroidManifest

```
<receiver android:name=".receivers.VampireReceiver"
    android:enabled="true"
    android:exported="true">
    <intent-filter>
        <action android:name="xyz.bauber.vampire.SEND_GLUCOSE" />
    </intent-filter>
</receiver>
```

Create your BroadCast Receiver

```
class VampireReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context, intent: Intent) {
        val bundle = intent.extras
        val units = bundle?.getString("units")
        val glucose = bundle?.getFloat("glucose")
        val time = bundle?.getLong("timestamp")
        val trend = bundle?.getString("trend")
        val source = bundle?.getString("source")
    }
}
```

Remember to write destination PackageName


## Reading data Health Connect

**Currently under development** but works, only a few things need to be finished about permissions 

Take a look at [Health Connect API](https://developer.android.com/guide/health-and-fitness/health-connect)

[Health Connect SDK](https://developer.android.com/guide/health-and-fitness/health-connect/sdk-changes)

[Health Connect Toolbox](https://developer.android.com/guide/health-and-fitness/health-connect-guidelines/test-guidance/health-connect-toolbox)


## Reading data with ContentProvider

**Removed**

## Reading data with WebServer

Surely, this is the easiest way to access the data, all the data read is stored in a table and made available to any app through a web server with only 2 endpoints to retrieve the information.

Open http://localhost:5566/glucoses from your app to read last 24 hours of glucoses

Open http://localhost:5566/glucose from your app to read last glucose value

All data are returned in JSON:

```json
{ 
  "glucoseType":"interstitial",
  "glucoseUnits":"mgdl",
  "glucoseValue":140.0,
  "origin":"Dexcom",
  "timestamp":1685461844641,
  "timestampOffset":120,
  "trend":"FLAT"
}
```

## ToDo
Code needs to be improved and unused parts removed

~~It would be very interesting if someone can test with FreeStyleLibre, right now I don't have any sensor to test, the package name would be com.freestylelibre3.app, I add it to VampireCollector code~~

## Bugs
Sometimes the service stops and does not continue to collect data. 
If anyone knows how to fix this, open an issue or make a pull request.

## Example of integration
[SocialDiabetes](https://socialdiabetes.com) App working with **Vampire** in real-time via Intent BroadCast

![SocialDiabetes](https://github.com/vicktor/vampire/assets/382114/a861c341-470f-4e59-b273-019dbfd441e5)


## Contributing
Of course, contributions are what make the open source community such an amazing place to learn, inspire, and create. 

If you have a suggestion that would make **Vampire** better, please fork the repo and create a pull request. 

Any contributions you make are greatly appreciated.

## Credits
Icon -> [icon-icons.com](https://icon-icons.com/icon/vampire/2710)

WebServer -> [NanoHTTPD](https://github.com/NanoHttpd/nanohttpd)

## Thanks
Again kudos to awesome xDrip+ Team and all people fighting against diabetes. 

And don't forget to star ☆, fork and contribute!
