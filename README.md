# Structured Semantic Supervision for Product Search with LLM-Augmented Annotations

This repository releases the annotations used in **Beyond Relevance: Structured
Semantic Supervision for Product Search with LLM-Augmented Annotations**. The
dataset augments query-product pairs sampled from the English Task 1 split of
the [Amazon Shopping Queries (ESCI) dataset](https://arxiv.org/abs/2206.06588)
with:

- structured, LLM-generated query and product attributes;
- an LLM-generated relevance assessment and explanation;
- human re-graded relevance and binary centrality labels;
- human-edited explanations, free-text comments, and edit-severity judgments.

The released corpus contains **1,952 annotated rows**, corresponding to
**1,887 unique query-product pairs**, **1,878 distinct product titles**, and
**266 unique queries**. Repeated pairs are retained because they support
cross-batch consistency and inter-annotator analysis.

## Dataset directory

```text
Dataset/
├── Annotation-A1.xlsx
├── Batch1-5-Annotation-A2.xlsx
└── Batch6-Annotation-A2.xlsx
```

### `Annotation-A1.xlsx`

Annotations from **A1**, the canonical annotator used for the labels reported
in the paper. The main data are divided across `Batch1`, `Batch2`, `Batch3`,
`Batch4`, `Batch5`, and `Batch 6`. The workbook also contains a small,
headerless auxiliary sheet named `Sheet1`; it is not part of the six-batch
corpus and should normally be ignored.

### `Batch1-5-Annotation-A2.xlsx`

The corresponding **A2** workbook for batches 1-5. It is retained for
annotation comparison and agreement analysis. Its worksheets are named
`Batch 1` through `Batch 5`.

### `Batch6-Annotation-A2.xlsx`

The **A2** workbook for batch 6, stored separately because that batch was
collected in a later annotation round. It contains one worksheet, `Batch6`.

The paper's formal reliability analysis uses the 120-pair joint calibration
batch and 13 additional pairs judged by both annotators. A1 labels remain the
canonical corpus labels.

## Batch statistics

| Batch | Annotated rows | Unique query-product pairs | Unique queries |
| --- | ---: | ---: | ---: |
| Batch 1 (joint calibration) | 120 | 120 | 86 |
| Batch 2 | 250 | 249 | 150 |
| Batch 3 | 250 | 250 | 155 |
| Batch 4 | 250 | 250 | 146 |
| Batch 5 | 250 | 249 | 156 |
| Batch 6 | 832 | 829 | 241 |
| **Total** | **1,952** | **1,887** | **266** |

Unique-query counts are not additive because some queries occur in multiple
batches.

## Column groups

Column names vary slightly in capitalization, spacing, and underscore usage
between batches. They fall into the following groups.

### Source fields

| Field | Description |
| --- | --- |
| `id` / `ID` | Row identifier when available. |
| `query` | Shopping query. |
| `product_title` | Candidate product title. |
| `median relevance` / `median_relevance` | Median relevance from the source data. |
| `relevance variance` / `relevance_variance` | Variance of source relevance judgments. |

### LLM-generated query attributes

Each attribute is represented by a categorical `*_value` column and a free-text
`*_explanation` column.

| Attribute | Typical values |
| --- | --- |
| `query_expansion_classification_type` | `Product-specific`, `Category`, `Feature-based`, `Brand` |
| `query_expansion_user_intent` | `Purchase`, `Research`, or a combination |
| `query_expansion_query_complexity` | `Simple keyword`, `Multi-aspect` |
| `query_expansion_primary_attribute_focus` | For example, `Brand`, `Feature`, `Product-specific` |

### LLM-generated product attributes

These fields also occur as paired `*_value` and `*_explanation` columns.

| Attribute | Description |
| --- | --- |
| `product_title_expansion_product_type` | Product, accessory, bundle, or related type. |
| `product_title_expansion_brand_presence` | Whether a brand is present or applicable. |
| `product_title_expansion_title_complexity` | Simple or multi-aspect title. |
| `product_title_expansion_primary_selling_point` | Main brand- or feature-oriented selling point. |
| `product_title_expansion_target_demographic` | General or specific target audience. |
| `product_title_expansion_price_indicator` | Inferred price positioning, when present. |
| `product_title_expansion_condition` | Inferred product condition. |

### Relevance and human annotation fields

| Field family | Description |
| --- | --- |
| `relevance assessment` / `relevance_assessment_result_relevance` | LLM-generated holistic relevance category. |
| `relevance_assessment_result_explanation` | LLM-generated explanation grounded in the structured attributes. |
| `Relevance Score` / `relevance score` / `Updated_relevance` | Human re-graded relevance label. |
| `Centrality Score` / `Centrality` / `Centrality_annotation` | Human binary centrality label. |
| `Comments` / `Explaination` | Human note explaining the final judgment. |
| `Edit` / `Post Editing Column` | Explanation-edit category. |
| `No edit`, `Minor edit`, `Major edit` | Batch-specific one-hot edit markers. |

Blank columns between field groups are visual separators from the annotation
spreadsheets and do not carry data.

## Label definitions

### Relevance

Human relevance is an ordered four-point scale aligned with ESCI:

| Score | Interpretation |
| ---: | --- |
| 4 | Exact match carrying the defining query details; intentionally rare. |
| 3 | Close, but not exact, match that is very likely to satisfy the user. |
| 2 | Partial match or close alternative with missing or mismatched details. |
| 1 | Little or nothing in common with the intended product. |

### Centrality

- `1`: a central, typical result that directly satisfies the primary query intent.
- `0`: a peripheral, atypical, or indirectly relevant result.

Centrality is distinct from topical relevance. For example, an accessory can
be related to a product query while remaining peripheral to its primary intent.

### Post-edit category

- `No edit`: the LLM explanation was retained.
- `Minor edit`: the explanation was lightly revised.
- `Major edit`: the explanation was substantially rewritten.

The post-edit category records annotation effort and is not a model target in
the associated experiments.

## License

This repository is released under the [MIT License](LICENSE).