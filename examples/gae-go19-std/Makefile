_localyaml: _vault
	@echo "$$LOCALYAML" > local.yaml;

run: _localyaml
	@goapp serve local.yaml

test: _vault
	@export VAULT_ADDR=$$VAULT_ADDR; \
	export VAULT_LOCAL_TOKEN="`cat ~/.vault-token`"; \
	export VAULT_SECRET_PATH="repo-name/secret/my-secrets"; \
	goapp test ./... -v -cover 

_vault:
	@export VAULT_ADDR=$$VAULT_ADDR; \
	vault login -method=github token=`cat ~/.config/vault/github` > /dev/null 2>&1;

VAULT_ADDR=https://vault.example.com

define LOCALYAML 
runtime: go
api_version: go1.9
service: default

inbound_services:
- warmup

handlers:
- url: /.*
  script: _go_app
env_variables:
  VAULT_ADDR: $(VAULT_ADDR)
  VAULT_LOCAL_TOKEN: "$(shell cat ~/.vault-token)"
  VAULT_SECRET_PATH: "repo-name/secret/my-secrets"
endef

.EXPORT_ALL_VARIABLES:
