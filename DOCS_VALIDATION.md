# DOCS_VALIDATION.md — Documentation Validation Commands

## 1. Goal

Run these checks before coding Phase B.

## 2. OpenAPI Validation

Recommended:

```bash
npx @redocly/cli lint openapi.yaml
```

Alternative syntax-only check:

```bash
python - <<'PY'
import yaml
yaml.safe_load(open('openapi.yaml', encoding='utf-8'))
print('YAML syntax OK')
PY
```

## 3. Task ID Check

```bash
python - <<'PY'
import re
from collections import Counter
text = open('TASKS.md', encoding='utf-8').read()
ids = []
for line in text.splitlines():
    if line.startswith('|') and not line.startswith('|---') and not line.startswith('| ID '):
        parts = [p.strip() for p in line.strip('|').split('|')]
        if parts and re.match(r'^[A-Z]+-[A-Z0-9]+', parts[0]):
            ids.append(parts[0])
dups = [x for x, c in Counter(ids).items() if c > 1]
print('Duplicate task IDs:', dups or 'NONE')
PY
```

## 4. Security Naming Check

Search for deprecated terms:

```bash
grep -R "AdminCookieAuth\|CsrfToken" .
```

Expected:

```text
No results
```

Use only:

```text
cookieAuth
csrfHeader
```

## 5. Deprecated Endpoint Check

The deprecated direct attach endpoint must not be implemented in Phase 1.

Search:

```bash
grep -R "POST /api/v1/admin/products/:id/media" .
```

Allowed only in documentation as deprecated warning.

## 6. Deferred Scope Check

Phase 1 must not implement:

```text
product_colors
product_specs
site_settings
```

unless explicitly re-scoped by Human.


## 7. Language Policy Check

Confirm file exists:

```bash
test -f LANGUAGE_POLICY.md && echo "LANGUAGE_POLICY.md exists"
```

Check English UI terms in prompts/docs manually before FE implementation.

Allowed English:

```text
Code identifiers
API fields
Enum values
Task IDs
File names
```

Not allowed in UI:

```text
View details
Add product
Save changes
Contact us
Upload media
```


## 8. Responsive Requirement Check

Confirm file exists:

```bash
test -f MOBILE_RESPONSIVE_REQUIREMENTS.md && echo "MOBILE_RESPONSIVE_REQUIREMENTS.md exists"
```

Confirm TASKS.md includes:

```text
FE-RESP-01
QA-RESP-01
```

Confirm Frontend Agent reads:

```text
MOBILE_RESPONSIVE_REQUIREMENTS.md
```


## 9. Android/iOS Compatibility Check

Confirm file exists:

```bash
test -f MOBILE_BROWSER_COMPATIBILITY.md && echo "MOBILE_BROWSER_COMPATIBILITY.md exists"
```

Confirm TASKS.md includes:

```text
FE-MOB-01
QA-MOB-01
```

Confirm Frontend Agent reads:

```text
MOBILE_BROWSER_COMPATIBILITY.md
```
