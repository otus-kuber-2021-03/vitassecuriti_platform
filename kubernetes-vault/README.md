# Homework 11

```
$ helm status consul
NAME: consul
LAST DEPLOYED: Fri Aug 20 22:21:37 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
Thank you for installing HashiCorp Consul!

Now that you have deployed Consul, you should look over the docs on using 
Consul with Kubernetes available here: 

https://www.consul.io/docs/platform/k8s/index.html


Your release is named consul.

To learn more about the release, run:

  $ helm status consul
  $ helm get all consul
```

-------------------
```
$ helm status vault
NAME: vault
LAST DEPLOYED: Fri Aug 20 22:25:12 2021
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
Thank you for installing HashiCorp Vault!

Now that you have deployed Vault, you should look over the docs on using
Vault with Kubernetes available here:

https://www.vaultproject.io/docs/


Your release is named vault. To learn more about the release, try:

  $ helm status vault
  $ helm get manifest vault
```
  -----------------------------
```
  $ kubectl logs vault-0
==> Vault server configuration:

             Api Address: http://10.244.2.5:8200
                     Cgo: disabled
         Cluster Address: https://vault-0.vault-internal:8201
              Go Version: go1.16.5
              Listener 1: tcp (addr: "[::]:8200", cluster address: "[::]:8201", max_request_duration: "1m30s", max_request_size: "33554432", tls: "disabled")
               Log Level: info
                   Mlock: supported: true, enabled: false
           Recovery Mode: false
                 Storage: consul (HA available)
                 Version: Vault v1.8.0
             Version Sha: 82a99f14eb6133f99a975e653d4dac21c17505c7

==> Vault server started! Log data will stream in below:

2021-08-20T21:26:04.629Z [INFO]  proxy environment: http_proxy="" https_proxy="" no_proxy=""
2021-08-20T21:26:04.629Z [WARN]  storage.consul: appending trailing forward slash to path
2021-08-20T21:26:14.214Z [INFO]  core: security barrier not initialized
2021-08-20T21:26:14.218Z [INFO]  core: seal configuration missing, not initialized
2021-08-20T21:26:18.149Z [INFO]  core: security barrier not initialized
2021-08-20T21:26:18.154Z [INFO]  core: seal configuration missing, not initialized
2021-08-20T21:26:23.099Z [INFO]  core: security barrier not initialized
2021-08-20T21:26:23.101Z [INFO]  core: seal configuration missing, not initialized
2021-08-20T21:26:28.083Z [INFO]  core: security barrier not initialized
2021-08-20T21:26:28.085Z [INFO]  core: seal configuration missing, not initialized
2021-08-20T21:26:33.149Z [INFO]  core: security barrier not initialized
2021-08-20T21:26:33.152Z [INFO]  core: seal configuration missing, not initialized
2021-08-20T21:26:38.114Z [INFO]  core: security barrier not initialized
2021-08-20T21:26:38.116Z [INFO]  core: seal configuration missing, not initialized
```
-----------
```
$ kubectl get pods
NAME                                    READY   STATUS    RESTARTS   AGE
consul-ntcdc                            1/1     Running   0          5m1s
consul-server-0                         1/1     Running   0          5m
consul-server-1                         1/1     Running   0          5m
consul-server-2                         1/1     Running   0          5m
consul-vgxhn                            1/1     Running   0          5m1s
consul-xmvv5                            1/1     Running   0          5m1s
vault-0                                 0/1     Running   0          85s
vault-1                                 0/1     Running   0          84s
vault-2                                 0/1     Running   0          83s
vault-agent-injector-6748569f56-g8gpr   1/1     Running   0          85s
```
-------------------------
```
$ kubectl exec -it vault-0 -- vault operator init -key-shares=1 -key-threshold=1
Unseal Key 1: 0V8U0EDl1CR28hfcgecXMGeu2UB6SbaxhGyRULtpT+I=

Initial Root Token: s.6sHD9iE979S5PDSdPJN4JmvY

Vault initialized with 1 key shares and a key threshold of 1. Please securely
distribute the key shares printed above. When the Vault is re-sealed,
restarted, or stopped, you must supply at least 1 of these keys to unseal it
before it can start servicing requests.

Vault does not store the generated master key. Without at least 1 keys to
reconstruct the master key, Vault will remain permanently sealed!

It is possible to generate new unseal keys, provided you have a quorum of
existing unseal keys shares. See "vault operator rekey" for more information.
```
-------
```
$ kubectl logs vault-0
...

2021-08-20T21:30:18.129Z [INFO]  core: security barrier not initialized
2021-08-20T21:30:18.150Z [INFO]  core: seal configuration missing, not initialized
2021-08-20T21:30:23.240Z [INFO]  core: security barrier not initialized
2021-08-20T21:30:23.338Z [INFO]  core: security barrier not initialized
2021-08-20T21:30:23.342Z [INFO]  core: seal configuration missing, not initialized
2021-08-20T21:30:24.318Z [INFO]  core: security barrier initialized: stored=1 shares=1 threshold=1
2021-08-20T21:30:24.910Z [INFO]  core: post-unseal setup starting
2021-08-20T21:30:24.957Z [INFO]  core: loaded wrapping token key
2021-08-20T21:30:24.957Z [INFO]  core: successfully setup plugin catalog: plugin-directory=""
2021-08-20T21:30:25.003Z [INFO]  core: no mounts; adding default mount table
2021-08-20T21:30:25.100Z [INFO]  core: successfully mounted backend: type=cubbyhole path=cubbyhole/
2021-08-20T21:30:25.100Z [INFO]  core: successfully mounted backend: type=system path=sys/
2021-08-20T21:30:25.101Z [INFO]  core: successfully mounted backend: type=identity path=identity/
2021-08-20T21:30:25.339Z [INFO]  core: successfully enabled credential backend: type=token path=token/
2021-08-20T21:30:25.401Z [INFO]  core: restoring leases
2021-08-20T21:30:25.401Z [INFO]  rollback: starting rollback manager
2021-08-20T21:30:25.405Z [INFO]  expiration: lease restore complete
2021-08-20T21:30:25.518Z [INFO]  identity: entities restored
2021-08-20T21:30:25.521Z [INFO]  identity: groups restored
2021-08-20T21:30:25.533Z [INFO]  core: usage gauge collection is disabled
2021-08-20T21:30:25.616Z [INFO]  core: post-unseal setup complete
2021-08-20T21:30:25.801Z [INFO]  core: root token generated
2021-08-20T21:30:25.801Z [INFO]  core: pre-seal teardown starting
2021-08-20T21:30:25.801Z [INFO]  rollback: stopping rollback manager
2021-08-20T21:30:25.801Z [INFO]  core: pre-seal teardown complete
```

