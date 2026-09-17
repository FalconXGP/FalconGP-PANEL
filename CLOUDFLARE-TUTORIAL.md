<p align="center">
  <img src="https://i.postimg.cc/dQBhBwQg/file-00000000923881fdb18fb3005102c2a6.png" alt="FalconGP-PANEL Banner" width="100%">
</p><h1 align="center">🦅 𝐅𝐚𝐥𝐜𝐨𝐧𝐆𝐏 — 𝐂𝐥𝐨𝐮𝐝𝐟𝐥𝐚𝐫𝐞 𝐓𝐮𝐧𝐧𝐞𝐥 𝐓𝐮𝐭𝐨𝐫𝐢𝐚𝐥</h1><p align="center">
  <strong>Expose FalconGP Panel or Minecraft Services Securely Through Cloudflare Tunnel</strong>
</p><p align="center">
  <a href="https://falconxgp.indevs.in">
    <img src="https://img.shields.io/badge/𝐎𝐟𝐟𝐢𝐜𝐢𝐚𝐥%20𝐒𝐢𝐭𝐞-6f42c1?style=for-the-badge" alt="Official Site">
  </a>
  <a href="https://youtube.com/@superytplayz01">
    <img src="https://img.shields.io/badge/𝐘𝐨𝐮𝐓𝐮𝐛𝐞-red?style=for-the-badge&logo=youtube&logoColor=white" alt="YouTube">
  </a>
  <a href="https://discord.gg/kcW4f2c8CV">
    <img src="https://img.shields.io/badge/𝐃𝐢𝐬𝐜𝐨𝐫𝐝-5865F2?style=for-the-badge&logo=discord&logoColor=white" alt="Discord">
  </a>
  <a href="https://github.com/FalconXGP/FalconGP-PANEL">
    <img src="https://img.shields.io/badge/𝐆𝐢𝐭𝐇𝐮𝐛-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub">
  </a>
</p>---

📌 𝟏. 𝐈𝐧𝐬𝐭𝐚𝐥𝐥 𝐂𝐥𝐨𝐮𝐝𝐟𝐥𝐚𝐫𝐞 𝐓𝐮𝐧𝐧𝐞𝐥

Update your VPS package list and install the required packages:

sudo apt update #AND
sudo apt install -y curl wget ca-certificates

Download and install "cloudflared":

curl -L --output cloudflared.deb https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb

Install the package:

sudo dpkg -i cloudflared.deb

If any dependencies are missing, run:

sudo apt install -f -y

Check the installation:

cloudflared --version

«Note: The command above is for an "x86_64 / amd64" VPS.
For ARM-based VPS systems, use the correct ARM package from the official Cloudflare releases.»

---

🌐 𝟐. 𝐂𝐡𝐨𝐨𝐬𝐞 𝐘𝐨𝐮𝐫 𝐂𝐨𝐧𝐧𝐞𝐜𝐭𝐢𝐨𝐧 𝐌𝐞𝐭𝐡𝐨𝐝

Option A — Quick Tunnel Without a Domain

Use this method if you do not own or manage a domain.

cloudflared tunnel --url http://localhost:3000

Cloudflare will generate a temporary public URL similar to:

https://example-random-name.trycloudflare.com

«Important: Quick Tunnels are temporary and are mainly useful for testing or temporary access.
They are not recommended for a permanent production panel.»

«Tip&Tricks: Use screen -s cloudflare to run your quicktunnel in background, to re enter use screen -r cloudflare.

«IssueSolve: if you had a problem durning open a screen use this - apt install -y screen
script /dev/null -c "screen -S cloudflare" example.
---

Option B — Named Tunnel With Your Own Domain

Use this method if you have a domain connected to Cloudflare.

Example domain:

panel.example.com

Example local service:

http://localhost:3000

Replace these example values with your own domain and port.

---

🔐 𝟑. 𝐋𝐨𝐠𝐢𝐧 𝐓𝐨 𝐂𝐥𝐨𝐮𝐝𝐟𝐥𝐚𝐫𝐞

Run:

cloudflared tunnel login

A login URL will appear in the terminal.

1. Open the displayed URL in your browser.
2. Log in to your Cloudflare account.
3. Select the domain you want to use.
4. Authorize Cloudflare Tunnel.
5. Return to your VPS terminal.

After successful login, Cloudflare will save the authentication certificate on your server.

---

🦅 𝟒. 𝐂𝐫𝐞𝐚𝐭𝐞 𝐀 𝐍𝐚𝐦𝐞𝐝 𝐓𝐮𝐧𝐧𝐞𝐥

Create a tunnel named "FalconGP":

cloudflared tunnel create FalconGP

Check the available tunnels:

cloudflared tunnel list

Copy your tunnel UUID from the output.

Example:

12345678-abcd-1234-abcd-123456789abc

