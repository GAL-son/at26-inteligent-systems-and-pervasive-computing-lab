# Lab 1

### Prerequisites
* Mosquitto broker
* Node-RED

## Instruction
1. Start Mosquitto broked - by default it runs on `localhost:1883`.
2. In Node-RED, create three "Inject" nodes with the numerical values ​​21, 25, and 28.
3. Connect them to an "MQTT out" node. Configure it to use the broker at `localhost:1883` and specify a topic, for example: `agh/<student-index-number>/room/temperature`.
4. Add an "MQTT in" node subscribed to the same topic, followed by a "JSON" node and a "Debug" node.
5. Add a "Switch" node with the condition: `msg.payload > 26`. Connect the output of this condition to a second "Debug" node, labeled "ALARM: temperature too high."
6. Click each of the "Inject" nodes. Readings should appear in the "Debug" panel, with the alarm triggering only for the value 28.
7. Change the threshold to 24 and observe how the system's behavior changes.

In this way, you have a complete workflow: measurement → MQTT message → decision in Node-RED → alarm.