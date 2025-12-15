
| Step                                         | Action                                                                                |
| -------------------------------------------- | ------------------------------------------------------------------------------------- |
| **1. [[Full backup before updating]]**       | Create and verify on-demand full backup (files + DB), confirm restore & off-site copy |
| **2. [[Review changelogs & compatibility]]** | Check breaking changes, PHP support, and known issues                                 |
| **3. [[Apply updates in staging]]**          | Apply updates in sandbox environment only                                             |
| **4. [[QA testing]]**                        | Validate UI, forms, logs, and performance                                             |
| **5. [[Conflict resolution]]**               | Roll back or pin compatible versions                                                  |
| **6. [[Deploy safely to production]]**       | Deploy only verified, stable changes                                                  |
| **7. [[Final post-deployment testing]]**     | Confirm live-site stability and functionality                                         |

---
