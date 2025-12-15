## **How to Approach: Full Backup Before Updating**

Approach this step with the mindset that **every update is potentially destructive** and that a backup is the **only guaranteed escape path**. Do not treat backups as a routine checkbox or rely on assumptions such as “there is already a scheduled backup.” The objective is to create a **fresh, complete, verifiable, and restorable snapshot** of the site immediately before any change is made.

Work slowly and deliberately. Ensure the site state is stable, use only one trusted backup method, and personally verify that the backup includes both files and database. Always confirm off-site storage and restore readiness. If at any point a backup cannot be verified or restored with confidence, the process must stop. Proceeding without a confirmed rollback is considered a process violation, not a judgment call.

## **STEP 1 — Full Backup Before Updating (Topics Overview)**

| Topic                               | Responsibility                                   | Brief                                       |
| ----------------------------------- | ------------------------------------------------ | ------------------------------------------- |
| **1. Select backup method**         | Choose one approved backup tool or hosting panel | Prevents inconsistent or mismatched backups |
| **2. Stabilize site state**         | Pause auto-updates and content changes           | Ensures backup consistency                  |
| **3. Execute manual full backup**   | Create on-demand files + database backup         | Establishes rollback point                  |
| **4. Monitor backup completion**    | Observe backup until successful finish           | Detects failures early                      |
| **5. Verify backup completeness**   | Confirm files + DB exist and size is reasonable  | Ensures backup usability                    |
| **6. Confirm storage & redundancy** | Verify server and off-site copies                | Protects against server loss                |
| **7. Confirm restore readiness**    | Ensure restore path is clear and accessible      | Enables fast rollback                       |
| **8. Verify backup freshness**      | Confirm backup is recent and pre-update          | Avoids outdated restores                    |
| **9. Record backup confirmation**   | Log backup details for traceability              | Enables team verification                   |

> _A backup is only valid if it can be restored with confidence.  
> If rollback is uncertain, updates must not proceed.

---
### **1. Select Backup Method**

`purpose: establish a single, reliable rollback mechanism`

**What:**  
The employee must select **one approved backup method** (either a trusted backup plugin _or_ the hosting control panel). Using multiple tools for the same backup is strictly prohibited.

**How (Instructions for New Employees):**

1. Decide whether the backup will be taken using:
    
    - The hosting provider’s backup system, **or**
        
    - One approved WordPress backup plugin.
        
2. Confirm the selected method supports:
    
    - Full website files
        
    - Full database backup
        
3. Use **only this method** for the entire backup operation.
    

**Training Note:**  
Using more than one backup method can create mismatched backups that cannot be restored safely.


### **2. Stabilize Site State**

`purpose: prevent data inconsistency during backup creation`

**What:**  
Before starting the backup, the employee must ensure the site is in a **stable, non-changing state**.

**How (Instructions for New Employees):**

1. Disable WordPress automatic updates.
    
2. Avoid editing pages, posts, or settings.
    
3. Pause scheduled tasks (cron jobs) if access is available.
    
4. Enable maintenance mode if the site has active visitors.
    

**Training Note:**  
A backup taken while the site is changing may be incomplete or unreliable.

### **3. Execute Manual Full Backup**

`purpose: create a reliable and complete restore point`

**What:**  
The employee must run an **on-demand manual backup** that includes all WordPress files and the complete database. Scheduled or automatic backups do not qualify for this step.

**How (Instructions for New Employees):**

1. Open the selected backup tool or hosting panel.
    
2. Start a **new manual backup** (do not wait for a schedule).
    
3. Explicitly include:
    
    - WordPress core files
        
    - Plugins
        
    - Themes
        
    - Uploads
        
    - Full database (all tables)
        
4. Confirm the backup job starts immediately.
    

**Training Note:**  
If files or database are excluded, the backup cannot be trusted for rollback.

### **4. Monitor Backup Completion**

`purpose: ensure the backup finishes successfully and without errors`

**What:**  
The employee must actively monitor the backup process until it completes.

**How (Instructions for New Employees):**

1. Stay on the backup progress screen while the job is running.
    
2. Watch for warning messages, timeouts, or skipped items.
    
3. Wait for a clear **successful completion** message.
    
4. Do not leave the task unattended until completion is confirmed.
    

**Training Note:**  
An unfinished or failed backup is equivalent to no backup.

### **5. Verify Backup Completeness**

`purpose: confirm the backup can fully restore the site`

**What:**  
After completion, the employee must verify that the backup includes **both website files and the full database**, and that the backup appears complete and usable.

**How (Instructions for New Employees):**

1. Open the backup record or backup details screen.
    
2. Confirm that:
    
    - A file archive is listed (files backup)
        
    - A database dump is listed (database backup)
        
3. Check the backup size and ensure it looks reasonable compared to the site’s known size.
    
4. Ensure no components are marked as missing or incomplete.
    

**Training Note:**  
A backup missing either files or database cannot restore the site and must not be accepted.

---

### **6. Confirm Storage and Redundancy**

`purpose: protect backups from server-level failure`

**What:**  
The employee must verify that the backup is stored **both locally on the server and off-site**, and that the off-site copy is accessible.

**How (Instructions for New Employees):**

1. Confirm the backup exists in the server or hosting storage.
    
2. Check the connected off-site destination (cloud storage or remote backup).
    
3. Verify the same backup appears there without errors.
    
4. Ensure storage limits or quotas are not exceeded.
    

**Training Note:**  
A backup stored only on the same server is vulnerable to total loss.Understood, Boss.  
Continuing with **STEP 1 — Task Declaration & Responsibilities**, **final two items**.

---

### **7. Confirm Restore Readiness**

`purpose: ensure rollback can be executed quickly if needed`

**What:**  
The employee must confirm that the backup can be **restored immediately** if a rollback is required.

**How (Instructions for New Employees):**

1. Locate the **restore / rollback** option in the backup tool or hosting panel.
    
2. Review the restore steps and confirm they are clear and accessible.
    
3. Ensure no additional configuration, credentials, or approvals are required to start a restore.
    
4. Confirm that restore can be initiated even if the site becomes partially inaccessible.
    

**Training Note:**  
A backup without a clear restore path is not a usable backup.


### **8. Verify Backup Freshness**

`purpose: ensure rollback reflects the current site state`

**What:**  
The employee must confirm the backup was created **immediately before** starting the update process.

**How (Instructions for New Employees):**

1. Check the backup timestamp.
    
2. Confirm it was created within the last few minutes.
    
3. Ensure it is not an older scheduled or automatic backup.
    

**Training Note:**  
Old backups create a false sense of safety and may cause data loss.

### **9. Record Backup Confirmation**

`purpose: ensure traceability and team-wide rollback confidence`

**What:**  
The employee must formally record that **STEP 1 — Full Backup Before Updating** has been completed and verified.

**How (Instructions for New Employees):**

1. Record the backup **date and time**.
    
2. Record the **backup method used** (plugin name or hosting panel).
    
3. Note the **storage locations** (server and off-site).
    
4. Save this information in the task log, ticket, or internal documentation.
    

**Training Note:**  
If a backup is not recorded, other team members cannot confidently verify rollback readiness.

---

If you want, next I can:

- Reformat **STEP 1** into a **single printable training page**
    
- Apply this same pattern to **STEP 2–7**
    
- Create a **trainee sign-off block** for compliance