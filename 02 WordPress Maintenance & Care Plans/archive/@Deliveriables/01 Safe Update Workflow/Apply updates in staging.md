## **How to Approach: Apply Updates in Staging**

Approach this step with the understanding that **staging is a controlled failure zone**, not a shortcut to production. The goal is to apply updates exactly as planned and observe their impact without risking live users or data. Only updates that were reviewed and approved in the previous step should be applied. Make no additional changes in staging, apply updates one at a time, and assume that if something breaks in staging, it would have broken in production. Any failure here is a success of the process, not a mistake.

## **STEP 3 — Apply Updates in Staging (Task Overview)**

| Task                                      | Responsibility                                             | Brief                                                      |
| ----------------------------------------- | ---------------------------------------------------------- | ---------------------------------------------------------- |
| **3.1 Confirm staging availability**      | Verify staging environment exists and is accessible        | Prevents updates from being applied directly to production |
| **3.2 Verify staging mirrors production** | Confirm WordPress, PHP, theme, and plugins match live site | Ensures test results are reliable                          |
| **3.3 Freeze staging changes**            | Limit changes strictly to approved updates                 | Isolates update-related effects                            |
| **3.4 Apply updates in controlled order** | Update core (if applicable), then plugins, then theme      | Reduces conflict and improves traceability                 |
| **3.5 Monitor update results**            | Observe errors, warnings, or site breakage                 | Detects failures early                                     |
| **3.6 Record update outcomes**            | Document success or issues found in staging                | Preserves traceability and handoff clarity                 |

---
### **1. Confirm Staging Environment**

`purpose: ensure updates are never applied directly to production`

**What:**  
The employee must confirm that a **dedicated staging environment** exists and that all update work will be performed there, not on the live site.

**How (Instructions for New Employees):**

1. Open the hosting control panel or site management tool.
    
2. Locate the environment labeled **staging**, **dev**, or **test**.
    
3. Verify the staging URL is different from the live domain.
    
4. Log in to the WordPress Admin Dashboard of the staging site.
    
5. Confirm the staging site loads correctly and is accessible.
    

**Training Note:**  
If you are not completely certain the environment is staging, **do not proceed**.

---

### **2. Verify Staging Mirrors Production**

`purpose: ensure testing results accurately represent live behavior`

**What:**  
The employee must confirm that the staging environment closely matches the production environment.

**How (Instructions for New Employees):**

1. Check the WordPress version in staging.
    
2. Confirm the active theme matches production.
    
3. Confirm all active plugins match production.
    
4. Check the PHP version via hosting panel or Site Health.
    
5. Verify that recent content and settings are present.
    

**Training Note:**  
Testing on a staging site that does not match production can hide real issues.

---

### **3. Freeze Update Scope**

`purpose: isolate the impact of approved updates`

**What:**  
The employee must ensure that **only approved updates** are applied in staging and no unrelated changes are introduced.

**How (Instructions for New Employees):**

1. Do not edit content, settings, or code in staging.
    
2. Do not install or remove plugins or themes.
    
3. Apply **only** the updates approved in STEP 2.
    
4. Do not bulk-update additional items, even if they appear safe.
    

**Training Note:**  
If it was not reviewed earlier, it must not be updated now.

---

### **4. Apply Updates in Controlled Order**

`purpose: reduce conflicts and simplify troubleshooting`

**What:**  
The employee must apply updates one at a time in a defined sequence.

**How (Instructions for New Employees):**

1. Update **WordPress core first**, if included.
    
2. Update plugins **one by one**, starting with lower-risk plugins.
    
3. Update the active theme **last**.
    
4. Wait for each update to fully complete before continuing.
    

**Training Note:**  
Applying updates individually makes it easier to identify the source of any issue.

---

### **5. Monitor Site After Each Update**

`purpose: detect failures as early as possible`

**What:**  
The employee must observe the staging site immediately after each update.

**How (Instructions for New Employees):**

1. Watch update messages for errors or warnings.
    
2. Refresh the staging site in the browser.
    
3. Confirm the WordPress Admin Dashboard remains accessible.
    
4. Look for visible layout or functional issues.
    

**Training Note:**  
If an issue appears, stop applying further updates.

---

### **6. Record Update Outcomes**

`purpose: preserve traceability and support team handoff`

**What:**  
The employee must document the outcome of each update applied in staging.

**How (Instructions for New Employees):**

1. Record the name of each updated component.
    
2. Mark the result as **success** or **issue found**.
    
3. Write a short note describing any error or unexpected behavior.
    
4. Mark staging as ready for QA only if no unresolved issues exist.
    

**Training Note:**  
Clear documentation prevents repeated mistakes and speeds up future work.

---

### **Doctrine Reminder**

> _Staging exists to absorb failure so production does not._

---

If you want, next I can:

- Match this same quality for **STEP 4 — QA Testing (Training Mode)**
    
- Convert STEP 3 into a **printable trainee worksheet**
    
- Create **common failure scenarios** for onboarding practice