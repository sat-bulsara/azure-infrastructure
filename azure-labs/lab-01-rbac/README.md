# 🔐 Azure Identity & RBAC (CLI)

---

## 🎯 Problem

In many environments, access is assigned directly to users.

This creates:
- Poor visibility
- Difficult permission management
- Over-permissioned identities

---

## 🏗️ Solution

I implemented a group-based RBAC model:

User → Group → Role → Resource

---

# 🧪 Implementation Walkthrough

---

## 👤 Step 1 — Tenant Context

```bash
az account show --query "{tenantId:tenantId, user:user.name}"
az ad signed-in-user show --query userPrincipalName
```

📸 Evidence:

<img width="742" height="369" alt="Screenshot 2026-03-29 at 18 41 21" src="https://github.com/user-attachments/assets/26e34382-c861-4063-922a-f542e1809e84" />

---

## 👤 Step 2 — User Verification

```bash
az ad user show --id lab-user1@satonthathotmailco.onmicrosoft.com
```

📸 Evidence:

<img width="785" height="392" alt="Screenshot 2026-03-29 at 19 35 07" src="https://github.com/user-attachments/assets/4e8ef249-9745-48d6-ad25-403cd07a4479" />

---

## 👥 Step 3 — Group Creation

```bash
az ad group show --group lab-group
```

📸 Evidence:

<img width="633" height="717" alt="Screenshot 2026-03-29 at 19 49 03" src="https://github.com/user-attachments/assets/f5546ffe-ac70-404c-ba48-85198224d7b5" />

---

## 🔗 Step 4 — Group Membership

```bash
az ad group member list --group lab-group
```

📸 Evidence:

<img width="625" height="364" alt="Screenshot 2026-03-29 at 19 51 17" src="https://github.com/user-attachments/assets/2d038104-741a-440c-8477-44e46c563fb8" />

---

## 🏗️ Step 5 — Resource Group

```bash
az group show --name rg-lab-f1
```

📸 Evidence:

<img width="629" height="374" alt="Screenshot 2026-03-29 at 20 01 12" src="https://github.com/user-attachments/assets/22259c9a-f221-4301-8e4f-c01bc295574b" />

---

## 🔐 Step 6 — RBAC Assignment

```bash
az role assignment list --scope $(az group show --name rg-lab-f1 --query id -o tsv) --output table
```

📸 Evidence:

<img width="626" height="347" alt="Screenshot 2026-03-29 at 20 05 02" src="https://github.com/user-attachments/assets/4d55c28f-ec53-4996-9f77-0879657bd3db" />
02.png)

---

## 🧪 Step 7 — Break/Fix

```bash
az ad group member remove --group lab-group --member-id <user-id>
az ad group member add --group lab-group --member-id <user-id>
```

📸 Evidence:

<img width="790" height="515" alt="Screenshot 2026-03-29 at 20 09 43" src="https://github.com/user-attachments/assets/d808919a-a3c1-45c1-8a72-56a5d2d8728c" />

---

## 🧠 Key Learnings

- Authentication vs Authorization
- RBAC via groups
- Inheritance model
- Immediate access removal
