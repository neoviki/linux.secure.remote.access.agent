# Secure Remote Access Agent

**Secure Remote Access Agent** is a utility to enable or disable secure remote access to your Linux machine using SSH reverse tunneling. It allows your local machine to be accessed by any remote user on the Internet through a public gateway server.

### Example Scenario

* **Local Machine (A)** – Your local Linux machine.
* **Public Server (M)** – Gateway server with a public IP and SSH access.
* **Remote User   (B)** – Can connect to Local Machine A via Public Server M once the agent is started.

```
Remote User (B) --> Public Server (M) --> Local Machine (A)
```

This shows the main function of this agent: Local Machine (A) is made temporarily accessible over the internet via a secure SSH reverse tunnel, controlled with `start` and `stop` commands.

---

## Features

* Enable (`start`) or disable (`stop`) remote access on-demand.
* Uses SSH reverse tunneling under the hood.
* Runs securely without permanently exposing your local machine.
* Can be integrated with your Linux apps or CLI tools.
* Works with a jump server / gateway with a public IP.

---

## Installation

1. Clone the repository:

```bash
git clone <repo-url>
cd <repo-folder>
```

2. Make the installer executable:

```bash
chmod +x installer.sh
```

3. Run the installer:

```bash
./installer.sh
```

This will install the application on your Linux machine.

---

## Usage

### Syntax

```bash
secure.remote.access.agent {start|stop} <local_port> <username>@<remote_server> [options]
```

* `<local_port>`: Local TCP port to expose.
* `<username>@<remote_server>`: SSH credentials for the gateway server.
* `[options]`: Optional flags (e.g., `-bg` to run in background).

---

### Example - Start Remote Access

```bash
secure.remote.access.agent start 2022 gw_user@test.domain.com
```

### Access Your Local Machine

* After starting the agent, you can access your local machine with:

```bash
ssh local_user@test.domain.com -p 2022
```

### Example - Stop Remote Access

```bash
secure.remote.access.agent stop 2022 
```

---

## Prerequisite 

* Ensure `sshpass` is installed on your local machine:

**Ubuntu / Debian:**

```bash
sudo apt-get install sshpass
```

**MacOS:**

```bash
brew install https://raw.githubusercontent.com/kadwanev/bigboybrew/master/Library/Formula/sshpass.rb
```

# On the Public Gateway Machine

* Copy the SSH port forwarding setup script to the gateway server:

```bash
scp -r server_setup/ <username>@<gateway_server>:/path/to/destination
```

* On the gateway server, enable SSH port forwarding, if not already enabled:

```bash
cd /path/to/destination/server_setup
./enable.ssh.port.forwarding
```

# Caution 

* The **remote server port** (in our example, port 2022) used for reverse tunneling must be **free**. If the port is already in use by another process, the tunnel will **fail**.
* Anyone who can access the **public server’s forwarded port** (in our example, port 2022) will be able to reach your local machine, so only expose the port number you intend to share.
* Always **stop the agent** after use to prevent unwanted exposure:

---

## Tested Systems

* Linux
* MacOS

---

## Uninstallation

1. Make sure the uninstaller is executable:

```bash
chmod +x uninstaller.sh
```

2. Run the uninstaller:

```bash
./uninstaller.sh
```

The uninstaller will:

* Remove each installed utility from the proper binary directory (`/usr/local/bin` or `/usr/bin`).

---

## License

MIT License

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
