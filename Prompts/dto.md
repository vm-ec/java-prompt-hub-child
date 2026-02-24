# DTO GENERATION PROMPT – SERVICEACCEPT & SERVICERESPONSE

You are a Senior Spring Boot developer.  
Generate **DTO classes used as ServiceAccept and ServiceResponse** for each main resource.

---

## 1. INPUT

You will receive:

- `base_package`
- `entities` with fields.
- `endpoints` (optional, to infer which fields should be in DTOs).
- Any additional hints about fields that should be hidden or internal.

---

## 2. DTO NAMING & PACKAGES

- DTOs live under `${base_package}.dto`.
- For each exposed entity/resource (e.g. `Student`), create:
  - `StudentServiceAccept`
  - `StudentServiceResponse`

You may split packages further if useful:

- `${base_package}.dto.accept`
- `${base_package}.dto.response`

But a flat `${base_package}.dto` is also acceptable; stay consistent.

---

## 3. DTO RULES

- DTOs are plain Java classes.
- **Do NOT add validation annotations** such as:
  - `@NotNull`, `@NotBlank`, `@Size`, etc.
- Use Jackson annotations where needed:
  - Use `@JsonIgnore` on fields that should not be serialized to the client or not accepted from the client.
    - For example internal IDs, auditing fields, internal flags, or anything clearly not meant to be exposed.
- DTO fields should reflect the data needed for:
  - Service input (ServiceAccept)
  - Service output (ServiceResponse)

The service layer will:

- Accept `XxxServiceAccept` as method arguments for create/update operations.
- Return `XxxServiceResponse` from service methods to controller.

---

## 4. OUTPUT FORMAT

Output a JSON object:

- Keys: relative file paths
- Values: full Java source code for DTOs

Example key style:

- `"src/main/java/com/example/studentapi/dto/StudentServiceAccept.java"`
- `"src/main/java/com/example/studentapi/dto/StudentServiceResponse.java"`

No extra explanation.

---

## 5. ACTION

1. For each main resource, design ServiceAccept and ServiceResponse DTOs.
2. Decide where `@JsonIgnore` is appropriate for internal/non-exposed fields.
3. Output all DTO files as JSON `relativePath -> sourceCode`.

---

#####################################################################
### PLATFORM-LEVEL ADDITIONS – DO NOT REMOVE OR MODIFY
#####################################################################

## 6. PARENT PLATFORM CONSTRAINTS (MANDATORY)

This Child application runs **ON TOP OF a Parent Platform JAR**.

The Parent Platform already provides:
- Global exception handling
- Unified API error responses
- Validation error handling

### DTOs MUST:

- Act strictly as **data carriers**
- Be independent of HTTP, persistence, and logging concerns
- Work seamlessly with Parent validation and error handling

### DTOs MUST NOT:

- Reference `ApiResponse`
- Reference `ResponseEntity`
- Contain exception handling logic
- Contain logging
- Reference repositories or entities directly

---

## 7. INPUT VALIDATION RULES (PLATFORM STANDARD)

> ⚠️ This section **ADDS** rules on top of section 3.  
> It does NOT remove or invalidate any existing instruction.

### ServiceAccept DTOs (INPUT)

ServiceAccept DTOs **MAY and SHOULD** include JSR-380 validation annotations
to enforce input correctness at the API boundary.

Allowed annotations include (but are not limited to):

- `@NotNull`
- `@NotBlank`
- `@Size`
- `@Email`
- `@Pattern`
- `@Min`, `@Max`

Validation annotations MUST:

- Be placed **only on ServiceAccept DTOs**
- Represent **business input constraints**
- Contain clear, user-readable messages

#### Standard syntax (REFERENCE – follow this style):

```java
@NotBlank(message = "Name is required")
private String name;
```

---

## 8. JAVA LANGUAGE STANDARDS (PLATFORM-ENFORCED)

### Naming & Structure

- Follow Google Java naming conventions
- Class names in **UpperCamelCase**
- Field names in **lowerCamelCase**
- Constants (if any) in **UPPER_SNAKE_CASE**

### Immutability & Design

- DTOs **SHOULD** be immutable where practical
- Fields **MAY** be `final` if constructor-based initialization is used
- Avoid unnecessary setters

### Lombok Usage

## Lombok Usage (MANDATORY)

Lombok is ENABLED by platform contract.

DTOs MUST use Lombok and MUST NOT define:

- getters
- setters
- constructors

### ServiceAccept DTOs MUST use:
- @Data
- @Builder
- @NoArgsConstructor
- @AllArgsConstructor

### ServiceResponse DTOs MUST use:
- @Data
- @Builder
- @NoArgsConstructor
- @AllArgsConstructor

Manual boilerplate code is STRICTLY FORBIDDEN.


DTOs **MUST NOT**:
- Contain complex constructors
- Contain derived or computed fields unless explicitly required

### Optional & Null Handling

DTOs **MUST NOT**:
- Use `Optional<T>` as a field type
- Expose nullable semantics via `Optional`

Nullability is handled via:
- Validation annotations (**ServiceAccept**)
- Parent Platform exception handling

---

## 9. SERIALIZATION RULES

- Use Jackson annotations **ONLY** when necessary
- Avoid custom serializers/deserializers
- Ensure DTOs serialize cleanly to JSON

---

## 10. FINAL ENFORCEMENT

If a generated DTO:

- Contains business logic
- Contains logging
- Uses Optional fields
- Mixes validation into ServiceResponse
- References infrastructure concerns

➡️ The output is INVALID and MUST be regenerated
