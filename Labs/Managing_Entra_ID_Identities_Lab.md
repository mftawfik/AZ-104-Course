
# Lab: Managing Entra ID Identities with Azure Cloud Shell

## Objective
Manage Entra ID identities by creating and configuring users, groups, and permissions using PowerShell and Bash in Azure Cloud Shell.

## Pre-requisites
- Access to an Azure subscription
- Access to Azure Cloud Shell

---

## Lab Steps

### Task 1: User Management
1. **Launch Cloud Shell**
   - Access **Azure Cloud Shell** and select **PowerShell**.

2. **Create a New User**
   - **PowerShell**:
     ```powershell
     New-AzADUser -UserPrincipalName "newuser@yourdomain.com" -DisplayName "New User" -PasswordProfile (New-Object -TypeName Microsoft.Open.AzureAD.Model.PasswordProfile -Property @{ Password = "UserPassword" })
     ```
   - **Bash**:
     ```bash
     az ad user create --display-name "New User" --user-principal-name "newuser@yourdomain.com" --password "UserPassword"
     ```

3. **List Existing Users**
   - **PowerShell**:
     ```powershell
     Get-AzADUser
     ```
   - **Bash**:
     ```bash
     az ad user list --output table
     ```

---

### Task 2: Group Management
1. **Create a New Group**
   - **PowerShell**:
     ```powershell
     New-AzADGroup -DisplayName "HR Team" -MailNickname "HRTeam"
     ```
   - **Bash**:
     ```bash
     az ad group create --display-name "HR Team" --mail-nickname "HRTeam"
     ```

2. **Add a User to a Group**
   - **PowerShell**:
     ```powershell
     Add-AzADGroupMember -TargetGroupDisplayName "HR Team" -MemberUserPrincipalName "newuser@yourdomain.com"
     ```
   - **Bash**:
     ```bash
     az ad group member add --group "HR Team" --member-id $(az ad user show --id "newuser@yourdomain.com" --query objectId -o tsv)
     ```

---

### Task 3: Permissions and Role Assignments
1. **Assign a Role to a User**
   - **PowerShell**:
     ```powershell
     New-AzRoleAssignment -RoleDefinitionName "Reader" -UserPrincipalName "newuser@yourdomain.com"
     ```
   - **Bash**:
     ```bash
     az role assignment create --assignee "newuser@yourdomain.com" --role "Reader"
     ```

---

### Task 4: Review and Clean Up
1. **List Group Members**
   - **PowerShell**:
     ```powershell
     Get-AzADGroupMember -TargetGroupDisplayName "HR Team"
     ```
   - **Bash**:
     ```bash
     az ad group member list --group "HR Team" --output table
     ```

2. **Delete User and Group**
   - **PowerShell**:
     ```powershell
     Remove-AzADUser -UserPrincipalName "newuser@yourdomain.com"
     Remove-AzADGroup -DisplayName "HR Team"
     ```
   - **Bash**:
     ```bash
     az ad user delete --id "newuser@yourdomain.com"
     az ad group delete --group "HR Team"
     ```

---

This structured approach should help you complete each task efficiently in Azure Cloud Shell, utilizing both PowerShell and Bash as needed.
