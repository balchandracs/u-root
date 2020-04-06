"default_action": ""
=========
Only "" default action supported at this point.

"collectors":
=========
Only four collectors supported at this point. They are dmi, file, storage and cpuid.
## dmi collector:

measures output of dmidecode based on input
type. input type (like BIOS, processor, system etc) 
can be provided in policy file as shown.
```
"type": "dmi",
"events": [
   {
     "label": "BIOS",
     "fields": []
   },
   {
     "label": "System",
     "fields": []
   },
   {
     "label": "Processor",
     "fields": []
   }
```

## file collector: 

measures any file that platform owner
wants to measure. target file to measure is input in the
paths field in policy file as shown
```
{
    "type": "files",
    "paths": [ "sda1:/Paul/foo" ]
},
```

## storage collector:

measures an entire disk. target disk to measure
can be provided in the policy file as shown
```
{
    "type": "storage",
    "paths": [ "/dev/sdb1" ]
},
```

## cpuid collector:

measures (hashes) cpuid data of the platform
and stores the result on a file. target file is input as
location field in policy file.
e.g
```
{
    "type": "cpuid",
    "location": "sda1:/Paul/foo"
}
```

"attestor": {}
=========
a nil slice is only supported at this point.

"launcher":
=========
launcher provides ability to kexec into a target kernel.
platform owners can choose which target kernel to kexec into
by setting launcher module in the platform policy file
as shown.
```
"launcher": {
    "type": "kexec",
    "params": {
    "initrd":"sdc1:/boot/initramfs-4.14.35-1941.el7uek.x86_64.img",
    "cmdline":"BOOT_IMAGE=/boot/vmlinuz-4.14.35-1941.el7uek.x86_64
root=/dev/mapper/ol-root ro rd.lvm.lv=ol/root LANG=en_US.UTF-8",
    "kernel":"sdc1:/boot/vmlinuz-4.14.35-1941.el7uek.x86_64"
    }
```
"eventlog":
=========
eventlog provides ability to parse event logs generated by
securelaunch kernel and write them to a file.The target file
can be specified in the policy file as shown.
```
"eventlog": {
    "type": "file",
    "location": "sda1:/evtlog"
}
```

***

Path format in policy file
=========
All of our file paths have the format device_identifier:/path/to/file.
device identifier is of the format 
1. sda:/path/to/file OR 
2. UUID:/path/to/file.

The following examples are acceptable formats for file path.
```
sda1:/foo/securelaunch.policy
sda2:/bar/evtlog
MWAAB8-Sunz-tRrp-E67t-sbal-xfYk-x9f6PZ:/zyd/cpuid.txt
```

SPECIAL NOTE: There is no need to prefix devices with /dev, so sda is sufficient.
Infact, if you enter a path as "/dev/sda", it will not be parsed by sluinit.