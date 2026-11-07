# JSON schema-drift checker — single-file Python script + verified sample run

## The script (drift_check.py, ~50 lines, stdlib only)

```python
#!/usr/bin/env python3
"""drift_check.py — report field-level JSON schema drift between two API responses."""
import json, sys, urllib.request

def fetch(url):
    req = urllib.request.Request(url, headers={"User-Agent": "drift-check/1.0"})
    with urllib.request.urlopen(req, timeout=20) as r:
        return json.loads(r.read().decode("utf-8"))

def flatten(obj, prefix="", out=None, depth=0):
    """Map 'dotted.path' -> python type name; nested objects at least one level deep."""
    if out is None:
        out = {}
    if isinstance(obj, dict):
        if not obj:
            out[prefix or "<root>"] = "empty-object"
        for k, v in obj.items():
            p = f"{prefix}.{k}" if prefix else k
            if isinstance(v, dict) and v:
                if depth >= 0:
                    flatten(v, p, out, depth + 1)
            elif isinstance(v, dict):
                out[p] = "empty-object"
            else:
                t = "null" if v is None else type(v).__name__
                out[p] = t
    else:
        t = "null" if obj is None else type(obj).__name__
        out[prefix or "<root>"] = t
    return out

def main():
    if len(sys.argv) != 3:
        print("usage: python drift_check.py URL1 URL2"); sys.exit(2)
    a, b = flatten(fetch(sys.argv[1])), flatten(fetch(sys.argv[2]))
    added = sorted(set(b) - set(a))
    removed = sorted(set(a) - set(b))
    changed = sorted(k for k in set(a) & set(b) if a[k] != b[k])
    if not (added or removed or changed):
        print("NO SCHEMA DRIFT"); sys.exit(0)
    print("SCHEMA DRIFT FOUND")
    for k in added:   print(f"  + added:   {k}: {b[k]}")
    for k in removed: print(f"  - removed: {k}: (was {a[k]})")
    for k in changed: print(f"  ~ changed: {k}: {a[k]} -> {b[k]}")
    sys.exit(1)

if __name__ == "__main__":
    main()
```

## Verified sample run (fetched live 2026-11-07)

Command 1 (drift found):
```
$ python3 drift_check.py https://jsonplaceholder.typicode.com/users/1 https://jsonplaceholder.typicode.com/todos/1
SCHEMA DRIFT FOUND
  + added:   completed: bool
  + added:   title: str
  + added:   userId: int
  - removed: address.city: (was str)
  - removed: address.geo.lat: (was str)
  - removed: address.geo.lng: (was str)
  - removed: address.street: (was str)
  - removed: address.suite: (was str)
  - removed: address.zipcode: (was str)
  - removed: company.bs: (was str)
  - removed: company.catchPhrase: (was str)
  - removed: company.name: (was str)
  - removed: email: (was str)
  - removed: name: (was str)
  - removed: phone: (was str)
  - removed: username: (was str)
  - removed: website: (was str)
exit code: 1
```

Command 2 (no drift, same schema different data):
```
$ python3 drift_check.py https://jsonplaceholder.typicode.com/users/1 https://jsonplaceholder.typicode.com/users/2
NO SCHEMA DRIFT
exit code: 0
```

## Edge cases
- Non-dict JSON roots (arrays, scalars) are reported as a single `<root>` type entry, so array item-shape drift inside lists is not tracked (documented limitation).
- `null` values are typed as `null`, so a field changing `str -> null` counts as a type change.
- Empty dicts map to `empty-object` so `{}` vs `{"a":1}` is reported as drift.
- Fields whose only difference is value (not type) are ignored — this is schema drift, not data diffing.
- Nested objects are flattened one-plus levels deep; deeper nesting keeps flattening with dotted paths.
