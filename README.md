# AI-Assisted CI/CD Pipeline Failure Analysis 
In this practical, you will create a small Python application, write automated tests, upload the project to GitHub,
and configure GitHub Actions to run the tests automatically whenever code is pushed. You will then deliberately
introduce a bug, observe the failed pipeline, use an AI assistant to understand the failure, correct the bug, and
verify that the pipeline becomes successful.

## CMD:

```bash
 py app.py
```
```bash
- py -m unittest -v
```
```bash
git init
git add .
git commit -m ""
git branch -M main
git remote add origin https://github.com/RISHABH12005/AI_CI_Practical.git
git push -u origin main
```
```bash
git push
```

## AI to Analyse the Failure  

```text
test_calculate_discount (test_app.TestApplication.test_calculate_discount) ... ok
test_calculate_total (test_app.TestApplication.test_calculate_total) ... FAIL

======================================================================
FAIL: test_calculate_total (test_app.TestApplication.test_calculate_total)
----------------------------------------------------------------------
Traceback (most recent call last):
  File "/home/runner/work/AI_CI_Practical/AI_CI_Practical/test_app.py", line 6, in test_calculate_total
    self.assertEqual(calculate_total(100, 2), 200)
AssertionError: 102 != 200

----------------------------------------------------------------------
Ran 2 tests in 0.000s

FAILED (failures=1)
Error: Process completed with exit code 1.
```

1. **Failed test:** `test_calculate_total` in `TestApplication`. The `test_calculate_discount` test passed.

2. **Expected and actual values:** The test expected `200` but `calculate_total(100, 2)` returned `102`.

3. **Error explanation:** The assertion in `test_app.py` compares the result of `calculate_total(100, 2)` with `200`. The `AssertionError: 102 != 200` means the returned value did not satisfy the test.

4. **Likely incorrect line:** In `app.py`, the `calculate_total` function currently uses:

   ```python
   return price + quantity
   ```

5. **Smallest correction:** Change the addition operator to multiplication:

   ```python
   return price * quantity
   ```

   This directly produces `100 * 2 = 200` without changing the rest of the application.

6. **Local verification:**

   ```bash
   py app.py
   py -m unittest -v
   ```

   The unittest output should show `Ran 2 tests` and `OK`.

   **GitHub Actions verification:** Commit and push the corrected `app.py`. Open the repository's **Actions** tab and confirm that the workflow triggered by the push completes successfully and runs both tests.

**Facts from the output:** One test passed, `test_calculate_total` failed, and the assertion received `102` instead of `200`.

**Possible explanation:** The output supports an incorrect calculation in `calculate_total`; source review is needed to confirm the exact line.

## Submission Checklist

- ✓ Python, VS Code, and Git installed.
- ✓ `app.py` created and executed successfully.
- ✓ `test_app.py` created and both tests passed locally.
- ✓ GitHub repository created and project pushed.
- ✓ `.github/workflows/ci.yml` created and pushed.
- ✓ Successful GitHub Actions run observed.
- ✓ Intentional bug introduced and failed test observed.
- ✓ AI prompt used to analyse the failure.
- ✓ Bug corrected and successful GitHub Actions run observed.

## Questions for Viva / Reflection

1. **What is Continuous Integration?**  
   Continuous Integration is the practice of frequently merging code into a shared repository and automatically building and testing each change.

2. **What is the purpose of a unit test?**  
   A unit test checks one small piece of code independently, such as a function or method, to verify that it produces the expected result.

3. **What is the role of Git in this practical?**  
   Git tracks source-file changes, records commits, manages history, and pushes the project to GitHub.

4. **What is the difference between Git and GitHub?**  
   Git is the local or distributed version-control system. GitHub is an online hosting platform for Git repositories that also provides collaboration and automation tools.

5. **What event triggers the workflow in this practical?**  
   Pushing commits or creating a pull request triggers the workflow, when configured with both `push` and `pull_request` events.

6. **What does the GitHub Actions runner do?**  
   The runner is a hosted execution machine that checks out the repository, installs Python, and runs the commands in the workflow.

7. **Why did the test fail when multiplication was changed to addition?**  
   The test expects a price and quantity to be multiplied. With `100 + 2`, the function returns `102`, while the test expects `200`.

8. **What information in the test output helped identify the problem?**  
   The failed test name, the assertion call, and `AssertionError: 102 != 200` identify the function and show the actual and expected values.

9. **How did the AI assistant help? Why should its suggestion still be verified?**  
   The AI explains the likely cause and suggests a focused correction. The suggestion must still be checked against the source code and tested locally and in GitHub Actions because AI output can be incomplete or wrong.

10. **Does this practical deploy the application to a live server? Explain your answer.**  
    No. It demonstrates continuous integration because the workflow checks out and tests the code. Deployment would require separate deployment steps and infrastructure.
