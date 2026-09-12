# Floci Local Cloud Setup

A clean, reproducible setup for Floci local AWS cloud emulator and Floci UI with Docker Compose, Makefile, and `mise`.

---

## 1. Services & Ports

* **Floci Emulator (AWS Core)**: `http://localhost:4566` (Drop-in compatibility with standard AWS tools and SDKs)
* **Floci Web UI Dashboard**: `http://localhost:4500` (Frontend: `4500`, API: `4501`)

---

## 2. Runtime Management with `mise`

Project toolchains (Python, uv, Node.js, Terraform, Terragrunt) are defined in [`mise.toml`](./mise.toml).

### Installation & Shell Activation

1. **Install `mise`**:
   ```bash
   curl https://mise.run | sh
   ```

2. **Activate in your shell**:

   * **Bash** (`~/.bashrc`):
     ```bash
     echo 'eval "$(~/.local/bin/mise activate bash)"' >> ~/.bashrc
     source ~/.bashrc
     ```

   * **Fish** (`~/.config/fish/config.fish`):
     ```fish
     echo '~/.local/bin/mise activate fish | source' >> ~/.config/fish/config.fish
     source ~/.config/fish/config.fish
     ```

3. **Install project tools**:
   Inside this repository directory, run:
   ```bash
   mise install
   ```

---

## 3. Managing the Stack

Control the stack using the provided [`Makefile`](./Makefile) or Docker Compose directly.

### Start Services
```bash
make up
# or: docker compose up -d
```

### Stop Services
```bash
make down
# or: docker compose down
```

Once started, the Floci Web UI is available at `http://localhost:4500`.

---

## 4. AWS CLI `floci` Profile Configuration

Configure a dedicated named profile (`floci`) in the AWS CLI to route commands to the local emulator without conflicting with your real AWS credentials.

### One-Time Configuration

```bash
aws configure set aws_access_key_id test --profile floci
aws configure set aws_secret_access_key test --profile floci
aws configure set region us-east-1 --profile floci
aws configure set endpoint_url http://localhost:4566 --profile floci
```

### Profile Usage

* **Per-command flag**:
  ```bash
  aws s3 ls --profile floci
  ```

* **Set for current shell session**:
  * **Bash**:
    ```bash
    export AWS_PROFILE=floci
    ```
  * **Fish**:
    ```fish
    set -gx AWS_PROFILE floci
    ```

* **Set default permanently (Fish)**:
  ```fish
  set -Ux AWS_PROFILE floci
  ```
