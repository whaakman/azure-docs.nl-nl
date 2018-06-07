---
title: Beveiligingsoverwegingen in Azure Data Factory | Microsoft Docs
description: Beschrijft de infrastructuur van de basisprincipes van beveiliging die services voor gegevensverplaatsing in Azure Data Factory gebruiken om u te helpen beveiligen uw gegevens.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/26/2018
ms.author: abnarain
ms.openlocfilehash: 80cec0bc8136142f30ea7b957de819379b1bb139
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34619130"
---
#  <a name="security-considerations-for-data-movement-in-azure-data-factory"></a>Beveiligingsoverwegingen voor gegevensverplaatsing in Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-data-movement-security-considerations.md)
> * [Versie 2 - Preview](data-movement-security-considerations.md)

In dit artikel beschrijft de infrastructuur van de basisprincipes van beveiliging die services voor gegevensverplaatsing in Azure Data Factory gebruiken om u te helpen beveiligen uw gegevens. Data Factory-management-resources zijn gebouwd op Azure beveiligingsinfrastructuur en alle mogelijke veiligheidsmaatregelen, die worden aangeboden door Azure gebruiken.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service is algemeen beschikbaar (GA), Zie [Data movement beveiligingsoverwegingen voor Data Factory versie 1](v1/data-factory-data-movement-security-considerations.md).

In een Data Factory-oplossing maakt u een of meer gegevens[pijplijnen](concepts-pipelines-activities.md). Een pijplijn is een logische groep activiteiten die samen een taak uitvoeren. Deze pijplijnen zich bevinden in de regio waar de gegevensfactory is gemaakt. 

