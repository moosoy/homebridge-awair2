# homebridge-awair2
This is a second generation Homebridge Dynamic Platform plugin for the Awair family of air quality sensors implemented in TypeScript for Nfarina's [Homebridge project](https://github.com/nfarina/homebridge). The Awair2 plugin is based on the [homebridge-awair](https://github.com/deanlyoung/homebridge-awair#readme) plugin developed by Dean L. Young.

The Awair2 plugin will query your Awair account using a Developer Token to determine your registered Awair devices setup through the Awair app on your iOS device. While running, the plugin will fetch current sensor conditions for each Awair device (e.g. Awair, Awair Glow, Awair Mint, Awair Omni, Awair 2nd Edition or Awair Element) and provide sensor status and value (e.g. temperature, humidity, carbon dioxide, TVOC, and dust/PM2.5/PM10) to HomeKit. You can look at the current Awair information via HomeKit enabled Apps on your iOS device or even ask Siri for them.

The plugin will fetch new data based on selected `endpoint` and User Account tier. For 'Hobbyist' tier, `15-min-avg` endpoint samples every 15 minutes, `5-min-avg` every 5 minutes, `latest` every 5 minutes and `raw` every 3.3 minutes (200 seconds). 

With iOS14, the icons and status have been refined for HomeKit. Air quality, temperature and humidity are grouped under a single "climate" status icon at the top of the HomeKit screen (first screen shots below). If you touch this icon a screen opens with all of the Climate devices in your HomeKit home (second screen shot).

![iOS14 Screenshots](screenshots/Image.png)

For those with multiple Awair devices, you can optionally list the macAddress of the device (found on the back or bottom of the device) which you want to exclude from HomeKit.

For Awair Omni, battery charge level, charging status, low battery and light level are also provided using the Local Sensors capability which is configured in the Awair App (reference screenshot below). Battery Status does not appear as a separate tile in the HomeKit interface. Battery charge level and status will be found in the Status menu for each of the sensors. A low battery indication will be identified as an alert in the HomeKit status (see 3rd and 4th screen shots).

![iOS14 Screenshots](screenshots/Image2.png)

<b>NOTE:</b> If you are setting up an Awair unit for the first time, it is recommended that you allow 6 to 12 hours after adding to the iOS Awair App for the unit to calibrate, update firmware if necessary and establish connection to the Awair API cloud.

Acknowledgment to @Sunoo for the homebridge-philips-air plugin which was used as a reference for implementation of the Awair Dynamic Platform TypeScript plugin.

# Installation

1. Install homebridge using: `[sudo] npm install -g homebridge`
2. Install this plugin using: `[sudo] npm install -g homebridge-awair2`
3. Update your configuration file. See the sample below.

The Awair2 plugin queries your Awair account to determine devices that you have registered. This is the same informaton that you have entered via the Awair app on your iOS dev ice.

You will need to request access to the [Awair Developer Console](https://developer.getawair.com) to obtain your Developer Token (`token`).

The [Awair Developer API Documentation](https://docs.developer.getawair.com) explains the inner workings of the Awair Developer API, but for the most part is not necessary to use this plugin.

# Changelog

Changelog is available [here](https://github.com/DMBlakeley/homebridge-awair2/blob/master/CHANGELOG.md).

# Plugin Configuration

Configuration sample:

See [config-sample.json](https://github.com/DMBlakeley/homebridge-awair2/blob/master/config-sample.json)

```
"platforms": [
  {
    "platform": "Awair2",
    "token": "AAA.AAA.AAA",
    "userType": "users/self",
    "airQualityMethod": "awair-score",
    "endpoint": "15-min-avg",
    "limit": 12,
    "logging": false,
    "verbose": false,
    "carbonDioxideThreshold": 1200,
    "carbonDioxideThresholdOff": 800,
    "ignoredDevices": [
      "70886B10xxxx"
    ]
  }
]
```

## Descriptions

Parameter | Description
------------ | -------------
`platform` | The Homebridge Accessory (REQUIRED, must be exactly: `Awair2`)
`token` | Developer Token (REQUIRED, see [Installation](#installation)) above.
`userType` | The type of user account (OPTIONAL, default = `users/self`, options: `users/self` or `orgs/###`, where ### is the Awair Organization `orgId`)
`airQualityMethod` | Air quality calculation method used to define the Air Quality Chracteristic (OPTIONAL, default = `awair-score`, options: `awair-score`, `aqi`, `nowcast-aqi`)
`endpoint` | The `/air-data` endpoint to use (OPTIONAL, default = `15-min-avg`, options: `15-min-avg`, `5-min-avg`, `raw` or `latest`)
`limit` | Number of consecutive 10 second data points returned per request, used for custom averaging of sensor values from `raw` endpoint (OPTIONAL, default = `12` i.e. 2 minute average). Defaults to 1 for `15-min-avg`, `5-min-avg` or `latest`.
`logging` | Whether to output logs to the Homebridge logs (OPTIONAL, default = `false`)
`verbose` | Whether to log results from API data calls (OPTIONAL, default = `false`). Requires `logging` to be `true`.
`carbonDioxideThreshold` | (OPTIONAL, default = `0` [i.e. OFF], the level at which HomeKit will trigger an alert for the CO2 in ppm)
`carbonDioxideThresholdOff` | (OPTIONAL, default = `0` [i.e. `carbonDioxideThreshold`], the level at which HomeKit will turn off the trigger alert for the CO2 in ppm, to ensure that it doesn't trigger on/off too frequently choose a number lower than `carbonDioxideThreshold`)
`ignoredDevices` | Array of Awair device macAddresses to be excluded from HomeKit (OPTIONAL).


# Resources

- Awair API: https://docs.developer.getawair.com/
- Homebridge: https://github.com/nfarina/homebridge
- Homebridge API: https://developers.homebridge.io/#/
- Homebridge examples: https://github.com/homebridge/homebridge-examples
- Homebridge Platform Plugin Template: https://github.com/homebridge/homebridge-plugin-template
- Homebridge plugin development: http://blog.theodo.fr/2017/08/make-siri-perfect-home-companion-devices-not-supported-apple-homekit/
- Using async-await and npm-promise with TypeScript: https://github.com/patdaburu/request-promise-typescript-example
- Another Awair plugin: https://github.com/henrypoydar/homebridge-awair-glow
- Reference AQ plugin: https://github.com/toto/homebridge-airrohr
- Refenerce temperature plugin: https://github.com/metbosch/homebridge-http-temperature
- AQI Calculation NPM package: https://www.npmjs.com/package/aqi-bot
- Homebridge-philips-air plugin: https://github.com/Sunoo/homebridge-philips-air
