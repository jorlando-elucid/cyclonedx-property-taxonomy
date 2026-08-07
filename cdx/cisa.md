# `cdx:cisa` Namespace Taxonomy

This is the namespace for official CycloneDX properties carrying SBOM data fields defined in the *2026 Minimum Elements for a Software Bill of Materials (SBOM)*, published July 29, 2026 by CISA and co-authoring organizations (referred to below as "the 2026 Minimum Elements"), which replaces the 2021 NTIA minimum elements. Only fields with no native CycloneDX field are defined here.

The official rules and processes apply - see [parent document](../cdx.md).

Value convention: where a property value names a data field, the name is the element name exactly as listed in Appendix A of the 2026 Minimum Elements (e.g. `Component Version`, `Component Producer`, `Component License`, `Component Hash Value`). Properties are repeatable unless stated otherwise. The key words MUST, SHOULD, and MAY are to be interpreted as described in RFC 2119.

Scope note: unknowns in the dependency graph itself are declared natively via `compositions` (aggregate `unknown` or `incomplete`); the properties below address the per-data-field statements the 2026 Minimum Elements require, which have no native mechanism.

----

| Property | Description |
|----------|-------------|
| `cdx:cisa:unknown-information` | Explicit statement that "the information is unknown to the SBOM author" (2026 Minimum Elements, *Explicitly Identifying Unknown Information*). Value: the Appendix A element name the statement applies to, one element name per property instance. Attach at the level the statement applies to: `metadata.properties` for SBOM Metadata elements, `components[].properties` for Component Data elements. |
| `cdx:cisa:withheld-information` | Explicit statement that "the SBOM author is withholding the information from the SBOM" (2026 Minimum Elements, *Explicitly Identifying Unknown Information*). Value: the Appendix A element name, one per property instance. Consumers MAY treat an SBOM as incomplete "if the SBOM author withholds essential component data." |
| `cdx:cisa:redacted-information-inquiry` | The "process for recipients to ask about any redacted, security-related information" (2026 Minimum Elements, *Explicitly Identifying Unknown Information*). Value: URL, mailto contact, or document reference describing the inquiry process. Attaches to `metadata.properties`. The same contact SHOULD also appear in `externalReferences` with type `security-contact`. |
| `cdx:cisa:unknown-provenance` | Explicit indication "that the component is of unknown provenance to acknowledge the lack of traceability" where "there is no clear indication of component producer" (2026 Minimum Elements, *Component Producer*). Value: `true`. Component-level. Equivalent in effect to `cdx:cisa:unknown-information` with value `Component Producer`; provided as the document's own phrase so consumers searching on it find it. |
| `cdx:cisa:alternate-name` | An alternate name for the component. The Component Name element states that implementations "should allow for multiple entries to capture alternate names" (2026 Minimum Elements, *Component Name*); the CycloneDX `name` field is a single string. Value: one alternate name per property instance. Component-level. |
| `cdx:cisa:sbom-version` | The author-designated SBOM version identifier where the versioning scheme is not expressible in the native integer `version` field, e.g. Semantic Versioning as the SBOM Version element recommends ("if the SBOM author uses Semantic Versioning, the major version of a published SBOM following these minimum elements should be '1'"). Attaches to `metadata.properties`. When present, it MUST change whenever the native `version` field increments. |
| `cdx:cisa:component-identifier` | An additional identifier for the component permitted by the Component Identifiers element ("may also include universally unique identifiers (UUID), organization-specific identifiers") where no native identifier field applies. Value SHOULD be prefixed with its scheme, e.g. `uuid:...` or `acme-part:...`. One identifier per property instance. Component-level. |
| `cdx:cisa:hash` | Reserved escape hatch for the Component Hash elements where the algorithm is not representable in the native `hashes[].alg` enumeration (for example `sha-224`, `sha-512/256`, `sm3`). Value: the IANA Hash Function Textual Name, a colon, and the hexadecimal digest, per the element's requirement to "identify the algorithm using Internet Assigned Numbers Authority (IANA) Hash Function Textual Names." MUST NOT be used when the native enumeration can represent the algorithm. Component-level. |
| `cdx:cisa:proprietary-license-conditions` | "Information about the existence of proprietary license conditions" (2026 Minimum Elements, *Component License*). Value: `true`, `false`, or a short statement or URL indicating where the full conditions are available. Component-level. |

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
      "properties": [
        { "name": "cdx:cisa:unknown-provenance", "value": "true" },
        { "name": "cdx:cisa:unknown-information", "value": "Component Version" },
        { "name": "cdx:cisa:withheld-information", "value": "Component Hash Value" },
        { "name": "cdx:cisa:alternate-name", "value": "legacy-parse-lib" }
      ]
    }
  ]
}
```
