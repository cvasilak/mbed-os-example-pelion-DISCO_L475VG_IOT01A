# Pelion DM - pre-compiled version

Note this is guide on how to generate a pre-compiled version of the application for Mbed OS and Pelion DM.
In addition, this repository is expected to be used out of the box with Mbed CLI and Mbed Studio for the DISCO_L475VG_IOT01A and the Arm Compiler 6.13 (supported in Mbed OS 5.15.1)

### Preparing a pre-compiled application

(skip if you only want to use it)

Overview: the process consist on replacing the source code with the objects pre-compiled and leave the python tools in place.

1. Import the application with all source code

```
mbed import https://github.com/ARMmbed/mbed-os-example-pelion

cd mbed-os-example-pelion

```

2. Install Pelion credentials to be able to compile


You should have the CLOUD_SDK_API_KEY configuration defined. You can check this using:

```
mbed config -G --list
```

Then install the Pelion credentials

```
mbed dm init -d "arm.com" --model-name "test" -q --force
```

3. Compile the application and generate the objects

```
mbed compile --library --no-archive -t ARM -m DISCO_L475VG_IOT01A
```

The generated objects should be in `.\BUILD\libraries`

4. Deleting unused source code from root directory

- mbed-cloud-client.lib
- mbed-cloud-client
- drivers
- mbed-os.lib
- mbed-os\cmsis
- mbed-os\components
- mbed-os\events
- mbed-os\features
- mbed-os\hal
- mbed-os\platform
- mbed-os\rtos
- mbed-os\targets

5. Move pre-compiled objects

From pre-compiled location: `BUILD\libraries\<app-name>\DISCO_L475VG_IOT01A\ARM\

- drivers --> root
- mbed-cloud-client --> root
- mbed-os\cmsis --> root/mbed-os/
- mbed-os\components --> root/mbed-os/
- mbed-os\events --> root/mbed-os/
- mbed-os\features --> root/mbed-os/
- mbed-os\hal --> root/mbed-os/
- mbed-os\platform --> root/mbed-os/
- mbed-os\rtos --> root/mbed-os/
- mbed-os\targets --> root/mbed-os/

Note: it's possible to integrate all objects from other libraries in a single repository as explained above. This is known to work ok with Mbed CLI but not with Mbed Studio. Hence I've had to move the pre-compiled Mbed OS to an external repository and re-introduce [mbed-os.lib](https://github.com/MarceloSalazar/mbed-os-example-pelion-DISCO_L475VG_IOT01A/blob/master/mbed-os.lib).

### Using this example with Mbed CLI

Importing, installing credentials and compiling:

```
mbed import <example URL>
cd <example>
mbed dm init -d "arm.com" --model-name "test" -q --force
mbed compile -t ARM -m DISCO_L475VG_IOT01A -c
```

Output:

```
| Module                                        |           .text |     .data |            .bss |
|-----------------------------------------------|-----------------|-----------|-----------------|
| BUILD\DISCO_L475VG_IOT01A                     |     4656(+4656) |     0(+0) |       369(+369) |
| [lib]\c_w.l                                   |   19717(+19717) |   20(+20) |       620(+620) |
| [lib]\fz_wm.l                                 |     1988(+1988) |     0(+0) |           0(+0) |
| [lib]\libcpp_w.l                              |           1(+1) |     0(+0) |           0(+0) |
| [lib]\libcppabi_w.l                           |         44(+44) |     0(+0) |           0(+0) |
| [lib]\m_wm.l                                  |       802(+802) |     0(+0) |           0(+0) |
| anon$$obj.o                                   |         64(+64) |     0(+0) |   85120(+85120) |
| drivers\COMPONENT_WIFI_ISM43362               |   13969(+13969) |     0(+0) |       898(+898) |
| mbed-cloud-client\factory-configurator-client |     8002(+8002) |     0(+0) |         18(+18) |
| mbed-cloud-client\mbed-client                 |   60981(+60981) |   22(+22) |         12(+12) |
| mbed-cloud-client\mbed-client-pal             |   13480(+13480) |     1(+1) |       155(+155) |
| mbed-cloud-client\source                      |     9100(+9100) |     5(+5) |         24(+24) |
| mbed-cloud-client\update-client-hub           |   28749(+28749) | 147(+147) |     5466(+5466) |
| mbed-os\components                            |     7112(+7112) |     0(+0) |         48(+48) |
| mbed-os\drivers                               |     8801(+8801) |     0(+0) |     1052(+1052) |
| mbed-os\events                                |     2116(+2116) |     0(+0) |     4680(+4680) |
| mbed-os\features                              | 134010(+134010) | 194(+194) |     9299(+9299) |
| mbed-os\hal                                   |     2178(+2178) |     8(+8) |       130(+130) |
| mbed-os\platform                              |     9106(+9106) |   80(+80) |     1113(+1113) |
| mbed-os\rtos                                  |   13784(+13784) | 168(+168) |     7913(+7913) |
| mbed-os\targets                               |   26429(+26429) |   40(+40) |     1609(+1609) |
| Subtotals                                     | 365089(+365089) | 685(+685) | 118526(+118526) |
Total Static RAM memory (data + bss): 119211(+119211) bytes
Total Flash memory (text + data): 365774(+365774) bytes
```

### Using this example with Mbed Studio

Note: these steps including a workround due to current limitations of Mbed Studio to recognize this example as Pelion DM compatible.

1. Import application and past the URL of this example

2. Find credentials that have been generated already for another project (either with Mbed Studio or Mbed CLI). Copy and paste the content in the file [mbed_cloud_dev_credentials.c](https://github.com/MarceloSalazar/mbed-os-example-pelion-DISCO_L475VG_IOT01A/blob/master/mbed_cloud_dev_credentials.c)

3. Compile & program
