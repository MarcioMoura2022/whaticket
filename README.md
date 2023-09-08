Repositorio Versão Whaticket DMIX Informática

Esta versão foi baseada no whaticket do CANOVE com algumas modificações!!!

Instruções de Instalação:

​​👉 ​INSTRUÇÕES BÁSICAS:

CRIAR SUBDOMINIO E APONTAR PARA O IP DA SUA VPS

================================================

sudo su root

cd ~

apt install mysql-server

mysql --version

sudo systemctl status mysql

sudo mysql -u root

mysql> CREATE DATABASE whaticket CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;

mysql> USE mysql;

mysql> UPDATE user SET plugin='mysql_native_password' WHERE User='root';

mysql> FLUSH PRIVILEGES;

mysql> exit;

service mysql restart

sudo su root

adduser deploy

usermod -aG sudo deploy

su deploy

sudo apt update && sudo apt upgrade

curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -

sudo apt-get install -y nodejs

sudo apt install apt-transport-https ca-certificates curl software-properties-common

sudo apt update

sudo usermod -aG mysql ${USER}

su - ${USER}

cd ~

git clone URL_GIT_PROJETO

cp whaticket/backend/.env.example whaticket/backend/.env

nano whaticket/backend/.env

NODE_ENV=

BACKEND_URL=https://subdominioapi.dominio.com.br

FRONTEND_URL=https://subdominioapp.dominio.com.br

PORT=8080

PROXY_PORT=443

CHROME_BIN=/usr/bin/google-chrome-stable

DB_DIALECT=mysql

DB_HOST=localhost

DB_USER=root

DB_PASS=

DB_NAME=whaticket

JWT_SECRET=SECRET

JWT_REFRESH_SECRET=SECRET

sudo apt-get install -y libgbm-dev wget unzip fontconfig locales gconf-service libasound2 libatk1.0-0 libc6 libcairo2 libcups2 libdbus-1-3 libexpat1 libfontconfig1 libgcc1 libgconf-2-4 libgdk-pixbuf2.0-0 libglib2.0-0 libgtk-3-0 libnspr4 libpango-1.0-0 libpangocairo-1.0-0 libstdc++6 libx11-6 libx11-xcb1 libxcb1 libxcomposite1 libxcursor1 libxdamage1 libxext6 libxfixes3 libxi6 libxrandr2 libxrender1 libxss1 libxtst6 ca-certificates fonts-liberation libappindicator1 libnss3 lsb-release xdg-utils

wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

sudo apt install ./google-chrome-stable_current_amd64.deb

cd whaticket/backend

npm install

npm run build

npx sequelize db:migrate

npx sequelize db:seed:all

sudo npm install -g pm2

pm2 start dist/server.js --name whaticket-backend

pm2 startup ubuntu -u deploy

sudo env PATH=$PATH:/usr/bin pm2 startup ubuntu -u deploy --hp /home/deploy

cd ../frontend

npm install

nano .env

REACT_APP_BACKEND_URL = https://subdominioapi.dominio.com.br

npm run build

pm2 start server.js --name whaticket-frontend

pm2 save

pm2 list

sudo service apache2 stop

sudo apt install nginx

sudo rm /etc/nginx/sites-enabled/default

sudo nano /etc/nginx/sites-available/whaticket-frontend

server {

server_name subdominioapp.dominio.com.br;

location / {

proxy_pass http://127.0.0.1:3333;

proxy_http_version 1.1;

proxy_set_header Upgrade $http_upgrade;

proxy_set_header Connection 'upgrade';

proxy_set_header Host $host;

proxy_set_header X-Real-IP $remote_addr;

proxy_set_header X-Forwarded-Proto $scheme;

proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

proxy_cache_bypass $http_upgrade;

}

}

sudo cp /etc/nginx/sites-available/whaticket-frontend /etc/nginx/sites-available/whaticket-backend

sudo nano /etc/nginx/sites-available/whaticket-backend

server {

server_name subdominioapi.dominio.com.br;

location / {

proxy_pass http://127.0.0.1:8080;

proxy_http_version 1.1;

proxy_set_header Upgrade $http_upgrade;

proxy_set_header Connection 'upgrade';

proxy_set_header Host $host;

proxy_set_header X-Real-IP $remote_addr;

proxy_set_header X-Forwarded-Proto $scheme;

proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

proxy_cache_bypass $http_upgrade;

}

}

cd /etc/nginx/sites-available/

ls

sudo ln -s /etc/nginx/sites-available/whaticket-frontend /etc/nginx/sites-enabled

sudo ln -s /etc/nginx/sites-available/whaticket-backend /etc/nginx/sites-enabled

sudo nginx -t

sudo service nginx restart

sudo nano /etc/nginx/nginx.conf
HANDLE BIGGER UPLOADS

client_max_body_size 20M;

sudo nginx -t

sudo service nginx restart

sudo apt-get install snapd

sudo snap install notes

sudo snap install --classic certbot

sudo certbot --nginx

==============================================================

LOGAR COM O USUÁRIO DEPLOY

PM2 STOP IDBACKEND

PM2 START IDBACKEND
