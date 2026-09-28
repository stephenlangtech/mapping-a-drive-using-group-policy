# Mapping a Network Drive Using Group Policy in Active Directory

In this tutorial, we configure a Group Policy Object (GPO) in an Active Directory environment to automatically map a network drive for domain users. This lab uses VMware Workstation with one Domain Controller and two Windows client virtual machines to demonstrate how Group Policy can centrally manage user configurations across multiple domain-joined computers.

## Environments and Technologies Used

- VMware Workstation
- Windows Server
- Active Directory Domain Services (AD DS)
- Group Policy Management
- Group Policy Objects (GPOs)
- Windows Command Line
- Network File Sharing

## Operating Systems Used

- Windows Server
- Windows 10

## Actions and Observations

### 1. Create the Drive Mapping GPO

- Open **Group Policy Management** on the Domain Controller.  
- Right-click the domain and select: **Create a GPO in this domain, and Link it here...**  
- Name the GPO: Drive Mapping   
- Right-click the newly created GPO and select:  Edit

### 2. Configure the Mapped Drive

- Navigate to:  
User Configuration  
    → Preferences  
        → Windows Settings  
            → Drive Maps  
- Right-click Drive Maps and select:  
New → Mapped Drive  
- Configure the mapped drive with the desired:  
Network path  
Drive letter  
Drive name  
- For example:  
Drive Letter: Z:  
Location: \\SERVER\SharedFolder  
- The network path should point to the shared folder being mapped.

### 3. Apply the GPO to the Appropriate Users

- Return to Group Policy Management.  
- Locate the Organizational Unit (OU) containing the users that should receive the mapped drive.  
- Link the Drive Mapping GPO to the appropriate OU.  
- Because the drive mapping is configured under User Configuration, the GPO should be applied to the OU containing the target user accounts.
- Example Active Directory structure:  
Domain  
│  
├── Users  
│   └── Test User  
│  
└── Computers  
    ├── CLIENT-1  
    └── CLIENT-2
  
### 4. Force a Group Policy Update

- Group Policy may take some time to automatically apply.  
- To immediately request a Group Policy refresh, open Command Prompt on the client VM and run:

gpupdate /force

- This forces the computer to retrieve and process the latest applicable Group Policy settings.  

### 5. Verify the Mapped Drive

- After the Group Policy update completes, log in as a domain user that is located within the targeted OU.  
Open File Explorer and navigate to:    
This PC  
- The configured drive should appear under Network Locations.  
Example:  
Z:  Shared Drive  
- The mapped drive should now be available to the domain user without manually configuring the drive on each workstation.  

Results:  
The Drive Mapping Group Policy Object successfully mapped a network drive to domain users.
This demonstrates how Active Directory and Group Policy can be used to centrally manage user configurations across multiple domain-joined computers rather than manually configuring each workstation.
