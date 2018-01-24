---
title: Beveiligingsoverwegingen voor gegevensverplaatsing in Azure Data Factory | Microsoft Docs
description: Meer informatie over het beveiligen van gegevensverplaatsing in Azure Data Factory.
services: data-factory
documentationcenter: 
author: nabhishek
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: f483109170ed1dda7506f7ef5f02fb8b42ea331e
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="azure-data-factory---security-considerations-for-data-movement"></a>Azure Data Factory - beveiligingsoverwegingen voor gegevensverplaatsing

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [data movement beveiligingsoverwegingen voor Data Factory versie 2](../data-movement-security-considerations.md).

## <a name="introduction"></a>Inleiding
In dit artikel beschrijft de infrastructuur van de basisprincipes van beveiliging die services voor gegevensverplaatsing in Azure Data Factory gebruiken om uw gegevens te beveiligen. Azure Data Factory-management-resources zijn gebouwd op Azure beveiligingsinfrastructuur en alle mogelijke veiligheidsmaatregelen, die worden aangeboden door Azure gebruiken.

In een Data Factory-oplossing maakt u een of meer gegevens[pijplijnen](data-factory-create-pipelines.md). Een pijplijn is een logische groep activiteiten die samen een taak uitvoeren. Deze pijplijnen zich bevinden in de regio waar de gegevensfactory is gemaakt. 

