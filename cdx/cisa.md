# `cdx:cisa` Namespace Taxonomy

This is the namespace for official CycloneDX properties carrying SBOM information defined in the *2026 Minimum Elements for a Software Bill of Materials (SBOM)*, published July 29, 2026 by CISA and
co-authoring organizations (referred to below as "the 2026 Minimum Elements"), which replaces the 2021 NTIA minimum elements. Only information without a complete native CycloneDX representation is
defined here.

The official rules and processes apply - see [parent document](../cdx.md).

Where a property value names a data field, the value MUST be the data field name exactly as listed in Appendix A of the 2026 Minimum Elements (for example, `Component Version`, `Component
Producer`, or `Component Hash Value`). Properties are repeatable unless stated otherwise. The key words MUST, SHOULD, and MAY are to be interpreted as described in RFC 2119.

When a required data field is unknown, its native CycloneDX representation SHOULD also be populated when the native field supports an unknown value. For example, use the literal `unknown` for
`components[].version` and `components[].manufacturer.name`, and use `compositions` to describe unknown or incomplete dependency relationships. The `cdx:cisa:unknown-information` property is still
required in these cases because it explicitly identifies which required information is unknown to the SBOM author.

----

| Property | Description |
|----------|-------------|
| `cdx:cisa:unknown-information` | Explicitly identifies a required data field whose information is unknown to the SBOM author, as specified by *Explicitly Identifying Unknown Information*. The
value MUST be the applicable Appendix A data field name. One property instance MUST be provided for each unknown data field, including when a native field contains `unknown` or another native
mechanism also describes the missing information. Use `metadata.properties` for SBOM Metadata fields and the applicable `components[].properties` for Component Data fields. |
| `cdx:cisa:withheld-information` | Explicitly identifies a required data field whose information the SBOM author is intentionally withholding from the SBOM, as specified by *Explicitly Identifying
Unknown Information*. The value MUST be the applicable Appendix A data field name. One property instance MUST be provided for each withheld data field. Use `metadata.properties` for SBOM Metadata
… +17 lines (ctrl + t to view transcript)
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
          "value": "uuid:3e671687-395b-41f5-a30f-a58921a69b79"
        }
      ]
    }
  ]
}
```
