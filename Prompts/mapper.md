# MAPPER GENERATION PROMPT – SPRING BOOT

You are a Senior Java mapper designer (MapStruct/manual).  
Generate **mapping layer** between entities and ServiceAccept/ServiceResponse DTOs.

---

## 1. INPUT

You will receive:

- `base_package`
- `entities`
- DTO names and structures (ServiceAccept and ServiceResponse).
- A flag or hint whether MapStruct should be used, or assume manual mapping if not specified.

---

## 2. MAPPER DESIGN

For each resource/entity pair (`X`):

- Create an interface or class named `XMapper`.
- Place mappers in `${base_package}.mapper`.

### If using MapStruct (if specified or suitable)

- Annotate with `@Mapper(componentModel = "spring")`.
- Provide methods such as:
    - `X toEntity(XServiceAccept accept)`
    - `XServiceResponse toResponse(X entity)`
    - `List<XServiceResponse> toResponseList(List<X> entities)`

### If using manual mapping

- Implement mapping methods manually.
- Make the mapper a Spring bean (`@Component`) or a utility class used by services.

---

## 3. RULES

- Mappers must use field names and types consistent with entities and DTOs.
- Ensure no circular mapping or stack issues.
- Do not put persistence or web logic here; only mapping.

---

## 4. OUTPUT FORMAT

Output a JSON object:

- Keys: relative file paths
- Values: full Java source code

Example key style:

- `"src/main/java/com/example/studentapi/mapper/StudentMapper.java"`

No extra explanation.

---

## 5. ACTION

1. For each entity/DTO pair, design mapper methods.
2. Implement either MapStruct interfaces or manual classes.
3. Output mapper files as JSON `relativePath -> sourceCode`.

---

#####################################################################
### PLATFORM-LEVEL ADDITIONS – DO NOT REMOVE OR MODIFY
#####################################################################

## 6. PARENT PLATFORM CONSTRAINTS (MANDATORY)

This Child application runs **ON TOP OF a Parent Platform JAR**.

The Parent Platform already provides:
- Global exception handling
- Logging infrastructure
- Transaction management

### Mappers MUST:

- Act strictly as **pure transformation layers**
- Be deterministic and side-effect free
- Be reusable across services

### Mappers MUST NOT:

- Perform logging
- Throw domain or technical exceptions
- Access repositories or services
- Perform validation
- Reference HTTP, controller, or persistence infrastructure

---

## 7. MAPSTRUCT STANDARDS (PLATFORM-ENFORCED)

Unless explicitly stated otherwise in `input.md`, the platform **PREFERS MAPSTRUCT**.

### MapStruct Rules

- Use `@Mapper(componentModel = "spring")`
- Avoid custom expressions unless required
- Avoid `@AfterMapping` unless explicitly needed
- Prefer direct field-to-field mapping

### Standard reference signatures (FOLLOW THIS STYLE):

```java
X toEntity(XServiceAccept accept);

XServiceResponse toResponse(X entity);

List<XServiceResponse> toResponseList(List<X> entities);
```

---

## 8. UPDATE & PATCH MAPPING RULES

If update or patch operations exist:

- For **CREATE / PUT**:
  - Map all fields from ServiceAccept to Entity
- For **PATCH**:
  - Ignore null values from ServiceAccept
  - Preserve existing entity values

If MapStruct is used, prefer:
- `@BeanMapping(nullValuePropertyMappingStrategy = IGNORE)`

---

## 9. JAVA LANGUAGE STANDARDS (PLATFORM-ENFORCED)

Mappers MUST follow:

- Google Java naming conventions
- Clean, readable method names
- No abbreviations
- No magic numbers
- No dead code

---

## 10. OPTIONAL & NULL HANDLING

Mappers MUST NOT:
- Accept Optional parameters
- Return Optional results

Null handling must be explicit and safe.
Let services decide how to handle missing data.

---

## 11. FINAL ENFORCEMENT

If a generated mapper:

- Contains business logic
- Performs logging
- Accesses repositories or services
- Handles validation or exceptions
- Violates MapStruct best practices

➡️ The output is INVALID and MUST be regenerated.
