# Part02. More Demo Cases

## 2.1 DemoCase001: How to ssh to FGT and run a FGT cmd

```
# -*- coding: robot -*-                  # Just put this line in every .robot file.

*** Settings ***
Documentation    An Example to ssh to FGT and run a command
Library    SSHLibrary  WITH NAME  ssh    # In Settings section, import Libraries needed in current test suite.
                                         # The basic format is: "Library  SSHLibrary",
                                         # but we can create an alias "ssh" just for convenience.
*** Variables ***
${fgt ip}    172.30.145.19               # the format of a variable: ${variable name}
${fgt user}  admin                       # Three variables are declared in Variables Section.
${fgt pwd}   fortinet123                 # don't forget the TWO or MORE SPACES between the elements.

*** Test Cases ***
DemoCase001.Get Wtp-profile from FGT     
    ssh.Open Connection  ${fgt ip}       # Create a ssh connection from local to remote device (fgt)
    ssh.Login  ${fgt user}  ${fgt pwd}   # Apply username/password on the connection.
                                         # Usage of a keyword in a library: library_name.Keyword_Name
    ssh.Write  get system status         # the keyword "write" write the cmd to FGT, just like type it on a ssh console.
    Sleep  2s                            # "Sleep" is a builtin keyword, used for waiting some time for respond from FGT.
    ${output}  ssh.Read                  # keyword "Read" will read output of the command "show wire...." into a variable "${output}"
    log to console  ${output}            # This line will print the content of "${output}" on console
                                         
    ssh.Close Connection                 # Close the connection we created. This is a good habit rather than leave it there until timeout.

*** Keywords ***
                                         # in this exmaple case, no keyword is defined. Just leave it blank.
```

output:
```
C:\Python27\Scripts\robot.exe -d results DemoCase002.Get_WTP_Profile_from_FGT.robot
==============================================================================
DemoCase001.Get System Status from FGT  :: An Example to ssh to FGT and run a ...
==============================================================================
DemoCase001.Get System Status from FGT 
FAP320C-default:
Version: FortiGate-61E v6.0.2,build0163,180725 (GA)
Virus-DB: 1.00000(2018-04-09 18:07)
Extended DB: 1.00000(2018-04-09 18:07)
IPS-DB: 6.00741(2015-12-01 02:30)
IPS-ETDB: 0.00000(2001-01-01 00:00)
...
Max number of virtual domains: 10
Virtual domains status: 1 in NAT mode, 0 in TP mode
Virtual domain configuration: disable
FIPS-CC mode: disable
Current HA mode: standalone
Branch point: 0163
--More--
| PASS |
------------------------------------------------------------------------------
DemoCase001.Get System Status from FGT  :: An Example to ssh to FGT a... | PASS |
1 critical test, 1 passed, 0 failed
1 test total, 1 passed, 0 failed
==============================================================================
Output: C:\AutomationTestCases\QATest\Demo\results\output.xml
Log: C:\AutomationTestCases\QATest\Demo\results\log.html
Report: C:\AutomationTestCases\QATest\Demo\results\report.html
 
Process finished with exit code 0
```

## 2.2 DemoCase002.SSH_to_MacBook_and_Run_CMDs.robot

Key points in this case:

- How to ssh to macbook client
- How to run a command on macbook and get its output.
- How to check the result of a command.
- How to use Tags to organize test cases.

Case:
```
# -*- coding: robot -*-
# Created on May 2019, Only used for WiFi QA Team @Fortinet

*** Settings ***
Documentation    Demo003: Showing how to ssh to a Macbook and run commands on it.
Library  SSHLibrary  WITH NAME  ssh

*** Variables ***
${macbook ip}   172.30.61.239
${mac user}     admin
${mac pwd}      fortinet

*** Test Cases ***
Case001.SSH to MacBook and Run a Command
    [Tags]  all  case001                                   # [Tags] gives more flexibility to run test cases
                                                           # more info, check:http://robotframework.org/robotframework/latest/RobotFrameworkUserGuide.html#by-tag-names
    ssh.Set default Configuration  width=400  height=800    # set bigger buffer window size for output, in case there're lots of output.
    ssh.Open Connection  ${macbook ip}
    ssh.login  ${mac user}  ${mac pwd}

    ssh.Write  ifconfig lo0
    sleep  2s
    ${output}  ssh.read
    log to console  \n\nThe output of "ifconfig en0":
    log to console  ${output}

    should contain  ${output}  127.0.0.2                  # An assertion to do a sanity-check. The case will fail if no "127.0.0.1" in the output
                                                          # Of course, it should passed for this case.
                                                          # There are many keywords like "should xxxxxx" or "should not xxxxx" in library of BuiltIn.
    ssh.Close Connection

*** Keywords ***
```


