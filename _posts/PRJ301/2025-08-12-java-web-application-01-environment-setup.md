---
title: Epoch 1 — Project Introduction & Environment Setup
description: >-
  Epoch 1 — Project Introduction & Environment Setup
author: [shandy]
date: 2025-08-12
categories: [(Java) Web Application, (PBL) Shopping Web]
tags: [(Java - PBL) Shopping Web]
sort_index: 101
# pin: true
# media_subpath: '/posts/01'
---
## Deliverables (what each student / group must submit for this epoch)
1. Short README (in the project repo) stating installed versions (JDK, NetBeans, Tomcat, Git) and machine OS version.

2. Screenshots (or short screencast) of:
- java -version and javac -version in a new Command Prompt.
- NetBeans → Tools → Java Platforms showing the JDK entry.
- Tomcat http://localhost:8080 page in a browser.
- The test JSP page output (e.g., “Hello JSP — environment OK”).

3. A Git repository initialized (can be private) with a top-level env-check/ folder containing the README and screenshots.

4. (Optional) mvn -v output if students install Maven CLI.

Evaluation: Pass/Fail for correct setup; small points (5–10%) for timeliness and completeness.

## Installation & verification:
### 1) Install JDK 1.8_231 (Java SE 8)
1. Download JDK 8 (1.8.0_231 or equivalent) from a trusted provider (Oracle/Adoptium/etc.).

2. Run the installer and install to the default path or a path such as:

```makefile
C:\Program Files\Java\jdk1.8.0_231
```
3. Set environment variables (recommended GUI method):

Option 1:
- Open Control Panel → System → Advanced system settings → Environment Variables.
- Under System variables, click New... and create:
- Find Path → Edit → New → add:
```
C:\Program Files\Java\jdk1.8.0_231\bin
```
- Click OK, close dialogs.

Option 2: Set via an elevated Command Prompt (one-line):

```cmd
setx PATH "C:\Program Files\Java\jdk1.8.0_231\bin" /M
```

4. Open a new Command Prompt (must be new to pick up env changes) and run:

```cmd
java -version
javac -version

> Expected output should include 1.8.0_231 (or the JDK 8 build you installed).
```

### 2) Install NetBeans 13

1. Download Apache NetBeans 13 installer and run it.
2. Start NetBeans.
3. Register the JDK inside NetBeans:
- Tools → Java Platforms → Add Platform → browse to C:\Program Files\Java\jdk1.8.0_231 → Finish.
- Verify the platform shows up and is set as default (or note it).
4. Quick build test:
- File → New Project → Java → Java Application → create a tiny app and run to ensure compile/run works.
- Deliverable check: Screenshot of NetBeans → Tools → Java Platforms showing JDK path.

### 3) Install Apache Tomcat 10.0.23
1. Download Tomcat 10.0.23 ZIP distribution, extract to:

```makefile
C:\apache-tomcat-10.0.23
```

2. Edit conf/tomcat-users.xml to allow NetBeans (manager) deployment. Add users and roles, for example:
```xml
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<user username="tomcatadmin" password="ChangeMe123!" roles="manager-gui,manager-script"/>
```
- Save the file.

3. Start Tomcat:
- Double-click C:\apache-tomcat-10.0.23\bin\startup.bat or run as admin:

```cmd
cd C:\apache-tomcat-10.0.23\bin
startup.bat
```
- Check console for errors.

4. Open a browser to:

```
http://localhost:8080
```
> You should see the Tomcat welcome page.

**Firewall note:** If Tomcat page fails to appear, open Windows Firewall settings and allow inbound TCP port 8080 or allow Java/Tomcat process.

### 4) Register Tomcat in NetBeans
1. In NetBeans: Services → Servers → Add Server.
2. Choose Apache Tomcat or TomEE → Next.
3. Browse to Tomcat installation folder C:\apache-tomcat-10.0.23. Use the username/password you added in tomcat-users.xml (e.g., tomcatadmin / ChangeMe123!) if NetBeans asks. Finish.
4. Start/Stop Tomcat from NetBeans and test deployment.

>**Deliverable check:** Screenshot of NetBeans Services showing Tomcat and “Started” status.

### 5) (Optional but recommended) Install Maven CLI & verify
NetBeans includes Maven, but you may want Maven on PATH:
- Download Maven binary, extract to e.g. C:\apache-maven-3.8.x.
- Add C:\apache-maven-3.8.x\bin to PATH system variable (Environment Variables).
- Verify:

```cmd
mvn -v
```
> **Expected:** Maven version output and Java home pointing to your JDK.