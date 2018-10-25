---
title: SSL-connectiviteit configureren in Azure Database for PostgreSQL
description: Instructies en informatie voor het configureren van Azure Database for PostgreSQL en gekoppelde toepassingen gebruiken SSL-verbindingen.
services: postgresql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: kfile
ms.service: postgresql
ms.custom: ''
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: cc8bd25b58cf898169a4d84154f7f3f81966bb92
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49985791"
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql"></a>SSL-connectiviteit configureren in Azure Database for PostgreSQL
Azure Database voor PostgreSQL verkiest het verbinden van uw clienttoepassingen met de PostgreSQL-service met Secure Sockets Layer (SSL). Het afdwingen van SSL-verbindingen tussen uw databaseserver en clienttoepassingen zorgt dat u bent beschermt tegen 'man in the middle'-aanvallen omdat de gegevensstroom tussen de server en uw toepassing wordt versleuteld.

De PostgreSQL-databaseservice is standaard geconfigureerd om te vereisen dat SSL-verbinding. Desgewenst kunt u uitschakelen dat SSL verbinding maken met uw databaseservice als uw clienttoepassing geen ondersteuning biedt voor SSL-connectiviteit is vereist. 

## <a name="enforcing-ssl-connections"></a>Afdwingen van SSL-verbindingen
Voor alle Azure-Database voor PostgreSQL-servers die zijn ingericht via de Azure portal en CLI is afdwingen van SSL-verbindingen standaard ingeschakeld. 

Verbindingsreeksen die vooraf zijn gedefinieerd in de instellingen 'Tekenreeksen voor databaseverbindingen' op uw server in Azure portal omvatten ook de vereiste parameters voor algemene talen verbinding maken met uw database-server met behulp van SSL. De SSL-parameter is afhankelijk van de connector, bijvoorbeeld ' ssl = true "of" sslmode = vereisen "of" sslmode = vereist ' en andere verschillen.

## <a name="configure-enforcement-of-ssl"></a>Afdwingen van SSL configureren
U kunt desgewenst uitschakelen uitvoerende SSL-connectiviteit. Microsoft Azure wordt aanbevolen om in te schakelen altijd **afdwingen van SSL-verbinding** instellen voor een betere beveiliging.

### <a name="using-the-azure-portal"></a>Azure Portal gebruiken
Ga naar uw Azure Database for PostgreSQL-server en klikt u op **verbindingsbeveiliging**. Gebruik de knop in-/ uitschakelen om in- of uitschakelen de **afdwingen van SSL-verbinding** instelling. Klik vervolgens op **opslaan**. 

![Verbindingsbeveiliging - Schakel SSL afdwingen](./media/concepts-ssl-connection-security/1-disable-ssl.png)

U kunt de instelling voor controleren via de **overzicht** pagina om te zien de **status voor afdwingen van SSL** indicator.

### <a name="using-azure-cli"></a>Azure CLI gebruiken
U kunt inschakelen of uitschakelen de **ssl afdwingen** met behulp van parameter `Enabled` of `Disabled` waarden respectievelijk in Azure CLI.

