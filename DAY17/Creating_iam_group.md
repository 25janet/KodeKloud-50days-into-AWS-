# Day 17 – KodeKloud Challenge

## Creating an IAM Group

---

## Overview

On Day 17 of the KodeKloud challenge, the task was to create an **IAM Group** in AWS. An IAM group is a collection of IAM users that share the same permissions. Instead of assigning permissions to individual users one by one, permissions are attached to a group and automatically applied to all users within that group.

This approach simplifies access management and aligns with AWS security best practices.

---

## What is an IAM Group?

An **IAM Group** is used to manage permissions for multiple IAM users who perform similar roles or functions. For example, users such as administrators, developers, or data engineers can be grouped together and assigned permissions based on their responsibilities.

A key advantage is that a single IAM user can belong to **multiple IAM groups**, allowing flexible permission management.

---

## Importance of Using IAM Groups

* **Centralized Permission Management**
  Permissions are managed at the group level instead of individually, reducing administrative overhead.

* **Scalability**
  IAM groups make it easy to manage permissions as the number of users increases.

* **Flexibility**
  Users can belong to multiple groups and inherit permissions from each group.

* **Consistency**
  Ensures users with similar roles always have the same level of access.

* **Permission Integration**
  IAM groups integrate seamlessly with AWS managed policies and custom IAM policies.

---

## Steps to Create an IAM Group

1. Sign in to the **AWS Management Console**
2. Navigate to **IAM** → **User groups**
3. Click **Create group**
4. Enter a group name (e.g., `Developers`, `Admins`, or `DataEngineers`)
5. Attach the required IAM policies
6. Add IAM users to the group
7. Review the configuration and create the group

---

## Key Takeaway

IAM groups provide a secure, scalable, and efficient way to manage permissions for multiple users in AWS. They improve consistency, simplify permission management, and support best practices for access control.

---

**Challenge:** KodeKloud Day 17
**Service Used:** AWS Identity and Access Management (IAM)
