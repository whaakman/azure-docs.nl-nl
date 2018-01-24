---
title: Beveiligingsoverwegingen in Azure Data Factory | Microsoft Docs
description: Beschrijft de infrastructuur van de basisprincipes van beveiliging die services voor gegevensverplaatsing in Azure Data Factory gebruiken om uw gegevens te beveiligen.
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: abnarain
ms.openlocfilehash: 8bd5ae2aac23b18aeb3ef44692f448b50b7e3d44
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="azure-data-factory---security-considerations-for-data-movement"></a>Azure Data Factory - beveiligingsoverwegingen voor gegevensverplaatsing
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-data-movement-security-considerations.md)
> * [Versie 2 - Preview](data-movement-security-considerations.md)

In dit artikel beschrijft de infrastructuur van de basisprincipes van beveiliging die services voor gegevensverplaatsing in Azure Data Factory gebruiken om uw gegevens te beveiligen. Azure Data Factory-management-resources zijn gebouwd op Azure beveiligingsinfrastructuur en alle mogelijke veiligheidsmaatregelen, die worden aangeboden door Azure gebruiken.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [data movement beveiligingsoverwegingen voor Data Factory versie 1](v1/data-factory-data-movement-security-considerations.md).

In een Data Factory-oplossing maakt u een of meer gegevens[pijplijnen](concepts-pipelines-activities.md). Een pijplijn is een logische groep activiteiten die samen een taak uitvoeren. Deze pijplijnen zich bevinden in de regio waar de gegevensfactory is gemaakt. 

