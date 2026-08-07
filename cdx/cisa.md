# `cdx:cisa` Namespace Taxonomy

This is the namespace for official CycloneDX properties carrying SBOM information defined in the [*2026 Minimum Elements for a Software Bill of Materials (SBOM)*](https://www.cisa.gov/sites/default/files/2026-07/2026_cisa_sbom_minimum_elements_508c.pdf), published July 29, 2026 by CISA and co-authoring organizations (referred to below as "the 2026 Minimum Elements"), which replaces the 2021 NTIA minimum elements. Only information without a complete native CycloneDX representation is defined here.

The official rules and processes apply - see [parent document](../cdx.md).

Where a property value names a data field, the value MUST be the data field name exactly as listed in Appendix A of the 2026 Minimum Elements (for example, `Component Version`, `Component Producer`, or `Component Hash Value`). Properties are repeatable unless stated otherwise. The key words MUST, SHOULD, and MAY are to be interpreted as described in [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119).

When a required data field is unknown, its native CycloneDX representation SHOULD also be populated when the native field supports an unknown value. For example, use the literal `unknown` for `components[].version` and `components[].manufacturer.name`, and use `compositions` to describe unknown or incomplete dependency relationships. The `cdx:cisa:unknown-information` property is still required in these cases because it explicitly identifies which required information is unknown to the SBOM author.

The CISA `SBOM Author` data field maps to `metadata.authors` when one or more people manually create the SBOM and to `metadata.manufacturer` when an organization creates it through an automated process.

----

| Property | Description |
|----------|-------------|
| `cdx:cisa:unknown-information` | Explicitly identifies a required data field whose information is unknown to the SBOM author. The 2026 Minimum Elements practice *Explicitly Identifying Unknown Information* requires SBOM authors to distinguish information that is unknown from information they are withholding. The value MUST be the applicable Appendix A data field name. One property instance MUST be provided for each unknown data field, including when a native field contains `unknown` or another native mechanism also describes the missing information. Use `metadata.properties` for SBOM Metadata fields and the applicable `components[].properties` for Component Data fields. |
| `cdx:cisa:withheld-information` | Explicitly identifies a required data field whose information the SBOM author is intentionally withholding. The 2026 Minimum Elements practice *Explicitly Identifying Unknown Information* requires this information to be distinguished from information that is unknown to the author. The value MUST be the applicable Appendix A data field name. One property instance MUST be provided for each withheld data field. Use `metadata.properties` for SBOM Metadata fields and the applicable `components[].properties` for Component Data fields. |
| `cdx:cisa:redacted-information-inquiry` | Describes the process for recipients to ask about redacted, security-related information, as specified by the 2026 Minimum Elements practice *Explicitly Identifying Unknown Information*. The value is free text and MAY contain a URL, email address, or document reference for the inquiry process. This document-level property MUST be placed in `metadata.properties` and MAY appear only once. |
| `cdx:cisa:alternate-name` | An alternate name for a component in addition to its native `name`. Data formats implementing Component Name are expected to allow multiple entries for alternate names. The value is one alternate name. One property instance MUST be provided for each alternate name. Component-level. |
| `cdx:cisa:component-identifier` | A UUID or organization-specific component identifier for which no more specific native CycloneDX identifier field is appropriate. Native fields such as `purl`, `cpe`, `swid`, `omniborId`, and `swhid` SHOULD be used when applicable. The value is free text and SHOULD identify its scheme, for example `urn:uuid:3e671687-395b-41f5-a30f-a58921a69b79` or `acme-part:12345`. One property instance MUST be provided for each additional identifier. Component-level. |

## Example

```json
{
  "metadata": {
    "properties": [
      {
        "name": "cdx:cisa:redacted-information-inquiry",
        "value": "Email product-security@example.com"
      }
    ]
  },
  "components": [
    {
      "type": "library",
      "name": "vendored-parser",
      "version": "unknown",
      "manufacturer": { "name": "unknown" },
      "properties": [
        { "name": "cdx:cisa:unknown-information", "value": "Component Version" },
        { "name": "cdx:cisa:unknown-information", "value": "Component Producer" },
        { "name": "cdx:cisa:unknown-information", "value": "Component Hash Value" },
        { "name": "cdx:cisa:alternate-name", "value": "legacy-parse-lib" },
        { "name": "cdx:cisa:alternate-name", "value": "parselib-ng" },
        {
          "name": "cdx:cisa:component-identifier",
          "value": "urn:uuid:3e671687-395b-41f5-a30f-a58921a69b79"
        }
      ]
    }
  ]
}
```
