# **How to Run:**
1. Pull the image from Docker Hub.
2. Run the container with port 2222 exposed for SSH access.
3. Connect via SSH using the predefined user credentials (username: `py`, password: `py`).

## **On Linux:**
1. **Build the Image:**
   ```bash
   docker build --tag py-ssh:latest .
   ```
2. **Run the Container:**
   ```bash
   docker run -d --gpus=all -p 127.0.0.1:2222:2222 -v /tmp/.X11-unix:/tmp/.X11-unix  --name=py-ssh --restart=unless-stopped --hostname=py-ssh py-ssh:latest
   ```
   Ensure X11 is running on your host system and accepts external sources (`xhost +`).
3. **Connect to the Container:**
   ```bash
   ssh -X -p2222 py@127.0.0.1
   ```

Optional: **Secure with SSH Private Key:**
 1. **Get Key:**
       ```bash
       sudo docker logs py-ssh | awk '/-----BEGIN OPENSSH PRIVATE KEY-----/,/-----END OPENSSH PRIVATE KEY-----/'
       ```

2. **Disable password login:**

        Edit `/etc/ssh/sshd_config` and set `PasswordAuthentication no`, restart container.
