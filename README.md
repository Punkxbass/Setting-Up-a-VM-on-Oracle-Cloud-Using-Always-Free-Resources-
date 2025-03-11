# Setting Up a Virtual Machine on Oracle Cloud (Using Always Free Resources)

This guide provides a detailed, step-by-step process to create a Virtual Machine (VM) on Oracle Cloud using Always Free resources. Follow these instructions carefully to ensure a successful setup.

## Prerequisites
- An Oracle Cloud account (sign up at [Oracle Cloud](https://www.oracle.com/cloud/))
- Basic understanding of SSH and Linux commands
- A stable internet connection

## Step 1: Sign In to Oracle Cloud
1. Go to [Oracle Cloud Console](https://cloud.oracle.com/).
2. Sign in with your Oracle Cloud credentials.
3. Navigate to the **Compute** section by selecting **Menu > Compute > Instances**.

## Step 2: Create a New Virtual Machine (VM)
1. Click **Create Instance**.
2. Enter a name for your instance (e.g., `ProjectZomboid-Server`).
3. Under **Placement**, select the **Region** closest to your location.
4. In the **Shape** section, click **Change Shape** and select **Ampere VM.Standard.A1.Flex** (ARM-based, Always Free eligible).
5. Set **OCPU Count** to `4` and **Memory (GB)** to `24` (maximum for Always Free).

## Step 3: Configure the OS Image
1. Under **Image and Shape**, click **Edit**.
2. Select **Ubuntu 22.04** as the OS.
3. Click **Select Image**.

## Step 4: Configure Networking
1. Under **Networking**, select **Create a New Virtual Cloud Network (VCN)**.
2. Ensure **Assign a Public IP Address** is enabled.
3. Click **Create Subnet** and ensure it is attached to the default VCN.

## Step 5: Add SSH Keys
1. Under **Add SSH Keys**, choose **Generate SSH Key Pair** (or upload an existing key).
2. If generating, download the private key (`.pem` file) and store it securely.
3. Click **Create** to launch the VM.

## Step 6: Connect to the VM
1. Once the instance is running, copy the **Public IP Address** from the instance details page.
2. Open a terminal and run:
   ```sh
   ssh -i /path/to/private-key.pem ubuntu@your-vm-ip
   ```
3. If prompted, type `yes` to confirm the connection.

## Step 7: Configure Firewall Rules
Run the following commands to allow traffic for Project Zomboid:
```sh
sudo su
iptables -I INPUT -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 16261 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 16261 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 16262 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 16262 -j ACCEPT
sudo iptables-save > /etc/iptables/rules.v4
sudo systemctl restart iptables
sudo ufw disable
```

## Step 8: Update the System
Run:
```sh
sudo apt update && sudo apt upgrade -y
reboot
```

## Next Steps
Your Oracle Cloud VM is now set up and ready for use! Proceed to the [Project Zomboid Server Setup Guide](https://github.com/Punkxbass/PZ-Dedicated-Server-on-an-ARM-Arch) to install and configure a dedicated server on your VM.
