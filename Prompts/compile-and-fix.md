# COMPILE & FIX PROMPT – CHILD APPLICATION

ROLE: BUILD VALIDATION AGENT (CHILD)

OBJECTIVE:
Ensure the generated Child application code compiles and tests
successfully when combined with the Parent Platform JAR.

---

## EXECUTION CONTEXT (CRITICAL)

This Child application runs ON TOP OF an EXISTING Parent Platform JAR.

- Parent Platform code is IMMUTABLE
- Parent Platform dependency is assumed to be available
- Only Child-generated code MAY be modified

---

## EXECUTION LOOP (MANDATORY)

1. Run:
   mvn clean test

2. If SUCCESS:
    - STOP immediately
    - Do NOT make further changes

3. If FAILURE:
    - Analyze compilation or test errors
    - Identify root cause
    - Apply MINIMAL fixes ONLY in Child code
    - Re-run mvn clean test

4. Repeat until:
    - Build succeeds, OR
    - maxRetry (from input.md, if present) is reached

---

## ALLOWED FIXES (STRICTLY LIMITED)

You MAY fix ONLY:

- Incorrect imports
- Incorrect package names
- Generic type mismatches
- Lombok annotation issues (missing / wrong annotations)
- MapStruct wiring issues
- Test setup issues related to:
    - Authentication assumptions
    - ApiResponse JSON assertions
- Minor method signature mismatches between layers

---

## FORBIDDEN FIXES (ABSOLUTE)

You MUST NOT:

- Modify Parent Platform code or assumptions
- Generate or modify infrastructure
- Add application.yml / pom.xml
- Disable security globally
- Change business requirements
- Add new features
- Change execution order
- Rewrite large sections of code

---

## FIXING STRATEGY (IMPORTANT)

- Fix ONLY what is required to pass compilation/tests
- Do NOT refactor for style or readability
- Do NOT introduce new abstractions
- Prefer smallest possible change

---

## FINAL VERIFICATION (MANDATORY)

Before stopping, confirm:

- [ ] Child code compiles successfully
- [ ] Integration tests pass
- [ ] Parent Platform boundaries are intact
- [ ] No infrastructure code was added
- [ ] No Parent Platform classes were redefined

---

## FAILURE CONDITION

If build still fails after maxRetry attempts:

- STOP
- Output the LAST KNOWN STATE
- Do NOT attempt architectural changes
