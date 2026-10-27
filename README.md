# AI Usage Declaration and Audit

*Group:* Edamame  
*Members:* 
- Lwin Oo (Edward) ( 6705140051) 
- Kaung Myat Tun (Kingston) ( 6705140059)
- Ngyein Chan Ko (Reka ) ( 6705140061)
- Han Htoo Aung (Erics ) (6705140067)
- Wathan Htat (Frost ) ( 6705140052) 

*Part D3 of the midterm — 20 marks.* Part 1 is required but carries no marks;
its absence is an academic-honesty matter. Part 2 is worth 16 (2 per test).
Parts 3 and 4 together are worth 4.

Before you submit, check all eight boxes:

- [ ] Part 1 declaration complete, or the "no AI used" statement written
- [ ] All eight tests appear in the summary table with a verdict
- [ ] All eight have *two* pasted evidence runs (shipped code, and your fixed code)
- [ ] Part 3 coverage table filled in against FINDINGS.md
- [ ] Part 4 verdict written, under 400 words
- [ ] Every audit entry names the member who did it
- [ ] No claim made that you cannot demonstrate from pasted output
- [ ] File committed as AI_USAGE.md in the repository root

---

## Part 1 — AI usage declaration (required, unmarked)

Every AI tool any member used anywhere in this project. Using AI is allowed.
Not declaring it is not.

| Tool | What we used it for | Where in the project | How we checked it before relying on it |
|---|---|---|---|
| Codex | Helped plan catalog smoke and regression tests, suggested the large-catalog slow test, reviewed the search fix and CI workflow, and helped draft the finding and both catalog AI audits. | tests/test_bookstore.py, bookstore_app/catalog.py, .github/workflows/tests.yml, FINDINGS.md, AI_USAGE.md | Used the suggested test and fix code after review, and edited the documentation wording. Verified with pytest that the regression test fails on the original code and passes after the fix. Ran all five catalog tests, measured the slow test's duration, and checked both supplied catalog tests on the original and fixed versions. |
| | | | |

Slow-test verification command: python3 -m pytest tests/test_bookstore.py -v --durations=5

text
2.07s call     tests/test_bookstore.py::test_search_with_large_catalog
============================== 5 passed in 2.09s ===============================

If you kept prompts worth showing, add them in an appendix at the end of this
file. Do not paste whole conversations.

---

## Part 2 — Audit of the supplied test set (16 marks)



### Summary table — fill this in first

| # | Test | What it claims to catch | Shipped | Fixed | Verdict | Auditor |
|---|---|---|---|---|---|---|
| 1 | test_cart_total_sums_items | Checks that the cart total includes all items. | FAIL | PASS | Genuine detection | Han Htoo Aung — Erics12487254 |
| 2 | test_login_rejects_wrong_password | Checks that login rejects an incorrect password. | PASS | PASS | Detects nothing | Kaung Myat Tun `Kaungmyattun8`|
| 3 | test_search_finds_exact_title | Search finds a product by its exact title | PASS | PASS | Detects nothing | Lwin Oo — lwinDoubleOmega |
| 4 | test_import_returns_count | Checks that bulk import returns the number of products imported. | FAIL | PASS | Genuine detection | Wathan Htat callmefrost27-dotcom |
| 5 | test_register_duplicate_returns_false | 	Checks that registering a duplicate username returns False. | PASS | PASS | 	Detects nothing | Kaung Myat Tun Kaungmyattun8 |
| 6 | | | | | | |
| 7 | test_add_to_cart_returns_true_for_known_product | Checks that adding a known product to the cart returns True. | PASS | PASS | Detects nothing | Wathan Htat callmefrost27-dotcom |
| 8 | test_search_is_limited_to_ten_results | Claims that catalogue search should return no more than ten matches. | FAIL | FAIL | Invented requirement | Lwin Oo — lwinDoubleOmega |

### Detail entries

Copy the block below once for each of the eight tests. Do not paste the test
code — it is in the repository and identical for everyone. Reference it by name
and line instead.

---
#### Test: test_login_rejects_wrong_password (ai_review/test_ai_generated.py:17)

*Audited by:* Kaung Myat Tun — Kaungmyattun8

*1. What it claims to catch.*

text
Login should reject an incorrect password.

*2. Evidence — against the shipped code*  
Run location: original worktree at commit 32957a2.

Command: python3 -m pytest ai_review/test_ai_generated.py -v

text
ai_review/test_ai_generated.py::test_login_rejects_wrong_password PASSED [ 25%]