Hoewel Data Factory is alleen beschikbaar in **VS-Oost**, **VS-Oost 2**, en **West-Europa** regio's (versie 2 preview), data movement service is beschikbaar [globaal in meerdere regio's](concepts-integration-runtime.md#azure-ir). Als u data movement service nog niet is geïmplementeerd voor deze regio, de Data Factory-service zorgt ervoor dat gegevens een geografisch gebied laat / regio tenzij u expliciet de opdracht de service geven moet gebruiken een andere regio. 

Azure Data Factory zelf slaat geen gegevens, met uitzondering van referenties van de gekoppelde service voor cloud-gegevensarchieven, die zijn gecodeerd met behulp van certificaten. U kunt er gegevensgestuurde werkstromen mee maken om de verplaatsing van gegevens te beheren tussen [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats) en om er gegevens mee te verwerken middels [Compute Services](compute-linked-services.md) in andere regio's of in een on-premises omgeving. Ook kunt u bewaken en beheren van werkstromen met behulp van SDK's en Azure-Monitor.

Verplaatsing van gegevens met behulp van Azure Data Factory is **gecertificeerd** voor:
-   [HIPAA/HITECH](https://www.microsoft.com/en-us/trustcenter/Compliance/HIPAA) 
-   [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27001)  
-   [ISO/IEC 27018](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27018)
-   [CSA STER](https://www.microsoft.com/en-us/trustcenter/Compliance/CSA-STAR-Certification)

Als u geïnteresseerd in Azure compatibiliteit en hoe Azure de eigen infrastructuur beveiligt bent, gaat u naar de [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx). 

In dit artikel controleert u beveiligingsoverwegingen in de volgende twee data movement scenario's: 

- **Cloudscenario**-In dit scenario wordt de bron- en doelserver zijn openbaar toegankelijk zijn via internet. Het gaat hierbij om services zoals Azure Storage, Azure SQL Data Warehouse, Azure SQL Database, Azure Data Lake Store, Amazon S3, Amazon-Redshift SaaS-services zoals Salesforce en web-protocollen, zoals FTP en OData beheerde cloudopslag. U vindt een volledige lijst met ondersteunde gegevensbronnen [hier](copy-activity-overview.md#supported-data-stores-and-formats).
- **Hybride scenario**: In dit scenario wordt de bron- of doelserver zich achter een firewall of binnen een on-premises zakelijke netwerk of de gegevens store is in een particulier netwerk / virtuele netwerk (meestal de bron) en is niet openbaar toegankelijk. Databaseservers die worden gehost op virtuele machines vallen ook onder dit scenario.

## <a name="cloud-scenarios"></a>Cloud-scenario 's
###<a name="securing-data-store-credentials"></a>Referenties opslaan, het beveiligen van gegevens
- Versleutelde referenties opgeslagen in Azure Data Factory beheerde archief.

   Azure Data Factory beveiligt uw referenties voor gegevensopslag door **versleutelen** ze met behulp van **certificaten die worden beheerd door Microsoft**. Deze certificaten worden gedraaid elke **twee jaar** (waaronder migratie van referenties en vernieuwen van certificaat). Deze versleutelde referenties zijn veilig opgeslagen in een **Azure Storage beheerd door Azure Data Factory-beheerservices**. Raadpleeg voor meer informatie over Azure Storage beveiliging [beveiligingsoverzicht van Azure Storage](../security/security-storage-overview.md).
- Referenties opslaan in Azure Key Vault 

   U kunt nu kiezen voor het opslaan van de referentie van de gegevensopslag in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), laat u Azure Data Factory om op te halen tijdens het uitvoeren van een activiteit. Zie voor meer informatie [Store referentie in Azure Key Vault](store-credentials-in-key-vault.md).

   > [!NOTE]
   > Op dit moment alleen [Dynamics connector](connector-dynamics-crm-office-365.md) deze functie wordt ondersteund. 

### <a name="data-encryption-in-transit"></a>Gegevensversleuteling onderweg
Als de gegevensopslag van de cloud HTTPS of TLS ondersteunt, alle gegevens worden overgedragen tussen services voor gegevensverplaatsing in Data Factory en een gegevensarchief cloud zijn via een beveiligde HTTPS- of TLS-kanaal.

> [!NOTE]
> Alle verbindingen met **Azure SQL Database** en **Azure SQL Data Warehouse** altijd versleuteling vereisen (SSL/TLS) wanneer gegevens verzonden naar en van de database. Tijdens het ontwerpen van een pijplijn met een JSON toevoegen de **versleuteling** eigenschap en wordt ingesteld op **true** in de **verbindingsreeks**. Voor **Azure Storage**, kunt u **HTTPS** in de verbindingstekenreeks.

### <a name="data-encryption-at-rest"></a>Versleuteling van inactieve gegevens
Sommige gegevens worden opgeslagen ondersteuning voor versleuteling van gegevens in rust. Het is raadzaam om de gegevens versleuteling mechanisme voor deze opgeslagen gegevens in te schakelen. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse
Transparent Data Encryption (TDE) in Azure SQL Data Warehouse kunt beveiligen tegen de dreiging van schadelijke activiteiten door realtime versleuteling en ontsleuteling van uw gegevens in rust in te voeren. Dit gedrag is transparant voor de client. Zie voor meer informatie [beveiligen van een database in SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

#### <a name="azure-sql-database"></a>Azure SQL Database
Azure SQL Database biedt ook ondersteuning voor transparante gegevensversleuteling (TDE), die helpt bij het beveiligen tegen de dreiging van schadelijke activiteiten door realtime versleuteling en ontsleuteling van de gegevens in te voeren zonder dat wijzigingen in de toepassing. Dit gedrag is transparant voor de client. Zie voor meer informatie [Transparent Data Encryption met Azure SQL Database](/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database). 

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Azure Data Lake store biedt ook versleuteling voor gegevens die zijn opgeslagen in het account. Wanneer dit is ingeschakeld, wordt Data Lake store automatisch versleutelt gegevens voordat het behouden blijven en ontsleutelt voordat ophalen, waardoor het transparant voor de client toegang tot de gegevens. Zie voor meer informatie [beveiliging in Azure Data Lake Store](../data-lake-store/data-lake-store-security-overview.md). 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Azure Blob Storage en Azure-tabelopslag
Azure Blob Storage en Azure Table storage ondersteunt opslag Service versleuteling (SSE), die automatisch worden uw gegevens worden versleuteld voordat behouden blijven van opslag en ontsleutelt voordat voor het ophalen. Zie voor meer informatie [Azure Storage Service: versleuteling van gegevens in rust](../storage/common/storage-service-encryption.md).

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3 ondersteunt zowel client als server versleuteling van gegevens in rust. Zie voor meer informatie [gegevens beveiligen met behulp van versleuteling](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html). Data Factory biedt momenteel geen ondersteuning voor Amazon S3 binnen een virtuele privécloud (VPC).

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift ondersteunt cluster versleuteling voor gegevens in rust. Zie voor meer informatie [Amazon Redshift Databaseversleuteling](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html). Data Factory biedt momenteel geen ondersteuning voor Amazon Redshift binnen een VPC. 

#### <a name="salesforce"></a>SalesForce
SalesForce ondersteunt Shield Platform versleuteling waarmee versleuteling van bestanden, bijlagen en aangepaste velden. Zie voor meer informatie [inzicht in de Web Server OAuth-verificatie stromen](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm).  

## <a name="hybrid-scenarios-using-self-hosted-integration-runtime"></a>Hybride scenario's (met behulp van de host zichzelf integratie runtime)
Hybride scenario's moet de host zichzelf integratie runtime worden geïnstalleerd in een on-premises netwerk of in een virtueel netwerk (Azure) of een virtuele privécloud (Amazon). De runtime host zichzelf integratie moet toegang kunnen krijgen tot de lokale gegevens worden opgeslagen. Zie voor meer informatie over de runtime host zichzelf integratie [zelf gehost integratie runtime](create-self-hosted-integration-runtime.md). 

![host zichzelf runtime-kanalen](media/data-movement-security-considerations/data-management-gateway-channels.png)

De **opdrachtkanaal** communicatie tussen services voor gegevensverplaatsing in Data Factory en host zichzelf integratie runtime. De communicatie bevat informatie met betrekking tot de activiteit. Het gegevenskanaal wordt gebruikt voor het overbrengen van gegevens tussen on-premises gegevensopslagexemplaren en cloud-gegevensarchieven.    

### <a name="on-premises-data-store-credentials"></a>On-premises gegevens opslaan van referenties
De referenties voor uw on-premises gegevensopslagexemplaren zijn altijd versleuteld en opgeslagen. Dit kan ofwel lokaal opgeslagen op de host zichzelf integratie runtime-machine of in Azure Data Factory beheerde opslag (net zoals referenties voor gegevensopslag cloud). 

1. U kunt **referenties lokaal opgeslagen**. Als u wilt coderen en referenties lokaal opslaan op de host zichzelf integratie-runtime, volg de stappen in [coderen van referenties op de host zichzelf integratie runtime](encrypt-credentials-self-hosted-integration-runtime.md). Alle connectors ondersteuning voor deze optie. De runtime host zichzelf integratie maakt gebruik van Windows [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx) de gevoelige gegevens versleutelen / referentie-informatie. 

   Gebruik **nieuw AzureRmDataFactoryV2LinkedServiceEncryptedCredential** cmdlet de referenties van de gekoppelde Service niet versleutelen / versleutelen van gevoelige gegevens in de gekoppelde Service. U kunt de JSON geretourneerd (met **EncryptedCredential** -element in de **connectionString**) voor het maken van een gekoppelde Service door **Set-AzureRmDataFactoryV2LinkedSevrice**cmdlet.  

2. Als u geen gebruik **nieuw AzureRmDataFactoryV2LinkedServiceEncryptedCredential** cmdlet als beschreven in de bovenstaande stap en gebruik in plaats daarvan rechtstreeks **Set AzureRmDataFactoryV2LinkedSevrice** cmdlet uit met de verbinding tekenreeksen / inline in de JSON-referenties en vervolgens de gekoppelde service **versleuteld en opgeslagen op Azure Data Factory beheerd opslag**. Certificaat nog steeds de gevoelige gegevens worden versleuteld en deze certificaten worden beheerd door Microsoft.



#### <a name="ports-used-during-encrypting-linked-service-on-self-hosted-integration-runtime"></a>Poorten die worden gebruikt bij het versleutelen van de gekoppelde Service op host zichzelf integratie runtime
PowerShell maakt standaard gebruik van de poort **8050** op de machine met host zichzelf integratie-runtime voor veilige communicatie. Indien nodig, kan deze poort kan worden gewijzigd.  

![HTTPS-poort voor de gateway](media/data-movement-security-considerations/https-port-for-gateway.png)

 


### <a name="encryption-in-transit"></a>Codering in transit
Alle gegevensoverdrachten zijn via een beveiligd kanaal **HTTPS** en **TLS via TCP** om man-in-the-middle-aanvallen te voorkomen dat tijdens de communicatie met Azure-services.

U kunt ook [IPSec VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md) of [Express Route](../expressroute/expressroute-introduction.md) voor verdere Beveilig het communicatiekanaal tussen uw on-premises netwerk en Azure.

Virtueel netwerk is een logische representatie van uw netwerk in de cloud. U kunt een on-premises netwerk verbinden met uw Azure-netwerk (VNet) door het instellen van IPSec-VPN (site-naar-site) of Express Route (Privépeering)     

De volgende tabel geeft een overzicht van het netwerk en aanbevelingen voor de configuratie van host zichzelf integratie runtime op basis van verschillende combinaties van de bron en bestemming locaties voor hybride gegevensverplaatsing.

| Bron      | Doel                              | Netwerkconfiguratie                    | Integratie runtime-instellingen                |
| ----------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| On-premises | Virtuele machines en cloudservices die zijn geïmplementeerd in virtuele netwerken | IPSec VPN (punt-naar-site of site-naar-site) | De runtime host zichzelf integratie kan worden geïnstalleerd van on-premises of in een Azure virtuele machine u (VM) in VNet |
| On-premises | Virtuele machines en cloudservices die zijn geïmplementeerd in virtuele netwerken | ExpressRoute (Privépeering)           | De runtime host zichzelf integratie kan worden on-premises geïnstalleerd of op een virtuele machine van Azure in VNet |
| On-premises | Azure-services waarvoor een openbaar eindpunt | ExpressRoute (openbare Peering)            | De host zichzelf integratie runtime moet geïnstalleerde on-premises worden |

De volgende afbeeldingen ziet het gebruik van zelf gehoste integratie-runtime voor het verplaatsen van gegevens tussen een on-premises database en de Azure-services met behulp van Express route- en IPSec-VPN (met een virtueel netwerk):

**Express Route:**

![Gebruik Express Route met gateway](media/data-movement-security-considerations/express-route-for-gateway.png) 

**IPSec VPN:**

![IPSec VPN met gateway](media/data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-whitelisting-ip-address-self-hosted-integration-runtime"></a>Firewall-configuraties en whitelisting IP-adres (runtime host zichzelf integratie)

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Firewallvereisten voor voor-premises/particulier netwerk  
In een onderneming, een **bedrijfsfirewall** wordt uitgevoerd op de centrale router van de organisatie. En, **Windows firewall** wordt uitgevoerd als een daemon op de lokale computer waarop de host zichzelf integratie-runtime is geïnstalleerd. 

De volgende tabel bevat **uitgaande poort** en vereisten voor het domein voor de **bedrijfsfirewall**.

| Domeinnamen                  | Uitgaande poorten | Beschrijving                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443, 80        | Vereist door de runtime host zichzelf integratie verbinding maken met services voor gegevensverplaatsing in Data Factory |
| `*.core.windows.net`          | 443            | Gebruikt door de runtime host zichzelf integratie verbinding maken met Azure Storage-Account wanneer u de [kopie gefaseerde](copy-activity-performance.md#staged-copy) functie. |
| `*.frontend.clouddatahub.net` | 443            | Vereist door de runtime host zichzelf integratie verbinding maken met de Azure Data Factory-service. |
| `*.database.windows.net`      | 1433           | (Optioneel) als bestemming voor de Azure SQL-Database nodig / Azure SQL Data Warehouse. De functie gefaseerde kopiëren gebruiken om gegevens te kopiëren naar Azure SQL Database/Azure SQL Data Warehouse zonder de poort 1433 openen. |
| `*.azuredatalakestore.net`    | 443            | (Optioneel) nodig hebt bij het doel Azure Data Lake store is |

> [!NOTE] 
> Wellicht hebt u voor het beheren van poorten / whitelisting-domeinen in de bedrijfsfirewall niveau zoals vereist door de respectieve gegevensbronnen. Deze tabel alleen maakt gebruik van Azure SQL Database, Azure SQL Data Warehouse, Azure Data Lake Store als voorbeelden.   

De volgende tabel bevat **binnenkomende poort** vereisten voor de **Windows firewall**.

| Poorten voor inkomend verkeer | Beschrijving                              |
| ------------- | ---------------------------------------- |
| 8050 (TCP)    | Vereist voor de versleuteling van de PowerShell-cmdlet, zoals beschreven in [coderen van referenties op de host zichzelf integratie runtime](encrypt-credentials-self-hosted-integration-runtime.md)/ referenties zijn veilig referenties voor on-premises gegevensopslagexemplaren instellen op de host zichzelf manager-toepassing integratie-runtime. |

![Vereisten voor de gateway-poort](media\data-movement-security-considerations/gateway-port-requirements.png) 

#### <a name="ip-configurationswhitelisting-in-data-store"></a>IP-configuraties/whitelisting in gegevensopslag
Sommige gegevens opslaat in de cloud, ook verplichten whitelisting van IP-adres van de computer die toegang hebben tot deze. Zorg ervoor dat het IP-adres van de runtime-machine host zichzelf integratie goedgekeurde lijst is / correct zijn geconfigureerd in de firewall.

De volgende cloud gegevensarchieven vereisen whitelisting van IP-adres van de runtime-machine host zichzelf integratie. Sommige van deze gegevensarchieven standaard vereist geen whitelisting van het IP-adres. 

- [Azure SQL Database](../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md)
- [Azure Data Lake Store](../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../cosmos-db/firewall-support.md)
- [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Vraag:** kan de runtime host zichzelf integratie worden gedeeld door andere data Factory?
**Antwoord:** wij bieden geen ondersteuning voor deze functie nog. Er wordt gewerkt erop.

**Vraag:** wat zijn de Poortvereisten voor de host zichzelf integratie runtime werken?
**Antwoord:** de runtime host zichzelf integratie HTTP-gebaseerde verbindingen maakt met internet te openen. De **uitgaande poorten 443 en 80** voor host zichzelf integratie-runtime voor deze verbinding moet worden geopend. Open **binnenkomende poort 8050** alleen op het niveau van de computer (niet op het niveau van de firewall van het bedrijf) voor de toepassing Referentiebeheer. Als Azure SQL Database- of Azure SQL Data Warehouse wordt gebruikt als bron / bestemming, dan hebt u nodig hebt om te openen **1433** ook poort. Zie voor meer informatie [Firewall-configuraties en whitelisting IP-adressen](#firewall-configurations-and-whitelisting-ip-address-of gateway) sectie. 


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de prestaties van de kopieeractiviteit [activiteit prestaties en prestatieafstemming handleiding kopiëren](copy-activity-performance.md).

 
