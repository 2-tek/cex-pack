# Auto Commit & Push Code Guideline

> **Severity: GUIDELINE**

1. Upon passing validation tests (`cexr run tests/cex_pack.test.cex`), code updates and governance rules should be staged and committed to git with clean, descriptive commit messages.
2. Auto push code on done: push to `.git` remote (`git push origin main`), try push `.cvm`, and if push cannot be completed, break step by commit only. See `RULE_AUTO_PUSH_ON_DONE.md`.
