# smartalert

smartalert - check the health status of your hard drives

This script checks the list of smart attributes for errors.
It also checks for smart errors.
A positive exit code is returned if errors have been found.

This is a zero-configuration script, meaning it requires no configuration.
It also means that no configuration is possible.

This was written for personal use only.
Use at your own risk.
Don't rely on this script if you're not sure.
Use smartd instead.



INSTALLATION
------------

Just add a root cronjob to call this script.



USAGE
-----

If called without arguments, all detected hard drives will be checked.
Only errors will be printed by default.
If no errors are found, nothing will be printed, exit code will be 0.
Use option -v to get verbose output (for debugging only).
Use option -l to list drives and skip the rest.

```
# ./smartalert -l
 /dev/sda
 /dev/sdb
 /dev/sdc
```

A list of drives can be provided to skip the drive scan.
Short names are allowed, like sda for /dev/sda.

```
# smartalert sdc sdg
2 error(s) on sdg:
- Runtime_Bad_Block has NONZERO value 2 !
- Command_Timeout has NONZERO value 1 !

1 error(s) on sdc:
- UDMA_CRC_Error_Count has NONZERO value 744 !
```

A text file with smart attributes (output of smartctl -A ...) can be provided.
In this case, the drive is obviously not checked for additional errors.

```
# smartctl -A /dev/sdc >/tmp/foo; ./smartalert /tmp/foo
1 error(s) on /tmp/foo:
- UDMA_CRC_Error_Count has NONZERO value 744 !
```

Good smart attributes but smart error:

```
# smartctl
FATAL: SMART ERROR ON: /dev/sdb
smartctl 6.2 2014-07-16 r3952 [i686-linux-3.19.1-201.fc21.i686] (local build)
Copyright (C) 2002-13, Bruce Allen, Christian Franke, www.smartmontools.org

=== START OF READ SMART DATA SECTION ===
SMART Error Log Version: 1
ATA Error Count: 1
    CR = Command Register [HEX]
    FR = Features Register [HEX]
    SC = Sector Count Register [HEX]
    SN = Sector Number Register [HEX]
    CL = Cylinder Low Register [HEX]
    CH = Cylinder High Register [HEX]
    DH = Device/Head Register [HEX]
    DC = Device Command Register [HEX]
    ER = Error register [HEX]
    ST = Status register [HEX]
Powered_Up_Time is measured from power on, and printed as
DDd+hh:mm:SS.sss where DD=days, hh=hours, mm=minutes,
SS=sec, and sss=millisec. It "wraps" after 49.710 days.

Error 1 occurred at disk power-on lifetime: 6612 hours (275 days + 12 hours)
  When the command that caused the error occurred, the device was active or idle.

  After command completion occurred, registers were:
  ER ST SC SN CL CH DH
  -- -- -- -- -- -- --
  04 61 0c 00 00 00 00  Device Fault; Error: ABRT

  Commands leading to the command that caused the error were:
  CR FR SC SN CL CH DH DC   Powered_Up_Time  Command/Feature_Name
  -- -- -- -- -- -- -- --  ----------------  --------------------
  ef 03 0c 00 00 00 00 00   1d+13:35:58.881  SET FEATURES [Set transfer mode]
  e5 00 00 00 00 00 00 00   1d+13:35:58.881  CHECK POWER MODE
  ec 00 00 00 00 00 00 00   1d+13:35:58.879  IDENTIFY DEVICE

```

Alternative path:

```
# SMARTCTL=/usr/local/sbin/smartctl ./smartalert
```



LICENSE
-------

MIT



DEPENDENCIES
------------

- Smartmontools
- Bash 4



