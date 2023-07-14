# Using Step and Certbot to Create a Private CA and Sign Certificates with the ACME Protocol (DNS Challenge)

This guide provides step-by-step instructions on how to use Step and Certbot to create a private certificate authority (CA) and sign certificates using the ACME protocol with the DNS challenge. The process involves setting up a server to act as the CA and a client to request and obtain signed certificates. Please follow the instructions below:

## Server Setup (CA)

1. Connect to the server where you want to set up the CA. In this example, we'll assume the server's IP address is 192.168.123.201.

2. Download and install the Step CLI by running the following commands:

    ```bash
    wget https://dl.smallstep.com/gh-release/cli/docs-cli-install/v0.23.4/step-cli_0.23.4_amd64.deb
    dpkg -i step-cli_0.23.4_amd64.deb
    ```

3. Download and install the Step CA by executing the following commands:

    ```bash
    wget https://dl.smallstep.com/gh-release/certificates/docs-ca-install/v0.23.2/step-ca_0.23.2_amd64.deb
    dpkg -i step-ca_0.23.2_amd64.deb
    ```

4. Initialize the Step CA by running the command:

    ```bash
    step ca init
    ```

5. Add the ACME provisioner to the CA by executing:

    ```bash
    step ca provisioner add acme --type ACME
    ```

6. Start the Step CA server by running:

    ```bash
    step-ca $(step path)/config/ca.json
    ```    

## Client Setup

1. Connect to the client machine where you want to obtain the signed certificate. In this example, we'll assume the client's IP address is 192.168.123.211.

2. Install Certbot on the client machine by running the command:

    ```bash
    apt install certbot
    ```

3. Store the root certificate from the CA server somewhere on the client machine. For this example, let's assume you save it as /home/root-ca.crt.

4. Request and obtain a certificate using Certbot and the ACME protocol with the DNS challenge. Execute the following command:

    ```bash
    REQUESTS_CA_BUNDLE=/home/root-ca.crt certbot certonly --manual --preferred-challenges dns -d example.com --server https://192.168.123.201/acme/acme/directory --email your_email@example.com
    ```


    - Replace /home/root-ca.crt with the actual path where you stored the root certificate.
    - Replace example.com with your domain for which you want to obtain the certificate.
    - Replace https://192.168.123.201/acme/acme/directory with the appropriate URL pointing to your CA server's ACME directory endpoint.
    - Replace your_email@example.com with your email address associated with the certificate.

5. Follow the on-screen prompts provided by Certbot to complete the DNS challenge verification and obtain the signed certificate.
