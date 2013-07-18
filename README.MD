# osPID Front-End

## Why the fork?
This is the Front-End required to view and send profile types 4 and 5, (described below) to an osPID controller running [hairykiwi/osPID-Firmware](https://github.com/hairykiwi/osPID-Firmware) v1.61j and later.

- Type 4: **step-output-period** - set the power Output (%) to a specified level for a specified period.
- Type 5: **step-output-until\_crossing\_temp** - set the power Output (%) to a specified level until a specified Input temperature is attained.
  
Both the above profile types can be mixed with any of Brett's original PID profiles - 1 Ramp, 2 Wait, 3 Step or 127 Buzz.

## Preamble
The following notes were created primarily for personal reference. Some content is copied or derived from the original instructions, found here: [osPID.com Front-End Software](http://www.ospid.com/docs/index.php?title=Front-End_Software)

## About
This application is a control panel designed to connect with an [osPID](http://www.ospid.com/) controller over a USB virtual com port (VCP).

When connected, PID performance can be observed and adjusted in real time. Additionally, a number of controller settings not directly accessible via the osPID controller menu can be adjusted.

The [original osPID Front-End](https://github.com/osPID/osPID-Front-End/) was created by Brett Beauregard. It is written in [Processing](http://processing.org/).

## Dependencies
The following software is required prior to running the application:

- [processing-1.5.1](https://processing.org/download/?processing) - perhaps later versions will also work, but this does the job.
- [controlP5_0.5.4](http://www.sojamo.de/libraries/controlP5/) - see install instructions below.
- [FTDI Virtual Com Port (VCP) driver](http://www.ftdichip.com/Drivers/VCP.htm) - see note below.

The dependencies listed above are the only versions tested with _this_ version of the osPID Front-End.

### Installing controlP5

If it does not already exist, create the **libraries** folder according to this folder structure:

    Processing
      libraries
        controlP5
          examples
          library
            controlP5.jar
          reference
          src

The Processing (sketches) folder can be found at the location according to the bold text below. The full path to the controlP5.jar file, (as above) has also been added below for completeness:

- Mac: **~/Documents/Processing**/libraries/controlP5/library/controlP5.jar
- Windows: **%Userprofile%\Documents\Processing**\libraries\controlP5\library\controlP5.jar

After library controlP5 has been unzipped to the library folder, restart Processing.

Although the controlP5 website claims controlP5-1.5.2 is the correct version to use with Processing v1.5.1, known code compatibility issues do exist. (These notes are an edit of the install instructions found in the controlP5-1.5.2 zip file as the install notes in 0.5.4 are non-existent.)

### FTDI driver notes

From the FTDI Install manual: "FTDI have previously provided two types of driver for Windows OS: a D2XX direct driver and a virtual COM port (VCP) driver. Previously, these drivers were mutually exclusive and could not be installed at the same time. The new Windows combined driver model (CDM) which may be installed on Windows 2000, XP, VISTA or Windows 7 allows applications to access FTDI devices through either the D2XX DLL or a COM port without having to change driver type. However, it should be noted that an application can only communicate through one of these interfaces at a time and cannot send commands to the D2XX DLL and the associated COM port at the same time."

## Usage

### Connecting the Front-End application to an osPID controller

The frontend scans available ports only on initial startup. For this reason connect the osPID via USB before starting the application.

- Running from code in Processing is the preferred method - especially while developing. (No compiled versions of this front-End have been built.)
- Only power the osPID controller via the same USB cable used form comms; the power socket at the rear of the osPID should NOT be used, as earth-loop glitches are suspected to cause communications errors.
- For convenience, configure the osPID_FrontEnd.pde file to open in Processing 1.5.1 when double-clicked.
- After the application starts, select the correct COM port, then click CONNECT. Under Windows, this can be determined by using Device Manager to visually detect a change in the active USB ports - by connecting and disconnecting the osPID controller.

#### Connection issues
Occasionally, the application fails to connect to the osPID. if so, disconnect the controller, close and restart the application right after reconnecting the controller.
Under the PROFILE tab, a profile will occasionally fail to load. If so, disconnect the controller and restart as above.

The above two problems were regularly seen while using the application on an older laptop running WinXP.

### Reflow soldering

The following notes detail the setup used to reflow solder the [OTM-02](https://github.com/hairykiwi/OTM-02) prototype PCB. The reflow profile used is [here](https://github.com/hairykiwi/osPID-Front-End/blob/master/osPID_FrontEnd/profiles/RoHS_Elgento024.txt). Images and more project details are [here](http://forum.energymicro.com/topic/582-otm-02-open-source-time-machine-2/?p=3518).

**Note:** The above profile should be considered a starting point - be sure to thoroughly test it with your own equipment prior to using it for real!

Prior to connecting the osPID controller, ensure Set Point (SP) is set to 0°C, Mode: AUTOMATIC, and ensure that the Output is ZERO prior to commencing a reflow cycle.

PID settings saved in the controller are not used and have no impact if using Type 5 'step-output-until\_crossing\_temp' profile types, to construct an entire profile.

#### Reflow soldering equipment
**Mini-Oven:** Elgento E048  - 9 litre, 800W / 250°C - [ASDA online](http://direct.asda.com/Elgento-E048-9L-Mini-Oven/001575061,default,pd.html) (UK)

**Thermocouple:** Fine wire, Type K, PTFE 1m - RS part number [409-4908](http://uk.rs-online.com/web/p/thermocouples/4094908/)

**Solid State Relay:** CKELE N4825D SSR 3-32VDC 48-480VAC 25A DC 3-25mA (eBay)

## Known Bugs
1. When a profile is loaded, it's name should be displayed in the Front-End Status window. It is currently possible to load a profile, view another, and then start the loaded one. This can lead to significant confusion if the profiles are 'similar, but different'.
2. For convenience (and added safety) a run/stop control should be displayed under the RUN tab.
4. If, when a profile begins, the Input temperature is already above the 'until\_crossing\_temp', (as stored in a Type 5 profile) and then Output, as specified in the same profile is applied, oven temperature run-away will almost certainly occur. Some logic similar to that in the Wait profile should solve this.

## Minutiae
**Version:** (hairykiwi) 0.61

**Licence:** GPLv3

## Credits
This Firmware is based on:

    /********************************************************
    * osPID Front-End Version 0.6
    * by Brett Beauregard
    * License: GPLv3
    * August 2012
    ********************************************************/