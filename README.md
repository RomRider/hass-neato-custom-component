# Neato Custom Component For Home Assistant

This is a custom component designed for Home Assistant to test new Neato features.

Currently testing `vacuum.neato_zone_cleaning` service (For D7 model only).

Developer Docs: https://developers.neatorobotics.com/api/robot-remote-protocol/housecleaning

Steps to install.

1. Create a `custom_components` folder in your home assistant configuration directory if it does not exist already
2. Download the code here and place all the files inside the `custom_components` folder.  Make sure that `neato.py` from the root of the project is directly inside the `custom_components` folder.  Make sure the 3 folders are also included.
3. Leave Neato configured as it is in your configuration.yaml (or configure it according to the docs: https://www.home-assistant.io/components/neato/)
4. Restart Home Assistant


Steps to gather Map data.

1. First create a persistent map in the Neato app if it does not exist.
2. Create the zones you desire in the app.
3. Start cleaning one of the zones using the app.
4. Use pybotvac to retrieve robot state (will only work if you have terminal access and are able to interact with the library, see pybotvac for details)
5. If you have terminal access type `python3` to enter the python terminal, make sure you activate your virtual environment.
6. `from pybotvac import Account`
7. Replace with your login data: `for robot in Account('USERNAME', 'PASSWORD').robots:print(robot)`
8. `state = robot.state`
9. `print(state)`
10. Assuming all steps were successful write down/save the `mapId`, the `boundary`: `id` and `boundary`: `name` (the name should appear like it did in the app)
11. Repeat steps to retrieve the additional `boundary` `id` and `name` for each zone you created.
12. Input the data in HA while making the service call.

Example state output:

`
{'version': 1, 'reqId': '1', 'result': 'ok', 'data': {}, 'error': None, 'alert': None, 'state': 2, 'action': 11, 'cleaning': {'category': 4, 'mode': 2, 'modifier': 1, 'navigationMode': 1, 'mapId': 'ABCD', 'boundary': {'id': 'ABCD', 'name': 'Hallway'}, 'spotWidth': 0, 'spotHeight': 0}, 'details': {'isCharging': False, 'isDocked': False, 'isScheduleEnabled': False, 'dockHasBeenSeen': True, 'charge': 94}, 'availableCommands': {'start': False, 'stop': True, 'pause': True, 'resume': False, 'goToBase': True}, 'availableServices': {'findMe': 'basic-1', 'generalInfo': 'basic-1', 'houseCleaning': 'basic-4', 'IECTest': 'advanced-1', 'logCopy': 'basic-1', 'manualCleaning': 'basic-1', 'maps': 'basic-2', 'preferences': 'basic-1', 'schedule': 'basic-2', 'softwareUpdate': 'basic-1', 'spotCleaning': 'basic-3', 'wifi': 'basic-1'}, 'meta': {'modelName': 'BotVacD7Connected', 'firmware': '4.3.1-180'}}
`

Enjoy using your Botvac in Home Assistant!
