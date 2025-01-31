---
title: Beveiligingsoverwegingen voor het verplaatsen van gegevens in Azure Data Factory | Microsoft Docs
description: Meer informatie over het beveiligen van verplaatsing van gegevens in Azure Data Factory.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 083770c24a6c8939f8d1ff9f0efd5d18aff9dcb0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60487033"
---
# <a name="azure-data-factory---security-considerations-for-data-movement"></a>Azure Data Factory - beveiligingsoverwegingen voor het verplaatsen van gegevens

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [data movement beveiligingsoverwegingen voor Data Factory](../data-movement-security-considerations.md).

## <a name="introduction"></a>Inleiding
Dit artikel beschrijft de basisvereisten voor beveiliging-infrastructuur die services voor gegevensverplaatsing in Azure Data Factory worden gebruikt om uw gegevens te beveiligen. Azure Data Factory-management-resources zijn gebouwd op Azure-beveiliging-infrastructuur en gebruik van alle mogelijke beveiligingsmaatregelen die worden aangeboden door Azure.

In een Data Factory-oplossing maakt u een of meer gegevens[pijplijnen](data-factory-create-pipelines.md). Een pijplijn is een logische groep activiteiten die samen een taak uitvoeren. Deze pipelines zich bevinden in de regio waar de data factory is gemaakt. 

