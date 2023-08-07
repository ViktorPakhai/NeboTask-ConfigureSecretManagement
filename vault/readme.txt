

1. Install Kubernetes Vault и Vault CSI Provider:

helm repo add hashicorp https://helm.releases.hashicorp.com

helm install vault hashicorp/vault    --set "server.dev.enabled=true"     --set "injector.enabled=false"     --set "csi.enabled=true"

helm install csi secrets-store-csi-driver/secrets-store-csi-driver     --set syncSecret.enabled=true    --set enableSecretRotation=true     --set "rotationPollInterval=30s"



########## Case with vault cluster ##############
helm install vault hashicorp/vault --values helm-vault-raft-values.yml
kubectl exec vault-0 -- vault operator init -key-shares=1 -key-threshold=1  -format=json > cluster-keys.json

kubectl exec vault-0 -- vault operator unseal SOeMqssDeuSuJahfPCj4HKldYBlUR+DfwMJ7to4xXJY=

kubectl exec -ti vault-1 -- vault operator raft join http://vault-0.vault-internal:8200
kubectl exec -ti vault-2 -- vault operator raft join http://vault-0.vault-internal:8200

kubectl exec -ti vault-1 -- vault operator unseal SOeMqssDeuSuJahfPCj4HKldYBlUR+DfwMJ7to4xXJY=
kubectl exec -ti vault-2 -- vault operator unseal SOeMqssDeuSuJahfPCj4HKldYBlUR+DfwMJ7to4xXJY=

kubectl exec --stdin=true --tty=true vault-0 -- /bin/sh

#################################################




kubectl exec --stdin=true --tty=true vault-0 -- /bin/sh


vault auth enable kubernetes
vault write auth/kubernetes/config \
    kubernetes_host="https://$KUBERNETES_PORT_443_TCP_ADDR:443"


Role:

vault write auth/kubernetes/role/database \
    bound_service_account_names=webapp-sa \
    bound_service_account_namespaces=default \
    policies=internal-app

Policy:
vault policy write internal-app - <<EOF
path "secret/data/test-pass" {
  capabilities = ["read", "list"]
}
EOF


vault kv put secret/test-pass test-password="db-secret-password"  db_user="nebo_db_user" db_passwor="neb_$trongP@sswd" dn_endpoint="some_db_endpoint_url"
