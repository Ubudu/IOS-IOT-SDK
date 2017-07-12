# IOS-IOT-SDK
This SDK is dedicated to handling connection + bidirectional communication with the `ub_ble_scanner` device.

## Installing
- Add `IoTSDK.framework` to your project.
- Add `IoTSDK.framework` to `Embeded Binaries` in General project settings
- Add `IoTSDK.framework` to `Linked Frameworks and Libraries`

## How to use?

To find dongles use a static function `discover` on from Discover class.

```
static public func discover(forTime discoverTimeout: TimeInterval = 0.0,
                                withFilter filter: BLEDeviceFilter?,
                                _ discoveryHandler: @escaping DiscoveryHandler = { _, _ in }) 
```

This function call `discoveryHandler` each time it finds a Device which passes `BLEDeviceFilter` or when error occurs.

On th `device` there is a possibility to `connect`, `disconnect`, `send` some data to the connected device or register for notifications from already connected device by setting `ReceiveingDataHandler`.

### Sample code

```
        Discovery.discover(withFilter: self) { device, error in
            // Print discovered device
            print("discover device: \(String(describing: device)) error: \(String(describing: error))")
            
            // Stop scanning immediately after finding a 1 device
            Discovery.stopDiscovering()
            
            // Connect to the device
            device?.connect() { error in
                
                guard error == nil else {
                    // Handle connection error
                    print("connection failed: \(String(describing: error?.localizedDescription))")
                    return
                }
                
                // Print tha we are already connected.
                print("connected with error: \(String(describing: error))")
                
                // Register for receiveing a notifications from the device
                device.receiveingDataHandler = { data, error in
                    guard let data = data else {
                        print("receiveing data handler error: \(String(describing: error))")
                        return
                    }
                    let message = String(data: data, encoding: String.Encoding.utf8)
                    print("receive data in string: \(String(describing: message)), error: \ (String(describing: error))")
                }
                
                // Send some data
                self.device?.send(data: data!) { data, error in
                    guard let data = data else {
                        print ("Sending data error")
                        return
                    }
                    
                    guard let dataString = String(data: data, encoding: String.Encoding.ascii) else { return }
                    
                    print("data (\(dataString)) sent with error: \(String(describing: error)))")
                }

            }
        }
```