output:
```
C:\Python27\Scripts\robot.exe -d results -t "Case001.SSH to MacBook and Run a Command" ./
==============================================================================
Demo
==============================================================================
Demo.DemoCase002.SSH to MacBook and Run CMDs :: Suite description
==============================================================================
Case001.SSH to MacBook and Run a Command
 
The output of "ifconfig en0":
lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 16384
options=1203<RXCSUM,TXCSUM,TXSTATUS,SW_TIMESTAMP>
inet 127.0.0.1 netmask 0xff000000
inet6 ::1 prefixlen 128
inet6 fe80::1%lo0 prefixlen 64 scopeid 0x1
nd6 options=201<PERFORMNUD,DAD>
QA-MBP-61s-MacBook-Pro:~ admin$
| PASS |
------------------------------------------------------------------------------
Demo.DemoCase002.SSH to MacBook and Run CMDs :: Suite description | PASS |
1 critical test, 1 passed, 0 failed
1 test total, 1 passed, 0 failed
==============================================================================
Demo | PASS |
1 critical test, 1 passed, 0 failed
1 test total, 1 passed, 0 failed
==============================================================================
Output: C:\Dropbox\80_Fortinet\QATest\Demo\results\output.xml
Log: C:\Dropbox\80_Fortinet\QATest\Demo\results\log.html
Report: C:\Dropbox\80_Fortinet\QATest\Demo\results\report.html
```

if change "should contain ${output} 127.0.0.1" to "should contain ${output} 127.0.0.2" 
the output will be:

```
C:\Python27\Scripts\robot.exe -d results -t "Case001.SSH to MacBook and Run a Command" ./
==============================================================================
Demo
==============================================================================
Demo.DemoCase002.SSH to MacBook and Run CMDs :: Suite description
==============================================================================
Case001.SSH to MacBook and Run a Command
 
The output of "ifconfig en0":
lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 16384
options=1203<RXCSUM,TXCSUM,TXSTATUS,SW_TIMESTAMP>
inet 127.0.0.1 netmask 0xff000000
inet6 ::1 prefixlen 128
inet6 fe80::1%lo0 prefixlen 64 scopeid 0x1
nd6 options=201<PERFORMNUD,DAD>
QA-MBP-61s-MacBook-Pro:~ admin$
| FAIL |
'lo0: flags=8049<UP,LOOPBACK,RUNNING,MULTICAST> mtu 16384
options=1203<RXCSUM,TXCSUM,TXSTATUS,SW_TIMESTAMP>
inet 127.0.0.1 netmask 0xff000000
inet6 ::1 prefixlen 128
inet6 fe80::1%lo0 prefixlen 64 scopeid 0x1
nd6 options=201<PERFORMNUD,DAD>
QA-MBP-61s-MacBook-Pro:~ admin$ ' does not contain '127.0.0.2'
------------------------------------------------------------------------------
Demo.DemoCase002.SSH to MacBook and Run CMDs :: Suite description | FAIL |
1 critical test, 0 passed, 1 failed
1 test total, 0 passed, 1 failed
==============================================================================
Demo | FAIL |
1 critical test, 0 passed, 1 failed
1 test total, 0 passed, 1 failed
==============================================================================
Output: C:\Dropbox\80_Fortinet\QATest\Demo\results\output.xml
Log: C:\Dropbox\80_Fortinet\QATest\Demo\results\log.html
Report: C:\Dropbox\80_Fortinet\QATest\Demo\results\report.html
```

## 2.3 DemoCase003.Config_Wtp_Profile_and_Check_FAP_Status.robot

Main points need to know from this demo:

