---
title: SSL-connectiviteit configureren in Azure Database voor PostgreSQL - één Server
description: Instructies en informatie voor het configureren van Azure-Database voor PostgreSQL - één Server en de bijbehorende toepassingen correct gebruik van SSL-verbindingen.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: 686adfb2998eff10ef4b9f378163b164ba970c56
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67461845"
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql---single-server"></a>SSL-connectiviteit configureren in Azure Database voor PostgreSQL - één Server
Azure Database voor PostgreSQL verkiest het verbinden van uw clienttoepassingen met de PostgreSQL-service met Secure Sockets Layer (SSL). Afdwingen van SSL-verbindingen tussen uw databaseserver en clienttoepassingen helpt te beschermen tegen 'man-in-the-middle'-aanvallen door het versleutelen van de gegevensstroom tussen de server en uw toepassing.

De PostgreSQL-databaseservice is standaard geconfigureerd om te vereisen dat SSL-verbinding. U kunt kiezen om uit te schakelen dat SSL is vereist als uw clienttoepassing geen ondersteuning biedt voor SSL-connectiviteit. 

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
Sommige toepassingsframeworks die PostgreSQL voor hun databaseservices gebruiken doen SSL niet standaard ingeschakeld tijdens de installatie. Als uw PostgreSQL-server SSL-verbindingen worden afgedwongen, maar de toepassing is niet geconfigureerd voor SSL, kan de toepassing geen verbinding maken met uw database-server. Raadpleeg de documentatie van uw toepassing voor informatie over het inschakelen van SSL-verbindingen.


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>Toepassingen waarvoor certificaatverificatie van het voor SSL-connectiviteit
In sommige gevallen vereisen toepassingen een lokale certificaatbestand gegenereerd op basis van een vertrouwde certificeringsinstantie (CA) certificaatbestand (.cer) om veilig verbinding te maken. Het certificaat verbinding maken met een Azure Database voor PostgreSQL-server bevindt zich in https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem. Download het certificaatbestand en sla deze op uw gewenste locatie. 

### <a name="connect-using-psql"></a>Verbinding maken met behulp van psql
Het volgende voorbeeld ziet hoe u verbinding maakt met uw PostgreSQL-server met behulp van het opdrachtregelprogramma psql. Gebruik de `sslmode=verify-full` instelling voor de verbindingsreeks om af te dwingen de verificatie van SSL-certificaat. Doorgeven van het lokale certificaatarchief pad naar de `sslrootcert` parameter.

Hieronder volgt een voorbeeld van de psql-verbindingsreeks:
```
psql "sslmode=verify-full sslrootcert=BaltimoreCyberTrustRoot.crt host=mydemoserver.postgres.database.azure.com dbname=postgres user=myusern@mydemoserver"
```

> [!TIP]
> Bevestig dat de waarde moet worden doorgegeven aan `sslrootcert` komt overeen met het pad voor het certificaat dat u hebt opgeslagen.


## <a name="next-steps"></a>Volgende stappen
Bekijk de verschillende connectiviteitsopties van toepassing in [verbindingsbibliotheken voor Azure Database for PostgreSQL](concepts-connection-libraries.md).
