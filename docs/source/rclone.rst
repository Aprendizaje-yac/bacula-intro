.. Bacula documentation master file, created by
   sphinx-quickstart on Wed Apr 24 11:45:26 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Configurando Subida a Drive mensuales
======================================

En este proceso, se utilizan 2 dockers.
* director
* restore-bacula



Configuración desde el director de bacula
------------------------------------------

Dentro de /etc/bacula el archivo restaurar_boostrap.sh es el encargado de hacer las restauraciones mensuales.
Utiliza la herramienta bextract la cual necesita la información brindada por los archivos boostrap ubicados en **/etc/bacula/backup-boostrap**

  .. code-block:: bash
    :linenos:

      #! /bin/bash
      dir_restaurado=/mnt/storage/restaurado
      cd /etc/bacula/backup-bootstrap
      DATE=`date +%Y-%m-%d`
      echo $DATE de hoy
      CONTADOR=1
      mkdir -p $dir_restaurado/$DATE
      for i in $( ls /etc/bacula/backup-bootstrap/ | grep -v dbarmy  | grep -v ffy-owncloud )
      do
              for j in $(ls ./$i | grep $DATE )
              do
              nombre=$(echo $i | sed 's/.bsr./_/g')
              mkdir -p $dir_restaurado/$DATE/$nombre"_$DATE"
              echo extrayendo $j
              /usr/sbin/bextract -p -b /etc/bacula/backup-bootstrap/$i/$j Backup-1 $dir_restaurado/$DATE/$nombre"_$DATE"/  2>&1 > /tmp/logs_bextract_$DATE.txt
              echo "ENVIANDO A CLIENTE DE DRIVE NUMERO $CONTADOR"
              ssh -o "StrictHostKeyChecking no" root@restore-bacula-2 /bin/bash /etc/bacula/crear_archivos.sh $DATE $nombre $CONTADOR
              if [ $CONTADOR -lt 5 ]; then
              let CONTADOR=CONTADOR+1
              else
              CONTADOR=1
              fi
      done
      done
      echo FIN

Una vez realizada la extraccion ejecuta

::

  ssh -o "StrictHostKeyChecking no" root@restore-bacula-2 /bin/bash /etc/bacula/crear_archivos.sh $DATE $nombre $CONTADOR

La cual llama al script crear_archivos.sh ubicado en el docker restore-bacula-2.
Este docker se encarga de **encriptar, comprimir, subir a drive**


Configuración desde el docker bacula-restore
--------------------------------------------

Dentro de /etc/bacula/crear_archivos.sh se realiza todo el procedimiento necesario para encriptar los archivos, dividirlos y subirlos a la nube

