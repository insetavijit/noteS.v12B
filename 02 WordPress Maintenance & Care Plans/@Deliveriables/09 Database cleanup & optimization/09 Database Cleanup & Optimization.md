Database maintenance is a **preventive performance discipline**, not a one-time cleanup task. The objective is to keep the database **lean, efficient, and query-optimized** as the site evolves. Assume that plugins, revisions, logs, and transients will accumulate continuously unless deliberately controlled. Every cleanup must be **preceded by a backup**, executed methodically, and validated through performance comparison.

### **[[1. Database Backup Before Cleanup]]**

```
Create full database backup · Verify backup integrity · Confirm restore availability · Label snapshot clearly
```

### **[[2. Remove Old Revisions & Drafts]]**

```
Identify excess post revisions · Remove outdated drafts · Limit future revision counts · Validate content integrity
```

### **[[3. Cleanup Spam & Trash Data]]**

```
Delete spam comments · Empty trash posts · Remove trashed metadata · Verify comment system stability
```

### **[[4. Delete Expired Transients]]**

```
Identify expired transients · Remove stale cache entries · Clear temporary option data · Verify application behavior
```

### **[[5. Optimize Database Tables]]**

```
Repair corrupted tables · Optimize and defragment tables · Rebuild indexes if required · Confirm table health
```

### **[[6. Remove Orphan Tables & Entries]]**

```
Identify unused tables · Detect orphaned options · Remove leftover plugin data · Validate no dependency breakage
```

### **[[7. Reduce Autoloaded Data]]**

```
Review autoload size · Identify heavy autoload options · Disable unnecessary autoload entries · Measure memory impact
```

### **[[8. Query Performance Review]]**

```
Analyze slow queries · Identify heavy database calls · Review plugin query behavior · Confirm performance improvement
```

