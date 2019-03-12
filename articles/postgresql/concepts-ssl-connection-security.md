---
title: SSL-connectiviteit configureren in Azure Database for PostgreSQL
description: Instructies en informatie voor het configureren van Azure Database for PostgreSQL en gekoppelde toepassingen gebruiken SSL-verbindingen.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: 5a0fc99052b18dc1fa837147aa914a473d27d832
ms.sourcegitcommit: 1902adaa68c660bdaac46878ce2dec5473d29275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/11/2019
ms.locfileid: "57730024"
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

### <a name="install-a-cert-decoder-on-your-machine"></a>De decoder van een certificaat installeren op uw computer 
U kunt [OpenSSL](https://github.com/openssl/openssl) moet worden gedecodeerd het certificaatbestand dat nodig is voor uw toepassing veilig verbinding maken met uw database-server. Als u wilt weten hoe u OpenSSL installeren, Zie de [OpenSSL installatie-instructies](https://github.com/openssl/openssl/blob/master/INSTALL). 


### <a name="decode-your-certificate-file"></a>Uw certificaatbestand decoderen
Het gedownloade basis-CA-bestand is in een versleutelde indeling. Gebruikmaken van OpenSSL moet worden gedecodeerd het certificaatbestand. Om dit te doen, moet u deze OpenSSL-opdracht uitvoeren:

```
openssl x509 -inform DER -in BaltimoreCyberTrustRoot.crt -text -out root.crt
```

### <a name="connecting-to-azure-database-for-postgresql-with-ssl-certificate-authentication"></a>Verbinding maken met Azure Database for PostgreSQL met SSL-verificatie
Nu dat u hebt uw certificaat is gedecodeerd, u kunt nu verbinding maken met uw databaseserver veilig via SSL. Om toe te staan certificaatcontrole server, moet het certificaat in het bestand ~/.postgresql/root.crt in de basismap van de gebruiker worden geplaatst. (In Microsoft Windows het bestand is met de naam % APPDATA%\postgresql\root.crt.). 

#### <a name="connect-using-psql"></a>Verbinding maken met behulp van psql
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

## <a name="next-steps"></a>Volgende stappen
Bekijk verschillende connectiviteitsopties van toepassing na [verbindingsbibliotheken voor Azure Database for PostgreSQL](concepts-connection-libraries.md).