.. code-block:: bash
   :linenos:

      #!/bin/bash
      cd $(dirname $0)
      DATE_FILTRO=$1
      bandera=$(echo "select count(*) as CantidadTerminadas from bacula.Job J WHERE J.Jobstatus Like 'R' and J.Type LIKE 'R';" | mysql -h database -ubacula -pbacula| sed ':a;N;$!ba;s/\n/ /g' |  awk '{print $2}')
      NUMERO_PROCES=15
      PASSPHRASE="AltaEnElCieloPSIUnAgilaGuerrera132!"
      CLIENTE=$2
      CONTADOR=$3
      DIR_RAIZ=/mnt/storage
      DIR_A_SUBIR=/mnt/storage/subir_a_drive
      DIR_A_ELIMINAR=/mnt/storage/eliminar
      echo $DATE_FILTRO recibido por parametro
      function encriptar {
      echo "RECIBIMOS PARA ENCRIPTAR $1"
      echo $CLIENTE ------------------------------------------------
              CLIENTE=$2
              fileset=$3
              for i in $(ls $1 | grep -v cript);do 
              echo "$(date) - PASO 2 encriptando $1 $i"
              openssl enc -aes-256-cbc -in $1/$i -out $1/$i.cript -k "$PASSPHRASE"
              echo "$(date) - PASO 3 dividiendo $i"
              split -a 5 -d -b 2048m $1/$i.cript $1/$i.cript. 
              mkdir -p $DIR_A_ELIMINAR/$DATE_FILTRO/$2
              mv $1/$i $DIR_A_ELIMINAR/$DATE_FILTRO/$2/
              rm -rf $DIR_A_ELIMINAR/$DATE_FILTRO/$2/
              rm $1/$i.cript
              carpeta=$(echo $1 | cut -d "/" -f6)
              echo "$(date) - PASO 4 subiendo  $carpeta"
              if [ -z $CONTADOR ] ;
              then
              rclone mkdir backup-$CONTADOR:/subir_a_drive/$DATE_FILTRO/
              rclone --bwlimit "08:00,200M 15:00,500M 19:00,off Fri-15:00,off Sat-00:00,off Sun-00:00,off" --transfers=32 --checkers=10 sync -vP $1 backup:/subir_a_drive/$DATE_FILTRO/$carpeta
              else
              rclone mkdir backup:/subir_a_drive/$DATE_FILTRO/
              rclone --bwlimit "08:00,200M 15:00,500M 19:00,off Fri-15:00,off Sat-00:00,off Sun-00:00,off" --transfers=32 --checkers=10 sync -vP $1 backup-$CONTADOR:/subir_a_drive/$DATE_FILTRO/$carpeta
              fi
      #       SIZE=$(du -x "$DIR_A_ELIMINAR/$DATE_FILTRO/$2/$i" | awk '{print $1}')
              if [ $? -eq 0 ]; then
      #       echo "INSERT INTO backups (servidor, dle, fecha, tipoBackup, ubicacion, tipoServidor,size) VALUES ('"$CLIENTE"','"$fileset"','"$DATE_FILTRO"', 'full', 'drive', 'virtual','"$SIZE"');"  >> /tmp/mysql.log
      #        echo "INSERT INTO backups (servidor, dle, fecha, tipoBackup, ubicacion, tipoServidor,size) VALUES ('"$CLIENTE"','"$fileset"','"$DATE_FILTRO"', 'full', 'drive', 'virtual','"$SIZE"');"  | mysql -h database -ubackup-admin -pbackup-admin history_backup
              echo $(date) - $1/$i - subido correctamente >> /etc/bacula/directorios_subidos.log
              rm -rf $1/$i
              else
              echo $(date) - ERROR al subir - $1/$i  >> /etc/bacula/directorios_sin_subir.log
              fi
              done
      }
      function comprimir {
      #DATE=$(echo $(date '+%Y-%m-%d %H:%M:%S' --date='-30 day' | cut -d"-" -f2))
      ls /mnt/storage/restaurado/$DATE_FILTRO > ./carpetas_a_subir
      while read -r line
      do
          client="$line"
          mkdir -p $DIR_A_SUBIR/$DATE_FILTRO/$client 
          echo creando en $DIR_A_SUBIR
          cd  /mnt/storage/restaurado/$DATE_FILTRO/$client/
              for directorio in `ls`
              do
                      if [ -d ./$directorio ];
                      then
                      cd $DIR_RAIZ/restaurado/$DATE_FILTRO/$client/$directorio
                      echo "$(date) - PASO 1 - comprimiendo $client $directorio"
                      tar -c . -| pigz -c -p $NUMERO_PROCES > "$DIR_A_SUBIR/$DATE_FILTRO/$client/$client_$directorio.tar.gz"
                      encriptar $DIR_A_SUBIR/$DATE_FILTRO/$client/ $client $directorio
      #DOY VUELTA             rm -r $DIR_RAIZ/restaurado/$DATE_FILTRO/$client/$directorio
                      cd /mnt/storage/restaurado/$DATE_FILTRO/$client/
                      rm -r $DIR_RAIZ/restaurado/$DATE_FILTRO/$client/$directorio
                      else
                      echo "$(date) - ERROR - no se pudo escriptar $directorio"
                      fi
              done
      cd /etc/bacula
      done < "./carpetas_a_subir"
      }
      function comprobar {
      if [ $bandera = 0 ] ; then
      comprimir
      #eliminar
      else 
      echo "espero"
      fi
      }
      while [ $bandera != 0 ]
      do
              echo $(date) "no se puede hacer - todavia no termino la tarea - espero 5 minutos "
              sleep 10
              bandera=$(echo "select count(*) as CantidadTerminadas from bacula.Job J WHERE J.Jobstatus Like 'R' and J.Type LIKE 'R';" | mysql -h database -ubacula -pbacula| sed ':a;N;$!ba;s/\n/ /g' |  awk '{print $2}')
              comprobar
      done
      comprobar
      echo "bandera" $bandera
      mkdir -p $DIR_A_ELIMINAR


