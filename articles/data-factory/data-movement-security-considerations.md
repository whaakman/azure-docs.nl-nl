---
title: Beveiligingsoverwegingen voor in Azure Data Factory | Microsoft Docs
description: Beschrijving basisvereisten voor beveiliging-infrastructuur die services voor gegevensverplaatsing in Azure Data Factory worden gebruikt om u te helpen beveiligen van uw gegevens.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: abnarain
ms.openlocfilehash: a996703f3719c2be90851241c1fe23c89f24e606
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57447945"
---
#  <a name="security-considerations-for-data-movement-in-azure-data-factory"></a>Beveiligingsoverwegingen voor het verplaatsen van gegevens in Azure Data Factory
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
>
> * [Versie 1:](v1/data-factory-data-movement-security-considerations.md)
> * [Huidige versie](data-movement-security-considerations.md)

Dit artikel beschrijft basisvereisten voor beveiliging-infrastructuur die services voor gegevensverplaatsing in Azure Data Factory worden gebruikt om u te helpen beveiligen van uw gegevens. Data Factory-management-resources zijn gebouwd op Azure-beveiliging-infrastructuur en gebruik van alle mogelijke beveiligingsmaatregelen die worden aangeboden door Azure.

In een Data Factory-oplossing maakt u een of meer gegevens[pijplijnen](concepts-pipelines-activities.md). Een pijplijn is een logische groep activiteiten die samen een taak uitvoeren. Deze pipelines zich bevinden in de regio waar de data factory is gemaakt. 

Hoewel Data Factory alleen beschikbaar in enkele regio's is, is het data movement service [beschikbaar wereldwijd](concepts-integration-runtime.md#integration-runtime-location) om de gegevensnaleving, efficiëntie en verminderde netwerk uitgaand verkeer kosten. 

