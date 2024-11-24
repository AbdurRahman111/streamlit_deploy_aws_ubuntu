# streamlit_deploy_aws_ubuntu

git clone https://github.com/kintahllc/kintah_dashboard.git
cd kintah_dashboard


sudo apt install python3-venv -y

python3 -m venv venv

source venv/bin/activate

pip install --upgrade pip
pip install -r requirements.txt

streamlit run app.py --server.port 8502 --server.address 0.0.0.0
http://your-server-ip:8502

sudo nano /etc/systemd/system/streamlit-dashboard.service

[Unit]
Description=Streamlit Dashboard Service
After=network.target

[Service]
User=ubuntu
WorkingDirectory=/home/ubuntu/kintah_dashboard
ExecStart=/home/ubuntu/kintah_dashboard/venv/bin/streamlit run app.py --server.port 8502 --server.address 0.0.0.0
Restart=always

[Install]
WantedBy=multi-user.target



sudo systemctl daemon-reload
sudo systemctl enable streamlit-dashboard


sudo systemctl start streamlit-dashboard

sudo systemctl status streamlit-dashboard



sudo nano /etc/nginx/sites-available/your-django-site


server {
    listen 80;
    server_name dashboard.example.com;

    location / {
        proxy_pass http://127.0.0.1:8502;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}



sudo systemctl restart nginx

http://your-domain.com/dashboard/

Verify Deployment
Without Nginx: Visit http://your-server-ip:8502.
With Nginx: Visit http://your-domain.com/dashboard.