La siguiente linea es la que se encarga de realizad la subida, especificando limites de ancho de banda 

:: 

             rclone --bwlimit "08:00,200M 15:00,500M 19:00,off Fri-15:00,off Sat-00:00,off Sun-00:00,off" --transfers=32 --checkers=10 sync -vP $1 backup-$CONTADOR:/subir_a_drive/$DATE_FILTRO/$carpeta




Para subir a drive configuraremos la herramienta **rclone** ubicado en

:archivo: /home/.config/rclone/rclone.config

con la siguiente configuración

  .. code-block:: bash
    :linenos:

    [backup-2]
    type = drive
    scope = drive
    token = {"access_token":"ya29.Gl0zB15cYDIPYYeV8VEGAIKM6CDzIsUAbyvlHxAZLCX8pBaKCL5_wmeSWzq_QV91O_noHlI-YTa8NYBSFDcfHFSSWkVJb-SwUYxJH5dznskuOIuA5VHalX_K_pJBDAQ","token_type":"Bearer","refresh_token":"1/_V2EjMqczZ13DeOu8_0fNQ4iEAjpXzDzJPoRO9hGgwfooTJ9eLNyI-k-VJTQNs61","expiry":"2019-06-26T15:38:09.372534093Z"}

    [backup-3]
    type = drive
    scope = drive
    token = {"access_token":"ya29.GlszB5jWCNgPzrejlm87QQBw43mgS2x6BbCf1yz70j1x8F1oVPTGYdXj0ps9zPLN0BTBPKtREoffisk76XsdeUxJHGk4vyFGI0cBmbsXW2DtM954QjvWX42DraPC","token_type":"Bearer","refresh_token":"1/H68-xv2qbYeaz0IYNwMoBdgZ76wYi9skIuMEJxSn0Gw","expiry":"2019-06-26T15:39:37.532405624Z"}

    [backup-4]
    type = drive
    scope = drive
    token = {"access_token":"ya29.GlszB3qyCozHivdUqsttld9zQxVyOmVk5xoq9r_waVxAPflkbP-YWz438GfrvZjFMEvgWTgyD_MAbwKYJApx6QahRdWVSc0oGgKYsw7EyPcfWwfe-b9hWlej95Xk","token_type":"Bearer","refresh_token":"1/qSROF8DHdmxHvH_CNhaJjHFaGr1X9P0J_DzygGRQH7w","expiry":"2019-06-26T10:13:36.539507811-03:00"}

    [backup-1]
    type = drive
    scope = drive
    token = {"access_token":"ya29.GlszB1-6O0TfWTt5kw-f5lN_XyGJBbdslGyPljx5DP0pxdEf6pAtFV-dT68V_attN8lMuJMVEQIFmrQwk4n74c0_x6-Bxdt6el2ng-ZumaU7G1ttNYRzEF2D4Bf4","token_type":"Bearer","refresh_token":"1/9i5NgMTu5YGKM3q1lohNClqFEGOonAWsgGhHQZq1XMI","expiry":"2019-06-26T14:36:04.28724739Z"}

    [backup]
    type = drive
    scope = drive
    token = {"access_token":"ya29.GlszB9vq-XYHcYkZDy5KEVh-zZZIZf2L5axW3hxzc6RzOwfwNEC2TxXbnbk0nmxyHCldjRPzuEQuDOHnXe2Ii0Vf1R2yAUPMOOsvX2-hsOEp7SntKozY6WxT9tYe","token_type":"Bearer","refresh_token":"1/9i5NgMTu5YGKM3q1lohNClqFEGOonAWsgGhHQZq1XMI","expiry":"2019-06-26T14:52:07.055561072Z"}

  

Esta configuración nos permite realizar subidas a drive con distintos usuarios para no correr el riesgo de exceder la quota diaria para subir archivos a la nube
