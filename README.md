# iOS SDK

## Overview

### Introduction

Here you can find instructions on how to integrate and use FriendlyScore Finance Manager for iOS.

To get started quickly with FriendlyScore Finance Manager for iOS, clone the GitHub repository and run the example. You will need to [sign-up](https://friendlyscore.com/getting-started) for the free API keys through our Developer Console.

### Requirements

- Xcode 10 or greater
- iOS 12.0 or greater
- [FriendlyScore API keys](https://friendlyscore.com/company/keys)

## Quickstart Demo App

Clone and run the demo project from our [GitHub repository](https://github.com/FriendlyScore/FriendlyScore-Finance-Manager-iOS-Example/).

## Installation

FriendlyScore Finance Manager is a framework distributed using [CocoaPods](https://cocoapods.org/) dependency manager. If you are not familiar with this concept, please follow [detailed instructions here](https://guides.cocoapods.org/using/getting-started.html).

To integrate, add `FriendlyScoreFinanceManager` to your `Podfile`

```bash
pod 'FriendlyScoreFinanceManager'
```

then run following command in your project main directory:
```bash
pod install
```
CocoaPods will install and embed all sources and dependencies into your app.

## Implementation
When `FriendlyScoreFinanceManager` is available in customer panel, the SDK will populate a grid of banks to enable connecting and data sharing.

### App Delegate

Start with importing `FriendlyScoreCore` and `FriendlyScoreFinanceManager` framework in your App Delegate:

```swift
import FriendlyScoreCore
import FriendlyScoreFinanceManager
```

Then configure `FriendlyScoreFinanceManager` in `application:didFinishLaunchingWithOptions`:

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    // Override point for customisation after application launch.
    FriendlyScore.configureFinanceManager()
    return true
}
```
### Check Report Availability

Before presenting any view to the user, there is posibilty to check report statuses. If you want to check that, start from importing `FriendlyScoreCore` and `FriendlyScoreFinanceManager`:


```swift
import FriendlyScoreCore
import FriendlyScoreFinanceManager
```
then call report availability function:

```swift
FriendlyScore.reportAvailability { reports in
    //print all available reports:
    for report in reports {
    switch report.id {
        case .insights:
        print("Insights status: \(report.status)")
        case .forecast:
        print("Forecase status: \(report.status)")
        }
    }
}
```

Possible status values: 
```swift
enum ReportStatus {
    case ready
    case notReady
    case noData
```

### UI

Start by importing `FriendlyScoreCore` and `FriendlyScoreFinanceManager`:

```swift
import FriendlyScoreCore
import FriendlyScoreFinanceManager
```

Create the `ClientId` object
```swift
let myClientId = ClientId(stringLiteral: "YOUR_CLIENT_ID")
```
Create the `Credentials` object using `userReference`, which is any alphanumeric string that identifies the user in your systems. It can be used later to access information from the FriendlyScore [API](https://friendlyscore.com/developers) :
```swift
let myCredentials = Credentials(clientId: myClientId, userReference: "YOUR_USER_REFERENCE", environment: .sandbox)
```
Now, using the `myCredentials` object, you can present the `insights` or `forecast` SDK anywhere in you code. Usually this can be done after the user's demand, e.g. tapping a button:

```swift
//somewhere in view controller
let insightsButton = UIButton(frame: CGRect(x: 0, y: 0, width: 130, height: 45))
insightsButton.setTitle("Launch FriendyScore Insights!", for: .normal)
insightsButton.addTarget(self, action: #selector(insightsButtonTapHandler), for: .touchUpInside)

self.view.addSubview(insightsButton)


let forecastButton = UIButton(frame: CGRect(x: 0, y: 65, width: 130, height: 45))
forecastButton.setTitle("Launch FriendyScore Forecast!", for: .normal)
forecastButton.addTarget(self, action: #selector(forecastButtonTapHandler), for: .touchUpInside)

self.view.addSubview(forecastButton)

/**
...
..
*/

@objc func insightsButtonTapHandler(button:UIButton) {
    FriendlyScore.showInsights(with: myCredentials)
}
@objc func forecastButtonTapHandler(button:UIButton) {
    FriendlyScore.showForecast(with: myCredentials)
}
```
`FriendlyScore` will be presented as a modal view at the automatically found top-most view. 


### Theme
`FriendlyScoreFinanceManager` can be presented with  `light` (deafult) or `dark` theme, wiich are predefined list of colors and icons.

```swift
    FriendlyScore.showInsights(with: myCredentials, theme:.dark)
```
There is also possibility to create custom theme by setting list of key-value json file. Please follow our tutorial to [learn more](https://github.com/FriendlyScore/FriendlyScore-FinanceManager-iOS-Example/edit/master/colors.md) .




### Events

`FriendlyScore` will populate different events at different stages. To handle events, please implement following:
```swift
FriendlyScore.eventsHandler = { event in
    switch event {
    case .userClosedView:
        print("SDK view closed by user.")
    case .userCompletedFlow:
        print("Friendy Score user completed flow.")
    }
}
```
### Errors

To receive information about errors implement `errorHandler` that throws various `FriendlyScoreError` objects:
```swift
FriendlyScore.errorHandler = { error in
    switch error {
    case .userReferenceAuth:
        print("There was an authentication error for the supplied `userReference`")
    case .server: //unabled to get config
        print("There was a critical error on the server. SDK view closed automatically")
    case .serviceDenied: //wrong client id, access endpoint unpaid
        print("The service you are trying to connect is denied. Reason: \(error.description). SDK view closed automatically")
    }
}
```
## Next Steps

### Access to Production Environment

You can continue to integrate FriendlyScore Connect in your app in our sandbox and development environments. Once you have completed testing, you can request access to the production environment in the developer console or speak directly to your account manager.

### Support 

Find commonly asked questions and answers in our [F.A.Q](https://friendlyscore.com/developers/faq). You can also contact us via email at [developers@friendlyscore.com](mailto:developers@friendlyscore.com) or speak directly with us on LiveChat.

You can find all the code for FriendlyScore Connect for Web component, iOS and Android on our [GitHub](https://github.com/FriendlyScore).
