fcpcag2.crt caCertsIssuedTofcpcag2.p7c caCertsIssuedByfcpcag2.p7c:
	curl -O http://repo.fpki.gov/fcpca/$@

%.p7c.pem : %.p7c
	@openssl pkcs7 -inform der -in $< -text -print_certs -out $@

%.crt.pem : %.crt
	@openssl x509 -inform der -in $< -outform pem -out $@
	@openssl x509 -inform der -in $< -text -noout >> $@