1. How to import external resource file.
   - There're already many ready-to-use keywords in the resource files, which name is like FortiRes_xxx.robot in directory of "Resource". Written in Robotframework.
   - There're more keywords in the library files, which names are like "FortiLib_xxxx.py", in the directory of "FortiLib". Written in Python
   - The documents of resource and library files are ongoing, just please stay tuned.
2. How to logon a FAP with private IP: ssh to FGT first then jump to FAP
3. How to add assertions: using keywords like "Should ......." or "Should not ......", most of which are in Library: BuiltIn.


DemoCase003.Config_Wtp_Profile_and_Check_FAP_Status.robot
```
=======================================================================================

# -*- coding: robot -*-

*** Settings ***
Documentation  Demo004:Config Wtp-profile with different beacon interval and check the FAP status
...            The output of FAP shluld be:
...            FortiAP-221E # rcfg | grep beacon
...               beacon intv    : 300
...               beacon intv    : 300
...            This cases will check if the output is correct.

Library   SSHLibrary  WITH NAME  ssh
Resource  ../Resource/FortiRes_AC.robot                ## import external resouce file, which includs keywords we developed.

*** Variables ***
${fgt ip}    172.30.145.19
${fgt user}  admin
${fgt pwd}   fortinet123

${ap id}        FP221E3X17001173
${ap ip}        192.168.1.111
${ap user}      admin
${ap pwd}       ${empty}               # This FAP has no password
${wtp-profile}  ${ap id}_profile
 
*** Test Cases ***
Case001.Change Beacon Interval to 200 and Check FAP
    [Tags]  all  001
    ssh.Open Connection  ${fgt ip}
    ssh.Login            ${fgt user}  ${fgt pwd}

    Config Beacon Interval in Both Radios  ${wtp-profile}  200    ## "Config Beacon Interval in Both Radios" is a self-defined keyword in "keywords" section
    Set Wtp-profile To WTP       ${ap id}  ${wtp-profile}              ##  "Set Wtp-profile To WTP" is a self-defined keyword in resource file: FortiRes_AC.robot
    Sleep  10s

    SSH AP Via FGT  ${ap ip}  ${ap user}  ${ap pwd}  ${ap id}  ## "SSH AP Via FGT" is a keywork defined in resouce file: ../Resource/FortiRes_AC.robot
                                                               ## FAP is using a private IP, it cannot connect to it deirectly, must do it via FGT.
    ssh.Write  rcfg | grep beacon                              ## run cmd on AP
    Sleep  2s
    ${output}  ssh.read
    log to console  ${output}                                  ## print output of "rcfg | grep beacon" on the console.

    ssh.Close Connection
    should contain  ${output}  beacon\ intv${space*4}:\ 200    ## Add an asserion here to check the output.

Case002.Change Beacon Interval to 300 and Check FAP
    [Documentation]  Case002 is same as case001 except the number of beacon interval.
    [Tags]  all  002
    ssh.Open Connection  ${fgt ip}
    ssh.Login            ${fgt user}  ${fgt pwd}

    Config Beacon Interval in Both Radios  ${wtp-profile}  300
    Set Wtp-profile To WTP       ${ap id}  ${wtp-profile}
    Sleep  10s

    SSH AP Via FGT  ${ap ip}  ${ap user}  ${ap pwd}  ${ap id}

    ssh.Write  rcfg | grep beacon
    Sleep  2s
    ${output}  ssh.read
    log to console  ${output}

    ssh.Close Connection
    should contain  ${output}  beacon\ intv${space*4}:\ 300    ## It's better check the output after closing connection. In case it could fail.

*** Keywords ***
Config Beacon Interval in Both Radios
    [Arguments]  ${wtp-profile}  ${interval}        ## There are two arguments for this keywords: ${wtp-profile} and ${interval}.
    ${config}  set variable                         ## "set variable" is a keyword to assign a value to a variable.
    ...  config wireless wtp-profile                ## "..." is used to connect lines together.
    ...     edit ${wtp-profile}                     ## ${config} is a list. Every line is an item in the list.
    ...         config radio-1
    ...             set beacon-interval ${interval}
    ...         end
    ...         config radio-2
    ...             set beacon-interval ${interval}
    ...         end
    ...     end
    Run Commands on FGT  ${config}               ## "Run Commands on FGT" is a self-defined keyword saved in a resource file, "../Resource/FortiRes_AC.robot",
                                                 ## "Run Commands on FGT" runs a series of commands saved in a list on the connection to FGt.
=======================================================================================
```

