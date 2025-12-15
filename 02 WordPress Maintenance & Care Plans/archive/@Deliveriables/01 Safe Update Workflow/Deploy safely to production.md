## **How to Approach: Deploy Safely to Production**

Approach production deployment as a **controlled replication**, not a new experiment. The goal is to apply **only** the changes that were tested, validated, and approved in staging—nothing more, nothing less. Production is not the place for discovery or troubleshooting. Move deliberately, apply updates in a predictable order, and assume that any deviation from the staged process introduces risk. If anything unexpected occurs, stop immediately and prepare to roll back.

## **STEP 6 — Deploy Safely to Production (Task Overview)**

| Task                                  | Responsibility                                        | Brief                                  |
| ------------------------------------- | ----------------------------------------------------- | -------------------------------------- |
| **6.1 Confirm staging approval**      | Verify staging passed QA and conflicts are resolved   | Ensures only validated changes go live |
| **6.2 Reconfirm backup availability** | Ensure recent rollback point exists                   | Protects against production failure    |
| **6.3 Apply updates to production**   | Apply the same updates approved in staging            | Replicates tested changes              |
| **6.4 Maintain controlled order**     | Update core (if applicable), then plugins, then theme | Reduces risk of dependency issues      |
| **6.5 Clear cache and CDN**           | Flush caches after updates                            | Prevents stale or inconsistent content |
| **6.6 Monitor deployment results**    | Watch for errors or access issues                     | Detects immediate production failures  |
| **6.7 Confirm deployment completion** | Verify updates completed successfully                 | Acts as the gate before final testing  |

---

### **6.1 Confirm Staging Approval**
`purpose: ensure only validated changes reach production`

**What:**  
The employee must confirm that all updates have **passed staging QA** and that no unresolved issues remain.

**How (Instructions for New Employees):**

1. Review QA results from **STEP 4** and confirm all required checks passed.
    
2. Verify that any issues found in **STEP 5** were resolved and documented.
    
3. Confirm the final update list exactly matches what was tested in staging.
    
4. Ensure there are no pending warnings, notes, or “to be checked later” items.
    

**Training Note:**  
If staging is not explicitly approved, **production must not be touched**.

### **6.2 Reconfirm Backup Availability**
`purpose: guarantee immediate rollback capability on production`

**What:**  
The employee must confirm a **recent, valid production backup** exists before applying any updates.

**How (Instructions for New Employees):**

1. Verify that a full production backup (files + database) exists.
    
2. Check the backup timestamp to ensure it is **recent and pre-deployment**.
    
3. Confirm the restore option is visible and accessible.
    
4. Ensure at least one backup copy is stored off-site.
    

**Training Note:**  
Deploying without a confirmed rollback point is a **critical process violation**.
### **6.3 Apply Updates to Production**
`purpose: replicate the verified staging state on the live site`

**What:**  
The employee must apply **only the updates that were tested and approved in staging** to the production environment.

**How (Instructions for New Employees):**

1. Log in to the **production** WordPress Admin Dashboard.
    
2. Navigate to **Dashboard → Updates**.
    
3. Apply **only** the updates listed in the approved staging update list.
    
4. Do not apply any additional updates, even if they appear minor or safe.
    
5. Wait for each update to complete before moving to the next.
    

**Training Note:**  
Production is not a place for discovery. If something was not tested in staging, it must not be applied here.

### **6.4 Maintain Controlled Update Order**
`purpose: minimize risk and simplify rollback if needed`

**What:**  
The employee must apply updates in the same order used in staging.

**How (Instructions for New Employees):**

1. Update **WordPress core first**, if included.
    
2. Update plugins **one at a time**, following the same sequence used in staging.
    
3. Update the active theme **last**.
    
4. Avoid bulk updates and parallel actions.
    

**Training Note:**  
Consistency between staging and production reduces unexpected behavior.

### **6.5 Clear Cache and CDN**
`purpose: ensure users see the updated, correct version of the site`

**What:**  
The employee must clear all relevant caches after updates are applied to production.

**How (Instructions for New Employees):**

1. Clear the WordPress cache using the active caching plugin (if present).
    
2. Clear server-side cache from the hosting panel, if available.
    
3. Clear CDN cache (Cloudflare or similar), if the site uses one.
    
4. Refresh the site in a private/incognito browser window to confirm changes are visible.
    

**Training Note:**  
Uncleared caches can make a successful deployment appear broken.

---

### **6.6 Monitor Deployment Results**
`purpose: detect immediate production issues`

**What:**  
The employee must observe the production site immediately after deployment for any errors or access issues.

**How (Instructions for New Employees):**

1. Reload the homepage and key pages on the live site.
    
2. Confirm the WordPress Admin Dashboard remains accessible.
    
3. Watch for:
    
    - Error messages
        
    - Broken layouts
        
    - Missing content
        
4. Monitor uptime or alert tools briefly, if available.
    

**Training Note:**  
If any unexpected issue appears, stop immediately and prepare to roll back.

### **6.7 Confirm Deployment Completion**
`purpose: act as the final gate before post-deployment testing`

**What:**  
The employee must confirm that all approved updates were applied successfully on the production site and that the deployment process completed without errors.

**How (Instructions for New Employees):**

1. Review the production **Updates** screen and confirm no approved updates remain pending.
    
2. Verify that no error messages or failed update notices were shown during deployment.
    
3. Check that the updated plugin, theme, or core versions match the versions tested in staging.
    
4. Confirm the site remains accessible to both users and administrators.
    

**Training Note:**  
If any approved update is incomplete, failed, or inconsistent with staging, **deployment is not complete** and post-deployment testing must not begin.

---
