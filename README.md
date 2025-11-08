# Active Directory Lab Series: Creating Organizational Units and Security Groups

This repository supports the second video in the **Active Directory Lab Series**, focusing on building a domain’s internal structure using **Organizational Units (OUs)** and **Security Groups**.

The lab demonstrates real-world administration skills: building a hierarchy, delegating tasks safely using least privilege, and managing resource access.

## 📺 Watch the Full Video
For the full walkthrough, including explanations and hands-on demos: https://www.youtube.com/watch?v=brcBRFsJmMU&t=1s  

**Active Directory Lab Series: Creating Organizational Units and Security Groups**

---

## 🛠️ Prerequisites

Before starting, ensure the following environment from the previous lab is in place:

- **Domain Controller (DC):** Windows Server with AD DS installed and promoted (e.g., `mydomain.com`).
- **Client Machine:** Windows client (e.g., Windows 11) joined to the domain.
- **Tools:** Access to **Active Directory Users and Computers (ADUC)**.

---

## 📝 Lab Steps

This lab contains six major steps: OU organization, user creation, group management, delegation, and access control.

---

## Step 1: Building the Core Location OUs

Create the top-level OUs directly under the root domain (`mydomain.com`). Organizing by location allows granular GPO targeting.

| Location OU | Action |
|-------------|--------|
| New York    | Right-click domain > New > Organizational Unit |
| California  | Right-click domain > New > Organizational Unit |
| Florida     | Right-click domain > New > Organizational Unit |

---

## Step 2: Creating Sub-OUs for Object Separation

Under each location (New York, California, Florida), create the following sub-OUs:

| Sub-OU Name     | Purpose |
|------------------|---------|
| Users            | Holds user accounts |
| Computers        | Holds computer accounts |
| Groups           | Holds security and distribution groups |
| Service Accounts | Holds service accounts |
| IT               | IT department users/groups |
| HR               | HR department users/groups |

Repeat this structure for each location.

---

## Step 3: Creating and Placing Sample User Accounts

Use the password **Password1**, with **Password never expires** enabled.

| Name | Logon | Location/Dept. OU |
|------|--------|--------------------|
| John Smith | jsmith | New York > IT > Users |
| Sarah Johnson | sjohnson | New York > IT > Users |
| Emily Davis | edavis | New York > HR > Users |
| Michael Brown | mbrown | New York > HR > Users |
| David Wilson | dwilson | California > IT > Users |
| Laura Martinez | lmartinez | California > IT > Users |
| Jennifer Lee | jlee | California > HR > Users |
| Brian Garcia | bgarcia | California > HR > Users |
| Kevin Thomas | kthomas | Florida > IT > Users |
| Rachel Moore | rmoore | Florida > IT > Users |
| Megan Taylor | mtaylor | Florida > HR > Users |
| Daniel Anderson | danderson | Florida > HR > Users |

---

## Step 4: Creating and Assigning Security Groups

Create the following Security Groups under each location’s **Groups** OU.

| Group Name | Location | Members |
|------------|-----------|---------|
| New York_All Users | New York > Groups | jsmith, sjohnson, edavis, mbrown |
| New York_IT Admins | New York > Groups | jsmith, sjohnson |
| New York_HR Staff | New York > Groups | edavis, mbrown |
| California_All Users | California > Groups | dwilson, lmartinez, jlee, bgarcia |
| California_IT Admins | California > Groups | dwilson, lmartinez |
| Florida_All Users | Florida > Groups | kthomas, rmoore, mtaylor, danderson |
| Florida_IT Admins | Florida > Groups | kthomas, rmoore |

Security groups are used to manage access and user rights.

---

## Step 5: Delegating Administrative Control (Least Privilege)

Demonstrate selective delegation for HR.

### Delegate Control
1. Right-click **New York > HR** OU.  
2. Select **Delegate Control…**  
3. Add **Emily Davis (edavis)**.  
4. Select: **Reset user passwords and force password change at next logon**

### Testing the Delegation

**Success Test:**  
- Sign in as Emily Davis.  
- Install RSAT.  
- Emily resets **Michael Brown’s** password (allowed).

**Failure Test:**  
- Emily attempts to reset **John Smith’s** password (denied).  
- Confirms access is restricted to HR OU.

---

## Step 6: Securing a Shared Folder Using Security Groups

### Create the Shared Folder
- On the DC, create: `C:\New York HR Share`

### Configure Sharing Permissions
- Properties > Sharing > Advanced Sharing  
- Share the folder  
- Grant **New York_HR Staff** *Full Control*

### Configure Security (NTFS) Permissions
- Properties > Security  
- Disable inheritance  
- Remove **Domain Users**  
- Grant **New York_HR Staff**:  
  - Modify  
  - Read & execute  
  - List folder contents  
  - Read  
  - Write  

### Test Access
- **Emily Davis** (HR Staff) can access `\\DC\New York HR Share`  
- **John Smith** (not HR) receives *Permission Denied*

---

This completes the structured setup of locations, users, groups, delegation, and access control.
