#Repositorio de Alojamiento


sudo mkdir -p ~/ckan/lib//default/
sudo chmod 777 ~/ckan/lib/default/

sudo chown `whoami` ~/ckan/lib/default/

cd ~/ckan/lib/default/


#se descarga la aplicacion desde el git clone https://github.com/jumargo73/harvest_docker.git

 #se descarga la aplicacion desde el git
https://github.com/jumargo73/harvest_docker.git


 #se Despliega la Aplicacion 

 docker compose build
 docker compose up -d

 #comandos importantes
 docker compose down  Baja la Aplicacion sin eliminar Volumenes Asociados
 docker compose down -v  Adicional elimina los Volumenes   
 docker restart <contenedor> si se desea reiniciar solo un contenedor

 #configuraciones Adicionales
 #migraciones de Ckan 

Se abre el contenedor de la aplicacion

docker exec -u root -it  ckan-padre-ckan-1 bash

ckan db upgrade ejecuta las migraciones de ckan creacion de las tablas basicas
python  harvest_migrated.py crea tablas de harvest

con quit se sale del contenedor


Validar que todo quedo ok
docker exec -it ckan-padre-db-1 bash  psql -U postgres
comandos
\l despliega las BD creadas
\dt Despliega las Tablas creadas


Validar Logs de los Contenedores  
docker logs -f  <contenedor>

Archivos importantes

.env variables de entorno
.docker-compose.yml archivo donde se configura los contenedores de la aplicacion
dockerfile archivo de configuracion del contenedor.

comandos importantes en harvest
ckan -c /srv/app/ckan.ini harvester jobs  
ckan -c /srv/app/ckan.ini harvester sources



