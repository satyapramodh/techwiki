Ref:
https://dev.to/felipecrs/simply-run-docker-on-wsl2-3o8


So, now that Docker Desktop is paid under certain scenarios, you may want to switch to something else.

This is a straight to the point guide on how to make Docker CE run fully on WSL2.

PS: the title says right way, but it is just my personal opinion. I am not claiming that any other guide does it in a wrong way.

What you will get
A full-fledged Docker installation on WSL2
Docker Daemon automatic start without any crazy hacks
What you will not get
Docker Daemon sharing between Windows and WSL (i.e. you cannot run docker from Windows PowerShell)
Docker Daemon sharing between WSL distributions
Requisites
I will consider that you already have WSL2 working, and you are using Ubuntu as your distribution.

Guide
Install Docker CE on Ubuntu by following the official guide:
# Ensures not older packages are installed
    $ sudo apt-get remove docker docker-engine docker.io containerd runc

# Ensure pre-requisites are installed
    $ sudo apt-get update
    $ sudo apt-get install \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg \
        lsb-release

# Adds docker apt repository
    $ echo \
        "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" |
        sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Adds docker apt key
    $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg |
        sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Refreshes apt repos
    $ sudo apt-get update

# Installs Docker CE
    $ sudo apt-get install docker-ce docker-ce-cli containerd.io
Perform the post-installation steps:
# Ensures docker group exists
    $ sudo groupadd docker

# Ensures you are part of it
    $ sudo usermod -aG docker $USER

# Now, close your shell and open another for taking the group changes into account
Make Docker Daemon start on WSL initialization:

If you are running Windows 11, you can use a brand-new feature of WSL to start the Docker Daemon during the initialization. You only need to add:

    [boot]
    command = "service docker start"

To your /etc/wsl.conf within your WSL distribution. Then, restart it with `wsl.exe --shutdown`. To verify that it works, you can run docker version. If you do not receive any permission denied error, you are good.


But if you are not running Windows 11, you can achieve a similar result with the following approach:


Open the ~/.profile (or ~/.zprofile if you are using ZSH rather than Bash) in your WSL distribution, and add a section like so to it:

    if service docker status 2>&1 | grep -q "is not running"; then
        wsl.exe -d "${WSL_DISTRO_NAME}" -u root -e /usr/sbin/service docker start >/dev/null 2>&1
    fi

This piece of code will run every time you open a new shell on your WSL distribution. It checks whether the Docker Daemon is running, and if not, starts it without prompting for credentials. Without any noticeable delay.


To verify, after making the changes, open a new shell and run the docker version command. If you do not receive any permission denied error, you are good.

If you are unsure about which method to choose, you can choose both. It's harmless, I do it on mine.

Bonus
Installing Docker Compose (v2+):
# Finds the latest version
    $ compose_version=$(curl -fsSL -o /dev/null -w "%{url_effective}" https://github.com/docker/compose/releases/latest | xargs basename)

# Downloads the binary to the plugins folder
    $ curl -fL --create-dirs -o ~/.docker/cli-plugins/docker-compose \
        "https://github.com/docker/compose/releases/download/${compose_version}/docker-compose-linux-$(uname -m)"

# Assigns execution permission to it
    $ chmod +x ~/.docker/cli-plugins/docker-compose
To verify it works you can run docker compose version.

Installing Docker Compose Switch (to use the docker-compose command):
# Finds the latest version
    $ switch_version=$(curl -fsSL -o /dev/null -w "%{url_effective}" https://github.com/docker/compose-switch/releases/latest | xargs basename)

# Downloads the binary
    $ sudo curl -fL -o /usr/local/bin/docker-compose \
        "https://github.com/docker/compose-switch/releases/download/${switch_version}/docker-compose-linux-$(dpkg --print-architecture)"

# Assigns execution permission to it
    $ sudo chmod +x /usr/local/bin/docker-compose
PS: I suggest to install the docker-compose to /usr/local/bin/ because otherwise, the VS Code - Remote Containers extension will not find it.

To verify it works, you can run docker-compose version.

Install the Docker Credential Helper:

You will need this if you want Docker to store your credentials securely when you perform docker login. Thanks to the WSL interoperability between Windows, you can install the Windows version of the Docker Credential Helper inside of WSL itself.
# Finds the latest version
    $ wincred_version=$(curl -fsSL -o /dev/null -w "%{url_effective}" https://github.com/docker/docker-credential-helpers/releases/latest | xargs basename)

# Downloads and extracts the .exe
    $ sudo curl -fL \
        "https://github.com/docker/docker-credential-helpers/releases/download/${wincred_version}/docker-credential-wincred-${wincred_version}-$(dpkg --print-architecture).zip" |
        zcat | sudo tee /usr/local/bin/docker-credential-wincred.exe >/dev/null

# Assigns execution permission to it
    $ sudo chmod +x /usr/local/bin/docker-credential-wincred.exe
Then, configure your Docker CLI to use it by assuring that the following is present in your ~/.docker/config.json:
    {
        "credsStore": "wincred.exe"
    }
To verify that it works, you can try to docker login and if not, Docker will complain about storing credentials in plain text.

Enabling Docker BuildKit:

As it came enabled by Docker Desktop before. It's simple, ensure that the following is present in your /etc/docker/daemon.json:
    {
        "features": {
            "buildkit": true
        }
    }
You will need to edit this file as root, so make sure to use sudo before running your editor (sudo nano /etc/docker/daemon.json).

Final considerations
The entire setup process may take some time, but you will have achieved almost everything that Docker Desktop used to provide to you (by the way, I use kind as an alternative to Docker Desktop's built-in K8s).

However, you can achieve a similar (and even higher/better) level of easiness that Docker Desktop provided to you by wrapping all the steps above in your dotfiles installation steps.

For example, in my dotfiles, every single of these steps are automated, including configuring /etc/docker/daemon.json, changing ~/.profile, and even providing a way to automatically update your extra binaries (docker-compose, or the wincred.exe) every time you update your dotfiles (by using a feature of chezmoi - a dotfiles manager which I totally recommend).

