ENV['OPENSSL_CONF'] = 'pseudo_ca.cnf'

def distinguished_name(common_name)
  "\"/C=US/O=Commonwealth Institute of Technology/OU=Bioscience/CN=#{common_name}\""
end

desc 'The root CA certificate'
file 'ca/ca.crt' do
  mkdir_p 'ca/issued_certs'
  touch 'ca/issued_certs/.gitkeep'
  touch 'ca/index.txt'
  File.write('ca/serial', "00\n")
  sh 'openssl genrsa -out ca/ca.key 2048'
  sh "openssl req -new -key ca/ca.key -out ca/ca.crt -subj #{distinguished_name('Root CA')} -x509 -sha256 -days #{20 * 365}"
end

desc 'Generate a key and issue a certificate for server at FQDN'
task :generate_server, [:fqdn] => 'ca/ca.crt' do |t, args|
  fqdn = args[:fqdn]

  mkdir_p fqdn
  keyfile = File.join fqdn, 'server.key'
  csrfile = File.join fqdn, 'server.csr'
  crtfile = File.join fqdn, 'server.crt'

  sh "openssl genrsa -out #{keyfile} 2048"
  sh "openssl req -new -key #{keyfile} -out #{csrfile} -subj #{distinguished_name(fqdn)}"

  ENV['SERVER_FQDN'] = fqdn
  sh "openssl ca -batch -in #{csrfile} -out #{crtfile} -extensions server_extensions"
  ENV['SERVER_FQDN'] = nil

  rm_f csrfile
end
