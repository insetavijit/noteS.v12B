## **S1A — Check Latest Backup Exists (Nested Brief List)**

- **A. Preconditions**
    
    - **1. Confirm backup system access —** Ensure access to hosting panel or WordPress backup plugin
        
        - **1.1 Hosting control panel access —** Required for server-level backups
            
        - **1.2 WordPress backup plugin access —** Required for application-level backups
            
    - **2. Identify approved backup method —** Select one approved backup source only
        
    - **3. Confirm correct site/environment —** Verify domain and production vs staging
        
    - **4. Confirm authority to halt updates —** Ensure permission to stop updates if backup fails
        
- **B. Execution**
    
    - **5. Open approved backup system —** Access the backup dashboard
        
        - **5.1 Navigate to backup dashboard —** Reach the backup management screen
            
    - **6. Access backup history —** View existing backup records
        
        - **6.1 Check filters —** Ensure recent backups are not hidden
            
    - **7. Locate most recent backup entry —** Identify the latest backup by timestamp
        
        - **7.1 Verify visibility —** Ensure backup is not archived or queued
            
    - **8. Inspect backup metadata —** Review technical details of the backup
        
        - **8.1 Status —** Must indicate completed / successful
            
        - **8.2 Timestamp —** Must be immediately pre-update
            
        - **8.3 Backup size —** Should match site scale
            
        - **8.4 Backup scope —** Must indicate files + database
            
    - **9. Create manual full backup —** Generate a full rollback point if none exists
        
        - **9.1 Initiate files backup —** Capture WordPress core, themes, plugins, uploads
            
        - **9.2 Initiate database backup —** Capture WordPress database
            
        - **9.3 Monitor execution —** Wait until backup completes