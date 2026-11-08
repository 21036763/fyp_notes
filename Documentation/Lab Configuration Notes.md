# Lab Configuration Notes

>[!info] Notes
> Underlying environment for the attack scenario in [[Web Server Attack Storyboarding]]. Progress tracked in [[Check list]]

## Computer Configurations

> [!warning] Windows Server 2022
> - NAT configuration: Host Only
> - Host name: `PALESSI-DC`
> - Directory Service Restore Password (DSRM): `Passw0rd`
> - Host IP: `10.10.10.10`
> - Credentials: `administrator:P@ssw0rd`

> [!warning] Ubuntu Web Server
> - NAT configuration: Host Only
> - Host name: `?`
> - Host IP: `192.168.1.151/25`
> - Credentials: `edbert:P@ssw0rd`

> [!warning] Windows 10 VM
> - NAT configuration: Host Only
> - Host name: `?`
> - Host IP: `192.168.1.131/25`
> - Credentials: `edbert:P@ssw0rd`

> [!warning] OPNsense VM (Firewall)
> - Adapters: VMnet1 (Host-Only), VMnet8 (NAT)
> - Host IP: `192.168.1.129/24`
> - Credentials: `root:toor`

## Active Directory

> [!note] Tree — `palessi.local`
> ```
> palessi.local
> └── OU=Palessi
>     ├── OU=Employees
>     ├── OU=IT Support
>     ├── OU=Service Accounts
>     ├── OU=Computers
>     └── OU=Groups
> ```

> [!note] Users
>
> | Display Name  | SamAccountName | Role             | Password     |
> | ------------- | -------------- | ---------------- | ------------ |
> | ==Chloe Sim== | chloe.sim      | User             | P@ssw0rd     |
> | Daniel Lim    | daniel.lim     | User             | ChangeMe123! |
> | Priya Nair    | priya.nair     | User             | ChangeMe123! |
> | Marcus Tan    | marcus.tan     | User             | ChangeMe123! |
> | Hana Yusof    | hana.yusof     | User             | ChangeMe123! |
> | ==Raj Kumar== | raj.kumar      | IT Support       | P@ssw0rd     |
> | Aisyah Rahman | aisyah.rahman  | IT Support       | ChangeMe123! |
> | Ethan Goh     | ethan.goh      | IT Support       | ChangeMe123! |
> | svc_webapp    | svc_webapp     | Service Account  | letmein123!      |

## Tech Stack

| Service                  | Operating System    |
| ------------------------ | -------------------- |
| Windows Active Directory | Windows Server 2022 |
| Vulnerable Server        | Ubuntu TLS           |
| Windows Machine          | Windows 10           |
| Firewall (Vnet)          | OPNsense             |

## Network Segmentation Zones

| Network Zone          | VM Adapter | OPNsense | Interface Name | Subnet           | Gateway      |
| --------------------- | ---------- | -------- | -------------- | ---------------- | ------------ |
| Untrusted Public Zone | VMnet2     | em0      | WAN            | 192.168.0.0/24   | 192.168.0.1  |
| DMZ (Web Server)      | VMnet3     | em1      | DMZ            | 192.168.1.128/25 | 192.168.18.0 |
| USER                  | VMnet4     | em2      | User           | 192.168.1.0/25   | 192.168.1.1  |
| Management            | VMnet5     | em3      | MGT            | 10.10.10.0/24    | 10.10.10.1   |

## EDR Design
Refer to [[EDR Design (MariaDB)|Database Diagram]]