## 2.4 DemoCase004: Reduce redundant code in test cases

Key points in DemoCase004:

1. How to reduce redundant code: make repeated codes into keywords.
    A Good Test Case must be straightforward and no redundant code.
    In the test suite in last page, DemoCase003, Case001 are almost same as Case002: only the beacon intervals are changed.

DemoCase004 will make reduce the redundant  codes by calling same keyword with different arguments.

If we put the two keywords into a separate resource file, this test suite will be very short: the bodies of test cases are only one line.

```
==========================================================================

# -*- coding: robot -*-

*** Settings ***
Documentation  Demo004:Config Wtp-profile with different beacon interval and check the FAP status
...            The output of FAP shluld be:
...            FortiAP-221E # rcfg | grep beacon
...               beacon intv    : 300
...               beacon intv    : 300
...            This cases will check if the output is correct.

Library   SSHLibrary  WITH NAME  ssh
Resource  ../Resource/FortiRes_AC.robot                ## import external resouce file, which includs keywords we developed.

*** Variables ***
${fgt ip}    172.30.145.19
${fgt user}  admin
${fgt pwd}   fortinet123

${ap id}        FP221E3X17001173
${ap ip}        192.168.1.111
${ap user}      admin
${ap pwd}       ${empty}               # This FAP has no password
${wtp-profile}  ${ap id}_profile

*** Test Cases ***
Case001.Change Beacon Interval to 200 and Check FAP
    [Tags]  all  001
    Change Beacon Interval in Wtp-profile and Check FAP  200

Case002.Change Beacon Interval to 300 and Check FAP
    [Tags]  all  002
    Change Beacon Interval in Wtp-profile and Check FAP  300

*** Keywords ***
Change Beacon Interval in Wtp-profile and Check FAP
    [Arguments]  ${interval}
    ssh.Open Connection  ${fgt ip}
    ssh.Login            ${fgt user}  ${fgt pwd}

    Config Beacon Interval in Both Radios  ${wtp-profile}  ${interval}
    Set Wtp-profile To WTP       ${ap id}  ${wtp-profile}
    Sleep  10s

    SSH AP Via FGT  ${ap ip}  ${ap user}  ${ap pwd}  ${ap id}

    ssh.Write  rcfg | grep beacon
    Sleep  2s
    ${output}  ssh.read
    log to console  ${output}

    ssh.Close Connection
    should contain  ${output}  beacon\ intv${space*4}:\ ${interval}

Config Beacon Interval in Both Radios
    [Arguments]  ${wtp-profile}  ${interval}
    ${config}  set variable
    ...  config wireless wtp-profile
    ...     edit ${wtp-profile}
    ...         config radio-1
    ...             set beacon-interval ${interval}
    ...         end
    ...         config radio-2
    ...             set beacon-interval ${interval}
    ...         end
    ...     end
    Run Commands on FGT  ${config}
=================================================================================================
```

## 2.5 DemoCase005: Connect MacBook to SSID

This test case will:

- ssh to a macbook,
- Connect to ssid "Automation_2.4G" and check connection
- Connect to ssid "Automation_5G" and check connnection
 
This case runs OSX commdands on the ssh connection by writing them directly. Next demo will use some self-defined keywords to finish the same functions.

Key Points:

- How to connect to a ssid on OSX.
- How to define a keyword
- The commands about network operation on OSX. 

Before running the case, Create a soft link for command, airport, on macbook:

```
ln -s /System/Library/PrivateFrameworks/Apple80211.framework/Versions/A/Resources/airport ./airport
```

