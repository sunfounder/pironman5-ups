7. Power Management
============================

Power Button Shutdown
----------------------

* **Short press**: Powers on the Raspberry Pi, wakes up the OLED display, or cycles through different OLED display modes.
* **Long press (2 seconds)**: Initiates a safe shutdown of the Raspberry Pi.
* **Long press (5 seconds)**: Forces an immediate power cut (hard shutdown).

.. note::  
   The 5-second long press is equivalent to physically unplugging the power supply. It should only be used when the system is unresponsive, as it may cause data loss or file system corruption.


-----------------------------------------------------

Power-Off After Raspberry Pi Shutdown
--------------------------------------

PiPower5 can detect when the Raspberry Pi has shut down and automatically cut power afterwards.

- Press and hold the PiPower button for 2 seconds (or when the battery is low) → Raspberry Pi shuts down → PiPower5 powers off.  
- Double-press the Raspberry Pi’s power button → Raspberry Pi shuts down → PiPower5 powers off.  
- Shut down from the Raspberry Pi desktop menu → PiPower5 powers off.  
- When the battery level drops below the configured shutdown percentage, PiPower5 notifies the Raspberry Pi to shut down. After shutdown, PiPower5 cuts power.  

.. note::  
   To use this feature, ensure that the **SDSIG jumper** on PiPower5 is connected to **PI3V3**.

Run the following command:

.. code-block:: shell

   sudo raspi-config

Navigate to:  
**6 Advanced Options → A11 Shutdown Behaviour → B1 Full power off**  
(*Switch off Pi power management (PMIC) outputs on Pi4 / Pi5 - recommended*).

Reboot when prompted after exiting `raspi-config`.


-----------------------------------------------------

Auto Power-On When Power is Connected
--------------------------------------

PiPower5 is configured by default in **Default On** mode, meaning it powers on automatically when external power is supplied.  
If this behavior is not desired, move the *Default On* jumper to the **OFF** position.


-----------------------------------------------------

Battery Shutdown Percentage
----------------------------

When running only on battery power and the battery voltage drops below the configured **Shutdown Percentage**, PiPower5 sends a **LOW BATTERY Shutdown Request** via I²C.  

- The host system can read this Shutdown Request signal.  
- When LOW BATTERY is detected, the system can safely shut down.  
- After shutdown, PiPower5 pulls the **SDSIG** pin high and cuts power.  

This ensures a controlled shutdown sequence on low battery. On Raspberry Pi, installing the **pipower5 library** automatically sets up the required service for safe shutdown.  

You can configure the shutdown percentage via:

- CLI command:  

  .. code-block:: shell

     pipower5 -sp [value]

- Or from the **Dashboard settings page**.

.. note::  
   On Raspberry Pi 5, if the power draw is high (e.g., >3A), the battery cannot sustain long-term operation. In this case, it is recommended to set the Shutdown Percentage to **100%**, so the Raspberry Pi shuts down immediately when external power is lost, protecting both the device and your data.



-----------------------------------------------------

Power Failure Simulation
--------------------------

The **Power Failure Simulation** feature allows you to test whether the system can remain powered during outages and how well the battery performs under load.  

**Preparation before testing**:  

- Back up important data.  
- Close sensitive applications or services.  
- Ensure the battery is charged to **at least 80%** and external power is connected.  

You can start the test in two ways:  

- **Dashboard**: Settings → *Power Failure Simulation* → follow on-screen instructions.  
- **CLI**:  

  .. code-block:: shell

     pipower5 -pfs

The test uses onboard MOSFETs to disconnect external input. Battery status is monitored for 1 minute, after which power is restored automatically. A simple report is generated, summarizing battery performance and estimated runtime.


-----------------------------------------------------

Desktop Battery Indicator
--------------------------

Once the PiPower5 service is installed and running, it registers as a system battery device.  
This enables a **battery icon in the system tray**, showing charge percentage and charging status.  

The device path is:

.. code-block:: shell

   /dev/pipower5


-----------------------------------------------------


Shutdown Alerts (Buzzer)
--------------------------

PiPower5 supports buzzer notifications, allowing audible alerts for specific events.

1. Configure buzzer events via **Dashboard** or **CLI**. When an event occurs, the buzzer plays the corresponding sound.  
2. Adjust buzzer volume from **1–10**.  
3. Preview event sounds to recognize them easily.  
4. Advanced: Create **custom sound effects**.  

   Steps:  
   (1) Open the configuration file:  

   .. code-block:: shell

      /opt/pipower5/venv/lib/python3.11/site-packages/pipower5/config.json

   (2) Locate the **pipower5_buzz_sequence** section.  
   (3) Each effect is a list entry in the format: `[action, duration]`.  
   (4) **Action** can be:  

       - A musical note (e.g., `"A4"`, `"D3"`, `"C#4"`).  
       - A frequency (integer).  
       - `"pause"` (silence).  

   (5) **Duration** is the length in milliseconds (ms).  



-----------------------------------------------------


Shutdown Alerts (Email)
--------------------------

PiPower5 software can also send **email notifications** for specific events.

1. You need an SMTP server. Most email providers support this.  

   - For Gmail, simply create an **App Password** (no extra SMTP activation required).  
   - For other providers, enable SMTP and generate a dedicated SMTP password (separate from your account password).  

2. Gather the following information:  

   (1) **SMTP server address** (e.g., `smtp.gmail.com`).  
   (2) **SMTP port** (e.g., `465`, `25` depending on encryption).  
   (3) **Encryption type** (None / SSL / TLS).  
   (4) **SMTP account** (usually your email address).  
   (5) **SMTP password** (App password or dedicated SMTP password).  

3. Enter this information via **Dashboard** or **CLI**.  
4. Configure the recipient email address.  

   - PiPower5 uses the SMTP server to log into your account and send notifications.  
   - You can use the same email as both sender and recipient (sending mail to yourself is valid).  

5. After setup, run the **test command** to verify connection with the SMTP server.
