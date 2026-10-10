# Architecture and Discovery

The installed EspoCRM version, extension conventions and Docker layout are the source of truth. Identify them before making assumptions.

## Map the layers

1. Web/UI: browser route, custom `views`, AMD module (`define()`), HTML template/`data-role` selectors, loaded CSS.
2. API: authorization identity, REST endpoint, query filters, transformations and HTTP response.
3. Application: PHP custom services, repositories, metadata/entity definitions, event hooks, scheduled jobs.
4. Data: relational DB, permissions, entity fields/statuses, queues, caches and integrations.
5. Runtime: Docker container image and UID, bind mounts, volumes, daemon process, CLI cache operations.

## Baseline discovery checklist

```bash
# Read-only examples; run only when authorized
pwd
git rev-parse --show-toplevel
git status --short
git rev-parse HEAD
git ls-files | head -40
docker ps --format '{{.Names}} {{.Image}} {{.Status}}'
```

Inspect `docker inspect` mounts without printing environment secrets. Determine whether staging and production share any source paths, volumes, DB, caches or cron daemon. Do not create staging using a bind mount from the live code volume.

## Dependencies

Client AMD `define('...', [...], ...)` dependencies may include modules not referenced directly from metadata. Check import graphs before archiving code. A file not present in a dashboard manifest can still be required dynamically.

Custom code path names vary. A copied repository directory named `client-custom` may not map directly to a deployed `client/custom` folder. Confirm the actual mapping, gitignore rules and backup paths; never assume the repository includes all frontend source files.

## Debugging flow

Write a one-line hypothesis, then seek direct evidence at each layer. For example: blank KPI -> API returns error -> PHP autoload read permission denied -> file owner/mode after Docker copy. Check actual PHP logs and readability as the PHP runtime user; don't patch the JS placeholder to hide the API failure.
