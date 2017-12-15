---
title: Detecteren, identificeren en classificeren van persoonlijke gegevens in Microsoft Azure | Microsoft Docs
description: "Meer informatie over het zoeken, classificeren, detecteren en gegevens worden geïdentificeerd"
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 0d99df534da4575f3c34ec6b3475cdd1bdc3308a
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="discover-identify-and-classify-personal-data-in-microsoft-azure"></a>Detecteren, identificeren en classificeren van persoonlijke gegevens in Microsoft Azure

In dit artikel biedt richtlijnen voor het detecteren, identificeren en classificeren van persoonlijke gegevens in verschillende Azure-hulpprogramma's en services, inclusief het gebruik van Azure Data Catalog, Azure Active Directory, SQL-Database, Power Query voor Hadoop-clusters in Azure HDInsight, Azure Gegevensbeveiliging, Azure Search en SQL-query's voor Azure Cosmos DB.

## <a name="scenario-problem-statement-and-goal"></a>Scenario, Probleemstelling en doelstellingen

Een bedrijf VS gebaseerde Sport verzamelt tal van privégegevens en andere gegevens. Dit omvat klanten en werknemersgegevens. Het bedrijf blijft in meerdere databases en slaat ze op in verschillende plaatsen in de Azure-omgeving. Naast het Sport apparatuur verkopen, ze ook hosten en beheren van de registratie voor bijzondere Sport gebeurtenissen over de hele wereld, met inbegrip van in de EU en in sommige gevallen de klantgegevens die zij verzamelen medische informatie bevat.

Omdat het bedrijf als host fungeert voor veel internationale bicycling rondleidingen jaarlijks en tijdelijk personeel in locaties wereldwijd heeft, wordt een aantal de gegevenssets behoorlijk groot zijn. Het bedrijf heeft ook developer gebouwd toepassingen die worden gebruikt door klanten en werknemers.

Het bedrijf wil de volgende problemen aanpakken:

- Klanten en werknemers persoonlijke gegevens moeten worden geclassificeerd/onderscheiden van andere gegevens die het bedrijf verzamelt om ervoor te zorgen de juiste toegang en beveiliging.
- De beheerder van de gegevens moet de locatie van persoonlijke gegevens van de klant gemakkelijk ontdekken in verschillende gebieden van de Azure-omgeving.
- Klanten en werknemers persoonlijke gegevens die wordt weergegeven in gedeelde documenten en e-mailberichten moeten worden voorzien om ervoor te zorgen dat deze wordt beveiligd.
- App-ontwikkelaars van het bedrijf moeten een manier om het eenvoudig voor klanten en werknemers persoonlijke gegevens in de web- en mobiele apps zoeken.
- Ontwikkelaars moeten ook hun documentdatabase voor persoonlijke gegevens opvragen.

### <a name="company-goals"></a>Bedrijfsdoelstellingen

- Alle klanten en werknemers persoonlijke gegevens moet gelabeld/aangetekend in Azure Data Catalog kunnen eenvoudig worden gevonden. In het ideale geval zijn klanten en werknemers persoonlijke gegevens gelabeld/aangetekend afzonderlijk.
- Persoonlijke gegevens van klanten en werknemers gebruikersprofielen en werkgegevens die zich in Azure Active Directory moet eenvoudig bevindt.
- Persoonlijke gegevens die zich in meerdere SQL-databases moet gemakkelijk worden opgevraagd. 
- Sommige van de grote gegevenssets van het bedrijf worden beheerd via Azure HDInsight en opgeslagen in Hadoop. Ze moeten worden geïmporteerd in Excel zodat ze kunnen worden opgevraagd voor persoonlijke gegevens.
- Persoonlijke gegevens die worden gedeeld in documenten en e-mailberichten moeten worden geclassificeerd met het label en beveiligd met Azure Information Protection.
- App-ontwikkelaars van het bedrijf moet kunnen klanten en werknemers persoonlijke gegevens in de apps ze hebt gemaakt, die ze met Azure Search doen kunnen detecteren.
- Ontwikkelaars moeten kunnen persoonlijke gegevens in de documentdatabase voor hun niet vinden.

