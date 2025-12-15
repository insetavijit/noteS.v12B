## **Verify Backup — Mandatory Gate Before Any Production Update (Brief List · Nested)**

### **A. Preconditions**

- **Confirm change window is locked —** Ensure no updates, migrations, imports, or DB-intensive jobs are running
    
    - **Confirm no update activity —** Core, theme, and plugin updates are idle
        
    - **Confirm no data operations —** No migrations, syncs, or bulk jobs in progress
        
- **Confirm backup authority granted —** Ensure operator is authorized to halt updates and restore site
    
    - **Permission to halt updates —** Updates can be stopped immediately if needed
        
    - **Permission to initiate restore —** Emergency rollback is allowed without delay
        
- **Identify backup system —** Ensure backup source is known and accessible
    
    - **Hosting-level backup —** Preferred primary source if available
        
    - **Backup plugin —** Secondary source if host backups are unavailable
        
    - **Hard rule —** Do not rely solely on a free plugin if host backups exist
        
- **Confirm site context —** Ensure the correct site is being prepared
    
    - **Confirm domain —** Exact target domain or subdomain verified
        
    - **Confirm environment —** Production vs staging vs development confirmed
        
    - **Confirm multisite status —** Single-site or multisite verified
        

---

### **B. Execution**

- **Execute S1A — Check latest backup exists —** Locate and open the most recent backup entry
    
    - **Confirm backup visibility —** Backup entry is visible and accessible
        
    - **Confirm backup not hidden —** Not archived, queued, or filtered
        
- **Confirm backup completeness —** Ensure backup is full and usable
    
    - **Confirm WordPress files included —** Core, themes, plugins, uploads, and custom directories
        
    - **Confirm database included —** All WordPress and custom tables present
        
- **Verify backup freshness —** Ensure backup reflects current site state
    
    - **Check timestamp —** Backup created ≤ 30 minutes before update start
        
    - **Or post-freeze backup —** Created immediately after change window lock
        
- **Confirm restore readiness —** Ensure rollback is technically possible
    
    - **Confirm restore option visible —** One-click, staging restore, or manual restore available
        
    - **Perform restore proof —** Start backup download or test restore if supported
        
- **Perform remediation if needed —** Create a new backup when validation fails
    
    - **Create manual full backup —** Files + database, on-demand
        
    - **Wait for completion —** Backup must finish successfully
        
    - **Re-run verification —** Repeat S1A and validation steps
        

---

### **C. Verification**

- **Declare backup usable —** Backup meets all safety criteria
    
    - **Complete —** Files + full database included
        
    - **Fresh —** Created pre-update or post-freeze
        
    - **Restorable —** Restore path proven accessible
        
- **Confirm rollback plan —** Ensure recovery is planned
    
    - **Restore method defined —** Exact restore approach documented
        
    - **Responsible person assigned —** Rollback owner named
        
    - **RTO defined —** Maximum acceptable downtime recorded
        
- **Record mandatory sign-off —** Verification is independently approved
    
    - **Separate reviewer —** Reviewer is not the updater
        
    - **Approval recorded —** “Backup verified — safe to proceed” with name + timestamp
        

---

