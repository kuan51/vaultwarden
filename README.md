# Install Vaultwarden

## Guide: Deploying Vaultwarden with Cloudflare Tunnel

In this guide, we will walk you through the process of deploying Vaultwarden, a self-hosted password manager, using a Cloudflare Tunnel. Cloudflare Tunnels allow you to securely expose your local web server to the internet without the need for port forwarding or exposing your server's IP address (Zero Trust).

### Prerequisites

Before you begin, make sure you have the following:

1. A Cloudflare account: Sign up for a free account at [cloudflare.com](https://www.cloudflare.com).

2. A [cloudflare tunnel token](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/get-started/create-remote-tunnel/).

3. A cloudflare tunnel id. Once you have the token, you can extract the id from it. The token is a base64 encoded json and the id is one of the json values. You can decode the token with this command:

    ```bash
    echo 'eyJhIjoiY2ZfYWNjb3VudF9pZCIsInQiOiJjbG91ZGZsYXJlX3R1bl9pZCIsInMiOiJjZl90dW5fc2VjcmV0In0=' | base64 -d
    ```

4. A bash shell with argon2 installed.

    ```bash
    sudo apt update && sudo apt install argon2 -y
    ```

### Installation

1. To enable the admin page, you need to set an authentication token. After this, the page will be available at `/admin`. Create the password hash and update the ADMIN_TOKEN value in `vaultwarden.env`.

    ```bash
    echo -n "MySecretPassword" | argon2 "$(openssl rand -base64 32)" -e -id -k 65540 -t 3 -p 4 | sed 's/\$/\$\$/g'
    ```

2. Update the `vaultwarden.env` file with your desired configurations.

3. Source the configuration values

    ```bash
    . vaultwarden.env
    ```

4. Start the project using docker compose. Run these commands from the project folder.

    ```bash
    docker compose up -d
    ```

    To stop:

    ```bash
    docker compose down
    ```
