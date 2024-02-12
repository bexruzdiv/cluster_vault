### 🚀 Welcome to My GitHub Repository! 🎉

This repository contains Ansible roles for setting up a robust infrastructure with Nginx load balancing, HashiCorp Vault, automated backups, and now, creating Kubernetes secrets with Vault secret collections. Before diving in, make sure to follow the steps below to customize the setup according to your requirements.

---

### Setup Instructions 🛠️

1. **Replace Variables**

    Before using the roles, ensure you replace the variables in the `defaults/main.yml` file with your desired configurations. This file contains essential variables for configuring Nginx, Vault, backup settings, and now, Kubernetes secrets.

2. **Configure Inventory**

    Be careful when using the `inventory.ini` file. Ensure that you use the following hostnames:

    - `leader` 🥇
    - `follower1` 🏃‍♂️
    - `follower2` 🏃‍♂️
    - `loadbalancer` 🔄

    Adjust the `inventory.ini` file accordingly with these hostnames for proper configuration and deployment.

3. **Role 1: nginx_add_vhost**

    This role sets up a load balancer using Nginx and obtains HTTPS certificates through Certbot.

4. **Role 2: vault**

    Utilize this role to create a cluster using HashiCorp Vault with Raft storage. Vault provides a secure and centralized way to manage secrets and sensitive data.

5. **Role 3: vault_backup**

    The `vault_backup` role allows you to take backups from Vault and schedule a cron job for regular backups. It's recommended to create a separate token specifically for this role to ensure security and proper access control.

6. **Role 4: vault_secret_operator**

    The `vault_secret_operator` role assists in creating Kubernetes secrets with Vault secret collections. This enhances the security of your Kubernetes deployments by managing secrets centrally in Vault.

---

### Usage 🚀

Follow these steps to deploy the infrastructure:

1. Clone this repository to your local machine.
2. Replace the variables in `defaults/main.yml` with your desired configurations.
3. Ensure that the `inventory.ini` file is configured with the correct hostnames.
4. Use the Ansible roles provided in this repository to set up Nginx, Vault, backup procedures, and Kubernetes secrets on your servers.

---

### Contributions 🤝

Contributions are welcome! If you have any improvements or suggestions, feel free to open an issue or pull request.

Let's build robust and secure infrastructures together! 🛠️

--- 

### About ℹ️

This project is maintained by Bexruz. For any questions or inquiries, please contact bexruzturobjonov0955@gmail.com or with telegram @blvck_sudo . 

Happy coding! 💻✨
