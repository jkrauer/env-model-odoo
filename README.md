# Utilidad
Este repositorio cumple con la función de plantear una estructura básica de desarrollo de Odoo, usando de base lo estudiado en el libro [Odoo Development Cookbook](https://www.packtpub.com/big-data-and-business-intelligence/odoo-development-cookbook) de Holger Brunn, Alexandre Fayolle y Daniel Reis.
Se asume también que el equipo está preparado para el desarrollo de Odoo. Esto significa que ya tiene PostgreSQL, NodeJS y sus dependencias

# Escenario
Todo el trabajo está hecho bajo la idea de trabajar bajo Ubuntu. Se usarán las siguientes carpetas:
**src:** contiene el código fuente creado clonando el repositorio de odoo
**env:** contiene el environment de python
**local:** contiene módulos que se implementarán fuera del código fuente
**filestore:** espacio donde se guardarán los archivos subidos a Odoo

# Qué hacer
En caso de no tener el equipo configurado, hay que preparar primero el Sistema para el desarrollo:
### Ubuntu
##### Instalamos las dependencias del sistema
`sudo apt install build-essential libjpeg-dev zlib1g-dev libssl-dev libffi-dev python-dev libldap2-dev libsasl2-dev` `libxml2-dev libxslt1-dev terminator mc wget curl git python-pip`

### PostgreSQL 9.6
##### Agregar repositorio
`sudo echo "deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main" >> /etc/apt/sources.list.d/pgdg.list`
`wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -`
`sudo apt update`

##### Instalar PostgreSQL, dev y pgadmin
`sudo apt install postgresql-9.6 postgresql-server-dev-9.6 pgadmin3`
`sudo -u postgres createuser $(whoami) --createdb`
`sudo -u postgres createdb $(whoami)`

### NodeJS y dependencias (para el front)

`curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -`
`sudo apt install -y nodejs node-less`
`sudo npm install -g less less-plugin-clean-css`
`sudo ln -s /usr/local/bin/lessc /usr/bin/lessc && sudo ln -s /usr/bin/nodejs /usr/bin/node`

### Ajustar carpetas de librerías

`sudo ln -s /usr/lib/x86_64-linux-gnu/libjpeg.so /usr/lib && sudo ln -s /usr/lib/x86_64-linux-gnu/libfreetype.so /usr/lib &&` `sudo ln -s /usr/lib/x86_64-linux-gnu/libz.so /usr/lib`

### Instalar wkhtmltopdf (Librería para exportar a PDF)
`wget https://downloads.wkhtmltopdf.org/0.12/0.12.1/wkhtmltox-0.12.1_linux-wheezy-amd64.deb && sudo dpkg -i wkhtml*`

#### 1- Clonar esto!
`git clone https://github.com/jkrauer/env-model-odoo.git $nombredelproyecto`
`cd $nombredelproyecto`

#### 2- Clonar Odoo
>En vez de "10.0" puede ser cualquier otra versión

`git clone https://github.com/odoo/odoo --depth 1 --single-branch -b 10.0 src`
#### 3- Crear environment
>Desde Odoo 11 se usa Python 3

`virtualenv -p python2 env`

#### 4- Iniciar environment
`source env/bin/activate`
>(se desactiva escribiendo `deactivate` en la consola)

#### 5- Instalar dependencias de Odoo
`pip install -r src/requirements.txt`

#### 6- Crear carpetas adicionales
`mkdir local filestore`

#### 7- Crear un módulo rápido. Esto ayuda a que el siguiente paso identifique a la carpeta donde estarán los módulos adicionales.
`./src/odoo-bin scaffold mymodule local`

#### 8- Primera Ejecución para configurar el archivo
`./src/odoo-bin -c odoo.conf --data-dir="filestore" --addons-path="src/odoo/addons,src/addons,local" --save --stop-after-init`

#### 9- Ejecución de ahora en más.
`./src/odoo-bin -c odoo.conf`

Hasta donde vi, PyCharm tiene la mejor compatibilidad en el desarrollo de Odoo, pero se puede usar cualquier IDE compatible con Python o incluso un bloc de notas.