## <a name="azure-active-directory-data-discovery"></a>Azure Active Directory: Gegevens te zoeken

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) van Microsoft cloud-gebaseerde, multitenant directory en identity management-service is. U kunt vinden, klanten en werknemers gebruikersprofielen en gebruikersgegevens werk die persoonlijke gegevens bevatten in uw [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD)-omgeving met behulp van de [Azure-portal](https://portal.azure.com/).

Dit is vooral nuttig zijn als u wilt zoeken of persoonlijke gegevens voor een specifieke gebruiker wijzigen. U kunt ook toevoegen of wijzigen van gebruikersprofiel en werkgegevens. U moet zich aanmelden met een account met globale beheerdersrechten voor de map.

### <a name="how-do-i-locate-or-view-user-profile-and-work-information"></a>Hoe zoeken of gebruikersprofiel weergeven ik informatie over en?

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account met globale beheerdersrechten voor de map.

2. Selecteer **meer services**, voer **gebruikers en groepen** in het tekstvak in en selecteer vervolgens **Enter**.

   ![hoe ik gebruikersprofiel kunnen vinden en werkgegevens](media/how-to-discover-classify-personal-data-azure/user-profile.png)

3. Op de **gebruikers en groepen** blade Selecteer **gebruikers**.

  ![Openen gebruikers en groepen](media/how-to-discover-classify-personal-data-azure/users-groups.png)

4. Op de **gebruikers en groepen - gebruikers** blade, selecteert u een gebruiker in de lijst en selecteer vervolgens het volgende op de blade voor de geselecteerde gebruiker **profiel** om weer te geven gebruikersprofielgegevens die mogelijk persoonlijke gegevens bevatten.

  ![gebruiker selecteren](media/how-to-discover-classify-personal-data-azure/select-user.png)

5. Als u wilt toevoegen of wijzigen van gebruikersprofielgegevens, kunt u doen en selecteer vervolgens in de opdrachtbalk **opslaan.**
6. Selecteer op de blade voor de geselecteerde gebruiker **Info werken** om werk gebruikersgegevens persoonlijke gegevens bevatten mogelijk weer te geven.

 ![werkgegevens weergeven](media/how-to-discover-classify-personal-data-azure/work-info.png)

7. Als u wilt toevoegen of gebruikersinformatie werk wijzigen, kunt u doen en selecteer vervolgens in de opdrachtbalk **opslaan.**

## <a name="azure-sql-database-data-discovery"></a>Azure SQL Database: Gegevens te zoeken

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) is een cloud-database die kan softwareontwikkelaars maken en onderhouden van toepassingen. Persoonlijke gegevens vindt u in [Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) met behulp van de standaard SQL-query's. Azure SQL-elastische query (preview) kan gebruikers op het uitvoeren van query's voor meerdere databases.

Een gedetailleerde [SQL-database](../sql-database/sql-database-technical-overview.md) zelfstudie wordt uitgelegd hoe veel aspecten van het gebruik van een SQL-database, met inbegrip van hoe een bouwen en uitvoeren van query's. Hier volgt een samenvatting van de informatie die beschikbaar zijn in de zelfstudie met koppelingen naar specifieke secties.

### <a name="how-do-i-build-a-sql-database"></a>Hoe maak ik een SQL-database

Er zijn drie manieren om dat te doen:

