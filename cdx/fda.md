# `cdx:fda` Namespace Taxonomy

This is the namespace for official CycloneDX properties carrying SBOM information for premarket submissions under the U.S. FDA guidance *Cybersecurity in Medical Devices: Quality Management System
Considerations and Content of Premarket Submissions*, issued February 3, 2026 (referred to below as "the guidance").

The official rules and processes apply - see [parent document](../cdx.md).

Guidance-defined properties identify the relevant sentence and section for traceability. Section V.A.4(b) permits the additional component-support elements to be provided as part of the SBOM or
separately in an addendum; this namespace provides an in-SBOM representation.

Dates MUST use the RFC 3339 `full-date` form, `YYYY-MM-DD`. Component-level properties attach to `components[].properties`, or to `metadata.component.properties` when they describe the device as a
whole. Properties are repeatable unless stated otherwise. The key words MUST, SHOULD, and MAY are to be interpreted as described in RFC 2119.

Vulnerability properties in this namespace record only the inspection results available when the SBOM is generated. They are not a vulnerability-management record and do not carry risk assessments,
remediation status, risk controls, or post-build findings. That information SHOULD be maintained in a separate, linked CycloneDX VDR/VEX or other vulnerability-management document. The SBOM's
`metadata.timestamp` identifies when the build-time inspection snapshot was created.

----

| Property | Description |
|----------|-------------|
| `cdx:fda:level-of-support` | The software level of support provided through monitoring and maintenance by the software component manufacturer (guidance §V.A.4(b)). The value is free text. The
guidance's examples `actively maintained`, `no longer maintained`, and `abandoned` SHOULD be used verbatim when applicable. MAY appear only once per component. |
| `cdx:fda:end-of-support-date` | The software component's end-of-support date (guidance §V.A.4(b)). If no date can be provided, this property MUST be omitted and a `cdx:fda:justification` property
… +23 lines (ctrl + t to view transcript)

```json
{
  "components": [
    {
      "type": "library",
      "bom-ref": "pkg:generic/dcmtk@3.6.9",
      "name": "dcmtk",
      "version": "3.6.9",
      "properties": [
        { "name": "cdx:fda:level-of-support", "value": "actively maintained" },
        { "name": "cdx:fda:end-of-support-date", "value": "2028-12-31" },
        { "name": "cdx:lifecycle:milestone:endOfSupport", "value": "2028-12-31" },
        { "name": "cdx:fda:support-assessed-date", "value": "2026-08-06" },
        { "name": "cdx:fda:update-or-replace-plan", "value": "QMS-PLN-0142" },
        { "name": "cdx:fda:known-vulnerability", "value": "CVE-2026-00000" },
        {
          "name": "cdx:fda:vulnerability-discovery-method",
          "value": "CVE-2026-00000: internal cybersecurity monitoring"
        }
      ]
    }
  ]
}
```
