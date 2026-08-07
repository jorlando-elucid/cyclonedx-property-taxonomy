# `cdx:cisa` Namespace Taxonomy

This is the namespace for official CycloneDX properties carrying SBOM data fields defined in the *2026 Minimum Elements for a Software Bill of Materials (SBOM)*, published July 29, 2026 by CISA and co-authoring organizations (referred to below as "the 2026 Minimum Elements"), which replaces the 2021 NTIA minimum elements. Only fields with no native CycloneDX field are defined here.

The official rules and processes apply - see [parent document](../cdx.md).

Value convention: where a property value names a data field, the name is the element name exactly as listed in Appendix A of the 2026 Minimum Elements (e.g. `Component Version`, `Component Producer`, `Component Hash Value`). Properties are repeatable unless stated otherwise. The key words MUST, SHOULD, and MAY are to be interpreted as described in RFC 2119.

Scope notes: unknowns in the dependency graph itself are declared natively via `compositions` (aggregate `unknown` or `incomplete`). Where the native field for a data element is an unconstrained string (e.g. `components[].version`, `components[].manufacturer.name`), unknown values SHOULD be expressed as the literal `unknown` in the native field itself, per the 2026 Minimum Elements' fallback-designation approach for unknown component producers. The properties below address what cannot be expressed that way: statements about format-constrained or enumerated fields, and the distinction between information that is unknown and information that is deliberately withheld.

----

| Property | Description |
|----------|-------------|
| `cdx:cisa:unknown-information` | Explicit statement that "the information is unknown to the SBOM author" (2026 Minimum Elements, *Explicitly Identifying Unknown Information*) for a data field whose native CycloneDX field is format-constrained or enumerated and therefore cannot carry a literal `unknown` (for example `Component Hash Value`, whose native field is regex-validated hexadecimal). Value: the Appendix A element name, one per property instance. Attach at the level the statement applies to: `metadata.properties` for SBOM Metadata elements, `components[].properties` for Component Data elements. For unconstrained string fields, use the literal `unknown` in the native field instead. |
| `cdx:cisa:withheld-information` | Explicit statement that "the SBOM author is withholding the information from the SBOM" (2026 Minimum Elements, *Explicitly Identifying Unknown Information*). Value: the Appendix A element name, one per property instance. Withholding cannot be expressed by a placeholder in any native field, so this property applies to all data fields. Consumers MAY treat an SBOM as incomplete "if the SBOM author withholds essential component data." |
| `cdx:cisa:redacted-information-inquiry` | The "process for recipients to ask about any redacted, security-related information" (2026 Minimum Elements, *Explicitly Identifying Unknown Information*). Value: URL, mailto contact, or document reference describing the inquiry process. Attaches to `metadata.properties`. The same contact SHOULD also appear in `externalReferences` with type `security-contact`. |
| `cdx:cisa:alternate-name` | An alternate name for the component, beyond the primary `name`. Repeatable: emit one property instance per alternate name; the set of instances forms the list of alternate names the Component Name element requires implementations to allow ("Data formats implementing the Component Name element should allow for multiple entries to capture alternate names"). The native `name` field is a single string. Component-level. |
| `cdx:cisa:component-identifier` | An additional identifier for the component permitted by the Component Identifiers element ("may also include universally unique identifiers (UUID), organization-specific identifiers") where no native identifier field applies: `bom-ref` is document-scoped rather than a cross-database lookup key, `externalReferences` requires a URL, and `swid` is a structured SWID tag. Value SHOULD be prefixed with its scheme, e.g. `uuid:...` or `acme-part:...`. One identifier per property instance. Component-level. |
| `cdx:cisa:hash` | Stopgap for the Component Hash elements where the algorithm is not representable in the native `hashes[].alg` enumeration, which lacks NIST-approved, IANA-registered algorithms such as `sha-224`, `sha-512/224`, `sha-512/256`, and `sha3-224`. Value: the IANA Hash Function Textual Name exactly as registered, a colon, and the hexadecimal digest (e.g. `sha-512/256:ab12...`), per the element's requirement to "identify the algorithm using Internet Assigned Numbers Authority (IANA) Hash Function Textual Names." A single combined value is used because repeatable name/value properties cannot be reliably paired. MUST NOT be used when the native enumeration can represent the algorithm. Intended to be retired if the specification extends or opens the `hashes[].alg` enumeration. Component-level. |

## Example

```json
{
  "metadata": {
    "properties": [
      { "name": "cdx:cisa:redacted-information-inquiry", "value": "mailto:product-security@example.com" }
    ]
  },
  "components": [
    {
      "type": "library",
      "bom-ref": "vendored-parser-a1b2c3",
      "name": "vendored-parser",
      "version": "unknown",
      "manufacturer": { "name": "unknown" },
      "properties": [
        { "name": "cdx:cisa:unknown-information", "value": "Component Hash Value" },
        { "name": "cdx:cisa:alternate-name", "value": "legacy-parse-lib" },
        { "name": "cdx:cisa:alternate-name", "value": "parselib-ng" },
        { "name": "cdx:cisa:component-identifier", "value": "uuid:3e671687-395b-41f5-a30f-a58921a69b79" }
      ]
    }
  ]
}
```
