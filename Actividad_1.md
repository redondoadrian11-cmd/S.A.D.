Creamos los grupos y los usuarios

dpkg -s acl

sudo groupadd operarios 
sudo groupadd ingenieria 
sudo groupadd supervisores 

sudo usermod -g operarios alberto
sudo usermod -g operarios oscar
sudo usermod -g operarios hugo

sudo usermod -g ingenieria nestor
sudo usermod -g ingenieria victor
sudo usermod -g ingenieria lucia

sudo usermod -g supervisores carlos
sudo usermod -g supervisores mary
sudo usermod -g supervisores john


Creamos los archivos

touch hmi_planta 

chmod +x hmi_planta 

mkdir -p planos 

touch planos/autoclave.dwg planos/caldera.dwg planos/destilacion.dwg 

Damos los permisos

sudo chown root:ingenieria hmi_planta 
sudo chmod 750 hmi_planta 
sudo chown root:ingenieria planos 
sudo chmod 750 planos 
sudo chmod 640 planos/*.dwg 

