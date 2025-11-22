# On-prem AD+ Exchange Hybird Lab
![AD](https://img.shields.io/badge/Active%20Directory-Expert-blue)
![Exchange](https://img.shields.io/badge/Exchange%202019-DAG%20%7C%20Hybrid-orange)
![AzureAD](https://img.shields.io/badge/Azure%20AD-DirSync%20%7C%20Oauth-blueviolet)
![AADConnect](https://img.shields.io/badge/AAD%20Connect-Sync-green)
![HCW](https://img.shields.io/badge/Hybrid%20Config%20Wizard-HCW-yellow)
![DNS](https://img.shields.io/badge/DNS-Public%20%7C%20Split%20Brain-lightgrey)
![SSL](https://img.shields.io/badge/SSL-Namecheap%20PFX-red)
![WireGuard](https://img.shields.io/badge/WireGuard-VPN%20Gateway-success)
![Linux](https://img.shields.io/badge/Linux-Ubuntu%20iptables-lightgrey)
![Networking](https://img.shields.io/badge/Networking-NAT%20%7C%20Routing%20%7C%20P2S-blue)
![Azure](https://img.shields.io/badge/Azure-P2S%20VPN%20%7C%20VM%20Lab-0089D6)
![Windows](https://img.shields.io/badge/Windows%20Server-2019%20%7C%202022-0078D6)

Home lab setup: Exchange 2019 DAG on Windows Server 2019 with AD DS

+-------------------------------------------------------------------------------------------------------+
| Layer                | Component / Role                     | Hostname        | IP Address            |
+-------------------------------------------------------------------------------------------------------+
| Client               | Outlook / OWA / ActiveSync           | Client PC       | 192.168.10.x          |
|                      |                                       |                 |                       |
+-------------------------------------------------------------------------------------------------------+
| Identity / DNS       | AD DS + DNS + File Share Witness     | WIN2019AD       | 192.168.10.50         |
|                      | - Domain Controller                   |                 |                       |
|                      | - DNS Server                          |                 |                       |
|                      | - DAG Witness: C:\DAGFileShare        |                 |                       |
+-------------------------------------------------------------------------------------------------------+
| Exchange Server 1    | Mailbox + Client Access Role         | MSEX2019        | 192.168.10.62         |
|                      | - Member of DAG01                     |                 |                       |
|                      | - HCW                                 |                 |                       |
+-------------------------------------------------------------------------------------------------------+
| Exchange Server 2    | Mailbox + Client Access Role         | WIN-EX2019      | 192.168.10.60         |
|                      | - Member of DAG01                     |                 |                       |
+-------------------------------------------------------------------------------------------------------+
| DAG Cluster          | Database Availability Group (DAG01)  | DAG01           | 192.168.10.70 *       |
|                      | - 2 Mailbox Servers                    |                 |                       |
|                      |   * MSEX2019                           |                 |                       |
|                      |   * WIN-EX2019                         |                 |                       |
|                      | - IP-less DAG (Exchange 2019)         |                 |                       |
+-------------------------------------------------------------------------------------------------------+
| Mail Namespace       | DNS for Outlook/OWA                   | mail.hk2uk.online | 192.168.10.62        |
|                      | DNS Round Robin Entry                 |                   | 192.168.10.60        |
+-------------------------------------------------------------------------------------------------------+



                         +---------------------------------+
                         |      Microsoft 365 Tenant       |
                         |  Exchange Online (EXO)          |
                         |  Azure AD (cloud identity)      |
                         +-----------------+---------------+
                                           ^
                                           | 3. DirSync (Azure AD Connect)
                                           |
+--------------------+     2. HTTPS / SMTP |       +-------------------------+
| On-prem Exchange   |<--------------------+------>|  Azure AD Connect Host  |
| 2019 DAG (DAG01)   |                           |  (could be WIN2019AD or  |
| MSEX2019 / WIN-EX2019                          |  another member server)   |
+---------+----------+                           +-----------+-------------+
          ^                                                  ^
          | 1. On-prem identities / groups                   |
          |                                                  |
+---------+----------+                             +---------+----------+
|  WIN2019AD         |                             |   Public DNS /     |
|  AD DS + DNS       |                             |   hk2uk.online     |
+--------------------+                             +--------------------+
            ^                                                     ^
            | 4. Clients use same namespace                       |
            |    (mail.hk2uk.online / autodiscover.hk2uk.online)  |
            |                                                     |
        +---+---------------------+                               |
        | Outlook / OWA Clients   |-------------------------------+
        +-------------------------+

