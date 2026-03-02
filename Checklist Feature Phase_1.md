# SSF Phase 1 -- Daily Checklist Feature (V1)

## Overview

Phase 1 introduces a structured **Daily Execution Checklist** into the
SSF application. The goal is to allow users to log daily execution,
track measurable progress, and view conversion metrics without
gamification.

------------------------------------------------------------------------

## Objectives

-   Provide a daily logging experience (default: Today)
-   Reuse existing time filters (Week / Month / Year / Lifetime /
    Custom)
-   Show Summary metrics below checklist
-   Display system-calculated conversions
-   Show Daily Completion %
-   Show static "✔ System Complete Today" label at 100%
-   No gamification elements

------------------------------------------------------------------------

# Checklist Structure

## Total Counted Tasks

-   Manual Tasks: 17
-   Auto-Fed Tasks: 2
-   Total Counted: 19
-   Derived rows are excluded

------------------------------------------------------------------------

## Section 1: Find Prospects (Auto-Fed)

-   Friend Requests & Messages Sent (auto)
-   Accepted (auto)
-   Lifetime Acceptance Rate (derived -- display only)

Auto-fed tasks count toward completion. Acceptance rate does NOT count.

------------------------------------------------------------------------

## Section 2: Build Relationships (Manual)

-   Rebuild Relationship
-   Prospect
-   Prospects Responded
-   Follow Up After Process
-   Final Follow Up
-   Lifetime Conversion Rate (derived -- display only)

All numeric fields must contain a value (including 0). Derived
conversion does NOT count toward completion.

------------------------------------------------------------------------

## Section 3: Stay Connected (Manual)

-   Personal Post (checkbox)
-   Message Who Engaged on Personal Post
-   Business Post (checkbox)
-   Message Who Engaged on Business Post
-   Reply to Stories
-   Comment on Friend's Posts
-   Notifications (checkbox)
-   Birthdays

------------------------------------------------------------------------

## Section 4: Facebook Group

-   Post (checkbox)
-   Comment on Posts

------------------------------------------------------------------------

## Section 5: Growth

-   New Customers
-   New Referrals

------------------------------------------------------------------------

# Completion Rules

A day is 100% complete only when:

-   Every required checkbox is checked
-   Every required number field contains a value (including 0)
-   Auto-fed tasks contain values
-   Blank ≠ 0
-   Derived rows excluded

### Formula

Daily Completion % = (Completed Tasks / 19) × 100

Display: - Round percentage - Remaining % = 100 - rounded value

------------------------------------------------------------------------

# System Complete Indicator

When daily_completion_percent == 100:

Display: ✔ System Complete Today

Rules: - No animation - No celebration graphics - No gamification

------------------------------------------------------------------------

# Save Behavior

## Auto Save

-   Silent on field change
-   No toast

## Manual Save

Button: Save Checklist

Toast: - If 100% → "Saved. You completed 100% today." - Else → "Saved.
You're at X% for today."

Anti-spam: - If clicked again within 10 seconds → Save but no toast

------------------------------------------------------------------------

# Conversion Formulas

## Acceptance Rate

If friend_requests_sent \> 0: accept_rate = accepted /
friend_requests_sent Else: 0%

Display: Percent only

Must be stored as normalized daily records.

------------------------------------------------------------------------

## Build Relationships Conversion

outcomes = new_customers + new_referrals

If prospects_responded \> 0: rate = outcomes / prospects_responded

Display: - If percent == 0 → 0% - Else → 1 in X (Y%)

Derived values are NOT stored.

------------------------------------------------------------------------

# Range Summary Rules

Range Completion % = Average daily_completion_percent Across days WITH
at least one checklist entry

Do NOT average across empty days.

Days at 100% = Count where daily_completion_percent == 100

------------------------------------------------------------------------

# Data Architecture

Table: checklist_entries

Fields: - id - user_id - date_key (YYYY-MM-DD normalized to user
timezone) - task_key - value_number (nullable int) - value_boolean
(nullable bool) - source_type (manual \| auto_find_prospects) -
created_at - updated_at - updated_by

Rules: - Use normalized records - Do NOT store JSON blobs - Do NOT store
derived values

------------------------------------------------------------------------

# Navigation Changes

1.  Dashboard
2.  Checklist
3.  Find Prospects
4.  Prospect Lists
5.  Training
6.  Referral

First Login → Training After First Login → Checklist

------------------------------------------------------------------------

# Phase 1 Goal

Transform SSF into a structured daily execution accountability system
with measurable conversion visibility and calm UX design.
