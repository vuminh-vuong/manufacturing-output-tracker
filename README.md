# Manufacturing Output System

> **Production tracking & automated reporting solution for a global industrial safety equipment manufacturer (factory)**

---

## Project Summary

A desktop application built in **C# .NET Framework 4.0** following the **MVVM (Model–View–ViewModel)** architectural pattern. The system digitizes and automates the entire shift-based production workflow on the factory floor — from real-time data entry by operators to automated multi-template report generation for management.

The solution replaced manual paper-based tracking, enabling supervisors to instantly generate production reports, package entry forms, and quality audit documents with a single action.

---

## Architecture & Design Patterns

| Concern | Approach |
|---|---|
| UI Pattern | **MVVM** — strict separation of View, ViewModel, and Model layers |
| Report Layer | **SAP Crystal Reports** — programmatic report generation via `ReportClass` inheritance |
| Report Caching | **`ICachedReport` interface** — cache-aware report documents with configurable `CacheTimeOut` |
| Data Layer | Structured **CSV** log files — one file per production week, parsed and processed at runtime |
| Config | `app.config` with `useLegacyV2RuntimeActivationPolicy` for mixed-mode .NET 4.0 hosting |

---

## Core Technical Features

### 1. Multi-Template Crystal Reports Engine

Three distinct report templates are managed programmatically:

- **`CurrentProductionReport`** — Weekly production summary, accepts a `Parameter_PeriodDate` parameter to filter data by date range. Exposed as both a standard `ReportClass` and a cacheable `CachedCurrentProductionReport` (`ICachedReport`).
- **`PackageEntry`** — Auto-populated package entry document generated per production batch.
- **`PackageEntryMark`** — Quality marking sheet attached to each package for traceability.

Each report class:
- Inherits `CrystalDecisions.CrystalReports.Engine.ReportClass`
- Overrides `ResourceName`, `FullResourceName`, and `NewGenerator` for proper `.rpt` template binding
- Exposes 5 named `Section` properties for layout control
- Has a companion `Cached*` class implementing `ICachedReport` with `GetCustomizedCacheKey()` support for ASP.NET-style caching scenarios

### 2. Production Data Model (CSV Schema)

Each weekly log captures rich manufacturing data per production entry:

```
Month, Week, PlanQty, QtyPerPack, SaveDate, WorkDate, Time, Shift,
UserName, UCard (worker barcode card ID), ProductOf (country of origin),
Name (product SKU), Size,
RealQty, D1–D22 (22 individual defect category counts),
PackageWeight, TotalDefect, TargetQty,
DefectRate (%), FPY — First Pass Yield (%), Productivity (%), Preserved
```

Key domain concepts implemented:
- **22-category defect granularity** — each defect type tracked independently per entry
- **FPY (First Pass Yield)** calculation — standard lean manufacturing KPI
- **Productivity %** — actual vs target output ratio per shift
- **Defect Rate %** — computed from TotalDefect / RealQty
- **Shift-based tracking** — data segmented by shift number for shift-level reporting
- **Worker identity** — operator linked via UCard (barcode scan ID), enabling per-worker productivity analysis

### 3. MVVM Application Structure

```
MvvmClient/
├── Files/
│   ├── Images/          # Embedded product/branding assets
│   └── Templates/       # Crystal Reports .rpt + strongly-typed C# wrappers
├── Logs/                # Weekly CSV production logs (Log_W{weekNumber}.csv)
└── Plan/                # Production planning data
```

---

## Manufacturing Domain Knowledge

This project required deep understanding of:
- **Lean manufacturing KPIs**: FPY, Defect Rate, OEE-adjacent Productivity metrics
- **Batch/package traceability** workflow in glove manufacturing
- **Shift scheduling** and operator-level accountability
- **SAP-integrated reporting** pipeline (Crystal Reports for enterprise-grade output)

---

## Technology Stack

- **Language**: C# / .NET Framework 4.0
- **Pattern**: MVVM
- **Reporting**: SAP Crystal Reports for .NET (`CrystalDecisions.*`)
- **Data**: CSV structured logs, parameterized report queries
- **Platform**: Windows Desktop (WPF/WinForms client)

---

## What This Demonstrates for Your Project

If you need any of the following, I can deliver:

- Custom **automated report generation** (Crystal Reports, RDLC, PDF export)
- **Desktop applications** with clean MVVM architecture in C# / WPF
- **Manufacturing / factory floor systems** — production tracking, quality control dashboards
- **CSV / Excel data pipelines** — parsing, transformation, KPI calculation
- **Enterprise integrations** — connecting factory data to management reporting tools

---

> **Source code is kept private to protect the client's intellectual property.**  
> This README documents the technical architecture and skills applied in the project.
