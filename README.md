# [EPOCH 1.0.6.2] Locate Vehicles

* Discussion URL: https://epochmod.com/forum/topic/43819-release-locate-vehicle-vkc-support/
	
* Tested as working on a blank Epoch 1.0.6.2
* This adds support for multiple vehicles per key (https://github.com/oiad/vkc)
* Designed to be light weight and optimized.

# REPORTING ERRORS/PROBLEMS

1. Please, if you report issues can you please attach (on pastebin or similar) your CLIENT rpt log file as this helps find out the errors very quickly. To find this logfile:

	```sqf
	C:\users\<YOUR WINDOWS USERNAME>\AppData\Local\Arma 2 OA\ArmA2OA.RPT
	```

# Install:

* This uses Click Actions by Mudzereli as a dependancy: https://github.com/mudzereli/DayZEpochDeployableBike/tree/master/overwrites/click_actions

**[>> Download <<](https://github.com/oiad/locateVehicle/archive/master.zip)**

# Mission folder install:

1. Move the <code>scripts</code> folder and file <code>scripts\locateVehicle.sqf</code> to your mission folder root preserving directory structure.
	```sqf
	scripts\locateVehicle.sqf
	```

2. Edit your clickActions\config.sqf and add this line to your DZE_CLICK_ACTIONS array:
	```sqf
	["ItemMap","Identify Keys","execVM 'scripts\locateVehicle.sqf';","true"]
	```

	For example:

	```sqf
	DZE_CLICK_ACTIONS = [
		["ItemGPS","Scan Nearby","if(isNil 'DZE_CLICK_ACTIONS_GPS_RANGE') then {DZE_CLICK_ACTIONS_GPS_RANGE = 1500;};DZE_CLICK_ACTIONS_ZOMBIE_COUNT = count ((position player) nearEntities ['zZombie_Base',DZE_CLICK_ACTIONS_GPS_RANGE]); DZE_CLICK_ACTIONS_MAN_COUNT = count ((position player) nearEntities ['CAManBase',DZE_CLICK_ACTIONS_GPS_RANGE]); format['Within %1 Meters: %2 AI/players, %3 zombies, %4 vehicles',DZE_CLICK_ACTIONS_GPS_RANGE,DZE_CLICK_ACTIONS_MAN_COUNT - DZE_CLICK_ACTIONS_ZOMBIE_COUNT,count ((position player) nearEntities ['zZombie_Base',DZE_CLICK_ACTIONS_GPS_RANGE]),count ((position player) nearEntities ['allVehicles',DZE_CLICK_ACTIONS_GPS_RANGE]) - DZE_CLICK_ACTIONS_MAN_COUNT] call dayz_rollingMessages;","true"],
		["ItemGPS","Range Up"   ,"if(isNil 'DZE_CLICK_ACTIONS_GPS_RANGE') then {DZE_CLICK_ACTIONS_GPS_RANGE = 1500;};DZE_CLICK_ACTIONS_GPS_RANGE = (DZE_CLICK_ACTIONS_GPS_RANGE + 100) min 2500; format['GPS RANGE: %1',DZE_CLICK_ACTIONS_GPS_RANGE] call dayz_rollingMessages;","true"],
		["ItemGPS","Range Down" ,"if(isNil 'DZE_CLICK_ACTIONS_GPS_RANGE') then {DZE_CLICK_ACTIONS_GPS_RANGE = 1500;};DZE_CLICK_ACTIONS_GPS_RANGE = (DZE_CLICK_ACTIONS_GPS_RANGE - 100) max 1000; format['GPS RANGE: %1',DZE_CLICK_ACTIONS_GPS_RANGE] call dayz_rollingMessages;","true"],
		["ItemMap","Identify Keys","execVM 'scripts\locateVehicle.sqf';","true"]
	];
	```

	If it's the last item in the array, then you must make sure you don't have a <code>,</code> at the end.

3. Download the <code>stringTable.xml</code> file linked below from the [Community Localization GitHub](https://github.com/oiad/communityLocalizations) and copy it to your mission folder, it is a community based localization file and contains translations for major community mods including this one.

**[>> Download stringTable.xml <<](https://github.com/oiad/communityLocalizations/blob/master/stringTable.xml)**

# BattlEye filters:

1. In your config\<yourServerName>\Battleye\scripts.txt around line 20: <code>5 deleteMarker</code> add this to the end of it:

	```sqf
	!="for \"_i\" from 0 to 60 do {deleteMarkerLocal (\"vehicleMarker\"+ (str _i));};"
	```

	So it will then look like this for example:

	```sqf
	5 deleteMarker !"} count allDead;\n\n\nif (dayz_oldBodyCount > _bodyCount) then {" !="for \"_i\" from 0 to 60 do {deleteMarkerLocal (\"vehicleMarker\"+ (str _i));};"
	```

2. In your config\<yourServerName>\Battleye\scripts.txt around line 14: <code>5 createMarker</code> add this to the end of it:

	```sqf
	!="_marker = createMarkerLocal [\"vehicleMarker\" + (str _i),[_position select 0,_position select 1]];"
	```

	So it will then look like this for example:

	```sqf
	5 createMarker <CUT> !="_marker = createMarkerLocal [\"vehicleMarker\" + (str _i),[_position select 0,_position select 1]];"
	```