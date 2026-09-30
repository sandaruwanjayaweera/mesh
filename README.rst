.. zephyr:code-sample:: ble_mesh
   :name: Mesh
   :relevant-api: bt_mesh bluetooth

   Publish custom packets over Bluetooth Mesh using the Zephyr Bluetooth Mesh stack.

Overview
********

This project is based on the standard Zephyr Bluetooth Mesh sample, but it has
been adapted for a custom testing setup. In particular, I implemented a mesh
node that can send Generic OnOff Set messages that carry
an additional 29-byte CAM-style payload for testing and debugging.


On boards with LEDs, the Generic OnOff Server model controls the first LED over
the mesh. On boards with buttons, the Generic OnOff Client sends OnOff
messages to all nodes in the network when the button is pressed.

Custom work included in this project
***********************************

- Extended the OnOff message payload with a 29-byte test CAM sequence to test
  non-standard metadata transmission through the mesh network.
- Kept the LED/button interaction and mesh status logging intact so the node can
  be used as a simple Bluetooth Mesh device for validation and debugging.

Requirements
************

* A board with Bluetooth LE support, or
* QEMU with BlueZ running on the host

Building and Running
********************

See :zephyr:code-sample-category:`bluetooth` samples for details on how
to run the sample inside QEMU.

For other boards, build and flash the application as follows:

.. zephyr-app-commands::
   :zephyr-app: samples/bluetooth/mesh
   :board: <board>
   :goals: flash
   :compact:

Refer to your :ref:`board's documentation <boards>` for alternative
flash instructions if your board doesn't support the ``flash`` target.

To run the application on an :zephyr:board:`nrf5340dk`, a Bluetooth controller application
must also run on the network core. The :zephyr:code-sample:`bluetooth_hci_ipc` sample
application may be used. Build this sample with configuration
:zephyr_file:`samples/bluetooth/hci_ipc/nrf5340_cpunet_bt_mesh-bt_ll_sw_split.conf`
to enable mesh support.

Interacting with the sample
***************************

The sample can either be provisioned into an existing mesh network with an
external provisioner device, or self-provision through a button press.

When provisioning with a provisioner device, the provisioner must give the
device an Application key and bind it to both Generic OnOff models.

When self-provisioning, the device will take a random unicast address and
bind a dummy Application key to these models.

Once provisioned, messages to the Generic OnOff Server will be used to turn
the LED on or off, and button presses will be used to broadcast OnOff
messages to all nodes in the same network.
