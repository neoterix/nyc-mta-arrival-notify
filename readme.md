**Introduction**
------------

This a basic python script that reads the NYC MTA realtime feed API for subway data and outputs when the next arrival is for a given station. It was created as an self-directed educational exercise to build something from scratch and hopefully you might find it useful too!

This script uses the New York MTA real-time data feed for its subway system:

* [NYC MTA Live Feed Documentation](http://datamine.mta.info/sites/all/files/pdfs/GTFS-Realtime-NYC-Subway%20version%201%20dated%207%20Sep.pdf)
* [List of Live MTA Feeds](http://datamine.mta.info/list-of-feeds)
* [Register for an API key](http://datamine.mta.info/user/register)

Because the New York MTA real-time data feed uses the [GTFS-realtime](https://developers.google.com/transit/gtfs-realtime/) data exchange format (which is based upon [protocol buffers](https://developers.google.com/protocol-buffers/)) this script relies on kaporzhu's [protobuf3-to-dict](https://github.com/kaporzhu/protobuf-to-dict) package to convert the MTA's GTFS feed into a more python-friendly set of python dictionaries and lists.

**Requirements**
-------------------------------
This script relies on the following packages: Python language bindings for GTFS, protobuf3-to-dict, and dotenv to insert the API key provided by the MTA. If you use PIP, the following commands will do the trick:

    pip install --upgrade gtfs-realtime-bindings
    pip install protobuf3_to_dict
    pip install -U python-dotenv

Access to the MTA live feeds requires signing up for an API key from the NYC MTA: http://datamine.mta.info/user/register

Put API key into the changeme.env file (after `API_KEY=`) and rename the file into an .env file within the root directory of the Python script.

**Script Behavior**
---------------

In short, the script pulls a realtime feed from the NYC MTA and converts it from GTFS-realtime into Python a set of Python dictionaries and lists (see the samples directory). This feed contains a variety of north and southbound subway trains and their predicted arrival and departure times for whatever stops it has ahead of it, such that each feed contains several times for any one given station and direction. Consequently, the script creates an empty list `collected_times` to collect these various times. The MTA offers different feeds that correspond to different subway lines, and currently the feed is hardcoded to the B/D subway feed.

The primary function `station_time_lookup` takes two arguments, a converted MTA data feed and a specific station ID, and loops through various nested dictionaries and lists to (1) filter out active trains, (2) search for the given station ID, and (3) append the arrival time of any instance of the station ID to `collected_times`. Note that the times in the MTA feed are in [epoch time](https://www.epochconverter.com/), and require conversion if you wish to display something more human readable (which the script later does).

The specific station ID is currently hardcoded in, and is set for the southbound Broadway-Lafayette stop. The list of subway stops and station IDs may be found in the MTA's collection of [*static* data feeds](http://web.mta.info/developers/developer-data-terms.html).

With the `collected_times` list now populated, you now have a list of all of the predicted arrival times and can do a variety of things with it. The rest of the script sorts this list to find the next two arrival times, pulls the current time to figure out the time until next arrival, and then prints out one of three different messages depending on the time to arrival.

**License**
------------
Uses the MIT license.

> Written with [StackEdit](https://stackedit.io/).
