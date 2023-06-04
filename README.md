# Final Project: Vault-Hashicorp, NGINX, Flask, Gunicorn in Docker

## Architecture

The completion of this architecture, which includes NGINX, Gunicorn, Flask, Vault inside Docker, and setting up Vault behind NGINX with TLS, presented a considerable learning curve. Due to the complexity and uniqueness of this project, finding relevant resources or personal assistance was challenging. Nevertheless, the architecture was successfully established, providing a robust environment for secure application deployment.

## Insights and Challenges

Understanding how to use Vault effectively within a Dockerized environment was a key challenge, primarily due to the scarcity of comprehensive documentation on setting up a Dockerized Vault behind NGINX. The system experienced multiple crashes during the project, leading to a deeper appreciation for versioning, and the necessity of creating new images.

In response to these challenges, it became necessary to gain a solid understanding of Docker. This was achieved through numerous hours of video tutorials and smaller projects, which elucidated the functionalities and use cases of Docker. Following this, the integration of Vault into Docker was tackled, requiring a better understanding of Vault's configuration within Docker, as well as an increased familiarity with NGINX configuration. This knowledge allowed the successful deployment of Vault behind NGINX while maintaining TLS.

## Configuring Vault

### Docker

Vault was installed into Docker using the command `docker pull vault`.

To create an image for NGINX and keep it running, the following command was used:

```bash
docker run -d -t --cap-add=IPC_LOCK -e 'VAULT_LOCAL_CONFIG={"storage": {"file": {"path": "/vault/file"}}, "listener": [{"tcp": { "address": "0.0.0.0:8200", "tls_disable": true}}], "default_lease_ttl": "168h", "max_lease_ttl": "720h", "ui": true}' -p 8200:8200 vault server
```
### Hardening Vault with TLS

The Vault configuration file (`vault.hcl`) was set up with specific parameters to ensure secure storage, listener configuration, and token lease settings. Permissions on `vault.hcl` were also set so that the service account could interact with it.

Vault was then set to run as a service with a new unit file, `vault.service`, created for this purpose. The service was checked for successful startup with `systemctl`.

### Vault Setup

Vault was initialized with five key shares and two key thresholds, along with an initial root token.

### Vault Behind NGINX

NGINX was configured to serve as a reverse proxy to Vault via port 443, with Vault having its own TLS certificates. The reverse proxy is located in `/etc/nginx/sites-available-project`.

## Conclusion

The final architecture includes an NGINX load balancer/reverse proxy as the ingress and egress to Flask via Gunicorn. Vault, which serves as the credential manager, is also situated behind NGINX. 

This project has resulted in substantial growth in understanding of dev ops and networking, as well as gaining practical experience in IAM, Networking, SysAdmin, DevOps, security, documentation, and scripting. The project posed significant challenges, but overcoming these has built confidence and readiness to tackle real-world problems. This work has shown that networking and configuration knowledge is vital for security, opening the door to many possibilities in the field.
