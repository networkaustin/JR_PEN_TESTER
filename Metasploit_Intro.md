# Tryhackme - Metasploit: Introduction

## Introduction to Metasploit

### So what is Metasploit?

Metasploit is a tool that can do everything in a penetration test from start to finish.

### There are two main versions of Metasploit

* Metasploit Pro: Commercial version which facilitates the automation and management of tasks. Also has a GUI.
* Metasploit Framework: Open-source version that works from the CLI.

### Main components of the Metasploit Framework

* msfconsole: The main command-line interface
* Modules: Supporting modules such as exploits, scanners, payloads, etc
* Tools: Stand-alone tools that will help vulnerability research, vulnerability assessment, or penetration testing.

To complete the first section, deploy the machine and access it via the AttackBox console or the VPN.

## Main Components of Metasploit

If you're going through the Attackbox, Metasploit can be launched in the terminal using the ```msfconsole``` command.
Note: Before running ```msfconsole``` you may want to run ```msfupdate``` to update Metasploit Framework

1. Click on the Terminal icon
2. Type ```msfconsole``` into the command line

![Directions][def]

Let's go over a few things before we get carried away:

* Exploits: Pieces of code that use vulnerabilities present on the target system
* Vulnerabilities: Flaws in the system that can be exploited.
* Payloads: The code that runs on a target system and grabs the information we want.

### Metasploit modules and categories

* Auxiliary: Any supporting module, such as scanners, crawlers, and fuzzers.
  * In the Attackbox, located in /opt/metasploit-framework/embedded/framework/modules
  * List all with the command ```tree -L 1 auxiliary/ auxiliary/```
* Encoders: Allow encoding of the exploit and payload in the hopes that a signature-based antivirus might miss them. Encoders might have a limited success rate since a signature-based antivirus might perform additional checks.
  * In the Attackbox, located in /opt/metasploit-framework/embedded/framework/modules
  * List all with the command ```tree -L 1 encoders/ encoders/```
* Evasion: Modules that will specifically try to evade antivirus software.
* Exploits: List of exploits organized by target system.
* NOPs: Stands for No OPeration and they will do nothing and are used as a buffer for payload size consistency
* Payloads: Codes that run on target systems
  * Four different directories under Payloads:
    * Adapters: Wraps single payloads to convert them into different formats
    * Singles: Self-contained payloads that do not need to download an additional component to run.
    * Stagers: Set up a connection between Metasploit and the target system, which can be useful when running staged payloads. These payloads upload stagers then download the rest of the payload, referred to as the stage. This allows the payload to be split up so that the initial size is way lower than the whole payload at once.
    * Stages: Downloaded by the stager
  * Single and staged payloads follow this naming convention
    * generic/shell_reverse_tcp - Inline (single) payload, look for a "_" between "shell" and "reverse"
    * windows/x64/shell/reverse_tcp - Staged payload, look for a "/" between "shell" and "reverse"
* Post: Useful in the final stage of penetration testing, "Post Exploitation"

### Section 2 Questions

#### What is the name of the code taking advantage of a flaw on the target system?

#### What is the name of the code that runs on the target system to achieve the attacker's goal?

#### What are self-contained payloads called?

#### Is "windows/x64/pingback_reverse_tcp" among singles or staged payload?

## Msfconsole

The msfconsole CLI will support most Linux commands. One exception is ```help```, which follows the convention ```help [command]```
The ```history``` command shows commands that have been typed earlier

Context is also important. Unless defined at the global level, variables will be lost when going from one module to another.

### How to use an exploit

To use an exploit, type the ```use``` command into the terminal followed by the exploit

* For example, ```use exploit/windows/smb/ms17_010_ethernalblue```
* ```show options``` will show the module options available, along with which ones are required and a description of each.
* You can use ```show``` followed by a module type in any context
* The command ```info``` followed by a module's path can also be used to show information on the modue such as its author, relevant sources, etc.
* The context can be exited using the ```back``` command
* The ```search``` command searches the database for modules relevant to the given parameter, such as CVE numbers, exploit names, or target systems

### Exploit rankings

Exploits are ranked in the search menu based on their reliability:

* ExcellentRanking
* GreatRanking
* GoodRanking
* NormalRanking
* AverageRanking
* LowRanking
* ManualRanking

### Section 3 Questions

#### How would you search for a module related to Apache?

#### Who provided the auxiliary/scanner/ssh/ssh_login module?

## Working with modules

### The set command

After entering a module's context using the ```use``` command followed by the module name, it's a good practice to use ```show options``` to see which parameters need to be set.
Parameters are set using the command ```set [PARAMETER_NAME] [VALUE]```
```show options``` can also be used to verify if a parameter was set correctly
Remember to always check the context of the CLI to make sure you are in the right spot

Commonly used parameters:

* RHOSTS: "Remote Host", or the IP address of the target system. Can also be a CIDR range like 192.168.0.0/24
* RPORT: "Remote Port", the port on the target system where the vulnerable application is running
* PAYLOAD: Payload used with the exploit
* LHOST: "Localhost", or the IP address of the attacking machine
* LPORT: "Local Port", the port used for a reverse shell on the attacking machine
* SESSION: Each connection established to a target system gets a session ID which can be used with post-exploitation modules to connect to a target system using an existing connection.

### The unset command

Any set parameter can be overridden using ```unset``` for a specific parameter or ```unset all``` for all of them at once

### The setg command

The ```setg``` command sets a value for all modules (or a global value)

### The unsetg command

In a similar fashion to ```set```, ```setg``` values can be overridden using ```unsetg```

### Using a module

After all parameters are set, the module is launched using the ```exploit``` command, and ```exploit -z``` can launch a module without using any parameters.
Some modules support the ```check``` option to check if a target system is vulnerable without exploiting it.

### Sessions

Sessions are created after vulnerabilities have been successfully exploited and serve as a communication channel between the target system and Metasploit
The ```background``` command backgrounds the session prompt and returns you to the msfconsole prompt. This also works with ```CTRL+Z```
The ```sessions``` command can be used from the msfconsole prompt or any context to see the existing sessions
Interact with any sessions using ```sessions -i``` followed by the ID number

### Section 4 Questions

#### How would you set the LPORT value to 6666?

What command is used to set parameters?

#### How would you set the global value for RHOSTS  to 10.10.19.23 ?

What command is used to set parameters globally?

#### What command would you use to clear a set payload?

How do you override a set parameter?

#### What command do you use to proceed with the exploitation phase?

What command is used after parameters are set?

## Summary

Take note of this in the Summary section:
"It would be best if you also had used the ms17_010_eternalblue exploit to gain access to the target VM."

Give it a shot! Go back to the "Working with modules" section and see if you can get it to work.

[def]: .\THM\JR_PEN_TESTER\Stuff\Metasploit_intro_1.png
