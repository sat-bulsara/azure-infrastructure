🔐 Azure Identity & RBAC Lab (CLI)

---

# 🧪 Lab Overview

## 🎯 Problem

Businesses often assign access directly to users, leading to:

- Poor access control visibility  
- Difficult permission management  
- Increased risk of over-permissioned users  

---

## 🏗️ Solution

I implemented a group-based access control model in Azure:

- Created a user identity  
- Placed the user into a group  
- Assigned permissions to the group instead of the user  
- Validated access behaviour using break/fix testing  

---

## ⚙️ Key Components

- Microsoft Entra ID (Users & Groups)  
- Azure RBAC (Reader role)  
- Azure Resource Group  
- Azure CLI  

---

## 🧠 Decisions / Thinking

- Used group-based RBAC instead of direct user assignment  
- Applied least privilege using Reader role  
- Validated access through break/fix testing  
- Used CLI to simulate real-world workflows  

---

## 💥 Outcome

- Improved access control structure  
- Demonstrated RBAC inheritance  
- Validated how access is granted and removed  
- Built a reusable access control pattern  

---

# 🧪 Implementation

---

## 👤 Step 1 — Create User

### 🎯 Problem
Manual identity creation is required for access control testing.

### 🏗️ Solution
Created a native Entra ID user using Azure CLI.

### ⚙️ Commands
```bash
az ad user create --display-name LabUser --user-principal-name lab-user1@satonthathotmailco.onmicrosoft.com --password 'TempPassword123!' --force-change-password-next-sign-in true
```

```bash
az ad user show --id lab-user1@satonthathotmailco.onmicrosoft.com
```

📸 Screenshots:
- [ ] User created
- [ ] User verified

---

## 👥 Step 2 — Create Group

### 🎯 Problem
Managing permissions per user does not scale.

### 🏗️ Solution
Created a security group to manage access centrally.

### ⚙️ Commands
```bash
az ad group create --display-name lab-group --mail-nickname labgroup
```

```bash
az ad group show --group lab-group
```

📸 Screenshots:
- [ ] Group created
- [ ] Group verified

---

## 🔗 Step 3 — Add User to Group

### 🎯 Problem
User has no access until linked to a permission structure.

### 🏗️ Solution
Added user to group to inherit permissions.

### ⚙️ Commands
```bash
az ad group member add --group lab-group --member-id $(az ad user show --id lab-user1@satonthathotmailco.onmicrosoft.com --query id -o tsv)
```

```bash
az ad group member list --group lab-group
```

📸 Screenshots:
- [ ] Membership verified

---

## 🏗️ Step 4 — Create Resource Group

### 🎯 Problem
RBAC requires a resource scope.

### 🏗️ Solution
Created a resource group to apply RBAC.

### ⚙️ Commands
```bash
az group create --name rg-lab-f1 --location uksouth
```

```bash
az group show --name rg-lab-f1
```

📸 Screenshots:
- [ ] Resource group created

---

## 🔐 Step 5 — Assign RBAC

### 🎯 Problem
User still has no access to resources.

### 🏗️ Solution
Assigned Reader role to the group at resource group scope.

### ⚙️ Commands
```bash
az role assignment create --assignee-object-id $(az ad group show --group lab-group --query id -o tsv) --assignee-principal-type Group --role Reader --scope $(az group show --name rg-lab-f1 --query id -o tsv)
```

```bash
az role assignment list --scope $(az group show --name rg-lab-f1 --query id -o tsv) --output table
```

📸 Screenshots:
- [ ] RBAC assigned

---

## 🧪 Step 6 — Break / Fix

### 🎯 Problem
Need to validate how access is controlled.

### 🏗️ Solution
Removed and re-added user to group to test access inheritance.

### ⚙️ Commands
```bash
az ad group member remove --group lab-group --member-id $(az ad user show --id lab-user1@satonthathotmailco.onmicrosoft.com --query id -o tsv)
```

```bash
az ad group member add --group lab-group --member-id $(az ad user show --id lab-user1@satonthathotmailco.onmicrosoft.com --query id -o tsv)
```

📸 Screenshots:
- [ ] User removed
- [ ] User restored

---

## 🧠 Key Learnings

- Authentication = identity  
- Authorization = permissions  
- RBAC should be group-based  
- Access is inherited  
- Removing group membership removes access  
"""

