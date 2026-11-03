# AGENTS.md

## Workflow

- Always read this `AGENTS.md` file before making changes.
- Do not rebuild the project after making changes unless explicitly requested.
- Always reformat the file after editing.
- Always leave an empty line before a `return` statement.

## Module Shape

- Common subpackages are: `controller`, `service`, `repository`, `dto`, `mapper`, `model`.

## Naming And CRUD

### Naming

- Use `camelCase` consistently for naming:
    - Files
    - Variables
    - Functions and methods
    - Directories
    - Paths
    - Project-defined identifiers

- Avoid `snake_case`, `kebab-case`, and other naming styles unless required by the language, framework, library, operating system, or an external API.

- When modifying existing code, follow established framework or project conventions if they conflict with the general `camelCase` rule.

- Keep names descriptive, concise, and consistent with the surrounding codebase.

#### Examples

- userService.java
- userRepository.java
- getUserDetails()
- currentUser
- userProfile/
- userProfile/settings/

### CRUD

- Follow CRUD standards for naming, method ordering, and CRUD implementation.
- Do not force read-first method ordering.
- When ordering controller and service methods by CRUD, follow true CRUD order: `create`, `read`, `update`, `delete`, then place filter, batch, or other custom endpoints after the core CRUD methods.
- Group all read operations together, for example `findAll`, `findById`, `filter`, and other query-style reads should stay adjacent in the read section.

## API Rules

- Controller endpoints should follow CRUD order.
- `create` endpoints should accept data through `@RequestBody`.
- `edit` endpoints should use the entity id as `@PathVariable` and accept the update payload through `@RequestBody`.
- `delete` endpoints should use the entity id as `@PathVariable`.

## DTOs

- Prefer `record` for DTOs to keep data-transfer objects immutable and concise. Use regular classes only when mutability or framework requirements make them necessary.
- Apply Jakarta Bean Validation annotations to request DTO fields whenever applicable.
- Use the validation annotation that best represents the field constraint, such as `@NotNull`, `@NotBlank`, `@NotEmpty`, `@Size`, `@Email`, `@Positive`, or other appropriate constraints.
- Perform basic input validation at the DTO boundary instead of duplicating null, empty, format, or range checks in the service layer.
- Keep business rules and domain-specific validation in the service or domain layer.

## Models

- Add `schema` to the `@Table` tag on model classes.
- Use `@EqualsAndHashCode(onlyExplicitlyIncluded = true, callSuper = false)` and explicitly include the relevant field(s).
- If a model contains fields such as `lastUpdate`, check whether `@PrePersist` and `@PreUpdate` exist and populate those fields with the corresponding values.
- For entities that use `@Version`, initialize the Java-side field on create when needed (for example in `@PrePersist` set it to `0L` if null), and ensure the database column also has a default value of `0` so manual SQL inserts do not break optimistic locking.
- Verify optimistic locking explicitly: entities should use the `@Version` annotation, not just a plain `version` field mapped with `@Column`.
- Every entity that uses `@Version` must have a `@PrePersist` implementation that initializes the version value when it is null.

## Mappers

- Use MapStruct mappers with `componentModel = "spring"`.

## Services And Helpers

- Service layer is made of an interface and an implementation class for better readability and maintainability.
- Prefer Lombok constructor annotations where appropriate:
    - `@NoArgsConstructor`
    - `@AllArgsConstructor`
    - `@RequiredArgsConstructor`
- Extract helper methods such as `getEntity` when the same code is duplicated, used in two or more places, or when a larger block should be separated for readability.
- Add Javadocs where needed, especially on helper methods and non-obvious logic.
- Do not name local error-message constants with implementation wording such as `..._MESSAGE_PREFIX`.
- Prefer names such as `TIP_NA_RAB_USPESNOST_NOT_FOUND` instead of `TIP_NA_RAB_USPESNOST_NOT_FOUND_MESSAGE_PREFIX`.
- Error-message constant values should be personalized business messages, for example `Tip na Rabotna Uspesnost with id ${id} not found.`, instead of generic messages such as `No record found with id:`.
- Messages should be written in English.

## Commits And Messages

- Before creating a commit, propose the exact commit message to the user and ask whether it is good or should be changed.
- Do not commit until the user confirms the commit message.
- Before committing, remove false-positive/no-op file changes from the working tree after verifying they contain no real user changes.
- Follow Conventional Commits format: `<type>(optional-scope): subject`.
- Allowed types: `feat`, `fix`, `refactor`, `chore`, `perf`, `ci`, `build`, `docs`, `style`, `revert`, `test`.
- Write subject lines in imperative mood (for example: `fix(vlezOsnovniSredstva): center action buttons in last column`).
- Keep subject concise (target 50 chars, max 72), start with lowercase after `type:`, and do not end with a period.
- Add a body when needed, separated by one blank line, wrapped near 72 chars.
- In the body, explain **why** the change was made and any side effects; avoid vague messages such as `tweaked a few things`.
- Keep one logical change per commit so history is easy to search, review, and revert.