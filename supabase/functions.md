## Simulate Supabase auth for testing and allow to have on-prem postgres
```sql
SELECT to_jsonb(auth.jwt());

{
    "aal": "aal1",
    "amr": [
        {
            "method": "password",
            "timestamp": 1748326412
        }
    ],
    "aud": "authenticated",
    "exp": 1748345170,
    "iat": 1748341570,
    "iss": "https://tpgyuqvncljnuyrohqre.supabase.co/auth/v1",
    "sub": "4657c4e0-2357-4679-b802-061989d64df3",
    "role": "authenticated",
    "email": "demo@gmail.com",
    "phone": "",
    "session_id": "0c5ab9d1-d6ad-41bb-ad50-a77c6693c4d1",
    "app_metadata": {
        "provider": "email",
        "providers": [
            "email"
        ]
    },
    "is_anonymous": false,
    "user_metadata": {
        "email_verified": true
    }
}

```
```sql
CREATE OR REPLACE FUNCTION public.request_context()
RETURNS TABLE(tenant_id integer, user_id integer, caller_id integer)
LANGUAGE plpgsql
AS $$
DECLARE
  v_user_id uuid;
BEGIN
  -- Fetch user id from incoming jwt
  -- select (auth.jwt() ->> 'sub')::text into v_user_id;

 select auth.uid() into v_user_id;
  

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
