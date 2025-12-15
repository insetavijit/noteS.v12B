### **Verify Backup Integrity — Step Overview**

This step verifies that the completed backup is **technically sound, accessible, and free from corruption** before it is relied upon for restore, rollback, or maintenance operations.

The objective is **not** to see a “successful” status alone.  
The objective is to **prove that the backup artifacts are usable and internally consistent**.

Backup integrity is only considered valid if **all conditions** are met:

- Backup execution completed with **no errors or warnings**
    
- Backup artifacts (archives, database dumps) are **accessible and readable**
    
- No corruption indicators, truncation, or missing files are detected
    
- Backup contents align with the declared scope (files + database)
    

This step must be treated as a **trust-validation gate**, not a cosmetic check.  
A backup that exists but cannot be read, extracted, or restored reliably **does not qualify as protection**.

The operator must approach this step using **restore-readiness thinking**:

> If this backup were used for restoration—  
> would it complete cleanly without file errors, database import failures,  
> or unexpected omissions?

If the answer is **not a confident yes**, the backup must be **rejected**.  
The correct response is to **identify the integrity issue, correct the cause, and regenerate the backup**.

---
## **Verify Backup Integrity — Preconditions (Nested List)**

- **1. Backup execution confirmed complete**
    
    - Backup status shows **Completed / Successful**
        
    - No in-progress, paused, queued, or retry state
        
    - Backup artifacts generated and listed
        
- **2. Backup artifacts accessible**
    
    - Backup archive downloadable or browsable
        
    - Database dump file accessible
        
    - No permission, timeout, or access errors
        
- **3. Integrity inspection capability available**
    
    - Ability to extract or inspect archive contents
        
    - Ability to preview or validate database dump
        
    - Sufficient local or server resources for inspection
        
- **4. Backup scope expectation defined**
    
    - Expected file scope documented
        
        - Core, themes, plugins, uploads, custom paths
            
    - Expected database scope documented
        
        - WordPress tables and custom tables
            
    - Multisite scope clarified (if applicable)
        
- **5. System stability ensured during backup**
    
    - No storage exhaustion during backup creation
        
    - No abrupt process termination or server restarts
        
    - No conflicting backup jobs overlapping execution
        
- **6. Operator authority confirmed**
    
    - Permission to inspect backup contents
        
    - Permission to reject and regenerate backup
        
    - Permission to halt dependent operations if integrity fails
        

---
## **Verify Backup Integrity — Execution (Nested List)**

- **1. Open validated backup entry**
    
    - Access the **approved backup system only**
        
        - Hosting control panel **or**
            
        - Approved WordPress backup plugin
            
    - Select the backup marked **Completed / Successful**
        
    - Ensure backup artifacts are visible and accessible
        
- **2. Verify backup artifact accessibility**
    
    - Confirm file archive is:
        
        - Downloadable **or**
            
        - Browseable within the backup interface
            
    - Confirm database dump is:
        
        - Present
            
        - Accessible
            
        - Non-zero in size
            
- **3. Inspect archive readability**
    
    - Attempt to:
        
        - Expand archive contents **or**
            
        - Preview file list / manifest
            
    - Verify no extraction errors, truncation warnings, or permission failures
        
- **4. Inspect database dump validity**
    
    - Confirm database dump:
        
        - Opens without error
            
        - Contains readable SQL or database format
            
    - Verify no obvious corruption markers or incomplete dumps
        
- **5. Check for execution warnings or errors**
    
    - Review backup logs for:
        
        - Warnings
            
        - Skipped files
            
        - Partial completion flags
            
    - Treat any unreviewed warning as a failure
        
- **6. Cross-verify scope consistency**
    
    - Confirm that accessible artifacts align with declared scope:
        
        - Files present match expected directories
            
        - Database dump aligns with site size and activity
            
- **7. Reject corrupted or ambiguous backups**
    
    - Immediately disqualify backups with:
        
        - Corrupted archives
            
        - Unreadable dumps
            
        - Missing artifacts
            
        - Unresolved warnings
            
- **8. Mark integrity verification result**
    
    - Record integrity status as:
        
        - **Validated — No errors**
            
    - Proceed only after integrity is confirmed
        

---
