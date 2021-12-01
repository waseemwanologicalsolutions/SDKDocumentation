
# Intempt iOS SDK

## Requirements
- iOS 12.0+
- Minimum Xcode 10.0


## Getting Started

### Integration :
1. At first download sdk from https://github.com/intempt/ios-sdk.git). Then open the folder frameworks. There are three sdks in  this folder **'device' 'simulator'** and **'universal'**. 
<img width="573" alt="1" src="https://user-images.githubusercontent.com/93919087/144225604-a6e8ca7e-9d0f-4210-ac21-c0d2bd8ac06f.png">


2. Open **universal** folder and Drag & Drop `Intempt.Framework` in your iOS app.
<img width="1417" alt="2" src="https://user-images.githubusercontent.com/93919087/144225626-73c69b69-cc2f-4f91-8b46-e7d832871460.png">
<img width="1244" alt="3" src="https://user-images.githubusercontent.com/93919087/144225684-83c33889-52e1-4f0f-adbb-1806cbb55d44.png">

3. `Intempt.framework` must set to `Embed & Sign`
<img width="1243" alt="4" src="https://user-images.githubusercontent.com/93919087/144225710-c4b4c9d0-a24f-4fc6-97ae-c82834185d27.png">




### Swift :
If Xcode 11.3 or above

Goto AppDelegate.swift file

``` swift
import Intempt
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        
        ///Your code here
        
        //Initialize Intempt SDK
        let intemptConfig = IntemptConfig(queueEnabled: true, withItemsInQueue: 7, withTimeBuffer: 15, withInitialDelay: 0.3, withInputTextCaptureDisabled: false)
        IntemptTracker.tracking(withOrgId: "Your Organization Id", withSourceId: "Your Source ID", withToken: "Your Token", withConfig: intemptConfig) { (status, result, error) in
            if(status) {
                if let dictResult = result as? [String: Any] {
                    print(dictResult)
                }
            }
            else {
                if let error = error {
                    print(error.localizedDescription)
                }
            }
        }
        
        return true
    }
```
Else you will have the `ViewController.swift` file and then paste the copied source snippet into the `viewDidLoad` function: 

``` swift
import Intempt
  override func viewDidLoad() {
        super.viewDidLoad()
      
	 //Initialize Intempt SDK
        let intemptConfig = IntemptConfig(queueEnabled: true, withItemsInQueue: 7, withTimeBuffer: 15, withInitialDelay: 0.3, withInputTextCaptureDisabled: false)
        IntemptTracker.tracking(withOrgId: "Your Organization Id", withSourceId: "Your Source ID", withToken: "Your Token", withConfig: intemptConfig) { (status, result, error) in
            if(status) {
                if let dictResult = result as? [String: Any] {
                    print(dictResult)
                }
            }
            else {
                if let error = error {
                    print(error.localizedDescription)
                }
            }
        }
}
```
### Objective C :

If you are using Xcode 11.3 or above go to `AppDelegate.m` file and paste the copied source snippet like the following:

``` objectivec
@import Intempt;
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // Your code here
    
    //initalize intempt SDK
    IntemptConfig *intemptConfig  = [[IntemptConfig alloc]initWithQueueEnabled:YES withItemsInQueue:7 withTimeBuffer:15 withInitialDelay:0.3 withInputTextCaptureDisabled:NO];
    [IntemptTracker trackingWithOrgId:@"Your Organization Id" withSourceId:@"Your Source ID" withToken:@"Your Token" withConfig:intemptConfig onCompletion:^(BOOL status, id result, NSError *error) {
        
    }];
    
    return YES;
}
```

Else you will have the `ViewController.m` file and then paste the copied source snippet like the following:

``` objectivec
@import Intempt
- (void)viewDidLoad {
    [super viewDidLoad];
    
     //initalize intempt SDK
    IntemptConfig *intemptConfig  = [[IntemptConfig alloc]initWithQueueEnabled:YES withItemsInQueue:7 withTimeBuffer:15 withInitialDelay:0.3 withInputTextCaptureDisabled:NO];
    [IntemptTracker trackingWithOrgId:@"Your Organization Id" withSourceId:@"Your Source ID" withToken:@"Your Token" withConfig:intemptConfig onCompletion:^(BOOL status, id result, NSError *error) {
        
    }];
}
```
### Get Organization ID and Source ID
To get your organization Id and source Id you need to create your account on https://app.intempt.com/ and then go to https://app.intempt.com/sources/ 
- 1 Create Organization
- 2 Create Source
- 3 Copy Source ID, Organization Id and Token in your Xcode app for IntemptSDK initialization. 

<img width="1256" alt="5" src="https://user-images.githubusercontent.com/93919087/144246773-6454df6b-8a38-4494-83a4-49f631f81a5d.png">

### Events Tracking

IntemptSDK track different type events, some of these are default and some are custom, default events are automatically started tracking when SDK is initialized and for custom events developer have to write code whereever required. Below types events are in IntemptSDK
- Application Launch Tracking (Default, automatically tracked)
- Screen Tracking (Default, automatically tracked)
- Interaction Tracking (Default, automatically tracked)
- Consent (Default, automatically tracked)
- Location (Depending on application, if user of the app has allowed location permission then country, city level location is tracked)
- Identity (Manual, Developer have to implement it)
- Custom (Developer have to create schema and implement in app)

#### Get VisitorId From Framework

``` swift
let visitorId = IntemptClient.shared()?.getVisitorId()
```
#### Identifying Visitors
Provide email or phone number.

``` swift
IntemptTracker.identify("test@example.com", withProperties: nil) { (status, error) in 
	if(status) {
		//Do something
	}
}
```
#### Custom Event
On using this a user can create a custom event based on the need of the project and can track the event details with that custom method.
To add custom event below should be flow

