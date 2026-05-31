# n8n-docker-windows
Install n8n Locally with Docker Desktop on Windows and MAC (Step-by-Step Guide)
===================================================================================

Study Guide: Local n8n Installation with Docker Desktop
1. Introduction to Local n8n Setup
Setting up n8n locally provides a powerful environment for building and testing workflows. While many users default to the command line, this guide focuses on utilizing the Docker Desktop graphical interface. This approach offers the best of both worlds: an installation that is technically identical to a CLI-based setup but managed through a user-friendly UI. By following this guide, you will ensure your local instance is robust, persistent, and ready for development.

2. Hardware Selection and Prerequisites
To ensure optimal performance, you must download the Docker Desktop version tailored to your specific hardware architecture. Using the incorrect version can lead to significant virtualization overhead and system instability.
Mac Users:
Apple Silicon: Select this if your Mac uses an M-series chip (M1, M2, M3).
Intel Chip: Select this for older Mac models using Intel processors.
Windows Users:
AMD64: The standard choice for the vast majority of Windows desktops and laptops.
ARM64: Specifically for newer "Copilot+" PCs using ARM-based processors.
System Permissions:
Administrator/Root Access: You must have full administrative privileges. Docker requires root-level access to manage the underlying virtualization engine.
WSL 2 (Windows Subsystem for Linux): This is a mandatory backend for Docker on Windows.
Pro-Tip: WSL 2 Troubleshooting If the Docker installer does not automatically prompt you to enable WSL 2, or if Docker fails to start after installation, open PowerShell as an Administrator and run the command wsl --install. This manually triggers the necessary Windows components for Docker to function.

    <img width="1783" height="972" alt="image" src="https://github.com/user-attachments/assets/f2ff1d36-4920-4770-824a-f9bf8fa56dd5" />


3. Docker Desktop Installation Process
The installation follows standard OS-specific procedures, but requires attention to the finalization steps.
For Mac:
Download the appropriate .dmg file.
Open the disk image and drag the Docker icon into your Applications folder.
For Windows:
Run the installation wizard.
Ensure the "Use WSL 2 instead of Hyper-V" option is checked (if prompted).
Complete the wizard and restart your machine if requested.

4. Establishing Data Persistence (Creating a Volume)
Before pulling the n8n software, you must prepare a place for its data to live. Without this step, your work is ephemeral.
Why This Matters By default, Docker containers are "stateless." If you restart your computer or update the n8n image, all your workflows and credentials will be deleted. Creating a Volume ensures that your data is stored on your hard drive independently of the container, providing persistence across reboots and updates.
Actionable Instruction: Navigate to the Volumes tab in Docker Desktop and click Create Volume. Give it a recognizable name (e.g., n8n_data).

5. Pulling and Configuring the n8n Image
With the storage infrastructure ready, you can now acquire the n8n application.
Stage 1: Pulling the Image: Navigate to the Images tab. Search for "n8n" and click Pull on the official n8n image. This downloads the latest stable version to your machine.
Stage 2: Container Initialization: Once the image status shows "Unused" or "Downloaded," click the Run button. Crucial: Do not click the second "Run" immediately in the pop-up; instead, click Optional Settings to access advanced configuration.

6. Advanced Container Configuration & Environment Variables
To achieve parity with a professional CLI installation, you must map your volume and set specific environment variables.
Container Mapping Settings:
Container Name: n8n
Host Port: 5678 (This is the port you will type into your browser to access the UI).
Volume Mapping: Under the Volumes section, select the volume you created in Step 4 from the Volume dropdown.
Container Path: Set this to /home/node/.n8n.
Key Takeaway: The path /home/node/.n8n is the standard internal directory where n8n stores its SQLite database and configuration files. Correct mapping here is what enables the data persistence mentioned in Step 4.
Mandatory Environment Variables: Add the following variables to ensure the application environment is correctly synchronized:
GENERIC_TIMEZONE: Set this to your region (e.g., Europe/Berlin).
TZ: Set this to the exact same value as GENERIC_TIMEZONE.
Note: Using both variables ensures that both the n8n application and the underlying Linux OS inside the container are synchronized. If you are unsure of your string, consult the IANA Time Zone Database.
N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS: Set to true. This enforces standard security protocols for your configuration files.
N8N_NODE_RUNNERS_ENABLED: Set to true. This enables the modern execution mode, matching the performance of a CLI-based setup.

7. Post-Installation and Functional Limitations
After clicking Run, the container will initialize. After a few seconds, the "Status" should change to "Running." You can then open your browser and navigate to localhost:5678 to register your owner account.
Warning: Webhooks In this local configuration, Webhooks will not work for external triggers (like receiving data from Typeform or GitHub). Because your instance is behind a local firewall, external services cannot "see" it. To enable webhooks, you would need to set up a tunnel (such as LocalTunnel or Cloudflare), which is a separate configuration process.
Final Pro-Tip: Always ensure Docker Desktop is running before you attempt to open n8n. If you see a "Connection Refused" error in your browser, check the Docker Desktop dashboard to ensure the container hasn't been stopped.
