export OPENSSL_CONF=openssl.cnf
ROOT_CA_VALIDITY_DAYS = 7305 # About 20 years

.PHONY: default
default: root_ca.crt.pem ca_data/

.PHONY: clean
clean:
	@rm -rf root_ca.*.pem ca_data/

.PHONY: lint
lint: root_ca.crt.pem
	@../zlint -longSummary $<

root_ca.crt.pem: root_ca.key.pem
	@openssl req -new -x509 -text -days $(ROOT_CA_VALIDITY_DAYS) -key $< -out $@

root_ca.key.pem:
	@openssl genrsa -out $@ 2048
	@chmod 600 $@

ca_data/:
	@mkdir -p $@/issued_certs
	@touch $@/issued_certs/.gitkeep
	@touch $@/index.txt
	@echo "00" > $@/serial

.PHONY: sign
sign: root_ca.crt.pem ca_data/
	@openssl ca -batch -in $(REQUEST) -out $(CERTIFICATE)
