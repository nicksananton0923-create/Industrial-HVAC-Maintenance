# Contributing

Thank you for your interest in improving this framework. This document describes how to propose changes, what kinds of changes are welcome, and what review the maintainers will apply before merging.

## 1. Scope of Contributions

This framework is a **vendor-neutral, OEM-agnostic** documentation system. Contributions are welcome in the following categories:

- **New templates** that fill gaps in the current set (e.g. a boiler inspection template, a humidity-control unit template, a VFD inspection template).
- **Improved documentation** that clarifies procedures, adds examples, or corrects errors.
- **Translations** of the documentation into other languages.
- **Diagram improvements** — better PNG schematics, additional Mermaid diagrams, correction of inaccuracies.
- **Regulatory updates** — new or updated regulatory citations (e.g. when ASHRAE 15 is revised, when EU F-Gas amendments come into force).
- **Site-specific addenda** shared for the benefit of the community (with site-identifying information redacted).

The following categories are **out of scope**:

- Equipment-specific operating limits (these belong in OEM manuals, not in this framework).
- Proprietary procedures owned by a service contractor.
- Site-specific data (real readings, real fault history, real asset registers).
- Software code that processes the templates (this framework is documentation, not a software project — companion tools are welcome as separate repositories).

## 2. Before You Start

If your change is non-trivial (a new template, a new procedure, a regulatory update), please open an issue first to discuss. The maintainers may have context that affects the design, and an open issue ensures the change is aligned with the framework's direction before significant work is invested.

For trivial changes (typo fixes, formatting, documentation clarifications), open a pull request directly.

## 3. Template Change Policy

The templates are the contract between this framework and its users. Changes to templates must preserve backward compatibility where possible:

- **Adding a column** is acceptable. New columns are added at the end of the header row; existing rows are valid in the new template (the new column is blank for existing rows). Document the addition in the CHANGELOG.
- **Renaming a column** is a breaking change. Open an issue first. If the rename is approved, the migration procedure in `recordkeeping-guide.md` Section 7 must be followed.
- **Removing a column** is a breaking change. Same as rename.
- **Changing the meaning of a column's values** (e.g. changing `severity` from `low/medium/high/critical` to `1/2/3/4`) is a breaking change. Same as rename.

## 4. Documentation Change Policy

Documentation changes are non-breaking. The maintainers will review for:

- **Technical accuracy** — does the proposed text correctly describe the procedure, standard, or concept?
- **Consistency** — does the proposed text align with the rest of the documentation?
- **Completeness** — does the proposed text stand alone, or does it require context that is missing?
- **Citations** — does the proposed text cite a standard or reputable source for any new procedure or limit?

## 5. Pull Request Process

1. Fork the repository.
2. Create a feature branch (`feature/<short-description>`).
3. Make your changes. Commit with clear, atomic commits (`Add boiler inspection template`, not `updates`).
4. Update the CHANGELOG with a one-line entry under the next version.
5. Open a pull request. Describe:
   - The problem being solved.
   - The change made.
   - The alternatives considered.
   - The testing performed (e.g. CSV validated, Mermaid rendered, procedure reviewed by a qualified engineer).
6. Respond to review feedback. Be patient — reviewers are volunteers.

## 6. Review Criteria

The maintainers will review against:

- **Safety** — does the change introduce any unsafe procedure or remove any safety-relevant warning?
- **Regulatory accuracy** — are cited standards current and correctly named?
- **OEM neutrality** — does the change avoid asserting equipment-specific limits or vendor-specific procedures?
- **Backward compatibility** — does the change preserve the integrity of existing templates?
- **Documentation quality** — does the change meet the content depth and clarity standards of the existing documentation?

If the change is not approved, the maintainers will explain why and suggest a path to approval.

## 7. Licensing

By contributing, you agree that your contributions are licensed under the MIT License, the same license as the rest of the framework. You confirm that you have the right to license your contributions under MIT (i.e. you are the original author, or you have permission from the copyright holder).

## 8. Code of Conduct

Be respectful. Be specific. Be patient. The framework exists to make HVAC maintenance safer and more consistent; contributions that advance that goal are welcome. Contributions that detract from it (spam, off-topic, abusive, or commercially promotional) are not.

## 9. Attribution

Contributors are acknowledged in the CHANGELOG by their GitHub username (or equivalent) and in any release notes. Contributors who make significant recurring contributions may be invited to join the maintainer team.