*3. Evidence — against your fixed code*  
Run location: main project checkout after applying the login fix.

Command: python3 -m pytest ai_review/test_ai_generated.py -v

text
ai_review/test_ai_generated.py::test_login_rejects_wrong_password PASSED [ 25%]

*4. What that pair proves.*

text
The test passes on both versions. It uses a completely different password,
so it does not detect the bug where an extra symbol could be removed from a
password before it was checked.

*5. Verdict* (tick one)

- [x] Detects nothing — PASS then PASS

---

#### Test: test_search_finds_exact_title  (ai_review/test_ai_generated.py:25)

*Audited by:* Lwin Oo — lwinDoubleOmega

*1. What it claims to catch.*
Search should find a product when given its exact title.

*2. Evidence — against the shipped code*

Run location: st211-midterm-Edamame-baseline, at original commit e311d22.

Command: python3 -m pytest ai_review/test_ai_generated.py::test_search_finds_exact_title -v

text
ai_review/test_ai_generated.py::test_search_finds_exact_title PASSED     [100%]
============================== 1 passed in 0.02s ===============================

*3. Evidence — against your fixed code*

Run location: st211-midterm-Edamame-new, on branch/catalog at fix commit 0500f59.

Command: python3 -m pytest ai_review/test_ai_generated.py::test_search_finds_exact_title -v

text
ai_review/test_ai_generated.py::test_search_finds_exact_title PASSED     [100%]
============================== 1 passed in 0.01s ===============================

*4. What that pair proves.*
The test passes on both versions. Its assertion,
`cat.search("Python Testing") == [1]`, uses exactly the title's
capitalization, so it does not detect the capitalization issue.
Using `cat.search("python") == [1]` would expose that issue,
as demonstrated by my regression test.

*5. Verdict* (tick one)

- [x] Detects nothing — PASS then PASS

---

#### Test: test_register_duplicate_returns_false (ai_review/test_ai_generated.py:38)

*Audited by:* Kaung Myat Tun — Kaungmyattun8

*1. What it claims to catch.*

text
Registering a duplicate username should return False.

*2. Evidence — against the shipped code*  
Run location: original worktree at commit 32957a2.

Command: python3 -m pytest ai_review/test_ai_generated.py -v

text
ai_review/test_ai_generated.py::test_register_duplicate_returns_false PASSED [ 62%]

*3. Evidence — against your fixed code*  
Run location: main project checkout.

Command: python3 -m pytest ai_review/test_ai_generated.py -v

text
ai_review/test_ai_generated.py::test_register_duplicate_returns_false PASSED [ 62%]

*4. What that pair proves.*

text
The test passes on both versions. It does not detect a bug fixed by the group.

*5. Verdict* (tick one)

- [x] Detects nothing — PASS then PASS

---

#### Test: test_search_is_limited_to_ten_results (ai_review/test_ai_generated.py:55)

*Audited by:* Lwin Oo — lwinDoubleOmega

*1. What it claims to catch.*

text
Catalogue search should return no more than ten results.

*2. Evidence — against the shipped code*

Run location: st211-midterm-Edamame-baseline, at original commit e311d22.

Command: python3 -m pytest ai_review/test_ai_generated.py::test_search_is_limited_to_ten_results -v

text
ai_review/test_ai_generated.py::test_search_is_limited_to_ten_results FAILED [100%]

E       AssertionError: assert 20 <= 10

============================== 1 failed in 0.03s ===============================

*3. Evidence — against your fixed code*

Run location: st211-midterm-Edamame-new, on branch/catalog with catalog fix 0500f59.

Command: python3 -m pytest ai_review/test_ai_generated.py::test_search_is_limited_to_ten_results -v

text
ai_review/test_ai_generated.py::test_search_is_limited_to_ten_results FAILED [100%]

E       AssertionError: assert 20 <= 10

============================== 1 failed in 0.03s ===============================

*4. What that pair proves.*

text
The test fails on both versions because the search returns 20 matching
products. The project does not specify a ten-result search limit, so this
test asserts an invented requirement.

*5. Verdict* (tick one)

- [x] Invented requirement — FAIL then FAIL

---

#### Test: test_cart_total_sums_items (ai_review/test_ai_generated.py:10)

*Audited by:* Han Htoo Aung — Erics12487254

*1. What it claims to catch.*

text
The cart total should include the prices of all items.

*2. Evidence — against the shipped code*  
Run location: original worktree at commit 32957a2.

Command: python3 -m pytest ai_review/test_ai_generated.py -v

