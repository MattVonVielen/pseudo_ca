export OPENSSL_CONF=openssl.cnf

.PHONY: default
default: serverauth_issuing_ca.crt.pem ca_data

.PHONY: clean clean_ca_data
clean: clean_ca_data
	@rm -rf serverauth_issuing_ca.*.pem
clean_ca_data:
	@rm -rf ca_data/

.PHONY: lint
lint: serverauth_issuing_ca.crt.pem
	@../zlint -longSummary $<

serverauth_issuing_ca.crt.pem: serverauth_issuing_ca.csr.pem
	$(MAKE) -C ../root_ca REQUEST=$(PWD)/$< CERTIFICATE=$(PWD)/$@ sign

serverauth_issuing_ca.csr.pem: serverauth_issuing_ca.key.pem
	@openssl req -new -text -key $< -out $@

serverauth_issuing_ca.key.pem:
	@openssl genrsa -out $@ 2048
	@chmod 600 $@

ca_data/:
	@mkdir -p $@/issued_certs
	@touch $@/issued_certs/.gitkeep
	@touch $@/index.txt
	@echo "01" > $@/serial

.PHONY: sign
sign: serverauth_issuing_ca.crt.pem ca_data/
	@openssl ca -batch -in $(REQUEST) -out $(CERTIFICATE)
