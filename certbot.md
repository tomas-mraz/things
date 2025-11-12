
# Certbot

# Expand an existing certificate

Adding domain `blog.domain.com` to the existing certificate for `domain.com`  
switch `-d` must define ALL domains in the certificate ... not only the new one

Show all certificates  
`certbot certificates`

Manual TXT record in DNS  
`certbot certonly --manual --preferred-challenges dns --expand -d domain.com -d blog.domain.com`

Certbot listen on port and LetsEncrypt server connect to it for verification  
`certbot certonly --standalone --expand -d domain.com -d blog.domain.com`
