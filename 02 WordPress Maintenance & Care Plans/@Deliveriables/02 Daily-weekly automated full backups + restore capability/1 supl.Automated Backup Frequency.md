## Strategic Context

**Foundational Principle:** Backup frequency defines your true safety margin, not the backup tool itself. The objective transcends mere scheduling—it requires ensuring that recovery points exist at predictable intervals and are continuously maintained without human dependency.

**Theoretical Foundation:** Recovery Point Objective (RPO) represents the maximum acceptable data loss window. A 24-hour RPO means accepting up to 24 hours of lost data in worst-case scenarios. Daily backups operationalize this constraint by ensuring no gap exceeds the RPO threshold.

**Critical Assumption:** Any missed or partial backup directly increases operational risk. Automation must be observable, verifiable, and policy-enforced—never trust-based.


|Task|Responsibility|Risk Mitigated|Validation Method|
|---|---|---|---|
|**1.1** Define backup schedule|Establish daily/weekly cadence aligned with RPO|Undefined recovery expectations|Documented policy approval|
|**1.2** Configure daily full backups|Automate complete backups (files + database)|Extended data loss windows|Successful execution log|
|**1.3** Configure weekly archive snapshot|Create long-term retention point|Delayed failure detection|Archive storage verification|
|**1.4** Confirm component completeness|Verify all critical components included|Partial/unusable backups|Manifest inspection|
|**1.5** Enable automation|Ensure execution without manual intervention|Human dependency failure|7-day execution history|
|**1.6** Verify backup job execution|Monitor daily completion status|Silent backup failures|Dashboard status check|
|**1.7** Review backup logs|Inspect for warnings or partial failures|Silent degradation|Log audit completion|
|**1.8** Configure failure alerting|Enable notifications for failures|Undetected backup gaps|Test alert verification|
|**1.9** Resolve failed/skipped backups|Investigate and remediate failures|Continued system unreliability|Resolution documentation|
|**1.10** Record backup frequency policy|Document schedule, rationale, approval|Governance gaps, audit failures|Policy repository entry|

---

## 1.1 Define Backup Schedule

### Purpose

Establish predictable recovery points aligned with documented RPO requirements and operational risk profile.

### Conceptual Foundation

The backup schedule represents a **risk-cost optimization decision**. More frequent backups reduce RPO (lower data loss risk) but increase storage costs and system load. The schedule must balance business continuity requirements against operational constraints.

### Decision Framework

**RPO-Based Cadence Selection:**

|Site Classification|RPO Target|Minimum Backup Frequency|Archive Frequency|
|---|---|---|---|
|**Mission-Critical**|≤4 hours|4x daily (every 6 hours)|Weekly + Monthly|
|**Business-Critical**|≤12 hours|2x daily|Weekly|
|**Standard Production**|≤24 hours|Daily|Weekly|
|**Low-Change**|≤48 hours|Daily (still required)|Weekly|

**RPO Calculation Logic:**

```
Maximum Data Loss = Time Since Last Successful Backup

If last backup: 11:00 PM yesterday
Failure occurs: 10:00 PM today
Data loss window: 23 hours (acceptable if RPO = 24 hours)
```

### Execution Protocol

**Step 1: Assess RPO Requirements**

Evaluate business impact factors:

- **Revenue implications**: Does site process transactions? What is hourly transaction volume?
- **Content velocity**: How frequently does critical content change?
- **User-generated data**: Are there comments, submissions, registrations that cannot be recreated?
- **Compliance requirements**: Do regulations mandate specific RPO thresholds?

**Decision Tree:**

```
Does site process financial transactions?
  └─ YES → Mission-Critical (RPO ≤4 hours)
  └─ NO → Continue evaluation

Does site have daily content updates or user submissions?
  └─ YES → Business-Critical (RPO ≤12 hours)
  └─ NO → Standard Production (RPO ≤24 hours)
```

**Step 2: Confirm Operational Constraints**

Validate the proposed schedule against:

- **Hosting resource limits**: Backup processes consume CPU, memory, I/O
- **Storage capacity**: Daily backups accumulate; ensure retention policy is sustainable
- **Traffic patterns**: Schedule during low-traffic windows to minimize performance impact
- **Backup window**: Can full backup complete within allocated time?

**Resource Capacity Check:**

```
Backup Size Estimate = Database Size + File System Size + 20% overhead
Backup Duration Estimate = Backup Size ÷ Average Transfer Rate
Available Window = Low-Traffic Period Duration

If Backup Duration > Available Window:
  → Consider incremental backups between full backups
  → Optimize database (remove transients, spam)
  → Upgrade hosting resources
```

**Step 3: Define Complete Schedule**

Document the following parameters:

|Parameter|Specification|Example|
|---|---|---|
|**Daily Backup Time**|Exact execution time (UTC)|02:00 UTC (lowest traffic)|
|**Weekly Archive Day**|Day of week for long-term snapshot|Sunday|
|**Monthly Archive Date**|Date for extended retention|1st of each month|
|**Backup Window**|Maximum allowed duration|2 hours|
|**RPO Justification**|Business rationale|"E-commerce site processing $2K/day; 24hr data loss = unacceptable customer impact"|

**Step 4: Obtain Approval**

Submit schedule for review including:

- Proposed frequency and timing
- RPO/RTO justification with business impact quantification
- Resource consumption estimates
- Cost implications (storage, compute, monitoring)

**Approval Authority:** Infrastructure Lead or designated backup policy owner

**Step 5: Document Policy**

Record approved schedule in:

- Configuration management database (CMDB)
- Backup policy repository
- Site-specific documentation

**Template:**

```
Site: [example.com]
Classification: [Business-Critical]
RPO: 12 hours
Backup Frequency: 2x daily (02:00 UTC, 14:00 UTC)
Archive Frequency: Weekly (Sunday 02:00 UTC)
Approved By: [Name, Date]
Next Review: [Date + 90 days]
```

### Failure Prevention Patterns

**Anti-Pattern 1: One-Size-Fits-All Scheduling**

- _Failure_: Applying same schedule to all sites regardless of risk profile
- _Consequence_: Under-protection of critical sites; over-investment in low-risk sites
- _Mitigation_: Site-specific RPO assessment with documented justification

**Anti-Pattern 2: Convenience-Based Timing**

- _Failure_: Scheduling backups based on staff availability rather than optimal system conditions
- _Consequence_: Performance degradation during high-traffic periods; incomplete backups due to resource contention
- _Mitigation_: Schedule during proven low-traffic windows; use analytics to identify optimal timing

**Anti-Pattern 3: Unapproved Schedule Changes**

- _Failure_: Modifying backup frequency without policy review
- _Consequence_: RPO drift; audit failures; uncoordinated changes
- _Mitigation_: Require formal approval for all schedule modifications

### Training Guidance

**Core Concept for New Team Members:**

> "A backup schedule is a risk management decision encoded as operational policy. You are not setting a reminder—you are defining the maximum acceptable data loss for a production system. Every hour between backups represents potential unrecoverable work. Treat schedule definition with the gravity it deserves."

**Common Misconceptions:**

1. ❌ "Daily backups are always sufficient"
    
    - ✅ Reality: RPO determines sufficiency; high-value sites may require sub-daily backups
2. ❌ "Backup timing doesn't matter"
    
    - ✅ Reality: Timing affects backup success rate, duration, and system performance impact
3. ❌ "Hosting provider backups eliminate the need for our schedule"
    
    - ✅ Reality: Provider backups are supplementary, not primary; we maintain independent backup responsibility

---

## 1.2 Configure Daily Full Backups

### Purpose

Ensure a complete and current recovery point exists every 24 hours, capturing entire site state for rapid restoration.

### Conceptual Foundation

A **full backup** captures the complete system state at a point in time, enabling single-step restoration without dependency on previous backups. Unlike incremental or differential backups (which require chains), full backups provide **restoration simplicity** at the cost of higher storage consumption and longer backup windows.

### Technical Specification

**Complete Backup Scope:**

|Component Category|Specific Inclusions|Criticality|Typical Size|
|---|---|---|---|
|**Database**|All WordPress tables (wp_*)|Critical|50-500 MB|
|**Core Files**|/wp-admin/, /wp-includes/|Critical|30-50 MB|
|**Themes**|/wp-content/themes/ (all, not just active)|High|10-100 MB|
|**Plugins**|/wp-content/plugins/ (all, not just active)|High|50-200 MB|
|**Uploads**|/wp-content/uploads/ (media library)|Critical|1-50 GB|
|**Configuration**|wp-config.php, .htaccess, webserver configs|Critical|<1 MB|
|**Custom Content**|/wp-content/mu-plugins/, custom directories|Medium|Variable|

**Total Backup Size Estimate:** 2-100 GB typical range

### Execution Protocol

**Step 1: Select Backup Tool**

Confirm tool selection from approved options:

- **Plugin-based**: UpdraftPlus, BlogVault, BackupBuddy
- **Hosting-integrated**: cPanel backups, managed hosting backup systems
- **Server-level**: Custom scripts with wp-cli + mysqldump
- **External service**: CodeGuard, VaultPress, ManageWP

**Selection Criteria:**

