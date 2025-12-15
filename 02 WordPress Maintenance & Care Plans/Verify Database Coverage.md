This step verifies that the completed backup includes the **entire WordPress database**, captured in a form that can be **fully and reliably restored**.

The objective is **not** to confirm that “a database file exists.”  
The objective is to **ensure complete data preservation**, including content, configuration, users, and plugin-generated data.

Database coverage is only considered valid if **all conditions** are met:

- The **full database dump** is present and accessible
    
- All WordPress tables (`wp_*`) are included
    
- Any **custom-prefix tables** are included
    
- Plugin- or feature-generated tables (e.g., WooCommerce, forms, security logs) are included
    
- The database dump size is **reasonable and non-trivial** relative to site scale
    

This step must be treated as a **data-integrity gate**, not a metadata check.  
A restore with missing tables can result in **silent data loss**, broken functionality, or corrupted site state.

The operator must approach this step using **data-recovery thinking**:

> If the site were restored from this backup—  
> would all content, users, settings, and transactions  
> reappear **exactly as they exist right now**?

If the answer is **not a clear yes**, the backup must be **rejected**.  
The correct response is to **correct database scope settings and regenerate the backup**.

This gate exists to ensure that **data-level completeness is guaranteed before freshness and restore validation**.