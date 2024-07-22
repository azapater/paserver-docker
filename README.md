# paserver-docker

<a href="https://www.embarcadero.com/products/rad-studio"><img alt="Embarcadero RAD Studio" src="https://raw.githubusercontent.com/azapater/paserver-docker/main/.github/images/rad-studio-logo.png" align="right"></a>
Docker script for RAD Studio Linux deployment via PAServer

- **Container available on [Docker Hub](https://hub.docker.com/r/radstudio/paserver)**
- [PAServer Documentation](http://docwiki.embarcadero.com/RADStudio/en/PAServer,_the_Platform_Assistant_Server_Application)
- [More information on RAD Studio](https://www.embarcadero.com/products/rad-studio)
- Other containers: [InterBase](https://github.com/Embarcadero/InterBase-Docker) only, [RAD Server](https://github.com/Embarcadero/pa-radserver-docker), and [RAD Server with InterBase](https://github.com/Embarcadero/pa-radserver-ib-docker)

The image defaults to running **PAServer** on port `64211` with the _password_ `securepass`

The 10.x images use Ubuntu 18.04.6 LTS (Bionic Beaver) while the 11.x images use Ubuntu 22.04.1 LTS (Jammy Jellyfish)

## 🚀 How to Use [`run.sh`] Script

The [`run.sh`] script is your go-to automation tool for setting up and deploying the PAServer application with ease and flexibility. Below are the instructions to utilize this script effectively, now including the option to set a password for an added layer of security.

### 📋 Prerequisites

Ensure Docker is installed on your system as this script leverages Docker for running the PAServer application.

### 🌟 Features

- **Customizable Name**: Assign a unique name to your PAServer container.
- **Bind Path**: Designate a custom path for volume mapping.
- **Detach Mode**: Opt for running your container in the background.
- **Port Configuration**: Select the port for PAServer operations.
- **Production Mode**: Activate production mode for your deployment.
- **Version Control**: Choose the specific PAServer version for deployment.
- **Password Protection**: Secure your PAServer with a custom password.

### 🛠️ Usage

Navigate to the directory containing [`run.sh`] in your terminal. Execute the script with your preferred options:

```bash
./run.sh [OPTIONS]
```

#### 📌 Options

- `--name` or `-n`: Container's name (e.g., `--name=myPAServer`).
- `--path` or `-pa`: Bind path for volume mapping (e.g., `--path=/my/custom/path`).
- `--detach` or `-d`: Run container in detach mode (background).
- `--port` or `-p`: Port for PAServer (e.g., `--port=64211`).
- `--production` or `-pr`: Enable production mode (`true`).
- `--version` or `-v`: PAServer version (e.g., `--version=latest`).
- `--password` or `-pw`: Set a password for PAServer (e.g., `--password=securepass`).

### 🌈 Examples

Run PAServer in production mode on port 64211 with a custom name and password:

```bash
./run.sh --name=myPAServer --port=64211 --production --password=securepass
```

Run PAServer in detach mode with a specific version, bind path, and password:

```bash
./run.sh --detach --version=1.2.3 --path=/my/custom/path --password=securepass
```

### 🔍 Note

Make sure you have the necessary permissions to execute `run.sh`. Use `chmod +x run.sh` to make it executable if needed.

Certainly! Below is a documentation section for users who prefer to use the `docker run` command directly instead of the script:

---

## 🐳 Using `docker run` Directly

For users who prefer a more hands-on approach or wish to customize their deployment further, you can directly use the `docker run` command to start your PAServer container. This method provides flexibility and allows you to manually specify each option.

### 🛠️ Command Structure

The basic structure of the command to run the PAServer Docker container is as follows:

```bash
docker run [OPTIONS] radstudio/paserver:[VERSION]
```

### 📌 Options

- `-e PA_SERVER_PASSWORD=[PASSWORD]`: Sets the password for the PAServer. Replace `[PASSWORD]` with your desired password.
- `--name [NAME]`: Assigns a custom name to your Docker container. Replace `[NAME]` with your preferred container name.
- `-p [PORT]:64211`: Maps a custom port on your host to the PAServer's default port (64211). Replace `[PORT]` with the port number you wish to use.
- `[DETACH_ARG]`: Use `-d` to run the container in detached mode (in the background).
- `[BIND_PATH_ARG]`: Use `-v [HOST_PATH]:[CONTAINER_PATH]` to bind a volume for persistent data or configurations. Replace `[HOST_PATH]` and `[CONTAINER_PATH]` with your specific paths.

### 🌈 Example

To run the PAServer in a Docker container named `myPAServer`, listening on port 64211, with a password of `securepass`, and running in detached mode, you would use the following command:

```bash
docker run -d \
           -e PA_SERVER_PASSWORD=securepass \
           --name myPAServer \
           -p 64211:64211 radstudio/paserver:latest
```

If you wish to bind a volume for persistent data, you can add the `-v` option:

```bash
docker run -d \
           -e PA_SERVER_PASSWORD=securepass \
           -v /path/on/host:/path/in/container \
           --name myPAServer \
           -p 64211:64211 radstudio/paserver:latest
```

### 🔍 Note

Ensure you replace `/path/on/host` and `/path/in/container` with the actual paths you wish to use for volume binding. The `latest` tag can be replaced with any specific version of the PAServer you wish to deploy.

Certainly! Below is a documentation section designed to guide users on customizing their Docker image based on the provided Dockerfile. This includes adding files or folders, including extra packages, and following best practices for Docker image customization.

Certainly! Below is a documentation section designed to guide users on customizing their Docker image based on the provided Dockerfile. This includes adding files or folders, including extra packages, and following best practices for Docker image customization.

---

## 🛠️ Customizing Your Docker Image

This guide will help you customize your Docker image to suit your specific needs, such as adding additional files or folders, installing extra packages, and making other modifications.

### 📁 Adding Files or Folders

To add files or folders to your Docker image, use the `COPY` or `ADD` instruction in your Dockerfile. `COPY` is preferred for copying local files, while `ADD` can handle remote URLs and tar extraction.

#### Example: Adding a Configuration File

```dockerfile
COPY ./myconfig.conf /etc/myapp/myconfig.conf
```

This command copies `myconfig.conf` from your project directory to `/etc/myapp/myconfig.conf` inside the Docker image.

### 📦 Installing Extra Packages

To install additional packages, you can modify the `RUN` command that installs packages. It's best to combine package installation commands into a single `RUN` instruction to reduce the number of layers in your Docker image.

#### Example: Installing Git and Vim

```dockerfile
RUN apt-get update && apt-get install -y \
    git \
    vim \
    && rm -rf /var/lib/apt/lists/*
```

Based on each project, specific libraries may be necessary. This command updates the package lists, installs Git and Vim, and cleans up afterward to keep the image size down.

To avoid extra layering in the final docker image, it's good practice to modify the already existing `RUN apt-get update` command, and include there your required libraries.

### 🛠️ Customizing Installation Commands

You can customize the Dockerfile to change environment variables, download different versions of software, or modify the installation process.

#### Example: Setting a Custom Environment Variable

```dockerfile
ENV MY_CUSTOM_VAR=myvalue
```

This sets an environment variable `MY_CUSTOM_VAR` that can be used by your application.

### 🏗️ Building Your Custom Image

After customizing your Dockerfile, you can build your Docker image using the `docker build` command.

```bash
docker build -t my-custom-image:latest .
```

This command builds a Docker image named `my-custom-image` with the `latest` tag, using the Dockerfile in the current directory.

### 🔑 Using Build Arguments

For values that might change between builds (like passwords or version numbers), you can use ARG instructions in your Dockerfile and pass values with the `--build-arg` option during the build.

#### Example: Specifying a Custom Password

```dockerfile
ARG password=securepass
```

Build with a custom password:

```bash
docker build --build-arg password=mypassword -t my-custom-image:latest .
```

### 📝 Best Practices

- **Minimize Layers**: Combine related commands into single `RUN` instructions where possible.
- **Clean Up**: Remove unnecessary files and packages to keep the image size down.
- **Use `.dockerignore`**: Add a `.dockerignore` file to your project to avoid copying unnecessary files into your Docker image.
- **Secure Secrets**: Avoid hardcoding sensitive information in your Dockerfile. Use build arguments for build-time secrets and environment variables or Docker secrets for runtime secrets.

---

## License and Copyright

This software is Copyright &copy; 2024 by [Embarcadero Technologies, Inc.](https://www.embarcadero.com/)

_You may only use this software if you are an authorized licensee of an Embarcadero developer tools product. See the latest [software license agreement](https://www.embarcadero.com/products/rad-studio/rad-studio-eula) for any updates._

![Embarcadero(Black-100px)](https://raw.githubusercontent.com/azapater/paserver-docker/main/.github/images/embt-logo-black.png#gh-light-mode-only)
![Embarcadero(White-100px)](https://raw.githubusercontent.com/azapater/paserver-docker/main/.github/images/embt-logo-white.png#gh-dark-mode-only)
