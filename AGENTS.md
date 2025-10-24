# AGENTS.md - Guide for AI Agents

## Repository Overview

This repository contains **Pon's comprehensive API and software development guidelines**, a derivative work based on Zalando's RESTful API guidelines. The documentation is written in AsciiDoc format and compiled into HTML and PDF outputs.

**License**: CC-BY 4.0 (Creative Commons Attribution 4.0)

## Project Structure

### Documentation Source
- **`index.adoc`**: Main entry point with formatting, RFC links, and styling attributes
- **`chapters/`**: All guideline content organized by topic
  - `introduction.adoc` - Software engineering philosophy
  - `general-guidelines.adoc` - API-first principles
  - `api-operation.adoc` - Publishing and monitoring APIs
  - `resources.adoc` - REST resource design patterns
  - `security.adoc` - OAuth 2.0 and endpoint security
  - `networking/` - HTTP requests and status codes
  - `integration/` - Design principles, events, solution design
  - `development/` - Code quality, testing, version control
  - `data-formats/` - JSON guidelines
  - `generic/` - Architecture, quality, privacy

### Build System
- **`build.sh`**: Main build script using Docker/Asciidoctor
- **`build_css.sh`**: CSS generation from SASS
- **`check_rule_ids.sh`**: Validates unique rule IDs
- **`new_rule_id.sh`**: Generates new unique rule IDs
- **`generate_rules_json.sh`**: Extracts rules to JSON format

### Output
- **`output/`**: Generated HTML and PDF files (created by build)
- **`legacy/`**: Legacy HTML files for backward compatibility

## Key Concepts

### Rule System
Each guideline has a **unique, immutable Rule ID** in the format `[#NNN]` (e.g., `[#100]`, `[#192]`).

**Rule ID Requirements**:
- Must be unique across all chapters
- Format: `[#<number>]` exactly (no spaces)
- Once assigned, IDs are durable and don't change unless content changes significantly
- Use `./new_rule_id.sh` to generate new IDs
- Use `./check_rule_ids.sh` to validate uniqueness

### Requirement Levels
Guidelines use RFC 2119 keywords:
- **{MUST}**: Mandatory requirements
- **{SHOULD}**: Strong recommendations
- **{MAY}**: Optional suggestions
- **{STATUS-RFP}**: Request for Proposal status

### Guild Markers
- **{INTEGRATION-GUILD}**: 🔄 Integration guild guidelines
- **{DEVELOPMENT-GUILD}**: ⚙️ Development guild guidelines

## Building the Documentation

### Using Docker (Preferred)
```bash
./build.sh
```

This will:
1. Validate rule IDs are unique
2. Pull `asciidoctor/docker-asciidoctor` image
3. Generate HTML: `output/index.html`
4. Generate PDF: `output/pon-guidelines.pdf`
5. Copy assets and models

### Manual Build
```bash
gem install asciidoctor asciidoctor-pdf
asciidoctor -D output index.adoc
asciidoctor-pdf -D output index.adoc
```

## Making Changes

### Adding New Guidelines

1. **Choose the appropriate chapter** in `chapters/`
2. **Generate a new rule ID**:
   ```bash
   ./new_rule_id.sh
   ```
3. **Add the guideline** following this format:
   ```asciidoc
   [#XXX]
   == {MUST|SHOULD|MAY} descriptive title
   
   Explanation of the guideline...
   
   [source,yaml]
   ----
   # Code example if needed
   ----
   ```
4. **Validate rule IDs**:
   ```bash
   ./check_rule_ids.sh
   ```

### Modifying Existing Guidelines

- **DO NOT change Rule IDs** unless the content changes significantly
- Maintain backward compatibility with references
- Update the `chapters/changelog.adoc` for major changes
- Include code examples where helpful
- Reference RFC standards using predefined attributes (e.g., `:RFC-7231:`)

### Pull Request Requirements

From `CONTRIBUTING.adoc`:
- **DON'T push to master directly** - always use pull requests
- Get at least **2 reviewer approvals** before merging
- Add **changelog entry** for major changes
- Ensure **rule IDs are unique** (use `check_rule_ids.sh`)
- Consider adding the check script as a pre-commit hook:
  ```bash
  cp check_rule_ids.sh .git/hooks/pre-commit
  ```

