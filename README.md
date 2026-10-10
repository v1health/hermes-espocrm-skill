# Hermes Agent Skill for EspoCRM

**A cautious, practical EspoCRM operations skill for Hermes Agent.** Debug custom PHP services, REST/API queries, client-side AMD views, dashboard layouts, Docker staging, RBAC, and release/rollback workflows without guessing or treating a green test result as proof of correctness.

EspoCRM is a flexible open-source CRM. Customizations can span server metadata, entity repositories, browser JavaScript, CSS, scheduled jobs, and integrations. This skill gives Hermes a repeatable process to identify the source of a defect, verify it on isolated staging, and request permission before touching production.

> This is an **instruction-only skill**. It grants no API permissions, doesn't contain credentials, and doesn't deploy or run anything by itself. Always check the actual EspoCRM version and environment.

## What it covers

- **Architecture:** EspoCRM PHP services, custom entities, metadata, client-side AMD view modules and Dockerized deployments.
- **Data correctness:** paginated API queries, filtered counting, null vs zero, loading/stale/error states and avoiding duplicated requests.
- **Frontend:** CSS Grid/Flexbox, `!important` conflicts, computed styles, mobile layouts, sticky sidebars and screenshot validation.
- **Security:** EspoCRM roles/teams, Business Units, administrator-backed API accounts, least privilege and avoiding UI-only authorization.
- **Operations:** isolated staging, backup with SHA256, file permissions, smoke tests, controlled deployment and rollback.

## Install

Copy the `espocrm-ops` folder to your Hermes user-level skills directory. Common layout:

```bash
mkdir -p ~/.hermes/skills/development
cp -a espocrm-ops ~/.hermes/skills/development/espocrm-ops
hermes skills list
```

If a skill of the same name already exists, **back it up and review the diff first**. Restart or open a new Hermes session if it does not appear immediately. Paths and CLI syntax can differ across Hermes versions; consult [Hermes skill documentation](https://hermes-agent.nousresearch.com/docs/guides/work-with-skills/).

### Example requests

```text
/espocrm-ops Investigate why our dashboard card totals differ from filtered repository counts. Read-only first.
/espocrm-ops Audit a CSS layout issue using computed styles, DOM geometry and screenshots from isolated staging.
/espocrm-ops Prepare a deployment readiness checklist with SHA256 backup and rollback. Do not deploy.
```

## Design principles

1. Read-only discovery before changing code.
2. No production code-swaps for staging or browser screenshots.
3. No deployment without explicit scope-specific authorization.
4. Errors, missing data and incomplete counts must not silently become zero or healthy.
5. Real browser screenshots and DOM measurements beat mockup-only approvals.
6. Server-side authorization is mandatory; JavaScript filters are not security.
7. Protect credentials and private customer data; sanitize anything made public.

## Structure

```text
espocrm-ops/
  SKILL.md
  references/
    architecture.md
    data-contracts.md
    api-and-pagination.md
    access-control.md
    frontend-and-ux.md
    staging-and-deployment.md
    testing-and-reporting.md
```

## Compatibility

Intended for EspoCRM 9.x customizations. Always verify APIs, PHP versions, folder layout, user/UID, and tool semantics on the *installed* version rather than assuming every deployment behaves identically.

## License

MIT. See [LICENSE](LICENSE).

Maintained by V1 Health as an open learning resource. **Not affiliated with or endorsed by EspoCRM or Hermes Agent.**
