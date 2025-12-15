### **1. Check latest backup exists**

**Task Description:**  
Verify that a **recent, successfully completed WordPress backup** (files + database) exists in the approved backup system before initiating any WordPress core, theme, or plugin updates.

**What this means:**  
This task requires a **manual confirmation** that a WordPress backup entry is visible and complete. Do not rely on assumed schedules or automated routines.

**How to do it:**  
Open the hosting control panel or WordPress backup plugin dashboard and locate the most recent WordPress backup. Confirm the status is **completed / successful**.

**What to verify:**

- A WordPress backup entry is visible
    
- Backup completed without errors
    
- Backup includes WordPress files and database
    
- Backup is newer than the last WordPress change
    
- Backup is not failed, partial, or outdated
    

**Why it matters:**  
Without a confirmed WordPress backup, any failed update can result in **site downtime, data loss, or extended recovery time**.

**Stop Rule:**  
If a valid WordPress backup is not present → **do not proceed**. Create a new full WordPress backup and re-verify.

| **#** | **Checklist Item**                                        | **Status** |
| ----- | --------------------------------------------------------- | ---------- |
| 1     | Latest **WordPress** backup entry is visible              | ⬜          |
| 2     | Backup completed successfully (no errors)                 | ⬜          |
| 3     | WordPress files included (core, themes, plugins, uploads) | ⬜          |
| 4     | WordPress database included                               | ⬜          |
| 5     | Backup timestamp is **immediately pre-update**            | ⬜          |
| 6     | Backup size confirms full (not partial) backup            | ⬜          |
| 7     | Restore method is available and accessible                | ⬜          |
| 8     | Backup created via approved tool / hosting panel          | ⬜          |

---

| **Task**                       | **Brief**                                                                                                         |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------------- |
| **Halt all update activity**   | Immediately stop any core, theme, or plugin updates to prevent irreversible site damage without a rollback point. |
| **Confirm backup absence**     | Recheck the approved backup system to ensure the backup is truly missing and not filtered, archived, or delayed.  |
| **Identify backup method**     | Determine whether backups should exist via hosting panel or WordPress backup plugin.                              |
| **Attempt manual full backup** | Initiate a manual backup including both files and database if tools and access permit.                            |
| **Monitor backup execution**   | Observe the backup process until completion or failure; do not proceed while status is pending.                   |
| **Verify backup creation**     | Confirm the new backup is visible, completed successfully, and marked as restorable.                              |
| **Escalate if backup fails**   | Notify the client or supervisor if backup cannot be created due to access, storage, or system limitations.        |
| **Document the incident**      | Record the backup failure, actions taken, and final outcome in the maintenance log.                               |
