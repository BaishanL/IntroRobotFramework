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