Azure Data Factory worden niet opgeslagen voor elk soort gegevens, met uitzondering van gekoppelde Servicereferenties voor cloudgegevensarchieven die zijn versleuteld met behulp van certificaten. Met Data Factory kunt u gegevensgestuurde werkstromen maken om te verplaatsen van gegevens tussen indelen [ondersteunde gegevensarchieven](copy-activity-overview.md#supported-data-stores-and-formats), en de verwerking van gegevens met behulp van [rekenservices](compute-linked-services.md) in andere regio's of in een on-premises omgeving. U kunt ook controleren en beheren van werkstromen met behulp van SDK's en Azure Monitor.

Data Factory is gecertificeerd voor:
| **[CSA STAR-certificering](https://www.microsoft.com/trustcenter/compliance/csa-star-certification)** |
| :----------------------------------------------------------- |
| **[ISO 20000-1:2011](https://www.microsoft.com/trustcenter/Compliance/ISO-20000-1)** |
| **[ISO 22301:2012](https://www.microsoft.com/trustcenter/compliance/iso-22301)** |
| **[ISO 27001:2013](https://www.microsoft.com/trustcenter/compliance/iso-iec-27001)** |
| **[ISO 27017:2015](https://www.microsoft.com/trustcenter/compliance/iso-iec-27017)** |
| **[ISO 27018:2014](https://www.microsoft.com/trustcenter/compliance/iso-iec-27018)** |
| **[ISO 9001:2015](https://www.microsoft.com/trustcenter/compliance/iso-9001)** |
| **[SOC 1, 2, 3](https://www.microsoft.com/trustcenter/compliance/soc)** |
| **[HIPAA BAA](https://www.microsoft.com/trustcenter/compliance/hipaa)** |

Als u wel interesse in Azure, naleving en hoe Azure beveiligt met een eigen infrastructuur, gaat u naar de [Microsoft Trust Center](https://microsoft.com/en-us/trustcenter/default.aspx). Voor de meest recente lijst met alle controle op naleving van de Azure-aanbiedingen - http://aka.ms/AzureCompliance.

In dit artikel bekijken we beveiligingsoverwegingen in de volgende twee verkeer scenario's: 

- **Cloudscenario**: In dit scenario zijn zowel de bron en bestemming voor de openbaar toegankelijk is via het internet. Het gaat hierbij om beheerde cloud storage-services zoals Azure Storage, Azure SQL Data Warehouse, Azure SQL Database, Azure Data Lake Store, Amazon S3, Amazon Redshift, SaaS-services zoals Salesforce en web-protocollen, zoals FTP- en OData. Een volledige lijst met ondersteunde gegevensbronnen in [ondersteunde gegevensarchieven en indelingen](copy-activity-overview.md#supported-data-stores-and-formats).
- **Hybride scenario**: In dit scenario is de bron of bestemming voor de achter een firewall of binnen een bedrijfsnetwerk on-premises. Of het gegevensarchief is in een particulier netwerk of een virtueel netwerk (meestal de bron) en is niet openbaar toegankelijk is. Databaseservers die worden gehost op virtuele machines kunnen ook worden onderverdeeld in dit scenario.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="cloud-scenarios"></a>Cloud-scenario 's

### <a name="securing-data-store-credentials"></a>Referenties opslaan, het beveiligen van gegevens

- **Versleutelde referenties in een beheerde Azure Data Factory-store Store**. Data Factory kunt uw referenties van de gegevensopslag beveiligen door ze te versleutelen met certificaten die worden beheerd door Microsoft. Deze certificaten zijn geroteerd om de twee jaar (waaronder certificaatvernieuwing en de migratie van referenties). De versleutelde referenties zijn veilig opgeslagen in een Azure storage account dat wordt beheerd door Azure Data Factory-management-services. Zie voor meer informatie over de beveiliging van Azure Storage [beveiligingsoverzicht voor Azure Storage](../security/security-storage-overview.md).
- **Referenties in Azure Key Vault Store**. U kunt ook het gegevensarchief referentie in opslaan [Azure Key Vault](https://azure.microsoft.com/services/key-vault/). Data Factory worden opgehaald van de referentie op die tijdens het uitvoeren van een activiteit. Zie voor meer informatie, [Store referentie in Azure Key Vault](store-credentials-in-key-vault.md).

### <a name="data-encryption-in-transit"></a>Gegevensversleuteling onderweg
Als het gegevensarchief cloud biedt ondersteuning voor HTTPS- of TLS, alle gegevens worden overgedragen tussen services voor gegevensverplaatsing in Data Factory en een gegevensarchief voor de cloud worden via een beveiligd kanaal HTTPS of TLS.

> [!NOTE]
> Alle verbindingen met Azure SQL Database en Azure SQL Data Warehouse vereisen versleuteling (SSL/TLS) terwijl de gegevens is onderweg naar en van de database. Wanneer u bent een pijplijn maakt met behulp van JSON, voeg de versleutelingseigenschap en stel deze in op **waar** in de verbindingsreeks. Voor Azure Storage, kunt u **HTTPS** in de verbindingsreeks.

> [!NOTE]
> Om in te schakelen versleuteling tijdens overdracht tijdens het verplaatsen van gegevens van Oracle Volg een van de onderstaande opties:
> 1. In de Oracle-server, gaat u naar Oracle Advanced Security (OAS) en configureer de versleutelingsinstellingen, die ondersteuning biedt versleuteling van Triple-DES (3DES) en Advanced Encryption Standard (AES), verwijzen [hier](https://docs.oracle.com/cd/E11882_01/network.112/e40393/asointro.htm#i1008759) voor meer informatie. ADF onderhandelt automatisch over de versleutelingsmethode voor het gebruik van de versie die u in OAS configureert bij het maken van verbinding met Oracle.
> 2. In ADF, voegt u toe EncryptionMethod = 1 in de verbindingsreeks (in de gekoppelde Service). Hiermee worden SSL/TLS gebruikt als de versleutelingsmethode. Om dit te gebruiken, moet u niet-SSL-coderingsinstellingen in OAS aan de serverzijde Oracle om te voorkomen dat versleuteling-conflict uitschakelen.

> [!NOTE]
> TLS-versie die wordt gebruikt is 1.2.

### <a name="data-encryption-at-rest"></a>Versleuteling van inactieve gegevens
Sommige gegevensarchieven ondersteuning voor versleuteling van data-at-rest. U wordt aangeraden dat u het mechanisme voor de versleuteling van gegevens voor deze gegevensarchieven inschakelt. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse
Transparante gegevensversleuteling (TDE) in Azure SQL Data Warehouse helpt te beschermen tegen de dreiging van schadelijke activiteiten door het uitvoeren van real-time versleuteling en ontsleuteling van uw gegevens in rust. Dit gedrag is transparant voor de client. Zie voor meer informatie, [beveiligen van een database in SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

#### <a name="azure-sql-database"></a>Azure SQL Database
Azure SQL Database biedt ook ondersteuning voor transparante gegevensversleuteling (TDE), waarmee worden beschermd tegen de dreiging van schadelijke activiteiten door het uitvoeren van real-time versleuteling en ontsleuteling van gegevens, zonder wijzigingen aan de toepassing. Dit gedrag is transparant voor de client. Zie voor meer informatie, [transparante gegevensversleuteling voor SQL-Database en Data Warehouse](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Azure Data Lake Store biedt ook versleuteling voor gegevens die zijn opgeslagen in het account. Wanneer dit is ingeschakeld, Data Lake Store versleutelt automatisch gegevens voor het persistent maken en ontsleutelt voordat ophalen, waardoor het transparant voor de client die toegang heeft tot de gegevens. Zie voor meer informatie, [beveiliging in Azure Data Lake Store](../data-lake-store/data-lake-store-security-overview.md). 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Azure Blob storage en Azure Table storage
Ondersteuning voor Storage Service Encryption (SSE), die automatisch worden uw gegevens worden versleuteld voordat opgeslagen en ontsleutelt voordat het ophalen van Azure Blob storage en Azure-tabelopslag. Zie voor meer informatie, [Azure Storage-Serviceversleuteling voor Data-at-Rest](../storage/common/storage-service-encryption.md).

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3 biedt ondersteuning voor de client- en versleuteling van data-at-rest. Zie voor meer informatie, [gegevens beveiligen met behulp van versleuteling](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html).

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift ondersteunt cluster versleuteling voor data-at-rest. Zie voor meer informatie, [Amazon Redshift-Databaseversleuteling](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html). 

#### <a name="salesforce"></a>SalesForce
SalesForce ondersteunt Shield Platform versleuteling waarmee het coderen van alle bestanden, bijlagen en aangepaste velden. Zie voor meer informatie, [inzicht in de Web Server OAuth-Verificatiestroom](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm).  

## <a name="hybrid-scenarios"></a>Hybride scenario 's
Hybride scenario's vereisen zelf-hostende integratieruntime moet worden geïnstalleerd op een on-premises netwerk, binnen een virtueel netwerk (Azure) of binnen een virtuele privécloud (Amazon). De zelf-hostende integratieruntime moet toegang hebben tot de lokale gegevensarchieven. Zie voor meer informatie over de zelf-hostende integratieruntime [maken en configureren van zelf-hostende integratieruntime](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime). 

![zelf-hostende integration runtime-kanalen](media/data-movement-security-considerations/data-management-gateway-channels.png)

Het opdrachtkanaal maakt communicatie tussen services voor gegevensverplaatsing in Data Factory en zelf-hostende integratieruntime. De communicatie bevat informatie met betrekking tot de activiteit. Het gegevenskanaal wordt gebruikt voor het overbrengen van gegevens tussen on-premises gegevensopslagexemplaren en cloudgegevensopslag.    

### <a name="on-premises-data-store-credentials"></a>On-premises gegevens opslaan van referenties
De referenties voor uw on-premises gegevensarchieven worden altijd versleuteld en opgeslagen. Ze kunnen worden lokaal opgeslagen op de zelf-hostende integration runtime-machine of opgeslagen in Azure Data Factory beheerde opslag (net als de referenties van de gegevensopslag in de cloud). 

- **Referenties lokaal Store**. Als u wilt coderen en lokaal opslaan van referenties in de zelf-hostende integratieruntime, volgt u de stappen in [versleutelen referenties voor on-premises gegevensarchieven in Azure Data Factory](encrypt-credentials-self-hosted-integration-runtime.md). Deze optie worden alle connectors ondersteund. De zelf-hostende integratieruntime maakt gebruik van Windows [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx) voor het versleutelen van de gevoelige gegevens en referentie-informatie. 

   Gebruik de **New-AzDataFactoryV2LinkedServiceEncryptedCredential** cmdlet voor het versleutelen van gekoppelde Servicereferenties en gevoelige gegevens in de gekoppelde service. Vervolgens kunt u de JSON geretourneerd (met de **EncryptedCredential** element in de connection string) te maken van een gekoppelde service met behulp van de **Set AzDataFactoryV2LinkedService** cmdlet.  

- **Store in Azure Data Factory beheerde opslag**. Als u rechtstreeks de **Set AzDataFactoryV2LinkedService** cmdlet met de verbinding verbindingsreeksen en referenties inline in de JSON, de gekoppelde service is versleuteld en opgeslagen in Azure Data Factory beheerde opslag. De gevoelige gegevens is nog steeds versleuteld met certificaten en Microsoft beheert deze certificaten.



#### <a name="ports-used-when-encrypting-linked-service-on-self-hosted-integration-runtime"></a>Poorten die worden gebruikt bij het versleutelen van gekoppelde service op een zelf-hostende integratieruntime
PowerShell gebruikt standaard poort 8050 op de machine met de zelf-hostende integratieruntime voor veilige communicatie. Indien nodig, kan deze poort kan worden gewijzigd.  

![HTTPS-poort voor de gateway](media/data-movement-security-considerations/https-port-for-gateway.png)

 


### <a name="encryption-in-transit"></a>Versleuteling tijdens overdracht
Alle gegevensoverdrachten worden via een beveiligd kanaal HTTPS- en TLS via TCP om man-in-the-middle-aanvallen te voorkomen dat tijdens de communicatie met Azure-services.

U kunt ook [IPSec VPN](../vpn-gateway/vpn-gateway-about-vpn-devices.md) of [Azure ExpressRoute](../expressroute/expressroute-introduction.md) naar verdere Beveilig het communicatiekanaal tussen uw on-premises netwerk en Azure.

Azure-netwerk is een logische weergave van uw netwerk in de cloud. U kunt een on-premises netwerk verbinden met het virtuele netwerk door het instellen van IPSec-VPN (site-naar-site) of ExpressRoute (persoonlijke peering).    

De volgende tabel geeft een overzicht van het netwerk en aanbevelingen voor de configuratie van zelf-hostende integration runtime op basis van verschillende combinaties van bron- en locaties voor hybride verplaatsing van gegevens.

| Bron      | Doel                              | Netwerkconfiguratie                    | Installatie van integratieruntime                |
| ----------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| On-premises | Virtuele machines en cloudservices die zijn geïmplementeerd in virtuele netwerken | IPSec-VPN (punt-naar-site of site-naar-site) | De zelf-hostende integratieruntime kan worden geïnstalleerd van on-premises of op een Azure-machine in een virtueel netwerk. |
| On-premises | Virtuele machines en cloudservices die zijn geïmplementeerd in virtuele netwerken | ExpressRoute (persoonlijke peering)           | De zelf-hostende integratieruntime kan worden geïnstalleerd van on-premises of op een Azure-machine in een virtueel netwerk. |
| On-premises | Azure-services waarvoor een openbaar eindpunt | ExpressRoute (openbare peering)            | De zelf-hostende integratieruntime moet geïnstalleerd on-premises worden. |

De volgende afbeeldingen ziet het gebruik van zelf-hostende integratieruntime voor het verplaatsen van gegevens tussen een on-premises database en Azure-services met behulp van ExpressRoute- en IPSec-VPN (met Azure Virtual Network):

**ExpressRoute**

![Gebruik van ExpressRoute met gateway](media/data-movement-security-considerations/express-route-for-gateway.png) 

**IPSec VPN**

![IPSec-VPN met gateway](media/data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-whitelisting-ip-address-of-gateway"></a> Firewallconfiguraties en IP-adressen in een whitelist opnemen

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Firewall-vereisten voor on-premises/particulier netwerk  
In een onderneming is een bedrijfsfirewall wordt uitgevoerd op de centrale-router van de organisatie. Windows Firewall wordt uitgevoerd als een daemon op de lokale computer waarop de zelf-hostende integratieruntime is geïnstalleerd. 

De volgende tabel bevat de vereisten voor uitgaande poort en het domein voor zakelijke firewalls:

| Domeinnamen                  | Uitgaande poorten | Description                              |
| ----------------------------- | -------------- | ---------------------------------------- |
| `*.servicebus.windows.net`    | 443            | Vereist door de zelf-hostende integratieruntime verbinding maken met services voor gegevensverplaatsing in Data Factory. |
| `*.frontend.clouddatahub.net` | 443            | Vereist door de zelf-hostende integratieruntime verbinding maken met de Data Factory-service. |
| `download.microsoft.com`    | 443            | Vereist door de zelf-hostende integratieruntime voor het downloaden van de updates. Als u automatische update hebt uitgeschakeld vervolgens u kunt deze stap overslaan. |
| `*.core.windows.net`          | 443            | Gebruikt door de zelf-hostende integratieruntime verbinding maken met de Azure storage-account wanneer u de [gefaseerd kopiëren](copy-activity-performance.md#staged-copy) functie. |
| `*.database.windows.net`      | 1433           | (Optioneel) Vereist bij het kopiëren van of naar Azure SQL Database of Azure SQL Data Warehouse. Gebruik de functie gefaseerd kopiëren om gegevens te kopiëren naar Azure SQL Database of Azure SQL Data Warehouse zonder poort 1433 openen. |
| `*.azuredatalakestore.net`<br>`login.microsoftonline.com/<tenant>/oauth2/token`    | 443            | (Optioneel) Vereist bij het kopiëren van of naar Azure Data Lake Store. |

> [!NOTE] 
> Mogelijk hebt voor het beheren van poorten of in de whitelist aan domeinen op het niveau van de firewall van het bedrijf zoals vereist door de respectieve gegevensbronnen. Deze tabel alleen maakt gebruik van Azure SQL Database, Azure SQL Data Warehouse en Azure Data Lake Store als voorbeelden.   

De volgende tabel bevat de vereisten van de binnenkomende poort voor de Windows Firewall:

| Poorten voor inkomend verkeer | Description                              |
| ------------- | ---------------------------------------- |
| 8050 (TCP)    | Vereist voor de PowerShell-cmdlet voor versleuteling, zoals beschreven in [versleutelen referenties voor on-premises gegevensarchieven in Azure Data Factory](encrypt-credentials-self-hosted-integration-runtime.md), en door de toepassing Referentiebeheer veilig referenties instellen voor on-premises gegevensopslagexemplaren in de zelf-hostende integratieruntime. |

![Vereisten voor de gateway-poort](media/data-movement-security-considerations/gateway-port-requirements.png) 

#### <a name="ip-configurations-and-whitelisting-in-data-stores"></a>IP-configuraties en in een whitelist opnemen in de gegevensopslag
Sommige gegevensarchieven in de cloud vereist ook dat u goedgekeurde IP-adres van de computer die toegang tot de store. Zorg ervoor dat het IP-adres van de zelf-hostende integration runtime-machine in de whitelist is of in de firewall op de juiste wijze geconfigureerd.

De volgende cloud-gegevensarchieven vereisen dat u goedgekeurde IP-adres van de zelf-hostende integration runtime-machine. Sommige van deze gegevensarchieven, standaard, mogelijk niet in de whitelist. 

- [Azure SQL Database](../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md)
- [Azure Data Lake Store](../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../cosmos-db/firewall-support.md)
- [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Kan de zelf-hostende integratieruntime worden gedeeld met andere data factory's?**

We bieden deze functie nog geen ondersteuning. We werken actief erop.

**Wat zijn de Poortvereisten voor de zelf-hostende integratieruntime om te werken?**

De zelf-hostende integratieruntime maakt op basis van HTTP-verbindingen voor toegang tot het internet. De uitgaande poorten 443 moeten worden geopend voor de zelf-hostende integratieruntime om deze verbinding te maken. Open de binnenkomende poort 8050 alleen op het niveau van de machine (niet het niveau van de firewall van het bedrijf) voor de toepassing Referentiebeheer. Als Azure SQL Database of Azure SQL Data Warehouse wordt gebruikt als de bron- of de doelserver, moet u ook poort 1433 openen. Zie voor meer informatie de [Firewall-configuraties en IP-adressen voor opname in de whitelist](#firewall-configurations-and-whitelisting-ip-address-of-gateway) sectie. 


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de prestaties van Azure Data Factory Copy Activity [Kopieeractiviteit prestatie- en afstemmingshandleiding](copy-activity-performance.md).

 
