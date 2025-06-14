# 📊 MicroK8s Dashboard Access Guide

This guide helps you access the Kubernetes Dashboard for a MicroK8s cluster, both locally and remotely, and troubleshoot common port and proxy issues.

---

## 🖥️ Local Dashboard Access

On the MicroK8s host node (e.g., `k8s-odin`), run:

```bash
microk8s dashboard-proxy

You should see output like:

Dashboard will be available at https://127.0.0.1:10443
Use the following token to login:
<your-token-here>

Open this URL in a browser on the same machine:

https://127.0.0.1:10443

Paste the token when prompted to log in.
🌐 Remote Dashboard Access (via SSH Tunnel)

To access the dashboard remotely (e.g., from your laptop fenrir), establish an SSH tunnel:

ssh -L 10443:127.0.0.1:10443 -p <custom-port> user@<ip-address>

Then, on your local machine, open:

https://127.0.0.1:10443

Login with the token output from the microk8s dashboard-proxy command.
🛠 Troubleshooting
🔍 Check for Running Proxy Processes

ps aux | grep dashboard-proxy

Expected Output Example:

vxadmin  12345  0.1  0.2 123456 23456 ? Ss 10:12  0:01 /snap/microk8s/...

❌ Kill a Stale Dashboard Proxy

kill 12345
# or, if it doesn't stop:
kill -9 12345

🧵 Port 10443 Already in Use?

Check what’s occupying the port:

sudo lsof -i :10443

Example Output:

COMMAND   PID     USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
kubectl  123400  vxadmin  9u  IPv4  123456     0t0  TCP *:10443 (LISTEN)

Kill the process:

kill -9 123400

🔁 Restart the Dashboard Proxy

After clearing any issues, restart:

microk8s dashboard-proxy

Then open:

https://127.0.0.1:10443

Log in with the token provided.
✅ Tips

    Keep your SSH tunnel open while using the dashboard.

    Avoid exposing the dashboard to the public internet.

    For convenience, consider adding an SSH config entry or wrapper script for repeated use.


---

Let me know if you want a version with collapsible `<details>` blocks or Markdown badges