-------
```
$ kubectl exec -it vault-0 -- vault status
Key                Value
---                -----
Seal Type          shamir
Initialized        true
Sealed             true
Total Shares       1
Threshold          1
Unseal Progress    0/1
Unseal Nonce       n/a
Version            1.8.0
Storage Type       consul
HA Enabled         true
command terminated with exit code 2
```

-------
```
$ kubectl exec -it vault-0 env | grep VAULT
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
VAULT_CLUSTER_ADDR=https://vault-0.vault-internal:8201
VAULT_API_ADDR=http://10.244.2.5:8200
VAULT_K8S_POD_NAME=vault-0
VAULT_K8S_NAMESPACE=default
VAULT_ADDR=http://127.0.0.1:8200
...
```
----
```
kubectl exec -it vault-0 -- vault operator unseal '0V8U0EDl1CR28hfcgecXMGeu2UB6SbaxhGyRULtpT+I='

Key                    Value
---                    -----
Seal Type              shamir
Initialized            true
Sealed                 false
Total Shares           1
Threshold              1
Version                1.8.0
Storage Type           consul
Cluster Name           vault-cluster-012484ac
Cluster ID             665cb964-18cb-3ee1-554a-ac9df01c5d40
HA Enabled             true
HA Cluster             n/a
HA Mode                standby
Active Node Address    <none>
```
```
kubectl exec -it vault-1 -- vault operator unseal '0V8U0EDl1CR28hfcgecXMGeu2UB6SbaxhGyRULtpT+I='

Key                    Value
---                    -----
Seal Type              shamir
Initialized            true
Sealed                 false
Total Shares           1
Threshold              1
Version                1.8.0
Storage Type           consul
Cluster Name           vault-cluster-012484ac
Cluster ID             665cb964-18cb-3ee1-554a-ac9df01c5d40
HA Enabled             true
HA Cluster             https://vault-0.vault-internal:8201
HA Mode                standby
Active Node Address    http://10.244.2.5:8200
```
```
kubectl exec -it vault-2 -- vault operator unseal '0V8U0EDl1CR28hfcgecXMGeu2UB6SbaxhGyRULtpT+I='

Key                    Value
---                    -----
Seal Type              shamir
Initialized            true
Sealed                 false
Total Shares           1
Threshold              1
Version                1.8.0
Storage Type           consul
Cluster Name           vault-cluster-012484ac
Cluster ID             665cb964-18cb-3ee1-554a-ac9df01c5d40
HA Enabled             true
HA Cluster             https://vault-0.vault-internal:8201
HA Mode                standby
Active Node Address    http://10.244.2.5:8200
```
-----
```
$ kubectl exec -it vault-0 -- vault auth list
Error listing enabled authentications: Error making API request.

URL: GET http://127.0.0.1:8200/v1/sys/auth
Code: 400. Errors:

* missing client token
command terminated with exit code 2
```
-----
```

$ kubectl exec -it vault-0 -- vault login
Token (will be hidden): 
Success! You are now authenticated. The token information displayed below
is already stored in the token helper. You do NOT need to run "vault login"
again. Future Vault requests will automatically use this token.

Key                  Value
---                  -----
token                s.6sHD9iE979S5PDSdPJN4JmvY
token_accessor       IGzaopROI9zfwmxg13qi3XUv
token_duration       ∞
token_renewable      false
token_policies       ["root"]
identity_policies    []
policies             ["root"]
```

