# homebridge-web-lock

[![npm](https://img.shields.io/npm/v/homebridge-web-lock.svg)](https://www.npmjs.com/package/homebridge-web-lock) [![npm](https://img.shields.io/npm/dt/homebridge-web-lock.svg)](https://www.npmjs.com/package/homebridge-web-lock)

## Description

This [homebridge](https://github.com/nfarina/homebridge) plugin exposes a web-based lock to Apple's [HomeKit](http://www.apple.com/ios/home/). Using HTTP requests, you can open/close the lock and update the plugin with the lock's current state. The plugin achieves this by setting up a listen server which listens for changes in state from your device and then feeds them real-time into HomeKit.

## Installation

1. Install [homebridge](https://github.com/nfarina/homebridge#installation-details)
2. Install this plugin: `npm install -g homebridge-web-lock`
3. Update your `config.json`

## Configuration

```json
"accessories": [
     {
       "accessory": "HTTPLock",
       "name": "Lock",
       "apiroute": "http://myurl.com"
     }
]
```

### Core
| Key | Description | Default |
| --- | --- | --- |
| `accessory` | Must be `HTTPLock` | N/A |
| `name` | Name to appear in the Home app | N/A |
| `apiroute` | Root URL of your device | N/A |

### Optional fields
| Key | Description | Default |
| --- | --- | --- |
| `pollInterval` _(optional)_ | Time (in seconds) between device polls | `300` |
| `autoLock` _(optional)_ | Whether your lock should re-lock after being opened| `false` |
| `autoLockDelay` _(optional)_ | Whether your lock should re-lock after being opened (if enabled) | `10` |

### Additional options
| Key | Description | Default |
| --- | --- | --- |
| `port` _(optional)_ | Port for your HTTP listener (only one listener per port) | `2000` |
| `timeout` _(optional)_ | Time (in milliseconds) until the accessory will be marked as _Not Responding_ if it is unreachable | `3000` |
| `http_method` _(optional)_ | HTTP method used to communicate with the device | `GET` |
| `username` _(optional)_ | Username if HTTP authentication is enabled | N/A |
| `password` _(optional)_ | Password if HTTP authentication is enabled | N/A |
| `model` _(optional)_ | Appears under the _Model_ field for the accessory | plugin |
| `serial` _(optional)_ | Appears under the _Serial_ field for the accessory | apiroute |
| `manufacturer` _(optional)_ | Appears under the _Manufacturer_ field for the accessory | author |
| `firmware` _(optional)_ | Appears under the _Firmware_ field for the accessory | version |

## API Interfacing

Your API should be able to:

1. Return JSON information when it receives `/status`:
```
{
    "lockCurrentState": INT_VALUE,
    "lockTargetState": INT_VALUE
}
```

2. Open/close the lock when it receives:
```
/lockTargetState/INT_VALUE
```

3. Update `lockCurrentState` when it opens/closes by messaging the listen server:
```
/lockCurrentState/INT_VALUE
```

4. Update `lockTargetState` following a manual override by messaging the listen server:
```
/lockTargetState/INT_VALUE
```

## LockState Key

| Number | Name |
| --- | --- |
| `0` | Unlocked |
| `1` | Locked |

