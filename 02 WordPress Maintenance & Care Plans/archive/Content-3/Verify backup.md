This step verifies that a **current, complete, correctly timed, and immediately restorable WordPress backup** exists before any core, theme, or plugin updates are attempted.

The objective is **not** to confirm that a backup exists somewhere in the system.  
The objective is to **prove rollback capability** under real failure conditions.

A backup is only considered valid if **all four conditions** are met:

- A backup entry is **visible and completed** (S1A)
    
- **Both files and database** are fully included (S1B)
    
- The backup timestamp is **strictly pre-update** (S1C)
    
- A **working restore path** is available and executable (S1D)
    

This step must be treated as a **non-negotiable safety gate**.  
Automated schedules, assumed backups, download-only archives, or historical success **do not qualify**.

The operator must approach this step using **failure-containment thinking** rather than checklist completion:

> If the update fails right now—  
> can this site be fully restored to its exact pre-update state,  
> without data loss, extended downtime, or escalation?

If the answer is **not a confident yes**, updates must **not proceed**.  
The correct response is to **halt, correct the backup state, and re-verify** before continuing.

This gate exists to ensure that **every update action is reversible**, predictable, and professionally defensible.

| **Task**                              | **Brief**                                                                                                            |
| ------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| **[[Check latest backup exists]]**    | Confirm that a recent backup entry is visible in the approved backup system before any update activity begins.       |
| **Confirm files & database included** | Ensure the backup contains both WordPress files and the complete database; partial backups are not acceptable.       |
| **Verify timestamp is pre-update**    | Check that the backup was created immediately before starting the update process, not from a scheduled or older run. |
| **Confirm restore option available**  | Verify that a clear and usable restore method exists (one-click restore or manual recovery path).                    |

---

## **Verify Backup — Preconditions (Nested List)**

- **1. Change window locked**
    
    - No WordPress core, theme, or plugin updates running
        
    - No migrations, imports, exports, or sync jobs active
        
    - No database-intensive background tasks executing
        
- **2. Operator authority confirmed**
    
    - Permission to halt or postpone updates
        
    - Permission to freeze writes if required
        
    - Permission to initiate restore / rollback
        
- **3. Backup system identified and accessible**
    
    - Approved backup source selected
        
        - Hosting-level backup system (preferred)
            
        - WordPress backup plugin (fallback only)
            
    - Access credentials verified
        
    - Backup interface reachable (UI/API loads correctly)
        
- **4. Site context fully confirmed**
    
    - Exact target domain verified
        
    - Correct environment confirmed
        
        - Production (not staging or development)
            
    - Multisite status confirmed
        
        - Single-site vs multisite (affects backup scope)
            
- **5. Storage and system readiness**
    
    - Sufficient storage available for full backup creation
        
    - No active backup jobs conflicting with verification
        

## **Verify Backup — Execution (Nested List)**

- **1. Check latest backup exists**
    
    - Open the **approved backup system only** (hosting panel or authorized plugin)
        
    - Navigate to the **backup history / backup list**
        
    - Verify that **filters, pagination, or date ranges** do not hide recent backups
        
    - Locate the **most recent completed backup entry**
        
    - Confirm the backup entry is:
        
        - Visible (not archived or rotated)
            
        - Accessible (clickable, expandable, inspectable)
            
        - Marked as **Completed / Successful**
            
    - Reject backups that are **failed, in-progress, paused, queued, or incomplete**
        
- **2. Confirm files & database included**
    
    - Open **backup details, manifest, or contents view**
        
    - Verify **file backup scope** explicitly includes:
        
        - WordPress core (`wp-admin`, `wp-includes`, root PHP files)
            
        - Themes (`/wp-content/themes`)
            
        - Plugins (`/wp-content/plugins`)
            
        - Uploads (`/wp-content/uploads`)
            
        - Custom directories or MU-plugins (if present)
            
    - Verify **database backup scope** explicitly includes:
        
        - All WordPress tables (`wp_*`)
            
        - Any **custom-prefix or plugin-generated tables**
            
    - Confirm backup type is **full site backup**  
        (files + database together, not files-only, DB-only, or incremental-only)
        
    - Perform a **sanity check on backup size** to ensure it aligns with site scale  
        (unexpectedly small backups indicate missing data)
        
    - Reject partial, selective, or ambiguous backups
        
- **3. Verify timestamp is pre-update**
    
    - Open **backup metadata** (created time and completed time)
        
    - Identify the **planned update start time**
        
    - Compare timestamps to ensure:
        
        - Backup creation started **before any update action**
            
        - Backup completed **before updates begin**
            
    - Prefer backups created **minutes before** the update window
        
    - Reject:
        
        - Older scheduled or automated backups
            
        - Backups created after updates started
            
        - Backups with timezone ambiguity not clearly resolved
            
    - Trigger a **new on-demand full backup** if freshness criteria are not met
        
- **4. Confirm restore option available**
    
    - Open the **restore / recovery interface** for the selected backup
        
    - Identify available restore methods:
        
        - One-click full restore
            
        - Restore to staging
            
        - Manual restore (download files + database)
            
    - Confirm restore actions are:
        
        - Enabled (not greyed out)
            
        - Accessible with current operator permissions
            
    - Verify restore scope supports:
        
        - Full site restore (files + database)
            
    - Perform **restore proof** (non-destructive):
        
        - Start backup download **or**
            
        - Reach final restore confirmation screen (without executing)
            
    - Reject any backup that lacks a **clear, executable restore path**
        

---
## **Verify Backup — Verification (Nested List)**

- **1. Backup validity confirmed**
    
    - Backup completed successfully
        
    - Backup marked as full (not partial or failed)
        
- **2. Backup completeness verified**
    
    - WordPress files confirmed
        
        - Core files present
            
        - Themes present
            
        - Plugins present
            
        - Uploads present
            
    - WordPress database confirmed
        
        - All tables included
            
- **3. Backup freshness verified**
    
    - Backup timestamp confirmed as pre-update
        
    - No newer site changes occurred after backup
        
- **4. Restore readiness verified**
    
    - Restore option visible and accessible
        
    - Restore method usable
        
        - One-click restore, or
            
        - Staging restore, or
            
        - Manual restore path confirmed
            
- **5. Rollback capability confirmed**
    
    - Operator able to initiate restore if required
        
    - Restore method understood and documented
        
- **6. Verification outcome recorded**
    
    - Backup declared **valid and usable**
        
    - Verification acknowledged (name + time, if required)
        

---
