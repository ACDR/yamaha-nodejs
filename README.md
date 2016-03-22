Yamaha-nodejs
==================

A node module to control your yamaha receiver. Tested with RX-V775, should work with all yamaha receivers with a network interface.

### Install
npm install yamaha-nodejs

## Example
```javascript
var YamahaAPI = require("yamaha-nodejs");
var yamaha = new YamahaAPI("192.168.0.100");
yamaha.powerOn().done(function(){
	console.log("powerOn");
	yamaha.setMainInputTo("NET RADIO").done( function(){
		console.log("Switched to Net Radio");
		yamaha.selectWebRadioListItem(1).done(function(){
			console.log("Selected Favorites");
			yamaha.selectWebRadioListItem(1).done(function(){});
		});

	});
});
```
## Prerequisites
* To power on the yamaha, network standby has to be enabled
* The Yamaha reveiver is stateful. Some commands only work work if the receiver is in the right state. E.g. to get web radio channels, the "NET RADIO" input has to be selected.

## Methods
```javascript
var yamaha = new Yamaha("192.168.0.100")
yamaha.powerOff(zone)
yamaha.powerOn(zone)
yamaha.isOn(zone)
yamaha.isOff(zone)

//Volume
yamaha.setVolumeTo(-500)
yamaha.volumeUp(50)
yamaha.volumeDown(50)
yamaha.muteOn(zone)
yamaha.muteOff(zone)

//Playback
yamaha.stop(zone)
yamaha.pause(zone)
yamaha.play(zone)
yamaha.skip(zone)
yamaha.rewind(zone)

//Switch Input
yamaha.setInputTo("USB", 2)
yamaha.setMainInputTo("NET RADIO")

//Party Mode
yamaha.partyModeOn()
yamaha.partyModeOff()
yamaha.partyModeUp()
yamaha.partyModeDown()

//Basic
yamaha.SendXMLToReceiver()

//Get Info
yamaha.getBasicInfo(zone).done(function(basicInfo){
    basicInfo.getVolume();
    basicInfo.isMuted();
    basicInfo.isOn();
    basicInfo.isOff();
    basicInfo.getCurrentInput();
    basicInfo.isPartyModeEnabled();
    basicInfo.isPureDirectEnabled();
})

yamaha.getSystemConfig()
yamaha.getAvailableInputs()
yamaha.isMenuReady("NET_RADIO")

// FM Tuner
yamaha.selectTunerPreset(1)
yamaha.selectTunerFrequency(band, frequency)

//Select Menu Items
yamaha.selectUSBListItem(1)
yamaha.selectWebRadioListItem(1)

// Single Commands, receiver has to be in the right state
yamaha.getWebRadioList()
yamaha.selectWebRadioListItem(1)

// Chained Commands, they ensure the receiver is in the right state
yamaha.switchToFavoriteNumber() 
    
    
```

#### Zones
the zone parameter is optional, you can pass a number or a string

#### Promises
All these methods return a promise:
```javascript
yamaha.isOn().then(function(result){
	console.log("Receiver is:"+result);
})
```
#### Execute Tests
```javascript
mocha mochatest.js --ip 192.168.0.25
```