- Een Azure SQL database kan worden gemaakt in de [Azure-portal](https://portal.azure.com/). In de zelfstudie gebruikt u een specifieke set berekenings- en bronnen binnen een resourcegroep en de logische server. Gebruikt u voorbeeldgegevens van een fictieve bedrijf AdventureWorks. U hebt ook een firewallregel op serverniveau maken. Voor meer informatie over hoe u dit doet, gaat u naar de [maken van een Azure SQL database in de Azure portal](../sql-database/sql-database-get-started-portal.md) zelfstudie.

  ![Azure SQL-Database maken](media/how-to-discover-classify-personal-data-azure/create-database.png)
- Een SQL-database kan ook worden gemaakt in de [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) CLI, een opdrachtregelprogramma op basis van een browser. Het hulpprogramma is beschikbaar in de Azure-portal en rechtstreeks van daaruit kan worden uitgevoerd. In deze zelfstudie maakt u start het hulpprogramma, scriptvariabelen definiëren, een resourcegroep en de logische server maken en een serverfirewallregel configureren. Vervolgens maakt u een database met voorbeeldgegevens. Voor informatie over het maken van de database op deze manier, gaat u naar de [maken van één Azure SQL database met de Azure CLI](../sql-database/sql-database-get-started-cli.md) zelfstudie.

  ![CLIT-zelfstudie](media/how-to-discover-classify-personal-data-azure/cli-tutorial.png)

>[!NOTE]
Azure CLI wordt doorgaans gebruikt door de Linux-beheerders en ontwikkelaars. Sommige gebruikers vinden het gemakkelijker en intuïtievere dan PowerShell, die is uw derde optie.

- Ten slotte kunt u een SQL-database met behulp van PowerShell, een opdrachtregel/script-hulpprogramma gebruikt voor het maken en beheren van Azure en andere bronnen. In deze zelfstudie maakt u start het hulpprogramma, scriptvariabelen definiëren, een resourcegroep en de logische server maken en een serverfirewallregel configureren. Vervolgens maakt u een database met voorbeeldgegevens.

De zelfstudie is de Azure PowerShell-moduleversie 4.0 of hoger vereist. Voer Get-Module - ListAvailable AzureRM uw versie vinden. Als u wilt installeren of upgraden, ziet u Installeer Azure PowerShell-module.

```PowerShell
New-AzureRmSQLDatabase -ResourceGroupName $resourcegroupname `
-ServerName $servername `
-DatabaseName $databasename `
-RequestedServiceObjectiveName "s0"
```

Voor informatie over het maken van de database op deze manier, gaat u naar de [maken van één Azure SQL database met behulp van Powershell](../sql-database/sql-database-get-started-powershell.md) zelfstudie.

>[!Note]
Windows-beheerders hebben vaak gebruik van PowerShell, maar sommige van deze Azure CLI geven de voorkeur.

### <a name="how-do-i-search-for-personal-data-in-sql-database-in-the-azure-portal"></a>Hoe kan ik persoonlijke gegevens in SQL-database in de Azure portal zoeken? **

U kunt het hulpprogramma van de ingebouwde query-editor binnen de Azure portal gebruiken om te zoeken naar persoonlijke gegevens. U Meld u aan het hulpprogramma met uw aanmelding voor SQL server-beheerder en het wachtwoord bij en voer vervolgens een query.

  ![sql via de portal zoeken](media/how-to-discover-classify-personal-data-azure/search-sql-portal.png)

Stap 5 van de zelfstudie toont een van de voorbeeldquery in het Querydeelvenster-editor, maar deze niet ligt de nadruk op persoonlijke of gevoelige gegevens (ook gegevens combineert uit twee tabellen en aliassen voor de bronkolom maakt in de gegevensset die wordt geretourneerd). De volgende schermafbeelding ziet de query uit stap 5, evenals het deelvenster met resultaten die wordt geretourneerd:

  ![queryeditor](media/how-to-discover-classify-personal-data-azure/query-editor.png)

Als uw database MyTable aangeroepen is, wordt een voorbeeldquery voor persoonlijke gegevens bevatten mogelijk naam, sofi-nummer en id-nummer en eruit als volgt:

"Selecteer naam, sofi-nummer, id-nummer van MijnTabel"

Kan de query uit te voeren en vervolgens de resultaten bekijken in de **resultaten** deelvenster.

Voor meer informatie over het opvragen van een SQL-database in de Azure portal, gaat u naar de [query uitvoeren op de SQL-database](../sql-database/sql-database-get-started-portal.md) gedeelte van de zelfstudie.

### <a name="how-do-i-search-for-data-across-multiple-databases"></a>Hoe kan ik zoeken voor gegevens over meerdere databases?

Elastische SQL-query (preview) kunt u meerdere databases en meerdere database-query's uitvoeren en retourneren een enkelvoudig resultaat wordt verkregen. De [overzicht van de zelfstudie](../sql-database/sql-database-elastic-query-overview.md) bevat een gedetailleerde beschrijving van scenario's en het verschil tussen het partitioneren van de verticale en horizontale database wordt uitgelegd. Horizontale partitioneren, heet 'sharding'.

  ![Verticale partities](media/how-to-discover-classify-personal-data-azure/vertical-partition.png)

  ![horizontale partitioneren](media/how-to-discover-classify-personal-data-azure/horizontal.png)

Om te beginnen, gaat u naar de [elastische query overzicht van Azure SQL Database (preview)](../sql-database/sql-database-elastic-query-overview.md) pagina.

#### <a name="power-query-for-importing-azure-hdinsight-hadoop-clusters-data-discovery-for-large-data-sets"></a>Power Query (voor het importeren van Azure HDInsight Hadoop-clusters): gegevens detectie voor grote gegevenssets

Hadoop is een open-source Apache opslag en verwerking service voor grote gegevenssets die worden geanalyseerd en opgeslagen in de Hadoop-clusters. [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) kunnen gebruikers werken met Hadoop-clusters in Azure. Power Query is een Excel-invoegtoepassing die, onder andere helpt gebruikers gegevens uit verschillende bronnen detecteren.

Persoonlijke gegevens die zijn gekoppeld aan de Hadoop-clusters in Azure HDInsight kan worden geïmporteerd in Excel met Power Query. Nadat de gegevens in Excel kunt u een query bij het identificeren ervan.

#### <a name="how-do-i-use-excel-power-query-to-import-hadoop-clusters-in-azure-hdinsight-into-excel"></a>Hoe gebruik Power Query voor Excel te importeren in Azure HDInsight Hadoop-clusters in Excel?

Een zelfstudie voor HDInsight begeleidt u door deze hele proces. Deze vereisten wordt uitgelegd en bevat een koppeling naar een [aan de slag met Azure HDInsight](../hdinsight/hadoop/apache-hadoop-linux-tutorial-get-started.md) zelfstudie. Instructies behandelen Excel 2016 evenals 2010 en 2013 (stappen zijn enigszins verschillen voor de oudere versies van Excel). Als u geen de Power Query voor Excel-invoegtoepassing, de zelfstudie ziet u hoe u deze. U de zelfstudie beginnen in Excel en een Azure Blob storage-account die is gekoppeld aan het cluster hebben.

  ![De query in Excel](media/how-to-discover-classify-personal-data-azure/excel.png)

Voor meer informatie over hoe u dit doet, gaat u naar de [verbinding maken met Excel en Hadoop via Power Query](../hdinsight/hadoop/apache-hadoop-connect-excel-power-query.md) zelfstudie.

Bron: [verbinding maken met Excel en Hadoop via Power Query](../hdinsight/hadoop/apache-hadoop-connect-excel-power-query.md)

## <a name="azure-information-protection-personal-data-classification-for-documents-and-email"></a>Azure Information Protection: persoonsgegevens classificatie van documenten en e-mailadres

[Azure Information Protection](https://www.microsoft.com/cloud-platform/azure-information-protection) kunt toepassen help Azure-klanten labels u wilt classificeren en beveiligen intern of extern gedeelde documenten en e-communicatie. Sommige van deze items kan de klant of werknemer persoonlijke gegevens bevatten. Regels en voorwaarden kunnen worden gedefinieerd automatisch of handmatig, door beheerders of gebruikers. Bijvoorbeeld, als een gebruiker een document met creditcardgegevens opslaat is, ziet hij of zij een label aanbeveling die is geconfigureerd door de beheerder.

### <a name="how-do-i-try-it"></a>Hoe ik het proberen?

Als u Azure Information Protection probeer om te zien wilt als een geschikt is voor uw organisatie kan het zijn, gaat u naar de [Quick Start-zelfstudie](https://docs.microsoft.com/information-protection/get-started/infoprotect-quick-start-tutorial). Deze doorloopt u vijf eenvoudige stappen: van installatie met het configureren van beleid om te zien van classificatie, labels en delen in actie: en minder dan een half uur duren.

### <a name="how-do-i-deploy-it"></a>Hoe kan ik het implementeren?

Als u implementeren van Azure Information Protection voor uw organisatie wilt, gaat u naar de [implementatieschema voor classificatie, labels en beveiliging](https://docs.microsoft.com/information-protection/plan-design/deployment-roadmap).

### <a name="is-there-anything-else-i-should-know"></a>Is er iets anders die ik moet weten?

Voor aanvullende informatie waarmee u bedenken hoe u instelt, gaat u naar de [gereed, stel kunt beveiligen.](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/21/azure-information-protection-ready-set-protect/)
blogbericht. En controleer de informatie meer koppelingen voor meer informatie over Azure Information Protection hieronder vermeld.

## <a name="azure-search-data-discovery-for-developer-apps"></a>Azure Search: detectie van gegevens voor ontwikkelaars apps

[Azure Search](https://azure.microsoft.com/services/search/) is een zoekopdracht cloudoplossing voor ontwikkelaars en biedt een uitgebreide gegevens zoekfunctie voor uw toepassingen. Azure Search kunt u gegevens over de gebruiker gedefinieerde indexen, afkomstig van Azure Cosmo DB, Azure SQL Database, Azure Blob Storage, Azure Table storage of aangepaste klant JSON-gegevens plaatsen. U kunt ook structureren Lucene query's op basis van de Azure Search REST-API om te zoeken naar persoonlijke gegevenstypen of de persoonlijke gegevens van specifieke personen. Functies omvatten zoeken in volledige tekst, vereenvoudigde querysyntaxis en Lucene-querysyntaxis. 

## <a name="how-do-i-use-sql-to-query-data"></a>Hoe gebruik SQL query uitvoeren op gegevens?

Allereerst de basisbeginselen, gaat u naar de [Azure CosmosD DB: een query met behulp van SQL](../cosmos-db/tutorial-query-documentdb.md) zelfstudie. De zelfstudie biedt een voorbeelddocument en twee voorbeeld SQL-query's en resultaten.

Voor meer gedetailleerde informatie over het bouwen van de SQL-query's, gaat u naar [SQL-query's voor Azure Cosmos DB Document DB API.](../cosmos-db/sql-api-sql-query.md)

Als u niet vertrouwd met Azure Cosmos DB bent en wil graag informatie over het maken van een database, een verzameling toevoegen en gegevens toevoegen, gaat u naar de [Azure Cosmos DB: een SQL-API-web-app bouwen](../cosmos-db/create-sql-api-dotnet.md) Quick Start-zelfstudie. Als u dit doen in een andere taal dan .NET, zoals Java of Python, kiest uw voorkeurstaal u eenmaal naar de site.

## <a name="next-steps"></a>Volgende stappen

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50)

[Wat is SQL Database?](../sql-database/sql-database-technical-overview.md)

[SQL Database Query-Editor in Azure-portal beschikbaar] (https://azure.microsoft.com/blog/t-sql-query-editor-in-browser-azure-portal/)

[Wat is Azure Information Protection?](https://docs.microsoft.com/information-protection/understand-explore/what-is-information-protection)

[Wat is Azure Rights Management?](https://docs.microsoft.com/information-protection/understand-explore/what-is-azure-rms)

[Azure Information Protection: Ready, instellen, beveiligen!](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/21/azure-information-protection-ready-set-protect/)
