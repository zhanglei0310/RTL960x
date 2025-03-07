# Slow Upload Speed
The `slow upload speed` when ODI DFP-34X-2C2 in 2.5Gb mode, this is known issue in some ISP where ISP using another Realtek ONU beside RTL960x

## Realtek ONU
ISP bought cheap ONU or ONR (Optical Network Router) and it using Realtek SoC where share similar to RTL960x, for example `D-Link DPN-FX3060V`

[![EuA0cO.md.jpg](https://pictr.com/images/2023/06/24/EuA0cO.md.jpg)](https://pictr.com/image/EuA0cO)
[![EuAPnr.md.jpg](https://pictr.com/images/2023/06/24/EuAPnr.md.jpg)](https://pictr.com/image/EuAPnr)

## Wrong OMCI
OLT send wrong OMCI value to ODI DFP-34X-2C2 which D-Link DPN-FX3060V is understand:

### D-Link DPN-FX3060V
```
bandwidth get egress port all
port: 0  rate:1024000

bandwidth get ingress port all
port: 0 rate:32767999
port: 1 rate:32767999
port: 2 rate:32767999
port: 3 rate:32767999
port: 4 rate:32767999
port: 5 rate:32767999
port: 6 rate:32767999
port: 7 rate:32767999
```

### ODI DFP-34X-2C2
```
bandwidth get egress port all
port: 0  rate:1048568
         queue: 0  apr-index: 0
         queue: 1  apr-index: 0
         queue: 2  apr-index: 0
         queue: 3  apr-index: 0
         queue: 4  apr-index: 0
         queue: 5  apr-index: 0
         queue: 6  apr-index: 0
         queue: 7  apr-index: 0
port: 2  rate:4194296
         queue: 0  apr-index: 0
         queue: 1  apr-index: 0
         queue: 2  apr-index: 0
         queue: 3  apr-index: 0
         queue: 4  apr-index: 0
         queue: 5  apr-index: 0
         queue: 6  apr-index: 0
         queue: 7  apr-index: 0

bandwidth get ingress port all
port: 0 rate:1048568
port: 2 rate:4194296
```

# Fixes
You may try these

## `OMCI_TM_OPT`
```
flash set OMCI_TM_OPT 0
reboot
```
0. Priority controlled
1. Rate controlled
2. Priority and rate controlled

## `diag`
```
/bin/diag bandwidth set egress port all rate 4194296
/bin/diag bandwidth set ingress port all rate 4194296
```
* these will not survive across reboot!

If these wont fix the issue, you can't use ODI DFP-34X-2C2, unless ODI fix the firmware.
