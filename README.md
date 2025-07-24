# Test Merly Mentor Action Repository

This repository contains tests for the [Merly Mentor GitHub Action](https://github.com/merly-ai/mentor-action). Each branch represents a different test scenario. To add a new scenario, create a branch and open a PR against this repository.

---

## Prerequisites

Before creating PRs, ensure your repository has:

* **Secrets**

  * `MM_KEY`: Your Mentor API key (used by branches testing a valid or invalid key).

* **Repository Variables**

  * `MODEL_PATH`: Relative path to the repository root (used by the `path_as_env_variable` branch).

---

## Branches Requiring PRs

### 1. `no_variables`

* **Description:** Tests the action with **no** variables set.
* **Expected output:**

  ```text
  Warning: ⚠️ ALERT: 6 New Issues Found, however only the top 3 are displayed using the free version.!
  ```

### 2. `mm_key_set`

* **Description:** Tests the action with a **valid** Mentor key provided.
* **Prerequisite:** Set the secret `MM_KEY` to a valid key.
* **Expected output:**

  ```text
  Warning: ⚠️ ALERT: 6 New Issues Found!
  ```

### 3. `incorrect_key`

* **Description:** Tests the action with an **unregistered** Mentor key.
* **Prerequisite:** Set the secret `MM_KEY` to an unregistered key.
* **Expected output:**

  ```text
  - Unregistered Basic
  - Key [XXXX-YYYY-ZZZZ-MMM]
  Warning: ⚠️ ALERT: 6 New Issues Found, however only the top 3 are displayed using the free version.!
  ```

### 4. `checkout_false_no_depth`

* **Description:** Tests the action when `checkout-code: false` **without** a fetch depth.
* **Prerequisite:**

  1. In the checkout step, omit `fetch-depth`.
  2. In the Mentor step, set `checkout-code: false`.
* **Expected output:**

  ```text
  Warning: ❌ Unable to determine commit parent for "/repo" at ee3d791589d4670dcb4e3003b9010a8d225a9289.
  ```

### 5. `origin/checkout_false_with_path_set`

* **Description:** Tests the action when `checkout-code: false` and custom paths are set.
* **Prerequisite:**

  1. Checkout step: `path: testpath`.
  2. Mentor step: `checkout-code: false` and `path: ${{ github.workspace }}/testpath`.
* **Expected output:**

  ```text
  Warning: ⚠️ ALERT: 6 New Issues Found!
  ```

### 6. `path_as_env_variable`

* **Description:** Tests the action when the repository path is specified via a variable.
* **Prerequisite:**

  1. Define repository variable `MODEL_PATH` (e.g., `testpath`).
  2. Mentor step: `path: ${{ github.workspace }}/${{ vars.MODEL_PATH }}`.
* **Expected output:**

  ```text
  Warning: ⚠️ ALERT: 6 New Issues Found!
  ```

### 7. `debug_logs_on`

* **Description:** Tests the action with detailed debug logging enabled.
* **Prerequisite:** Set `debug: true` in the Mentor step.
* **Expected output:**

  * Logs include verbose debug messages, for example:

    ```text
    DEBUG: <<
    ```

### 8. `max_issue_count_fail`

* **Description:** Tests that when the total issues found meets or exceeds the `max-issue-count`, the pipeline fails.
* **Prerequisite:**
  1. In the Mentor step, set `max-issue-count: 2`.
* **Expected output:**

  ```text
  Warning:  ❌ ALERT: 6 New Issues Found (meets or exceeds max-issue-count of 2).
  ```

### 9. `max_issue_count_succeed`

* **Description:** Tests that when the `max-issue-count` is above the total issues found, the pipeline succeeds.
* **Prerequisite:**
  1. In the Mentor step, set `max-issue-count: 10`.
* **Expected output:**

  ```text
  ✅ Warning: ⚠️ ALERT: 6 New Issues Found!
  ```

### 10. `max_issue_priority_string_fail`

* **Description:** Tests that when using a string for `max-issue-priority`, issues at or above that priority fail.
* **Prerequisite:**
  1. In the Mentor step, set `max-issue-priority: Critical`.
* **Expected output:**

  ```text
  Warning:  ❌ ALERT: at least one issue’s priority meets or exceeds max-issue-priority of Critical.
  ```

### 11. `max_issue_priority_number_fail`

* **Description:** Tests that when using a number for `max-issue-priority`, issues at or above that numeric level fail.
* **Prerequisite:**
  1. In the Mentor step, set `max-issue-priority: 3`.
* **Expected output:**

  ```text
  Warning:  ❌ ALERT: at least one issue’s priority meets or exceeds max-issue-priority of High.
  ```

### 12. `priority_threshold_above_issues_found`

* **Description:** Tests that when all issues found have a lower priority than the `max-issue-priority` threshold, the pipeline succeeds.
* **Prerequisite:**
  1. In the Mentor step, set `max-issue-priority: Critical` (or a numeric level above existing issue priorities).
* **Expected output:**

  ```text
  Warning: Issue ID=0:1, state = anomaly and priority = High
   - test_new_file.py:2:3: issue => Issue ID=0:1, state = anomaly and priority = High @ https://github.com/leeboydmerly/test-action/blob/c11da89d6455227e4abc2f8d02e28d14e7b71e12/test_new_file.py#L2

  Warning:  ⚠️ ALERT: 1 New Issues Found!
  ```

---

## Other Suggested Test Scenarios

* **Out‑of‑Range Values:** Set `max-issue-count` or `max-issue-priority` to values outside valid ranges (e.g., `max-issue-priority: 10` or `max-issue-count: -1`) and verify error handling.
* **Zero Defaults:** Explicitly set `max-issue-count: 0` and `max-issue-priority: 0` to confirm they default correctly (no failures when unused).
* **Descending Order Verification:** Confirm issues are printed in descending order by priority and timestamp.
* **Key Combinations:** Run scenarios both with and without the `MM_KEY` secret to validate the new variables function correctly under free vs. licensed modes.