- Supports full backup mode (not incremental-only)
- Includes all required components by default or configuration
- Provides backup verification/integrity checking
- Integrates with monitoring/alerting systems
- Meets security requirements (encryption in transit/at rest)

**Step 2: Configure Backup Scope**

Within chosen tool, explicitly enable:

```
[✓] WordPress Database (full export, all tables)
[✓] WordPress Core Files (/wp-admin/, /wp-includes/)
[✓] Themes Directory (/wp-content/themes/)
[✓] Plugins Directory (/wp-content/plugins/)
[✓] Uploads Directory (/wp-content/uploads/)
[✓] Configuration Files (wp-config.php, .htaccess)
[✓] Custom Directories (specify any additional paths)
[✓] .htaccess and server configuration
```

**Critical Configuration Check:**

- Verify "Include inactive themes/plugins" is enabled
    - _Rationale_: Restoration may require inactive components; excluding them creates incomplete backups
- Confirm "Follow symlinks" if applicable
    - _Rationale_: Symlinked directories must be backed up, not just their links
- Enable "Preserve file permissions"
    - _Rationale_: Incorrect permissions post-restore cause functionality failures

**Step 3: Set Automation Schedule**

Configure automated execution:

|Setting|Value|Rationale|
|---|---|---|
|**Frequency**|Once per 24 hours|Meets standard RPO ≤24 hours|
|**Execution Time**|02:00-04:00 AM local time|Lowest traffic period|
|**Timezone**|Server timezone (documented)|Prevents schedule drift|
|**Backup Type**|Full (not incremental)|Simplifies restoration; no dependency chains|
|**Timeout**|2-4 hours|Allows large backups to complete|
|**Retry Logic**|2 attempts with 15-min delay|Handles transient failures|

**Step 4: Execute Manual Test Run**

Before activating automation:

1. **Initiate manual backup** via tool interface
2. **Monitor execution** in real-time
3. **Record execution metrics:**
    - Start time
    - Completion time
    - Total duration
    - Final backup size
    - Any warnings or errors

**Acceptance Criteria:**

- Status = "Completed" or "Success" (not "Partial" or "Failed")
- No error messages in execution log
- Backup duration < allocated backup window
- Backup size consistent with component sizing expectations

**Step 5: Verify Backup Artifact**

Inspect the completed backup:

1. **Locate backup file** in configured storage location
2. **Check file integrity:**
    - File size > 0 bytes
    - File not corrupted (attempt to open/extract archive)
    - Filename includes timestamp for identification
3. **Inspect backup contents** (sample check):
    - Extract or browse backup archive
    - Confirm presence of database dump file
    - Verify wp-content/ directory structure exists
    - Spot-check a few uploads to confirm media backup

**Red Flags:**

- ⚠️ Backup size dramatically smaller than expected (incomplete)
- ⚠️ Backup file cannot be opened or extracted (corrupted)
- ⚠️ Missing expected directories in archive
- ⚠️ Database dump file absent or empty

**Step 6: Activate Automation**

Once manual test succeeds:

1. **Enable scheduled execution** in backup tool
2. **Disable conflicting schedules** (if multiple backup tools exist)
3. **Wait for first automated execution** (next scheduled time)
4. **Verify automated run completes successfully**
5. **Confirm backup artifact created in expected location**

### Failure Prevention Patterns

**Anti-Pattern 1: Assuming Default Inclusions**

- _Failure_: Not explicitly verifying backup scope; relying on tool defaults
- _Consequence_: Critical components excluded (common: uploads directory, custom mu-plugins)
- _Mitigation_: Explicit configuration verification; manual inspection of first backup

**Anti-Pattern 2: Incremental-Only Strategies**

- _Failure_: Using incremental backups without periodic full backups
- _Consequence_: Restore requires complex chain reconstruction; single link failure breaks entire chain
- _Mitigation_: Daily full backups as baseline; incremental only as supplementary

**Anti-Pattern 3: Resource Contention During Backups**

- _Failure_: Scheduling backups during peak traffic
- _Consequence_: Backup timeouts, incomplete backups, site performance degradation
- _Mitigation_: Schedule during verified low-traffic windows; monitor server load during execution

### Training Guidance

**Core Concept:**

> "A daily full backup is your time machine with 24-hour granularity. If something breaks today, you can return to yesterday's state completely. But only if yesterday's backup actually captured everything. 'Full' means full—not 'most things' or 'important things.' Every exclusion is a potential restore failure."

**Verification Mantra:** "Backup reported success ≠ Backup actually complete"

Always verify:

1. Tool reports success
2. Artifact exists in storage
3. Artifact size is reasonable
4. Contents can be extracted/browsed
5. Database dump is present and non-empty

---

## 1.3 Configure Weekly Archive Snapshot

### Purpose

Preserve long-term recovery points beyond daily rotation to protect against delayed failure detection and silent data corruption.

### Conceptual Foundation

**The Delayed Failure Problem:** Not all failures are immediately apparent. Silent data corruption, gradual malware infiltration, or configuration drift may not manifest for weeks. Daily backups rotate (get deleted) based on retention policies, potentially eliminating all clean recovery points before the issue is discovered.

**Archive Strategy:** Weekly snapshots create **temporal depth** in backup coverage, enabling restoration to known-good states from weeks or months prior.

**Retention Asymmetry:**

```
Daily Backups: High frequency, short retention (30-60 days)
Weekly Archives: Low frequency, long retention (12-26 weeks)
Monthly Archives: Lowest frequency, longest retention (12-24 months)
```

### Execution Protocol

**Step 1: Define Archive Day**

Select weekly execution day based on operational patterns:

|Day|Rationale|Best For|
|---|---|---|
|**Sunday**|Lowest weekend traffic; captures week's final state|Most sites|
|**Saturday**|Alternative weekend option|Sites with Sunday content updates|
|**Friday**|Captures week's work before weekend|Business sites closed weekends|

**Decision Factors:**

- Traffic patterns (analytics-verified lowest day)
- Content publication schedule
- Update maintenance windows

**Step 2: Configure Archive Isolation**

Ensure weekly snapshot is **protected from daily rotation logic**:

**Tool-Specific Approaches:**

**Plugin-Based (UpdraftPlus, BackupBuddy):**

```
Settings → Retention Rules
- Daily backups: Retain 30 (overwrites after 30 days)
- Weekly backups: Retain 12 (kept for 12 weeks)
- Enable "Separate retention for weekly backups"
```

**Server-Level (Custom Scripts):**

```bash
# Daily backup path
/backups/daily/site-2025-01-15.tar.gz

# Weekly archive path (separate directory)
/backups/weekly/site-2025-W03.tar.gz

# Pruning logic skips /backups/weekly/
```

**Hosting-Integrated:**

- Verify provider offers separate weekly backup tier
- Configure if available; supplement with plugin if not

**Step 3: Configure Archive Storage**

Archive snapshots should utilize **secondary storage** when possible:

|Storage Type|Use Case|Cost|Retrieval Time|
|---|---|---|---|
|**Primary Cloud**|Quick access archives (recent weeks)|$$$|Minutes|
|**Archive Cloud**|Long-term retention (AWS Glacier, etc.)|$|Hours|
|**Local Secondary**|On-premise NAS for hybrid approach|$$|Minutes|

**Recommended Architecture:**

```
Daily Backups → Primary Cloud (S3 Standard)
Weekly Archives → Primary Cloud (first 4 weeks) + Archive Cloud (older)
Monthly Archives → Archive Cloud only
```

**Step 4: Label Archives Clearly**

Implement consistent naming convention:

```
Format: {site}-{backup-type}-{date-identifier}.{extension}

Examples:
example-com-weekly-2025-W03.tar.gz
example-com-weekly-2025-01-21.zip
example-com-archive-2025-Q1.tar.gz
```

**Metadata to Include:**

- Backup date/week/month identifier
- Backup type (daily/weekly/monthly)
- Site identifier
- Backup tool version (for restoration compatibility)

**Step 5: Verify Archive Retention**

**30-Day Verification Test:**

1. **Execute first weekly archive** (Day 0)
2. **Wait 7 days** → Verify archive still exists (Day 7)
3. **Wait 30 days** → Verify archive still exists (Day 30)
4. **Verify daily backups from Day 0-7** have been pruned (if 7-day retention)
5. **Confirm weekly archive** remains untouched

**Retention Validation Checklist:**

```
[✓] Weekly archives not deleted by daily rotation logic
[✓] Archive count matches retention policy (12 weeklies = 12 weeks)
[✓] Oldest archive date aligns with retention window
[✓] Storage capacity sufficient for retention policy
[✓] Archive retrievability tested (can download and extract)
```

**Step 6: Document Archive Policy**

Record in policy repository:

```
Archive Configuration:
- Frequency: Weekly (every Sunday 02:00 UTC)
- Retention: 12 weeks (3 months)
- Storage: AWS S3 Standard (weeks 1-4), Glacier Deep Archive (weeks 5-12)
- Naming: {site}-weekly-{YYYY-Wnn}.tar.gz
- Verification: Monthly archive retrieval test
- Owner: [Infrastructure Lead]
- Last Review: [Date]
```

### Failure Prevention Patterns

**Anti-Pattern 1: Weekly Archives Deleted by Daily Logic**

