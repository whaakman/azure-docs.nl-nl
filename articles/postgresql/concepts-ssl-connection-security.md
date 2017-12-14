---
title: SSL-verbindingen in de Azure-Database configureren voor PostgreSQL | Microsoft Docs
description: Instructies en informatie voor het configureren van Azure-Database voor PostgreSQL en de bijbehorende toepassingen SSL-verbindingen niet normaal kunnen gebruiken.
services: postgresql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: postgresql
ms.custom: 
ms.topic: article
ms.date: 11/01/2017
ms.openlocfilehash: d84a9fd45f2e6e44218ebd36d19c6a6c5f3438ce
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql"></a>SSL-verbindingen in de Azure-Database configureren voor PostgreSQL
Azure-Database voor PostgreSQL verkiest verbinding maken met uw clienttoepassingen naar de PostgreSQL-service met Secure Sockets Layer (SSL). Afdwingen van SSL-verbindingen tussen uw database-server en client-toepassingen beschermt tegen 'man-in het midden'-aanvallen door het versleutelen van de gegevensstroom tussen de server en uw toepassing.

De PostgreSQL-database-service is standaard geconfigureerd om SSL-verbinding vereist. U kunt desgewenst uitschakelen SSL verbinding maken met de database-service als de clienttoepassing biedt geen ondersteuning voor SSL-verbindingen vereisen. 

## <a name="enforcing-ssl-connections"></a>Afdwingen van SSL-verbindingen
Afdwinging van SSL-verbindingen is standaard ingeschakeld voor alle Azure-Database voor PostgreSQL-servers die zijn ingericht via de Azure portal en de CLI. 

Verbindingsreeksen die vooraf zijn gedefinieerd in de instellingen 'Verbindingsreeksen' onder uw server in de Azure portal omvatten ook de vereiste parameters voor algemene talen verbinding maken met uw database-server met behulp van SSL. De parameter SSL varieert op basis van de connector, bijvoorbeeld ' ssl = true ' of ' sslmode = vereisen ' of ' sslmode = vereist ' en andere variaties.

## <a name="configure-enforcement-of-ssl"></a>Afdwinging van SSL configureren
U kunt desgewenst uitschakelen afdwingen SSL-verbindingen. Microsoft Azure raadt aan om in te schakelen altijd **afdwingen SSL-verbinding** instellen voor een betere beveiliging.

### <a name="using-the-azure-portal"></a>Azure Portal gebruiken
Ga naar uw Azure-Database voor PostgreSQL-server en op **verbindingsbeveiliging**. Gebruik de wisselknop inschakelen of uitschakelen de **afdwingen SSL-verbinding** instelling. Klik vervolgens op **opslaan**. 

![Verbindingsbeveiliging - Disable SSL afdwingen](./media/concepts-ssl-connection-security/1-disable-ssl.png)

U kunt de instelling controleren door de **overzicht** om te zien de **SSL afdwingen status** indicator.

### <a name="using-azure-cli"></a>Azure CLI gebruiken
U kunt inschakelen of uitschakelen de **ssl-afdwinging** met behulp van de parameter `Enabled` of `Disabled` waarden respectievelijk in Azure CLI.

