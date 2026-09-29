# Mapping a Network Drive Using Group Policy in Active Directory

In this tutorial, we configure a Group Policy Object (GPO) in an Active Directory environment to automatically map a network drive for domain users. This lab uses VMware Workstation with one Domain Controller and two Windows client virtual machines to demonstrate how Group Policy can centrally manage network drive configurations across multiple domain-joined computers.

## Environments and Technologies Used

* VMware Workstation
* Windows Server
* Active Directory Domain Services (AD DS)
* Group Policy Management
* Group Policy Objects (GPOs)
* Group Policy Preferences
* Network File Sharing
* Windows Command Line

## Operating Systems Used

* Windows Server
* Windows 10

## Actions and Observations

### 1. Create the Drive Mapping GPO

* Open **Group Policy Management** on the Domain Controller.
* Right-click the domain and select:
  **Create a GPO in this domain, and Link it here...**
* Name the GPO:
  **Drive Mapping**
* Right-click the newly created GPO and select:
  **Edit**

<img src="Screenshot 2026-09-27 102522.png" width="50%" height="50%">  

<img src="Screenshot 2026-09-27 102821.png" width="50%" height="50%">  

<img src="Screenshot 2026-09-27 102833.png" width="50%" height="50%">  

### 2. Configure the Mapped Drive

* Navigate to:

  **User Configuration**
  → **Preferences**
  → **Windows Settings**
  → **Drive Maps**

* Right-click **Drive Maps** and select:
  **New → Mapped Drive**

* Configure the mapped drive with the desired:

  * **Name:** Enter the name of the mapped drive.
  * **Location:** Enter the network path of the shared folder.
  * **Drive Letter:** Select the drive letter that should represent the mapped drive.

The network path should point to a shared folder that the domain users have permission to access.  

<img src="Screenshot 2026-09-27 102902.png" width="50%" height="50%">  

<img src="Screenshot 2026-09-27 102931.png" width="50%" height="50%">  

<img src="Screenshot 2026-09-27 103129.png" width="50%" height="50%">  

<img src="Screenshot 2026-09-27 103203.png" width="50%" height="50%">  

### 3. Apply the GPO to the Appropriate OU

* Return to **Group Policy Management**.
* Navigate to:
  **Group Policy Objects**
* Locate the **Drive Mapping** GPO.
* Link the GPO to the Organizational Unit (OU) containing the users who should receive the mapped drive.
* For testing purposes, the GPO can be applied to an OU containing the lab users.

Because the drive mapping is configured under **User Configuration**, the GPO should be applied to the OU containing the targeted user accounts.  

<img src="Screenshot 2026-09-27 103249.png" width="50%" height="50%">  
  
### 4. Force a Group Policy Update

* Group Policy may take some time to automatically apply.
* To immediately request a Group Policy refresh, open **Command Prompt** on the client VM and run:

```cmd
gpupdate /force
```

* This forces the client to retrieve and process the latest applicable Group Policy settings.

<img src="Screenshot 2026-09-27 101210.png" width="50%" height="50%">  

### 5. Verify the Mapped Drive

* Log in to the client VM using a domain user located within the targeted OU.
* Open **File Explorer**.
* Navigate to:
  **This PC**
* Locate the mapped drive under **Network Locations**.

The mapped drive should now be available to the domain user without manually configuring the drive on the workstation.

* Test the GPO on additional domain-joined client VMs within the applicable OU to verify that the drive mapping is being centrally enforced.

## Results

* Successfully created a **Drive Mapping** Group Policy Object (GPO) in Active Directory.
* Configured the GPO to automatically map a network drive using **Group Policy Preferences**.
* Applied the drive mapping to users within the targeted Organizational Unit (OU).
* Successfully forced a Group Policy update using `gpupdate /force`.
* Verified that the network drive appeared automatically in **File Explorer** on the domain-joined client.
* Confirmed that the drive mapping can be centrally managed through Group Policy without manually configuring each workstation.
* This lab demonstrated how Group Policy can be used to centrally manage network resources and provide users with consistent access to shared folders across domain-joined computers.
