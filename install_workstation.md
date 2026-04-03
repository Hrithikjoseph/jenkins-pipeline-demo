**Omniverse Nucleus Installation**

**Overview**

The Omniverse Enterprise Nucleus Server was installed using the official
NVIDIA Docker-based deployment method on Ubuntu 22.04 LTS. This approach
is recommended for production environments due to its scalability,
modular architecture, and containerised services.

**Hardware Sizing Guide**

Nucleus is available as a fully scalable solution within the Omniverse
Enterprise Nucleus Server. The information below explains the sizing and
hardware requirements deploying your own Nucleus Enterprise Server.

**Sizing specification**

<table>
<colgroup>
<col style="width: 37%" />
<col style="width: 40%" />
<col style="width: 22%" />
</colgroup>
<thead>
<tr>
<th><strong>Users</strong></th>
<th><strong>Notes</strong></th>
<th><strong>Operating System</strong></th>
</tr>
</thead>
<tbody>
<tr>
<td><ul>
<li><p>500 Concurrent Users</p></li>
<li><p>8-30** Live Edit Users (Per Session)</p></li>
</ul></td>
<td><ul>
<li><p>Designed for Production Environments</p></li>
<li><p>Single Sign-On (SSO) Suppo</p></li>
<li><p>SSL Support</p></li>
</ul></td>
<td><ul>
<li><p>Linux (Docker)</p></li>
</ul></td>
</tr>
</tbody>
</table>

**Minimum Hardware Requirements**

| **Hardware** | **Requirement**              |
|--------------|------------------------------|
| CPU          | 16 Cores​ (3.0 GHz or Higher) |
| Memory/RAM   | 32 GB                        |
| Storage\*    | 500 GB                       |

\* Storage size is dependent on the amount of assets stored within the
Nucleus service. For best performance, SSD-type disks are recommended.

**Prerequisites**

Before installation, the following requirements were ensured:

- Ubuntu 22.04 LTS server installed

- System updated with latest patches

- Required utilities installed:

sudo apt-get update

sudo apt-get upgrade -y

sudo apt-get install apt-transport-https ca-certificates curl gnupg
lsb-release

**Docker Installation**

Docker (version 20.x or latest) needs to be installed as it is required
for running the Nucleus container stack.

Run the following commands which add the proper Docker repositories:

sudo mkdir -p /etc/apt/keyrings

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg \| sudo gpg
--dearmor -o /etc/apt/keyrings/docker.gpg

Run the following command, which adds the Docker repository to your apt
sources file:

sudo echo \\