And, run and check all commands work:
```
AutoMac01:~ admin$ ./airport -s | grep Automation
                 Automation_2.4G 70:4c:a5:60:9a:88 -18  11      Y  US WPA2(PSK/AES/AES)
                 Automation_5G 70:4c:a5:60:9a:90 -21  100     Y  US WPA2(PSK/AES/AES)
 
AutoMac01:~ admin$ ./airport -I
     agrCtlRSSI: -29
     agrExtRSSI: 0
    agrCtlNoise: -92
    agrExtNoise: 0
          state: running
        op mode: station
     lastTxRate: 780
        maxRate: 867
lastAssocStatus: 0
    802.11 auth: open
      link auth: wpa2-psk
          BSSID: 70:4c:a5:60:9a:90
           SSID: Automation_5G
            MCS: 9
        channel: 100,80
 
AutoMac01:~ admin$ networksetup -setairportnetwork en0 Automation_2.4G fortinet
 
admins-MacBook-Pro-2:~ admin$ ./airport -I
     agrCtlRSSI: -47
     agrExtRSSI: 0
    agrCtlNoise: -96
    agrExtNoise: 0
          state: running
        op mode: station
     lastTxRate: 54
        maxRate: 289
lastAssocStatus: 0
    802.11 auth: open
      link auth: wpa2-psk
          BSSID: 8:5b:e:79:16:14
           SSID: baishan-2
            MCS: -1
        channel: 36
```

Test case: 

```
# -*- coding: robot -*-
 
*** Settings ***
Documentation    Demo how to connect macbook to ssid
Library  SSHLibrary  WITH NAME  ssh
 
*** Variables ***
${macbook ip}    172.30.145.172
${macbook user}  admin
${macbook pwd}   fortinet
 
${SSID-1}    Automation_2.4G
${SSID-2}    Automation_5G
${SSID pwd}  fortinet
 
*** Test Cases ***
Case1.SSH to Mac and Run some commands
    ssh.Open Connection  ${macbook ip}
    ssh.Login  ${macbook user}  ${macbook pwd}
 
    ssh.Write  networksetup -setairportnetwork en0 ${SSID-1} ${SSID pwd}
    Sleep  15s
    ssh.Write  ./airport -I | grep SSID:
    Sleep  2s
 
    Print and Check output  ${SSID-1}
 
    ssh.Write  networksetup -setairportnetwork en0 ${SSID-2} ${SSID pwd}
    Sleep  15s
    ssh.Write  ./airport -I | grep SSID:
    Sleep  2s
 
    Print and Check output  ${SSID-2}
 
    ssh.Close Connection
 
*** Keywords ***
Print and Check output
    [Arguments]  ${ssid}
    ${output}  ssh.Read
    log to console  ${\n}${output}
    should contain  ${output}  ${ssid}
```

## 2.6 DemoCase006: Using self-defined keywords connect to SSID

What the test case do:

- ssh to macbook
- connect the macbook to a SSID and ping 8.8.8.8: if it cannot connect to SSID or cannot ping 8.8.8.8, the test case will fail, pass otherwise.
- close the ssh connection

Only three lines in each test case in this demo, but it does lots of work actually: all steps are encapsulated into a self-defined keyword, like: Connect Macbook to SSID and Check Ping

This keyword is pre-defined in resource file: FortiRes_OSX.robot
and be imported in "Settings" section: Resource    ../Resource/FortiRes_OSX.robot

Note: 
1. case2 will fail, because the destination ip 8.8.8.80 is unreachable.
2. open FortiRes_OSX.robot to check the details of the keywords.

Key points in this demo:
- how to import self-defined library/resource
- how to use self-defined keywords

```
=============================================================================================================================
# -*- coding: robot -*-

*** Settings ***
Documentation    Demo how to connect macbook to ssid
Library  SSHLibrary  WITH NAME  ssh
Resource   ../Resource/FortiRes_OSX.robot

*** Variables ***
${macbook ip}    172.30.145.172
${macbook user}  admin
${macbook pwd}   fortinet

${SSID-1}    Automation_2.4G
${SSID-2}    Automation_5G
${SSID pwd}  fortinet

*** Test Cases ***
Case1.SSH Macbook to SSID-1
    [Tags]  all  case1
    SSH to Macbook  ${macbook ip}  ${macbook user}  ${macbook pwd}
    Connect Macbook to SSID and Check Ping  ${macbook ip}  ${macbook user}  ${macbook pwd}  ${ssid-1}  ${ssid pwd}  8.8.8.8
    ssh.Close Connection

Case2.SSH Macbook to SSID-2
    [Tags]  call  case2
    SSH to Macbook  ${macbook ip}  ${macbook user}  ${macbook pwd}
    Connect Macbook to SSID and Check Ping  ${macbook ip}  ${macbook user}  ${macbook pwd}  ${ssid-2}  ${ssid pwd}  8.8.8.80
    ssh.Close Connection

*** Keywords ***
=============================================================================================================================
```

