# SpeakThis
A module for interacting with the EMIC 2 Text-to-Speech Converter using MQTT.

As part of my Summer Research Scholarship Program at the Institute of Teaching and Learning Innovation (ITaLI) at the University of Queensland, I have designed a MQTT module for interfacing with the EMIC 2 Text-to-Speech Converter.

The main git repository for the project is: https://github.com/CodeSergeant/SpeakThis. Other elements of the project, including preliminary work on a web-based MQTT publisher for the SpeakThis module, and some beginning code for the ESP8266 WiFi module, are included in the general repository: https://github.com/CodeSergeant/UQSummer2014-15.

In the SpeakThis repository, the main file of interest is SpeakThis.js. This is the module that subscribes to an MQTT topic, and reads the messages over a serial interface from the Raspberry Pi to the EMIC 2 module. The SpeakThis bash script is used to daemonise SpeakThis.js, so that it runs when the Raspberry Pi boots. There are also two JSON config files. hardwareConfig.json configures the hardware and connection settings, such as the server, subscribed topics, and serial port info. SpeakThis uses the port "/dev/ttyAMA0" on the Raspberry Pi (baudrate: 9600). The default MQTT topic that SpeakThis is subscribed to (the topic messages to be read are published to) is "SpeakThis", and status messages are published to "SpeakThis/report". messageConfig.json configures the default and validation settings used by the SpeakThis module. This includes the default settings for the different message properties, and the maximum and minimum allowed values for parameters.

The SpeakThis module reads JSON strings in the following format: '{"priority": <message priority (integer)>, "algorithm": <algorithm ID (integer)>, "speaker": <speaker ID (integer)>, "language": <language ID (integer)>, "volume": <message volume (integer)>, "rate": <speaking rate (integer)>, "text": <message to be spoken (string)> }'

Note: the keys can also be the first letter only of the normal keys (i.e. "volume" and "v" both work), and every property with the exception of the message to be spoken ("text") can be omitted. If omitted, default values are substituted, as defined in messageConfig.json. With regards to priority, the smaller the number, the earlier the message is spoken.

As an example, to read "Hello World" in English, at at the highest priority, with a volume of 5 dB, at a rate of 150 wpm, with the EPSON parser, using the default voice, publish '{"priority": 1, "algorithm": 1, "language": 0, "volume": 5, "rate": 150, "text": "Hello World"}' to the topic SpeakThis (or whatever you set it to).
