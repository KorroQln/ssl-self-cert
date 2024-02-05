# ssl-self-cert

## Generate a self-signed ssl certificate using openssl
interactive command: 
```openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -sha256 -days 365```
- Add ```-nodes``` (short for "no DES") if you don't want to protect private key with a passphrase. Otherwise it will prompt you for "at least a 4 character" password.
- Self-signed certificates are not validated with any third party unless import them to the browsers. If need more security, should use a certificate signed by a certificate authority (CA): (Let's Encrypt)

## More
- https://devopscube.com/create-self-signed-certificates-openssl/

## Tutorial
To generate a self-signed certificate for your proxy server, you can follow a similar process as outlined earlier for a general self-signed certificate. Below are the steps specifically for generating a self-signed certificate for a proxy server using OpenSSL:

Step 1: Install OpenSSL (if not already installed):
Ensure that OpenSSL is installed on your system. If not, you can install it using your system's package manager. For example, on Ubuntu, you can use:
```
sudo apt update
sudo apt install openssl
```

Step 2: Generate a Private Key:
Generate a private key for your proxy server:

```
openssl genpkey -algorithm RSA -out proxy-private-key.pem
```

Step 3: Generate a Certificate Signing Request (CSR):
Generate a Certificate Signing Request (CSR) using the private key:
```
openssl req -new -key proxy-private-key.pem -out proxy-csr.pem
```
This command prompts you to enter information for the certificate. Enter the appropriate information, including the Common Name (CN), which should be the hostname or IP address of your proxy server.

Step 4: Generate a Self-Signed Certificate:
Generate a self-signed certificate using the private key and CSR:
```
openssl x509 -req -days 365 -in proxy-csr.pem -signkey proxy-private-key.pem -out proxy-certificate.pem
```
This command creates a self-signed certificate valid for 365 days. Adjust the duration (-days) as needed.

Step 5: Verify the Generated Certificate:
You can view the contents of the generated certificate:
```
openssl x509 -text -noout -in proxy-certificate.pem
```

Step 6: Cleanup (Optional):
You can remove the Certificate Signing Request (CSR) file as it's no longer needed:
```
rm proxy-csr.pem
```

Step 7: Use the Certificate and Private Key:
Now, you have a self-signed certificate (proxy-certificate.pem) and a private key (proxy-private-key.pem) for your proxy server. You can use these files in your Nginx or other web server configuration.

Update your Nginx configuration or the proxy server configuration to use these files, similar to the example provided in a previous response. Adjust the file paths in the configuration to point to the actual locations of your certificate and private key files.
```
server {
    listen 443 ssl;
    server_name proxy.example.com;

    ssl_certificate /path/to/proxy-certificate.pem;
    ssl_certificate_key /path/to/proxy-private-key.pem;

    # Other SSL-related settings...
}
```
Make sure to replace /path/to/proxy-certificate.pem and /path/to/proxy-private-key.pem with the actual paths to your certificate and private key files.

After making these changes, save the configuration and restart your proxy server.

```
sudo nginx -t
```
```
sudo systemctl restart nginx
```

## Setup https
- ```nginx-https.conf```
- ```curl -k "https://192.168.1.76:8080" "http://www.google.com" -v```
- https://chat.openai.com/share/1db9485f-2bd3-409e-a0c2-bfd120e5c391



