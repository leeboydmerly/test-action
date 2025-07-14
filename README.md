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

---

*Add additional branches by following the format above.*
