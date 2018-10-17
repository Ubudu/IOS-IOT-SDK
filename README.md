# IOS-IOT-SDK
This SDK is dedicated to handling connection + bidirectional communication with the bluetooth LE devices.

## Requirements
- Xcode 10+
- Swift 4.2+

## Installing
- Add `IoTSDK.framework` to your project.
- Add `IoTSDK.framework` to `Embeded Binaries` in General project settings
- Add `IoTSDK.framework` to `Linked Frameworks and Libraries`

## How to use?
Before using IOT SDK it is good to set a reusable identifier for the SDK:

`IOTSDK.setSharedInstance(with: "com.company.someIdentier")`

To find dongles use a static function `scan` from IOTSDK.

```
IOTSDK.scan { scanResult in
	switch (scanResult) {
		case .scanStarted:
			NSLog("IoTSDK scan started")
		case .scanResult(peripheral: let peripheral, advertisementData: let advertisementData, RSSI: let rssi):
			NSLog("IoTSDK scan result: \(peripheral), advertisement data: \(advertisementData), RSSI: \(rssi)")
		case .scanStopped(error: let error):
			NSLog("IoTSDK scan stopped with error: \(error ?? nil)")
    }
}
```

This function call `scanResult` each time it finds a Device or when error occurs.

IOT SDK provides a possibility to `connect`, `disconnect`, `send` some data to the connected device.

### Sample code

```
IOTSDK.connect(peripheral, completion: { connectionResult in
	guard connectionResult.isSuccess == true else { /* handle error */ return }
}
```