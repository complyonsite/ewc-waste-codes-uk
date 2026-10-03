# EWC waste code dataset by ComplyOnSite

A reusable CSV and JSON copy of the 842 entries in the UK List of Waste, commonly called EWC codes. Each record includes the official code and description, its chapter and sub-chapter, whether the code is hazardous, and the WM3 entry type.

- [CSV](ewc-waste-codes-uk.csv) — one record per row, suitable for spreadsheets and databases
- [JSON](ewc-waste-codes-uk.json) — dataset metadata followed by the same records
- [Method](METHOD.md) — source lineage, transformations and checks
- [Licence](LICENSE.md) — re-use terms and attribution
- [Checksums](CHECKSUMS.sha256) — SHA-256 for the two data files

Use the [free ComplyOnSite EWC code finder](https://complyonsite.com/tools/ewc-code-finder) to search the list by code or waste description. See [ComplyOnSite](https://complyonsite.com/) for practical UK construction and waste compliance tools.

## Fields

| Field | Meaning |
|---|---|
| `code` | Six digits with no spaces or asterisk. Keep this as text so leading zeroes survive. |
| `code_spaced` | Six digits in the usual `17 09 04` display format, without an asterisk. |
| `display_code` | Display code with `*` where the List of Waste marks it hazardous. |
| `hazardous` | Boolean derived from the legal list's asterisk. |
| `entry_type` | WM3 Appendix A type: `AH`, `AN`, `MH` or `MN`. |
| `entry_type_label` | Expanded WM3 entry type. |
| `requires_mirror_assessment` | `true` for a mirror hazardous or mirror non-hazardous entry. It does not mean the waste has been assessed. |
| `description` | Official List of Waste description. |
| `note` | Official entry footnote where present; otherwise empty in CSV and `null` in JSON. |
| `chapter_code`, `chapter_title` | Two-digit chapter and official chapter title. |
| `subchapter_code`, `subchapter_title` | Four-digit sub-chapter and official sub-chapter title. |
| `named_mirror_partner_codes` | Semicolon-separated in CSV and an array in JSON. These are the other-side entries identified from the List wording, plus three chapter 17 groups explicitly paired by GOV.UK's construction and demolition guide. An empty value does not prove an entry has no alternative assessment route. |

## Counts

- 842 codes
- 408 hazardous codes
- WM3 types: 235 `AH`, 256 `AN`, 173 `MH`, 178 `MN`
- 20 chapters and 111 sub-chapters

## Limits

This is reference data, not a waste classification. Choosing a code can depend on where and how the waste arose, its composition, contamination, hazardous properties, persistent organic pollutants and the mirror-entry assessment. Check WM3 and the applicable regulator guidance, and use a competent person where the assessment requires it.

The dataset is a dated snapshot. Check the official sources for amendments before relying on it for current legal or operational decisions.
