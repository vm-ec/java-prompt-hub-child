# Child Prompt Hub – Copilot Execution Guide

## Purpose

This repository contains the **Child Prompt Hub**, which generates
**business and domain-specific Spring Boot code** on top of an existing
**Parent Platform JAR**.

The Parent Platform provides all infrastructure.
The Child Prompt Hub provides ONLY application-specific logic.

This separation is STRICT and MUST be respected at all times.

---

## High-Level Architecture (LOCKED)

### Parent Platform (Already Generated)

The Parent Platform JAR provides:

- ApiResponse (standard response wrapper)
- ApiException and ErrorCode
- GlobalExceptionHandler
- Logging and trace ID propagation
- Security (CSRF, JWT, OAuth, etc.)
- External API connectors
- Base configuration and filters

The Parent Platform is IMMUTABLE from the Child Hub.

---

### Child Prompt Hub (This Repository)

The Child Prompt Hub generates ONLY:

- Controllers
- Services and service implementations
- Repositories
- Entities / Models
- DTOs (ServiceAccept / ServiceResponse)
- Mappers
- Domain-specific exceptions (extending Parent ApiException)
- Feature-level Java configuration (ONLY if explicitly required)
- Integration tests

The Child Hub MUST NOT recreate any Parent functionality.

---

## Repository Structure (MANDATORY)

child-prompt-hub/
├── README.md # This file (execution guide)
├── INSTRUCTIONS.md # Hard execution contract
├── input.md # User / business requirement specification
└── prompts/
├── orchestrator.md
├── controller.md
├── service.md
├── repository.md
├── model.md
├── dto.md
├── mapper.md
├── exception.md
├── config.md
└── integration-tests.md

---

## Execution Model

Copilot MUST behave as an **autonomous build agent**, not as a chat assistant.

Execution flow:

README.md → explains HOW to execute
INSTRUCTIONS.md → defines STRICT rules
input.md → defines WHAT to build (immutable)
prompts/ → define HOW to build each layer

---

## How to Run This in Copilot (IMPORTANT)

### Step 1: Open the Repository

Open this repository in VS Code so that Copilot has access to:
- README.md
- INSTRUCTIONS.md
- input.md
- prompts/ folder

---

### Step 2: Open Copilot Chat (Agent Mode)

- Windows / Linux: `Ctrl + Shift + I`
- macOS: `Cmd + Shift + I`

Ensure Copilot can read the entire workspace.

---

### Step 3: Use the START COMMAND (MANDATORY)

Paste **exactly** the following into Copilot Chat:

You are an autonomous build agent.

Read README.md and INSTRUCTIONS.md completely.
Read input.md as immutable configuration.
Assume the Parent Platform JAR already exists and is available as a dependency.
Execute prompts/orchestrator.md and all child prompts in order.
Generate ONLY business-layer code.
Ensure ALL code depends on the Parent Platform.
Output the result as a single JSON object mapping file paths to file contents.

Begin execution now.

---

## What Copilot MUST NOT Do

Copilot MUST NOT:

- Generate pom.xml
- Generate application.yml or application.properties
- Configure logging
- Configure security
- Create ApiResponse or error wrappers
- Create GlobalExceptionHandler
- Modify input.md
- Generate infrastructure code of any kind

---

## Verification Checklist (Copilot Self-Check)

Before stopping, Copilot MUST confirm:

- [ ] README.md and INSTRUCTIONS.md were read first
- [ ] input.md was treated as immutable
- [ ] Parent Platform dependency was assumed
- [ ] No infrastructure code was generated
- [ ] Controllers return Parent ApiResponse
- [ ] Domain exceptions extend Parent ApiException
- [ ] Integration tests assert Parent response structure

---

## Completion Criteria

Execution is SUCCESSFUL only when:

✔ All required layers are generated  
✔ Parent–Child boundary is respected  
✔ Generated code compiles with the Parent Platform

---

## Final Note

This README is an **execution contract**, not documentation.

Copilot MUST follow it EXACTLY.
Any deviation is incorrect behavior.

