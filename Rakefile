desc "Generate a new key"
task :gen_key do
 domain = get_env(:domain)
 filename = "#{domain}.key"

 `openssl genrsa -out #{filename} 2048`
end

desc "Generate a new CSR"
task :gen_csr => :gen_key do
 domain = get_env(:domain)
 csr_filename = "#{domain}.csr"
 key_filename = "#{domain}.key"

 `openssl req -new -utf8 -sha256 -key #{key_filename} -out #{csr_filename}`
 `cat #{csr_filename} | pbcopy`
end

desc "Generate a proper nginx cert file from Namecheap Comodo certificate download"
task :assemble_cert do
 cert_dir = get_env(:cert_dir)
 domain = get_env(:domain)
 pem_path = "#{domain}.crt"

 File.open(pem_path, 'w+') do |pem_file|
   add_file_to_cert pem_file, domain.gsub(/\./, '_') + '.crt'
   add_file_to_cert pem_file, 'COMODORSADomainValidationSecureServerCA.crt'
   add_file_to_cert pem_file, 'COMODORSAAddTrustCA.crt'
   add_file_to_cert pem_file, 'AddTrustExternalCARoot.crt'
 end

 puts "Wrote pem to #{pem_path}"
end

def add_file_to_cert(pem_file, filename)
 cert_dir = get_env(:cert_dir)
 full_path = File.join(cert_dir, filename)
 puts "Adding #{full_path} to pem"
 pem_file.write(File.read(full_path))
end

def get_env(name)
 val = ENV[name.to_s]
 raise "Required env variable missing: #{name}" unless val && val != ''
 val
end
