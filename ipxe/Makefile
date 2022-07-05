IPXE_SRC=ipxe/src
IPXE_TGT=bin-x86_64-efi/ipxe.efi
IPXE_EFI=$(IPXE_SRC)/$(IPXE_TGT)

comma:=,
empty:=
space:= $(empty) $(empty)

EMBED=$(PWD)/chain-to-sideload.ipxe
ALLCERTS=ipxe-ca.pem isrgrootx1.pem lets-encrypt-r3.pem
CERT=$(subst $(space),$(comma),$(addprefix $(PWD)/,$(ALLCERTS)))
TRUST=$(CERT)

all: ipxe

submodules:
	git submodule update --init --recursive

update:
	git submodule foreach git pull origin master
	curl -L -o ipxe-ca.pem http://ca.ipxe.org/ca.crt
	curl -L -o isrgrootx1.pem https://letsencrypt.org/certs/isrgrootx1.pem
	curl -L -o lets-encrypt-r3.pem https://letsencrypt.org/certs/lets-encrypt-r3.pem

clean:
	$(MAKE) -C $(IPXE_SRC) clean

patch: submodules
	cp -av $(wildcard config/*) $(IPXE_SRC)/config/local

$(IPXE_EFI): $(ALLCERTS) submodules patch
	$(MAKE) -C $(IPXE_SRC) EMBED=$(EMBED) CERT=$(CERT) TRUST=$(TRUST) $(IPXE_TGT)

ipxe: $(IPXE_EFI)

.PHONY: submodules ipxe $(IPXE_EFI)
