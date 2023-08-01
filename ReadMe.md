# Authentik for Traefik

This repository provides a guide and configuration files for setting up Authentik as an authentication solution for
web pages protected by Traefik.
Authentik is a powerful and flexible authentication proxy that can help secure access to your web applications.

## Prerequisites

Before you get started, make sure you have the following prerequisites installed and configured:

1. **Docker**: Authentik can be run in a Docker container, so ensure you have Docker and Docker-Compose installed on your server.

2. **Traefik**: You should already have Traefik set up as a reverse proxy to manage your web traffic.
    Here is an example: [erkenes/docker-traefik](https://github.com/erkenes/docker-traefik)

## Usage

Follow these steps to set up Authentik for authentication behind Traefik:

1. Clone this repository to your server:
   ```shell
   git clone https://github.com/erkenes/docker-authentik.git
   cd docker-authentik
   ```

2. Create a .env file to store environment variables. You can use the .env.example file as a template:
    ```shell
    cp .env.example .env
    ```
   Edit the .env file and provide values for the environment variables as needed.

3. Change the value of the secrets to store your SMTP-Password, PostgreSql-Password and the Authentik-Secret Key.

    Change the PostgreSql password and Authentik Secret Key:
    ```shell
    openssl rand -base64 14 > secrets/authentik_secret_key
    openssl rand -base64 14 > secrets/postgresql_password
    ```
   Save your SMTP password and username into the following files:
    - `secrets/smtp_user`: SMTP username
    - `secrets/smtp_password`: SMTP password

4. Start Authentik and Traefik by running:
    ```shell
    docker compose up -d
    ```

5. Access the Authentik admin panel by navigating to http://your-server-ip:9000/if/flow/initial-setup/ in your web browser. Use the admin credentials you specified in the .env file to log in.

6. Configure authentication providers and policies in the Authentik admin panel as required for your use case.

7. Update your Traefik configuration to [include the Authentik authentication middleware](examples/traefik-middlewar.yml) for the desired routes. Refer to Traefik's documentation on middleware configuration for guidance.

8. Test your setup by accessing your protected web pages. You should be prompted to authenticate through Authentik.

## Configuration

- `docker-compose.yml`: Defines the services for Authentik and Traefik. You can modify this file to adjust container names, network settings, or ports.

- `.env.example`: Example environment variable configuration. Copy this file to .env and customize it with your specific configuration.

- `secrets/authentik_secret_key`: The Security Key for authentik. Do not change the value after the first initialization! [Read more](https://goauthentik.io/docs/installation/configuration#authentik_secret_key)

- `secrets/postgresql_password`: The password for the PostgreSQL user

- `secrets/smtp_user`: The username for the smtp server 

- `secrets/smtp_password`: The password for smtp user

## Troubleshooting

If you encounter issues or need further assistance, please check the logs of the Authentik and Traefik containers for error messages.
Additionally, refer to the documentation for Authentik and Traefik for detailed configuration options and troubleshooting tips.

## License

This project is licensed under the [MIT License](LICENSE).

## Acknowledgments

- [Authentik](https://github.com/goauthentik/authentik): The powerful authentication provider used in this setup.
- [Traefik](https://traefik.io/): The reverse proxy and load balancer used to manage web traffic.

## Contributing

Contributions are welcome! If you have any improvements, bug fixes, or feature requests, please open an issue or submit a pull request.
