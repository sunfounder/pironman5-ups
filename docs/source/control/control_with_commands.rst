.. note::

    Hello, welcome to the SunFounder Raspberry Pi & Arduino & ESP32 Enthusiasts Community on Facebook! Dive deeper into Raspberry Pi, Arduino, and ESP32 with fellow enthusiasts.

    **Why Join?**

    - **Expert Support**: Solve post-sale issues and technical challenges with help from our community and team.
    - **Learn & Share**: Exchange tips and tutorials to enhance your skills.
    - **Exclusive Previews**: Get early access to new product announcements and sneak peeks.
    - **Special Discounts**: Enjoy exclusive discounts on our newest products.
    - **Festive Promotions and Giveaways**: Take part in giveaways and holiday promotions.

    👉 Ready to explore and create with us? Click [|link_sf_facebook|] and join today!

.. _ups_view_control_commands:

Control with Commands
========================================

In addition to viewing data from the Pironman 5 and controlling various devices through the Dashboard, you can also use commands to control them.

.. .. note::

..   * For the **Home Assistant** system, you can only monitor and control the Pironman 5 through the dashboard by opening the webpage at ``http://<ip>:34001``.
..   * For the **Batocera.linux** system, you can only monitor and control the Pironman 5 via commands. It is important to note that any changes to the configuration require a restart of the service using ``pironman5 restart`` to take effect.



The Basic Configurations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``pironman5`` module offers basic configurations for Pironman, which you can review with the following command.

.. code-block:: shell

  sudo pironman5 -c

The standard configurations appear as follows:

.. code-block:: 

  {
      "system": {
          "data_interval": 1,
          "debug_level": "INFO",
          "database_retention_days": 30,
          "temperature_unit": "C",
          "oled_enable": true,
          "oled_rotation": 0,
          "oled_sleep_timeout": 10,
          "shutdown_percentage": 10,
          "enable_history": true,
          "oled_pages": [
              "mix",
              "battery",
              "input",
              "rpi_power"
          ],
          "rgb_matrix_enable": true,
          "rgb_matrix_style": "rainbow",
          "rgb_matrix_color": "#0a1aff",
          "rgb_matrix_color2": "#00ff00",
          "rgb_matrix_speed": 50,
          "rgb_matrix_brightness": 100,
          "send_email_on": [
              "low_battery",
              "power_disconnected",
              "power_restored",
              "power_insufficient",
              "battery_critical_shutdown",
              "battery_voltage_critical_shutdown"
          ],
          "send_email_to": "",
          "smtp_email": "",
          "smtp_password": "",
          "smtp_server": "",
          "smtp_port": 465,
          "smtp_security": "ssl"
      }
  }

Customize these configurations to fit your needs.

Use ``sudo pironman5`` or ``sudo pironman5 -h`` for instructions.
qizn
.. code-block::

  usage: pironman5 [-h] [-v] [-c] [-drd [DATABASE_RETENTION_DAYS]]
                  [-dl [{DEBUG,INFO,WARNING,ERROR,CRITICAL,debug,info,warning,error,critical}]] [-rd] [-cp [CONFIG_PATH]]
                  [-eh [ENABLE_HISTORY]] [-u [{C,F}]] [-oe [OLED_ENABLE]] [-or [{0,180}]] [-op [OLED_PAGES]]
                  [-os [OLED_SLEEP_TIMEOUT]] [-rme [RGB_MATRIX_ENABLE]] [-rms [RGB_MATRIX_STYLE]] [-rmc [RGB_MATRIX_COLOR]]
                  [-rmc2 [RGB_MATRIX_COLOR2]] [-rmp [RGB_MATRIX_SPEED]] [-rmb [RGB_MATRIX_BRIGHTNESS]]
                  {pipower5} ... [{start,stop}]

  Pironman 5 UPS command line interface

  positional arguments:
    {start,stop}          Command

  options:
    -h, --help            show this help message and exit
    -v, --version         Show version
    -c, --config          Show config
    -drd [DATABASE_RETENTION_DAYS], --database-retention-days [DATABASE_RETENTION_DAYS]
                          Database retention days
    -dl [{DEBUG,INFO,WARNING,ERROR,CRITICAL,debug,info,warning,error,critical}], --debug-level [{DEBUG,INFO,WARNING,ERROR,CRITICAL,debug,info,warning,error,critical}]
                          Debug level
    -rd, --remove-dashboard
                          Remove dashboard
    -cp [CONFIG_PATH], --config-path [CONFIG_PATH]
                          Config path
    -eh [ENABLE_HISTORY], --enable-history [ENABLE_HISTORY]
                          Enable history, True/true/on/On/1 or False/false/off/Off/0
    -u [{C,F}], --temperature-unit [{C,F}]
                          Temperature unit
    -oe [OLED_ENABLE], --oled-enable [OLED_ENABLE]
                          OLED enable True/true/on/On/1 or False/false/off/Off/0
    -or [{0,180}], --oled-rotation [{0,180}]
                          Set to rotate OLED display, 0, 180
    -op [OLED_PAGES], --oled-pages [OLED_PAGES]
                          OLED pages, split by ',': mix,performance,ips,disk,battery,input,rpi_power
    -os [OLED_SLEEP_TIMEOUT], --oled-sleep-timeout [OLED_SLEEP_TIMEOUT]
                          OLED sleep timeout in seconds
    -rme [RGB_MATRIX_ENABLE], --rgb-matrix-enable [RGB_MATRIX_ENABLE]
                          RGB enable True/False
    -rms [RGB_MATRIX_STYLE], --rgb-matrix-style [RGB_MATRIX_STYLE]
                          RGB style: ['solid', 'breathing', 'rainbow', 'rainbow_reverse', 'spin', 'dual_spin', 'rainbow_spin',       
                          'shift_spin']
    -rmc [RGB_MATRIX_COLOR], --rgb-matrix-color [RGB_MATRIX_COLOR]
                          RGB color in hex format without # (e.g. 00aabb)
    -rmc2 [RGB_MATRIX_COLOR2], --rgb-matrix-color2 [RGB_MATRIX_COLOR2]
                          RGB color in hex format without # (e.g. 00aabb)
    -rmp [RGB_MATRIX_SPEED], --rgb-matrix-speed [RGB_MATRIX_SPEED]
                          RGB speed 0-100
    -rmb [RGB_MATRIX_BRIGHTNESS], --rgb-matrix-brightness [RGB_MATRIX_BRIGHTNESS]
                          RGB brightness 0-100

  Subcommands:
    {pipower5}



