# API Accounts, Business Units and RBAC

The user viewing a dashboard, the identity used by an API proxy, and the service account used for background integrations can have different privileges.

## Security review

1. Identify actual authenticated API user and its effective EspoCRM roles, teams and entity/field permissions.
2. Determine whether the endpoint enforces access per record and per Business Unit *server-side*.
3. Check whether the UI accepts a `businessUnitId` or team ID from the client and validates it against the caller's authorized scopes.
4. For aggregation/count endpoints apply the same data restrictions as for list endpoints.
5. Audit default-to-global behavior, filtering bypass, cache keys (must include user/BU where appropriate) and background workers.
6. Prefer a dedicated non-admin API account scoped to least required access, where deployment requirements allow.

JavaScript filtering is useful for presentation, but it is never a tenant isolation control. If an API token belongs to an administrator it can potentially access data across units even if the UI shows only a subset.

## Safe tests

Use synthetic non-sensitive fixtures for distinct business units. Verify authorized units and denied units via user-scoped access. Never copy real customers' records into public test cases or GitHub issues.

Never print access tokens, keys, or the full contents of configuration or Docker environment variables in a report.
