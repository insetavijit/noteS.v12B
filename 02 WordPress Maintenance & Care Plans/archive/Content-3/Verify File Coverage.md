This step verifies that the backup process has **fully completed without errors** and has transitioned into a **stable, usable state** before any validation or reliance occurs.

The objective is **not** to see a backup entry in history.  
The objective is to **confirm that the backup finished cleanly and is eligible for validation**.

A backup is only considered successfully completed if **all conditions** are met:

- Backup status explicitly shows **Completed / Successful**
    
- No warnings, partial failures, or skipped components are reported
    
- The backup entry is **accessible and inspectable**
    
- Backup artifacts are fully generated and not pending or queued
    

This step must be treated as a **hard execution checkpoint**, not an assumption.  
Backups that appear in logs but are **in-progress, paused, failed, or silently incomplete** do not qualify.

The operator must approach this step using **completion-certainty thinking**:

> If validation or restore were attempted right now—  
> would this backup behave as a **finished, stable artifact**,  
> or would it fail due to incomplete execution?

If the answer is **not an immediate yes**, the backup must be **rejected**.  
The correct response is to **identify the failure, correct the cause, and rerun the backup**.

This gate exists to ensure that **only fully completed backups proceed to validation**, preventing false confidence and rollback failure later in the workflow.

If you want the **next one**, here is the **parallel overview** for **Verify File Coverage**, written in the same doctrine style.

---

### **Verify File Coverage — Step Overview**

This step verifies that the completed backup includes **all required WordPress file components** necessary to fully restore the site’s operational state.

The objective is **not** to see “files included” in a summary.  
The objective is to **confirm that every critical directory and custom asset is actually captured**.

File coverage is only considered valid if **all conditions** are met:

- WordPress **core files** (`wp-admin`, `wp-includes`, root PHP files) are included
    
- **Themes** directory (active and inactive) is present
    
- **Plugins** directory (active and inactive) is present
    
- **Uploads** directory (media library) is present
    
- **Custom or MU-plugin directories** (if applicable) are included
    

This step must be treated as a **restore-readiness check**, not a surface inspection.  
Missing any single directory can render a restore **functionally broken**, even if the database is intact.

The operator must approach this step using **full-site reconstruction thinking**:

> If the site were restored using this backup—  
> would every plugin, theme, and asset required for operation  
> be present **without manual reconstruction**?

If the answer is **not a confident yes**, the backup must be **rejected**.  
The correct response is to **correct backup scope settings and regenerate the backup**.

This gate exists to ensure that **file-level completeness is guaranteed before database validation begins**.

---

Say **“next”** and I’ll continue with **Verify Database Coverage** in the same format.