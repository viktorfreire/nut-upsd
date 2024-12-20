# Network UPS Tools server

Docker image for Network UPS Tools server v2.7.4.

## Usage

This image provides a complete UPS monitoring service (USB driver only).

Start directly the container:

```console
# Example 
docker run \
	--name nut-upsd \
  --network host \ 
  -v nut_data:/nut \
	--env UPS_NAME="ups" \
	--env UPS_DESC="eaton570i" \
	--env UPS_DRIVER="usbhid-ups" \
	--env UPS_PORT="auto" \
	--env API_USER="upsmon" \
	--env API_PASSWORD="secret" \
	--env ADMIN_USER="admin" \
	--env ADMIN_PASSWORD="adminSecret" \
	--env SHUTDOWN_CMD="my-shutdown-command-from-container" \
	--device /dev/bus/usb/001/001 \
  --restart unless-stopped \
  --detach \
	viktorfreire/nut-upsd
```

Start the container using Docker Compose
```console
#Example
version: "3.4"
services:
  nut-upsd:
    container_name: nut-upsd
    pull_policy: always
    image: viktorfreire/nut-upsd
    network_mode: "host"
    volumes:
      - nut_data:/nut
    environment:
      - UPS_NAME="ups"
      - UPS_DESC="eaton570i"
      - UPS_DRIVER="usbhid-ups"
      - UPS_PORT="auto"
      - API_USER="upsmon"
      - API_PASSWORD="secret"
      - ADMIN_USER="admin"
      - ADMIN_PASSWORD="adminSecret"
      - SHUTDOWN_CMD="my-shutdown-command-from-container"
    devices:
      - /dev/bus/usb/001/001
    restart: unless-stopped
```

## Dedicate persistant volume

Withing the latest updates was introduced the possibility to use dedicated volume instead of using the OS directories, with this I make it possible to have persistante data.

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

*Default vaue*: `runs an script that shutdown the server where NUT is running`

This is the command upsmon will run when the system needs to be brought down. The command will be run from inside the container.
It's possible to provide the location of the script that it's intended to be executed when the UPS is running low battery lvl, for that the script need to be stored in the /nut/scripts/ folder.
