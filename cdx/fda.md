# `cdx:fda` Namespace Taxonomy

This is the namespace for official CycloneDX properties carrying SBOM data fields required by U.S. FDA premarket cybersecurity guidance for medical devices: *Cybersecurity in Medical Devices: Quality Management System Considerations and Content of Premarket Submissions*, final guidance, February 3, 2026 (referred to below as "the guidance").

The official rules and processes apply - see [parent document](../cdx.md).

Each property description quotes the sentence in the guidance that requires the field, with its section number, so the property is traceable to source. The guidance permits these elements to be provided "as part of the SBOM, or they may provide it separately, such as in an addendum" (§V.A.4(b)); this namespace is the in-SBOM carrier, and any separately submitted addendum SHOULD be generated from these property values so the two cannot diverge.

Conventions: dates are ISO 8601 (RFC 3339 profile). Component-level properties attach to `components[].properties` or `metadata.component.properties`. Vulnerability-level properties attach to `vulnerabilities[].properties`. Properties are repeatable unless stated otherwise. The key words MUST, SHOULD, and MAY are to be interpreted as described in RFC 2119.

----

## Component-level properties

| Property | Description |
|----------|-------------|
| `cdx:fda:level-of-support` | "The software level of support provided through monitoring and maintenance from the software component manufacturer" (guidance §V.A.4(b)). Values follow the guidance's enumeration: `actively-maintained`, `no-longer-maintained`, `abandoned`. The guidance's list is illustrative ("e.g."); additional values MAY be used where these three do not describe the arrangement (for example `extended-support-contract`), and any additional value SHOULD be defined in accompanying documentation. |
| `cdx:fda:end-of-support-date` | "The software component's end-of-support date" (guidance §V.A.4(b)). ISO 8601 date. If the date is unknown, this property MUST be omitted and `cdx:fda:justification` emitted instead of a placeholder value. Where `cdx:lifecycle:milestone:endOfSupport` is also present on the same component, the two values MUST be identical; this property exists so the FDA element is explicitly identifiable, not to carry a second value. |
| `cdx:fda:end-of-life-date` | Component end-of-life date, from the guidance's labeling recommendation to provide "information, if known or anticipated, concerning device cybersecurity (including components) end of support and end of life" (§VI.A). ISO 8601 date. Where `cdx:lifecycle:milestone:endOfLife` is also present on the same component, the two values MUST be identical. |
| `cdx:fda:end-of-support-plan` | Reference to the plan "for how third-party software components could be updated or replaced if support ends or other software issues arise" (guidance §V.A.4). Value: a quality-system controlled-document identifier or URL. When the value is a URL, the same URL SHOULD also appear in `externalReferences` with type `support` on the component. |
| `cdx:fda:justification` | "A justification for why the information cannot be included in the premarket submission" (guidance §V.A.4(b)), applied at component granularity. The value MUST identify which element is missing and why, e.g. `end-of-support-date: upstream project publishes no lifecycle policy; support status inferred from release cadence per documented procedure`. |
| `cdx:fda:support-assessed-date` | ISO 8601 date on which the `cdx:fda:level-of-support` and end-of-support determinations were made. Not a guidance-defined field: SBOM documents are immutable while support status changes over time, and this property anchors the claim to its evidence window for total product lifecycle review. |
| `cdx:fda:source-code-custody` | The custodial-control arrangement for this component's source code, per the guidance's expectation that manufacturers "establish and maintain custodial control of device source code" through methods "such as source code escrow or source code backups" (§V.A.4), extended to purchased software via purchasing controls acquiring source "should the purchased software reach end of support" (§V.A.4, fn. 36). Recommended values: `first-party`, `escrow`, `backup`, `licensor-held`, optionally followed by a reference to the arrangement. |
| `cdx:fda:risk-transfer-process` | Reference to the "pre-established and pre-communicated process for transferring the risks" where the device remains in service following end of support (guidance §VI.A). Value: controlled-document identifier or URL. Attaches to the component the transfer concerns, or to `metadata.component` for the device as a whole. |
| `cdx:fda:traceability-ref` | Reference linking this component to its record in the documentation providing "traceability between the threat model, cybersecurity risk assessment, SBOM, and testing documentation" (guidance §V.A). Value: identifier of the corresponding traceability matrix row or record. |

## Vulnerability-level properties

The guidance requires identification of "all known vulnerabilities associated with the device and the software components, including those identified in CISA's Known Exploited Vulnerabilities Catalog" (§V.A.4(b)). Known vulnerabilities themselves are carried natively in `vulnerabilities[]`, or in a separate CycloneDX VDR/VEX document referencing the SBOM. The properties below carry the guidance's per-vulnerability documentation requirements, which have no native field.

| Property | Description |
|----------|-------------|
| `cdx:fda:vulnerability-discovery` | Description of "how the vulnerabilities were discovered to demonstrate whether the assessment methods were sufficiently robust" (guidance §V.A.4(b)). Value: the discovery method and tooling, e.g. `software composition analysis against per-image SBOM`, `fuzz testing`, `penetration test`, `coordinated disclosure`. |
| `cdx:fda:safety-and-security-risk-assessment` | Reference to "a safety and security risk assessment of each known vulnerability (including device and system impacts)" (guidance §V.A.4(b)). Value: the controlled-document identifier of the risk record in the manufacturer's security risk management report and, where patient harm is in scope, the ISO 14971 risk file. A one-sentence summary MAY follow the identifier. |
| `cdx:fda:risk-controls` | Reference to or description of the "details of applicable safety and security risk controls to address the vulnerability" (guidance §V.A.4(b)). Where the controls are compensating controls, the value MUST identify them as such, as the guidance requires those to "be described in an appropriate level of detail." |
| `cdx:fda:kev-listed` | Whether the vulnerability is "identified in CISA's Known Exploited Vulnerabilities Catalog" (guidance §V.A.4(b)). Values: `true`, `false`. The date of the KEV check is conveyed by the `updated` timestamp of the enclosing vulnerability object. |

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
        { "name": "cdx:fda:level-of-support", "value": "actively-maintained" },
        { "name": "cdx:fda:end-of-support-date", "value": "2028-12-31" },
        { "name": "cdx:lifecycle:milestone:endOfSupport", "value": "2028-12-31" },
        { "name": "cdx:fda:support-assessed-date", "value": "2026-08-06" },
        { "name": "cdx:fda:end-of-support-plan", "value": "QMS-PLN-0142" }
      ]
    }
  ],
  "vulnerabilities": [
    {
      "id": "CVE-2026-00000",
      "affects": [{ "ref": "pkg:generic/dcmtk@3.6.9" }],
      "analysis": { "state": "resolved", "response": ["update"] },
      "properties": [
        { "name": "cdx:fda:vulnerability-discovery", "value": "software composition analysis against per-image SBOM" },
        { "name": "cdx:fda:safety-and-security-risk-assessment", "value": "QMS-RSK-0311" },
        { "name": "cdx:fda:risk-controls", "value": "Upgraded to 3.6.10 in release 2026.3; see QMS-RSK-0311" },
        { "name": "cdx:fda:kev-listed", "value": "false" }
      ]
    }
  ]
}
```
