# TCP Port Tunneler

**TCP Port Tunneler** is a utility to start or stop TCP port tunneling using SSH reverse connections.
It allows you to make any local TCP port accessible over the internet through a remote gateway server with a public IP.

### Example Application Scenarios

1. To share or expose your **local web server** so that it can be accessed by any user on the Internet.
2. To share your **local SSH port**, allowing remote users to connect securely to your computer via SSH.
3. To expose a **local database or API service** temporarily for external testing or integration.
4. To provide **remote access to IoT devices or home servers** behind NAT or firewalls without changing router settings.

### How It Works

* **Local Machine (A)** – The machine hosting the service (for example, a web server on port 8080 or SSH on port 22).
* **Public Server (M)** – A gateway server with a public IP address and SSH access, used as the intermediary.
* **Remote User (B)** – A user who connects to the service running on Local Machine (A) through the Public Server (M) once the tunnel is established.

```

Remote User (B) --> Public Server (M) --> Local Machine (A)

````

This illustrates the main function of this agent: **Local Machine (A)** becomes temporarily accessible over the Internet via a secure SSH reverse tunnel, controlled with `start` and `stop` commands.


---

## Installation

1. Clone the repository:

```bash
git clone <repo-url>
cd <repo-folder>
````

2. Make the installer executable:

```bash
chmod +x installer.sh
```

3. Run the installer:

```bash
./installer.sh
```

This installs the utility on your Linux machine.

---

## Usage

### Syntax

```bash
tcp.port.tunneler {start|stop} <local_port> <username>@<remote_server> [options]
```

* `<local_port>` – Local TCP port you want to tunnel.
* `<username>@<remote_server>` – SSH credentials for the gateway server.
* `[options]` – Optional flags (e.g., `-bg` to run in background).

---

### Example - Start Tunnel

```bash
tcp.port.tunneler start 8080 user@gateway.example.com
```

After the tunnel starts, the utility will display the **public address and port** where your local service can be accessed.

Example output:

```
####################################################################
 Local Port : 8080
 Public Host: gateway.example.com
 Public Port: 10001
####################################################################
```

Now anyone can reach your local service via:

```
http://gateway.example.com:10001
```

---

### Example - Stop Tunnel

```bash
tcp.port.tunneler stop 8080
```

This stops the reverse SSH tunnel for the given local port.

---

## Prerequisites

Ensure `sshpass` is installed on your local machine:

**Ubuntu / Debian:**

```bash
sudo apt-get install sshpass
```

**MacOS:**

```bash
brew install https://raw.githubusercontent.com/kadwanev/bigboybrew/master/Library/Formula/sshpass.rb
```

---

### On the Public Gateway Machine

1. Copy the SSH port forwarding setup script to the gateway server:

```bash
scp -r server_setup/ <username>@<gateway_server>:/path/to/destination
```

2. On the gateway server, enable SSH port forwarding (if not already enabled):

```bash
git clone <repo-url>
cd linux.tcp.port.tunneler
cd server.setup.script
./enable.ssh.port.forwarding
```

This updates `/etc/ssh/sshd_config` to allow reverse tunnels.

---

## Caution

* The **remote port** assigned for reverse tunneling must be **free** on the gateway server.
  If the port is already in use, the tunnel will **fail** to start.
* Anyone who can access the **gateway’s forwarded port** can reach your local service.
  Expose only the ports you intend to share.
* Always **stop the tunnel** after use to prevent unwanted exposure:

```bash
tcp.port.tunneler stop <local_port>
```

---

## Tested Systems

* Linux
* MacOS

---

## Uninstallation

1. Make the uninstaller executable:

```bash
chmod +x uninstaller.sh
```

2. Run the uninstaller:

```bash
./uninstaller.sh
```

The uninstaller will:

* Remove the installed binary from your system (e.g., `/usr/local/bin` or `/usr/bin`).
* Clean up related configuration files if any.

---

## License

This project is licensed under the MIT License.

---

## Note on Project and Application Naming Convention

I use **dot-separated names** for projects and applications (e.g., `linux.remote.access.agent`) to make each project’s purpose immediately clear. With over 40 projects - many of which I use regularly - this naming system helps me **quickly use applications without forgetting their names**, and **easily track, remember, and understand what each project does** without even opening it.

I specifically chose **dots (`.`)** instead of underscores (`_`), hyphens (`-`), or PascalCase because:

* Underscores require **Shift** to type on most keyboards, which is slower.
* Hyphens are **not consistently accepted in file naming** on all systems, and their key position is farther from the base row than the dot key.
* With Linux terminal auto-complete, using dots makes it easier to find and run applications - for example, typing `linux.<Tab>` quickly lists all applications starting with “linux.”
* Dots are **widely supported across major operating systems** (Linux, macOS, and Windows) and are easy to type.
* This makes naming simple, consistent, and cross-platform friendly (I rarely use Windows).

This approach is based on **my personal experience and workflow over many years**. It has proven efficient and intuitive for me, though preferences may vary among developers - it is simply the convention that works best in my environment.