## 2.7 Using Suite/Test Setup/Teardown

What the test case do:
- ssh to macbook
- connect the macbook to a SSID and ping 8.8.8.8: if it cannot connect to SSID or cannot ping 8.8.8.8, the test case will fail, pass otherwise.
- close the ssh connection
  
This demo does same the previous one. The only changes are moving "ssh to macbook" and "close connection" to "*** Settings ***" section.

If some steps are  needed to do before every test case, and other after the test, which are "ssh/disconnect to macbook" in this demo, those steps can be placed in "Test Setup/Teardown"

The benefits are:
- save codes.
- It guarantees the teardown part will run, even though the case fails 

Note: 

1. Suite Setup/Suite Teardown do the similar job,  which will run at beginning/end of whole test suite.
2. I just put "no operation", which does nothing, in "Suite Setup/Teardown"

Key points in this demo:
- how to use Test Setup/Test Teardown
- how to use Suite Setup/Suite Teardown

```
=============================================================================================================================
# -*- coding: robot -*-

*** Settings ***
Documentation    Demo how to connect macbook to ssid
Library  SSHLibrary  WITH NAME  ssh
Resource   ../Resource/FortiRes_OSX.robot

Suite Setup  no operation
Suite Teardown  no operation
Test Setup  SSH to Macbook  ${macbook ip}  ${macbook user}  ${macbook pwd}
Test Teardown  ssh.Close Connection

*** Variables ***
${macbook ip}    172.30.145.172
${macbook user}  admin
${macbook pwd}   fortinet

${SSID-1}    Automation_2.4G
${SSID-2}    Automation_5G
${SSID pwd}  fortinet

*** Test Cases ***
Case1.SSH Macbook to SSID-1
    [Tags]  all  case1

    Connect Macbook to SSID and Check Ping  ${macbook ip}  ${macbook user}  ${macbook pwd}  ${ssid-1}  ${ssid pwd}  8.8.8.8

Case2.SSH Macbook to SSID-2
    [Tags]  call  case2

    Connect Macbook to SSID and Check Ping  ${macbook ip}  ${macbook user}  ${macbook pwd}  ${ssid-2}  ${ssid pwd}  8.8.8.80

*** Keywords ***

=============================================================================================================================
```

## 2.10 DemoCase010.Capture Packets using Tshark

What the test case do:
- ssh to macbook
- connect the macbook to a SSID.
- Using tshark to capture packets and parse the packets into plain text
- check the key words in the output (the plain text)

Note: 
1. Before run the case on your setup, you need:
  - Modify /etc/sudoers on macbook, which helps run “sudo <cmd>” without password needed.
    ```
    QA-MBP-10:~ admin$ sudo vim /etc/sudoers
    ```
  - Add a new line if it’s not in the file (‘admin’ is the macbook user):
    ```
    admin   ALL=(ALL) NOPASSWD:ALL
    ```

2. Install tshark (will be installed with wireshark) and check it’s version:

    It  could be:
    ```
    QA-MBP-10:~ admin$ tshark -v           
    TShark (Wireshark) 2.6.11 (v2.6.11-0-gc9496ea5)

    QA-MBP-13:~ admin$ tshark -v
    TShark 1.12.7 (v1.12.7-0-g7fc8978 from master-1.12)
    ```

    The commands may be different (in the filters mainly) on different version (v1.x vs v2.x), please try the command manually first on macbook.

    In the demo case:

    For v2.x:   
    ```
    sudo tshark -i en0 -I -Y "(wlan.ssid == ${ssid}) && (wlan.fc.type_subtype == 0x0008)" -V -a duration:${time}
    ```

    for v1.x:
    ```
    sudo tshark -i en0 -I -Y "(wlan_mgt.ssid == ${ssid}) && (wlan.fc.type_subtype == 0x0008)" -V -a duration:${time}
    ```

 3. This demo shows how to get beacon interval. Just change the filter If you’d like to capture other packets.

Key points in this demo:
- how to use 3rd part app in roboframework, tshark in this demo case.
- how to check the fields in packets captured.