## Common Patterns

### Cross-referencing
- Internal links: `<<rule-id>>` or `<<rule-id, link text>>`
- Section references: `<<section-anchor>>`
- HTTP methods: Use predefined attributes like `{GET}`, `{POST}`, `{PUT}`

### Code Examples
```asciidoc
[source,http]
----
GET /api/v1/resources
----

[source,yaml]
----
openapi: 3.0.0
----

[source,json]
----
{
  "example": "value"
}
----
```

### Important Callouts
```asciidoc
IMPORTANT: Critical information

[TIP]
====
Helpful tip content
====

[NOTE]
====
Additional context
====

[WARNING]
====
Warning message
====
```

## File Naming Conventions

- **AsciiDoc files**: `kebab-case.adoc`
- **Shell scripts**: `snake_case.sh`
- **YAML models**: `kebab-case-version.yaml` (e.g., `money-1.0.0.yaml`)

## Testing Your Changes

1. **Validate syntax**: Run `./check_rule_ids.sh`
2. **Build locally**: Run `./build.sh`
3. **Review output**: Check `output/index.html` in a browser
4. **Check links**: Verify all cross-references work
5. **Validate examples**: Ensure code examples are correct

## Special Attributes

The `index.adoc` defines many reusable attributes:
- RFC links: `:RFC-XXXX:` (e.g., `:RFC-7231:`)
- Standards: `:ISO-8601:`, `:ISO-3166-1-a2:`, `:BCP47:`
- Keywords: `:MUST:`, `:SHOULD:`, `:MAY:`
- HTTP methods: `:GET:`, `:POST:`, `:PUT:`, etc.
- Icons: `:LANG-JAVASCRIPT-ICON:`, `:LANG-GO-ICON:`, etc.

## Output Locations

After building:
- **HTML**: `output/index.html`
- **PDF**: `output/pon-guidelines.pdf`
- **Assets**: `output/assets/`
- **Models**: `output/money-1.0.0.yaml`
- **Rules JSON**: `output/rules.json` (machine-readable)

## Target Audience

These guidelines are intended for:
- **API developers** designing RESTful APIs
- **Integration engineers** building system integrations
- **Software developers** following Pon's coding standards
- **Architects** making design decisions
- **Technical reviewers** evaluating API specifications

## Key Guidelines to Remember

### API Design
- **Rule #100**: Follow API-first principle (define before implementing)
- **Rule #101**: Provide API specification (OpenAPI/Swagger)
- **Rule #138**: Avoid actions, think about resources
- **Rule #141**: Keep URLs verb-free

### Security
- **Rule #104**: Secure all endpoints (prefer OAuth 2.0)
- **Rule #105**: Define and assign permissions/scopes

### Operations
- **Rule #192**: Publish OpenAPI specification
- **Rule #193**: Monitor API usage

### Development
- **Rule #244**: Write logically structured code
- **Rule #248**: Focus on code quality (functional + structural)
- **Rule #254**: Keep code simple and concise

## Workflow for AI Agents

When working with this repository:

1. **Understanding context**: Read relevant `.adoc` files in `chapters/`
2. **Adding guidelines**: Generate rule ID, add content, validate
3. **Modifying content**: Preserve rule IDs, update changelog if major
4. **Building**: Run `./build.sh` to verify changes
5. **Quality checks**: Validate rule IDs, check cross-references
6. **Documentation**: Focus on clarity, examples, and standards references

## Shell Environment

- **Default shell**: WSL (Windows Subsystem for Linux)
- **Script format**: Bash scripts with `#!/usr/bin/env bash`
- **Docker support**: Preferred method for building

## Additional Resources

- **BUILD.adoc**: Technical build documentation
- **CONTRIBUTING.adoc**: Contribution guidelines
- **MAINTAINERS**: List of project maintainers
- **LICENSE**: CC-BY-SA 4.0 license terms

---

**Last Updated**: October 24, 2025
**Repository**: pondevelopment/restful-api-guidelines
**Branch**: master