text
ai_review/test_ai_generated.py::test_cart_total_sums_items FAILED [ 12%]

*3. Evidence — against your fixed code*  
Run location: main project checkout after applying the cart-total fix.

Command: python3 -m pytest ai_review/test_ai_generated.py -v

text
ai_review/test_ai_generated.py::test_cart_total_sums_items PASSED [ 12%]

*4. What that pair proves.*

text
The test fails on the shipped code and passes after the fix. It detects the
cart-total bug where the last item was omitted from the total.

*5. Verdict* (tick one)

- [x] Genuine detection — FAIL then PASS

---

#### Test: test_import_returns_count (ai_review/test_ai_generated.py:31)

*1. What it claims to catch.*

*Audited by:* Wathan Htat —  callmefrost27-dotcom

text
Bulk import should return the number of products imported.

*2. Evidence — against the shipped code*  
Run location: original worktree at commit 32957a2.

Command: python3 -m pytest ai_review/test_ai_generated.py -v

text
ai_review/test_ai_generated.py::test_import_returns_count FAILED [ 50%]

*3. Evidence — against your fixed code*  
Run location: main project checkout after applying the import-count fix.

Command: python3 -m pytest ai_review/test_ai_generated.py -v

text
ai_review/test_ai_generated.py::test_import_returns_count PASSED [ 50%]

*4. What that pair proves.*

text
The test fails on the shipped code and passes after the fix. It detects the
import-count bug, which caused the function to report one extra product.

*5. Verdict* (tick one)

- [x] Genuine detection — FAIL then PASS

---
#### Test: test_add_to_cart_returns_true_for_known_product (ai_review/test_ai_generated.py:51)

*Audited by:* Wathan Htat —  callmefrost27-dotcom

*1. What it claims to catch.*

text
Adding a known product to the cart should return True.

*2. Evidence — against the shipped code*  
Run location: original worktree at commit 32957a2.

Command: python3 -m pytest ai_review/test_ai_generated.py -v

text
ai_review/test_ai_generated.py::test_add_to_cart_returns_true_for_known_product PASSED [ 87%]

*3. Evidence — against your fixed code*  
Run location: main project checkout.

Command: python3 -m pytest ai_review/test_ai_generated.py -v

text
ai_review/test_ai_generated.py::test_add_to_cart_returns_true_for_known_product PASSED [ 87%]

*4. What that pair proves.*

text
The test passes on both versions. It checks normal cart behaviour but does not
detect a bug fixed by the group.

*5. Verdict* (tick one)

- [x] Detects nothing — PASS then PASS

---

## Part 3 — Coverage against your own bug hunt

One row per bug you recorded in FINDINGS.md, plus a row for any bug the
supplied set caught that you had not found yourself.

| Bug (your description) | We found it | The supplied set catches it | Which test, and how you established that |
|---|---|---|---|
| Catalog.search() misses lowercase python in Python Testing. | YES | NO | test_search_finds_exact_title passes on both shipped and fixed code. It searches using the title’s exact capitalization, so it does not check whether lowercase python finds the book. My regression test checks that case and fails on shipped code. |
| Users.login() accepts a wrong password when it differs from the stored password only by an extra symbol. | YES | NO | test_login_rejects_wrong_password passes on both versions. It uses an entirely different password, so it does not check whether an extra symbol is incorrectly removed. |
| Cart.total() omits the last item from the total. | YES | YES | test_cart_total_sums_items fails on shipped code and passes on fixed code, showing it detects the bug. |
| Bulk import reports one more product than it imported. | YES | YES | test_import_returns_count fails on shipped code and passes on fixed code, showing it detects the incorrect count. |
| | | | |
| | | | |

This is a factual cross-reference, not an opinion. Every "yes" in the third
column must point at a test you classified as a genuine detection in Part 2.

---


## Part 4 — Verdict (4 marks, 400 words maximum)

Four questions. Be specific and cite your own table.

1. *Which failure mode was most common* in the supplied set? Give the count
   from your summary table.
2. **Why is a test that passes against buggy code more dangerous than one that
   crashes?** A crashing test gets attention. Say what happens to the other
   kind.
3. *Did the supplied set catch anything you had missed?* If yes, what, and
   what would have led you to it. If no, say so — that is a legitimate answer
   and does not cost marks.
4. *What will you check before trusting a generated test in future?* Name
   checks you actually performed in this exercise, not general principles.

Answers that could have been written about any project score half marks at
best.

---

## Appendix (optional)

Prompts, or notes on AI use, that support Part 1.
