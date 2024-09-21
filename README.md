# Flask Application Development and Deployment with Uv

## Local Development

### Step 1: Set Up Your Local Environment

1. Install Python on your local machine.
2. Install Uv:
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

### Step 2: Clone the Boilerplate

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/flask-uv-boilerplate.git
   cd flask-uv-boilerplate
   ```

### Step 3: Run Your Flask App Locally with Uv

1. Use the `uv` command to run your app:
   ```bash
   uv run app.py
   ```

2. Access your app at `http://localhost:5000`

### Step 4: Development Workflow

1. Make changes to the code as needed.
2. Run tests (if applicable):
   ```bash
   uv run pytest
   ```
3. Commit your changes:
   ```bash
   git add .
   git commit -m "Description of changes"
   ```
4. Push to your repository (if you have write access):
   ```bash
   git push origin main
   ```

## Deploying a Flask Application on Digital Ocean Using Uv

### Step 1: Create a Droplet

1. Log in to [Digital Ocean](https://www.digitalocean.com) and create a new Droplet.
2. Choose your preferred operating system (Ubuntu is recommended).
3. Select a plan based on your needs.
4. Add your SSH keys for secure access.
5. Launch the Droplet.

### Step 2: Connect to Your Droplet

1. Open your terminal.
2. Connect via SSH:
   ```bash
   ssh root@your_droplet_ip
   ```

### Step 3: Set Up Your Environment

1. Update your package lists:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. Install Python and pip:
   ```bash
   sudo apt install python3 python3-pip -y
   ```

### Step 4: Install Uv 

1. Install the `uv` package:
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

### Step 5: Clone Your Repository

1. Ensure you have added your SSH key to GitHub (follow the steps below).
2. Clone your repository:
   ```bash
   git clone git@github.com:your_username/your_repository.git
   ```

### Step 6: Run Your Flask App with Uv

1. Change to the directory of your cloned repository:
   ```bash
   cd your_repository
   ```
2. Use the `uv` command to run your app (replace `app:app` with the correct entry point if needed):
   ```bash
   uv run app:app --host 0.0.0.0 --port 8000
   ```

### Step 7: Configure Firewall

1. If you’re using UFW (Uncomplicated Firewall), allow the port:
   ```bash
   sudo ufw allow 8000
   ```

### Step 8: Access Your App

Open your browser and navigate to `http://your_droplet_ip:8000`. You should see your Flask application.

### Step 9: Adding Your SSH Key to GitHub

### Step 9.1: Generate an SSH Key (if you don’t have one)

1. Open a terminal on your local machine.
2. Generate a new SSH key:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```
   Press Enter to accept the default file location, and provide a passphrase if desired.

### Step 9.2: Copy the SSH Key

1. Copy your SSH key to the clipboard:
   ```bash
   cat ~/.ssh/id_rsa.pub
   ```
   Alternatively, you can use:
   ```bash
   pbcopy < ~/.ssh/id_rsa.pub  # For macOS
   clip < ~/.ssh/id_rsa.pub     # For Windows
   ```

### Step 9.3: Add the SSH Key to GitHub

1. Log in to your [GitHub account](https://github.com).
2. Go to **Settings** (click on your profile picture in the top right).
3. In the left sidebar, click on **SSH and GPG keys**.
4. Click on the **New SSH key** button.
5. Give your key a title (e.g., "Digital Ocean Droplet").
6. Paste your SSH key into the "Key" field.
7. Click on **Add SSH key**.

### Step 9.4: Test Your SSH Connection

1. Test the connection to GitHub:
   ```bash
   ssh -T git@github.com
   ```
   You should see a message saying you've successfully authenticated.

## Step 10: Running the App in Production (Optional)

For production deployment, consider using a process manager like **systemd** or **supervisor** to manage your application.

### Additional Steps for Nginx (Optional)

1. Install Nginx:
   ```bash
   sudo apt install nginx -y
   ```
2. Configure Nginx to reverse proxy requests to your `uv` server:
   ```nginx
   server {
       listen 80;
       server_name your_droplet_ip;
       #to change to domain name do server_name your_domain_name;
       location / {
           proxy_pass http://127.0.0.1:PORT;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
       }
   }
   ```
3. Enable the configuration:
   ```bash
   sudo ln -s /etc/nginx/sites-available/your_app /etc/nginx/sites-enabled/
   ```
4. Test the Nginx configuration:
   ```bash
   sudo nginx -t
   ```
5. Restart Nginx:
   ```bash
   sudo systemctl restart nginx
   ```

### Step 6: Securing Nginx with Let's Encrypt

To enable HTTPS and secure your Nginx server with SSL/TLS certificates, follow these steps:

1. Install Certbot:
   ```bash
   sudo snap install core; sudo snap refresh core
   sudo snap install --classic certbot
   sudo ln -s /snap/bin/certbot /usr/bin/certbot
   ```

2. Obtain an SSL certificate:
   ```bash
   sudo certbot --nginx -d your_domain -d www.your_domain
   ```
   Replace `your_domain` with your actual domain name.

3. Follow the prompts to configure your HTTPS settings. Certbot will automatically modify your Nginx configuration to use the new certificate.

4. Test automatic renewal:
   ```bash
   sudo certbot renew --dry-run
   ```

Your Nginx server is now secured with Let's Encrypt SSL/TLS certificates, and HTTPS is enabled for your Flask application.

