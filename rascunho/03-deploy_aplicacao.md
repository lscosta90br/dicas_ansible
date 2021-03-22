# Configuracao django Producao

# bat - maquina windows
```
d:
cd \masters\lc\python
uckus
7z a -r -tzip web web\
pscp web.zip dj-voice-uc@192.168.x.x:/home/dj-voice-uc/app/download
```

# Passo para preparação de ambiente no linux
```
# criar usuario: rdo
 sudo adduser rdo
 sudo passwd
 sudo - rdo

# Criação de diretorios e descompactação da app
mkdir -p ~/app/download
mkdir -p ~/app/site/logs
mkdir -p ~/app/site/public/media
mkdir -p ~/app/site/public/static
unzip web.zip

# Ajustando os arquivos de desenvolvimento nos diretorios criados
cd ~/app/download/web
rm -rf static
mv contrib db.sqlite.3 manage.py projeto/ requiriments.txt ~/app
mv media/* ~/app/site/public/media

# Alterando conteudo de arquivos para ambiente de produção
sed -i 's/.3.46:8000/.4.116/g'  ~/app/projeto/core/templates/index.html
sed -i "s|os.path.join(BASE_DIR, 'static')|'/home/dj-voice-uc/app/site/public/static'|g" ~/app/projeto/settings.py
sed -i "s|os.path.join(BASE_DIR, 'media')|'/home/dj-voice-uc/app/site/public/media'|g" ~/app/projeto/settings.py
sed -i "s|DEBUG=True|DEBUG=False|g" ~/app/projeto/.env

# Ajustado o Apache para receber ambiente de produção 
# /etc/apache2/sites-available/01-voip-uc.conf
cd /etc/apache2/sites-available/
sudo a2ensite 01-voip-uc.conf
sudo apachectl configtest
sudo cp /etc/apache2/mods-available/wsgi.load /etc/apache2/mods-enabled/
sudo cp /etc/apache2/mods-available/wsgi.conf /etc/apache2/mods-enabled/
sudo apachectl configtest
sudo systemctl restart apache2

# Ajustando e criando ambiente de produção python
cd ~app/
python3 -m venv venv
source venv/bin/activate
pip install -r requiriments.txt
pip freeze

# Ajustando os direitos na aplicação
sudo chmod 755 ~/app
sudo chown :www-data ~/app/db.sqlite3
sudo chmod 664 ~/app/db.sqlite3
sudo chown :www-data ~/app/
sudo chown -R :www-data ~/app/site/public/media/
sudo chmod -R 775 ~/app/site/public/media

sudo chmod 775 app/
sudo systemctl restart apache2

# Criar arquivo .env
DEBUG=True
SECRET_KEY=nhkdn73&^3c2ou-i#3p5b6fxnev*wgu4myd2hrk%sy!f&xdc29
ALLOWED_HOSTS=127.0.0.1, .localhost, 192.168.x.x
#DATABASE_URL=postgres://USER:PASSWORD@HOST:PORT/NAME
#DEFAULT_FROM_EMAIL=
#EMAIL_BACKEND=django.core.mail.backends.smtp.EmailBackend
#EMAIL_HOST=
#EMAIL_PORT=
#EMAIL_USE_TLS=
#EMAIL_HOST_USER=
#EMAIL_HOST_PASSWORD=

# Listando Arquivos de configuracao apache
sudo vi /etc/apache2/sites-available/01-voip-uc.conf

Define home_path /home/dj-voice-uc
Define public_path /home/dj-voice-uc/app/site/public
Define app-dj_path ${home_path}/app
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
#       ServerName djangoproject.localhost
        DocumentRoot "/home/dj-voice-uc/app/site/public/html/"

        Alias /www /home/dj-voice-uc/app/site/public/html
        <Directory /home/dj-voice-uc/app/site/public/html/>
                Require all granted
        </Directory>

        ErrorLog ${app-dj_path}/site/logs/error.log
        CustomLog ${app-dj_path}/site/logs/access.log combined


        Alias /static /home/dj-voice-uc/app/site/public/static
        <Directory /home/dj-voice-uc/app/site/public/static>
                Require all granted
        </Directory>

        Alias /media /home/dj-voice-uc/app/site/public/media
        <Directory /home/dj-voice-uc/app/site/public/media>
                Require all granted
        </Directory>

        <Directory ${app-dj_path}/projeto>
                <Files wsgi.py>
                        Require all granted
                </Files>
        </Directory>

        WSGIScriptAlias / ${app-dj_path}/projeto/wsgi.py
        WSGIDaemonProcess django_app python-path=${app-dj_path} python-home=${app-dj_path}/venv
        WSGIProcessGroup django_app

</VirtualHost>
```






