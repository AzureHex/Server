## Installed Apps

```sh
sudo apt install glances powertop tailscale tailscale-archive-keyring tree
```

1. Set up Docker's `apt` repository.

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

2. Install the Docker packages.

To install the latest version, run:

```sh
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```


## Linux post-installation steps for Docker Engine

To create the `docker` group and add your user:

1. Create the `docker` group.
    
    ```sh
    sudo groupadd docker
    ```
    
2. Add your user to the `docker` group.
    
    ```sh
    sudo usermod -aG docker $USER
    ```
    
3. Log out and log back in so that your group membership is re-evaluated.
    
    > If you're running Linux in a virtual machine, it may be necessary to restart the virtual machine for changes to take effect.
    
    You can also run the following command to activate the changes to groups:
    
    ```console
    newgrp docker
    ```
    
4. Verify that you can run `docker` commands without `sudo`.
    
    ```console
    docker run hello-world
    ```
    
    This command downloads a test image and runs it in a container. When the container runs, it prints a message and exits.
    
    If you initially ran Docker CLI commands using `sudo` before adding your user to the `docker` group, you may see the following error:
    
    ```none
    WARNING: Error loading config file: /home/user/.docker/config.json -
    stat /home/user/.docker/config.json: permission denied
    ```
    
    This error indicates that the permission settings for the `~/.docker/` directory are incorrect, due to having used the `sudo`command earlier.
    
    To fix this problem, either remove the `~/.docker/` directory (it's recreated automatically, but any custom settings are lost), or change its ownership and permissions using the following commands:
    
    ```sh
    sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
    sudo chmod g+rwx "$HOME/.docker" -R
    ```
    

## [Configure Docker to start on boot with systemd](https://docs.docker.com/engine/install/linux-postinstall/#configure-docker-to-start-on-boot-with-systemd)

Many modern Linux distributions use [systemd](https://systemd.io/) to manage which services start when the system boots. On Debian and Ubuntu, the Docker service starts on boot by default. To automatically start Docker and containerd on boot for other Linux distributions using systemd, run the following commands:

```sh
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

To stop this behavior, use `disable` instead.

```sh
sudo systemctl disable docker.service
sudo systemctl disable containerd.service
```

---

## [Uninstall Docker Engine](https://docs.docker.com/engine/install/debian/#uninstall-docker-engine)

1. Uninstall the Docker Engine, CLI, containerd, and Docker Compose packages:
    
    ```sh
    sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
    ```
    
2. Images, containers, volumes, or custom configuration files on your host aren't automatically removed. To delete all images, containers, and volumes:
    
    ```sh
    sudo rm -rf /var/lib/docker
    sudo rm -rf /var/lib/containerd
    ```
    
3. Remove source list and keyrings
    
    ```sh
    sudo rm /etc/apt/sources.list.d/docker.list
    sudo rm /etc/apt/keyrings/docker.asc
    ```
    

Delete any edited configuration files manually.