---
```
$ kubectl exec -it vault-0 -- vault auth list
Path      Type     Accessor               Description
----      ----     --------               -----------
token/    token    auth_token_148216db    token based credentials

```
-----
```
$ kubectl exec -it vault-0 -- vault auth list
Path      Type     Accessor               Description
----      ----     --------               -----------
token/    token    auth_token_148216db    token based credentials
```
```
$ kubectl exec -it vault-0 -- vault secrets enable --path=otus kv
Success! Enabled the kv secrets engine at: otus/
```
```
$ kubectl exec -it vault-0 -- vault secrets list --detailed
Path          Plugin       Accessor              Default TTL    Max TTL    Force No Cache    Replication    Seal Wrap    External Entropy Access    Options    Description                                                UUID
----          ------       --------              -----------    -------    --------------    -----------    ---------    -----------------------    -------    -----------                                                ----
cubbyhole/    cubbyhole    cubbyhole_29be3f9e    n/a            n/a        false             local          false        false                      map[]      per-token private secret storage                           5d8e0eed-f26d-42ec-4358-db4a8c5bf67d
identity/     identity     identity_b0692bc4     system         system     false             replicated     false        false                      map[]      identity store                                             d95cbcdc-7360-b92e-8eb4-d557fbc05719
otus/         kv           kv_c6960386           system         system     false             replicated     false        false                      map[]      n/a                                                        0d1b86b0-661b-5b50-91af-1ab7d9904d8a
sys/          system       system_18c5f57c       n/a            n/a        false             replicated     false        false                      map[]      system endpoints used for control, policy and debugging    2ec5cdc6-0d17-43ce-c3a4-297c0706d693
```
```
$ kubectl exec -it vault-0 -- vault kv put otus/otus-ro/config username='otus'
password='asajkjkahs'Success! Data written to: otus/otus-ro/config
```
```
$ kubectl exec -it vault-0 -- vault kv put otus/otus-ro/config username='otus' password='asajkjkahs'
Success! Data written to: otus/otus-ro/config
```
```
$ kubectl exec -it vault-0 -- vault kv put otus/otus-rw/config username='otus' password='asajkjkahs'
Success! Data written to: otus/otus-rw/config
```
```
$ kubectl exec -it vault-0 -- vault read otus/otus-ro/config
Key                 Value
---                 -----
refresh_interval    768h
password            asajkjkahs
username            otus
```
```
$ kubectl exec -it vault-0 -- vault kv get otus/otus-rw/config
====== Data ======
Key         Value
---         -----
password    asajkjkahs
username    otus
```
----
```
$ kubectl exec -it vault-0 -- vault auth enable kubernetes
Success! Enabled kubernetes auth method at: kubernetes/
```
---
```
$ kubectl exec -it vault-0 -- vault auth list
Path           Type          Accessor                    Description
----           ----          --------                    -----------
kubernetes/    kubernetes    auth_kubernetes_885b3832    n/a
token/         token         auth_token_148216db         token based credentials
```
---
```
$ kubectl create serviceaccount vault-auth
serviceaccount/vault-auth created
```
---
```
$ kubectl apply -f vault-auth-service-account.yml
clusterrolebinding.rbac.authorization.k8s.io/role-tokenreview-binding created
```
---
```
export VAULT_SA_NAME=$(kubectl get sa vault-auth -o jsonpath="{.secrets[*]['name']}")

$ echo $VAULT_SA_NAME
vault-auth-token-7s59r

export SA_JWT_TOKEN=$(kubectl get secret $VAULT_SA_NAME -o jsonpath="{.data.token}" | base64 --decode; echo)

$ echo $SA_JWT_TOKEN
eyJhbGciOiJSUzI1NiIsImtpZCI6IjByaHZNSlhHaUEyWkZlRkpmT1p4VDh4endVX1lmR2kzOEtPVl9LcFNrZlEifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJkZWZhdWx0Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6InZhdWx0LWF1dGgtdG9rZW4tN3M1OXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoidmF1bHQtYXV0aCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6ImI4OTkwNDFiLTQ2MjktNDA4ZS05Nzg2LWEyYThlOTJmMGNiNiIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OnZhdWx0LWF1dGgifQ.bZBS7TguIppG7o71Jehpx5nipAlIWcTRY2lSBzcJms_MfTlWtYcEMxR7w9ZzyvL2r0GN6yJCJsvUD82wmtiTe-8Np4H4AplUm1buEN32zoeagXy11IUmvMc2GJuOo2xL_XatlTNlD67WCTe8cX3a1IMBj8gdhwcdOXoiaLy9Ee-XOkS2sZPJ-pqpVDp6dFYrI6I9FsK7cC4_vrJzoaVpmHSUkMZLrseCQATHIZdt9avONLP0fuhjSw4g82RrpgD49FTWISH9N1kQV7mbaJqsJNa1ySkhWLeI0q32X_IjAGwvczhcXpcUTPj0qZ8_5NH9ztcwZJwd5l6j15V0U8cX2Q


export SA_CA_CRT=$(kubectl get secret $VAULT_SA_NAME -o jsonpath="{.data['ca\.crt']}" | base64 --decode; echo)

$ echo $SA_CA_CRT

-----BEGIN CERTIFICATE----- MIIC5zCCAc+gAwIBAgIBADANBgkqhkiG9w0BAQsFADAVMRMwEQYDVQQDEwprdWJl cm5ldGVzMB4XDTIxMDgyMDIxMTc1MVoXDTMxMDgxODIxMTc1MVowFTETMBEGA1UE AxMKa3ViZXJuZXRlczCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAJw1 dXW4idhQLrt/9L7LbYOcjNs8MmkZLULjvV8Do1fL6TH/5Op4X5E1lXyqFQ0iKLeX nxA3Vs4IyblxxQ93V9uvC4E/PxA+qvlvz1mG58d0JvSJOgP9LNofR+Sv4SUpv1Vh uFYfjXYD5/1NCbSG52ZMIaDXr4oPRdxeYfPMF3BvmzoaGk9gggHiF3vIwZv4Rq/S AXS1L8vik5Du1SDPjK92Bv9kxiBkEtLg69dy3KSgn2uM7KluRMGxL/Tq+yUd/xhq bl3kPmWGsIEW06h/Ub69Bgt1HzPCq4U/rrcZQPFEMjAnuJ2wL2lJxXPdU2mVwznI nAKZeWcfvyTPy+iDR5kCAwEAAaNCMEAwDgYDVR0PAQH/BAQDAgKkMA8GA1UdEwEB /wQFMAMBAf8wHQYDVR0OBBYEFDGVuGE4cyrySP8Plws5gBRiR0afMA0GCSqGSIb3 DQEBCwUAA4IBAQBPDjXhu9YVWVFVxTt8fWx4JoHxhajRxNuztWv8U+eVsJ0a/DBy T3ukdnvK+vm3o6UX1MIu4qdqBHjVWnf1t/eF8czdidcpRxN4/YIVM6W1qmsdgros 3pFc+eXvgnR1PdxN6uXqO5kfQvKlJkAGmve21hknti1YnMN5iwpEym+RjGDxOce9 SGruX9e/Ov7tPmB0uWJp+xrtc1p6zxeeLGOWc2/2/Lhkn8nrY55vzScr+OPsfB87 NDIT5/uVWGl6vZx/meHB9KgLsbe9HkEZ2nI6a3DcmFtVF4PWb/xLMZvxslhM0OYm LNsjMVMGpXpg4O1lhsJLNLrdYfnUvPM1U0uq -----END CERTIFICATE-----

export K8S_HOST=$(more ~/.kube/config | grep server |awk '/http/ {print $NF}')

$ echo $K8S_HOST
https://34.71.116.197 https://34.121.39.251 https://34.133.94.119 https://34.136.195.107 https://127.0.0.1:36238
```
---
```
kubectl exec -it vault-0 -- vault write auth/kubernetes/config token_reviewer_jwt="$SA_JWT_TOKEN" kubernetes_host="$K8S_HOST" kubernetes_ca_cert="$SA_CA_CRT"
```
---
```
$ kubectl exec -it vault-0 -- vault write auth/kubernetes/config token_reviewer_jwt="$SA_JWT_TOKEN" kubernetes_host="$K8S_HOST" kubernetes_ca_cert="$SA_CA_CRT"
Success! Data written to: auth/kubernetes/config
```
---
```
$ kubectl cp otus-policy.hcl vault-0:/tmp/

$kubectl exec -it vault-0 -- vault policy write otus-policy /tmp/otus-policy.hcl
Success! Uploaded policy: otus-policy

$kubectl exec -it vault-0 -- vault write auth/kubernetes/role/otus bound_service_account_names=vault-auth bound_service_account_namespaces=default policies=otus-policy ttl=24h
Success! Data written to: auth/kubernetes/role/otus
```
---
```
$ kubectl run --generator=run-pod/v1 tmp --rm -i --tty --serviceaccount=vault-auth --image alpine:3.7 

#apk add curl jq

#VAULT_ADDR=http://vault:8200
#KUBE_TOKEN=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)

#TOKEN=$(curl -k -s --request POST --data '{"jwt": "'$KUBE_TOKEN'", "role": "test"}' $VAULT_ADDR/v1/auth/kubernetes/login | jq '.auth.client_token' | awk -F\" '{print $2}')

```
---
!!!!
Так как otus/otus-rw имеет разрешение create мы можем записать otus-rw/config1, но otusrw/config уже существует, а на изменение разрешения нет. Для разрешения внесения изменения необзодимо добавить разрешение update 


