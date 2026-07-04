---
title: Vibecoding Capstone Ideation
aliases: [Capstone Ideas, Brainstorming]
type: context
project: vibecoding-capstone
status: evergreen
created: 2026-07-03
updated: 2026-07-03
tags: [brainstorm, ideas]
source: 
---

# Vibecoding Capstone Ideation

> [!abstract] TL;DR
> Initial brainstorm of four potential projects for the capstone, mapped to the four competition tracks and aligned with the required course concepts.

## Idea 1: Med-Minder (Agents for Good Track)
**The Problem:** Elderly patients or individuals with complex health conditions often struggle to manage multiple prescriptions and avoid dangerous drug interactions.
**The Solution:** An agent system that reads uploaded images of prescriptions or doctors' notes, checks for conflicts against a medical database via an MCP server, and generates a safe, clear daily schedule.
**Required Concepts Demonstrated:**
1. **Multi-agent system:** (1) Vision/Parser Agent, (2) Medical Conflict Checker Agent, (3) Scheduler Agent.
2. **MCP Server:** A custom MCP server connecting to an open drug interaction API (like RxNorm).
3. **Agent skills:** Using vision models to read handwriting/labels.

## Idea 2: Auto-Triage (Agents for Business Track)
**The Problem:** Engineering managers spend too much time reading, categorizing, and assigning new bug reports or feature requests in their issue trackers.
**The Solution:** An agent that connects to Jira/GitHub, reads new issues, assesses their severity, categorizes them, tags the appropriate team members, and even suggests a preliminary fix or questions for the reporter.
**Required Concepts Demonstrated:**
1. **MCP Server:** Connecting to the Jira/GitHub API to fetch issues securely.
2. **Antigravity:** Using Antigravity to coordinate the automated workflow and execute terminal commands to pull code context.
3. **Deployability:** Packaged as a GitHub Action or a scheduled cron job.

## Idea 3: Local-Librarian (Concierge Agents Track)
**The Problem:** People have massive local collections of PDFs, epubs, and personal documents, but no easy way to query them without uploading private data to the cloud.
**The Solution:** A privacy-first, local personal assistant. It uses a local MCP server to index the user's specific document folders and answers complex questions based entirely on their personal library, guaranteeing data privacy.
**Required Concepts Demonstrated:**
1. **Security features:** Strict local sandboxing; the agent cannot access files outside the designated library directory, and no data is sent to external APIs (using local LLMs if possible, or strict API data retention policies).
2. **MCP Server:** Exposing local file system read operations securely.
3. **Agent skills:** Advanced document QA and summarization.

## Idea 4: Hackathon-Hacker (Freestyle Track)
**The Problem:** Hackathon participants waste the first 12 hours setting up boilerplate, configuring environments, and arguing about architecture instead of building the core feature.
**The Solution:** A meta-agent designed to bootstrap hackathon projects. You give it your idea, and it generates the architecture diagram, sets up the Git repo, scaffolds the boilerplate code via terminal commands, and creates a task list.
**Required Concepts Demonstrated:**
1. **Antigravity:** Heavy use of Antigravity for terminal execution (creating directories, running `npm init`, etc.).
2. **Multi-agent system:** (1) Architect Agent, (2) Coder Agent, (3) DevOps Agent.
3. **Deployability:** The final output of the agent is a fully deployable template with Dockerfiles or Vercel configurations.

## Related

- [[competition-rules]] - Ensure these ideas meet the requirements.
- [[vibecoding-capstone]] - Project MOC
