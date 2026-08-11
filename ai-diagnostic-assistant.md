---
layout: page
title: Speeding up schedule exports, then teaching an AI agent to do the diagnosis
subtitle: Delevant Business Solutions · 2024–present
---

## The Problem

I support AspenTech's Aspen Plant Scheduler (Aspen SCM) for specialty chemicals manufacturing clients — an approximately 50,000-line, largely undocumented codebase of MIMI rules, FMTDATA format routines, and SCM JavaScript, spanning 400+ tables and dozens of interdependent rulesets, with logic that branches per plant site.

When a client reported an incident — an order silently disappearing, a scheduling calculation coming out wrong, an export to their ERP mismatching — diagnosing it meant tracing which of dozens of rulesets touched which of 400+ tables, then checking whether a materially similar case had already been solved for a different client, sometimes years earlier, buried in an old email thread nobody could find quickly.

In practice, that meant only one or two senior developers who had built up years of tribal knowledge of the codebase could reliably root-cause a non-trivial incident. Everyone else escalated, and escalation was the real bottleneck: senior time was the scarce resource, while knowledge that already existed — in the code, in its dependency structure, in old email threads — sat unreusable by anyone else on the team.

## What I Built

I built a diagnostic assistant on top of Claude and gave it three things no single person carries in their head: the full codebase (rulesets, format routines, JavaScript), a dependency graph I generated mapping which rule calls or fires which other rule or table, and a table/catalog index I built to make the 400+ tables navigable. I also fed it the historical support email threads across our client base, having it read and categorize them by root cause.

Given a new incident, it now proposes a ranked, hierarchical list of probable causes — citing the actual rule, table, and format routine involved, not a generic description — and if a materially similar case has been solved before, it surfaces that resolution directly instead of leaving us to rediscover it. Where the root cause is on the client's side and a precedent exists, I am extending it to draft a case-specific reply email automatically; otherwise it points the team at the specific area of code to investigate. I have since packaged parts of this into reusable skills and kept prototyping on top as new incident types surface.

## Who Used It

Built for, and used daily by, our internal implementation and support team — the same engineers who previously had no choice but to escalate anything beyond routine issues to one of our senior developers.

## What Changed

Diagnosis is no longer gated on which one or two people happened to have memorized this specific undocumented codebase. An engineer without years of tenure on the system can now get pointed to the exact rule and table behind an incident in roughly the time it used to take just to locate the right file, because the assistant does in seconds what previously meant manually tracing dependencies through 50,000 lines of code, or searching years of email by hand.

Issues that used to sit in a senior developer's queue now get triaged directly by whoever picks them up, and incidents that are recurrences of something we've already solved get resolved instead of being investigated again from scratch. The bottleneck moved from "who remembers this" to "what does the code and history actually say" — which is answerable by anyone on the team, not just the two people who built the system.