"deb \[arch=\$(dpkg --print-architecture)
signed-by=/etc/apt/keyrings/docker.gpg\]
[https://download.docker.com/linux/ubuntu
\\](https://download.docker.com/linux/ubuntu%20\)

\$(lsb_release -cs) stable" \| sudo tee
/etc/apt/sources.list.d/docker.list \> /dev/null

Next, run the following command to update all your local apt
repositories:

sudo apt-get update

Run the following command to display a list of available Docker versions
within the repository. As noted above, the recommended version of Docker
is version 20. The latest version of Docker 20 as of this writing is
20.10.24.

sudo apt-cache madison docker-ce \| awk '{ print \$3 }'

To install the recommended version of Docker, run the following
commands:

VERSION_STRING=5:26.1.4-1~ubuntu.24.04~noble

sudo apt-get install docker-ce=\$VERSION_STRING
docker-ce-cli=\$VERSION_STRING containerd.io docker-compose-plugin

To confirm Docker and the correct version is installed, run the
following command:

docker --version

**Omniverse Enterprise Nucleus Server Installation**

Download the latest nucleus-stack (.tar.gz) package to a local temporary
directory (e.g., /tmp) on your server. The file can be downloaded from
NVIDIA NGC (NVIDIA GPU Cloud) after logging in with a licensed account
(Organisation/Company account).

**Extracting Installation Files**

Enter the temporary directory:

cd /tmp

Create an install directory (Recommended location: /opt/ove):

sudo mkdir /opt/ove

Extract the nucleus-stack package to your install directory:

sudo tar xzvf
nucleus-stack-2023.2.8+tag-2023.2.8.gitlab.24971896.fea9b67c.tar.gz -C
/opt/ove --strip-components=1

The following structure will now be within the /opt/ove directory:

drwxr-xr-x 7 root root 112 Nov 1 18:32 .

drwxr-xr-x 7 root root 98 Nov 1 18:31 ..

-rw-rw-rw- 1 root root 2450 Oct 31 05:06 README.md

-rw-r--r-- 1 root root 52 Oct 31 05:06 VERSION

drwxr-xr-x 6 root root 328 Nov 2 22:16 base_stack

drwxr-xr-x 3 root root 67 Nov 1 18:32 navigator

drwxr-xr-x 2 root root 39 Nov 1 18:32 ssl

drwxr-xr-x 2 root root 68 Nov 1 18:32 sso

drwxr-xr-x 2 root root 101 Nov 1 18:32 templates

For this installation guide, we will focus on
configuring nucleus-stack.env within the base_stack directory.

**Configuration of Nucleus**

Enter the base_stack directory:

cd /opt/ove/base_stack

**Generating Required Secrets**

Run the generate-sample-insecure-secrets.sh.

sudo ./generate-sample-insecure-secrets.sh

Using your preferred text editor (nano), make the following changes to
nucleus-stack.env:

sudo nano nucleus-stack.env

Uncomment Accept EULA:

ACCEPT_EULA=1

Uncomment Security Reviewed:

SECURITY_REVIEWED=1

Set the IP or Hostname:

SERVER_IP_OR_HOST=\<your-server-ip\>

Configure Nucleus passwords:

MASTER_PASSWORD=\<MY_NEW_PASSWORD\>

SERVICE_PASSWORD=\<MY_NEW_PASSWORD\>

Set the location for your Nucleus data:

DATA_ROOT=var/lib/nucleus-data (Provide the location with most space)

Configure your subnet:

Near the bottom of the nucleus-stack.env file, locate the subnet
section. If the subnet defined in CONTAINER_SUBNET conflicts with an
existing subnet already present in your network, change it here. IP
Addresses for Nucleus Docker containers will be allocated from this
subnet.

CONTAINER_SUBNET=192.168.2.0/26

By default, the WEB_PORT is configured to TCP 8080.

**\[Optional\] Mount Configurations**

The following configuration modifications are optional. These changes
enable you to configure your Enterprise Nucleus Server to mount a
different S3 bucket on or post deployment.

If you choose to rename the mount path from /NVIDIA, unmount the
original path using Nucleus Navigator prior to stopping the services.
This can be achieved by having an admin user right-click the /NVIDIA
mount, and click Unmount.

**Configure your mount path**

Choose to disable or enable reference mount (default= 1). Disabling will
start the Enterprise Nucleus Server without any reference path mounted:

REFERENCE_CONTENT_MOUNT_ENABLE=1

Choose the mount path within the Enterprise Nucleus Server (default=
/NVIDIA). The path must start with / as this is the root of Nucleus:

REFERENCE_CONTENT_MOUNT_TARGET=/NVIDIA

Define the S3 URL the mount will point to:

REFERENCE_CONTENT_SOURCE="content-production.omniverse.nvidia.com"

Define the bucket name for the reference path. While it is common for
many S3 URLs to include the bucket name, not all do. Supply the bucket
name if the URL does not contain the bucket name and/or Nucleus is
unable to connect to it:

REFERENCE_CONTENT_BUCKET=""

Choose to enable secure connections (HTTPS) to the S3 bucket (default=
1):

REFERENCE_CONTENT_SECURE=1

Configuration using a private bucket is possible, however; additional
configuration parameters are required:

\# (Must enable and supply all 3 parameters if enabled.)

\#

REFERENCE_CONTENT_USE_CREDENTIALS=0

REFERENCE_CONTENT_SOURCE_REGION=""

REFERENCE_CONTENT_BUCKET_ACCESS_KEY_ID=""

REFERENCE_CONTENT_BUCKET_SECRET_ACCESS_KEY=""

It is possible the S3 compliant storage may not support the full
expected schema. This option loosens the requirements on the schema, but
it is not recommended to change this unless your storage requires less
restrictions:

REFERENCE_CONTENT_NON_COMPLIANT_XML_SCHEMA=0

Once all configuration changes are complete, save the file using CTRL+O,
then exit the nano editor using CTRL+X.

**Running the Nucleus Stack**

Authenticate with NVIDIA Container Registry. For the 2023.2.9 release,
you must reauthenticate on nvcr.io with your NGC key

docker login nvcr.io

Username: \$oauthtoken

Password: NGC API Key

This command will pull the containers from the NVIDIA repository:

sudo docker compose --env-file /opt/ove/base_stack/nucleus-stack.env -f
/opt/ove/base_stack/nucleus-stack-no-ssl.yml pull

This command will start the stack in foreground:

sudo docker compose --env-file /opt/ove/base_stack/nucleus-stack.env -f
/opt/ove/base_stack/nucleus-stack-no-ssl.yml up

It is recommended to watch the logs initially to spot any errors or
issues. If none are observed, stop the stack by pressing Ctrl+C and
waiting for it to fully shut down, then restart it in “daemon” mode:

sudo docker compose --env-file /opt/ove/base_stack/nucleus-stack.env -f
/opt/ove/base_stack/nucleus-stack-no-ssl.yml up -d

**Testing the Installation**

Once the stack has been started using the above commands, open a web
browser on a workstation and access your Enterprise Nucleus Server using
the IP Address or Hostname with the port it’s configured to use.

http://\<server-ip\>:8080

If configured correctly, Nucleus Navigator should appear.

**Congratulations! You have successfully installed and configured your
Enterprise Nucleus Server!**
