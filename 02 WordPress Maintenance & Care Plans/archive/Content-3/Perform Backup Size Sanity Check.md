This step verifies that the backup’s **archive size aligns with the expected scale of the site**, serving as a quick but critical signal of completeness.

The objective is **not** to measure exact size.  
The objective is to **detect obvious anomalies** that indicate missing files, excluded directories, or incomplete database dumps.

A backup passes the sanity check only if **all conditions** are met:

- Backup size is **consistent with historical backups** for the site
    
- File archive size reasonably reflects **uploads, themes, plugins, and custom assets**
    
- Database dump size is **non-trivial** and proportionate to site activity
    
- No sudden, unexplained size drops or spikes are present
    

This step must be treated as an **anomaly-detection gate**, not a precision metric.  
Backups that are dramatically smaller (or unexpectedly larger) than normal often signal **silent failures**.

The operator must approach this step using **pattern-recognition thinking**:

> If this backup were restored—  
> does its size suggest a **complete snapshot**,  
> or does it hint that something critical is missing?

If the answer is **uncertain or negative**, the backup must be **rejected**.  
The correct response is to **inspect backup scope settings, correct exclusions, and regenerate the backup**.

This gate exists to catch **high-risk backup defects early**, before time is spent on restore testing or updates.

---

Say **“next”** and I’ll continue with **Confirm Restore Capability**.