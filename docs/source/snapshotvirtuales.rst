.. Bacula documentation master file, created by
   sphinx-quickstart on Wed Apr 24 11:45:26 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Configurando Snaphot de virtuales
======================================

Herramienta utilizada: VGHETTOBACKUP

Servidores que interfieren:
**drogo
**arya


Configurando script en arya
---------------------------


  .. code-block:: bash
    :linenos:

       #!/bin/bash                                                                                                                                                                                                                                  
       cd /srv/docker-persist/rclone-snapshots                                                                                                                                                                                                      
       cd $(dirname $0)                                                                                                                                                                                                                             
       NUMERO_PROCES=15                                                                                                                                                                                                                                                              
       PASSPHRASE="AltaEnElCieloPSIUnAgilaGuerrera132!"                                                                                                                                                                                                                              
       DATE_FILTRO=$(date '+%Y-%m-%d')                                                                                                                                                                                                                                               
       #DATE_FILTRO="2019-02-14"                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                                                     
       SERVIDOR=$1                                                                                                                                                                                                                                                                   
       VIRTUAL_DAR_BAJA=$2                                                                                                                                                                                                                                                           
                                                                                                                                                                                                                                                                                     
       function comprimir {                                                                                                                                                                                                                                                          
               #RECORRO LOS VIRTUALES POR SERVIDOR                                                                                                                                                                                                                                   
               for virtual in $(echo $VIRTUAL_DAR_BAJA); do                                                                                                                                                                                                                          
                       echo "$(date) - Comprimiendo $virtual del servidor $SERVIDOR..."                                                                                                                                                                                              
                       #ME FIJO SI LA CARPETA DEL VIRTUAL EXISTE                                                                                                                                                                                                                     
                       echo /drogo-storage/backups-virtuales/$virtual/                                                                                                                                                                                                                                                                                                                            
                       if [ -d /drogo-storage/backups-virtuales/$virtual/ ]; then                                                                                                                                                                                                                                                                                                                 
                               #VERIFICO DENTRO DE LA CARPETA DEL VIRTUAL SI SE ENCUENTRA EL ARCHIVO STATUS.OK PARA SABER SI EL CLONADO DIO OK                                                                                                                                                                                                                                                    
                               for comprimir in $(find /drogo-storage/backups-virtuales/$virtual -type f -name 'STATUS.ok' | grep $DATE_FILTRO | cut -d'/' -f5); do                                                                                                                                                                                                                               
                                       echo "$(date) - Comprimiendo $comprimir con $NUMERO_PROCES procesadores..."                                                                                                                                                                                                                                                                                
                                       tar -cv ./$virtual/$comprimir | pigz -c -p $NUMERO_PROCES > /drogo-storage/comprimidos-bajas/"$virtual"_"$DATE_FILTRO".tar.gz                                                                                                                                                                                                                              
                                       if [ 0 -eq 0 ] ; then                                                                                                                                                                                                                                                                                                                                      
                                       #ENCRIPTO                                                                                                                                                                                                                                                                                                                                                  
                                       echo "$(date) - Encriptando $comprimir "                                                                                                                                                                                                                                                                                                                   
                                       openssl enc -aes-256-cbc -in /drogo-storage/comprimidos-bajas/"$virtual"_"$DATE_FILTRO".tar.gz  -out /drogo-storage/comprimidos-bajas/"$virtual"_"$DATE_FILTRO".tar.gz.cript -k "$PASSPHRASE"                                                                                                                                                              
                                       echo "$(date) - Subiendo"                                                                                                                                                                                                                                                                                                                                  
                                       rclone --transfers=32 --checkers=1 sync -v /drogo-storage/comprimidos-bajas/"$virtual"_"$DATE_FILTRO".tar.gz.cript  backup:/snapshot_virtuales/$virtual"_"$DATE_FILTRO                                                                                                                                                                                     
                                       #VERIFICO QUE SE HAYA SUBIDO CORRECTAMENTE                                                                                                                                                                                                                                                                                                                 
                                               if [ $? -eq 0 ] ; then                                                                                                                                                                                                                                                                                                                             
                                               echo "$(date) - Terminado de subir $comprimir ..."                                                                                                                                                                                                                                                                                                 
                                               else                                                                                                                                                                                                                                                                                                                                               
                                               echo "$(date) - Error al subir $comprimir..."                                                                                                                                                                                                                                                                                                      
                                               fi                                                                                                                                                                                                                                                                                                                                                 
                                       fi                                                                                                                                                                                                                                                                                                                                                         
                               SIZE=$(du -x /drogo-storage/comprimidos-bajas/"$virtual"_"$DATE_FILTRO".tar.gz | awk '{print $1}')                                                                                                                                                                                                                                                                 
                               if [ $? -eq 0 ];then                                                                                                                                                                                                                                                                                                                                               
                                       echo "$(date) - Terminado de comprimir $comprimir ..."                                                                                                                                                                                                                                                                                                     
                                       ####ACA IRIA EL INSERT DEL MYSQL                                                                                                                                                                                                                                                                                                                           
                                       echo "INSERT INTO backups (servidor, dle, fecha, tipoBackup, ubicacion, tipoServidor, size) VALUES ('"$virtual"','"snapshot"','"$DATE_FILTRO"', '"full"','"drive-bajas"', '"snapshot"','"$SIZE"');"  | mysql -h 172.23.15.150 --port 3316 -ubackup-admin -pbackup-admin history_backup                                                                     
                                       rm -rf /drogo-storage/comprimidos-bajas/"$virtual"_"$DATE_FILTRO".tar.gz                                                                                                                                                                                                                                                                                   
                               else                                                                                                                                                                                                                                                                                                                                                               
                                       echo "$(date) - Error al comprimir $comprimir..."                                                                                                                                                                                                                                                                                                          
                               fi                                                                                                                                                                                                                                                                                                                                                                 
                               done                                                                                                                                                                                                                                                                                                                                                               
                       fi
               done
       }
       function backup {
               echo haciendo backup de $SERVIDOR a las $(date)
              ssh -o "StrictHostKeyChecking=no" $SERVIDOR.psi /vmfs/volumes/datastore-backup/ghettoVCB/ghettoVCB.sh -m $VIRTUAL_DAR_BAJA
               if [ $? -eq 0 ];then
               echo "$(date) - Copiado terminado - Empezando a comprimir"
               comprimir
               else
               echo "no se hizo correctamente"
               fi
       }
       if [ -z $SERVIDOR ] && [ -z $VIRTUAL_DAR_BAJA ]; then
       echo faltan variables
       dialog --title "Servidor" --inputbox "En que servidor se aloja el virtual:" 0 0 2>answer
       SERVIDOR=$(<answer)
       dialog --title "Virtual"  --inputbox "Como se llama el virtual:" 0 0 2>answer
       VIRTUAL_DAR_BAJA=$(<answer)
       dialog --title "Subir a Drive??" --backtitle "Sistema de Snapshots by teambackup psinox" --yesno "Subir el backup a drive?" 7 60
       response=$?
       case $response in
          0) SUBIR_A_DRIVE=0;;
          1) SUBIR_A_DRIVE=1;;
          255) echo "[ESC] key pressed.";;
       esac
       clear
       echo el servidor es ----- $name y el virtual $virtual
       fi
       
       echo $SERVIDOR $VIRTUAL_DAR_BAJA
       if [ ! -z $SERVIDOR ] && [ ! -z $VIRTUAL_DAR_BAJA ]; then
       echo "$(date) - Servidor recibido por parametro: $SERVIDOR"
       cd /drogo-storage/backups-virtuales
       mkdir -p /drogo-storage/comprimidos-bajas/
       backup
       fi
       exit 0

La linea que se encarga de realizar el snapshot es la siguiente

::

        ssh -o "StrictHostKeyChecking=no" $SERVIDOR.psi /vmfs/volumes/datastore-backup/ghettoVCB/ghettoVCB.sh -m $VIRTUAL_DAR_BAJA

En cada servidor FISICO que tenga instalado VSPHERE, se monta el directorio /vmfs/volumes/datastore-backup/ghettoVCB/ donde allí se ubica el archivo ghettoVCV.sh con la siguiente linea

:: 

        VM_BACKUP_VOLUME=/vmfs/volumes/datastore-backup/backups-virtuales

Una vez realizado el backup, se procede a comprimir y subir a drive con la herramienta rclone.
