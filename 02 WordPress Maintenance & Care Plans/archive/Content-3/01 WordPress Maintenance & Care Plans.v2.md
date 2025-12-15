
> [!quote] **Lord Krishna** (Spiritual Teacher, Mathura, c. 3228 BCE)  
> **तस्मादसक्तः सततं कार्यं कर्म समाचर ।  
> असक्तो ह्याचरन्कर्म परमाप्नोति पूरुषः ॥**  
> _Bhagavad Gita — Chapter 3, Verse 19_
> 
> **“Therefore, perform your duty without attachment; for by performing action without attachment, one attains the Supreme.”**
## **How to Approach This Service (Operating Principle)**

WordPress Maintenance & Care Plans must be executed as a **discipline-driven operational practice**, not as reactive firefighting. The approach prioritizes **process correctness, verification at every stage, and detachment from outcomes until execution is complete**.

### **Operational Approach**

1. **Process First, Tools Second**  
    Every action follows a defined sequence (backup → change → verify → document). Tools support the process; they do not replace it.
    
2. **Prevent Before You Repair**  
    Monitoring, testing, and early detection are valued higher than emergency fixes. The objective is _risk avoidance_, not heroics.
    
3. **One Change at a Time**  
    Updates, fixes, and optimizations are applied in controlled order to preserve observability and rollback clarity.
    
4. **Verification Is Mandatory**  
    No task is considered complete without functional checks, logs, and confirmation of site stability.
    
5. **Documentation Is Part of Delivery**  
    If an action is not logged or reported, it is treated as not performed. Transparency is non-negotiable.
    

### **WordPress Maintenance — Dependency-Ordered Execution Table**

| **Deliverable**                             | **Step-by-Step Scope & Execution**                                                                                                                                                                                                                                                                   |
| ------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **[[Automated Full Backups]]**              | [[Verify backup schedule]] · [[Trigger on-demand backup]] · [[Confirm files + DB included]] · [[Verify integrity]] · [[Confirm restore path]] · [[Log backup status]]                                                                                                                                |
| **[[Staging Updates (Optional)]]**          | [[Sync staging with production]] · [[Apply updates on staging]] · [[Run functional tests]] · [[Validate layout & UX]] · [[Approve deployment]]                                                                                                                                                       |
| **[[@Core, Theme & Plugin Updates]]**       | [[Verify backup gate]] · [[Review changelogs]] · [[Confirm compatibility]] · [[Prepare update environment]] · [[Update plugins]] · [[Update themes]] · [[Update core]] · [[Monitor errors & logs]] · [[Test site functionality]] · [[Validate layout]] · [[Clear cache & CDN]] · [[Confirm success]] |
| **[[Security Monitoring & Malware Scans]]** | [[Monitor security alerts]] · [[Run scheduled malware scans]] · [[Detect threats]] · [[Clean infections]] · [[Apply hardening if required]] · [[Log security incident]]                                                                                                                              |
| **[[Uptime Monitoring & Response]]**        | [[Monitor uptime]] · [[Receive downtime alert]] · [[Diagnose root cause]] · [[Restore service]] · [[Confirm stability]] · [[Record incident]]                                                                                                                                                        |
| **[[Performance Monitoring & Checks]]**     | [[Monitor Core Web Vitals]] · [[Identify bottlenecks]] · [[Apply safe optimizations]] · [[Re-test performance]] · [[Record results]]                                                                                                                                                                 |
| **[[Database Cleanup & Optimization]]**     | [[Verify backup availability]] · [[Remove revisions, transients & junk]] · [[Optimize tables]] · [[Verify site functionality]] · [[Log cleanup]]                                                                                                                                                     |
| **[[Broken Link Scans & Fixes]]**           | [[Scan internal & external links]] · [[Validate affected pages]] · [[Fix or replace links]] · [[Re-scan to confirm]] · [[Log fixes]]                                                                                                                                                                 |
| **[[Monthly Small Fixes]]**                 | [[Receive request]] · [[Validate scope & limits]] · [[Apply fix]] · [[Test change]] · [[Close task]]                                                                                                                                                                                                 |
| **[[Priority Support & Ticketing]]**        | [[Receive ticket]] · [[Assess severity]] · [[Resolve issue]] · [[Escalate if required]] · [[Notify client]]                                                                                                                                                                                          |
| **[[Monthly Maintenance Report]]**          | [[Collect activity data]] · [[Summarize updates & incidents]] · [[Highlight risks & recommendations]] · [[Prepare report]] · [[Send to client]]                                                                                                                                                      |
