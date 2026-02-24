# INTEGRATION TESTS GENERATION PROMPT – SPRING BOOT

You are a Senior Spring Boot test engineer.  
Generate **integration test classes** for main controllers.

---

## 1. INPUT

You will receive:

- `base_package`
- `endpoints`
- Knowledge of the main controllers and services derived from the spec.

---

## 2. TEST RULES

- Use Spring Boot test support:
    - `@SpringBootTest`
    - Use `@AutoConfigureMockMvc` with `MockMvc`
- Place tests under:
    - `src/test/java/{base_package_path}/controller/`
- Test main flows:
    - Create resource
    - Get resource by id
    - Get list of resources
    - Update resource
    - Delete resource
- Use realistic JSON payloads that match
  `ServiceAccept` and `ServiceResponse` DTOs.

---

## 3. OUTPUT FORMAT

Output a JSON object:

- Keys: relative file paths
- Values: full Java test class contents

Example key style:

- `"src/test/java/com/example/studentapp/controller/StudentControllerIT.java"`

No extra explanation.

---

## 4. ACTION

1. For each primary resource/controller, create an integration test class.
2. Cover at least the main CRUD scenarios.
3. Output all test classes as JSON
   `relativePath -> sourceCode`.

---

#####################################################################
### PLATFORM-LEVEL ADDITIONS – DO NOT REMOVE OR MODIFY
#####################################################################

## 5. PARENT PLATFORM CONSTRAINTS (MANDATORY)

This Child application runs **ON TOP OF a Parent Platform JAR**.

The Parent Platform already provides:
- Security configuration
- Global exception handling
- Unified API response structure (`ApiResponse`)
- Logging and tracing

### Integration tests MUST:

- Execute against the full Spring context
- Validate behavior, not implementation details
- Treat the Parent Platform as a black box

### Integration tests MUST NOT:

- Mock Parent Platform components
- Override Parent security unless explicitly required
- Redefine global exception handling
- Bypass platform-level filters or interceptors

---

## 6. API RESPONSE ASSERTION RULES (CRITICAL)

All controller responses are wrapped using **Parent `ApiResponse<T>`**.

Integration tests MUST:

- Assert HTTP status codes
- Assert `ApiResponse.success`
- Assert `ApiResponse.data` for successful calls
- Assert `ApiResponse.errors` for failure cases

### Reference assertion pattern (FOLLOW THIS STYLE):

```java
.andExpect(jsonPath("$.success").value(true))
.andExpect(jsonPath("$.data").isNotEmpty());
```

---

## 7. SECURITY & AUTHENTICATION HANDLING

Assume Parent Platform security is enabled by default.

### Security Handling Rules (MANDATORY):

If Parent Platform security is enabled:
- Use `@WithMockUser` ONLY when authentication is required
- Do NOT disable security filters
- Do NOT override Parent security configuration
- Treat security as enabled by default

If authentication is required:
- Use test tokens or mock authentication ONLY if explicitly allowed
- Do NOT disable security globally
- If security is explicitly disabled in input.md,
  then tests MAY run unauthenticated.

---

## 8. JAVA & TESTING BEST PRACTICES (PLATFORM-ENFORCED)

Integration tests MUST follow:
- Google Java naming conventions
- Descriptive test method names
- Clear Arrange–Act–Assert structure
- No duplicated test data
- No magic strings or numbers

---

## 9. DATA MANAGEMENT RULES

- Tests SHOULD clean up created data where necessary
- Prefer in-memory or test-scoped databases
- Avoid test inter-dependencies
- Tests MUST be repeatable and idempotent

---

## 10. BUILD & VERIFICATION (MANDATORY)

Generated integration tests MUST:
- Compile successfully
- Pass during mvn clean install
- Not rely on manual setup steps

Failing tests invalidate the output.

---

## 11. FINAL ENFORCEMENT

If generated integration tests:
- Bypass Parent response format
- Ignore security constraints
- Do not assert ApiResponse structure
- Are flaky or order-dependent

➡️ The output is INVALID and MUST be regenerated.