.. note::

  Each time you modify the status of ``pironman5.service``, you need to use the following command to make the configuration changes take effect.

  .. code-block:: shell

    sudo systemctl restart pironman5.service


* Verify the ``pironman5`` program status using the ``systemctl`` tool.

  .. code-block:: shell

    sudo systemctl status pironman5.service

* Alternatively, inspect the program-generated log files.

  .. code-block:: shell

    ls /var/log/pironman5/



Check the OLED Screen
-----------------------------------

When you have installed the ``pironman5`` library, the OLED screen displays CPU, RAM, Disk Usage, CPU Temperature, and the Raspberry Pi's IP Address, and it shows this every time you reboot.

If your OLED screen does not display any content, you need to first check if the OLED's FPC cable is connected properly.

Then you can check the program log to see what might be the problem through the following command.

.. code-block:: shell

  cat /var/log/pironman5/pironman5.log

Or check if the OLED's i2c address 0x3C is recognized:

.. code-block:: shell

  i2cdetect -y 1


Setting the OLED
-----------------------------------

You can manage OLED behavior through ``pironman5`` commands.

* **Enable or Disable OLED**

  .. code-block:: shell

     sudo pironman5 -oe on
     sudo pironman5 -oe off

* **Rotate OLED Display**

  Rotate the OLED by 0° or 180°:

  .. code-block:: shell

     sudo pironman5 -or 180

* **Set OLED Sleep Timeout**

  Configure sleep time to automatically turn off the OLED after inactivity (5–600 seconds):

  .. code-block:: shell

     sudo pironman5 -os 10
  
* **Set OLED Pages**

  You can configure which pages the OLED cycles through. Each option corresponds to an independent display page.  
  Multiple pages can be selected by separating them with commas.

  Available pages include:

  * ``mix`` : Displays a combined overview of CPU usage, IP addresses, and temperature.
  * ``performance`` : Shows performance metrics such as CPU, Temperature, RAM, Fan speed.
  * ``ips`` : Displays the current IP addresses of the Raspberry Pi.
  * ``disk`` : Shows disk usage and available storage.
  * ``battery`` : Displays battery percentage, charging/discharging status, and voltage.
  * ``input`` : Shows input power information.
  * ``rpi_power`` : Displays Raspberry Pi power state.

  Example:

  .. code-block:: shell

    sudo pironman5 -op mix,battery,rpi_power


Configure the RGB Matrix
-----------------------------------

The RGB matrix can be customized for style, colors, speed, and brightness.

* **Enable or Disable RGB**

  .. code-block:: shell

     sudo pironman5 -rme true
     sudo pironman5 -rme false