Set your tunnel UUID as an environment variable:

export TUNNEL_UUID="YOUR-TUNNEL-UUID"

«Replace "YOUR-TUNNEL-UUID" with your actual tunnel UUID.»

---

📁 𝟓. 𝐂𝐫𝐞𝐚𝐭𝐞 𝐓𝐡𝐞 𝐂𝐨𝐧𝐟𝐢𝐠 𝐃𝐢𝐫𝐞𝐜𝐭𝐨𝐫𝐲

Create the Cloudflare configuration directory:

sudo mkdir -p /etc/cloudflared

Copy your tunnel credentials:

sudo cp ~/.cloudflared/*.json /etc/cloudflared/

Create the configuration file:

sudo nano /etc/cloudflared/config.yml

Add the following configuration:

tunnel: YOUR-TUNNEL-UUID
credentials-file: /etc/cloudflared/YOUR-TUNNEL-UUID.json

ingress:
  - hostname: panel.example.com
    service: http://localhost:3000

  - service: http_status:404

Change these values:

YOUR-TUNNEL-UUID
panel.example.com
localhost:3000

Save the file:

CTRL + O
ENTER
CTRL + X

Check the configuration:

cloudflared tunnel ingress validate /etc/cloudflared/config.yml

---

🔗 𝟔. 𝐑𝐨𝐮𝐭𝐞 𝐘𝐨𝐮𝐫 𝐃𝐨𝐦𝐚𝐢𝐧

Create a DNS route for your tunnel:

cloudflared tunnel route dns FalconGP panel.example.com

Replace:

FalconGP

with your tunnel name, and replace:

panel.example.com

with your real domain or subdomain.

Example:

cloudflared tunnel route dns FalconGP panel.anoscloud.example

---

🧪 𝟕. 𝐓𝐞𝐬𝐭 𝐓𝐡𝐞 𝐓𝐮𝐧𝐧𝐞𝐥

Run the tunnel manually first:

cloudflared tunnel --config /etc/cloudflared/config.yml run FalconGP

Open your configured domain in a browser:

https://panel.example.com

If your FalconGP panel is running on port "3000", it should now be accessible through your Cloudflare domain.

---

🖥️ 𝟖. 𝐑𝐮𝐧 𝐓𝐡𝐞 𝐓𝐮𝐧𝐧𝐞𝐥 𝐖𝐢𝐭𝐡 𝐒𝐜𝐫𝐞𝐞𝐧

Install Screen:

sudo apt install -y screen

Create a detached Screen session:

screen -S cloudflare

Start Cloudflare Tunnel inside the Screen session:

cloudflared tunnel --config /etc/cloudflared/config.yml run FalconGP

Detach from Screen without stopping the tunnel:

CTRL + A
D

The Cloudflare Tunnel will continue running after you leave the Screen session.

---

🔎 𝟗. 𝐒𝐜𝐫𝐞𝐞𝐧 𝐂𝐨𝐦𝐦𝐚𝐧𝐝𝐬

List active Screen sessions:

screen -ls

Reconnect to the Cloudflare session:

screen -r cloudflare

Stop the Cloudflare Tunnel:

CTRL + C

Close the Screen session:

screen -S cloudflare -X quit

---

🔄 𝟏𝟎. 𝐑𝐞𝐬𝐭𝐚𝐫𝐭 𝐓𝐡𝐞 𝐓𝐮𝐧𝐧𝐞𝐥

If the tunnel stops, reconnect to the VPS and run:

screen -r cloudflare

If the Screen session does not exist, create it again:

screen -S cloudflare

Then start the tunnel:

cloudflared tunnel --config /etc/cloudflared/config.yml run FalconGP

Detach again:

CTRL + A
D

---

⚠️ 𝐈𝐦𝐩𝐨𝐫𝐭𝐚𝐧𝐭 𝐍𝐨𝐭𝐞𝐬

- Replace every example domain with your own domain.
- Replace "YOUR-TUNNEL-UUID" with your real tunnel UUID.
- Make sure FalconGP is running on the configured local port.
- The example configuration uses "localhost:3000".
- Quick Tunnels are temporary and should not be used as a permanent production solution.
- Never share your Cloudflare tunnel credentials or certificate files.
- Cloudflare Tunnel is designed for HTTP/HTTPS services. Minecraft TCP/UDP services may require a suitable Cloudflare product or another tunneling solution.
- Keep the Cloudflare Tunnel running inside Screen if you are not using a system service.

---

<p align="center">
  <strong>𝐀𝐧𝐨𝐬𝐂𝐥𝐨𝐮𝐝™ — 𝐏𝐨𝐰𝐞𝐫𝐞𝐝 𝐁𝐲 𝐅𝐚𝐥𝐜𝐨𝐧𝐆𝐏</strong>
</p>
