.. zephyr:code-sample:: espnow
   :name: ESP-NOW
   :relevant-api: thread_apis

   Send and receive ESP-NOW broadcast heartbeat beacons between two ESP32 boards.

Overview
********

This sample demonstrates the ESP-NOW peer-to-peer radio protocol on Espressif
ESP32 boards running Zephyr. No Wi-Fi access point is required — both devices
lock to the same channel and communicate directly.

The device role is selected at build time:

- **BIDIR** (default) — transmits a heartbeat beacon every
  ``CONFIG_ESPNOW_BEACON_INTERVAL_S`` seconds *and* prints all received beacons.
  Flash two boards with this role for a loopback test.
- **SENDER** — beacon thread only; ignores received frames.
- **RECEIVER** — RX callback only; no beacon thread.

Each heartbeat carries: sequence counter, uptime in milliseconds, and the
sender's MAC address. Total wire size is 14 bytes.

Requirements
************

- Two ESP32 DevKit boards connected via USB.
- Both boards must use the same ``CONFIG_ESPNOW_CHANNEL`` (default: 1).

Building and Running
********************

Flash the default **BIDIR** role to two boards (ESP32-S3 shown; substitute
``esp32c6_devkitc/esp32c6/hpcore`` for an ESP32-C6):

.. zephyr-app-commands::
   :zephyr-app: samples/boards/espressif/espnow
   :board: esp32s3_devkitc/esp32s3/procpu
   :goals: build flash
   :compact:

To build as receiver only:

.. code-block:: bash

   west build -b esp32s3_devkitc/esp32s3/procpu -- -DCONFIG_ESPNOW_ROLE_RECEIVER=y

Sample Output
*************

Sender side::

   [00:00:00.186,000] <inf> espnow_sample: ESP-NOW v2 ready  ch=1  role=BIDIR
   [00:00:00.186,000] <inf> espnow_sample: Beacon thread started  interval=5s
   [00:00:00.186,000] <inf> espnow_sample: TX beacon seq=0  uptime=186 ms  mac=24:d7:eb:55:87:8c
   [00:00:00.188,000] <inf> espnow_sample: TX OK  → ff:ff:ff:ff:ff:ff

Receiver side::

   [00:00:05.034,000] <inf> espnow_sample: RX [BCAST] from 24:d7:eb:55:87:8c  rssi=-42  len=14
   [00:00:05.034,000] <inf> espnow_sample:   heartbeat seq=0  uptime=5001 ms  src=24:d7:eb:55:87:8c