* **Set RGB Style**

  Available styles include: ``solid``, ``breathing``, ``rainbow``, ``rainbow_reverse``, ``spin``, ``dual_spin``, ``rainbow_spin``, and ``shift_spin``.

  .. code-block:: shell

     sudo pironman5 -rms rainbow

* **Set RGB Colors**

  Primary and secondary colors must be in hex format without ``#``. The secondary color is only used in some styles (Dual Spin).

  .. code-block:: shell

     sudo pironman5 -rmc ff0000
     sudo pironman5 -rmc2 00ff00

* **Adjust RGB Speed**

  Value range: 0–100.

  .. code-block:: shell

     sudo pironman5 -rmp 50

* **Adjust RGB Brightness**

  Value range: 0–100.

  .. code-block:: shell

     sudo pironman5 -rmb 80


The PiPower5 Configurations
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

PiPower5 Module is the part of the Pironman5 UPS System. It have some configurations which can be set by using the following commands.

Use ``sudo pironman5 pipower5 -h`` for instructions.

.. code-block::

  usage: pipower5 [-h] [-v] [-c] [-drd [DATABASE_RETENTION_DAYS]]
                  [-dl [{debug,info,warning,error,critical}]] [-rd]
                  [-cp [CONFIG_PATH]] [-sp [SHUTDOWN_PERCENTAGE]] [-iv] [-ic]
                  [-ov] [-oc] [-bv] [-bc] [-bp] [-bs] [-ii] [-ichg] [-do] [-sr]
                  [-pb] [-cc] [-a] [-fv] [-pfs [POWER_FAILURE_SIMULATION]]
                  [-seo [SEND_EMAIL_ON]] [-set [SEND_EMAIL_TO]]
                  [-ss [SMTP_SERVER]] [-smp [SMTP_PORT]] [-se [SMTP_EMAIL]]
                  [-spw [SMTP_PASSWORD]] [-ssc [SMTP_SECURITY]] [-bzo [BUZZ_ON]]
                  [-bzv [BUZZER_VOLUME]] [-bzt [BUZZER_TEST]] [-u [{C,F}]]
                  [{start,stop}]

  PiPower 5

  positional arguments:
    {start,stop}          Command

  options:
    -h, --help            show this help message and exit
    -v, --version         Show version
    -c, --config          Show config
    -drd [DATABASE_RETENTION_DAYS], --database-retention-days [DATABASE_RETENTION_DAYS]
                          Database retention days
    -dl [{debug,info,warning,error,critical}], --debug-level [{debug,info,warning,error,critical}]
                          Debug level
    -rd, --remove-dashboard
                          Remove dashboard
    -cp [CONFIG_PATH], --config-path [CONFIG_PATH]
                          Config path
    -sp [SHUTDOWN_PERCENTAGE], --shutdown-percentage [SHUTDOWN_PERCENTAGE]
                          Set shutdown percentage, leave empty to read
    -iv, --input-voltage  Read input voltage
    -ic, --input-current  Read input current
    -ov, --output-voltage
                          Read output voltage
    -oc, --output-current
                          Read output current
    -bv, --battery-voltage
                          Read battery voltage
    -bc, --battery-current
                          Read battery current
    -bp, --battery-percentage
                          Read battery percentage
    -bs, --battery-source
                          Read battery source
    -ii, --is-input-plugged_in
                          Read is input plugged in
    -ichg, --is-charging  Read is charging
    -do, --default-on     Read default on
    -sr, --shutdown-request
                          Read shutdown request
    -pb, --power-btn      Read power button
    -cc, --charging-current
                          Max charging current
    -a, --all             Show all status
    -fv, --firmware       PiPower5 firmware version
    -pfs [POWER_FAILURE_SIMULATION], --power-failure-simulation [POWER_FAILURE_SIMULATION]
                          Power failure simulation
    -seo [SEND_EMAIL_ON], --send-email-on [SEND_EMAIL_ON]
                          Send email on: ['battery_activated', 'low_battery',
                          'power_disconnected', 'power_restored',
                          'power_insufficient', 'battery_critical_shutdown',
                          'battery_voltage_critical_shutdown']
    -set [SEND_EMAIL_TO], --send-email-to [SEND_EMAIL_TO]
                          Email address to send email to
    -ss [SMTP_SERVER], --smtp-server [SMTP_SERVER]
                          SMTP server
    -smp [SMTP_PORT], --smtp-port [SMTP_PORT]
                          SMTP port
    -se [SMTP_EMAIL], --smtp-email [SMTP_EMAIL]
                          SMTP email
    -spw [SMTP_PASSWORD], --smtp-password [SMTP_PASSWORD]
                          SMTP password
    -ssc [SMTP_SECURITY], --smtp-security [SMTP_SECURITY]
                          SMTP security, 'none', 'ssl' or 'tls'
    -bzo [BUZZ_ON], --buzz-on [BUZZ_ON]
                          Buzz on: ['battery_activated', 'low_battery',
                          'power_disconnected', 'power_restored',
                          'power_insufficient', 'battery_critical_shutdown',
                          'battery_voltage_critical_shutdown']
    -bzv [BUZZER_VOLUME], --buzzer-volume [BUZZER_VOLUME]
                          Buzz volume
    -bzt [BUZZER_TEST], --buzzer-test [BUZZER_TEST]
                          Test buzzer on selected event.
    -u [{C,F}], --temperature-unit [{C,F}]
                          Temperature unit


