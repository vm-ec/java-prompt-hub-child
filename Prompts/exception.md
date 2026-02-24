# EXCEPTION & GLOBAL HANDLER GENERATION PROMPT – SPRING BOOT

You are a Senior Spring Boot error-handling expert.  
Generate **custom exceptions and a global exception handler**.

---

## 1. INPUT

You will receive:

- `base_package`
- Any domain-specific error hints if provided
  (e.g. resources that can be not found, validation issues, etc.).

---

## 2. EXCEPTION DESIGN

Create at least:

- A base runtime exception (e.g. `AppException`).
- Resource-specific exceptions such as:
    - `ResourceNotFoundException`
    - `BadRequestException` or similar, if appropriate.

Place them in: `${base_package}.exception`.

---

## 3. GLOBAL EXCEPTION HANDLER

- Create a class such as `GlobalExceptionHandler`
  in `${base_package}.exception`.
- Annotate with `@RestControllerAdvice` or `@ControllerAdvice`.
- Provide `@ExceptionHandler` methods for:
    - `ResourceNotFoundException` → return 404
    - Generic `Exception` → return 500
    - Any other domain-specific exceptions.
- Return a structured JSON error body, including fields like:
    - `timestamp`
    - `status`
    - `error`
    - `message`
    - `path`

You may use Spring’s `ProblemDetail` in Spring Boot 3.x
or a custom error response class.

---

## 4. OUTPUT FORMAT

Output a JSON object:

- Keys: relative file paths
- Values: full Java source code

Example key style:

- `"src/main/java/com/example/studentapi/exception/ResourceNotFoundException.java"`
- `"src/main/java/com/example/studentapi/exception/GlobalExceptionHandler.java"`

No extra explanation.

---

## 5. ACTION

1. Create base and domain-specific exceptions.
2. Create a global exception handler with appropriate mappings.
3. Output exception classes and handler as JSON
   `relativePath -> sourceCode`.

---

#####################################################################
### PLATFORM-LEVEL ADDITIONS – DO NOT REMOVE OR MODIFY
#####################################################################

## 6. PARENT PLATFORM CONSTRAINTS (CRITICAL)

This Child application runs **ON TOP OF a Parent Platform JAR**.

⚠️ The Parent Platform ALREADY provides:
- Base `ApiException`
- `ErrorCode` enum
- `GlobalExceptionHandler`
- Unified error response format

### Therefore, in the Child Prompt Hub:

❌ **You MUST NOT generate a GlobalExceptionHandler**
❌ **You MUST NOT create a new base exception**
❌ **You MUST NOT define error response structures**

---

## 7. CHILD EXCEPTION RESPONSIBILITY (MANDATORY)

The Child Prompt Hub is responsible for:

- **Domain-specific exceptions ONLY**
- Exceptions that represent business failures
- Exceptions that extend **Parent `ApiException`**

### Required rules:

- Every child exception MUST:
    - Extend Parent `ApiException`
    - Use Parent `ErrorCode`
- One exception per meaningful business failure

---

## 8. STANDARD EXCEPTION PATTERN (REFERENCE – FOLLOW THIS STYLE)

```java
public class StudentNotFoundException extends ApiException {

    public StudentNotFoundException(Long id) {
        super(ErrorCode.RESOURCE_NOT_FOUND,
              "Student not found with id: " + id);
    }
}
```

---

## 9. JAVA & DESIGN STANDARDS (PLATFORM-ENFORCED)

Child exceptions MUST:

- Be immutable
- Contain minimal logic
- Not perform logging
- Not catch or wrap other exceptions unnecessarily
- Follow Google Java naming conventions

Child exceptions MUST NOT:

- Reference HTTP classes
- Contain `@ResponseStatus`
- Handle serialization
- Perform any side effects

---

## 10. FINAL ENFORCEMENT

If a generated exception file:

- Defines `@ControllerAdvice`
- Defines a global exception handler
- Does not extend Parent ApiException
- Uses custom error payloads

➡️ The output is INVALID and MUST be regenerated.