Hoewel Data Factory is alleen beschikbaar in de VS-Oost, VS-Oost 2 en West-Europa regio's (preview versie 2), data movement service is beschikbaar [globaal in meerdere regio's](concepts-integration-runtime.md#azure-ir). Als data movement service nog niet is geïmplementeerd voor deze regio, wordt de Data Factory-service zorgt ervoor dat gegevens laat een geografisch gebied of regio tenzij u expliciet de opdracht de service geven moet gebruiken een andere regio. 

Azure Data Factory slaat geen gegevens, met uitzondering van referenties van de gekoppelde service voor cloud-gegevensarchieven, die zijn gecodeerd met behulp van certificaten. Met Data Factory u gegevensgestuurde werkstromen maken voor het indelen van de verplaatsing van gegevens tussen [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats), en het verwerken van gegevens met behulp van [compute-services](compute-linked-services.md) in andere regio's of in een on-premises omgeving. U kunt ook bewaken en beheren van werkstromen met behulp van SDK's en Azure-Monitor.

Verplaatsing van gegevens met behulp van de Data Factory is gecertificeerd voor:
-   [HIPAA/HITECH](https://www.microsoft.com/en-us/trustcenter/Compliance/HIPAA) 
-   [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27001)  
-   [ISO/IEC 27018](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27018)
-   [CSA STER](https://www.microsoft.com/en-us/trustcenter/Compliance/CSA-STAR-Certification)

Als u geïnteresseerd in Azure compatibiliteit en hoe Azure de eigen infrastructuur beveiligt bent, gaat u naar de [Microsoft Trust Center](https://www.microsoft.com/trustcenter).

In dit artikel controleert u beveiligingsoverwegingen in de volgende twee data movement scenario's: 

- **Cloudscenario**: In dit scenario wordt de bron- en de doelserver zijn openbaar toegankelijk zijn via internet. Het gaat hierbij om beheerde cloudservices zoals Azure Storage, Azure SQL Data Warehouse, Azure SQL Database, Azure Data Lake Store, Amazon S3, Amazon-Redshift SaaS-services zoals Salesforce en web-protocollen, zoals FTP en OData. Een volledige lijst met ondersteunde gegevensbronnen in [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md#supported-data-stores-and-formats).
- **Hybride scenario**: In dit scenario wordt de bron- of uw doelserver achter een firewall of binnen een bedrijfsnetwerk lokaal is. Of het gegevensarchief bevindt zich in een particulier netwerk of een virtueel netwerk (meestal de bron) en is niet openbaar toegankelijk. Databaseservers die worden gehost op virtuele machines vallen ook onder dit scenario.

## <a name="cloud-scenarios"></a>Cloud-scenario 's

### <a name="securing-data-store-credentials"></a>Referenties opslaan, het beveiligen van gegevens

- **Versleutelde referenties opgeslagen in een beheerde Azure Data Factory-archief**. Data Factory kunt u uw referenties voor gegevensopslag beveiligen door ze te versleutelen met certificaten die worden beheerd door Microsoft. Deze certificaten worden geroteerd om de twee jaar (waaronder certificaatvernieuwing en de migratie van referenties). De versleutelde referenties zijn veilig opgeslagen in Azure storage-account worden beheerd door Azure Data Factory-beheerservices. Zie voor meer informatie over Azure Storage beveiliging [beveiligingsoverzicht van Azure Storage](../security/security-storage-overview.md).
- **Referenties opslaan in Azure Key Vault**. U kunt ook de referentie in de gegevensopslag opslaan [Azure Key Vault](https://azure.microsoft.com/services/key-vault/). Data Factory haalt de referentie tijdens het uitvoeren van een activiteit. Zie voor meer informatie [Store referentie in Azure Key Vault](store-credentials-in-key-vault.md).

### <a name="data-encryption-in-transit"></a>Gegevensversleuteling onderweg
Als de gegevensopslag van de cloud HTTPS of TLS ondersteunt, alle gegevens worden overgedragen tussen services voor gegevensverplaatsing in Data Factory en een gegevensarchief cloud zijn via een beveiligde HTTPS- of TLS-kanaal.

> [!NOTE]
> Alle verbindingen met Azure SQL Database en Azure SQL Data Warehouse versleuteling (SSL/TLS) wanneer gegevens verzonden naar en van de database. Als u een pijplijn maakt bent met behulp van de JSON, voeg de versleutelingseigenschap en stel deze in op **true** in de verbindingstekenreeks. U kunt gebruiken voor Azure Storage **HTTPS** in de verbindingstekenreeks.

### <a name="data-encryption-at-rest"></a>Versleuteling van inactieve gegevens
Sommige gegevens worden opgeslagen ondersteuning voor versleuteling van gegevens in rust. U wordt aangeraden het mechanisme voor de versleuteling van gegevens voor deze opgeslagen gegevens in te schakelen. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse
Transparent Data Encryption (TDE) in Azure SQL Data Warehouse biedt bescherming tegen de dreiging van schadelijke activiteiten door realtime versleuteling en ontsleuteling van uw gegevens in rust in te voeren. Dit gedrag is transparant voor de client. Zie voor meer informatie [beveiligen van een database in SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

#### <a name="azure-sql-database"></a>Azure SQL Database
Azure SQL Database biedt ook ondersteuning voor transparante gegevensversleuteling (TDE), waarmee worden beschermd tegen de dreiging van schadelijke activiteiten door het uitvoeren van realtime versleuteling en ontsleuteling van de gegevens, zonder dat wijzigingen in de toepassing. Dit gedrag is transparant voor de client. Zie voor meer informatie [transparante gegevensversleuteling voor SQL-Database en datawarehouse](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Azure Data Lake Store biedt ook versleuteling voor gegevens die zijn opgeslagen in het account. Wanneer dit is ingeschakeld, wordt Data Lake Store automatisch versleutelt gegevens voordat het behouden blijven en ontsleutelt voordat ophalen, waardoor het transparant voor de client die toegang heeft tot de gegevens. Zie voor meer informatie [beveiliging in Azure Data Lake Store](../data-lake-store/data-lake-store-security-overview.md). 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Azure Blob storage en Azure Table storage
Azure Blob storage en Azure Table storage ondersteuning voor opslag Service versleuteling (SSE), die automatisch worden uw gegevens worden versleuteld voordat behouden blijven van opslag en ontsleutelt voordat ophalen. Zie voor meer informatie [Azure Storage Service: versleuteling van gegevens in rust](../storage/common/storage-service-encryption.md).

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3 ondersteunt zowel client als server versleuteling van gegevens in rust. Zie voor meer informatie [gegevens beveiligen met behulp van versleuteling](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html).

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift ondersteunt cluster versleuteling voor gegevens in rust. Zie voor meer informatie [Amazon Redshift Databaseversleuteling](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html). 

#### <a name="salesforce"></a>SalesForce
SalesForce ondersteunt Shield Platform versleuteling waarmee versleuteling van alle bestanden, bijlagen en aangepaste velden. Zie voor meer informatie [inzicht in de Web Server OAuth-verificatie stromen](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm).  

## <a name="hybrid-scenarios"></a>Hybride scenario 's
Hybride scenario's moet de host zichzelf integratie runtime moet worden geïnstalleerd op een on-premises netwerk binnen een virtueel netwerk (Azure) of binnen een virtuele privécloud (Amazon). De runtime host zichzelf integratie moet toegang kunnen krijgen tot de lokale gegevens worden opgeslagen. Zie voor meer informatie over de integratie van host zichzelf runtime [maken en configureren zelf gehost integratie runtime](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime). 

![host zichzelf runtime-kanalen](media/data-movement-security-considerations/data-management-gateway-channels.png)

Het opdrachtkanaal kunt communicatie tussen services voor gegevensverplaatsing in Data Factory en host zichzelf integratie runtime. De communicatie bevat informatie met betrekking tot de activiteit. Het gegevenskanaal wordt gebruikt voor het overbrengen van gegevens tussen on-premises gegevensopslagexemplaren en cloud-gegevensarchieven.    

### <a name="on-premises-data-store-credentials"></a>On-premises gegevens opslaan van referenties
De referenties voor uw on-premises gegevensopslagexemplaren zijn altijd versleuteld en opgeslagen. Ze kunnen worden lokaal opgeslagen op de host zichzelf integratie runtime-machine of opgeslagen in Azure Data Factory beheerd storage (net zoals referenties voor gegevensopslag cloud). 

- **Opslaan van referenties lokaal**. Als u wilt coderen en referenties lokaal opslaan op de host zichzelf integratie-runtime, volg de stappen in [versleutelen referenties voor on-premises gegevensopslagexemplaren in Azure Data Factory](encrypt-credentials-self-hosted-integration-runtime.md). Alle connectors ondersteuning voor deze optie. De runtime host zichzelf integratie maakt gebruik van Windows [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx) voor het versleutelen van de gevoelige gegevens en de referentie-informatie. 

   Gebruik de **nieuw AzureRmDataFactoryV2LinkedServiceEncryptedCredential** cmdlet voor het versleutelen van de gekoppelde Servicereferenties en gevoelige gegevens in de gekoppelde service. U kunt de JSON geretourneerd (met de **EncryptedCredential** element in de verbindingsreeks) voor het maken van een gekoppelde service met behulp van de **Set AzureRmDataFactoryV2LinkedService** cmdlet.  

- **Opslaan in Azure Data Factory beheerd opslag**. Als u rechtstreeks de **Set AzureRmDataFactoryV2LinkedService** cmdlet uit met de verbinding tekenreeksen en inline in de JSON-referenties, de gekoppelde service is versleuteld en opgeslagen in Azure Data Factory beheerd opslag. De gevoelige gegevens worden nog steeds versleuteld certificaat en Microsoft beheert deze certificaten.



#### <a name="ports-used-when-encrypting-linked-service-on-self-hosted-integration-runtime"></a>Poorten die worden gebruikt bij het versleutelen van de gekoppelde service op host zichzelf integratie runtime
PowerShell gebruikt standaard poort 8050 op de machine met host zichzelf integratie-runtime voor veilige communicatie. Indien nodig, kan deze poort kan worden gewijzigd.  

![HTTPS-poort voor de gateway](media/data-movement-security-considerations/https-port-for-gateway.png)

 


### <a name="encryption-in-transit"></a>Codering in transit
Alle gegevensoverdrachten zijn via een beveiligd kanaal HTTPS- en TLS via TCP om man-in-the-middle-aanvallen te voorkomen dat tijdens de communicatie met Azure-services.

U kunt ook [IPSec VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md) of [Azure ExpressRoute](../expressroute/expressroute-introduction.md) voor verdere Beveilig het communicatiekanaal tussen uw on-premises netwerk en Azure.

Azure-netwerk is een logische representatie van uw netwerk in de cloud. U kunt een on-premises netwerk verbinden met het virtuele netwerk door het instellen van IPSec-VPN (site-naar-site) of ExpressRoute (privépeering).    

De volgende tabel geeft een overzicht van het netwerk en aanbevelingen voor de configuratie van host zichzelf integratie runtime op basis van verschillende combinaties van de bron en bestemming locaties voor hybride gegevensverplaatsing.

| Bron      | Doel                              | Netwerkconfiguratie                    | Installatie van integratieruntime                |
| ----------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| On-premises | Virtuele machines en cloudservices die zijn geïmplementeerd in virtuele netwerken | IPSec VPN (punt-naar-site of site-naar-site) | De runtime host zichzelf integratie kan worden on-premises geïnstalleerd of op een virtuele machine van Azure in een virtueel netwerk. |
| On-premises | Virtuele machines en cloudservices die zijn geïmplementeerd in virtuele netwerken | ExpressRoute (privépeering)           | De runtime host zichzelf integratie kan worden on-premises geïnstalleerd of op een virtuele machine van Azure in een virtueel netwerk. |
| On-premises | Azure-services waarvoor een openbaar eindpunt | ExpressRoute (openbare peering)            | De host zichzelf integratie runtime moet geïnstalleerde on-premises worden. |

De volgende afbeeldingen ziet het gebruik van zelf gehoste integratie-runtime voor het verplaatsen van gegevens tussen een on-premises database en de Azure-services met behulp van ExpressRoute- en IPSec-VPN (met Azure Virtual Network):

**ExpressRoute**

![ExpressRoute gebruiken met gateway](media/data-movement-security-considerations/express-route-for-gateway.png) 

**IPSec VPN**

![IPSec VPN met gateway](media/data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-whitelisting-ip-address-of-gateway"></a> Firewall-configuraties en whitelisting IP-adressen

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Firewallvereisten voor voor-premises/particulier netwerk  
In een onderneming is een bedrijfsfirewall wordt uitgevoerd op de centrale router van de organisatie. Windows Firewall wordt uitgevoerd als een daemon op de lokale computer waarop de host zichzelf integratie-runtime is geïnstalleerd. 

De volgende tabel bevat de uitgaande poort en het domein vereisten voor bedrijfsfirewalls:

| Domeinnamen                  | Uitgaande poorten | Beschrijving                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443            | Vereist door de runtime host zichzelf integratie verbinding maken met services voor gegevensverplaatsing in Data Factory. |
| `*.core.windows.net`          | 443            | Door de runtime host zichzelf integratie waarmee verbinding met de Azure storage-account wanneer u de [kopie gefaseerde](copy-activity-performance.md#staged-copy) functie. |
| `*.frontend.clouddatahub.net` | 443            | Vereist door de runtime host zichzelf integratie verbinding maken met de Data Factory-service. |
| `*.database.windows.net`      | 1433           | (Optioneel) Wanneer u van of naar Azure SQL Database- of Azure SQL Data Warehouse kopiëren vereist. De functie gefaseerde kopiëren gebruiken om gegevens te kopiëren naar Azure SQL Database- of Azure SQL Data Warehouse zonder poort 1433 openen. |
| `*.azuredatalakestore.net`<br>`login.microsoftonline.com/<tenant>/oauth2/token`    | 443            | (Optioneel) Vereist bij het kopiëren van of naar Azure Data Lake Store. |

> [!NOTE] 
> Mogelijk moet de poorten of whitelisting domeinen op het niveau van de firewall van het bedrijf zoals vereist door de respectieve gegevensbronnen beheren. Deze tabel alleen gebruik van Azure SQL Database, Azure SQL Data Warehouse en Azure Data Lake Store als voorbeelden.   

De volgende tabel bevat de vereisten van de binnenkomende poort voor Windows Firewall:

| Poorten voor inkomend verkeer | Beschrijving                              |
| ------------- | ---------------------------------------- |
| 8050 (TCP)    | Vereist voor de PowerShell-cmdlet voor versleuteling, zoals beschreven in [versleutelen referenties voor on-premises gegevensopslagexemplaren in Azure Data Factory](encrypt-credentials-self-hosted-integration-runtime.md), en door de toepassing van de manager referentie referenties voor on-premises gegevensopslagexemplaren veilig instellen op de host zichzelf integratie-runtime. |

![Vereisten voor de gateway-poort](media\data-movement-security-considerations/gateway-port-requirements.png) 

#### <a name="ip-configurations-and-whitelisting-in-data-stores"></a>IP-configuraties en whitelisting in gegevensarchieven
Sommige gegevens opslaat in de cloud vereist ook dat u geaccepteerde het IP-adres van de computer die toegang tot de store. Zorg ervoor dat het IP-adres van de runtime-machine host zichzelf integratie goedgekeurde lijst of in de firewall op de juiste wijze geconfigureerd.

De volgende cloud gegevensarchieven vereisen dat u geaccepteerde het IP-adres van de runtime-machine host zichzelf integratie. Sommige van deze gegevensarchieven standaard mogelijk niet door. 

- [Azure SQL Database](../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md)
- [Azure Data Lake Store](../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../cosmos-db/firewall-support.md)
- [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Kan de runtime host zichzelf integratie in andere data Factory worden gedeeld?**

We bieden deze functie nog geen ondersteuning. Er wordt gewerkt erop.

**Wat zijn de Poortvereisten voor de host zichzelf integratie runtime werken?**

De runtime host zichzelf integratie maakt op basis van HTTP-verbindingen voor toegang tot het internet. De uitgaande poorten 443 en 80 moeten worden geopend voor de host zichzelf integratie-runtime voor deze verbinding. Open de binnenkomende poort 8050 alleen op computerniveau (niet het niveau bedrijfsfirewall) voor referentie manager-toepassing. Als Azure SQL Database- of Azure SQL Data Warehouse wordt gebruikt als de bron- of de doelserver, moet u ook poort 1433 openen. Zie voor meer informatie de [Firewall-configuraties en whitelisting IP-adressen](#firewall-configurations-and-whitelisting-ip-address-of-gateway) sectie. 


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de prestaties van Azure Data Factory-Kopieeractiviteit [Kopieeractiviteit prestaties en prestatieafstemming handleiding](copy-activity-performance.md).

 
