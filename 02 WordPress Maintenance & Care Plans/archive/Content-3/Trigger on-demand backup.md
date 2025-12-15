### **Trigger On-Demand Backup — Step Overview**

This step verifies that a **fresh, manually initiated full backup** is created when scheduled backups are **insufficient, outdated, or unsafe** for the operation about to be performed.

The objective is **not** to rely on routine automation.  
The objective is to **establish a precise rollback point** that accurately reflects the site’s current state immediately before risk is introduced.

An on-demand backup must be triggered if **any** of the following conditions apply:

- The most recent scheduled backup is **outdated or stale**
    
- Backup integrity, scope, or restore readiness is **unverified**
    
- Site changes have occurred since the last validated backup
    
- Upcoming operations carry **elevated risk** (updates, migrations, cleanup)
    

This step must be treated as a **risk-reset control**, not redundancy.  
Proceeding without a fresh backup when conditions have changed **does not qualify as safe practice**.

The operator must approach this step using **point-of-failure thinking**:

> If the next action fails immediately—  
> does this backup allow a **clean, exact rollback**  
> to the site state that exists right now?

If the answer is **not an unequivocal yes**, a new full backup must be created **before proceeding**.

This backup must include:

- **All WordPress files** (core, themes, plugins, uploads, custom directories)
    
- The **entire database**
    
- A **clear, executable restore path**
    

This gate exists to ensure that **every high-risk operation begins from a fully protected state**, making failures recoverable, contained, and professionally defensible.

---
## **Trigger On-Demand Backup — Preconditions (Nested List)**

- **1. Scheduled backup deemed insufficient**
    
    - Latest scheduled backup is **outdated** relative to upcoming operation
        
    - Backup freshness does not align with current site state
        
    - Backup integrity or scope has not been fully validated
        
- **2. Risk-bearing operation identified**
    
    - Core, theme, or plugin updates planned
        
    - Database cleanup or optimization planned
        
    - Migration, staging sync, or structural change planned
        
    - Any operation with **rollback risk**
        
- **3. Site state stabilized**
    
    - No active updates running
        
    - No content publishing or configuration changes in progress
        
    - No background jobs that may mutate data during backup
        
- **4. Operator authority confirmed**
    
    - Permission to initiate manual backup
        
    - Permission to pause or delay dependent operations
        
    - Permission to restore from the backup if required
        
- **5. Backup system ready for manual execution**
    
    - Approved backup method identified
        
    - Manual / on-demand backup option available
        
    - Sufficient storage available for full backup creation
        
- **6. Backup scope defaults verified**
    
    - Full file system backup enabled
        
        - Core, themes, plugins, uploads, custom directories
            
    - Full database backup enabled
        
    - Incremental-only or partial modes **not selected**
        

---
## **Trigger On-Demand Backup — Execution**

- **1. Confirm on-demand backup is required**
    
    - Verify that scheduled backups are:
        
        - Outdated, stale, or contextually unsafe **or**
            
        - Unverified for integrity, scope, or restore readiness
            
    - Confirm upcoming operation carries elevated risk:
        
        - Core, theme, or plugin updates
            
        - Database cleanup or optimization
            
        - Migration, staging sync, or structural change
            
- **2. Stabilize site state**
    
    - Confirm no active:
        
        - Core, theme, or plugin updates
            
        - Content publishing or configuration changes
            
        - Background jobs affecting database or files
            
    - Freeze site changes for the duration of the backup
        
- **3. Open approved backup system**
    
    - Access the **authorized backup source only**
        
        - Hosting control panel **or**
            
        - Approved WordPress backup plugin
            
    - Confirm manual / on-demand backup option is available
        
- **4. Configure backup scope**
    
    - Select **Full Site Backup** mode
        
        - Files + database together
            
    - Verify inclusion of:
        
        - WordPress core files
            
        - Themes and plugins
            
        - Uploads and custom directories
            
        - Entire database
            
    - Ensure incremental-only or selective modes are **not selected**
        
- **5. Initiate backup execution**
    
    - Start manual / on-demand backup explicitly
        
    - Confirm backup job is created and queued
        
    - Note start time for later freshness validation
        
- **6. Monitor backup execution**
    
    - Observe progress in real time
        
    - Watch for:
        
        - Errors
            
        - Timeouts
            
        - Pauses or retries
            
    - Do not leave backup unattended until status is visible
        
- **7. Confirm backup completion**
    
    - Verify backup status transitions to:
        
        - **Completed / Successful**
            
    - Reject backups that:
        
        - Fail
            
        - Remain in-progress
            
        - Complete with warnings
            
- **8. Proceed to validation sequence**
    
    - Move immediately to:
        
        - Confirm files + database included
            
        - Verify integrity
            
        - Validate freshness
            
        - Confirm restore path
            
    - Do not rely on the backup until validation is complete
        

---
