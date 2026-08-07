# `cdx:fda` Namespace Taxonomy

This is the namespace for official CycloneDX properties carrying SBOM information for premarket submissions under the U.S. FDA guidance [*Cybersecurity in Medical Devices: Quality Management System Considerations and Content of Premarket Submissions*](https://www.fda.gov/media/119933/download), issued February 3, 2026 (referred to below as "the guidance").

The official rules and processes apply - see [parent document](../cdx.md).

Guidance-defined properties cite the relevant section for traceability. Section V.A.4(b) permits the additional component-support elements to be provided as part of the SBOM or separately in an addendum; this namespace provides an in-SBOM representation. Properties identified below as namespace conventions support submission workflows but are not fields defined by the guidance.

Dates MUST use the RFC 3339 `full-date` form, `YYYY-MM-DD`. Component-level properties attach to `components[].properties`, or to `metadata.component.properties` when they describe the device as a whole. Properties are repeatable unless stated otherwise. The key words MUST, SHOULD, and MAY are to be interpreted as described in [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119).

As a namespace convention, vulnerability properties record only the inspection results available when the SBOM is generated. The inspection MUST be performed as part of SBOM generation, and the SBOM's `metadata.timestamp` identifies when the snapshot was created. These properties are not a vulnerability-management record and do not carry risk assessments, remediation status, risk controls, or post-build findings. That information SHOULD be maintained in a separate, linked CycloneDX VDR/VEX or other vulnerability-management document. FDA's recommendations to provide risk assessments and applicable risk controls for known vulnerabilities still apply to the premarket submission even when those details are maintained outside the SBOM.

----

| Property | Description |
|----------|-------------|
| `cdx:fda:level-of-support` | The software level of support provided through monitoring and maintenance by the software component manufacturer (guidance §V.A.4(b)). The value is free text. The guidance's examples `actively maintained`, `no longer maintained`, and `abandoned` SHOULD be used verbatim when applicable. MAY appear only once per component. |
| `cdx:fda:end-of-support-date` | The software component's end-of-support date (guidance §V.A.4(b)). If no date can be provided, this property MUST be omitted and a `cdx:fda:justification` property provided. When `cdx:lifecycle:milestone:endOfSupport` is also present for the component, it MUST also use `YYYY-MM-DD` and its value MUST be identical. MAY appear only once per component. |
| `cdx:fda:end-of-life-date` | A date representing the known or anticipated component end of life described in guidance §VI.A. Representing this information as a date, and using `cdx:fda:justification` when no date can be provided, are namespace conventions. If no date can be provided, this property MUST be omitted and a `cdx:fda:justification` property provided. When `cdx:lifecycle:milestone:endOfLife` is also present for the component, it MUST also use `YYYY-MM-DD` and its value MUST be identical. MAY appear only once per component. |
| `cdx:fda:justification` | A justification for why information cannot be included in the premarket submission (guidance §V.A.4(b)). The value MUST contain the affected `cdx:fda` property name, a colon, and a free-text justification, for example `cdx:fda:end-of-support-date: upstream project publishes no support date`. One property instance MUST be provided for each property that cannot be answered. |
| `cdx:fda:update-or-replace-plan` | Describes the plan for how a third-party software component could be updated or replaced if support ends or other software issues arise (guidance §V.A.4). The value is free text and MAY contain a controlled-document identifier or URL. |
| `cdx:fda:source-code-custody` | Describes how custodial control of device source code is established and maintained, such as through source code escrow or backups (guidance §V.A.4 and footnote 36). The value is free text and MAY contain a controlled-document identifier or URL. |
| `cdx:fda:risk-transfer-process` | Describes the pre-established and pre-communicated process for transferring risks when a device remains in service after end of support (guidance §VI.A). The value is free text and MAY contain a controlled-document identifier or URL. |
| `cdx:fda:traceability` | Identifies the record providing traceability between the threat model, cybersecurity risk assessment, SBOM, and testing documentation, as recommended by guidance §V.A. The value is the corresponding traceability record identifier. |
| `cdx:fda:support-assessed-date` | The date on which the component's level-of-support and end-of-support determinations were made. This is a namespace-defined submission-support field, not a field defined by the guidance. It anchors time-varying support information to the assessment used for the submission. MAY appear only once per component. |
| `cdx:fda:known-vulnerability` | Records the result of the build-time inspection for known vulnerabilities associated with the component, including CISA Known Exploited Vulnerabilities (guidance §V.A.4(b)). Using the literal `false` to record a negative result is a namespace convention. When none are known at inspection time, the value MUST be `false` and the property MAY appear only once. Otherwise, the value MUST be a known vulnerability identifier and one property instance MUST be provided for each vulnerability found; `false` MUST NOT be present in that case. This property records discovery only, not analysis or disposition. |
| `cdx:fda:vulnerability-discovery-method` | Records how a vulnerability was found during the build-time inspection so the robustness of the assessment method can be evaluated (guidance §V.A.4(b)). The value MUST contain the known vulnerability identifier, a colon, and a free-text description of how it was discovered, for example `CVE-2026-00000: software composition analysis`. One property instance MUST be provided for each vulnerability and discovery method pair found during the inspection. |

## Example

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
          "value": "CVE-2026-00000: software composition analysis"
        }
      ]
    }
  ]
}
```
