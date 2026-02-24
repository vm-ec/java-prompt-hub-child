# CHILD PROMPT HUB – HARD EXECUTION CONTRACT

This file defines NON-NEGOTIABLE rules for Copilot execution.

---

## DOCUMENTATION RULES (MANDATORY)

- JavaDocs MUST be generated for:
   - Every class
   - Every method
   - Every field

- Each JavaDoc MUST include:
   1) PURPOSE:
      - What this class / method / field is responsible for
   2) HOW TO USE:
      - How and when it should be used by developers

- One-line, empty, or generic JavaDocs are NOT allowed.
- Missing JavaDocs invalidate the output.

---

## LOMBOK RULES (PLATFORM STANDARD)

- Lombok MUST be used wherever applicable.
- Explicit getters, setters, or constructors MUST NOT be written manually.
- Preferred Lombok annotations:
   - @Getter / @Setter (entities)
   - @Builder (DTOs where appropriate)
   - @RequiredArgsConstructor (services, controllers)
   - @NoArgsConstructor / @AllArgsConstructor only when required by JPA

---

## PLATFORM BOUNDARY RULES (STRICT)

- Parent Platform is IMMUTABLE.
- Parent Platform classes MUST NOT be redefined or shadowed.
- ApiResponse, ApiException, ErrorCode MUST be imported ONLY from Parent Platform.

---

## FORBIDDEN ACTIONS

Copilot MUST NOT:
- Generate infrastructure code
- Generate application.yml / pom.xml
- Configure logging or security
- Create GlobalExceptionHandler
- Modify input.md

Violation of any rule INVALIDATES the output.
