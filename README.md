```
vault login -method=userpass username=reza password=qazwsx
vault kv get kv/reza
vault kv get kv/another-user  # This should result in a permission denied error.
```


```
vault token lookup <user_token>
path "sys/mounts/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}
vault policy write manage-kv manage-kv.hcl

vault write auth/userpass/users/<username> policies="default,manage-kv"
vault secrets enable -path=kv kv

path "kv/*" {
  capabilities = ["create", "read", "update", "delete", "list"]
}

vault write auth/approle/login role_id=<role_id> secret_id=<secret_id>

vault policy write read-default - <<EOF
path "sys/policies/acl/*" {
  capabilities = ["read", "list"]
}
EOF

vault write auth/userpass/users/reza policies="default,read-default"

vault login -method=userpass username=reza password=pass
vault policy read default

vault login <root_token>


vault policy write admin - <<EOF
path "*" {
  capabilities = ["create", "read", "update", "delete", "list", "sudo"]
}
EOF
vault write auth/userpass/users/reza policies="default,admin"
```
