<p align="center">
  <a href="https://github.com/homebridge/homebridge"><img src="https://raw.githubusercontent.com/homebridge/branding/master/logos/homebridge-color-round-stylized.png" height="140"></a>
</p>

<span align="center">

# homebridge-web-lock

[![npm](https://img.shields.io/npm/v/homebridge-web-lock.svg)](https://www.npmjs.com/package/homebridge-web-lock) [![npm](https://img.shields.io/npm/dt/homebridge-web-lock.svg)](https://www.npmjs.com/package/homebridge-web-lock)

</span>

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
| `autoLock` | Whether your lock should re-lock after being opened| `false` |
| `autoLockDelay` | Whether your lock should re-lock after being opened (if enabled) | `10` |

### Additional options
| Key | Description | Default |
| --- | --- | --- |
| `pollInterval` | Time (in seconds) between device polls | `300` |
| `port` | Port for your HTTP listener (only one listener per port) | `2000` |
| `timeout` | Time (in milliseconds) until the accessory will be marked as _Not Responding_ if it is unreachable | `3000` |
| `http_method` | HTTP method used to communicate with the device | `GET` |
| `username` | Username if HTTP authentication is enabled | N/A |
| `password` | Password if HTTP authentication is enabled | N/A |
| `model` | Appears under the _Model_ field for the accessory | plugin |
| `serial` | Appears under the _Serial_ field for the accessory | apiroute |
| `manufacturer` | Appears under the _Manufacturer_ field for the accessory | author |
| `firmware` | Appears under the _Firmware_ field for the accessory | version |

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
/lockTargetState?value=INT_VALUE
```

3. Update `lockCurrentState` when it opens/closes by messaging the listen server:
```
/lockCurrentState?value=INT_VALUE
```

4. Update `lockTargetState` following a manual override by messaging the listen server:
```
/lockTargetState?value=INT_VALUE
```

## LockState Key

| Number | Name |
| --- | --- |
| `0` | Unlocked |
| `1` | Locked |

