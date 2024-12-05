# Network UPS Tools server

Docker image for Network UPS Tools server v2.7.4.

## Usage

This image provides a complete UPS monitoring service (USB driver only).

Start directly the container:

```console
# Example with default values 
docker run \
	--name nut-upsd \
	--detach \
	--publish 3493:3493 \
	--device /dev/bus/usb/001/001 \
	--env UPS_NAME="ups" \
	--env UPS_DESC="eaton570i" \
	--env UPS_DRIVER="usbhid-ups" \
	--env UPS_PORT="auto" \
	--env API_USER="upsmon" \
	--env API_PASSWORD="secret" \
	--env ADMIN_USER="admin" \
	--env ADMIN_PASSWORD="adminSecret" \
	--env SHUTDOWN_CMD="my-shutdown-command-from-container" \
	viktorfreire/nut-upsd
```

Start the container using Docker Compose
```console
#Example with default values
version: "3.4"
services:
  nut-upsd:
    container_name: nut-upsd
    pull_policy: always
    image: viktorfreire/nut-upsd
    network_mode: "host"
    build:
      context: .
      args:
        - API_PASSWORD="secret"
        - ADMIN_PASSWORD="adminSecret"
    environment:
      - UPS_NAME="ups"
      - UPS_DESC="eaton570i"
      - UPS_DRIVER="usbhid-ups"
      - UPS_PORT="auto"
      - API_USER="upsmon"
      - ADMIN_USER="admin"
      - SHUTDOWN_CMD="my-shutdown-command-from-container"
    devices:
      - /dev/bus/usb/001/001
    restart: unless-stopped
```

## Auto configuration via environment variables

This image supports total customization via environment variables.

### UPS_NAME

*Default value*: `ups`

The name of the UPS.

### UPS_DESC

*Default value*: `eaton570i`

This allows you to set a brief description that upsd will provide to clients that ask for a list of connected equipment.

### UPS_DRIVER

*Default value*: `usbhid-ups`

This specifies which program will be monitoring this UPS.

### UPS_PORT

*Default vaue*: `auto`

This is the serial port where the UPS is connected.

### API_USER

*Default vaue*: `upsmon`

This is the username used for communication between upsmon and upsd processes.

### API_PASSWORD

*Default vaue*: `secret`

This is the password for the upsmon user.

### ADMIN_USER

*Default vaue*: `admin`

This is the username used for administration.

### ADMIN_PASSWORD

*Default vaue*: `adminSecret`

This is the password for the admin user.

### SHUTDOWN_CMD

*Default vaue*: `echo 'System shutdown not configured!'`

This is the command upsmon will run when the system needs to be brought down. The command will be run from inside the container.
Can also be provided the location of the script that it's intended to be executed when the UPS is running low battery lvl.
