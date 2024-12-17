# comm-docker-config

This package provides a set of hooks for managing Docker installation, upgrade, and removal processes. It automates common administrative tasks such as adding users to the Docker group, configuring Docker service settings, and ensuring that Docker starts on boot.

## Features

- **Post-Install**: 
  - Creates the Docker group if it doesn't exist.
  - Adds all users with UID >= 1000 (except `nobody`) to the Docker group.
  - Configures the `containerd` service to limit the number of open files if necessary.
  - Ensures Docker starts on boot and starts the Docker service.
  - Updates the current user's session with the Docker group.
  - Changes permissions on Docker socket to allow unrestricted access.

- **Post-Upgrade**:
  - If the `containerd` service has an open file limit set to infinity, it adjusts the value to `1048576` and restarts the service if it is active.
  - Reloads the systemd daemon after making changes to the `containerd` service configuration.

- **Post-Remove**:
  - Checks if Docker is installed; if so, prevents the Docker service from being disabled.
  - If Docker is not installed, disables the Docker service and deletes the Docker group.

## Usage

### Installation
Once installed, the following actions will be performed automatically depending on the state of Docker:

- **Post-Install Hook**: Executes after Docker is installed.
- **Post-Upgrade Hook**: Executes after upgrading Docker.
- **Post-Remove Hook**: Executes when Docker is removed.

These hooks ensure that Docker is correctly configured and maintained throughout its lifecycle on the system.

### Requirements

- Linux-based system with `systemd` for service management.
- Docker installed via the system's package manager.
- `awk`, `systemctl`, and `groupadd` utilities are required.