- _Failure_: Weekly backups stored in same location/policy as dailies; get pruned unintentionally
- _Consequence_: No long-term recovery points despite configured weekly backups
- _Mitigation_: Separate storage paths, explicit retention overrides, monthly verification

**Anti-Pattern 2: Archive Label Ambiguity**

- _Failure_: Archives labeled identically to daily backups
- _Consequence_: During crisis, cannot quickly identify appropriate archive to restore
- _Mitigation_: Clear naming conventions with backup type in filename

**Anti-Pattern 3: Archive Storage in Same Location as Dailies**

- _Failure_: Archives subject to same risks as daily backups (provider outage, account compromise)
- _Consequence_: Catastrophic failure eliminates both daily and archive recovery points
- _Mitigation_: Geographic or provider diversity for archive storage

### Training Guidance

**Core Concept:**

> "Daily backups protect you from yesterday's mistakes. Weekly archives protect you from mistakes you haven't discovered yet. When malware silently corrupts data over three weeks, and all your daily backups contain the corrupted data, your weekly archive from before the infection is the only clean restoration point. Archives are insurance against delayed failure detection."

**Real-World Scenario:**

```
Timeline:
- Week 1: Malware infection (undetected)
- Week 2-4: Gradual data corruption spreads
- Week 5: Corruption discovered

Daily backups: All contain corrupted data (30-day retention)
Weekly archives: Week 1 archive is pre-infection (clean)
Result: Restoration possible only because of weekly archive retention
```

---

## 1.4 Confirm Component Completeness

### Purpose

Ensure backups are fully restorable by verifying all critical components are included; detect partial backups that appear successful but are invalid.

### Conceptual Foundation

**The Partial Backup Hazard:** Backup tools may report "success" even when components are excluded due to:

- Permission errors (files/directories inaccessible)
- Timeout limitations (backup window too short)
- Configuration errors (paths misconfigured)
- Storage capacity (backup truncated when storage fills)
- Plugin conflicts (backup process interrupted)

**Verification Principle:** Backup validity is determined by inspection, not tool reporting. A backup is only complete when explicitly verified as containing all restoration prerequisites.

### Component Validation Matrix

|Component|Verification Method|Acceptance Criteria|Failure Indicator|
|---|---|---|---|
|**Database**|Check for .sql file in archive|File present, size >1 MB|Missing file or <1 MB|
|**Core Files**|Verify wp-admin/ directory exists|Directory present with subdirectories|Missing or empty|
|**Themes**|Check wp-content/themes/|Active + inactive themes present|Only active theme|
|**Plugins**|Check wp-content/plugins/|All plugins present (20+ typical)|<5 plugins|
|**Uploads**|Check wp-content/uploads/ size|Matches expected media library size (±10%)|Dramatically smaller|
|**Config**|Verify wp-config.php present|File present, contains DB credentials|Missing or empty|

### Execution Protocol

**Step 1: Access Most Recent Backup Artifact**

**Location varies by tool:**

- Plugin backups: Check plugin's backup directory or remote storage
- Hosting backups: Access via hosting control panel backup section
- Server-level: Navigate to configured backup directory path

**Step 2: Perform Size Sanity Check**

**Baseline Establishment (First Backup):**

```
1. Record initial backup size: [e.g., 2.4 GB]
2. Document component breakdown:
   - Database: 250 MB
   - Files: 180 MB
   - Uploads: 2.0 GB
3. Set acceptable variance: ±20% for dailies, ±50% for growth
```

**Ongoing Monitoring (Subsequent Backups):**

```
If backup size varies by:
  <20% from baseline → Normal (daily content changes)
  20-50% from baseline → Investigate (major content upload or deletion)
  >50% from baseline → RED FLAG (likely incomplete or corrupted)
  
If backup size is consistently smaller → CRITICAL (components being excluded)
```

**Common Size Indicators:**

- Small backup (<100 MB) = Database only, files excluded
- Medium backup (100-500 MB) = DB + files, uploads excluded
- Large backup (1-50 GB) = Complete backup including media

**Step 3: Inspect Backup Contents**

**Method A: Archive Extraction (Most Reliable)**

```bash
# Extract backup to temporary location
mkdir /tmp/backup-verification
tar -xzf backup-2025-01-15.tar.gz -C /tmp/backup-verification

# Verify directory structure
ls -lh /tmp/backup-verification/
```

**Expected Structure:**

```
/tmp/backup-verification/
├── database.sql (database dump)
├── wp-admin/ (core)
├── wp-includes/ (core)
├── wp-content/
│   ├── themes/
│   ├── plugins/
│   ├── uploads/
│   └── mu-plugins/ (if applicable)
├── wp-config.php
└── .htaccess
```

**Method B: Archive Listing (Faster, Less Thorough)**

```bash
# List archive contents without extracting
tar -tzf backup-2025-01-15.tar.gz | head -50

# Check for critical paths
tar -tzf backup-2025-01-15.tar.gz | grep -E "(wp-config|database\.sql|wp-content/uploads)"
```

**Method C: Tool-Specific Manifest (If Available)**

Some backup tools generate manifest files:

```
backup-2025-01-15.tar.gz
backup-2025-01-15-manifest.json  ← Inventory of backed-up files
```

Inspect manifest for component counts and sizes.

**Step 4: Database Verification**

**Critical Checks:**

1. **Database dump file exists** (often named database.sql, dump.sql, or {site}.sql)
2. **File size reasonable** (typical: 50-500 MB; minimum: 1 MB for basic site)
3. **File format correct** (should be plain text SQL)

**Deep Verification (Sample):**

```bash
# Check first few lines of database dump
head -20 database.sql
```

**Expected Content:**

```sql
-- MySQL dump
-- Host: localhost
-- Database: wp_database

CREATE TABLE `wp_posts` ( ... );
CREATE TABLE `wp_options` ( ... );
INSERT INTO `wp_posts` VALUES ( ... );
```

**Red Flags:**

- Empty file (0 bytes)
- Binary data instead of SQL text
- Contains only structure (CREATE) without data (INSERT)
- Missing critical tables (wp_posts, wp_options, wp_users)

**Step 5: Uploads Directory Verification**

**Uploads are often the largest component and most likely to be excluded due to:**

- Timeouts during backup
- Storage quota limits
- Incorrect path configuration
- Exclusion rules (accidental)

**Verification Steps:**

1. **Check if wp-content/uploads/ directory exists in backup**
2. **Verify upload size** matches expected media library size (±20%)
3. **Spot-check year/month subdirectories** (e.g., 2024/01/, 2024/12/)
4. **Confirm recent uploads present** (check current month directory)

**Size Comparison:**

```bash
# On live site
du -sh /path/to/wordpress/wp-content/uploads/
# Output: 5.2G

# In backup archive
tar -tzf backup.tar.gz | grep "wp-content/uploads" | wc -l
# Output: 12,458 files

# Size from extracted backup
du -sh /tmp/backup-verification/wp-content/uploads/
# Output: 5.1G (within 20% tolerance)
```

**Step 6: Configuration File Verification**

**Critical Config Files:**

|File|Purpose|Verification|
|---|---|---|
|`wp-config.php`|Database credentials, security keys|Must be present, >2 KB|
|`.htaccess`|URL rewriting, security rules|Must be present if using permalinks|
|`php.ini` (if custom)|PHP configuration|Include if custom settings applied|

**wp-config.php Inspection:**

```bash
grep "DB_NAME\|DB_USER\|DB_PASSWORD\|DB_HOST" wp-config.php
```

**Expected Output:**

```php
define('DB_NAME', 'wp_database');
define('DB_USER', 'wp_user');
define('DB_PASSWORD', 'secure_password');
define('DB_HOST', 'localhost');
```

**Security Note:** Backup files contain sensitive credentials; ensure appropriate storage access controls.

**Step 7: Flag Incomplete Backups Immediately**

**If ANY component is missing or anomalous:**

1. **STOP** - Do not mark backup as valid
2. **Create incident ticket** with details:
    - Backup date/timestamp
    - Missing/anomalous components
    - Size discrepancies
    - Tool errors/warnings (if any)
3. **Escalate to Infrastructure Lead** immediately
4. **Do not proceed** with subsequent steps until resolved
5. **Re-run backup manually** and re-verify
6. **Document root cause** once identified

**Escalation Template:**

```
SUBJECT: INCOMPLETE BACKUP DETECTED - [Site Name] - [Date]

Backup: [site]-2025-01-15.tar.gz
Status: INVALID - INCOMPLETE

Missing/Anomalous Components:
- [✗] Uploads directory: Expected 5.2 GB, backup shows 150 MB
- [✓] Database: Present, 250 MB
- [✓] Themes: Present, 12 themes
- [✓] Plugins: Present, 28 plugins

Tool Warnings:
- [Log excerpt showing timeout or error]

Action Required:
1. Investigate root cause (timeout? storage quota? permission?)
2. Remediate issue
3. Re-run backup
4. Verify completeness before resuming automation

Reported By: [Your Name]
Date/Time: [Timestamp]
```

### Failure Prevention Patterns

**Anti-Pattern 1: "Success" Status Acceptance Without Verification**

- _Failure_: Trusting tool's "backup successful" message without inspecting contents
- _Consequence_: Discovering missing components only during actual restoration (too late)
- _Mitigation_: Mandatory post-backup verification; tool status is insufficient

