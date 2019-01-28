# Neato Custom Component For Home Assistant

This is a custom component designed for Home Assistant to test new Neato features.

Currently testing `vacuum.neato_zone_cleaning` service (For D7 model only).  Please see device attributes for the zone ID.

Developer Docs: https://developers.neatorobotics.com/api/robot-remote-protocol/housecleaning

Steps to install.

1. Create a `custom_components` folder in your home assistant configuration directory if it does not exist already
2. Download the code here and place all the files inside the `custom_components` folder.  Make sure that `neato.py` from the root of the project is directly inside the `custom_components` folder.  Make sure the 3 folders are also included.
3. Leave Neato configured as it is in your configuration.yaml (or configure it according to the docs: https://www.home-assistant.io/components/neato/)
4. Restart Home Assistant

Then finally the service call data:

`
{"entity_id":"ENTITY_ID",
"mode":2,
"navigation":1,
"category":4,
"boundary_id":"BOUNDARY_ID",
"name":"FRIENDLY_NAME"}
`

Note: `mode` and `navigation` can be modified (https://github.com/stianaske/pybotvac/blob/master/pybotvac/robot.py#L64) however `category` cannot be.

`boundary_id` can be found in device attributes

Enjoy using your Botvac in Home Assistant!

Steps to gather Map data.

1. First create a persistent map in the Neato app if it does not exist.
2. Create the zones you desire in the app.
3. Use pybotvac to retrieve the Map ID and Boundary ID's (will only work if you have terminal access and are able to interact with the library, see pybotvac for details)
4. If you have terminal access type `python3` to enter the python terminal, make sure you activate your virtual environment.
5. `import pybotvac`
6. `account = pybotvac.Account("USERNAME", "PASSWORD")`
7. `account.refresh_robots(`)
8. `print(account.persistent_maps)`

Example output:

`
{'ROBOT_SERIAL': [{'id': 'MAP_ID', 'name': 'MAP_NAME', 'url':
`

Then to retrieve the boundary information:

1. Using the terminal continue with the following steps
2. `robot = Robot('ROBOT_SERIAL', 'ROBOT_SECRET', 'ROBOT_NAME')` If you don't know your robots secret use this method to find it: https://github.com/stianaske/pybotvac#account
3. `boundary = robot.get_map_boundaries('MAP_ID').json()`
4. `print (boundary)`
5. Repeat steps 2-4 for each botvac you have

Example output:

`
{'version': 1, 'reqId': '1', 'result': 'ok', 'data': {'boundaries': [{'id': 'BOUNDARY_ID', 'type': 'polygon', 'vertices': [[0.3913, 0.4919], [0.6008, 0.4919], [0.6008, 0.6337], [0.3913, 0.6337]], 'name': 'BOUNDARY_NAME', 'color': '#5B7BA5', 'enabled': True}, {'id': 'BOUNDARY_ID', 'type': 'polygon', 'vertices': [[0.6202, 0.2937], [0.7045, 0.2937], [0.7045, 0.3778], [0.6202, 0.3778]], 'name': 'BOUNDARY_NAME', 'color': '#E49770', 'enabled': True}]}}
`
