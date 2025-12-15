This step ensures that a **fresh, on-demand full backup** is created when an existing backup does not meet operational safety requirements.

The objective is **not** to run backups routinely.  
The objective is to **establish a known-good rollback point** that accurately represents the site’s current state immediately before risk-introducing actions.

A manual or on-demand backup is required if **any** of the following conditions apply:

- No recent backup exists that is suitable for upcoming operations
    
- The latest backup is **scheduled, outdated, partial, or unverified**
    
- Site changes have occurred since the last confirmed backup
    
- Backup freshness or integrity is uncertain
    

This step must be treated as a **risk-reset control**, not a redundancy.  
Proceeding without a fresh backup when conditions have changed **does not qualify as safe practice**.

The operator must approach this step using **point-of-failure thinking**:

> If the site fails immediately after the next action—  
> does this backup allow a **clean, exact rollback**  
> to the site state that exists right now?

If the answer is **not an unequivocal yes**, a new full backup must be created **before proceeding**.

This backup must include:

- **All WordPress files** (core, themes, plugins, uploads, custom directories)
    
- The **entire database**
    
- A **clear, executable restore path**
    

This gate exists to ensure that **every critical operation begins from a protected state**, making failures recoverable, contained, and professionally defensible.