---
tags:
  - active-directory
  - kerberos
  - privilege-escalation
  - networking
  - post-exploitation
aliases:
  - Kerberoasting
---

# Active Directory & Kerberoasting

## 1. Core Architecture
**Active Directory (AD)** is a centralized identity and access management system created by Microsoft. Under the hood, it is a proprietary wrapper around three standard protocols:
*   **LDAP:** The database (stores users, groups, computers, permissions).
*   **Kerberos:** The authentication protocol (issues cryptographic tickets).
*   **DNS:** The routing protocol (helps computers find the controller).

**Domain Controller (DC):** The central server that runs AD. It acts as the ultimate authority on the network. If an attacker compromises the DC, they compromise the entire domain.

## 2. Authentication vs. Authorization (The Big Flaw)
Kerberos architecture strictly separates proving *who you are* from checking *what you can do*.

*   **Authentication (The DC):** Validates credentials and issues access tickets. It acts as a ticket booth. It **does not** check if you actually have permissions to use the target service.
*   **Authorization (The Target Service):** The end-service (e.g., SQL server, File Share) receives your ticket, decrypts it, reads your identity, and decides whether to let you in based on its own internal access control lists.

## 3. The Kerberos Ticket System
*   **TGT (Ticket Granting Ticket):** "General Admission." You get this when you log into your machine with your password. It proves you are a valid domain user.
*   **TGS (Ticket Granting Service):** "VIP Ticket." You request this from the DC when you want to access a specific network service. 
*   **SPN (Service Principal Name):** The unique identifier (the exact name) of the service you are requesting a TGS for (e.g., `MSSQLSvc/db-server-01:1433`).

## 4. The Kerberoasting Attack
**Goal:** Extract encrypted TGS tickets from the network and brute-force them offline to steal highly privileged service account passwords.

### Target Selection: Machine vs. User Accounts
Services in AD are tied to one of two account types. Attackers **only** target User Accounts.

| Account Type | Password Origin | Vulnerability |
|---|---|---|
| **Machine Account (`$`)** | Auto-generated, 120+ chars, rotates every 30 days | Uncrackable |
| **User Service Account** | Manually set by humans, often weak/reused | **Highly Vulnerable** |

### The Attack Flow
1. **Reconnaissance (LDAP):** The attacker queries the DC for all accounts that have an `SPN` registered **AND** are `User` accounts (filtering out machine accounts).
2. **Ticket Request (Kerberos):** The attacker presents their TGT to the DC and requests TGS tickets for those specific SPNs. The DC blindly complies.
3. **Extraction:** The DC encrypts the TGS using the **NTLM password hash of the target service account**. The attacker saves this encrypted ticket to memory.
4. **Offline Cracking:** The attacker extracts the tickets, takes them off the network (leaving no failed login logs), and uses tools like `Hashcat` to brute-force the encryption lockbox. 

> **Key Takeaway:** The attacker never actually connects to the target service (e.g., the SQL database). The vulnerability lies entirely in the DC's willingness to hand out encrypted tickets to any authenticated user who asks.