**Anti-Pattern 2: Ignoring Size Anomalies**

- _Failure_: Not establishing baseline backup size; not monitoring for deviations
- _Consequence_: Gradual component exclusion (e.g., uploads) goes unnoticed for months
- _Mitigation_: Document baseline size; alert on >20% variance

**Anti-Pattern 3: Assuming Active-Only Backup is Sufficient**

- _Failure_: Backing up only active themes/plugins, excluding inactive ones
- _Consequence_: Cannot restore if inactive component was recently deactivated and then needed
- _Mitigation_: Always backup entire wp-content/ directory, not just active components

### Training Guidance

**Core Concept:**

> "A backup is Schrödinger's cat—simultaneously valid and invalid until you open it and verify. The tool's 'success' message tells you the process completed, not that the backup is restorable. Verification converts uncertainty into confidence. Never skip it."

**Verification Checklist for New Team Members:**

Every backup must pass this 5-point check:

```
[✓] 1. Backup artifact exists in storage
[✓] 2. Size is within expected range (baseline ±20%)
[✓] 3. Database dump file present and non-empty
[✓] 4. Uploads directory present and correct size
[✓] 5. Configuration files (wp-config.php) present
```

If ANY checkbox is unchecked → Backup is INVALID → Escalate immediately

---

## 1.5 Enable Automation

### Purpose

Eliminate human dependency and ensure consistent backup execution without requiring manual initiation.

### Conceptual Foundation

**The Automation Imperative:** Manual backups fail not due to technical issues but due to human factors—forgetting, deprioritization, turnover, or assumption that "someone else will do it." Automation converts backup execution from a remembered task to a guaranteed system behavior.

**Reliability Hierarchy:**

```
Fully Automated + Monitored >> Automated Without Monitoring >> Manual + Reminder >> Manual Only
     (99.9% reliability)              (95% reliability)           (70% reliability)    (40% reliability)
```

### Execution Protocol

**Step 1: Confirm Scheduling Mechanism**

Identify and verify the scheduling system:

|Mechanism|Description|Reliability|Verification Method|
|---|---|---|---|
|**Plugin Scheduler**|Backup plugin's built-in cron|High (if WP-Cron functional)|Check plugin dashboard for next run time|
|**Hosting Scheduler**|cPanel/Plesk scheduled tasks|Very High|Verify in hosting control panel|
|**Server Cron**|Linux crontab entries|Very High|Execute `crontab -l` to view|
|**External Service**|ManageWP, MainWP orchestration|High|Verify in service dashboard|

**WP-Cron Limitation Awareness:**

WordPress's pseudo-cron (WP-Cron) depends on site visits to trigger:

- Low-traffic sites may experience schedule delays
- If no visitors for 24 hours, backup may not execute until next visit

**Mitigation:**

```bash
# Disable WP-Cron in wp-config.php
define('DISABLE_WP_CRON', true);

# Add real system cron job
0 2 * * * curl https://example.com/wp-cron.php > /dev/null 2>&1
```

**Step 2: Verify Schedule Matches Policy**

Cross-reference configured schedule against approved backup policy:

|Policy Parameter|Configured Value|Match Status|
|---|---|---|
|Frequency|Daily|✓ Matches|
|Execution Time|02:00 UTC|✓ Matches|
|Backup Type|Full|✓ Matches|
|Components|All (DB + Files)|✓ Matches|

**Mismatch Resolution:**

- If ANY parameter doesn't match policy → Reconfigure before proceeding
- Document reason for deviation if approved exception
- Update policy if configuration is intentionally different

**Step 3: Eliminate Manual Intervention Requirements**

Verify backup executes WITHOUT:

- Manual button clicks
- UI confirmations
- Login requirements
- Popup dismissals
- Email confirmations

**Test Procedure:**

1. Schedule backup for 15 minutes from now
2. Close all browser tabs
3. Wait 20 minutes
4. Check if backup executed and completed
5. If YES → Automation is independent
6. If NO → Dependencies exist; troubleshoot

**Step 4: Disable Conflicting Schedules**

**Common Conflict Scenarios:**

**Scenario A: Multiple Backup Plugins**

```
Plugin A: Scheduled for 02:00 daily (active)
Plugin B: Scheduled for 02:00 daily (should disable)
→ Risk: Resource contention, incomplete backups
→ Action: Disable Plugin B's schedule
```

**Scenario B: Hosting + Plugin Overlap**

```
Hosting: Automated backup at 01:00
Plugin: Automated backup at 02:00
→ Decision: Keep both (redundancy) OR disable one
→ If keeping both: Ensure storage handles 2x backups
```

**Scenario C: Legacy Manual Backup Reminders**

```
Calendar Reminder: "Run backup manually - Every Monday"
Automation: Now handles all backups
→ Action: Delete manual backup reminders to prevent confusion
```

**Step 5: Execute Full Automation Cycle**

**7-Day Validation Protocol:**

|Day|Action|Expected Result|Actual Result|Status|
|---|---|---|---|---|
|Day 1|Enable automation|Backup runs at 02:00|[Verify]|[✓/✗]|
|Day 2|No action (hands-off)|Backup runs at 02:00|[Verify]|[✓/✗]|
|Day 3|No action|Backup runs at 02:00|[Verify]|[✓/✗]|
|Day 7|Verify 7 backups exist|7 backups in storage|[Verify]|[✓/✗]|

**Pass Criteria:**

- All 7 days show successful execution
- All backups are complete (per 1.4 verification)
- No manual intervention required for any backup
- Execution time variance <15 minutes from scheduled time

**Fail Criteria:**

- Any missed backup execution
- Any incomplete backup
- Manual intervention required
- Execution time drift >30 minutes

**If failed:** Investigate root cause, remediate, restart 7-day cycle

**Step 6: Document Automation Status**

Record in configuration management:

```
Backup Automation Status: ACTIVE

Site: example.com
Automation Mechanism: UpdraftPlus Plugin Scheduler
Schedule: Daily 02:00 UTC
Last Manual Verification: 2025-01-15
7-Day Validation: PASSED (2025-01-08 to 2025-01-15)
Next Review: 2025-04-15 (90 days)

Configured By: [Name]
Approved By: [Infrastructure Lead]
```

### Failure Prevention Patterns

**Anti-Pattern 1: Trust Without Verification**

- _Failure_: Enabling automation and assuming it will work indefinitely
- _Consequence_: Silent failures for weeks/months; discovered during disaster
- _Mitigation_: 7-day validation cycle; ongoing monitoring (see 1.6)

**Anti-Pattern 2: WP-Cron Dependency on Low-Traffic Sites**

- _Failure_: Relying on WP-Cron for low-traffic sites (<100 visits/day)
- _Consequence_: Backups delayed or skipped due to no visitors to trigger cron
- _Mitigation_: Disable WP-Cron, use system cron for reliable scheduling

**Anti-Pattern 3: Multiple Conflicting Schedules**

- _Failure_: Running multiple backup systems simultaneously without coordination
- _Consequence_: Resource exhaustion, backup corruption, server overload
- _Mitigation_: Single authoritative backup schedule; disable redundant systems

### Training Guidance

**Core Concept:**

> "Automation is not 'set it and forget it'—it's 'set it, verify it, monitor it.' The goal is not to eliminate human involvement, but to eliminate human dependency for routine execution. You should never need to remember to run a backup, but you must remember to verify backups are running."

**New Team Member Checklist:**

```
Before considering automation complete:
[✓] Schedule configured to match policy exactly
[✓] Manual intervention is NOT required
[✓] Conflicting schedules disabled
[✓] 7-day hands-off test completed successfully
[✓] All 7 backups verified as complete
[✓] Automation status documented
[✓] Monitoring enabled (Step 1.6)
```

---

## 1.6 Verify Backup Job Execution

### Purpose

Confirm scheduled backups execute successfully through rapid daily status checks; detect failures immediately.

### Conceptual Foundation

**The Detection Gap:** The time between backup failure and human awareness is the period of unprotected operation. A backup that failed 3 days ago but wasn't noticed until today represents 3 days of unacceptable RPO exposure.

**Monitoring Principle:** Daily verification converts silent failures into immediate signals, enabling rapid response before data loss events occur.

**Effort vs. Value Optimization:** This step prioritizes speed and frequency over depth—quick daily checks (2-3 minutes) rather than deep weekly audits.

### Execution Protocol

**Step 1: Establish Daily Check Routine**

**Timing Options:**

|Timing|Rationale|Best For|
|---|---|---|
|**Morning Start (9:00 AM)**|First task of workday; overnight backups complete|Most teams|
|**Post-Backup Window (+2 hours)**|Immediate verification after scheduled execution|Critical sites|
|**End of Day**|Final check before EOD|Teams in same timezone as backup execution|

**Recommended:** Morning start (9:00 AM) for consistency and workflow integration

**Step 2: Access Backup Status Dashboard**

Navigate to backup monitoring interface:

**Plugin-Based:**

- Log into WordPress admin
- Navigate to backup plugin dashboard
- View "Last Backup" or "Backup History" section

**Hosting-Based:**

- Log into hosting control panel (cPanel, Plesk, etc.)
- Navigate to backup section
- View backup log or status page

**External Service:**

