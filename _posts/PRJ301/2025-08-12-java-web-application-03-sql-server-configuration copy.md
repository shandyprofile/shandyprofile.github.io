---
title: Epoch 3 - SQL Server Configuration
description: >-
  Set up SQL Server so the JSP Shop project can connect successfully, using TCP/IP and System Administrator (SA) authentication mode.
author: [shandy]
date: 2025-08-12
categories: [(Java) Web Application, (PBL) Shopping Web]
tags: [(Java - PBL) Shopping Web]
sort_index: 103
# pin: true
# media_subpath: '/posts/01'
---
## 1. Open SQL Server Configuration Manager

- Press Windows + S and search for:

  - "SQL Server Configuration Manager"
(Exact name may vary depending on SQL Server version: e.g., SQL Server 2019 Configuration Manager)

- Open it — you’ll see multiple configuration options for network protocols.

## 2. Enable TCP/IP Protocol
- In the left pane, navigate to:
  - SQL Server Network Configuration → Protocols for MSSQLSERVER
  > (If you installed a named instance, it will show as Protocols for `<InstanceName>`)

- In the right pane, locate TCP/IP → Right-click → Enable.
- Click OK if prompted.

## 3. Configure TCP/IP Port
- Double-click TCP/IP to open Properties.
- Go to the IP Addresses tab.
- Scroll down to IPAll.
- In the TCP Port box, enter: **1433**
> *(Default port for SQL Server TCP/IP)*
- Click OK.

## 4. Restart SQL Server Service
- In SQL Server Configuration Manager, click SQL Server Services.
- Find your instance name, e.g.:
- SQL Server (MSSQLSERVER) (Default instance)
- Right-click → Restart.

## 5. Enable Mixed Mode Authentication (SA Account)
- Open SQL Server Management Studio (SSMS).
- Connect to your server instance as Windows Authentication.
- Right-click on the server name in Object Explorer → Properties.
- Go to Security → Select SQL Server and Windows Authentication mode.
- Click OK.
- Restart the SQL Server service again.

## 6. Enable and Set Password for SA Account
1. In SSMS, expand: Security → Logins
2. Right-click sa → Properties.
3. On General: Set a strong password (e.g., P@ssword123)
4. On Status: Ensure Login is set to Enabled.
5. Click OK.

## 7. Test the Connection
1. Open SSMS.
2. On login screen:
3. Server name: localhost,1433
4. Authentication: SQL Server Authentication
   - Login: sa
   - Password: (your password)
5. Click Connect — if successful, the configuration is correct.