Hoewel Data Factory is alleen beschikbaar in **VS-West**, **VS-Oost**, en **Noord-Europa** regio's, data movement service is beschikbaar [wereldwijd in meerdere regio's](data-factory-data-movement-activities.md#global). Data Factory-service zorgt ervoor dat gegevens niet een geografische gebied laat / regio, tenzij u expliciet opgeven de service dat te gebruiken een andere regio als data movement service nog niet is geïmplementeerd voor deze regio. 

Azure Data Factory zelf slaat geen gegevens, met uitzondering van gekoppelde Servicereferenties voor cloudgegevensarchieven die zijn versleuteld met behulp van certificaten. U kunt er gegevensgestuurde werkstromen mee maken om de verplaatsing van gegevens te beheren tussen [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) en om er gegevens mee te verwerken middels [Compute Services](data-factory-compute-linked-services.md) in andere regio's of in een on-premises omgeving. U kunt er ook [werkstromen mee bewaken en beheren](data-factory-monitor-manage-pipelines.md) met zowel programmatische als gebruikersinterfacemechanismen.

Verplaatsing van gegevens met Azure Data Factory is **gecertificeerde** voor:
-   [HIPAA/HITECH](https://www.microsoft.com/en-us/trustcenter/Compliance/HIPAA)  
-   [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27001)  
-   [ISO/IEC 27018](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27018) 
-   [CSA STAR](https://www.microsoft.com/en-us/trustcenter/Compliance/CSA-STAR-Certification)
     
Als u geïnteresseerd in Azure, naleving en hoe Azure een eigen infrastructuur beveiligt bent, gaat u naar de [Microsoft Trust Center](https://microsoft.com/en-us/trustcenter/default.aspx). 

In dit artikel bekijken we beveiligingsoverwegingen in de volgende twee verkeer scenario's: 

- **Cloudscenario**-In dit scenario, de bron- en doelserver zijn openbaar toegankelijk zijn via internet. Het gaat hierbij om beheerde cloud storage services zoals Azure Storage, Azure SQL Data Warehouse, Azure SQL Database, Azure Data Lake Store, Amazon S3, Amazon Redshift, SaaS-services zoals Salesforce en web-protocollen, zoals FTP- en OData. U vindt een volledige lijst met ondersteunde gegevensbronnen [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats).
- **Hybride scenario**: In dit scenario, de bron- of doelservers zich achter een firewall of in een on-premises zakelijke netwerk of de gegevens opslag bevindt zich in een particulier netwerk / virtuele netwerk (meestal de bron) en is niet openbaar toegankelijk is. Databaseservers die worden gehost op virtuele machines kunnen ook worden onderverdeeld in dit scenario.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="cloud-scenarios"></a>Cloud-scenario 's
### <a name="securing-data-store-credentials"></a>Referenties opslaan, het beveiligen van gegevens
Azure Data Factory beschermt uw referenties voor het opslaan van gegevens door **versleutelen** ze met behulp van **certificaten die worden beheerd door Microsoft**. Deze certificaten worden gedraaid elke **twee jaar** (waaronder verlenging van het certificaat en de migratie van referenties). Deze versleutelde referenties zijn veilig opgeslagen een **Azure Storage beheerd door Azure Data Factory-beheerservices**. Raadpleeg voor meer informatie over de beveiliging van Azure Storage [beveiligingsoverzicht voor Azure Storage](../../security/security-storage-overview.md).

### <a name="data-encryption-in-transit"></a>Gegevensversleuteling onderweg
Als het gegevensarchief cloud biedt ondersteuning voor HTTPS- of TLS, alle gegevens worden overgedragen tussen services voor gegevensverplaatsing in Data Factory en een gegevensarchief voor de cloud worden via een beveiligd kanaal HTTPS of TLS.

> [!NOTE]
> Alle verbindingen met **Azure SQL Database** en **Azure SQL Data Warehouse** altijd versleuteling (SSL/TLS) terwijl gegevens tijdens overdracht naar en van de database. Bij het ontwerpen van een pijplijn met behulp van een JSON-editor, toevoegen de **versleuteling** eigenschap instelt op **waar** in de **verbindingsreeks**. Wanneer u gebruikt de [Kopieerwizard](data-factory-azure-copy-wizard.md), de wizard stelt deze eigenschap standaard. Voor **Azure Storage**, kunt u **HTTPS** in de verbindingsreeks.

### <a name="data-encryption-at-rest"></a>Versleuteling van inactieve gegevens
Sommige gegevensarchieven ondersteuning voor versleuteling van data-at-rest. Het is raadzaam om de gegevens-versleutelingsmechanisme voor deze gegevensarchieven in te schakelen. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse
Transparante gegevensversleuteling (TDE) in Azure SQL Data Warehouse helpt bij de bescherming tegen de dreiging van schadelijke activiteiten door het uitvoeren van real-time versleuteling en ontsleuteling van uw gegevens in rust. Dit gedrag is transparant voor de client. Zie voor meer informatie, [beveiligen van een database in SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

#### <a name="azure-sql-database"></a>Azure SQL Database
Azure SQL Database biedt ook ondersteuning voor transparante gegevensversleuteling (TDE), die helpt bij de bescherming tegen de dreiging van schadelijke activiteiten door het uitvoeren van real-time versleuteling en ontsleuteling van de gegevens zonder wijzigingen aan de toepassing. Dit gedrag is transparant voor de client. Zie voor meer informatie, [Transparent Data Encryption met Azure SQL Database](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database). 

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Azure Data Lake store biedt ook versleuteling voor gegevens die zijn opgeslagen in het account. Wanneer dit is ingeschakeld, Data Lake store versleutelt automatisch gegevens voor het persistent maken en ontsleutelt voordat ophalen, waardoor het transparant voor de client toegang tot de gegevens. Zie voor meer informatie, [beveiliging in Azure Data Lake Store](../../data-lake-store/data-lake-store-security-overview.md). 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Azure Blob Storage en Azure Table Storage
Azure Blob Storage en Azure Table storage biedt ondersteuning voor Storage Service Encryption (SSE), die automatisch worden uw gegevens worden versleuteld voordat opgeslagen en ontsleutelt voordat ophalen. Zie voor meer informatie, [Azure Storage-Serviceversleuteling voor Data-at-Rest](../../storage/common/storage-service-encryption.md).

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3 biedt ondersteuning voor de client- en versleuteling van data-at-Rest. Zie voor meer informatie, [gegevens beveiligen met behulp van versleuteling](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html). Data Factory biedt op dit moment geen ondersteuning voor Amazon S3 binnen een virtuele privécloud (VPC).

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift ondersteunt cluster versleuteling voor data-at-rest. Zie voor meer informatie, [Amazon Redshift-Databaseversleuteling](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html). Data Factory biedt op dit moment geen ondersteuning voor Amazon Redshift binnen een VPC. 

#### <a name="salesforce"></a>Salesforce
SalesForce ondersteunt Shield Platform versleuteling waarmee het coderen van alle bestanden, bijlagen en aangepaste velden. Zie voor meer informatie, [inzicht in de Web Server OAuth-Verificatiestroom](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm).  

## <a name="hybrid-scenarios-using-data-management-gateway"></a>Hybride scenario's (met behulp van Data Management Gateway)
Hybride scenario's vereisen Data Management Gateway worden geïnstalleerd in een on-premises netwerk of in een virtueel netwerk (Azure) of een virtuele privécloud (Amazon). De gateway moet toegang hebben tot de lokale gegevensarchieven. Zie voor meer informatie over de gateway, [Data Management Gateway](data-factory-data-management-gateway.md). 

![Data Management Gateway kanalen](media/data-factory-data-movement-security-considerations/data-management-gateway-channels.png)

De **opdrachtkanaal** communicatie tussen services voor gegevensverplaatsing in Data Factory en Data Management Gateway toestaat. De communicatie bevat informatie met betrekking tot de activiteit. Het gegevenskanaal wordt gebruikt voor het overbrengen van gegevens tussen on-premises gegevensopslagexemplaren en cloudgegevensopslag.    

### <a name="on-premises-data-store-credentials"></a>On-premises gegevens opslaan van referenties
De referenties voor uw on-premises gegevensarchieven worden lokaal opgeslagen (niet in de cloud). Ze kunnen worden ingesteld op drie verschillende manieren. 

- Met behulp van **tekst zonder opmaak** (minder veilig) via HTTPS van Azure Portal / Wizard kopiëren. De referenties worden in tekst zonder opmaak doorgegeven aan de on-premises gateway.
- Met behulp van **cryptografie met JavaScript-bibliotheek van de Wizard kopiëren**.
- Met behulp van **klikt u op-als op basis van referenties manager app**. De Klik-zodra de toepassing wordt uitgevoerd op de on-premises computer die toegang heeft tot de gateway en stelt de aanmeldingsgegevens voor de gegevensopslag. Deze optie en de volgende zijn de meest veilige keuzes. De app credential manager standaard, gebruikt de poort 8050 op de machine met de gateway voor veilige communicatie.  
- Gebruik [New-AzDataFactoryEncryptValue](/powershell/module/az.datafactory/New-azDataFactoryEncryptValue) PowerShell-cmdlet voor het versleutelen van referenties. De cmdlet maakt gebruik van het certificaat dat de gateway is geconfigureerd om te gebruiken voor het versleutelen van de referenties. U kunt de versleutelde referenties die zijn geretourneerd door deze cmdlet gebruiken en deze toe te voegen **EncryptedCredential** element van de **connectionString** in het JSON-bestand dat u met gebruikt de [ Nieuwe AzDataFactoryLinkedService](/powershell/module/az.datafactory/new-azdatafactorylinkedservice) cmdlet of in de JSON-codefragment in de Data Factory Editor in de portal. Deze optie en klik op-zodra de veiligste opties-toepassing zijn. 

#### <a name="javascript-cryptography-library-based-encryption"></a>Versleuteling op basis van een bibliotheek met JavaScript cryptografie
U kunt de referenties van de gegevensopslag met behulp van versleutelen [cryptografie met JavaScript-bibliotheek](https://www.microsoft.com/download/details.aspx?id=52439) uit de [Wizard kopiëren](data-factory-copy-wizard.md). Wanneer u deze optie selecteert, wordt de Wizard kopiëren opgehaald van de openbare sleutel van de gateway en gebruikt voor het versleutelen van de referenties van de gegevensopslag. De referenties worden door de gatewaycomputer ontsleuteld en worden beschermd door Windows [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx).

**Ondersteunde browsers:** IE8, IE9, IE10, IE11, Microsoft Edge, en de meest recente Firefox, Chrome, Opera, Safari-browser. 

#### <a name="click-once-credentials-manager-app"></a>Klik op-één keer referenties manager-app
U kunt de klik starten-eenmaal op basis van referentie manager app vanuit Azure portal/kopiëren Wizard bij het ontwerpen van pijplijnen. Deze toepassing zorgt ervoor dat referenties niet worden doorgegeven als tekst zonder opmaak via de kabel. Standaard wordt de poort **8050** op de machine met de gateway voor veilige communicatie. Indien nodig, kan deze poort kan worden gewijzigd.  
  
![HTTPS-poort voor de gateway](media/data-factory-data-movement-security-considerations/https-port-for-gateway.png)

Op dit moment Data Management Gateway maakt gebruik van een enkel **certificaat**. Dit certificaat is gemaakt tijdens de installatie van de gateway (van toepassing op Data Management Gateway is gemaakt na November 2016 en versie 2.4.xxxx.x of hoger). U kunt dit certificaat vervangen door uw eigen SSL/TLS-certificaat. Dit certificaat wordt gebruikt door de klik-referentie eenmaal manager toepassing veilig verbinding maken met het gateway-apparaat voor het instellen van referenties van de gegevensopslag. Gegevens worden opgeslagen referenties van de gegevensopslag veilig on-premises met behulp van de Windows [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx) op de machine met de gateway. 

> [!NOTE]
> Oude gateways die zijn geïnstalleerd voordat u November 2016 of van versie 2.3.xxxx.x echter ook doorgaan met referenties versleuteld en opgeslagen in de cloud. Zelfs als u een upgrade uitvoert van de gateway naar de nieuwste versie, zijn de referenties niet gemigreerd naar een on-premises machine    
  
| Gatewayversie (tijdens het maken van) | Referenties die zijn opgeslagen | Versleuteling van referenties / security | 
| --------------------------------- | ------------------ | --------- |  
| < = 2.3.xxxx.x | Op de cloud | Versleuteld met behulp van certificaten (die afwijken van de die werd gebruikt door Credential manager app) | 
| > = 2.4.xxxx.x | On-premises | Beveiligd via DPAPI | 
  

### <a name="encryption-in-transit"></a>Versleuteling tijdens overdracht
Alle gegevensoverdrachten zijn via een beveiligd kanaal **HTTPS** en **TLS via TCP** om man-in-the-middle-aanvallen te voorkomen dat tijdens de communicatie met Azure-services.
 
U kunt ook [IPSec VPN](../../vpn-gateway/vpn-gateway-about-vpn-devices.md) of [Express Route](../../expressroute/expressroute-introduction.md) naar verdere Beveilig het communicatiekanaal tussen uw on-premises netwerk en Azure.

Virtueel netwerk is een logische weergave van uw netwerk in de cloud. U kunt een on-premises netwerk verbinden met uw Azure-netwerk (VNet) door het instellen van IPSec-VPN (site-naar-site) of Express Route (persoonlijke Peering)     

De volgende tabel geeft een overzicht van de netwerk- en gateway-configuratie-aanbevelingen op basis van verschillende combinaties van bron- en doellocaties voor hybride verplaatsing van gegevens.

| source | Bestemming | Netwerkconfiguratie | Gateway-installatie |
| ------ | ----------- | --------------------- | ------------- | 
| On-premises | Virtuele machines en cloudservices die zijn geïmplementeerd in virtuele netwerken | IPSec-VPN (punt-naar-site of site-naar-site) | Gateway kan worden geïnstalleerd van on-premises of op een virtuele Azure voor machine (VM) in VNet | 
| On-premises | Virtuele machines en cloudservices die zijn geïmplementeerd in virtuele netwerken | ExpressRoute (persoonlijke Peering) | Gateway kan worden geïnstalleerd van on-premises of op een Azure-VM in VNet | 
| On-premises | Azure-services waarvoor een openbaar eindpunt | ExpressRoute (openbare Peering) | Gateway moet geïnstalleerd on-premises worden | 

De volgende afbeeldingen ziet het gebruik van Data Management Gateway voor het verplaatsen van gegevens tussen een on-premises database en Azure-services met behulp van expressroute en IPSec-VPN (met een Virtueelnetwerk):

**Express Route:**
 
![Express Route gebruikt met gateway](media/data-factory-data-movement-security-considerations/express-route-for-gateway.png) 

**IPSec VPN:**

![IPSec-VPN met gateway](media/data-factory-data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-whitelisting-ip-address-of-gateway"></a>Firewall-configuraties en in de whitelist aan IP-adres van gateway

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Firewall-vereisten voor on-premises/particulier netwerk  
In een onderneming, een **bedrijfsfirewall** wordt uitgevoerd op de centrale-router van de organisatie. En **Windows firewall** wordt uitgevoerd als een daemon op de lokale computer waarop de gateway is geïnstalleerd. 

De volgende tabel bevat **uitgaande poort** en vereisten voor het domein voor de **bedrijfsfirewall**.

| Domeinnamen | Uitgaande poorten | Description |
| ------------ | -------------- | ----------- | 
| `*.servicebus.windows.net` | 443, 80 | Vereist door de gateway verbinding maken met services voor gegevensverplaatsing in Data Factory |
| `*.core.windows.net` | 443 | Gebruikt door de gateway verbinding maken met Azure Storage-Account wanneer u de [gefaseerd kopiëren](data-factory-copy-activity-performance.md#staged-copy) functie. | 
| `*.frontend.clouddatahub.net` | 443 | Vereist door de gateway verbinding maken met de Azure Data Factory-service. | 
| `*.database.windows.net` | 1433   | (Optioneel) die nodig zijn als de bestemming Azure SQL-Database is / Azure SQL Data Warehouse. Gebruik de functie gefaseerd kopiëren om gegevens te kopiëren naar Azure SQL Database-/ Azure SQL Data Warehouse zonder het openen van de poort 1433. | 
| `*.azuredatalakestore.net` | 443 | (Optioneel) die nodig zijn als de bestemming Azure Data Lake store is | 

> [!NOTE] 
> Mogelijk moet u poorten beheren / domeinen in een whitelist opnemen in de bedrijfsfirewall niveau zoals vereist door de respectieve gegevensbronnen. Deze tabel alleen maakt gebruik van Azure SQL Database, Azure SQL Data Warehouse, Azure Data Lake Store als voorbeelden.   

De volgende tabel bevat **binnenkomende poort** vereisten voor de **windows firewall**.

| Poorten voor inkomend verkeer | Description | 
| ------------- | ----------- | 
| 8050 (TCP) | Vereist door de toepassing Referentiebeheer veilig referenties instellen voor on-premises gegevensopslagexemplaren op de gateway. | 

![Vereisten voor de gateway-poort](media/data-factory-data-movement-security-considerations/gateway-port-requirements.png)

#### <a name="ip-configurations-whitelisting-in-data-store"></a>IP-configuraties / in de whitelist aan gegevens opslaan
Sommige gegevensarchieven in de cloud moeten ook zwarte lijst plaatsen van IP-adres van de toegang tot deze computer. Zorg ervoor dat het IP-adres van het gateway-apparaat in de whitelist is / in de firewall op de juiste wijze geconfigureerd.

De volgende cloud-gegevensarchieven vereisen zwarte lijst plaatsen van IP-adres van het gateway-apparaat. Sommige van deze gegevensarchieven, standaard, mogelijk niet de zwarte lijst plaatsen van het IP-adres. 

- [Azure SQL Database](../../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-get-started-provision.md)
- [Azure Data Lake Store](../../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../../cosmos-db/firewall-support.md)
- [Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Vraag:** Kan de Gateway worden gedeeld met andere data factory's?
**Antwoord:** We bieden deze functie nog geen ondersteuning. We zijn hier druk mee bezig.

**Vraag:** Wat zijn de Poortvereisten voor de gateway om te werken?
**Antwoord:** Gateway maakt op basis van HTTP-verbindingen met het openbare internet. De **uitgaande poorten 443 en 80** moet worden geopend voor de gateway deze verbinding te maken. Open **binnenkomende poort 8050** alleen op het machineniveau van de (niet op het niveau van de firewall van het bedrijf) voor de toepassing Referentiebeheer. Als Azure SQL Database of Azure SQL Data Warehouse wordt gebruikt als bron / bestemming, dan hebt u nodig hebt om te openen **1433** ook poort. Zie voor meer informatie, [Firewall-configuraties en IP-adressen voor opname in de whitelist](#firewall-configurations-and-whitelisting-ip-address-of gateway) sectie. 

**Vraag:** Wat zijn de vereisten voor certificaten voor de Gateway?
**Antwoord:** Huidige gateway vereist een certificaat dat wordt gebruikt door de toepassing Referentiebeheer om referenties van de gegevensopslag veilig in te stellen. Dit certificaat is een zelfondertekend certificaat gemaakt en geconfigureerd door de gateway-installatie. U kunt uw eigen TLS / SSL-certificaat in plaats daarvan. Zie voor meer informatie, [klikt u op-één keer credential manager-toepassing](#click-once-credentials-manager-app) sectie. 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de prestaties van kopieeractiviteit [en afstemmingshandleiding van activiteit kopiëren](data-factory-copy-activity-performance.md).

 