- Log into ManageWP, MainWP, or monitoring service
- View site-specific backup status
- Check for completion indicators

**Step 3: Perform 4-Point Quick Check**

**Checklist (60-90 seconds):**

```
[✓] 1. Latest backup timestamp is within last 24 hours
[✓] 2. Status shows "Completed" or "Success" (not "Failed", "Partial", or "Running")
[✓] 3. Backup size is consistent with previous backups (±20%)
[✓] 4. No error messages or warnings visible
```

**Visual Status Indicators:**

|Indicator|Interpretation|Action|
|---|---|---|
|✅ Green/Success|Backup completed normally|Proceed; no action needed|
|⚠️ Yellow/Warning|Completed with non-critical warnings|Review warnings (Step 1.7)|
|❌ Red/Failed|Backup did not complete|Escalate immediately (Step 1.9)|
|⏸️ Gray/Pending|Backup in progress or scheduled|Check again in 1 hour|

**Step 4: Record Check Completion**

**Simple Tracking Methods:**

**Method A: Checklist (Paper/Digital)**

```
Date       | Status | Time Checked | Initials
-----------|--------|--------------|----------
2025-01-15 | ✓ OK   | 09:05        | JD
2025-01-16 | ✓ OK   | 09:12        | JD
2025-01-17 | ✗ FAIL | 09:08        | JD → Ticket #1234
```

**Method B: Monitoring Dashboard**

- Many tools auto-log checks when viewed
- Verify dashboard shows "Last Viewed" timestamp

**Method C: Automated Logging**

- For multiple sites: Use monitoring aggregation (ManageWP, uptime monitors)
- Receive daily summary email with all site statuses

**Step 5: Escalation Path for Failures**

**If ANY check fails (status ≠ Success):**

```
1. DO NOT mark check as complete
2. CREATE incident ticket immediately
3. NOTIFY Infrastructure Lead if mission-critical site
4. PROCEED to Step 1.9 (Resolve Failed/Skipped Backups)
5. DO NOT resume daily checks until issue resolved
```

**Escalation Template:**

```
TITLE: Backup Failure - [Site Name]
PRIORITY: High

Site: example.com
Last Successful Backup: 2025-01-14 02:00 UTC
Current Status: Failed
Discovered: 2025-01-15 09:05

Initial Observation:
- [Error message from tool, if visible]
- [Last backup timestamp]
- [Any visible warnings]

Action Taken:
- Issue logged
- Infrastructure Lead notified
- Awaiting investigation (Step 1.9)
```

### Failure Prevention Patterns

**Anti-Pattern 1: Checking Only When Remembered**

- _Failure_: Sporadic verification without routine
- _Consequence_: Failures go undetected for days/weeks
- _Mitigation_: Calendar reminder, morning routine integration, checklist accountability

**Anti-Pattern 2: Checking Without Recording**

- _Failure_: Viewing status but not documenting check completion
- _Consequence_: No audit trail; cannot prove due diligence; missed patterns invisible
- _Mitigation_: Simple check log (even paper checklist); automated summary emails

**Anti-Pattern 3: Accepting "Partial" or "Warning" as Acceptable**

- _Failure_: Treating warnings as informational rather than actionable
- _Consequence_: Gradual degradation; incomplete backups normalized
- _Mitigation_: Only "Success" is acceptable; warnings require investigation (Step 1.7)

### Training Guidance

**Core Concept:**

> "This is a signal check, not a deep investigation. You're answering one question: 'Did the backup run successfully in the last 24 hours?' Yes = proceed. No = escalate. This 2-minute daily habit prevents catastrophic failures from hiding in silence."

**New Team Member Daily Routine:**

```
09:00 - Start workday
09:02 - Open backup dashboard
09:04 - Verify last backup: Timestamp + Status
09:05 - Mark check complete OR create incident ticket
09:06 - Proceed to other morning tasks
```

**Total time investment:** 2-4 minutes/day **Value:** Immediate detection of 100% of backup failures

---

## 1.7 Review Backup Logs

### Purpose

Detect silent warnings or partial failures that don't block backup completion but compromise restore reliability.

### Conceptual Foundation

**The Partial Success Problem:** Backup tools often report "success" when the job completes, even if:

- Some files were skipped due to permissions
- Database export included warnings
- Storage upload was partially incomplete
- Timeout occurred but tool continued

**Log Analysis Goal:** Identify early warning signals that precede catastrophic failures.

**Effort Allocation:** This is a weekly task (not daily) requiring 10-15 minutes of focused review.

### Execution Protocol

**Step 1: Access Backup Logs**

**Location varies by tool:**

**Plugin-Based:**

- Navigate to plugin settings → Logs or History
- Download recent log files if available
- Check WordPress debug.log for backup-related entries

**Hosting-Based:**

- Access cPanel → Backup Logs
- Check email notifications (many hosts send log summaries)
- Review system logs if accessible

**Server-Level:**

- SSH access: `cat /var/log/backup.log`
- Review cron execution logs: `grep CRON /var/log/syslog`

**Step 2: Identify Log Review Scope**

**Weekly Review Cadence:**

- Review logs from last 7 days
- Focus on most recent 2-3 backups in detail
- Scan older logs for recurring patterns

**What to Review:**

```
Priority 1: ERROR messages (always investigate)
Priority 2: WARNING messages (investigate if recurring)
Priority 3: Skipped files/directories (assess criticality)
Priority 4: Performance metrics (duration, size)
```

**Step 3: Scan for Critical Indicators**

**Error Keywords to Search:**

```
ERROR | FAIL | FATAL | ABORT | TIMEOUT | PERMISSION DENIED | 
ACCESS DENIED | CANNOT | UNABLE | EXCEPTION | CORRUPT
```

**Warning Keywords to Flag:**

```
WARNING | SKIP | PARTIAL | INCOMPLETE | RETRY | SLOW | 
TIMEOUT | EXCEEDED | LIMIT
```

**Search Method (if logs are text files):**

```bash
# Search for errors in log file
grep -i "error\|fail\|fatal" backup.log

# Search for warnings
grep -i "warning\|skip\|partial" backup.log

# Count occurrences
grep -c "permission denied" backup.log
```

**Step 4: Interpret Common Log Patterns**

**Pattern A: Permission Errors**

```
Log Entry:
"WARNING: Cannot read file /wp-content/uploads/2024/12/image.jpg - Permission denied"

Interpretation:
- File excluded from backup due to filesystem permissions
- Backup marked "success" but is incomplete

Action Required:
- Fix file permissions: chmod 644 file, chmod 755 directory
- Re-run backup to capture previously excluded files
- Verify file now included in next backup
```

**Pattern B: Timeout Warnings**

```
Log Entry:
"WARNING: Backup duration (3h 15m) exceeded recommended window (2h)"

Interpretation:
- Backup completing but taking too long
- Risk of timeout on next run if site grows
- May impact server performance during backup

Action Required:
- Optimize database (remove spam, transients)
- Exclude unnecessary files (cache, logs)
- Increase backup window or split into incremental approach
```

**Pattern C: Skipped Directories**

```
Log Entry:
"INFO: Skipping /wp-content/cache/ (excluded by configuration)"

Interpretation:
- Intentional exclusion (cache files don't need backup)
- Verify exclusion is deliberate, not accidental

Action Required:
- Confirm exclusion aligns with backup policy
- Document why directory is excluded
- No action if intentional
```

**Pattern D: Storage Warnings**

```
Log Entry:
"WARNING: Storage location approaching capacity (85% full)"

Interpretation:
- Backup successful now but future backups may fail
- Storage management needed soon

Action Required:
- Review retention policy; delete old backups if appropriate
- Increase storage allocation
- Implement automated pruning
```

**Step 5: Document Findings**

**Log Review Summary Template:**

```
Backup Log Review - Week of [Date Range]

Site: example.com
Logs Reviewed: 2025-01-08 to 2025-01-15 (7 days, 7 backups)
Reviewed By: [Your Name]
Date: 2025-01-15

Summary:
- Total Backups: 7
- Successful: 7
- Errors: 0
- Warnings: 3 (non-critical)

Findings:
1. [Warning Type]: [Description]
   - Frequency: [X times]
   - Action Taken: [Resolution or "monitoring"]
   - Status: [Resolved / Monitoring / Escalated]

2. [Next finding...]

Recommendation:
[Any policy adjustments, configuration changes, or escalations needed]

Next Review: 2025-01-22
```

**Step 6: Escalation Criteria**

**Escalate immediately if:**

- ANY error messages (ERROR, FAIL, FATAL)
- Repeated warnings (same warning 3+ consecutive backups)
- Skipped critical components (database, uploads)
- Storage capacity <20% remaining
- Backup duration increasing trend (each backup slower than previous)

**Monitor (no immediate action) if:**

- Single informational warning
- Intentional exclusions logged
- Performance metrics stable and within tolerance

### Failure Prevention Patterns

**Anti-Pattern 1: Ignoring Warnings**

- _Failure_: Dismissing warnings as "informational only"
- _Consequence_: Warnings escalate to errors; preventable failures become emergencies
- _Mitigation_: Investigate recurring warnings; trend analysis over time

**Anti-Pattern 2: Log Review Fatigue**

