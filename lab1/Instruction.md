# Lab 1

### Prerequisites
* Mosquitto broker
* Node-RED

## Task 1
1. Start Mosquitto broked - by default it runs on `localhost:1883`.
2. In Node-RED, create three "Inject" nodes with the numerical values ​​21, 25, and 28.
3. Connect them to an "MQTT out" node. Configure it to use the broker at `localhost:1883` and specify a topic, for example: `agh/<student-index-number>/room/temperature`.
4. Add an "MQTT in" node subscribed to the same topic, followed by a "JSON" node and a "Debug" node.
5. Add a "Switch" node with the condition: `msg.payload > 26`. Connect the output of this condition to a second "Debug" node, labeled "ALARM: temperature too high."
6. Click each of the "Inject" nodes. Readings should appear in the "Debug" panel, with the alarm triggering only for the value 28.
7. Change the threshold to 24 and observe how the system's behavior changes.

In this way, you have a complete workflow: measurement → MQTT message → decision in Node-RED → alarm.

## Task 2
1. To current flow add automatic fan control.
    * Temperature ≥ 26°C → send `ON` message to topic `at/<student-index>/room/<room-number>/fan`;
    * Temperature ≤ 24°C → send `OFF` to the same topic;
    * If temperature is in ragne between 24°C and 26°C → don't change fan state.

2. Test system by sending different temperatures via `inject` nodes.
Use given values in order to test every possible fan state transition:
    > 23, 27, 25, 24

3. Expected result
    > ON, OFF, No change, ON

## Task 3
Lighting control based of presence and luminosity

Instead of sending raw values like temperatures via MQTT we could send json payload.

1. Prepare Node-RED flow that will send JSON message via MQTT:
    ```json
    {"presence": true, "lux": 100}
    ```

2. Next receive this message and based on that turn the light off or on:
    * **ON** → when there is someone inside the room - `presence == true` and room luminosity is below 200 - `lux < 200`
    * **OFF** → when room is empty or the luminance goes over 300lx
    * **NO CHANGE** → don't the room change if its not empty and luminance is btween 200 and 300lx stan.

3. Send those signals via **MQTT** on topic `at/<student-index>/room/<room-number>/light`.
4. Receive those signals and display them via **debug**