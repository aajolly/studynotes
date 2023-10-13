# Vault Installation
[Vault with integrated storage deployment guide](https://developer.hashicorp.com/vault/tutorials/day-one-raft/raft-deployment-guide#install-vault)

## Configuration of Vault Notes
### Configure TLS certificates
mkdir -p /opt/vault/tls
cp /root/*.pem /opt/vault/tls
chown root:root /opt/vault/tls/*.pem
chown root:vault /opt/vault/tls/*-key.pem
chmod 0644 /opt/vault/tls/*-cert.pem /opt/vault/tls/academy-ca.pem
chmod 0640 /opt/vault/tls/*-key.pem

### Create the vault config file
cluster_addr  = "https://vault-server-2:8201"
api_addr      = "https://vault-server-2:8200"
disable_mlock = true

listener "tcp" {
  address            = "0.0.0.0:8200"
  tls_cert_file      = "/opt/vault/tls/vault-server-2-cert.pem"
  tls_key_file       = "/opt/vault/tls/vault-server-2-key.pem"
  tls_client_ca_file = "/opt/vault/tls/academy-ca.pem"
}

storage "raft" {
  path    = "/opt/vault/data"
  node_id = "node2"

retry_join {
    leader_api_addr         = "https://vault-server-1:8200"
    leader_ca_cert_file     = "/opt/vault/tls/academy-ca.pem"
}

retry_join {
    leader_api_addr         = "https://vault-server-3:8200"
    leader_ca_cert_file     = "/opt/vault/tls/academy-ca.pem"
  }
}


### Create the storage directory
mkdir -p /opt/vault/data
chown vault:vault /opt/vault/data

### Configure the license
cp /root/vault.hclic /opt/vault/vault.hclic
chown root:vault /opt/vault/vault.hclic
chmod 0640 /opt/vault/vault.hclic
echo "license_path = \"/opt/vault/vault.hclic\"" >> /etc/vault.d/vault.hcl

### Enable GUI
echo "ui = true" >> /etc/vault.d/vault.hcl

### Start Vault service
systemctl enable vault.service
systemctl start vault.service
systemctl status vault.service

### Set env variables
export VAULT_CACERT=/opt/vault/tls/academy-ca.pem
echo "export VAULT_CACERT=/opt/vault/tls/academy-ca.pem" >> /root/.bash_profile

### Vault status
vault status