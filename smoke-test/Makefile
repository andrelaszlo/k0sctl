
footloose := $(shell which footloose)
ifeq ($(footloose),)
footloose := $(shell go env GOPATH)/bin/footloose
endif

envsubst := $(shell which envsubst)
ifeq ($(envsubst),)
$(error 'envsubst' NOT found in path, please install it and re-run)
endif

.PHONY: k0sctl
k0sctl:
	$(MAKE) -C .. k0sctl

$(footloose):
	go install github.com/weaveworks/footloose/...@0.6.3

id_rsa_k0s:
	ssh-keygen -t rsa -f ./id_rsa_k0s -N ""

smoke-basic: $(footloose) id_rsa_k0s k0sctl
	./smoke-basic.sh

smoke-files: $(footloose) id_rsa_k0s k0sctl
	./smoke-files.sh

smoke-init: $(footloose) id_rsa_k0s k0sctl
	./smoke-init.sh

smoke-upgrade: $(footloose) id_rsa_k0s k0sctl
	./smoke-upgrade.sh

smoke-reset: $(footloose) id_rsa_k0s k0sctl
	./smoke-reset.sh

smoke-os-override: $(footloose) id_rsa_k0s k0sctl
	FOOTLOOSE_TEMPLATE=footloose.yaml.osoverride.tpl K0SCTL_CONFIG=k0sctl-single.yaml OS_RELEASE_PATH=$(realpath os-release) OS_OVERRIDE="ubuntu" ./smoke-basic.sh

smoke-backup-restore: $(footloose) id_rsa_k0s k0sctl
	./smoke-backup-restore.sh