- _Failure_: Logs too verbose; reviewer skims without reading
- _Consequence_: Critical signals missed in noise
- _Mitigation_: Use search keywords; focus on ERROR/WARNING only; automate log parsing if possible

**Anti-Pattern 3: No Follow-Up on Findings**

- _Failure_: Documenting issues but not resolving them
- _Consequence_: Log review becomes performative; issues persist
- _Mitigation_: Every finding requires either immediate action or explicit decision to monitor

### Training Guidance

**Core Concept:**

> "You are not debugging—you are pattern matching. Look for ERROR (always bad), WARNING (investigate), SKIP (verify intentional). If you see clean execution, you're done in 5 minutes. If you see problems, escalate. Your job is detection, not resolution."

**Simple Decision Tree:**

```
See ERROR? → Escalate immediately
See WARNING? → Is it recurring? → Yes: Escalate | No: Monitor
See SKIP? → Is it intentional? → Yes: Document | No: Escalate
See INFO only? → Mark review complete
```

---

## 1.8 Configure Failure Alerting

### Purpose

Enable immediate detection of backup failures through automated notifications, eliminating dependency on manual checking.

### Conceptual Foundation

**Proactive vs. Reactive Monitoring:**

- **Reactive:** Human checks daily; detects failure 12-24 hours after occurrence
- **Proactive:** System alerts immediately; detects failure within minutes

**Alert Design Principle:** Alerts must be actionable, timely, and distinguished by severity to prevent alert fatigue.

### Execution Protocol

**Step 1: Identify Available Alert Channels**

**Built-In Tool Notifications:**

|Tool Type|Alert Capabilities|Configuration Location|
|---|---|---|
|**Plugin (UpdraftPlus)**|Email on failure, success optional|Settings → Email Notifications|
|**Plugin (BackupBuddy)**|Email, SMS (premium)|Destinations → Notifications|
|**Hosting (cPanel)**|Email on cron job failure|Cron Jobs → Email settings|
|**External Service**|Dashboard alerts, email, SMS, Slack|Account → Notifications|

**Third-Party Monitoring:**

- UptimeRobot: HTTP monitoring for backup status endpoints
- Freshping: Similar to UptimeRobot
- Custom: Zapier/IFTTT integrations

**Step 2: Configure Alert Triggers**

**Essential Alert Types:**

|Alert|Trigger Condition|Urgency|Notification Method|
|---|---|---|---|
|**Backup Failed**|Backup job exits with error code|Critical|Email + SMS (if available)|
|**Backup Missed**|Scheduled backup did not execute|High|Email|
|**Backup Incomplete**|Partial backup (missing components)|High|Email|
|**Storage Warning**|Storage capacity <20%|Medium|Email|
|**Duration Warning**|Backup time >2x baseline|Low|Email (daily digest)|

**Configuration Example (UpdraftPlus):**

```
Settings → Email Notifications

[✓] Email me when a backup finishes
Recipient: backups@yourcompany.com

[✓] Email me when a backup fails  
Recipient: critical-alerts@yourcompany.com

[✗] Email me for successful backups (disable to reduce noise)

Alert Subject Line Format:
"[CRITICAL] Backup FAILED - {{site_name}} - {{date}}"
"[HIGH] Backup MISSED - {{site_name}} - {{date}}"
```

**Step 3: Set Up Alert Routing**

**Alert Destination Strategy:**

**For Small Teams (1-5 people):**

```
Critical Alerts → Email to team@ + SMS to on-call person
High Alerts → Email to team@
Medium/Low Alerts → Email to operations@
```

**For Larger Organizations:**

```
Critical Alerts → PagerDuty/Opsgenie → On-call rotation
High Alerts → Ticketing system (Jira, ServiceNow)
Medium Alerts → Dashboard + weekly digest email
Low Alerts → Dashboard only
```

**Email Alert Best Practices:**

1. **Distinct subject lines** by severity:
    
    - `[CRITICAL]` for failures requiring immediate action
    - `[WARNING]` for issues requiring investigation
    - `[INFO]` for successful completions (if enabled)
2. **Actionable content**:
    
    ```
    Subject: [CRITICAL] Backup FAILED - example.com - 2025-01-15
    
    Site: example.com
    Backup Type: Daily Full Backup
    Scheduled Time: 02:00 UTC
    Failure Time: 02:34 UTC
    Error: "Database connection timeout after 300 seconds"
    
    Action Required:
    1. Check database server status
    2. Verify database credentials in wp-config.php
    3. Re-run backup manually once resolved
    4. Escalate to Infrastructure Lead if unresolved within 2 hours
    
    Dashboard: https://backups.example.com/site/123
    Troubleshooting Guide: https://docs.internal/backup-failures
    ```
    
3. **Include recovery information**:
    
    - Link to backup dashboard
    - Link to troubleshooting documentation
    - Escalation contact information

**Step 4: Test Alert Delivery**

**Test Method A: Simulate Failure (if tool supports)**

Some backup tools allow triggering test alerts:

```
Settings → Notifications → Send Test Alert
```

**Test Method B: Intentional Failure**

1. Temporarily misconfigure backup (e.g., incorrect storage path)
2. Wait for next scheduled run
3. Verify alert is received within expected timeframe
4. Fix configuration
5. Verify success notification (if enabled)

**Test Method C: Manual Trigger**

For server-level backups:

```bash
# Create script that deliberately fails
#!/bin/bash
echo "Test backup failure" | mail -s "[TEST] Backup Failed" alerts@example.com
exit 1
```

**Step 5: Verify Alert Receipt**

**Confirmation Checklist:**

```
[✓] Alert received in expected inbox
[✓] Subject line clearly indicates severity
[✓] Alert arrived within 5 minutes of test trigger
[✓] Alert content includes actionable information
[✓] Alert formatting is readable (not garbled)
[✓] Alert links are functional
```

**If ANY check fails:**

- Troubleshoot email delivery (check spam folders)
- Verify email addresses are correct
- Test alternative notification methods (SMS, Slack)
- Confirm firewall/network allows outbound email

**Step 6: Establish Alert Response SLA**

**Define response time expectations:**

|Alert Severity|Acknowledgment Time|Resolution Time|Escalation Path|
|---|---|---|---|
|**Critical**|15 minutes|1 hour|Immediate escalation to on-call|
|**High**|1 hour|4 hours|Escalate if unresolved after 2 hours|
|**Medium**|4 hours|24 hours|Weekly review if recurring|
|**Low**|24 hours|Next maintenance window|Monthly review|

**Document in runbook:**

```
When you receive a [CRITICAL] backup failure alert:
1. Acknowledge within 15 minutes (reply to alert or update ticket)
2. Begin investigation immediately
3. If cannot resolve within 30 minutes, escalate to [Name]
4. Do not mark resolved until backup successfully re-run
5. Document root cause and prevention steps
```

### Failure Prevention Patterns

**Anti-Pattern 1: Alert Fatigue**

- _Failure_: Too many low-severity alerts; team stops reading
- _Consequence_: Critical alerts ignored; buried in noise
- _Mitigation_: Only alert on failures and warnings; disable success notifications unless required

**Anti-Pattern 2: Unactionable Alerts**

- _Failure_: Alerts say "backup failed" without error details
- _Consequence_: Responder must login and investigate; slows response
- _Mitigation_: Alerts must include error message, affected site, timestamp, and next steps

**Anti-Pattern 3: Single Point of Failure in Alerting**

- _Failure_: All alerts go to one person's email
- _Consequence_: Alerts missed if person on vacation, email down, or left company
- _Mitigation_: Alerts to team distribution lists; backup SMS/phone for critical alerts

### Training Guidance

**Core Concept:**

> "A backup failure you don't hear about does not exist operationally. Alerting converts silent failures into immediate signals. But alerts are only valuable if they are timely, actionable, and responded to. Configure alerts, test them, and establish clear response protocols before the first failure occurs."

**New Team Member Onboarding:**

```
Before going live with alerts:
[✓] Understand what each alert type means
[✓] Know who to escalate to for each severity
[✓] Test that you actually receive alerts (don't assume)
[✓] Add alert email addresses to safe sender list
[✓] Document your response SLA commitments
[✓] Verify you have access to troubleshooting docs
```

---

## 1.9 Resolve Failed or Skipped Backups

### Purpose

Restore backup reliability by immediately investigating and remediating any failed or skipped backup execution.

### Conceptual Foundation

**The Compounding Failure Risk:** Every day without a successful backup extends the RPO window. A backup that fails on Day 1 but isn't fixed until Day 3 creates a 72-hour recovery gap—triple the intended 24-hour RPO.

**Resolution Priority:** Failed backups are P1 (high priority) incidents requiring same-day resolution, not "monitor and see if it fixes itself."

### Common Failure Patterns & Resolutions

#### Failure Pattern A: Storage Capacity Exceeded

**Symptoms:**

```
Log: "ERROR: Insufficient storage space"
Log: "WARNING: Storage location 98% full"
Status: Backup failed at 45% completion
```

**Root Cause:** Retention policy not pruning old backups; storage quota too small

**Resolution Steps:**

1. **Immediate:** Manually delete oldest backups to free space
    
    ```bash
    # Identify old backups
    ls -lht /backups/ | tail -20
    
    # Delete backups older than 60 days
    find /backups/ -name "*.tar.gz" -mtime +60 -delete
    ```
    
2. **Short-term:** Run backup manually to verify success with freed space
    
3. **Long-term:**
    
    - Implement automated retention policy
    - Increase storage allocation
    - Compress backups more aggressively
    - Move archives to cheaper long-term storage

**Verification:** Monitor storage utilization; ensure <80% after resolution

---

#### Failure Pattern B: Database Connection Timeout

**Symptoms:**

```
Log: "ERROR: Database connection timeout after 300 seconds"
Log: "SQLSTATE[HY000] [2002] Connection timed out"
Status: Backup failed during database export
```

**Root Cause:** Database server overloaded; network connectivity issues; incorrect credentials

**Resolution Steps:**

1. **Verify database accessibility:**
    
    ```bash
    mysql -u wpuser -p -h localhost
    # If connection fails, database is down or credentials wrong
    ```
    
2. **Check database server load:**
    
    ```sql
    SHOW PROCESSLIST;
    # Look for long-running queries blocking backup
    ```
    
3. **Increase timeout settings:**
    
    ```
    Backup Tool Settings → Database Export Timeout: 300 → 900 seconds
    ```
    
4. **Optimize database before next backup:**
    
    ```sql
    -- Remove spam comments
    DELETE FROM wp_comments WHERE comment_approved = 'spam';
    
    -- Delete transients
    DELETE FROM wp_options WHERE option_name LIKE '_transient_%';
    
    -- Optimize tables
    OPTIMIZE TABLE wp_posts, wp_options, wp_comments;
    ```
    
5. **Re-run backup** and verify success
    

**Verification:** Database export completes within timeout; backup succeeds

---

#### Failure Pattern C: File Permission Errors

**Symptoms:**

```
Log: "WARNING: Cannot read /wp-content/uploads/2024/12/file.jpg - Permission denied"
Log: "ERROR: 247 files skipped due to permission errors"
Status: Backup completed but marked partial
```

**Root Cause:** Incorrect file ownership or permissions after file upload, migration, or manual changes

**Resolution Steps:**

1. **Identify affected files:**
    
    ```bash
    # Find files not readable by backup user
    find /var/www/html -type f ! -perm -444
    ```
    
2. **Fix permissions (WordPress standard):**
    
    ```bash
    # Fix ownership (assuming www-data user)
    chown -R www-data:www-data /var/www/html/wp-content/
    
    # Fix permissions
    find /var/www/html/ -type d -exec chmod 755 {} \;
    find /var/www/html/ -type f -exec chmod 644 {} \;
    ```
    
3. **Verify backup user can access files:**
    
    ```bash
    sudo -u www-data ls /wp-content/uploads/2024/12/
    # Should list files without errors
    ```
    
4. **Re-run backup** and verify no permission warnings
    

**Verification:** Log shows zero skipped files; backup size matches expected

---

#### Failure Pattern D: Backup Process Killed (Timeout/Resource Limits)

**Symptoms:**

```
Log: "Backup started at 02:00:15"
Log: [no completion message]
Status: Backup shows "Running" but never completes
```

**Root Cause:** PHP timeout, server resource limits (memory, CPU), or process killed by hosting provider

**Resolution Steps:**

1. **Check server resource limits:**
    
    ```php
    <?php
    echo "Max execution time: " . ini_get('max_execution_time') . " seconds\n";
    echo "Memory limit: " . ini_get('memory_limit') . "\n";
    ?>
    ```
    
2. **Increase PHP limits (in wp-config.php):**
    
    ```php
    ini_set('max_execution_time', 3600); // 1 hour
    ini_set('memory_limit', '512M');
    ```
    
3. **Check if hosting provider killed process:**
    
    - Review hosting error logs
    - Contact support if process killed by automated systems
    - Consider upgrading hosting tier for more resources
4. **Split large backups:**
    
    - Configure incremental backups between full backups
    - Break backup into components (DB separate from files)
    - Schedule components at different times
5. **Re-run with increased limits** and monitor completion
    

**Verification:** Backup completes within timeout; process not killed

---

#### Failure Pattern E: Network/Storage Upload Failure

**Symptoms:**

```
Log: "Backup completed locally"
Log: "ERROR: Failed to upload to remote storage - Connection reset"
Status: Backup exists locally but not in remote storage
```

**Root Cause:** Network connectivity issues; storage service downtime; incorrect credentials; firewall blocking

**Resolution Steps:**

1. **Test remote storage connectivity:**
    
    ```bash
    # For S3
    aws s3 ls s3://your-backup-bucket/
    
    # For FTP
    ftp backup-server.example.com
    ```
    
2. **Verify storage credentials:**
    
    - Re-enter API keys/passwords
    - Check for expired tokens
    - Confirm storage account is active
3. **Check storage service status:**
    
    - Visit provider status page (status.aws.amazon.com, etc.)
    - If provider outage, wait and retry later
4. **Manual upload test:**
    
    - Upload backup file manually
    - If succeeds, issue is with automation
    - If fails, issue is with credentials or network
5. **Re-configure remote storage** settings in backup tool
    
6. **Retry upload** for local backup
    

**Verification:** Backup successfully uploaded to all configured remote locations

---

### General Resolution Protocol

**For ANY Failed Backup:**

**Step 1: Triage (5 minutes)**

- Review error message in backup log
- Categorize failure type (storage, database, permissions, timeout, network)
- Assess urgency based on days since last successful backup

**Step 2: Immediate Mitigation (15-30 minutes)**

- Apply quickest fix for identified failure pattern
- Do NOT attempt comprehensive optimization yet
- Goal: Get ONE successful backup completed today

**Step 3: Manual Backup Verification (10 minutes)**

- Trigger manual backup execution
- Monitor in real-time until completion
- Verify backup artifact created and complete
- Confirm no errors or warnings in log

**Step 4: Root Cause Analysis (30-60 minutes)**

- Investigate why failure occurred
- Document trigger event (if identifiable)
- Identify preventive measures
- Implement longer-term fix if immediate mitigation was temporary

**Step 5: Automation Restoration (10 minutes)**

- Confirm automated schedule is still active
- Verify next scheduled backup time
- Monitor next automated execution for success
- Document resolution in incident ticket

**Step 6: Follow-Up (Next 7 days)**

- Monitor next 7 automated backups
- Ensure failure does not recur
- Update documentation if new failure pattern discovered

### Escalation Criteria

**Escalate to Infrastructure Lead if:**

- Cannot identify root cause within 30 minutes
- Fix requires infrastructure changes (server config, hosting upgrade)
- Failure recurs after attempted resolution
- Multiple sites experiencing same failure (systemic issue)
- Days since last successful backup >2 (RPO breach imminent)

**Escalation Template:**

```
SUBJECT: Backup Failure Escalation - [Site Name] - [Date]

Site: example.com
Failure Type: [Database Timeout / Storage / etc.]
Days Since Last Successful Backup: 2 days
RPO Status: BREACH RISK

Resolution Attempts:
1. [What was tried]
2. [Result of attempt]
3. [Why escalating]

Impact:
- Current RPO window: 48 hours (target: 24 hours)
- Next failure window closes: [Date]
- Business risk level: [High / Medium / Low]

Assistance Needed:
- [Specific help required]
- [Resources needed]
- [Decision required]

Ticket: #[ticket number]
Reported By: [Your Name]
```

### Training Guidance

**Core Concept:**

> "Do not 'watch and wait.' A failed backup is an active incident requiring same-day resolution. Your priority order: (1) Get one successful backup today, (2) Understand why it failed, (3) Prevent future failures. Speed matters—every hour without a backup increases risk."

**Resolution Decision Tree:**

```
Backup Failed?
├─ Can I fix in <30 minutes? 
│  ├─ YES → Fix, re-run, verify, document
│  └─ NO → Escalate immediately
│
└─ Fix successful?
   ├─ YES → Monitor next 7 backups, close ticket
   └─ NO → Escalate immediately
```

---

## 1.10 Record Backup Frequency Policy

### Purpose

Preserve governance and auditability by documenting the complete backup frequency configuration, rationale, and approval chain.

### Conceptual Foundation

**Documentation as Institutional Memory:** Undocumented systems fail when:

- Staff turnover occurs (knowledge walks out the door)
- Audits require evidence of compliance
- Incidents require historical reference
- Scaling requires replication across multiple sites

**Policy Documentation = Executable Specification:** Document should enable any qualified person to validate compliance or replicate configuration.

### Documentation Structure

**Required Documentation Components:**

|Component|Purpose|Storage Location|
|---|---|---|
|**Policy Statement**|Defines schedule and rationale|Policy repository|
|**Configuration Details**|Technical implementation specifics|Configuration management database|
|**Approval Record**|Governance and authority trail|Approval tracking system|
|**Review Schedule**|Ensures periodic validation|Calendar/ticketing system|

### Execution Protocol

**Step 1: Complete Policy Template**

**Backup Frequency Policy Template:**

