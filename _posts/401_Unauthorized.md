---
title: Erro 401 Unauthorized mesmo com as credenciais corretas 
date: 2020-02-07 21:18:23 Z
categories:
- posts
---
Você já tentou se autenticar em algum serviço no OCI via CLI ou API e recebeu um erro **401  Unauthorized** mesmo estando com as credenciais corretas? 


Caso você utilize o oci-cli, fica fácil de identificar que o problema está na data/hora do computador que está tentando se autenticar que não pode ter mais de 5 minutos de diferença com o servidor:

![enter image description here](https://i.imgur.com/bfkOkZT.png)


Mas e quando a data está correta?

    [oracle@dbtanaka ~]$ date
    Sat Feb  8 21:41:50 UTC 2020
    [oracle@dbtanaka ~]$ java -jar oci_install.jar -host https://objectstorage.sa-saopaulo-1.oraclecloud.com \
    >   -pvtKeyFile /home/oracle/.oci/oci_api_key.pem \
    >   -pubFingerPrint XXXXXXXXXXX \
    >   -uOCID ocid1.user.XXXXXXXXX \
    >   -tOCID XXXXXXXXXXXXX\
    >   -walletDir $ORACLE_HOME/opc/wallet \
    >    -bucket bkp-rman \
    >   -configFile $ORACLE_HOME/dbs/opc${ORACLE_SID}.ora
    Oracle Database Cloud Backup Module Install Tool, build 19.3.0.0.0DBBKPCSBP_2019-10-16
      Error: Could not authenticate to Oracle Database Cloud Backup Module
    Exception in thread "main" java.lang.RuntimeException: java.io.IOException: testConnection: 401 Unauthorized.
    	at oracle.backup.opc.install.BmcConfig.testConnection(BmcConfig.java:380)
    	at oracle.backup.opc.install.BmcConfig.doBmcConfig(BmcConfig.java:227)
    	at oracle.backup.opc.install.BmcConfig.main(BmcConfig.java:219)
    Caused by: java.io.IOException: testConnection: 401 Unauthorized.
    	at oracle.backup.opc.install.BmcConfig.testConnection(BmcConfig.java:365)
    	... 2 more
    [oracle@dbtanaka ~]$ 


Mesmo que a hora esteja correta, o timezone também precisa estar configurado corretamente, então não se esqueçam de deixar sempre o pacote tzdata atualizado ou então definir manualmente as informações (o que não é muito recomendado):

    [oracle@dbtanaka ~]$ date
    Fri Feb  7 21:54:13 -03 2020
    [oracle@dbtanaka ~]$ java -jar oci_install.jar -host https://objectstorage.sa-saopaulo-1.oraclecloud.com   -pvtKeyFile /home/oracle/.oci/oci_api_key.pem   -pubFingerPrint xxxxxxxxx   -uOCID xxxxxxxx   -tOCID xxxxxxx   -walletDir $ORACLE_HOME/opc/wallet    -bucket bkp-rman   -configFile $ORACLE_HOME/dbs/opc${ORACLE_SID}.ora
    Oracle Database Cloud Backup Module Install Tool, build 19.3.0.0.0DBBKPCSBP_2019-10-16
    Oracle Database Cloud Backup Module credentials are valid.
    Backups would be sent to bucket bkp-rman.
    Oracle Database Cloud Backup Module wallet created in directory /u01/app/oracle/product/11.2.0/xe/opc/wallet.
    Oracle Database Cloud Backup Module initialization file /u01/app/oracle/product/11.2.0/xe/dbs/opcXE.ora created.
    Skipping library download because option -libDir is not specified.
    [oracle@dbtanaka ~]$ 




> Written with [StackEdit](https://stackedit.io/).

