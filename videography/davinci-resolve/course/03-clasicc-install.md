
# DAVINCI RESOLVE - INSTALL

## LINUX CLASIC (UBUNTU)

### Install necessary libraries

- Ejecuta el siguiente comando para instalar las versiones actuales de las librerías que solicita el instalador

    : sudo apt update && sudo apt install -y libapr1 libaprutil1 libasound2t64 libglib2.0-0


### Omitir la comprobación de paquetes 

- Dado que el instalador seguirá buscando específicamente libasound2 (y no encontrará libasound2t64), debes forzar la instalación usando la variable de entorno SKIP_PACKAGE_CHECK=1: 

    : chmod +x DaVinci_Resolve_20.3.1_Linux.run


## Ejecuta el instalador con el comando de omisión:

    : sudo SKIP_PACKAGE_CHECK=1 ./DaVinci_Resolve_20.3.1_Linux.run -i






