# IOS-IOT-SDK
This SDK is dedicated to handling connection + bidirectional communication with the `ub_ble_scanner` device.

## Installing
- Add `IoTSDK.framework` to your project.
- Add `IoTSDK.framework` to `Embeded Binaries` in General project settings
- Add `IoTSDK.framework` to `Linked Frameworks and Libraries`

## How to use?

### Connection

An instance of `ConnectionManager` is required to try to connect to a dongle.

When creating the ConnectionManager, you need to pass the object that will act as the `ConnectionManagerDelegate` and receive callbacks when a connection is successfully established or when it fails.

You can also pass an instance of `DongleFilter` to the `ConnectionManager` in order to influence the dongle which will be used.

When calling the `connect` method, the iPhone will try to find nearby connectable devices and connect to the first dongle which matches the filter. It eventually call one of the two callbacks of the `ConnectionManagerDelegate`:

```
func didConnect(sender: ConnectionManager, toDongle dongle: Dongle)
```

or

```
func didFailToConnect(sender: ConnectionManager, withError error: Error?)
```

### Communication

After the successful connection, it is possible to send some data in both directions between the iPhone and the dongle by using:

```
public func send(data: Data)
```

And by implementing `DongleDelegate` protocol which will be notified of communication events via the following callbacks:

```
func didReceiveData(data: Data?)
```

This method is called after receiveing data from the dongle.

```
func didSendData(data: Data?)
```

This method is called after sending data to the dongle.

```
func didReceiveError(error: Error?)
```

This method is called if some error occured while receiveing data from the dongle.


