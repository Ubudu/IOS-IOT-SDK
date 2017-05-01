# IOS-IOT-SDK
This SDK is dedicated to handling connection + bidirectional communication with the `ub_ble_scanner` device.

## Installing
- Add `IoTSDK.framework` to your project.
- Add `IoTSDK.framework` to `Embeded Binaries` in General project settings
- Add `IoTSDK.framework` to `Linked Frameworks and Libraries`

## How to use?

To find and connect to Dongle use object of class `ConnectionManager` and call `connect` method. This method will try to find and connect to dongle. It eventually call one of the two callbacks via `ConnectionManagerDelegate`:

```    func didConnect(sender: ConnectionManager, toDongle dongle: Dongle)```
or
```    func didFailToConnect(sender: ConnectionManager, withError error: Error?) {
```

### Communication

After successful connection, it is possible to send some data to dongle by using:
```public func send(data: Data)```. Implement `DongleDelegate` protocol to be notified about events:

`func didReceiveData(data: Data?)` - This method is called after receiveing data from Dongle.
`func didSendData(data: Data?)` - This method is called after sending data to Dongle.
`func didReceiveError(error: Error?)` - This method is called if some error occured while receiveing data from Dongle.