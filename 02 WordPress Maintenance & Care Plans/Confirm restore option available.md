## **S1D — Confirm Restore Option Available (Nested Brief List)**

- **A. Preconditions**
    
    - **1. Confirm restore permissions —** Ensure authority to perform a restore if required
        
        - **1.1 Hosting-level restore permission —** Required for server backups
            
        - **1.2 Plugin-level restore permission —** Required for WordPress backups
            
    - **2. Confirm restore method matches backup source —** Same tool must support restore
        
    - **3. Confirm no active restore lock —** Ensure system is not in maintenance or freeze state
        
    - **4. Confirm business approval for rollback —** Restore may overwrite recent changes
        
- **B. Execution**
    
    - **5. Open backup restore interface —** Access restore controls for the selected backup
        
        - **5.1 Locate restore / rollback action —** Button, link, or CLI option
            
    - **6. Verify restore scope options —** Confirm what can be restored
        
        - **6.1 Files restore option —** WordPress core, themes, plugins, uploads
            
        - **6.2 Database restore option —** Full WordPress database
            
        - **6.3 Full restore option —** Files + database together
            
    - **7. Validate restore readiness —** Ensure restore is executable
        
        - **7.1 Restore action enabled —** Not greyed out or disabled
            
        - **7.2 No dependency warnings —** Storage, permissions, quota issues
            
        - **7.3 No expiration flags —** Backup not expired or locked
            
    - **8. Dry-run confirmation (visual) —** Validate restore without executing
        
        - **8.1 Review confirmation modal —** Scope, site, and overwrite warnings
            
        - **8.2 Verify target environment —** Production vs staging
            
- **C. Verification**
    
    - **9. Confirm restore option available —** Restore can be initiated if required
        
    - **10. Confirm correct restore scope —** Full site restore possible
        
    - **11. Reject backups without restore path —** Download-only backups are insufficient
        
    - **12. Record restore readiness —** Mark backup as rollback-safe
        
    - **13. Confirm clearance to proceed —** Updates may now begin
        

---

With **S1A–S1D**, your **Verify Backup** gate is now complete and trainee-safe.

If you want next:  
• a **single-page “Verify Backup” checklist** mapped to S1A–S1D  
• a **STOP / GO decision table**  
• or move forward to **[[Review Changelogs]] (S2)**