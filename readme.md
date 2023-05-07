# CVE-2023-23397 Proof of Concept

This is a proof of concept (PoC) exploit for CVE-2023-23397, a vulnerability in the Windows Microsoft Outlook client.

## Prerequisites

- Python 3
- `pywin32` package

## How to use

### Windows

1. Install the required `pywin32` package by running `pip install pywin32`.
2. Run the script with the following command: `python Exploit.py <save_or_send> <target_email> <attacker_ip>`.

### Kali Linux

1. Install the required `pywin32` package by running `pip install pywin32`.
2. Start an SMB server on the attacker machine, such as Metasploit's SMB module.
3. Run the script with the following command: `python Exploit.py <save_or_send> <target_email> <attacker_ip>`.

## Options

- `<save_or_send>`: Choose between "save" or "send". "save" will save the malicious email as a file, while "send" will send the email directly to the target.
- `<target_email>`: The email address of the target user.
- `<attacker_ip>`: The IP address of the attacker's SMB server.

# CVE-2023-23397 High-Level Overview

This is a high-level overview of CVE-2023-23397, a vulnerability in the Windows Microsoft Outlook client.

## Description

CVE-2023-23397 is a vulnerability that allows an attacker to craft a malicious email, which when processed by the Outlook client, automatically leaks the target user's Net-NTLMv2 hashes. No user interaction is required to trigger the exploit. The leaked hashes can be used for relay attacks or to crack the underlying credentials.

The vulnerability exists due to the way Outlook handles the PidLidReminderFileParameter property in email messages. This property allows the message sender to set a custom notification sound for items like meeting notifications. By crafting a message with a custom PidLidReminderFileParameter property that contains a Universal Naming Convention (UNC) path pointing to an attacker-controlled SMB server, the attacker can cause the target's Outlook client to leak the Net-NTLMv2 hashes when it connects to the SMB server.

## Attack Steps

1. The attacker crafts a malicious email with a custom PidLidReminderFileParameter property containing a UNC path pointing to their SMB server.
2. The target user receives the email in their Outlook client.
3. Outlook processes the email and attempts to fetch the custom notification sound file specified in the PidLidReminderFileParameter property.
4. As Outlook connects to the attacker-controlled SMB server, it leaks the target user's Net-NTLMv2 hashes.
5. The attacker can then use the leaked hashes for relay attacks to other systems supporting NTLMv2 or attempt to crack the hashes to obtain the underlying cleartext credentials.

## Exploit Usage

The provided proof of concept (PoC) exploit script generates a malicious email with the custom PidLidReminderFileParameter property and either saves it as a file or sends it directly to the target email address. The attacker needs to set up an SMB server to capture the leaked Net-NTLMv2 hashes.

## Note

This high-level overview is for educational purposes only. Understanding the vulnerability and its impact can help improve security practices and develop effective defenses against similar attacks.