```azurecli
az postgres server update --resource-group myresourcegroup --name mypgserver-20170401 --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Zorg ervoor dat uw toepassing of framework ondersteunt SSL-verbindingen
Veel voorkomende toepassingsframeworks die PostgreSQL voor de databaseservices, zoals Drupal en Django gebruiken, doen SSL niet standaard ingeschakeld tijdens de installatie. Inschakelen van SSL-verbindingen moet worden uitgevoerd na de installatie of via de CLI-opdrachten die specifiek zijn voor de toepassing. Als uw server PostgreSQL is afdwingen van SSL-verbindingen en de bijbehorende toepassing niet juist is geconfigureerd, kan de toepassing geen verbinding maken met uw database-server. Raadpleeg de documentatie van uw toepassing voor informatie over het inschakelen van SSL-verbindingen.


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>Toepassingen waarvoor certificaatverificatie van het voor SSL-verbindingen
In sommige gevallen moet toepassingen een lokaal certificaatbestand gegenereerd op basis van een vertrouwde certificeringsinstantie (CA) certificaatbestand (.cer) veilig verbinding te maken. Zie de volgende stappen voor het verkrijgen van het cer-bestand, het decoderen van het certificaat en bindt dit aan uw toepassing.

### <a name="download-the-certificate-file-from-the-certificate-authority-ca"></a>Download het certificaat van de certificeringsinstantie (CA) 
Het certificaat nodig om te communiceren via SSL met uw Azure-Database voor PostgreSQL-server zich bevindt [hier](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt). Download het certificaatbestand lokaal op.

### <a name="download-and-install-openssl-on-your-machine"></a>Download en installeer OpenSSL op uw computer 
Voor het decoderen van het certificaatbestand die nodig zijn voor uw toepassing naar een veilige verbinding met uw database-server, moet u OpenSSL installeren op uw lokale computer.

#### <a name="for-linux-os-x-or-unix"></a>Voor Linux, Unix of OS X
De OpenSSL-bibliotheken vindt u in de broncode rechtstreeks vanuit de [OpenSSL Software Foundation](http://www.openssl.org). De volgende instructies leiden u door de stappen die nodig zijn voor het installeren van OpenSSL op uw PC Linux. In dit artikel worden de opdrachten om te werken op Ubuntu 12.04 bekende en hoger gebruikt.

Open een terminalsessie en OpenSSL downloaden.
```bash
wget http://www.openssl.org/source/openssl-1.1.0e.tar.gz
``` 
Pak de bestanden uit het gedownloade pakket.
```bash
tar -xvzf openssl-1.1.0e.tar.gz
```
Geef de map waar de bestanden zijn uitgepakt. Standaard moet als volgt.

```bash
cd openssl-1.1.0e
```
OpenSSL configureren door de volgende opdracht wordt uitgevoerd. Als u de bestanden in een map anders dan /usr/local/openssl wilt, zorg er dan voor dat het volgende wijzigen.

```bash
./config --prefix=/usr/local/openssl --openssldir=/usr/local/openssl
```
Nu OpenSSL correct is geconfigureerd, moet u eerst compileren als u wilt converteren van uw certificaat. Als u wilt compileren, voer de volgende opdracht:

```bash
make
```
Zodra de compilatie is voltooid, bent u klaar voor het installeren van OpenSSL als een uitvoerbaar bestand met de volgende opdracht:
```bash
make install
```
Om te bevestigen dat u hebt OpenSSL geïnstalleerd op uw systeem, moet u de volgende opdracht en controle om ervoor te zorgen dat u dezelfde uitvoer uitvoeren.

```bash
/usr/local/openssl/bin/openssl version
```
U ziet het volgende bericht als dit lukt.
```bash
OpenSSL 1.1.0e 7 Apr 2014
```

#### <a name="for-windows"></a>Voor Windows
OpenSSL installeren op een Windows-PC kan worden uitgevoerd in de volgende manieren:
1. **(Aanbevolen)**  Met behulp van de ingebouwde Bash voor Windows-functionaliteit in Windows 10 en hoger, OpenSSL wordt standaard geïnstalleerd. Vindt u instructies voor het inschakelen van Bash voor Windows-functionaliteit in Windows 10- [hier](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).
2. Via het downloaden van een toepassing Win32/64 is geleverd door de community. Terwijl de OpenSSL Software Foundation biedt geen of een specifieke Windows-installatieprogramma's tekent, bieden ze een lijst met beschikbare installatieprogramma's [hier](https://wiki.openssl.org/index.php/Binaries).

### <a name="decode-your-certificate-file"></a>Uw certificaatbestand decoderen
Het gedownloade bestand voor de basis-CA is in een versleutelde indeling. Gebruik OpenSSL decoderen van het certificaatbestand. Voer deze opdracht OpenSSL om dit te doen:

```dos
openssl x509 -inform DER -in BaltimoreCyberTrustRoot.crt -text -out root.crt
```

### <a name="connecting-to-azure-database-for-postgresql-with-ssl-certificate-authentication"></a>Verbinding maken met Azure-Database voor PostgreSQL met SSL-verificatie
Nu dat u hebt uw certificaat is gedecodeerd, kunt u nu verbinding met de databaseserver veilig via SSL. Zodat serververificatie certificaat moet het certificaat in het bestand ~/.postgresql/root.crt in de basismap van de gebruiker worden geplaatst. (Microsoft Windows het bestand de naam % APPDATA%\postgresql\root.crt.). Hieronder vindt u instructies voor het verbinden met Azure-Database voor PostgreSQL.

#### <a name="using-psql-command-line-utility"></a>Werken met het opdrachtregelhulpprogramma psql
Het volgende voorbeeld laat zien hoe om verbinding te maken met uw PostgreSQL-server met het opdrachtregelprogramma psql. Gebruik de `root.crt` -bestand gemaakt en de `sslmode=verify-ca` of `sslmode=verify-full` optie.

Met de opdrachtregelinterface PostgreSQL, voer de volgende opdracht:
```bash
psql "sslmode=verify-ca sslrootcert=root.crt host=mypgserver-20170401.postgres.database.azure.com dbname=postgres user=mylogin@mypgserver-20170401"
```
Als dit lukt, wordt de volgende uitvoer:
```bash
Password for user mylogin@mypgserver-20170401:
psql (9.6.2)
WARNING: Console code page (437) differs from Windows code page (1252)
     8-bit characters might not work correctly. See psql reference
     page "Notes for Windows users" for details.
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-SHA384, bits: 256, compression: off)
Type "help" for help.

postgres=>
```

#### <a name="using-pgadmin-gui-tool"></a>Met behulp van pgAdmin GUI-hulpprogramma
Configureren van pgAdmin 4 veilig verbinding te maken via SSL moet worden ingesteld de `SSL mode = Verify-CA` of `SSL mode = Verify-Full` als volgt:

![Schermopname van pgAdmin - verbinding - SSL-modus vereisen](./media/concepts-ssl-connection-security/2-pgadmin-ssl.png)

## <a name="next-steps"></a>Volgende stappen
Bekijk verschillende toepassing connectiviteitsopties na [verbindingsbibliotheken voor Azure-Database voor PostgreSQL](concepts-connection-libraries.md).
