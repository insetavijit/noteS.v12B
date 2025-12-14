## **How to Approach: Conflict Resolution (If Needed)**

Approach conflict resolution with the priority of **restoring stability, not forcing updates**. The goal is to identify which update caused the issue, contain the problem, and return the site to a stable state in staging. Avoid trial-and-error changes on production and never attempt to “patch around” a broken update without understanding the cause. Stability always takes precedence over being on the latest version.

---

## **STEP 5 — Conflict Resolution (Task Overview)**

| Task                                    | Responsibility                                                 | Brief                                    |
| --------------------------------------- | -------------------------------------------------------------- | ---------------------------------------- |
| **5.1 Identify the conflicting update** | Determine which plugin, theme, or core update caused the issue | Isolates the root cause of the problem   |
| **5.2 Revert or disable the update**    | Roll back the affected component or temporarily disable it     | Restores site stability                  |
| **5.3 Validate site recovery**          | Re-test broken functionality after rollback                    | Confirms the issue is resolved           |
| **5.4 Assess alternative actions**      | Decide whether to delay, replace, or reconfigure the component | Prevents repeated failures               |
| **5.5 Re-run QA testing**               | Verify overall site functionality after fixes                  | Ensures no secondary issues remain       |
| **5.6 Document the conflict**           | Record the issue, cause, and resolution                        | Supports future maintenance and audits   |
| **5.7 Decide readiness to proceed**     | Approve or block progression to deployment                     | Acts as the final gate before production |

---
### **5.1 Identify the Conflicting Update**

`purpose: isolate the exact source of the failure`

**What:**  
The employee must identify **which specific update** (plugin, theme, or core) caused the issue observed during staging or QA testing.

**How (Instructions for New Employees):**

1. Review the **update sequence** applied in staging (from STEP 3 notes).
    
2. Identify the **last update applied** before the issue appeared.
    
3. Reproduce the issue by:
    
    - Refreshing the affected page, or
        
    - Repeating the action that failed (form submit, login, checkout, etc.)
        
4. Temporarily disable or roll back **only that component** to confirm the issue disappears.
    
5. Re-enable it once to verify the issue returns (only if safe to do so).
    

**Training Note:**  
Do not guess or disable multiple components at once.  
Conflict resolution starts only when the **root cause is confirmed**, not assumed.

### **5.2 Revert or Disable the Update**

`purpose: restore a stable staging environment`

**What:**  
The employee must **remove the immediate cause of the failure** by reverting the problematic update or temporarily disabling the affected component.

**How (Instructions for New Employees):**

1. If a rollback option is available, revert the component to the **last known stable version**.
    
2. If rollback is not available, **deactivate** the plugin or switch away from the updated theme.
    
3. Do **not** edit code or configuration files unless instructed by a senior team member.
    
4. Avoid making multiple changes at once—apply only one corrective action.
    

**Training Note:**  
Rolling back or disabling an update is a **correct and professional action**, not a failure.
### **5.3 Verify Site Recovery**

`purpose: confirm stability has been restored`

**What:**  
The employee must confirm that reverting or disabling the update has **successfully resolved the issue** and restored the site to a stable state in staging.

**How (Instructions for New Employees):**

1. Reload the page or feature that was previously broken.
    
2. Repeat the exact action that caused the failure (e.g., submit form, login, open page).
    
3. Confirm:
    
    - No errors appear
        
    - The page loads correctly
        
    - The WordPress Admin Dashboard is accessible
        
4. Check that no _new_ issues were introduced by the rollback or disablement.
    

**Training Note:**  
If the problem still exists, the root cause may be incorrect or incomplete—do not proceed.

---

### **5.4 Evaluate Next Action**

`purpose: decide how the problematic update should be handled`

**What:**  
The employee must decide the safest next step for the conflicting update.

**How (Instructions for New Employees):**

1. Choose one of the following actions:
    
    - **Delay the update** until a fix is released
        
    - **Replace the component** with a compatible alternative
        
    - **Escalate** the issue for deeper investigation
        
2. Do **not** attempt advanced fixes, patches, or workarounds without approval.
    
3. Ensure the chosen action is clearly recorded.
    

**Training Note:**  
Delaying an update is often the safest and most professional decision.

### **5.5 Re-run QA Testing**
`purpose: ensure no secondary issues remain after resolution`

**What:**  
The employee must re-test all affected areas of the site after rolling back or disabling the problematic update.

**How (Instructions for New Employees):**

1. Re-run QA checks related to the affected functionality (pages, forms, login, checkout, etc.).
    
2. Confirm that previously working features remain functional.
    
3. Look for any new issues introduced by the rollback action.
    

**Training Note:**  
Conflict resolution is incomplete unless QA is repeated and passed.

---

### **5.6 Document the Conflict**

`purpose: preserve knowledge and prevent repeat failures`

**What:**  
The employee must clearly document the conflict and how it was handled.

**How (Instructions for New Employees):**

1. Record:
    
    - Component name
        
    - Version involved
        
    - Issue observed
        
    - Action taken (rollback, disable, delay)
        
2. Save the notes in the maintenance log or task system.
    
3. Ensure the documentation is clear enough for another team member to understand without explanation.
    

**Training Note:**  
Good documentation reduces future risk and saves team time.

### **5.7 Decide Readiness to Proceed**
`purpose: act as the final gate before moving forward`

**What:**  
The employee must decide whether the site is now **stable enough** to continue the update workflow.

**How (Instructions for New Employees):**

1. Confirm that:
    
    - The original issue is fully resolved
        
    - No new issues were introduced
        
    - Relevant QA checks have been re-run and passed
        
2. Review documentation created in **5.6**.
    
3. Decide one of the following:
    
    - **Proceed** to the next step (only if stability is confirmed)
        
    - **Stop and escalate** if any uncertainty remains
        

**Training Note:**  
Proceeding without confidence in stability is a process violation.

---

### **Doctrine Reminder**

> _Conflict resolution exists to restore stability, not to force progress.  
> If stability is not guaranteed, the workflow must stop._

---
