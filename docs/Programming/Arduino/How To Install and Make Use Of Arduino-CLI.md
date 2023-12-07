---
title: How To Install and Make Use Of Arduino-CLI
tags:
  - arduino
date: 07.12.2023
---
# How To Install and Make Use Of Arduino-CLI

[Arduino CLI](https://arduino.github.io/arduino-cli/0.35/) is an All-in-One solution for managing libraries and boards, building and uploading sketches and many other tools. It can be used for all boards and platforms compatible with Arduino. It enables you to control them via the command line.

Arduino CLI is not only a stand-alone tool, but also the heart of all official Arduino development software (Arduino IDE, Arduino Web Editor). 

# Installation of the Arduino-CLI on macOS

On macOS it‘s most convenient to make use of [HomeBrew](https://brew.sh) to install the Arduino-CLI:

```bash
brew update
brew install arduino-cli
```

## Validating the installation

To check the installation you can let `arduino-cli` print its version number in the terminal:

```bash
arduino-cli version                                                              arduino-cli  Version: 0.34.2 Commit: 963c1a76 Datum: 2023-09-11T09:50:44Z
arduino-cli help core                                                            

Arduino-Kern-Operationen.
Usage:
  arduino-cli core [command]
  
Beispiele:
  arduino-cli core update-index

...

```

The Arduino CLI consists of a single executable file. By default, the Arduino CLI uses three directories for its functionalities.

**data** - for storing all configurations and platform files
**downloads** - for downloading installation packages, libraries, etc.
**user** - where the libraries and sketches are stored

The paths to these directories are determined by:

```bash
arduino-cli config dump                                                          board_manager:
  additional_urls: []
build_cache:
  compilations_before_purge: 10
  ttl: 720h0m0s
daemon:
  port: "50051"
directories:
  data: /Users/<user>/Library/Arduino15
  downloads: /Users/<user>/Library/Arduino15/staging
  user: /Users/<user>/Documents/Arduino
library:
  enable_unsafe_install: false
logging:
  file: ""
  format: text
  level: info
metrics:
  addr: :9090
  enabled: true
output:
  no_color: false
sketch:
  always_export_binaries: false
updater:
  enable_notification: true
```

The data folder is the same as the [Arduino IDE](https://www.arduino.cc/en/software) folder where all your platforms and configurations are stored. This **path should not be changed**. The download directory can also be retained. However, the user directory may need to be changed if you want to use already installed libraries and created sketches. To do this, a configuration file must be created specifically for the Arduino CLI. This can be done with the following command:

```bash
arduino-cli config init
```

This simply creates a new YAML file in the data directory. This file has the same content as when you execute the `config dump` command. You can then open the file in a text editor and change the user directory to the existing sketches directory and save it. 

In my case this folder is set tof:  `~/Development/personal/arduino/Sketches`. 

To test whether the configuration works, simply try to list the installed libraries with the following command:

```bash
arduino-cli lib list                                                             Name                   Installiert   Verfügbar Ort                Beschreibung   
Arduino_TensorFlowLite 2.4.0-ALPHA   -         LIBRARY_LOCATION_USER   -         
ezTime                 0.8.3         -         LIBRARY_LOCATION_USER.  -         
M5ez                   2.3.0         -         LIBRARY_LOCATION_USER   -       
M5Stack                0.4.6         -         LIBRARY_LOCATION_USER.  -
```

## Building a sketch

The target board must be known in order to translate a sketch. The **Fully Qualified Board Name (FQBN)** is decisive. The following command is used to determine the FQBN of a board connected via USB:

```bash
arduino-cli board list                                                           Port                            Protokoll Typ               Platinenname        FQBN                        Kern             
/dev/cu.Bluetooth-Incoming-Port serial    Serial Port       Unbekannt           
/dev/cu.usbmodem2101            serial    Serial Port (USB) Arduino Nano 33 BLE arduino:mbed:nano33ble      arduino:mbed     
                                          Serial Port (USB) Arduino Nano 33 BLE arduino:mbed_nano:nano33ble arduino:mbed_nano
```

In this example an **Arduino Nano 33 BLE** is connected via USB.
The `compile` command gets called from the parent directory using the specification of:

- FQBN of the target board
- Name of the sketch

```bash
arduino-cli compile --fqbn arduino:mbed:nano33ble Blink                         

Der Sketch verwendet 84280 Bytes (8%) des Programmspeicherplatzes. Das Maximum sind 983040 Bytes.

Globale Variablen verwenden 43968 Bytes (16%) des dynamischen Speichers, 218176 Bytes für lokale Variablen verbleiben. Das Maximum sind 262144 Bytes.

Used platform Version Pfad                                                      
arduino:mbed  3.3.0   /Users/<user>/Library/Arduino15/packages/arduino/hardware/mbed/3.3.0
```