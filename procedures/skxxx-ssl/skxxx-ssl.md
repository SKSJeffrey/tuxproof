# SKXXX - Create SSL Certificates
## Purpose - Generate secured certificates for internal and external domains
## Procedure

## Internal Website

### Creating a self-signed SSL certificate
A self-signed SSL certificate is a certificate that is signed with its own private key
```bash
openssl req -x509 -sha256 -nodes -days 3650 -newkey rsa:4096 -keyout /etc/httpd/ssl/<fqdn>.key -out /etc/httpd/ssl/<fqdn>.crt
```
In a VHosts configuration block
```bash
SSLCertificate /etc/httpd/ssl/<fqdn>.crt

SSLCertificateKeyFile /etc/httpd/ssl/<fqdn>.key
```

## External Website

### Generate a CSR (Certificate Signing Request)
A CSR is generated and submitted to a CA (Certificate Authority)
```bash
openssl req -new -newkey rsa:2048 -nodes -keyout /etc/httpd/ssl/<fqdn>.key -out /etc/httpd/ssl/<fqdn>.csr
```
Once the CSR is submitted to the CA, they will return two files
```bash
<fqdn>.crt
<CA>.crt
```
In a VHosts configuration block
```bash
SSLCertificate /etc/httpd/ssl/<fqdn>.crt

SSLCertificateKeyFile /etc/httpd/ssl/<fqdn>.key

SSLCertificateChainFile /etc/httpd/ssl/<CA>.crt
```

## Additional Information
Refer to [shaaaaaaaaaaaaa](https://shaaaaaaaaaaaaa.com/) for more information