Hoewel Data Factory is alleen beschikbaar in **VS-West**, **VS-Oost**, en **Noord-Europa** regio's, data movement service is beschikbaar [globaal in verschillende regio's](data-factory-data-movement-activities.md#global). Data Factory-service zorgt ervoor dat gegevens een geografisch gebied laat / regio tenzij u expliciet de opdracht de service geven moet een andere regio gebruiken als data movement service nog niet is geïmplementeerd voor deze regio. 

Azure Data Factory zelf slaat geen gegevens, met uitzondering van referenties van de gekoppelde service voor cloud-gegevensarchieven, die zijn gecodeerd met behulp van certificaten. U kunt er gegevensgestuurde werkstromen mee maken om de verplaatsing van gegevens te beheren tussen [ondersteunde gegevensarchieven](data-factory-data-movement-activities.md#supported-data-stores-and-formats) en om er gegevens mee te verwerken middels [Compute Services](data-factory-compute-linked-services.md) in andere regio's of in een on-premises omgeving. U kunt er ook [werkstromen mee bewaken en beheren](data-factory-monitor-manage-pipelines.md) met zowel programmatische als gebruikersinterfacemechanismen.

Verplaatsing van gegevens met behulp van Azure Data Factory is **gecertificeerd** voor:
-   [HIPAA/HITECH](https://www.microsoft.com/en-us/trustcenter/Compliance/HIPAA)  
-   [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27001)  
-   [ISO/IEC 27018](https://www.microsoft.com/en-us/trustcenter/Compliance/ISO-IEC-27018) 
-   [CSA STER](https://www.microsoft.com/en-us/trustcenter/Compliance/CSA-STAR-Certification)
     
Als u geïnteresseerd in Azure compatibiliteit en hoe Azure de eigen infrastructuur beveiligt bent, gaat u naar de [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/default.aspx). 

In dit artikel controleert u beveiligingsoverwegingen in de volgende twee data movement scenario's: 

- **Cloudscenario**-In dit scenario wordt de bron- en doelserver zijn openbaar toegankelijk zijn via internet. Het gaat hierbij om services zoals Azure Storage, Azure SQL Data Warehouse, Azure SQL Database, Azure Data Lake Store, Amazon S3, Amazon-Redshift SaaS-services zoals Salesforce en web-protocollen, zoals FTP en OData beheerde cloudopslag. U vindt een volledige lijst met ondersteunde gegevensbronnen [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats).
- **Hybride scenario**: In dit scenario wordt de bron- of doelserver zich achter een firewall of binnen een on-premises zakelijke netwerk of de gegevens store is in een particulier netwerk / virtuele netwerk (meestal de bron) en is niet openbaar toegankelijk. Databaseservers die worden gehost op virtuele machines vallen ook onder dit scenario.

## <a name="cloud-scenarios"></a>Cloud-scenario 's
###<a name="securing-data-store-credentials"></a>Referenties opslaan, het beveiligen van gegevens
Azure Data Factory beveiligt uw referenties voor gegevensopslag door **versleutelen** ze met behulp van **certificaten die worden beheerd door Microsoft**. Deze certificaten worden gedraaid elke **twee jaar** (waaronder migratie van referenties en vernieuwen van certificaat). Deze versleutelde referenties zijn veilig opgeslagen in een **Azure Storage beheerd door Azure Data Factory-beheerservices**. Raadpleeg voor meer informatie over Azure Storage beveiliging [beveiligingsoverzicht van Azure Storage](../../security/security-storage-overview.md).

### <a name="data-encryption-in-transit"></a>Gegevensversleuteling onderweg
Als de gegevensopslag van de cloud HTTPS of TLS ondersteunt, alle gegevens worden overgedragen tussen services voor gegevensverplaatsing in Data Factory en een gegevensarchief cloud zijn via een beveiligde HTTPS- of TLS-kanaal.

> [!NOTE]
> Alle verbindingen met **Azure SQL Database** en **Azure SQL Data Warehouse** altijd versleuteling vereisen (SSL/TLS) wanneer gegevens verzonden naar en van de database. Tijdens het ontwerpen van een pijplijn met een JSON-editor, voeg de **versleuteling** eigenschap en wordt ingesteld op **waar** in de **verbindingsreeks**. Wanneer u gebruikt de [Wizard kopiëren](data-factory-azure-copy-wizard.md), de wizard stelt deze eigenschap standaard. Voor **Azure Storage**, kunt u **HTTPS** in de verbindingstekenreeks.

### <a name="data-encryption-at-rest"></a>Versleuteling van inactieve gegevens
Sommige gegevens worden opgeslagen ondersteuning voor versleuteling van gegevens in rust. Het is raadzaam om de gegevens versleuteling mechanisme voor deze opgeslagen gegevens in te schakelen. 

#### <a name="azure-sql-data-warehouse"></a>Azure SQL Data Warehouse
Transparent Data Encryption (TDE) in Azure SQL Data Warehouse kunt beveiligen tegen de dreiging van schadelijke activiteiten door realtime versleuteling en ontsleuteling van uw gegevens in rust in te voeren. Dit gedrag is transparant voor de client. Zie voor meer informatie [beveiligen van een database in SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-overview-manage-security.md).

#### <a name="azure-sql-database"></a>Azure SQL Database
Azure SQL Database biedt ook ondersteuning voor transparante gegevensversleuteling (TDE), die helpt bij het beveiligen tegen de dreiging van schadelijke activiteiten door realtime versleuteling en ontsleuteling van de gegevens in te voeren zonder dat wijzigingen in de toepassing. Dit gedrag is transparant voor de client. Zie voor meer informatie [Transparent Data Encryption met Azure SQL Database](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database). 

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Azure Data Lake store biedt ook versleuteling voor gegevens die zijn opgeslagen in het account. Wanneer dit is ingeschakeld, wordt Data Lake store automatisch versleutelt gegevens voordat het behouden blijven en ontsleutelt voordat ophalen, waardoor het transparant voor de client toegang tot de gegevens. Zie voor meer informatie [beveiliging in Azure Data Lake Store](../../data-lake-store/data-lake-store-security-overview.md). 

#### <a name="azure-blob-storage-and-azure-table-storage"></a>Azure Blob Storage en Azure-tabelopslag
Azure Blob Storage en Azure Table storage ondersteunt opslag Service versleuteling (SSE), die automatisch worden uw gegevens worden versleuteld voordat behouden blijven van opslag en ontsleutelt voordat voor het ophalen. Zie voor meer informatie [Azure Storage Service: versleuteling van gegevens in rust](../../storage/common/storage-service-encryption.md).

#### <a name="amazon-s3"></a>Amazon S3
Amazon S3 ondersteunt zowel client als server versleuteling van gegevens in rust. Zie voor meer informatie [gegevens beveiligen met behulp van versleuteling](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingEncryption.html). Data Factory biedt momenteel geen ondersteuning voor Amazon S3 binnen een virtuele privécloud (VPC).

#### <a name="amazon-redshift"></a>Amazon Redshift
Amazon Redshift ondersteunt cluster versleuteling voor gegevens in rust. Zie voor meer informatie [Amazon Redshift Databaseversleuteling](http://docs.aws.amazon.com/redshift/latest/mgmt/working-with-db-encryption.html). Data Factory biedt momenteel geen ondersteuning voor Amazon Redshift binnen een VPC. 

#### <a name="salesforce"></a>SalesForce
SalesForce ondersteunt Shield Platform versleuteling waarmee versleuteling van bestanden, bijlagen en aangepaste velden. Zie voor meer informatie [inzicht in de Web Server OAuth-verificatie stromen](https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/intro_understanding_web_server_oauth_flow.htm).  

## <a name="hybrid-scenarios-using-data-management-gateway"></a>Hybride scenario's (met behulp van Data Management Gateway)
Hybride scenario's moet Data Management Gateway moet worden geïnstalleerd in een on-premises netwerk of in een virtueel netwerk (Azure) of een virtuele privécloud (Amazon). De gateway moet toegang kunnen krijgen tot de lokale gegevens worden opgeslagen. Zie voor meer informatie over de gateway [Data Management Gateway](data-factory-data-management-gateway.md). 

![Data Management Gateway kanalen](media/data-factory-data-movement-security-considerations/data-management-gateway-channels.png)

De **opdrachtkanaal** communicatie tussen services voor gegevensverplaatsing in Data Factory- en Data Management Gateway. De communicatie bevat informatie met betrekking tot de activiteit. Het gegevenskanaal wordt gebruikt voor het overbrengen van gegevens tussen on-premises gegevensopslagexemplaren en cloud-gegevensarchieven.    

### <a name="on-premises-data-store-credentials"></a>On-premises gegevens opslaan van referenties
De referenties voor uw on-premises gegevensopslagexemplaren worden lokaal opgeslagen (niet in de cloud). Ze kunnen worden ingesteld op drie verschillende manieren. 

- Met behulp van **tekst zonder opmaak** (minder veilig) via HTTPS van Azure Portal / Wizard kopiëren. De referenties zijn tekst zonder opmaak doorgegeven aan de lokale gateway.
- Met behulp van **cryptografie met JavaScript-bibliotheek van de Wizard kopiëren**.
- Met behulp van **Klik-zodra op basis van referenties manager app**. De Klik-zodra de toepassing wordt uitgevoerd op de on-premises machine die toegang heeft tot de gateway en Hiermee stelt u referenties voor de gegevensopslag. Deze optie en de volgende zijn de veiligste opties. De referentie manager-app gebruikt de poort 8050 standaard op de machine met de gateway voor veilige communicatie.  
- Gebruik [nieuw AzureRmDataFactoryEncryptValue](/powershell/module/azurerm.datafactories/New-AzureRmDataFactoryEncryptValue) PowerShell-cmdlet om referenties te versleutelen. Het certificaat wordt gebruikt door de cmdlet die gateway is geconfigureerd om te gebruiken om de referenties te versleutelen. U kunt de versleutelde referenties die zijn geretourneerd door deze cmdlet gebruiken en toe te voegen aan **EncryptedCredential** element van de **connectionString** in het JSON-bestand dat u met gebruikt de [ Nieuwe AzureRmDataFactoryLinkedService](/powershell/module/azurerm.datafactories/new-azurermdatafactorylinkedservice) cmdlet of in het JSON-fragment in de Data Factory-Editor in de portal. Deze optie en klik op-als de toepassing de veiligste opties zijn. 

#### <a name="javascript-cryptography-library-based-encryption"></a>Versleuteling op basis van een bibliotheek met JavaScript cryptografie
U kunt met behulp van referenties voor gegevensopslag versleutelen [cryptografie met JavaScript-bibliotheek](https://www.microsoft.com/download/details.aspx?id=52439) van de [Wizard kopiëren](data-factory-copy-wizard.md). Wanneer u deze optie selecteert, wordt de Wizard kopiëren haalt de openbare sleutel van de gateway en gebruikt voor het versleutelen van de referenties voor de gegevensopslag. De referenties worden ontsleuteld door de gatewaycomputer en beveiligd door Windows [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx).

**Ondersteunde browsers:** IE8, IE9, IE10, IE11, Microsoft Edge en meest recente Firefox, Chrome, Opera, Safari-browser. 

#### <a name="click-once-credentials-manager-app"></a>Klik op-één keer referenties manager-app
U kunt het klikt u op starten-zodra op basis van de app voor de referentie van een Azure-portal/kopie Wizard bij het ontwerpen van pijplijnen. Deze toepassing zorgt ervoor dat referenties niet worden overgezet als tekst zonder opmaak via de kabel. Standaard wordt de poort **8050** op de machine met de gateway voor veilige communicatie. Indien nodig, kan deze poort kan worden gewijzigd.  
  
![HTTPS-poort voor de gateway](media/data-factory-data-movement-security-considerations/https-port-for-gateway.png)

Data Management Gateway gebruikt momenteel, één **certificaat**. Dit certificaat is gemaakt tijdens de installatie van de gateway (geldt voor Data Management Gateway is gemaakt nadat November 2016 en versie 2.4.xxxx.x of hoger). U kunt dit certificaat vervangen door uw eigen certificaat voor SSL/TLS. Dit certificaat wordt gebruikt door het klikt u op-één keer credential manager-toepassing veilig verbinding te maken met de gateway-VM voor het instellen van referenties voor gegevensopslag. Gegevens worden opgeslagen referenties voor gegevensopslag veilig on-premises door gebruik van de Windows [DPAPI](https://msdn.microsoft.com/library/ms995355.aspx) op de machine met de gateway. 

> [!NOTE]
> Oudere gateways die zijn geïnstalleerd voordat November 2016 of van versie 2.3.xxxx.x blijven versleuteld en opgeslagen in de cloud-referenties gebruiken. Zelfs als u een upgrade uitvoeren van de gateway naar de nieuwste versie, worden de referenties niet gemigreerd naar een lokale machine    
  
| Gatewayversie (tijdens het maken van) | Referenties die zijn opgeslagen | Versleuteling van referenties / beveiliging | 
| --------------------------------- | ------------------ | --------- |  
| < = 2.3.xxxx.x | In de cloud | Versleuteld met certificaat (anders dan de door de app voor de referentie gebruikt) | 
| > = 2.4.xxxx.x | On-premises | Beveiligd via DPAPI | 
  

### <a name="encryption-in-transit"></a>Codering in transit
Alle gegevensoverdrachten zijn via een beveiligd kanaal **HTTPS** en **TLS via TCP** om man-in-the-middle-aanvallen te voorkomen dat tijdens de communicatie met Azure-services.
 
U kunt ook [IPSec VPN](../../vpn-gateway/vpn-gateway-about-vpn-devices.md) of [Express Route](../../expressroute/expressroute-introduction.md) voor verdere Beveilig het communicatiekanaal tussen uw on-premises netwerk en Azure.

Virtueel netwerk is een logische representatie van uw netwerk in de cloud. U kunt een on-premises netwerk verbinden met uw Azure-netwerk (VNet) door het instellen van IPSec-VPN (site-naar-site) of Express Route (Privépeering)     

De volgende tabel geeft een overzicht van de netwerk- en gateway aanbevelingen voor de configuratie op basis van verschillende combinaties van bron- en doellocaties voor hybride gegevensverplaatsing.

| Bron | Doel | Netwerkconfiguratie | Gatewayinstallatie |
| ------ | ----------- | --------------------- | ------------- | 
| On-premises | Virtuele machines en cloudservices die zijn geïmplementeerd in virtuele netwerken | IPSec VPN (punt-naar-site of site-naar-site) | Gateway kan worden geïnstalleerd van on-premises of in een Azure virtuele machine u (VM) in VNet | 
| On-premises | Virtuele machines en cloudservices die zijn geïmplementeerd in virtuele netwerken | ExpressRoute (Privépeering) | Gateway kan worden on-premises geïnstalleerd of op een virtuele machine van Azure in VNet | 
| On-premises | Azure-services waarvoor een openbaar eindpunt | ExpressRoute (openbare Peering) | Gateway moet geïnstalleerde on-premises worden | 

De volgende afbeeldingen ziet het gebruik van Data Management Gateway voor het verplaatsen van gegevens tussen een on-premises database en de Azure-services met behulp van Express route- en IPSec-VPN (met een virtueel netwerk):

**Express Route:**
 
![Gebruik Express Route met gateway](media/data-factory-data-movement-security-considerations/express-route-for-gateway.png) 

**IPSec VPN:**

![IPSec VPN met gateway](media/data-factory-data-movement-security-considerations/ipsec-vpn-for-gateway.png)

### <a name="firewall-configurations-and-whitelisting-ip-address-of-gateway"></a>Firewall-configuraties en whitelisting IP-adres van gateway

#### <a name="firewall-requirements-for-on-premisesprivate-network"></a>Firewallvereisten voor voor-premises/particulier netwerk  
In een onderneming, een **bedrijfsfirewall** wordt uitgevoerd op de centrale router van de organisatie. En, **Windows firewall** wordt uitgevoerd als een daemon op de lokale computer waarop de gateway is geïnstalleerd. 

De volgende tabel bevat **uitgaande poort** en vereisten voor het domein voor de **bedrijfsfirewall**.

| Domeinnamen | Uitgaande poorten | Beschrijving |
| ------------ | -------------- | ----------- | 
| `*.servicebus.windows.net` | 443, 80 | Vereist voor de gateway verbinding maken met services voor gegevensverplaatsing in Data Factory |
| `*.core.windows.net` | 443 | Gebruikt door de gateway verbinding maken met Azure Storage-Account wanneer u de [kopie gefaseerde](data-factory-copy-activity-performance.md#staged-copy) functie. | 
| `*.frontend.clouddatahub.net` | 443 | Vereist door de gateway verbinding maken met de Azure Data Factory-service. | 
| `*.database.windows.net` | 1433   | (Optioneel) als bestemming voor de Azure SQL-Database nodig / Azure SQL Data Warehouse. De functie gefaseerde kopiëren gebruiken om gegevens te kopiëren naar Azure SQL Database/Azure SQL Data Warehouse zonder de poort 1433 openen. | 
| `*.azuredatalakestore.net` | 443 | (Optioneel) nodig hebt bij het doel Azure Data Lake store is | 

> [!NOTE] 
> Wellicht hebt u voor het beheren van poorten / whitelisting-domeinen in de bedrijfsfirewall niveau zoals vereist door de respectieve gegevensbronnen. Deze tabel alleen maakt gebruik van Azure SQL Database, Azure SQL Data Warehouse, Azure Data Lake Store als voorbeelden.   

De volgende tabel bevat **binnenkomende poort** vereisten voor de **windows firewall**.

| Poorten voor inkomend verkeer | Beschrijving | 
| ------------- | ----------- | 
| 8050 (TCP) | Vereist door de toepassing van de manager referentie referenties voor on-premises gegevensopslagexemplaren veilig instellen op de gateway. | 

![Vereisten voor de gateway-poort](media\data-factory-data-movement-security-considerations/gateway-port-requirements.png) 

#### <a name="ip-configurations-whitelisting-in-data-store"></a>IP-configuraties / whitelisting in gegevens opslaan
Sommige gegevens opslaat in de cloud, ook verplichten whitelisting van IP-adres van de computer die toegang hebben tot deze. Zorg ervoor dat het IP-adres van de gatewaycomputer is goedgekeurde lijst / correct zijn geconfigureerd in de firewall.

De volgende cloud gegevensarchieven vereisen whitelisting van IP-adres van het gateway-apparaat. Sommige van deze gegevensarchieven standaard vereist geen whitelisting van het IP-adres. 

- [Azure SQL Database](../../sql-database/sql-database-firewall-configure.md) 
- [Azure SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-get-started-provision.md)
- [Azure Data Lake Store](../../data-lake-store/data-lake-store-secure-data.md#set-ip-address-range-for-data-access)
- [Azure Cosmos DB](../../cosmos-db/firewall-support.md)
- [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) 

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Vraag:** kan de Gateway worden gedeeld door andere data Factory?
**Antwoord:** wij bieden geen ondersteuning voor deze functie nog. Er wordt gewerkt erop.

**Vraag:** wat zijn de Poortvereisten voor de gateway werkt?
**Antwoord:** Gateway op basis van HTTP verbindingen maakt met internet te openen. De **uitgaande poorten 443 en 80** voor de gateway voor deze verbinding moet worden geopend. Open **binnenkomende poort 8050** alleen op het niveau van de computer (niet op het niveau van de firewall van het bedrijf) voor de toepassing Referentiebeheer. Als Azure SQL Database- of Azure SQL Data Warehouse wordt gebruikt als bron / bestemming, dan hebt u nodig hebt om te openen **1433** ook poort. Zie voor meer informatie [Firewall-configuraties en whitelisting IP-adressen](#firewall-configurations-and-whitelisting-ip-address-of gateway) sectie. 

**Vraag:** wat zijn certificaten vereist voor de Gateway?
**Antwoord:** huidige gateway vereist een certificaat dat wordt gebruikt door de toepassing van de manager referentie voor het veilig instellen van referenties voor gegevensopslag. Dit certificaat is een zelfondertekend certificaat gemaakt en geconfigureerd door de gateway-instellingen. U kunt uw eigen TLS / SSL-certificaat in plaats daarvan. Zie voor meer informatie [klikt u op-één keer credential manager-toepassing](#click-once-credentials-manager-app) sectie. 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de prestaties van de kopieeractiviteit [activiteit prestaties en prestatieafstemming handleiding kopiëren](data-factory-copy-activity-performance.md).

 
