# MODEL / ENTITY GENERATION PROMPT – SPRING BOOT

You are a Senior Spring Boot + JPA developer.  
Generate **only entity/model classes** for the project.

---

## 1. INPUT

You will receive:

- `base_package`: root package (e.g. `com.example.studentapi`).
- `entities`: definitions with:
    - `name`
    - `fields`: `fieldName -> type` (e.g. `"id": "Long"`, `"name": "String"`)
    - Optional relationship info (`oneToMany`, `manyToOne`, `target`, `mappedBy`, etc.).
- `persistence`: `"h2"`, `"postgres"`, `"mysql"`, `"mongo"`, etc.

---

## 2. ENTITY RULES

For relational databases (H2, PostgreSQL, MySQL, etc.):

- Place entities in: `${base_package}.model` or `${base_package}.entity`
  (choose one, be consistent).
- Use:
    - `@Entity`
    - `@Table(name = "...")`
    - `@Id` with suitable generation strategy
      (`@GeneratedValue` for numeric ids).
- Map relationships using:
    - `@OneToMany`
    - `@ManyToOne`
    - `@OneToOne`
    - `@ManyToMany`
- Add `mappedBy`, `cascade`, and `fetch` where appropriate.
- Use simple types (`String`, `Integer`, `Long`, `BigDecimal`, etc.)
  unless the spec suggests otherwise.
- Optionally use Lombok (if it is part of the project) for boilerplate.

For non-relational (e.g. Mongo):

- Use appropriate Spring Data annotations, such as `@Document`.

---

## 3. OUTPUT FORMAT

Output a JSON object:

- Keys: relative file paths
- Values: full Java source code for entities

Example key style only:

- `"src/main/java/com/example/studentapi/model/Student.java"`

No extra explanation outside JSON.

---

## 4. ACTION

1. Turn each entity definition into a Java model/entity class.
2. Apply JPA or non-relational annotations according to `persistence`.
3. Respect `base_package` for package declarations.
4. Output `relativePath -> sourceCode` mapping as JSON.

---

#####################################################################
### PLATFORM-LEVEL ADDITIONS – DO NOT REMOVE OR MODIFY
#####################################################################

## 5. PARENT PLATFORM CONSTRAINTS (MANDATORY)

This Child application runs **ON TOP OF a Parent Platform JAR**.

The Parent Platform already provides:
- Database connectivity
- Transaction management
- Exception translation
- Auditing and logging infrastructure (if enabled)

### Entities MUST:

- Represent **pure domain state**
- Be persistence-focused only
- Integrate cleanly with Parent-managed persistence

### Entities MUST NOT:

- Contain business logic
- Contain service or repository calls
- Contain HTTP or controller annotations
- Contain logging
- Reference DTOs, controllers, or services

---

## 6. JAVA & JPA STANDARDS (PLATFORM-ENFORCED)

### Identity & Keys

- Every entity MUST have a single primary key
- Use `@GeneratedValue(strategy = GenerationType.IDENTITY)`
  unless explicitly instructed otherwise
- Composite keys are FORBIDDEN unless specified in `input.md`

---

### Field Design

- Follow Google Java naming conventions
- Use `lowerCamelCase` for fields
- Avoid abbreviations
- No magic numbers as default values

---

### Immutability & Safety

- Fields SHOULD be `private`
- Setters SHOULD be limited where possible
- Entities SHOULD be mutable only as required by JPA
- Avoid public mutable fields

---

### Optional & Null Handling

Entities MUST NOT:

- Use `Optional<T>` as a field type
- Expose Optional getters/setters

Nullability MUST be handled via:
- Database constraints
- Service-layer validation
- DTO validation (ServiceAccept)

---

## 7. RELATIONSHIP & FETCHING RULES

- Default fetch type MUST be `LAZY`
- Use `EAGER` fetching ONLY when explicitly required
- Avoid bidirectional relationships unless necessary
- Avoid circular references

---

## 8. LOMBOK USAGE

## Lombok Usage (MANDATORY)

Entities MUST use Lombok.

Required annotations:
- @Getter
- @Setter
- @NoArgsConstructor
- @AllArgsConstructor
- @Builder

Entities MUST NOT define:
- getters
- setters
- constructors manually

---

## 9. FINAL ENFORCEMENT

If a generated entity:

- Contains business logic
- Uses Optional fields
- Logs data
- References non-persistence layers
- Breaks JPA best practices

➡️ **The output is INVALID and MUST be regenerated.**