---
```
$ kubectl get pods
NAME                                    READY   STATUS    RESTARTS   AGE
consul-ntcdc                            1/1     Running   1          143m
consul-server-0                         1/1     Running   1          143m
consul-server-1                         1/1     Running   1          143m
consul-server-2                         1/1     Running   1          143m
consul-vgxhn                            1/1     Running   1          143m
consul-xmvv5                            1/1     Running   1          143m
vault-0                                 1/1     Running   1          140m
vault-1                                 1/1     Running   1          140m
vault-2                                 1/1     Running   1          139m
vault-agent-injector-6748569f56-g8gpr   1/1     Running   3          140m
vault-agent-otus                        1/1     Running   1          4m41s

```
---
```
$kubectl exec -it vault-0 -- vault secrets enable pki
Success! Enabled the pki secrets engine at: pki/

$kubectl exec -it vault-0 -- vault secrets tune -max-lease-ttl=87600h pki
Success! Tuned the secrets engine at: pki/

kubectl exec -it vault-0 -- vault write -field=certificate pki/root/generate/internal common_name="exmaple.ru" ttl=87600h > CA_cert.crt
```
---
```
kubectl exec -it vault-0 -- vault write pki/config/urls issuing_certificates="http://vault:8200/v1/pki/ca" crl_distribution_points="http://vault:8200/v1/pki/crl"
Success! Data written to: pki/config/urls
```
---
```
$kubectl exec -it vault-0 -- vault secrets enable --path=pki_int pki
kubectl exec -it vault-0 -- vault secrets enable --path=pki_int pki

$kubectl exec -it vault-0 -- vault secrets tune -max-lease-ttl=87600h pki_int
Success! Tuned the secrets engine at: pki_int/

$kubectl exec -it vault-0 -- vault write -format=json pki_int/intermediate/generate/internal common_name="example.ru Intermediate Authority" | jq -r '.data.csr' > pki_intermediate.csr 
```
---
```
$kubectl cp pki_intermediate.csr vault-0:/tmp/
$kubectl exec -it vault-0 -- vault write -format=json pki/root/sign-intermediate csr=@/tmp/pki_intermediate.csr format=pem_bundle ttl="43800h" | jq -r '.data.certificate' > intermediate.cert.pem
$kubectl cp intermediate.cert.pem vault-0:/tmp/

$kubectl exec -it vault-0 -- vault write pki_int/intermediate/set-signed certificate=@/tmp/intermediate.cert.pem
Success! Data written to: pki_int/intermediate/set-signed
```
---
```
$kubectl exec -it vault-0 -- vault write pki_int/roles/example-dot-ru allowed_domains="example.ru" allow_subdomains=true max_ttl="720h"
Success! Data written to: pki_int/roles/example-dot-ru
```
---
```
$ kubectl exec -it vault-0 -- vault write pki_int/issue/example-dot-ru common_name="gitlab.example.ru" ttl="24h"
Key                 Value
---                 -----
ca_chain            [-----BEGIN CERTIFICATE-----
MIIDnDCCAoSgAwIBAgIUYLcSSaKkTA5ph+EGYReG1Uc3ULkwDQYJKoZIhvcNAQEL
BQAwFTETMBEGA1UEAxMKZXhtYXBsZS5ydTAeFw0yMTA4MjAyMzU3MDdaFw0yNjA4
MTkyMzU3MzdaMCwxKjAoBgNVBAMTIWV4YW1wbGUucnUgSW50ZXJtZWRpYXRlIEF1
dGhvcml0eTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKgmZTr8KObQ
L0/6/gwd50r4IcOrlQSzMRjDdBBC23wUB0A38HmXne23SlH8jk0tYhQAJJrHSG3A
jS21Yues9ti3NvQH9l1HroeGABa4LHUab1Kw8eLhkTM6CnD7ilcQyhbSBYFjlWst
ND4k3ZU7mIpYg+efO11gglJrRxCKC9SibeQZV+yua/zOz0kb5mpDwZyrj4KO6Q0C
lEbzh+Lysi9PPxK8Ul+x2XvBd4m2lnjSdU0VABaXTrKcDQxkqMLxn91gCVcJI4x+
aaifejdsdWDpb+tzuC7CAzKSrOn5a2/CKfXHps6fOW5eVAQoUowr1MiDeOr5uCX/
Drg62unbg80CAwEAAaOBzDCByTAOBgNVHQ8BAf8EBAMCAQYwDwYDVR0TAQH/BAUw
AwEB/zAdBgNVHQ4EFgQUjhbKPSjVXQ5EgAWT1T8Vp97xDQkwHwYDVR0jBBgwFoAU
mG0GpwLo4jlCrNKHlaPsMpbGp4gwNwYIKwYBBQUHAQEEKzApMCcGCCsGAQUFBzAC
hhtodHRwOi8vdmF1bHQ6ODIwMC92MS9wa2kvY2EwLQYDVR0fBCYwJDAioCCgHoYc
aHR0cDovL3ZhdWx0OjgyMDAvdjEvcGtpL2NybDANBgkqhkiG9w0BAQsFAAOCAQEA
FqpJxSJ/sa9M2LElPaYDD0FGYZd2rULXJmGNYcv1qpVbG9FZMH74pK1BJ5AF6WyI
GWr3FfK9tNQ7XQqt5kDtxuf3ZN3MhodTbAzwThCOuXVQxq18oHPV70h+HWzOIPdz
5d0Q4hkQd4rlA7ShoRDKKamJH6EoaS2IQaZ1fldYJeX9kh6wsPe/MVOtBPk4hNio
R4Enk6alXOpXPTSbRK/+/JGXXUL5n8C5KaUErS7TfwDR6AmnsA6t8Xfc2FtSSAyX
w7MGJmAobY5qk0RxFe29JEoSfa/dFQcOcYEEnvP+YYbka2gG5QlsTr0sWnBG9MAY
QtOzQ+FKPFRiYgATWkV1Lg==
-----END CERTIFICATE-----]
certificate         -----BEGIN CERTIFICATE-----
MIIDZzCCAk+gAwIBAgIUQd3pzWUU7Y+aI7nlv8QPEZPhNCgwDQYJKoZIhvcNAQEL
BQAwLDEqMCgGA1UEAxMhZXhhbXBsZS5ydSBJbnRlcm1lZGlhdGUgQXV0aG9yaXR5
MB4XDTIxMDgyMTAwMDI0MloXDTIxMDgyMjAwMDMxMVowHDEaMBgGA1UEAxMRZ2l0
bGFiLmV4YW1wbGUucnUwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDx
FKoLk6fC0h+sw4Pz2tQGqd0xlcf91Jlh9SnvOkuidqeOT8/yj/O36gCxMeUuT8ft
/jyniBWECk/+ZWCzO3wVe5ERUrrChtDE+NmWwWIKNpgZkrOQ76JO2LOeXdkiJWJg
6e3SVe0Y1l0ifskFe1YrBQYsrQzhTsPvGyr+Idpffzeb7GYAoOVUnFCOgeQwsTo+
2wVJWd6cHKM47zO5GfA6yf9w1RwwDQq3X4yBlOwz1i3Le08Xy4PcAY8e8Sh+8CNO
ByJ+sBlIglIeROnogtFklM8i5MktSSE5CkoicXBCKVb7hBRowJFIPbPObhEUi8xE
tqIyVn8BFsi7AwOLD+ShAgMBAAGjgZAwgY0wDgYDVR0PAQH/BAQDAgOoMB0GA1Ud
JQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjAdBgNVHQ4EFgQUP2ZUjJodol+8y3DL
NMZH3iWz0YEwHwYDVR0jBBgwFoAUjhbKPSjVXQ5EgAWT1T8Vp97xDQkwHAYDVR0R
BBUwE4IRZ2l0bGFiLmV4YW1wbGUucnUwDQYJKoZIhvcNAQELBQADggEBADXqrzIu
s2geAdxhz8yXDnFBnWj7tlYzDWQRjyU/+jyyirtiO4OaalBT0ojyE0bZPIEfM6rB
jDpX7ONWErIqrjHwVH6idhgxbhdM3pcRxKQgblb+F1j7O6RXEh1SFQlUIz+xiV7X
Bj4lQxxKbpsgFZ7pxeWlIa3/Bk9uxDuEQopSTysOn3m7Plj1VGNOvjYgFxcEQuip
Sv3pgqg1pcW3r7qGkOf43aqZs6BfmKvsx3HzF15epST2w7wqwxFFFQaIHTyK593V
XXyKBBxfsQ0RPSfAOOqme/p0xYnBRVwKM1iwbUQ8TR3yBB4syuTTo1vOHNhkNdgG
FX07QeZwrqiNIIo=
-----END CERTIFICATE-----
expiration          1629590591
issuing_ca          -----BEGIN CERTIFICATE-----
MIIDnDCCAoSgAwIBAgIUYLcSSaKkTA5ph+EGYReG1Uc3ULkwDQYJKoZIhvcNAQEL
BQAwFTETMBEGA1UEAxMKZXhtYXBsZS5ydTAeFw0yMTA4MjAyMzU3MDdaFw0yNjA4
MTkyMzU3MzdaMCwxKjAoBgNVBAMTIWV4YW1wbGUucnUgSW50ZXJtZWRpYXRlIEF1
dGhvcml0eTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKgmZTr8KObQ
L0/6/gwd50r4IcOrlQSzMRjDdBBC23wUB0A38HmXne23SlH8jk0tYhQAJJrHSG3A
jS21Yues9ti3NvQH9l1HroeGABa4LHUab1Kw8eLhkTM6CnD7ilcQyhbSBYFjlWst
ND4k3ZU7mIpYg+efO11gglJrRxCKC9SibeQZV+yua/zOz0kb5mpDwZyrj4KO6Q0C
lEbzh+Lysi9PPxK8Ul+x2XvBd4m2lnjSdU0VABaXTrKcDQxkqMLxn91gCVcJI4x+
aaifejdsdWDpb+tzuC7CAzKSrOn5a2/CKfXHps6fOW5eVAQoUowr1MiDeOr5uCX/
Drg62unbg80CAwEAAaOBzDCByTAOBgNVHQ8BAf8EBAMCAQYwDwYDVR0TAQH/BAUw
AwEB/zAdBgNVHQ4EFgQUjhbKPSjVXQ5EgAWT1T8Vp97xDQkwHwYDVR0jBBgwFoAU
mG0GpwLo4jlCrNKHlaPsMpbGp4gwNwYIKwYBBQUHAQEEKzApMCcGCCsGAQUFBzAC
hhtodHRwOi8vdmF1bHQ6ODIwMC92MS9wa2kvY2EwLQYDVR0fBCYwJDAioCCgHoYc
aHR0cDovL3ZhdWx0OjgyMDAvdjEvcGtpL2NybDANBgkqhkiG9w0BAQsFAAOCAQEA
FqpJxSJ/sa9M2LElPaYDD0FGYZd2rULXJmGNYcv1qpVbG9FZMH74pK1BJ5AF6WyI
GWr3FfK9tNQ7XQqt5kDtxuf3ZN3MhodTbAzwThCOuXVQxq18oHPV70h+HWzOIPdz
5d0Q4hkQd4rlA7ShoRDKKamJH6EoaS2IQaZ1fldYJeX9kh6wsPe/MVOtBPk4hNio
R4Enk6alXOpXPTSbRK/+/JGXXUL5n8C5KaUErS7TfwDR6AmnsA6t8Xfc2FtSSAyX
w7MGJmAobY5qk0RxFe29JEoSfa/dFQcOcYEEnvP+YYbka2gG5QlsTr0sWnBG9MAY
QtOzQ+FKPFRiYgATWkV1Lg==
-----END CERTIFICATE-----
private_key         -----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEA8RSqC5OnwtIfrMOD89rUBqndMZXH/dSZYfUp7zpLonanjk/P
8o/zt+oAsTHlLk/H7f48p4gVhApP/mVgszt8FXuREVK6wobQxPjZlsFiCjaYGZKz
kO+iTtiznl3ZIiViYOnt0lXtGNZdIn7JBXtWKwUGLK0M4U7D7xsq/iHaX383m+xm
AKDlVJxQjoHkMLE6PtsFSVnenByjOO8zuRnwOsn/cNUcMA0Kt1+MgZTsM9Yty3tP
F8uD3AGPHvEofvAjTgcifrAZSIJSHkTp6ILRZJTPIuTJLUkhOQpKInFwQilW+4QU
aMCRSD2zzm4RFIvMRLaiMlZ/ARbIuwMDiw/koQIDAQABAoIBAEsmIt+a9md/coo7
JA3Gv+MX3jCPvRi9xdZIQvskk+Ef1ZlB/dNh1hoVYoPZxtQJ4IuqfaPHgtV3FXp6
hYs5VrOnog/hVwA+YCOWYtVgkLwYSo9mMH1UhabIXC1Ymc/QEXueUBkJ2e+tGrkf
BnCkArdESKlyhBpToDYPpPY0/UpY1c2Tb0hvWjLISLZ6FWRq1OT0NPHQN/5J+jAv
tQHyKLuwvcM3SravkE2I1A5dAkaKxdJkzkzbAEjJ/DS4w341i7l27lJ808Lts7jI
SIRcvUdG7KqZk+751/QFrVw28BWKrGn4PqlO141B+DSRVvAQsgqX1acuHiW+0H4z
WkYr78ECgYEA+eKT4o+kS6az942qA2vhByO/u/j9uiZXtwCkGf+NTS3Exbyff6lD
kfF+r78UQNfARY7NPinFr7QD+cb7O+/D9W5azi83COtlIbGQZSwxHYfrjS/OP0Na
/nfDXwIxlsyLmhEQa52FWKBdyD23zj4fR2aLh/YVELmp67DOc6bRSdkCgYEA9vru
XQW2LfWa6UDEezjf30zEOIjMZj4Utykk+maw4ZVd0/BFU5Hy5mt2zahC7sIWx6D1
lecToulEvFG+2/fiAgB4vbSFzhbYgo+ES29S/2/87d8Cjqswc6h7fp6fdXRUF7V5
YNH6tmFOEEzPoWSbiOY3B3Nu8KSR9yEwgjA7LAkCgYEA01gsJdHBbm6HBdgeNBiY
0VYOAyi7SCbHxzLWmFqIov8Tzv4SlIGPca8jq/bbZWBU8T+vHWVtGocRWb9Om8nV
Hg6A5KQQUw3skgvBCaDPxZ9AvT/ym5UXL+QRLJkJYfaMF/lYvvwSXPv9da+ldt0Z
zWTQnGSoOmYdbgczBaPQnTkCgYAvMJ7eszqt6WGh87gW+zT7S4Wqb6juWCpJHNlt
5rUhRRLabewxHY/Vqu7WOLIhQIBtwDlsXOyJkhyKBux6xxAt5b0sMhPm3sKbn6Rd
bXXkTJd9M8EfVWI6lxSRiulY3dM7fHcmorhOpKTvxF1frwNw0tvs1od5/1fMalEE
bwa/UQKBgEbJ6Wi/hAS/UrWaIgmEOJxsyuAy4guhPUhcpBlG3GGK/OOR1uRLq5GX
nNuI4/PxHmB89tt9qppHsaiXNVWjuUxdV3gSI72ZX3GdB526WtqbEsP8uQJCpt3t
gagLL7tkEWMiWxfk/VqNtK8d8MbczlPrwMv2bpOTbyiZatlZsLs5
-----END RSA PRIVATE KEY-----
private_key_type    rsa
serial_number       41:dd:e9:cd:65:14:ed:8f:9a:23:b9:e5:bf:c4:0f:11:93:e1:34:28
```
---
```
$ kubectl exec -it vault-0 -- vault write pki_int/revoke serial_number="41:dd:e9:cd:65:14:ed:8f:9a:23:b9:e5:bf:c4:0f:11:93:e1:34:28"
Key                        Value
---                        -----
revocation_time            1629504248
revocation_time_rfc3339    2021-08-21T00:04:08.233993161
```