- 1 Visit the Intempt Console https://app.intempt.com/sources/
- 2 Select Organization -> Source -> Schema
- 3 Drag 'Add collection' from the right panel(Schema Builder) to the exisiting collections list
- 5 Drag the 'Add field' to the added collection, add as many fields as required.
- 6 Set field type carefully e.g if the data from app is string and field type set in int then there will be error.
- 7 If you want to link the events with the visitor then add 'visitorId' as foreign key of 'Profile' collection

**Please becarefull when renaming, Collection and Field name 
always start with small letter**

<img width="1258" alt="6" src="https://user-images.githubusercontent.com/93919087/144252093-2047820a-5132-4b1f-8aac-08393b1c2001.png">


```swift 
let arrayData = [{       
                "flightId" : "1",
                "bookingDate" : "2020-05-26",
                "bookingId": "2",
                "bookingStatus" : "booked"
              }]
        IntemptTracker.track("flight-booking", withProperties: arrayData as? [Any]) { (status, result, error) in
            if(status) {
                if let dictResult = result as? [String: Any] {
                    print(dictResult)
                }
            }
            else {
                if let error = error {
                    print(error.localizedDescription)
                }
            }
        }
}             
```
Schema of above example 'flight-booking' looks like below
<img width="1265" alt="7" src="https://user-images.githubusercontent.com/93919087/144252300-6f478ddd-07f7-4ea8-9bf5-69fb63fc8c35.png">


#### Disable Text Capture
Call this method if you want to disable capturing input texts like UItextField, UItextView. By default its `false`. Secure entries are also excluded for privacy.

```swift 
IntemptTracker.disableTextInput(true)
```

#### Disable Default Event Tracking
Call this method if you want to disable default tracking. This action is persistent, once disabled then developer must need to enable again when want to track default events again.

```swift 
IntemptClient.disableTracking()
```

#### Enable Default Event Tracking
Call this method if you had disabled tracking and want to enable again.

```swift 
IntemptClient.enableTracking()
```

#### Enable Event Logging
Call this method if you want to see the logs of all generated events, errors for debug purposes. By default logging is disabled

```swift 
IntemptClient.enableLogging()
```
#### Disable Event Logging
Call this method if you want not see any output in console.

```swift 
IntemptClient.disableLogging()
```

## Intempt Proximity
### ibeacon does not support Xcode iOS Simulator. You need a real device to test the ibeacon.
Install an Intempt beacon At your preferable place. Add this SDK in your existing application. Thus it will help you to know about your entry and exit range.
This sdk supports any kind of beacon that allows you to change the UUID in the vendors Beacon Manager. The UUID is a unique broadcasting number which allows Intempt to know which types of beacons to look for. You can think of it like tuning to a radio station.

Today, Intempt’s mobile SDK looks for ONE UUID by default. Use any of them with your existing beacons to enable them with Intempt.

### Key components

Intempt Proximity SDK is built on top of two key components: _

- Entry -  when you are enter a region.

- Exit -  when you are exit a region.


Go to app's Info.plist file and add the privacy keys.


| Key | Value |
| ------ | ------ |
| Privacy - Location Always Usage Description | Location used to track where you are |
| Privacy - Location When In Use Usage Description | Location used to track where you are |
| Privacy - Location Always and When In Use Usage Description | Location used to track where you are |
| Privacy - Bluetooth Always Usage Description | This app uses Bluetooth communication to detect nearby beacons |
| Privacy - Bluetooth Peripheral Usage Description | This app uses Bluetooth communication to detect nearby beacons |




### Swift :

ViewController.swift file

``` swift
import Intempt
  override func viewDidLoad() {
        super.viewDidLoad()
        IntemptTracker.beacon(withOrgId: "Your Organization ID", andSourceId: "Your Source ID", andToken: "Your Token", andDeviceUUID:"Beacon device UUID")
        IntemptClient.shared()?.delegate = self
  }
```

Next implement this methods:

``` swift
/*Helps to detect the entry time , when application entered the monitoring region*/
func  didEnterRegion(_ beaconData: CLBeacon!) {
}

/*Helps to detect the exit time , when application exits the monitoring region*/
func  didExitRegion(_ beaconData: CLBeacon!) {
}
```

### Objective C :
ViewController.m file

``` objectivec
@import Intempt;
- (void)viewDidLoad {
    [super viewDidLoad];
    [IntemptTracker beaconWithOrgId:@"Your Organization ID" andSourceId:@"Your Source ID" andToken:@"Your Token" andDeviceUUID:@"Beacon device UUID"];
    IntemptClient.sharedClient.delegate = self;
}
```

Next implement this methods:

``` objectivec
/*Helps to detect the entry time , when application entered the monitoring region*/
- (void)didEnterRegion:(CLBeacon*)beaconData {
}

/*Helps to detect the exit time , when application exits the monitoring region*/
- (void)didExitRegion:(CLBeacon*)beaconData {
}
```


#### Identifying Visitors (Optional)
Provide email or phone number.

``` swift
IntemptTracker.identifyUsingBeacon("test@example.com", withProperties: nil) { (status, error) in 
	if(status) {
		//Do something
	}
}
```
Use this method when you want to identify using beacon API separately.  
##How to test beacon functionality using Beacon Android simulator?
### Installation :
* At first Download App from https://play.google.com/store/apps/details?id=net.alea.beaconsimulator&hl=en_IN

* Then open the iOS app you integrated Intempt framework.

* Next set up beacon simulator in each room.
The room you go to will get the entry and exit value of the room.







