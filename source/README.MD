thruster_config
===============
This tool allows for the configuring of VideoRay MSS type thrusters.


Usage
-----
The application supports the following command line options (in order of
precedence):

Option            Short  Parameter   Brief Description
----------------  -----  ----------  ------------------------------------------
help                h                Show the usage message.
version             v                Show the version information.
port                p    Port Name   Active serial port.
log_configuration   c    File Name   Log configuration file.
minimum_level       l    Level Name  Send log messages to the console and
                                     specify the minimum message level.
show_port                            Show the on screen serial port.
show_status                          Show the on screen thruster interface
                                     status.
setup_ini                File Name   Generate a template configuration (INI)
                                     file populated with default values.
setup_log                File Name   Generate a template log configuration file
                                     populated with suggested values.
emulate             e    Boolean     Emulate hardware.

Help and Version Options
The "help" and "version" options terminate the program after their messages
are displayed. The "help" option displays a usage message and the locations
that will be searched for the log configuration file. The "version" option
will display "version" information for the application.

Setup Options
The "setup_ini" and "setup_log" options direct all log messages to the console
and terminate the program after the template file(s) are generated. The file
that the "setup_ini" option generates contains all of the settings populated
with their default values. The file that the "setup_log" option generates
contains definitions for common logging appenders and layouts in addition to
all of the loggers, populated with suggested default values.

Logging Options
Only one of the "log_configuration" and "minimum_level" options should occur
on the command line. If no logging options occur on the command line, the
default log configuration file will be used if it exist, otherwise log messages
will be sent to the console.

If the name of the log configuration file does not begin with a slash (or
drive letter and slash on Microsoft Windows), the program will search for the
log configuration file and will use the first one it locates. See the "help"
option message for the locations that will be searched.

On Screen Display Options 
The "show_port" option toggles if the active serial port is displayed the
main window. The "show_status" option toggles if the thruster interface status
is displayed on the main window.

Emulation Option
The "emulate" option controls if the application emulates the hardware using
the settings in the [emulation_delays], [emulation_formulas],
[emulation_serial_ports], and [emulation_thrusters] sections of the
configuration files.

Port Option
The "port" option controls the serial port used to communicate with the
thrusters. If neither this option nor the "[default_options] port"
configuration setting is provided, or if the supplied value is invalid,
the default serial port is the first (in alphabetic order) serial port.
Whenever the active serial port is changed, the new serial port will be written
to the "[default_options] port" setting so that it becomes the default the next
time the application is run.


Configuration Files
-------------------
The application reads configuration values (except the log configuration)
from the following files:
- User and application specific settings.
  - windows: C:\Users\{User Name}\AppData\Roaming\VideoRay\thruster_config.ini
  - linux: $HOME/.config/VideoRay/thruster_config.ini
- User specific settings for all VideoRay applications.
  - windows: C:\Users\{User Name}\AppData\Roaming\VideoRay.ini
  - linux: $HOME/.config/VideoRay.ini
- Common application specific settings.
  - windows: C:\ProgramData\VideoRay\thruster_config.ini
  - linux: /etc/xdg/VideoRay/thruster_config.ini
- Common settings shared by all VideoRay applications.
  - windows: C:\ProgramData\VideoRay.ini
  - linux: /etc/xdg/VideoRay.ini

Any changes that the application makes to the configuration values will be
written to the user and application specific settings file. The other files
can be used to override the default value(s).

The "default.ini" file contains the default values for all of the settings.
See the --setup_ini command line option above.


Supported Environments
----------------------
This tool is supported in the following environments:
- Windows 7
- Windows 10
- Ubuntu 14.04 LTS
- Ubuntu 16.04 LTS


Notes
-----
- Under Ubuntu, ensure the user is in the dialout group.
    sudo usermod -a -G dialout $USER
- Under Ubuntu, the "libstdc++.so.6" file is only needed for 14.04 LTS.
- Ensure that the directories that the log configuration file refers to
  exist because the logging system silently discards log messages if the
  destination directory does not exist.
- Under Windows, there is a known problem with accessing the serial port.
  To work around the problem perform either of the following. This only
  needs to be performed once after rebooting or attaching the USB serial
  port.
  - Use a third party application (e.g. PuTTY, Realterm, or Tera Term)
    to open the serial port. 
  - Execute the supplied python script (windows\serialPort.py)
      python serialPort.py


Know Issues / To Do
-------------------
1.  Add support for a 'Set Defaults' push button that will write
    'default' values to the selected thruster after user verification.

2.  Add support for standard configuration modeless dialog that will
    display an 'image' of the selected vehicle that the user can click
    on to display the standard configuration values for each thruster
    and optionally copy the values to the main window's edit fields
    so that they can be written to the thruster.

3.  Restore the selected node after changing its IDs.


History
-------
1.0.7 (2017-10-27)
Reworked scanning for thrusters.
Added show_port and show_status command line options and settings.

1.0.6 (2017-10-26)
Restored prior handing of protocol errors (except the log level).

1.0.5 (2017-10-26)
Corrected MSIS issues:
1.  Rename 'Read' push button as 'Discard changes' and swap with the
    'Write' push button.

2.  Show minimum and maximum values and units for user editable fields.

3.  Disable group ID bit (0x80) processing and changed limits to be
    [128, 254].

4.  Reworked configuration control enable/disable processing.
    A.  Enable 'Discard changes' only if the configuration values do
        not match the last read values.

    B.  Enable 'Write' only if there the configuration values have been
        changed and they all are valid.

    C.  Enable 'Test' only if there are no changes to the configuration
        values and they are all valid.

    D.  Disable the all of the configuration controls if the selected
        thruster has an invalid node ID or group ID.

5.  Ignore duplicate node IDs and serial numbers if they do not affect
    a thruster (e.g. two LEDs with the same node ID that does not
    duplicate any thruster node ID).

1.0.4 (2017-10-24)
Added support to change IDs if there are no duplicate serial numbers.
ource
1.0.3 (2017-10-13)
Changed test mode speed edit box to a spin box and changed value to be a a
percentage.

Reworked header prefix for special include files to avoid doxygen warnings.

1.0.2 (2017-10-12)
Added support to handle factor default IDs.

1.0.1 (2017-10-10)
Changed application name under Linux.

1.0.0 (2017-10-09)
Initial release.

