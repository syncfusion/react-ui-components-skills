## Overview

This document provides common troubleshooting scenarios for Syncfusion React components across Getting Started, Advanced Features, and Globalization areas. Each issue includes a short resolution to quickly fix problems during development. 

## Table of Contents

- [Getting Started Issues)](#getting-started-issues)
- [Advanced Features Issues](#advanced-features-issues)
- [Globalization Issues](#globalization-issues)

---

# Getting Started Issues

## Issue: Module not found error

**Cause:** Package not installed

**Resolution:** Run `npm install` for required Syncfusion package

---

## Issue: Component not rendering

**Cause:** Incorrect setup or missing directives

**Resolution:** Ensure proper component structure and directives

---

## Issue: Styles not applied

**Cause:** CSS files not imported

**Resolution:** Import required Syncfusion theme CSS files

---

## Issue: Build errors in framework setup

**Cause:** Incorrect framework configuration

**Resolution:** Verify Next.js/Vite/Remix setup and config

---

## Issue: Grid or component empty

**Cause:** Invalid or empty dataSource

**Resolution:** Provide valid array data and correct field mapping

---

# Advanced Features Issues

## Issue: Animation not working

**Cause:** Element not available or incorrect usage

**Resolution:** Ensure element reference exists before animation

---

## Issue: Drag and drop not functioning

**Cause:** Incorrect DOM target or configuration

**Resolution:** Verify Draggable and Droppable setup

---

## Issue: State persistence not working

**Cause:** LocalStorage issues or disabled setting

**Resolution:** Enable `enablePersistence` and check browser storage

---

# Globalization Issues

## Issue: RTL not applied

**Cause:** RTL not enabled correctly

**Resolution:** Use `enableRtl(true)` or component-level prop

---

## Issue: Localization not working

**Cause:** Locale data not loaded

**Resolution:** Load locale using `L10n.load()` and set locale

---

## Issue: Incorrect number/date format

**Cause:** CLDR data not loaded

**Resolution:** Install and load CLDR data properly |