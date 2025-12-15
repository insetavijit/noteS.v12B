This step enforces the **explicit rejection** of any backup that fails to meet required safety, completeness, or restore-readiness standards.

The objective is **not** to salvage a questionable backup.  
The objective is to **prevent unsafe backups from being relied upon** in critical operations.

A backup **must be rejected immediately** if **any** of the following conditions apply:

- Backup execution did **not complete successfully**
    
- Files-only or database-only backups
    
- Missing WordPress directories, missing database tables, or partial scope
    
- Backup is **stale**, outdated, or created after changes occurred
    
- Restore option is missing, disabled, expired, or untested
    
- Backup size indicates missing or excluded data
    
- Backup storage lacks redundancy or adequate retention
    

This step must be treated as a **hard stop**, not a judgment call.  
Proceeding with an invalid backup **creates unbounded risk** and shifts failure impact to recovery time and data loss.

The operator must approach this step using **elimination thinking**:

> If this backup fails during restore—  
> will the damage be limited to downtime,  
> or will it result in irreversible data loss?

If there is **any uncertainty**, the backup must be **rejected**.  
The correct response is to **correct the underlying issue, regenerate a new full backup, and restart validation**.

This gate exists to ensure that **only backups meeting strict safety criteria advance**, protecting both system integrity and professional accountability.

---

Say **“next”** and I’ll continue with **Approve Backup for Rollback**.