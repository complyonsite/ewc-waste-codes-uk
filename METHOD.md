# Method and provenance

## Sources

The code, asterisk, official description, footnote, chapter and sub-chapter fields come from the [Annex to Commission Decision 2000/532/EC on legislation.gov.uk](https://www.legislation.gov.uk/eudn/2000/532/annex), the retained UK List of Waste. The extracted edition is the revised version valid from 31 December 2020, read on 27 September 2026 from the legislation.gov.uk XML endpoint.

The entry types come from Appendix A of [Waste classification: guidance on the classification and assessment of waste (WM3)](https://www.gov.uk/government/publications/waste-classification-technical-guidance), first edition v1.2.GB, updated 28 September 2021. WM3 defines:

- `AH`: absolute hazardous
- `AN`: absolute non-hazardous
- `MH`: mirror hazardous
- `MN`: mirror non-hazardous

The sources were read separately. Hazardous status comes from the asterisk in the legal list; the WM3 type is an added guidance field. Validation checks that both agree for every code.

## Transformations

1. Normalise each code to six digits in `code`, retaining a human-readable spaced form separately.
2. Treat the legal list's asterisk as the hazardous flag and retain it in `display_code`.
3. Preserve official descriptions and the two entry footnotes. Whitespace and the source's two occurrences of a space before `*` are normalised; wording is not rewritten.
4. Add the matching WM3 Appendix A entry type.
5. Resolve named mirror partners from `other than those mentioned in` or `except` references in non-hazardous mirror descriptions. Add the three chapter 17 groups which the [GOV.UK construction and demolition waste guide](https://www.gov.uk/guidance/construction-and-demolition-waste-how-to-classify) explicitly sets against one hazardous entry.
6. Serialise the same record set to RFC 4180-style CSV and structured JSON.

The build reads the typed EWC data used by the ComplyOnSite finder. Rebuild from a ComplyOnSite checkout with:

```sh
COMPLYONSITE_SOURCE_ROOT=/path/to/ComplyOnSite pnpm exec tsx build-dataset.mts
```

## Verification

The snapshot checks assert:

- 842 unique, sorted six-digit codes;
- 408 codes marked hazardous;
- 20 chapters and 111 sub-chapters;
- WM3 type totals of 235 `AH`, 256 `AN`, 173 `MH` and 178 `MN`;
- the hazardous asterisk agrees with `AH` or `MH` on every record;
- every resolved mirror relationship is reciprocal and crosses hazardous/non-hazardous sides.

On 2 October 2026, the official WM3 PDF was re-fetched and still matched the retained source SHA-256 `90c70ee417ab9651b69c74528970d1b7041c1e28cb40468f73639dcc2378e6c8`. The legislation.gov.uk XML response had changed from the retained byte hash because its metadata changed; all 842 code rows, descriptions and hazardous asterisks were compared with this dataset and matched.

## Known boundary

`named_mirror_partner_codes` is an aid to navigation. WM3 can mark entries as mirror entries without the legal description naming a unique partner, and classification still depends on the waste assessment. Do not infer that an empty partner list makes a mirror code absolute.