```
===========================================
BACKUP FREQUENCY POLICY
===========================================

Site Information:
- Site Name: [example.com]
- Site Classification: [Mission-Critical / Business-Critical / Standard]
- Business Owner: [Name, Title]
- Technical Owner: [Name, Title]

Recovery Objectives:
- Recovery Point Objective (RPO): [4 hours / 12 hours / 24 hours]
- Recovery Time Objective (RTO): [1 hour / 4 hours / 24 hours]
- Business Justification: [E.g., "E-commerce site processing $50K/month; 
  24-hour data loss = $1,667 revenue impact + customer trust damage"]

Backup Schedule:
- Daily Full Backup Frequency: [Daily / 2x daily / 4x daily]
- Daily Backup Time: [02:00 UTC]
- Weekly Archive Day: [Sunday]
- Weekly Archive Time: [02:00 UTC]
- Monthly Archive Date: [1st of month]

Backup Scope:
- WordPress Database: [✓ Included]
- Core WordPress Files: [✓ Included]
- Themes (all): [✓ Included]
- Plugins (all): [✓ Included]
- Uploads/Media: [✓ Included]
- Configuration Files: [✓ Included]
- Custom Directories: [Specify if any]

Automation Configuration:
- Backup Tool: [UpdraftPlus Premium / BlogVault / Custom]
- Automation Mechanism: [Plugin Scheduler / WP-Cron / System Cron]
- Backup Window: [Maximum 2 hours]
- Timeout Setting: [900 seconds]

Storage Configuration:
- Primary Storage: [AWS S3 us-east-1]
- Secondary Storage: [Backblaze B2 us-west]
- Archive Storage: [AWS Glacier Deep Archive]

Retention Policy:
- Daily Backups: Retain [30 days]
- Weekly Archives: Retain [12 weeks]
- Monthly Archives: Retain [12 months]
- Pre-Update Snapshots: Retain [30 days]

Monitoring & Alerting:
- Daily Execution Verification: [✓ Enabled]
- Weekly Log Review: [✓ Enabled]
- Failure Alerting: [✓ Email to team@example.com]
- Alert Response SLA: [15 minutes acknowledgment, 1 hour resolution]

Restore Testing:
- Test Frequency: [Monthly]
- Last Test Date: [2025-01-10]
- Test Result: [Success - RTO 42 minutes]
- Next Test Date: [2025-02-10]

Policy Approval:
- Approved By: [Name, Title]
- Approval Date: [2025-01-15]
- Authority: [Infrastructure Lead / CTO]
- Digital Signature: [Reference to approval system]

Policy Review:
- Review Frequency: [Quarterly - every 90 days]
- Next Review Date: [2025-04-15]
- Review Owner: [Name]

Change History:
- Version 1.0 | 2025-01-15 | Initial policy | [Author Name]
- [Future changes logged here]

===========================================
```

**Step 2: Store in Policy Repository**

**Repository Options:**

|Repository Type|Best For|Access Control|
|---|---|---|
|**Confluence/Wiki**|Teams with existing wiki|Role-based access|
|**Git Repository**|Technical teams|Version-controlled|
|**SharePoint/Google Drive**|Business teams|Folder permissions|
|**Dedicated CMDB**|Enterprise environments|Integrated with other config|

**File Naming Convention:**

```
backup-policy-[site-identifier]-[version].md

Example:
backup-policy-example-com-v1.0.md
```

**Folder Structure:**

```
/policies/
├── /backup-policies/
│   ├── backup-policy-example-com-v1.0.md
│   ├── backup-policy-client-site-v1.0.md
│   └── backup-policy-template-v2.0.md
├── /retention-policies/
└── /disaster-recovery/
```

**Step 3: Link to Configuration Management**

**CMDB Entry (if applicable):**

```
Configuration Item: example.com Backup System
Type: Backup Service
Status: Active
Owner: [Infrastructure Team]

Related Policies:
- Backup Frequency Policy: [Link to policy document]
- Data Retention Policy: [Link]
- Disaster Recovery Plan: [Link]

Configuration References:
- Backup Tool: UpdraftPlus Premium (License #12345)
- Storage Accounts: AWS S3 (backup-bucket-prod), Backblaze (account-id-6789)
- Monitoring: ManageWP (Site ID: 456)

Dependencies:
- WordPress Site: example.com (CI #789)
- Database Server: db-prod-01 (CI #234)
- Storage Services: AWS Account (CI #567)

Last Updated: 2025-01-15
Next Review: 2025-04-15
```

**Step 4: Establish Review Cadence**

**Create Recurring Calendar Event:**

```
Title: Quarterly Backup Policy Review - example.com
Frequency: Every 90 days
Attendees: [Infrastructure Lead, Site Owner]
Duration: 30 minutes

Agenda:
1. Verify backup schedule still meets RPO/RTO requirements (5 min)
2. Review retention policy adequacy (5 min)
3. Assess storage costs vs. business value (5 min)
4. Review incident history since last review (5 min)
5. Update policy if needed (5 min)
6. Document review completion (5 min)

Pre-Work:
- Pull backup success rate metrics (last 90 days)
- Calculate storage costs (last 90 days)
- Review any backup-related incidents
- Check for business changes affecting backup requirements
```

**OR Create Ticketing System Reminder:**

```
Ticket: Backup Policy Review - example.com
Type: Scheduled Maintenance
Priority: Medium
Assigned: [Infrastructure Lead]
Due Date: [Current Date + 90 days]
Recurrence: Every 90 days

Description:
Conduct quarterly review of backup policy per governance requirements.

Tasks:
[✓] Review policy document for accuracy
[✓] Verify configuration matches policy
[✓] Assess if RPO/RTO still appropriate
[✓] Check compliance with backup/restore testing
[✓] Update policy version if changes made
[✓] Document review completion
```

**Step 5: Communicate Policy**

**Internal Communication:**

```
TO: [Site stakeholders, operations team]
SUBJECT: Backup Policy Established - example.com

Team,

The backup frequency policy for example.com has been finalized and approved.

Key Details:
- Daily backups at 02:00 UTC
- Weekly archives every Sunday
- 30-day retention for daily backups
- Automated monitoring and alerting enabled

Policy Document: [Link to policy]

What This Means For You:
- Site is protected with RPO of 24 hours
- Recovery capability tested monthly
- Backup failures will be alerted and resolved same-day

Questions? Contact [Infrastructure Lead Name]

---
[Your Name]
[Date]
```

**Step 6: Archive Approval Evidence**

**Approval Documentation:**

- Email approval from authority figure
- Digital signature in policy system
- Meeting minutes documenting approval
- Approval workflow system record

**Retention:** Maintain approval evidence for audit purposes (typically 3-7 years depending on industry regulations)

### Training Guidance

**Core Concept:**

> "Policy without documentation is folklore—it exists only in people's heads and disappears when they leave. Documentation without review becomes obsolete and misleading. This step closes the loop: document what you built, why you built it, who approved it, and when to review it again."

**New Team Member Checklist:**

```
Before considering Step 1 complete:
[✓] Policy document created from template
[✓] All required fields populated
[✓] Technical configuration details accurate
[✓] Approval obtained and documented
[✓] Policy stored in official repository
[✓] CMDB updated (if applicable)
[✓] Review calendar event created
[✓] Stakeholders notified
[✓] Policy linked in site documentation
```

---

## Step 1 Summary: Automated Backup Frequency

### Completion Criteria

Step 1 is considered **complete** when ALL of the following are verified:

```
[✓] 1.1  Backup schedule defined and approved
[✓] 1.2  Daily full backups configured and tested
[✓] 1.3  Weekly archive snapshots configured and isolated
[✓] 1.4  Component completeness verified (all critical components included)
[✓] 1.5  Automation enabled and 7-day cycle successful
[✓] 1.6  Daily execution verification routine established
[✓] 1.7  Weekly log review process implemented
[✓] 1.8  Failure alerting configured and tested
[✓] 1.9  Resolution protocol documented and understood
[✓] 1.10 Policy documented, approved, and stored
```

### Key Outcomes Achieved

When Step 1 is properly executed:

**Operational Outcomes:**

- Recovery points exist at guaranteed intervals (RPO ≤24 hours)
- Backups execute without human dependency (fully automated)
- Failures are detected immediately (alerting + monitoring)
- Backup validity is continuously verified (completeness checks)

**Governance Outcomes:**

- Backup approach is documented and auditable
- Approval chain is established and recorded
- Review cadence ensures ongoing relevance
- Incident response is defined and repeatable

**Risk Reduction:**

- Maximum data loss bounded by RPO (24 hours typical)
- Backup gaps detected within hours (not days/weeks)
- Silent failures eliminated through verification
- Recovery capability assured through completeness validation

### Common Pitfalls Avoided

By completing all 10 sub-steps, you have avoided:

❌ **Automation without verification** → Replaced with daily checks and alerting  
❌ **Partial backups accepted** → Replaced with explicit completeness validation  
❌ **Silent failures** → Replaced with immediate failure alerting  
❌ **Undocumented systems** → Replaced with formal policy documentation  
❌ **One-time setup forgotten** → Replaced with quarterly review cadence

### Next Steps

With Step 1 complete, proceed to:

**Step 2: Storage in Multiple Locations** (3-2-1 backup rule implementation)  
**Step 3: Version Retention Strategy** (Retention policy and pruning automation)  
**Step 4: Restore Testing Schedule** (Validation that backups are actually restorable)

---

**Step 1 Status: COMPLETE** ✅

_This framework has successfully operationalized backup frequency from concept to auditable, repeatable system._