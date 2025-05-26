## Simulate Supabase auth for testing and allow to have on-prem postgres

```sql
CREATE OR REPLACE FUNCTION public.request_context()
RETURNS TABLE(tenant_id integer, user_id integer, caller_id integer)
LANGUAGE plpgsql
AS $$
DECLARE
  v_user_id uuid;
BEGIN
  v_user_id := current_setting('request.jwt.claim.sub', true)::uuid;

  RETURN QUERY
  SELECT
    p.tenant_id::integer,
    p.uid::integer as user_id,
    0 as caller_id
  FROM profiles p
  WHERE p.id = v_user_id;
END;
$$;

```
### Call function locally
```sql

-- Set the user manually for testing only!
SET LOCAL request.jwt.claim.sub = '4657c4e0-2357-4679-b802-061989d64df3';

-- Then call your function
select * from request_context();

```

### Call function inside other function to get current user

```sql
DECLARE
    v_tenant_id INTEGER;
    v_user_id INTEGER;
    v_caller_id INTEGER;
BEGIN
    -- Get tenant/user/caller context
    SELECT tenant_id, user_id, caller_id
    INTO v_tenant_id, v_user_id, v_caller_id
    FROM request_context();
END
```
