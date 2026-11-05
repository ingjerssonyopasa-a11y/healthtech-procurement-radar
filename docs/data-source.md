# Data source: SECOP II Open Data

The workflow reads from **SECOP II - Procesos de Contratación**, a public Colombian procurement dataset maintained by Datos Abiertos Colombia and supplied by Agencia Nacional de Contratación Pública - Colombia Compra Eficiente.

- Dataset page: <https://www.datos.gov.co/d/p6dx-8zbt>
- Socrata resource endpoint: <https://www.datos.gov.co/resource/p6dx-8zbt.json>
- Dataset identifier: `p6dx-8zbt`

The dataset describes procurement processes registered in SECOP II, whether or not they have been awarded. Its public metadata indicates national coverage and daily updates. Check the source page for current metadata and field definitions.

## Query approach

The n8n HTTP Request node uses SoQL query parameters, including a bounded `$limit`, a publication-date ordering, and a text predicate (`$where`) covering healthcare-related terms in procedure names and descriptions.

The classification stage then applies JavaScript rules. API-side keyword filtering is a retrieval optimization, not the final commercial classification.

## Important interpretation rules

- A record categorized as healthcare-related is not automatically an active tender.
- A `Publicado` state or an offer phase in the source does not independently prove that bids are still being accepted.
- If no closing date is detected, the alert asks the recipient to verify the process directly in SECOP.
- Users should inspect the official process page and contracting documents before making a commercial decision.

## Data freshness

The dataset is maintained independently from this project. Do not assume a fixed update timestamp in this repository. The workflow's local execution timestamp is stored in the radar output and should be compared with the public source when reviewing opportunities.
