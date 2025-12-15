This step verifies that a **reliable, recurring backup schedule** is actively in place and operating as expected before the site is relied upon for maintenance, updates, or recovery.

The objective is **not** to confirm that a schedule exists in settings.  
The objective is to **prove ongoing backup continuity over time**, not one-time protection.

A backup schedule is only considered valid if **all conditions** are met:

- Backups run on a **defined frequency** (daily or weekly, based on site activity)
    
- Scheduled backups **execute successfully**, not just appear configured
    
- A **retention policy** preserves multiple restore points
    
- Backups are **not silently failing**, skipped, or overwritten prematurely
    

This step must be treated as a **continuity control**, not a configuration check.  
Schedules that exist but fail silently, retain only a single copy, or overwrite usable backups **do not qualify**.

The operator must approach this step using **time-based failure thinking**:

> If corruption or compromise is discovered days later—  
> will a clean, unaffected restore point still exist,  
> or will all usable backups already be gone?

If the answer is **not a confident yes**, dependent operations must **not proceed**.  
The correct response is to **correct schedule frequency, retention, or monitoring**, then re-verify.
## Verify Backup Schedule — Preconditions

- **1. Approved backup method confirmed**
    
    - Single authorized backup system selected
        
        - Hosting-level backup system **or**
            
        - Approved WordPress backup plugin
            
    - Backup method supports **scheduled full backups** (files + database)
        
- **2. Operator access and visibility confirmed**
    
    - Permission to view backup schedules and logs
        
    - Ability to inspect past backup executions
        
    - Ability to modify schedule or retention if required
        
- **3. Schedule configuration identifiable**
    
    - Backup frequency clearly defined
        
        - Daily (high-change sites)
            
        - Weekly (low-change sites)
            
    - Scheduled execution time visible
        
    - Timezone understood (server vs local)
        
- **4. Retention policy visible and adjustable**
    
    - Number of retained backups confirmed
        
    - Retention duration confirmed (days / weeks)
        
    - Auto-deletion or rotation rules understood
        
- **5. Backup execution history available**
    
    - Past scheduled backups visible in history
        
    - Success / failure status visible
        
    - No gaps or unexplained missing runs
        
- **6. Monitoring or alerting awareness**
    
    - Failure notifications enabled **or**
        
    - Manual review responsibility defined
        
    - Silent failure risk acknowledged and mitigated
        
## **Verify Backup Schedule — Execution

- **1. Open approved backup system**
    
    - Access the **authorized backup source only**
        
        - Hosting control panel **or**
            
        - Approved WordPress backup plugin
            
    - Confirm backup interface loads without errors
        
- **2. Locate backup schedule configuration**
    
    - Navigate to **automation / schedule / cron settings**
        
    - Identify the active backup schedule
        
    - Confirm schedule is **enabled**, not paused or disabled
        
- **3. Verify backup frequency**
    
    - Confirm frequency is explicitly defined:
        
        - Daily for high-change or transactional sites
            
        - Weekly for low-change informational sites
            
    - Reject undefined, ad-hoc, or manual-only setups
        
- **4. Verify scheduled execution timing**
    
    - Confirm scheduled run time is visible
        
    - Verify timezone (server vs local) is understood
        
    - Ensure schedule does not conflict with:
        
        - Peak traffic periods
            
        - Heavy background jobs
            
- **5. Verify execution history**
    
    - Open **backup history / logs**
        
    - Confirm scheduled backups have:
        
        - Actually executed
            
        - Completed successfully
            
    - Identify and flag:
        
        - Missed runs
            
        - Silent failures
            
        - Irregular execution gaps
            
- **6. Verify retention policy**
    
    - Confirm retention count or duration is defined
        
    - Verify multiple restore points are preserved
        
    - Reject configurations that:
        
        - Retain only the latest backup
            
        - Auto-overwrite backups too aggressively
            
- **7. Verify storage destination**
    
    - Confirm backups are stored in:
        
        - Primary storage location
            
        - Secondary / off-site location (if supported)
            
    - Ensure backups are not stored only on the live server
        
- **8. Verify failure visibility**
    
    - Confirm failure alerts are:
        
        - Enabled **or**
            
        - Explicitly monitored manually
            
    - Ensure backup failures cannot occur silently
        
- **9. Validate schedule reliability**
    
    - Assess consistency of recent runs
        
    - Confirm no unresolved warnings or failures
        
    - Treat inconsistent schedules as **unsafe**
        
- **10. Record schedule verification**
    
    - Log:
        
        - Backup frequency
            
        - Retention policy
            
        - Last successful run
            
    - Mark schedule as **verified** or **requires correction**

---
