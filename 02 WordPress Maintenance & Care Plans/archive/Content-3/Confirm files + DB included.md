### **Confirm Files + Database Included — Step Overview**

This step verifies that the backup includes **complete file coverage** and the **entire WordPress database**, ensuring the site can be fully reconstructed from a restore.

The objective is **not** to confirm that “files” or a “database” exist in isolation.  
The objective is to **prove full-site recoverability** without manual reconstruction or data loss.

A backup is only considered valid if **all conditions** are met:

- All **WordPress core files** are included
    
- **Themes** (active and inactive) are included
    
- **Plugins** (active and inactive) are included
    
- **Uploads / media library** is included
    
- Any **custom directories or MU-plugins** are included
    
- The **entire database** is present, including all WordPress and custom tables
    

This step must be treated as a **restore-integrity gate**, not a metadata check.  
Files-only or database-only backups **do not qualify**, regardless of backup success status.

The operator must approach this step using **full-reconstruction thinking**:

> If this backup were restored right now—  
> would the site return **exactly as it exists today**,  
> without missing functionality, content, or configuration?

If the answer is **not a confident yes**, the backup must be **rejected**.  
The correct response is to **correct backup scope settings and regenerate a full backup**.

---
## **L1:Preconditions**

- **1. Backup execution completed successfully**
    
    - Backup status shows **Completed / Successful**
        
    - Backup entry is visible and accessible for inspection
        
    - No partial, paused, or in-progress state remains
        
- **2. Backup inspection access available**
    
    - Ability to open backup details / manifest
        
    - Ability to browse file list or archive contents
        
    - Ability to view database dump information
        
- **3. File system scope identifiable**
    
    - WordPress root structure visible
        
        - `wp-admin`
            
        - `wp-includes`
            
        - Root PHP files (`wp-config.php`, etc.)
            
    - `wp-content` directory visible
        
        - `/themes`
            
        - `/plugins`
            
        - `/uploads`
            
    - Custom directories or **MU-plugins** identifiable (if present)
        
- **4. Database scope identifiable**
    
    - Database dump present and accessible
        
    - Database name identifiable
        
    - Table list visible or inferred
        
        - Standard `wp_*` tables
            
        - Custom-prefix tables
            
        - Plugin-generated tables (e.g., WooCommerce, forms)
            
- **5. Backup type confirmed**
    
    - Backup configured as **full-site backup**
        
        - Files + database together
            
    - Incremental-only, differential-only, or selective backups **not relied upon**
        
- **6. Site context understood**
    
    - Single-site vs multisite confirmed
        
    - Multisite scope understood
        
        - Network tables included if multisite
            
    - Custom storage paths (if any) accounted for
        

---
## **L2:Execution

- **1. Open validated backup entry**
    
    - Access the **approved backup system only**
        
        - Hosting control panel **or**
            
        - Approved WordPress backup plugin
            
    - Select the backup entry marked:
        
        - **Completed / Successful**
            
    - Ensure backup details or manifest view is accessible
        
- **2. Inspect file system coverage**
    
    - Expand file list, archive view, or manifest
        
    - Confirm inclusion of **WordPress core files**:
        
        - `wp-admin`
            
        - `wp-includes`
            
        - Root PHP files (`index.php`, `wp-config.php`, etc.)
            
    - Confirm inclusion of **wp-content** directories:
        
        - `/themes` (active and inactive)
            
        - `/plugins` (active and inactive)
            
        - `/uploads`
            
    - Verify presence of:
        
        - Custom directories
            
        - MU-plugins (if applicable)
            
- **3. Inspect database coverage**
    
    - Locate database dump or database section
        
    - Confirm database backup exists and is accessible
        
    - Verify inclusion of:
        
        - All standard WordPress tables (`wp_*`)
            
        - Custom-prefix tables
            
        - Plugin-generated tables (e.g., WooCommerce, forms)
            
- **4. Confirm backup type**
    
    - Verify backup is configured as **Full Site Backup**
        
        - Files + database together
            
    - Reject:
        
        - Files-only backups
            
        - Database-only backups
            
        - Incremental-only or partial snapshots
            
- **5. Perform scope consistency check**
    
    - Cross-check declared backup scope against:
        
        - Actual file list
            
        - Database contents
            
    - Flag any missing or excluded components
        
- **6. Handle multisite or special cases (if applicable)**
    
    - Confirm multisite network tables included
        
    - Confirm uploads for all sites included
        
    - Confirm custom storage paths accounted for
        
- **7. Reject incomplete backups**
    
    - Immediately disqualify backups missing:
        
        - Any required file directories
            
        - Any database tables
            
    - Do not attempt partial recovery assumptions
        
- **8. Mark coverage verification result**
    
    - Record coverage status:
        
        - **Files confirmed**
            
        - **Database confirmed**
            
    - Proceed to **Verify Backup Integrity** only after both are confirmed
        

---

### **Execution Rule**

If **either files or database coverage is incomplete**, the backup is **invalid**.

The correct response is to **correct backup scope settings and trigger a new full backup** before continuing.

If you want next, I can:

- produce the execution list for **Verify Backup Integrity**, or
    
- map this step directly to your **Verify File Coverage / Verify Database Coverage** gates, or
    
- compress this into a **trainee checklist**.