Cases:
```
=============================================================================================================================
# -*- coding: robot -*-

*** Settings ***
Documentation    This Suite shows how to capture packets on macbook

Library    SSHLibrary  WITH NAME  ssh
Library    ../FortiLib/FortiLib_SanityTest.py
Library    ../FortiLib/FortiLib_String.py
Resource   ../Resource/FortiRes_OSX.robot

*** Variables ***
${macbook ip}    172.30.145.204
${macbook user}  admin
${macbook pwd}   fortinet

${SSID-1}    Automation_2.4G
${SSID-2}    Automation_5G
${SSID pwd}  fortinet

${fgt ip}    172.30.145.85

${ap id}    FP221ETF18013652
${ap ip}    192.168.1.110
${ap user}      admin
${ap pwd}       fortinet

${Wtp-profile}  FP221ETF18013652_profile

*** Test Cases ***
Case001.Capture Packets Using Tshark
    [Tags]  all  case001
    ssh to FGT  ${fgt ip}  admin  fortinet123
    Set Wtp-profile To WTP  ${ap id}  ${Wtp-profile}

    ## Access to Macbook
    ssh to macbook  ${MACBOOK IP}  ${macbook user}  ${macbook pwd}  ## Note: Case-Insensitive in Robotframework

    ## Caputure beacon on Macbook
    ${connected}  Connect MacBook to SSID  ${ssid-2}  ${ssid pwd}
    log to console  Connected to ${ssid-2}?: ${connected}
    should be equal  ${connected}  ${True}
    ${beacon info}  Capture Beacon Packets  ${ssid-2}  10  old
    ${beacon interval}  Get Beacon Interval from Beacon  ${beacon info}
    ${dtim period}  Get DTIM Period from Beacon  ${beacon info}

    log to console  beacon interval: ${beacon interval}
    log to console  DTIM Period: ${dtim period}

*** Keywords ***
Capture Beacon Packets
    [Arguments]  ${ssid}  ${time}=30  ${tshark version}=new

    :For  ${i}  IN RANGE  ${9}
    \  log to console  ${\n}:::: Try ${i}/9 :::
    \  log to console  ${\n}:::: capture beacons for ${time} seconds ::::
    \  ${cmd}  set variable if  "${tshark version}"=="new"  sudo tshark -i en0 -I -Y "(wlan.ssid == ${ssid}) && (wlan.fc.type_subtype == 0x0008)" -V -a duration:${time}
                                          ...               sudo tshark -i en0 -I -Y "(wlan_mgt.ssid == ${ssid}) && (wlan.fc.type_subtype == 0x0008)" -V -a duration:${time}
    \  log to console  Run: ${cmd}
    \  ssh.Write  ${cmd}
    \  sleep  ${time}s
    \  ${output}  ssh.Read
    \  ${lines}  Get Line Count  ${output}
    \  log to console  Lines of output: ${lines}
    \  exit for loop if  ${lines}>400

    run keyword if  ${lines}<400  Fail  ===>>> Fail! no enough packets.

    [Return]  ${output}
=============================================================================================================================
```

## 2.11 DemoCase011 Upgrading/Downgrading FGT

