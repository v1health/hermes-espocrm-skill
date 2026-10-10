# API Filtering, Counts and Pagination

EspoCRM API versions, custom proxies and repositories can differ. Verify behavior with the deployed version instead of assuming filtered count semantics.

## Common failure modes

- `maxSize=1` plus `list.length` is not the number of matching records.
- A `total` field may omit filters in a buggy custom proxy; alternatively count may be disabled or unavailable.
- A page of 200 results is not all records. Do not claim an exact count when more may exist.
- A failed or unauthorized response with `list=[]` must not become `0`.
- Filtering by a status that is not an allowed value can quietly report an empty queue.
- Missing boolean fields are not automatically `false`; distinguish `false`, `0`, `null`, absent and invalid values based on schema.

## Verification algorithm

1. Determine allowed statuses and field types from metadata.
2. Construct a read-only query with explicit select fields, filters and sort.
3. Compare the API count against a trusted repository-side count on staging.
4. Paginate until exhaustion, honoring provider pagination tokens/pages and limits. Track unique IDs if records can change during scan.
5. If complete exact counting is impractical, use a server-side aggregation endpoint with RBAC or display `Unknown` / `Partial` explicitly.
6. Test page sizes 0, 1, maximum, and larger than maximum with representative fixtures.

## Error handling pseudo-code

```js
function normalizeListResponse(resp) {
  if (!resp || resp.ok === false || resp.__err || resp.error) {
    return { state: 'unknown', items: [] };
  }
  if (!Array.isArray(resp.list)) {
    return { state: 'unknown', items: [] };
  }
  return { state: 'partial', items: resp.list };
}
```

The result above is intentionally **partial** until pagination/completeness is verified. It should never be rendered as a confirmed global total just because the list is valid.