```azurecli
az postgres server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Zorg ervoor dat uw toepassing of framework ondersteunt SSL-verbindingen
Veel veelvoorkomende toepassingsframeworks die PostgreSQL voor de databaseservices, zoals Drupal en Django gebruiken, doen SSL niet standaard ingeschakeld tijdens de installatie. SSL-verbinding moet worden uitgevoerd na de installatie of via de CLI-opdrachten die specifiek zijn voor de toepassing. Als uw PostgreSQL-server is afdwingen van SSL-verbindingen en de bijbehorende toepassing niet correct is geconfigureerd, kan de toepassing geen verbinding maken met uw database-server. Raadpleeg de documentatie van uw toepassing voor informatie over het inschakelen van SSL-verbindingen.


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>Toepassingen waarvoor certificaatverificatie van het voor SSL-connectiviteit
In sommige gevallen vereisen toepassingen een lokale certificaatbestand gegenereerd op basis van een vertrouwde certificeringsinstantie (CA) certificaatbestand (.cer) om veilig verbinding te maken. Zie de volgende stappen voor het verkrijgen van het cer-bestand, het certificaat decoderen en koppel deze aan uw toepassing.

### <a name="download-the-certificate-file-from-the-certificate-authority-ca"></a>Download het certificaatbestand van de certificeringsinstantie (CA) 
Het certificaat nodig om te communiceren via SSL met uw Azure Database voor PostgreSQL-server zich bevindt [hier](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt). Download het certificaatbestand lokaal.

### <a name="download-and-install-openssl-on-your-machine"></a>Download en installeer de OpenSSL op uw computer 
Als u wilt decoderen het certificaatbestand dat nodig is voor uw toepassing veilig verbinding maken met uw databaseserver, moet u OpenSSL installeren op uw lokale computer.

#### <a name="for-linux-os-x-or-unix"></a>Voor Linux, Unix of OS X
De OpenSSL-bibliotheken zijn opgegeven in de broncode rechtstreeks vanuit de [OpenSSL Software Foundation](https://www.openssl.org). De volgende instructies leiden u door de benodigde stappen voor het OpenSSL installeren op uw Linux-computer. In dit artikel maakt gebruik van opdrachten om te werken op Ubuntu 12.04 bekende en hoger.

Open een terminalsessie en downloaden van OpenSSL.
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
OpenSSL configureren door de volgende opdracht wordt uitgevoerd. Als u wilt dat de bestanden in een map anders dan /usr/local/openssl, zorg ervoor dat de volgende zo nodig wijzigen.

```bash
./config --prefix=/usr/local/openssl --openssldir=/usr/local/openssl
```
Nu OpenSSL juist is geconfigureerd, moet u voor het compileren van het om te zetten van uw certificaat. Als u wilt compileren, voer de volgende opdracht:

```bash
make
```
Zodra de compilatie is voltooid, u kunt OpenSSL installeren als een uitvoerbaar bestand met de volgende opdracht:
```bash
make install
```
Om te bevestigen dat u OpenSSL hebt geïnstalleerd op uw systeem, moet u de volgende opdracht en controle om ervoor te zorgen dat u hetzelfde resultaat uitvoeren.

```bash
/usr/local/openssl/bin/openssl version
```
U ziet het volgende bericht weergegeven als dit lukt.
```bash
OpenSSL 1.1.0e 7 Apr 2014
```

#### <a name="for-windows"></a>Voor Windows
OpenSSL installeren op een Windows-PC kan worden uitgevoerd in de volgende manieren:
1. **(Aanbevolen)**  Met behulp van de ingebouwde Bash voor Windows-functionaliteit in Windows 10 en hoger, OpenSSL, wordt standaard geïnstalleerd. Instructies voor het inschakelen van Bash voor Windows-functionaliteit in Windows 10 vindt [hier](https://msdn.microsoft.com/commandline/wsl/install_guide).
2. Via het downloaden van een Win32/64-toepassing die is opgegeven door de community. Hoewel de OpenSSL Software Foundation niet opgeven of enkele aanbeveling voor een specifieke Windows-installatieprogramma's, beschikt u over een lijst met beschikbare installatieprogramma's [hier](https://wiki.openssl.org/index.php/Binaries).

### <a name="decode-your-certificate-file"></a>Uw certificaatbestand decoderen
Het gedownloade basis-CA-bestand is in een versleutelde indeling. Gebruikmaken van OpenSSL moet worden gedecodeerd het certificaatbestand. Om dit te doen, moet u deze OpenSSL-opdracht uitvoeren:

```dos
openssl x509 -inform DER -in BaltimoreCyberTrustRoot.crt -text -out root.crt
```

### <a name="connecting-to-azure-database-for-postgresql-with-ssl-certificate-authentication"></a>Verbinding maken met Azure Database for PostgreSQL met SSL-verificatie
Nu dat u hebt uw certificaat is gedecodeerd, u kunt nu verbinding maken met uw databaseserver veilig via SSL. Om toe te staan certificaatcontrole server, moet het certificaat in het bestand ~/.postgresql/root.crt in de basismap van de gebruiker worden geplaatst. (In Microsoft Windows het bestand is met de naam % APPDATA%\postgresql\root.crt.). Hieronder vindt u instructies voor het verbinden met Azure Database voor PostgreSQL.

#### <a name="using-psql-command-line-utility"></a>Met behulp van psql-opdrachtregel-hulpprogramma
Het volgende voorbeeld ziet hoe u verbinding maken met de PostgreSQL-server met behulp van het opdrachtregelprogramma psql. Gebruik de `root.crt` -bestand hebt gemaakt en de `sslmode=verify-ca` of `sslmode=verify-full` optie.

Met behulp van de PostgreSQL-opdrachtregelinterface, dan de volgende opdracht:
```bash
psql "sslmode=verify-ca sslrootcert=root.crt host=mydemoserver.postgres.database.azure.com dbname=postgres user=mylogin@mydemoserver"
```
Als dit lukt, wordt de volgende uitvoer:
```bash
Password for user mylogin@mydemoserver:
psql (9.6.2)
WARNING: Console code page (437) differs from Windows code page (1252)
     8-bit characters might not work correctly. See psql reference
     page "Notes for Windows users" for details.
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-SHA384, bits: 256, compression: off)
Type "help" for help.

postgres=>
```

#### <a name="using-pgadmin-gui-tool"></a>Met behulp van pgAdmin GUI-hulpprogramma
Configureren van pgAdmin 4 veilig verbinding maken via SSL vereist dat u om in te stellen de `SSL mode = Verify-CA` of `SSL mode = Verify-Full` als volgt:

![Schermafbeelding van pgAdmin - connection - SSL-modus vereist](./media/concepts-ssl-connection-security/2-pgadmin-ssl.png)

## <a name="next-steps"></a>Volgende stappen
Bekijk verschillende connectiviteitsopties van toepassing na [verbindingsbibliotheken voor Azure Database for PostgreSQL](concepts-connection-libraries.md).
