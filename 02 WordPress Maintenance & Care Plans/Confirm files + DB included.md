Below is a **directly parallel version** to **S1A — Check Latest Backup Exists**, with the **same structure and simple numbering only**, for:

## **S1B — Confirm Files + Database Included (Nested Brief List)**

- **A. Preconditions**
    
    - **1. Confirm access to backup details —** Ensure ability to view backup contents
        
        - **1.1 Hosting backup file explorer access —** Required to inspect archive contents
            
        - **1.2 WordPress backup plugin detail view —** Required to view included components
            
    - **2. Confirm backup source consistency —** Use the same approved backup method
        
    - **3. Confirm correct backup entry —** Ensure the selected backup is the intended one
        
    - **4. Confirm authority to re-run backup —** Ensure permission to regenerate backup if incomplete
        
- **B. Execution**
    
    - **5. Open selected backup entry —** Access backup detail / inspection view
        
        - **5.1 Expand backup contents —** View included items
            
    - **6. Verify file system inclusion —** Confirm WordPress files are present
        
        - **6.1 Core files —** `wp-admin`, `wp-includes`, root PHP files
            
        - **6.2 Themes directory —** `/wp-content/themes/`
            
        - **6.3 Plugins directory —** `/wp-content/plugins/`
            
        - **6.4 Uploads directory —** `/wp-content/uploads/`
            
    - **7. Verify database inclusion —** Confirm WordPress database is included
        
        - **7.1 Database indicator —** Labeled as database / SQL / DB dump
            
        - **7.2 Database name match —** Matches site’s active database
            
        - **7.3 Database size sanity check —** Size is reasonable for site scale
            
    - **8. Regenerate full backup if incomplete —** Create a corrected rollback point
        
        - **8.1 Initiate full files backup —** Ensure all directories are selected
            
        - **8.2 Initiate database backup —** Ensure database option is enabled
            
        - **8.3 Monitor backup execution —** Wait until completion
            
- **C. Verification**
    
    - **9. Confirm files included —** All required WordPress directories present
        
    - **10. Confirm database included —** Database confirmed in backup
        
    - **11. Reject partial backups —** Files-only or DB-only backups are invalid
        
    - **12. Record backup completeness —** Mark backup as valid for rollback
        
    - **13. Confirm readiness to proceed —** Backup approved for update operations
        

---

If you want next, we can produce:  
• **S1C — Verify Backup Timestamp (Pre-Update)**  
• **S1D — Confirm Restore Option Available**  
• or a **failure-handling branch** mapped to these numbers (e.g., fail at 7 → halt updates).