```
# -*- coding: robot -*-

*** Settings ***
Documentation    This Demo shows how to upgrade FGT by automation script
Library    SSHLibrary  WITH NAME  ssh
Resource   ../Resource/FortiRes_AC.robot

Suite Setup  Suite Setup Actions
Suite Teardown  Suite Teardown Actions
Test Setup  Test Setup Actions
Test Teardown  Test Teardown Actions

*** Variables ***
${fgt ip}   172.30.xx.xxx
${fgt usr}  admin
${fgt pwd}  fortinet

*** Test Cases ***
Case.Demo011.upgrade or downgrade fgt
    [Setup]  ssh to fgt  ${fgt ip}  ${fgt usr}  ${fgt pwd}
    ########################## output example #######################################################
    #Testbed2-500E # execute restore image ftp /home/Images/FortiOS/v6.00/images/build1722/FGT_500E-v6-build1722-FORTINET.out 172.16.100.71 wifiqaauto f6~tcdxo++eN
    #This operation will replace the current firmware version!
    #Do you want to continue? (y/n)y
    #
    #Please wait...
    #
    #Connect to ftp server 172.16.100.71 ...
    #Get image from ftp server OK.
    #
    #Checking new firmware integrity ... pass
    #This operation will downgrade the current firmware version.
    #Do you want to continue? (y/n)y
    ########################## output example end ####################################################

    ${fgt version}=  Get FGT Version
    log to console  fgt version: ${fgt version}

    ${image path}  set variable  /home/Images/FortiOS/v6.00/images/build1723/FGT_500E-v6-build1723-FORTINET.out
    ssh.write   execute restore image ftp ${image path} 172.16.100.71 wifiqaauto f6~tcdxo++eN
    sleep  5s
    # When do it manually, it only need input a "y", and does not need "enter". So here is "ssh.write Bare" instead of "ssh.write"
    # If using "ssh.write  y" here, it will ignore next step, input the 2nd "y", when downgrading.
    ssh.write Bare  y
    ${output}  ssh.read
    log to console  ${output}

    # if downgrade, it needs double check and input a "y"
    sleep  20s
    ssh.write bare  y
    ${output}  ssh.read
    log to console  ${output}

    # FGT will reboot during upgrading/downgrading and the current connection will be interrupted, 
    # wait enough time here and ssh to fgt again.
    sleep  150s
    ssh to fgt  ${fgt ip}  ${fgt usr}  ${fgt pwd}

    ${fgt version}=  Get FGT Version
    log to console  fgt version: ${fgt version}

    [Teardown]  ssh.close all connections

*** Keywords ***
Suite Setup Actions
    no operation

Suite Teardown Actions
    no operation

Test Setup Actions
    log to console  ${\n}

Test Teardown Actions
    no operation
```

## 2.13.Check_and_Set_CFG_values_on_FAP

```
# -*- coding: robot -*-

*** Settings ***
Documentation    Demo how to get and set value with 'cfg' on FAP
Library    SSHLibrary  WITH NAME  ssh
Resource  ../Resource/FortiRes_AC.robot

*** Variables ***
${fgt ip}    192.168.50.141
${fgt user}  admin
${fgt pwd}  fortinet

${fap ip}  192.168.1.1
${fap user}  admin
${fap pwd}  fortinet
${fap id}  FP231FTF20026177

*** Test Cases ***
DemoCase001.Get and Set CFG values on FAP
    [Documentation]  If BAUD_RATE==9600, update it to 115200,
    ...              if BAUD_RATE==115200, update it to 9600
    [Tags]  case001
    SSH to FGT  ${fgt ip}  ${fgt user}  ${fgt pwd}
    SSH AP Via FGT  ${fap ip}  ${fap user}  ${fap pwd}  ${fap id}

    ${value}  check value in cfg -s on FAP  BAUD_RATE
    log to console   1. BAUD_RATE: ${value}

    FOR  ${i}  IN RANGE  1
        run keyword if  '${value}'=='9600'  set value by cfg -a on FAP  BAUD_RATE  115200
        run keyword if  '${value}'=='9600'  exit for loop
        run keyword if  '${value}'=='115200'  set value by cfg -a on FAP  BAUD_RATE  9600
    END

    ${value}  check value in cfg -s on FAP  BAUD_RATE
    log to console   2. BAUD_RATE: ${value}

    Exit SSH AP Via FGT v2  FP221E5518043977
    [Teardown]  ssh.close connection

*** Keywords ***
check value in cfg -s on FAP
    [Arguments]  ${field}
    ssh.write  cfg -s | grep ${field}
    sleep  2s
    ${output}  ssh.read
    ${lines}  Get Lines Containing String  ${output}  ${field}
#    ${value}  evaluate  "${lines}".strip().split(":=")[1]    # will get syntaxError of EOF
    ${value}  evaluate  """${lines}""".strip().split(":=")[1]
    # the output of "cfg -s":
        # FortiAP-221E # cfg -s
        # BAUD_RATE:=115200
        # FIRMWARE_UPGRADE:=0
        # FACTORY_RESET:=0
    [Return]  ${value}

set value by cfg -a on FAP
    [Arguments]  ${field}  ${value}
    ssh.write  cfg -a ${field}=${value}
    ssh.Write  cfg -c
    sleep  3s
    ${new value}  check value in cfg -s on FAP  ${field}
    should be equal  ${value}  ${new value}
    [Return]  ${value}
```