.. note::

  To take effect the changes, restart the service:

  .. code-block:: shell

    sudo systemctl restart pironman5.service

Email Notification Configuration
-----------------------------------

The **Pironman 5** can send automated email notifications for system events such as power loss, low battery, or system shutdowns.  

* **Define Trigger Events**

  Possible trigger events include:

  - ``battery_activated``
  - ``low_battery``
  - ``power_disconnected``
  - ``power_restored``
  - ``power_insufficient``
  - ``battery_critical_shutdown``
  - ``battery_voltage_critical_shutdown``

  Example:

  .. code-block:: shell

     sudo pironman5 pipower5 -seo low_battery,power_restored

* **Configure Email Recipient**

  Specify the email address where notifications should be sent:

  .. code-block:: shell

     sudo pironman5 pipower5 -set user@example.com

* **Set SMTP Server Settings**

  Configure the SMTP server details to enable outgoing email:

  .. code-block:: shell

     sudo pironman5 pipower5 -ss smtp.example.com
     sudo pironman5 pipower5 -smp 465
     sudo pironman5 pipower5 -se user@example.com
     sudo pironman5 pipower5 -spw password123
     sudo pironman5 pipower5 -ssc ssl


Buzzer Configuration
-----------------------------------

The buzzer provides audible alerts for critical system events. It can be customized to trigger on specific conditions and adjusted for volume.

* **Enable Buzzer on Events**

  Define which events should trigger the buzzer:

  .. code-block:: shell

     sudo pironman5 pipower5 -bzo low_battery,power_disconnected

* **Adjust Buzzer Volume**

  Set buzzer volume from ``0`` (mute) to ``10`` (maximum):

  .. code-block:: shell

     sudo pironman5 pipower5 -bzv 5

* **Test Buzzer**

  Simulate a buzzer event for testing purposes:

  .. code-block:: shell

     sudo pironman5 pipower5 -bzt low_battery



System Monitoring Options
-----------------------------------

PiPower5 provides commands to monitor system power and battery status.

* **Power Input and Output**

  - Read input voltage:  

    .. code-block:: shell

       sudo pironman5 pipower5 -iv

  - Read input current:  

    .. code-block:: shell

       sudo pironman5 pipower5 -ic

  - Read output voltage:  

    .. code-block:: shell

       sudo pironman5 pipower5 -ov

  - Read output current:  

    .. code-block:: shell

       sudo pironman5 pipower5 -oc

* **Battery Monitoring**

  - Read battery voltage:  

    .. code-block:: shell

       sudo pironman5 pipower5 -bv

  - Read battery current:  

    .. code-block:: shell

       sudo pironman5 pipower5 -bc

  - Read battery percentage:  

    .. code-block:: shell

       sudo pironman5 pipower5 -bp

  - Read battery source:  

    .. code-block:: shell

       sudo pironman5 pipower5 -bs

* **System Status Checks**

  - Check if input is plugged in:  

    .. code-block:: shell

       sudo pironman5 pipower5 -ii

  - Check if battery is charging:  

    .. code-block:: shell

       sudo pironman5 pipower5 -ichg

  - Read shutdown request status:  

    .. code-block:: shell

       sudo pironman5 pipower5 -sr

  - Check power button status:  

    .. code-block:: shell

       sudo pironman5 pipower5 -pb


Advanced Configuration
-----------------------------------

* **Shutdown Percentage**

  Define the battery percentage threshold for automatic system shutdown:

  .. code-block:: shell

     sudo pironman5 pipower5 -sp 15

* **Power Failure Simulation**

  Test system behavior during simulated power failures:

  .. code-block:: shell

     sudo pironman5 pipower5 -pfs 1

* **Temperature Unit**

  Switch between Celsius and Fahrenheit for temperature readings:

  .. code-block:: shell

     sudo pironman5 pipower5 -u C
     sudo pironman5 pipower5 -u F

* **Firmware Version**

  Display the current PiPower5 firmware version:

  .. code-block:: shell

     sudo pironman5 pipower5 -fv