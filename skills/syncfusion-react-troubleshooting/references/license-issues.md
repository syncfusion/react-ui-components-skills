# Overview 

This document provides common licensing issues in Syncfusion React applications, including license generation, registration, and runtime errors. Each issue includes a clear explanation and concise resolution to help quickly fix licensing problems. 

## Table of Contents

- [License key not generated correctly](#license-key-not-generated-correctly)
- [License key not registered](#license-key-not-registered)
- [Invalid license key](#invalid-license-key)
- [Trial license expired](#trial-license-expired)
- [Platform mismatch error](#platform-mismatch-error)
- [Version mismatch error](#version-mismatch-error)
- [Environment variable not detected](#environment-variable-not-detected)
- [NPX activation command errors](#npx-activation-command-errors)
- [License not working in CI/CD](#license-not-working-in-cicd)
- [Multiple Syncfusion versions conflict](#multiple-syncfusion-versions-conflict)
- [License key formatting issues](#license-key-formatting-issues)
- [Cache issues after activation](#cache-issues-after-activation)

---

## License key not generated correctly

**Resolution:** Ensure the correct version and edition/platform are selected when generating the key.

Using the wrong version or edition will create an incompatible license key that fails during validation.

---

## License key not registered

**Resolution:** Set the `SYNCFUSION_LICENSE` environment variable and run `npx syncfusion-license activate`.

Without registration, the application shows license warnings even though components still render.

---

## Invalid license key

**Resolution:** Re-copy the license key without spaces or line breaks and ensure it matches your account.

Invalid formatting or partial copying is a common reason for this error.

---

## Trial license expired

**Resolution:** Renew your license, apply for a community license, or request a trial extension.

Expired licenses allow the app to run but show warnings until a valid key is registered.

---

## Platform mismatch error

**Resolution:** Generate a license key for the correct platform (React for React apps).

Older versions require platform-specific keys, and mismatches cause validation errors.

---

## Version mismatch error

**Resolution:** Ensure all Syncfusion packages use the same version and generate a matching license key.

A license key is valid only for the version it was generated for.

---

## Environment variable not detected

**Resolution:** Restart your IDE or terminal after setting `SYNCFUSION_LICENSE`.

Environment variables are not available until the session is refreshed.

---

## NPX activation command errors

**Resolution:** Run activation after ensuring environment variable or license file is correctly set.

Errors occur if the key is missing, invalid, or not accessible to the command.

---

## License not working in CI/CD

**Resolution:** Store the license key as a secret variable and pass it during build execution.

Hardcoding keys or missing secrets causes activation failure in pipelines.

---

## Multiple Syncfusion versions conflict

**Resolution:** Ensure all Syncfusion packages use the same version across the project.

Mixed versions cause validation inconsistencies and unexpected license errors.

---

## License key formatting issues

**Resolution:** Ensure the key is a single continuous string with no spaces or line breaks.

Formatting issues often happen during copy-paste from the portal.

---

## Cache issues after activation

**Resolution:** Clear `node_modules/.cache` and restart the application.

Old cached data may prevent the new license from being recognized correctly.