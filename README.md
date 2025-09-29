<p align="center">
  <img src="https://github.com/jorge-nbb/images/raw/main/logo-azure-ad.png" alt="ad logo" width="300"/>
</p>

<h1 align="center">Group Policy Management</h1>

---

## 🎯 Project Purpose

- Create and link a GPO that blocks access to *Control Panel* on domain-joined client machines
- Demonstrate how Group Policy Objects (GPOs) can centrally enforce user restrictions

  ---

  ## 🛠️ Tools & Services Used

- **Windows Server 2022 (DC-Server)**
- **Windows 10 Pro (Client-PC)**
- **Active Directory Domain Services**
- **Group Policy Management Console (GPMC)**

  ---

  ## ⚙️ Configuration Steps

### STEP 1: Install Group Policy Management Console (GPMC)

<table>
  <tr>
    <td style="vertical-align: top; width: 60%;">

1. Log into **DC-Server**
2. Open **Server Manager** > Click **Add Roles and Features**
3. Click Next until you reach **Features**
4. Scroll down and check *Group Policy Management*
5. Click Next > Install (no reboot needed)
   
</td>
<td style="vertical-align: top; text-align: right; width: 40%;">
  <img src="https://github.com/jorge-nbb/images/blob/main/p36.png" alt="36" width="350"/>
</td>
  </tr>
</table>

---

### STEP 2: Create a New GPO

<table>
  <tr>
    <td style="vertical-align: top; width: 60%;">

1. Open **Group Policy Management** from **Tools** menu
2. Expand Forest > Domains > adminlab.local
3. Right-click domain > **Create a GPO in this domain, and Link it here**
4. Name: `Test GPO - Block Control Panel`
5. Click **OK**

</td>
<td style="vertical-align: top; text-align: right; width: 40%;">
<img src="https://github.com/jorge-nbb/images/blob/main/p37.png" alt="37" width="350"/>
   <hr>
<img src="https://github.com/jorge-nbb/images/blob/main/p37.1.png" alt="37.1" width="350"/>
</td>
</tr>
</table>

---

### STEP 3: Link the GPO to the Domain

<table>
<tr>
  <td style="vertical-align: top; width: 60%;">

1. Right-click the GPO you created > **Edit** 
2. Navigate to: User Configuration > Administrative Templates > Control Panel  
3. Double-click **Prohibit access to Control Panel and PC settings**  
4. Select **Enabled** > Click **OK** 

</td>
<td style="vertical-align: top; text-align: right; width: 40%;">
<img src="https://github.com/jorge-nbb/images/blob/main/p38.png" alt="38" width="350"/>
</td>
</tr>
</table>

---

### STEP 4: Confirm Scope and Permissions

<table>
<tr>
 <td style="vertical-align: top; width: 60%;">

1. Select the GPO and go to the **Scope** tab  
2. Confirm that `Authenticated Users` is listed under **Security Filtering** ✅  
3. Go to the **Delegation > Advanced** tab  
4. Ensure `Authenticated Users` has **Read** and **Apply Group Policy** permissions ✅  

⚠️ Do **not** remove `Authenticated Users` unless you are configuring more advanced targeting.

</td>
<td style="vertical-align: top; text-align: right; width: 40%;">
<img src="https://github.com/jorge-nbb/images/blob/main/p39.png" alt="39" width="350"/>
</td>
</tr>
</table>

---

### STEP 5: Apply Policy to Domain User

<table>
<tr>
 <td style="vertical-align: top; width: 60%;">

1. Confirm `testuser` is located in the default **Users** container
   - *(No need to move it to a new OU)*  
2. Log into **Client-PC** as `testuser`  
3. Open **Command Prompt** and run:
```
gpupdate /force
```
4. Reboot or log off/on
   
</table>

---

### STEP 6: Verify GPO Applied

<table>
<tr>
 <td style="vertical-align: top; width: 60%;">

1. On Client-PC, log in as `testuser`
2. Open Command Prompt and run:
   ```
   gpresult /r
   ```
4. Under **Applied Group Policy Objects**, confirm that: `Test GPO - Block Control Panel` is listed ✅
5. Attempt to open **Control Panel**
   - There should see a restriction message

</td>
<td style="vertical-align: top; text-align: right; width: 40%;">
<img src="https://github.com/jorge-nbb/images/blob/main/p40.png" alt="40" width="350"/>
</td>
</tr>
</table>

---

## 📝 Results

- Group Policy Management Console (GPMC) installed on DC-Server
- GPO created, linked, and applied to domain users
- `testuser` blocked from accessing the Control Panel
- Demonstrated successful policy enforcement through Group Policy

---

