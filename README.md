Test Merly Mentor Action Repository

This repository contains tests for the Merly Mentor GitHub Action. Each branch represents a different test scenario. To add a new scenario, create a branch and open a PR against this repository.

Prerequisites

Make sure the following are configured in your repository before creating PRs:

Secrets

MM_KEY: Your Mentor API key (used by branches testing a valid or invalid key).

Variables

MODEL_PATH: Path to the repository root (used by path_as_env_variable branch).

Branches Requiring PRs

no_variablesTests the action with no variables set.Expected output:Warning: ⚠️ ALERT: 6 New Issues Found, however only the top 3 are displayed using the free version.!

mm_key_setTests the action with the Mentor key provided.Prerequisite: Set mm-key: ${{ secrets.MM_KEY }} to a valid Mentor key.Expected output:Warning: ⚠️ ALERT: 6 New Issues Found!

incorrect_keyTests the action with an unregistered key.Prerequisite: Set mm-key: ${{ secrets.MM_KEY }} to an unregistered Mentor key.Expected output:- Unregistered Basic- Key [XXXX-YYYY-ZZZZ-MMM]Warning: ⚠️ ALERT: 6 New Issues Found, however only the top 3 are displayed using the free version.!

checkout_false_no_depthTests the action when checkout-code: false is set without specifying a fetch depth.Prerequisite: Set checkout-code: false in the Merly Mentor step and omit fetch-depth in the checkout action.Expected output:Warning: ❌ Unable to determine commit parent for "/repo" at ee3d791589d4670dcb4e3003b9010a8d225a9289.

origin/checkout_false_with_path_setTests the action when checkout-code: false and custom paths are set.Prerequisite: Set checkout-code: false, configure the checkout action with path: testpath, and set path: ${{ github.workspace }}/testpath in the Merly Mentor step.Expected output:Warning: ⚠️ ALERT: 6 New Issues Found!

path_as_env_variableTests the action when the repository path is specified via a GitHub variable.Prerequisite: Define MODEL_PATH as a repository variable and set path: ${{ github.workspace }}/${{ vars.MODEL_PATH }} in the Merly Mentor step.Expected output:Warning: ⚠️ ALERT: 6 New Issues Found!

debug_logs_onTests the action with detailed debug logging enabled.Prerequisite: Set debug: true in the Merly Mentor step.Expected output:Includes verbose debug messages, for example:DEBUG: <<

