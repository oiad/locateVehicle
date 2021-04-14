# [EPOCH 1.0.7] Locate Vehicles
Locate Vehicles for Epoch 1.0.7 by salival updated by Airwaves Man (https://github.com/oiad)

* Discussion URL: https://epochmod.com/forum/topic/43819-release-locate-vehicle-vkc-support/
	
* Tested as working on a blank Epoch 1.0.7
* This adds support for multiple vehicles per key (https://github.com/oiad/vkc)
* Designed to be light weight and optimized.

# REPORTING ERRORS/PROBLEMS

1. Please, if you report issues can you please attach (on pastebin or similar) your CLIENT rpt log file as this helps find out the errors very quickly. To find this logfile:

	```sqf
	C:\users\<YOUR WINDOWS USERNAME>\AppData\Local\Arma 2 OA\ArmA2OA.RPT
	```
	
# Index:

* [Mission folder install](https://github.com/oiad/locateVehicle#mission-folder-install)
* [BattlEye filter install](https://github.com/oiad/locateVehicle#battleye-filter-install)
* [Old Releases](https://github.com/oiad/locateVehicle#old-releases)


**[>> Download <<](https://github.com/oiad/locateVehicle/archive/master.zip)**

# Install:

* This install basically assumes you have a custom compiles.sqf.

** If not, visit this repo and follow the steps there**
https://github.com/AirwavesMan/custom-epoch-functions

* If you have not installed the Click Actions visit this  
https://github.com/AirwavesMan/epoch-right-click-actions


# Mission folder install:

1. Move the <code>scripts</code> folder and file <code>scripts\locateVehicle.sqf</code> to your mission folder root preserving directory structure.
	
	```sqf
	scripts\locateVehicle.sqf	
	```
	
2. Open your compiles.sqf and search for:

	```sqf
	if (!isDedicated) then {
	```
	
	And add this inside the square brackets so it looks like this:
	
	```sqf
	if (!isDedicated) then {
		locateVehicle = compile preprocessFileLineNumbers "scripts\locateVehicle.sqf";
	};	
	```

3. Edit your clickActions\config.sqf and add this line to your DZE_CLICK_ACTIONS array:

	```sqf
	["ItemMap","Identify Keys","[] spawn locateVehicle;","true"]
	```

	For example:

	```sqf
	DZE_CLICK_ACTIONS = [
		["ItemGPS","Scan Nearby","if(isNil 'DZE_CLICK_ACTIONS_GPS_RANGE') then {DZE_CLICK_ACTIONS_GPS_RANGE = 1500;};DZE_CLICK_ACTIONS_ZOMBIE_COUNT = count ((position player) nearEntities ['zZombie_Base',DZE_CLICK_ACTIONS_GPS_RANGE]); DZE_CLICK_ACTIONS_MAN_COUNT = count ((position player) nearEntities ['CAManBase',DZE_CLICK_ACTIONS_GPS_RANGE]); format['Within %1 Meters: %2 AI/players, %3 zombies, %4 vehicles',DZE_CLICK_ACTIONS_GPS_RANGE,DZE_CLICK_ACTIONS_MAN_COUNT - DZE_CLICK_ACTIONS_ZOMBIE_COUNT,count ((position player) nearEntities ['zZombie_Base',DZE_CLICK_ACTIONS_GPS_RANGE]),count ((position player) nearEntities ['allVehicles',DZE_CLICK_ACTIONS_GPS_RANGE]) - DZE_CLICK_ACTIONS_MAN_COUNT] call dayz_rollingMessages;","true"],
		["ItemGPS","Range Up"   ,"if(isNil 'DZE_CLICK_ACTIONS_GPS_RANGE') then {DZE_CLICK_ACTIONS_GPS_RANGE = 1500;};DZE_CLICK_ACTIONS_GPS_RANGE = (DZE_CLICK_ACTIONS_GPS_RANGE + 100) min 2500; format['GPS RANGE: %1',DZE_CLICK_ACTIONS_GPS_RANGE] call dayz_rollingMessages;","true"],
		["ItemGPS","Range Down" ,"if(isNil 'DZE_CLICK_ACTIONS_GPS_RANGE') then {DZE_CLICK_ACTIONS_GPS_RANGE = 1500;};DZE_CLICK_ACTIONS_GPS_RANGE = (DZE_CLICK_ACTIONS_GPS_RANGE - 100) max 1000; format['GPS RANGE: %1',DZE_CLICK_ACTIONS_GPS_RANGE] call dayz_rollingMessages;","true"],
		["ItemMap","Identify Keys","[] spawn locateVehicle;","true"]
	];
	```

	If it's the last item in the array, then you must make sure you don't have a <code>,</code> at the end.

# BattlEye filter install:

1. In your config\<yourServerName>\Battleye\scripts.txt around line 26: <code>5 createMarker</code> add this to the end of it:

	```sqf
	!=">> \"CfgVehicles\" >> _vehicle >> \"displayName\");\n_marker = createMarkerLocal [\"vehicleMarker\" + (str _i),[_position select 0,_pos"
	```

	So it will then look like this for example:

	```sqf
	5 createMarker !=">> \"CfgVehicles\" >> _vehicle >> \"displayName\");\n_marker = createMarkerLocal [\"vehicleMarker\" + (str _i),[_position select 0,_pos"
	```

2. In your config\<yourServerName>\Battleye\scripts.txt around line 32: <code>5 deleteMarker</code> add this to the end of it:

	```sqf
	!="ocateMarkerTime = 60; \n\n_i = 0;\nfor \"_i\" from 0 to 60 do {deleteMarkerLocal (\"vehicleMarker\"+ (str _i));};\n\nif (count _keyIDS < "
	```

	So it will then look like this for example:

	```sqf
	5 deleteMarker <CUT> !="ocateMarkerTime = 60; \n\n_i = 0;\nfor \"_i\" from 0 to 60 do {deleteMarkerLocal (\"vehicleMarker\"+ (str _i));};\n\nif (count _keyIDS < "
	```
	
3. In your config\<yourServerName>\Battleye\scripts.txt around line 70: <code>5 setMarkerColor</code> add this to the end of it:

	```sqf
	!="eLocal \"ICON\";\n_marker setMarkerTypeLocal \"DOT\";\n_marker setMarkerColorLocal \"ColorOrange\";\n_marker setMarkerSizeLocal [1.0, 1.0"
	```

	So it will then look like this for example:

	```sqf
	5 setMarkerColor <CUT> !="eLocal \"ICON\";\n_marker setMarkerTypeLocal \"DOT\";\n_marker setMarkerColorLocal \"ColorOrange\";\n_marker setMarkerSizeLocal [1.0, 1.0"
	```	
	
4. In your config\<yourServerName>\Battleye\scripts.txt around line 73: <code>5 setMarkerShape</code> add this to the end of it:

	```sqf
	!="tr _i),[_position select 0,_position select 1]];\n_marker setMarkerShapeLocal \"ICON\";\n_marker setMarkerTypeLocal \"DOT\";\n_marker s"
	```

	So it will then look like this for example:

	```sqf
	5 setMarkerShape <CUT> !="tr _i),[_position select 0,_position select 1]];\n_marker setMarkerShapeLocal \"ICON\";\n_marker setMarkerTypeLocal \"DOT\";\n_marker s"
	```	
	
5. In your config\<yourServerName>\Battleye\scripts.txt around line 74: <code>5 setMarkerSize</code> add this to the end of it:

	```sqf
	!="DOT\";\n_marker setMarkerColorLocal \"ColorOrange\";\n_marker setMarkerSizeLocal [1.0, 1.0];\n_marker setMarkerTextLocal format [\"%1\","
	```

	So it will then look like this for example:

	```sqf
	5 setMarkerSize <CUT> !="DOT\";\n_marker setMarkerColorLocal \"ColorOrange\";\n_marker setMarkerSizeLocal [1.0, 1.0];\n_marker setMarkerTextLocal format [\"%1\","
	```		
	
6. In your config\<yourServerName>\Battleye\scripts.txt around line 75: <code>5 setMarkerText</code> add this to the end of it:

	```sqf
	!="rOrange\";\n_marker setMarkerSizeLocal [1.0, 1.0];\n_marker setMarkerTextLocal format [\"%1\",_name];\nsystemChat format[localize \"STR"
	```

	So it will then look like this for example:

	```sqf
	5 setMarkerText <CUT> !="rOrange\";\n_marker setMarkerSizeLocal [1.0, 1.0];\n_marker setMarkerTextLocal format [\"%1\",_name];\nsystemChat format[localize \"STR"
	```		
	
7. In your config\<yourServerName>\Battleye\scripts.txt around line 76: <code>5 setMarkerType</code> add this to the end of it:

	```sqf
	!=" select 1]];\n_marker setMarkerShapeLocal \"ICON\";\n_marker setMarkerTypeLocal \"DOT\";\n_marker setMarkerColorLocal \"ColorOrange\";\n_m"
	```

	So it will then look like this for example:

	```sqf
	5 setMarkerType <CUT> !=" select 1]];\n_marker setMarkerShapeLocal \"ICON\";\n_marker setMarkerTypeLocal \"DOT\";\n_marker setMarkerColorLocal \"ColorOrange\";\n_m"
	```	
	
# Old Releases:

**** *Epoch 1.0.6.2* ****
**[>> Download <<](https://github.com/oiad/locateVehicle/archive/refs/tags/Epoch_1.0.6.2.zip)**



