---
title: Zelfstudie voor multitenant SaaS - Azure SQL Database | Microsoft Docs
description: Implementeren en de toepassing multitenant SaaS Wingtip, die u laat zien SaaS patronen met Azure SQL Database verkennen.
keywords: zelfstudie sql-database
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: sstein
ms.openlocfilehash: f624485d32901abb5f0d01c86fc30ddaaae261a3
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="deploy-and-explore-a-multi-tenant-application-that-uses-azure-sql-database---wingtip-saas"></a>Implementeren en een multitenant-toepassing die gebruikmaakt van Azure SQL Database - Wingtip SaaS verkennen

In deze zelfstudie, implementeren en Verken de Wingtip SaaS-toepassing. De toepassing maakt gebruik van het SaaS-toepassingspatroon database-per-tenant om zo meerdere tenants te kunnen bedienen. De toepassing is ontworpen om hier functies van Azure SQL Database die inschakelen SaaS-scenario's vereenvoudigen.

Vijf minuten nadat u hieronder op de knop *Distribueren naar Azure* hebt geklikt, beschikt u over een SaaS-toepassing voor meerdere tenants die met behulp van SQL Database wordt uitgevoerd in de cloud. De toepassing wordt geïmplementeerd met drie voorbeeld-tenants, elk met een eigen database, alle geïmplementeerde in een elastische SQL-groep. De app wordt geïmplementeerd op uw Azure-abonnement, zodat u volledige toegang om te verkennen en werken met de afzonderlijke toepassingsonderdelen. De bron en het beheer toepassingsscripts zijn beschikbaar in de WingtipSaaS GitHub-repo.


In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]

> * De Wingtip SaaS-toepassing implementeren
> * Waar de broncode van de toepassing en scripts voor downloaden
> * Informatie over de servers, pools en databases waaruit de app bestaat
> * Hoe tenants zijn toegewezen aan hun gegevens met de *catalogus*
> * Het inrichten van een nieuwe tenant
> * Tenant-activiteit in de app bewaken

Om een beeld te krijgen van verschillende ontwerp- en beheerpatronen voor SaaS-toepassingen, hebben we een [reeks gerelateerde zelfstudies](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials) ontwikkeld die voortbouwt op deze initiële implementatie. Terwijl u de verschillende zelfstudies doorneemt, kunt u de bijgeleverde scripts bestuderen en ontdekken hoe de verschillende SaaS-patronen worden geïmplementeerd. U kunt de scripts in elke zelfstudie stapsgewijs doorlopen om inzicht te krijgen in de verschillende SQL Database-functies die beschikbaar zijn om op een eenvoudige manier SaaS-toepassingen te ontwikkelen.

## <a name="prerequisites"></a>Vereisten

U kunt deze zelfstudie alleen voltooien als aan de volgende vereisten wordt voldaan:

* Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

## <a name="deploy-the-wingtip-saas-application"></a>De Wingtip SaaS-toepassing implementeren

Implementeer de Wingtip SaaS-app:

1. Te klikken op de **implementeren in Azure** knop opent u de Azure portal voor de sjabloon Wingtip SaaS-implementatie. De sjabloon vereist twee parameterwaarden; een naam voor een nieuwe resourcegroep en de naam van een gebruiker die deze implementatie van andere implementaties van de app Wingtip SaaS onderscheidt. De volgende stap biedt details voor het instellen van deze waarden.

   Zorg ervoor dat Let op de exacte waarden die u gebruikt, u hebt ze later naar een configuratiebestand invoeren.

   <a href="http://aka.ms/deploywtpapp" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. Voer de vereiste parameterwaarden in voor de implementatie:

    > [!IMPORTANT]
    > Om het overzichtelijk te houden zijn bepaalde verificatieprocessen weggelaten. Ook zijn de firewalls op servers uitgeschakeld voor deze zelfstudie. **Maak een nieuwe resourcegroep**, en gebruik niet bestaande resourcegroepen, servers of groepen. Gebruik deze toepassing of alle bronnen die wordt gemaakt, niet voor productie. Verwijder deze resourcegroep wanneer u klaar bent om gerelateerde facturering te stoppen.

    * **Resourcegroep** : Selecteer **nieuw** en geef een **naam** voor de resourcegroep. Selecteer een **locatie** uit de vervolgkeuzelijst.
    * **Gebruiker** -resources vereisen namen die globaal uniek zijn. Om ervoor te zorgen uniekheid, elke keer dat u implementeert de toepassing een waarde opgeven om te onderscheiden van de resources die u maakt, van de resources die zijn gemaakt door een andere implementatie van de toepassing Wingtip. Het is raadzaam om een korte naam op te geven voor **Gebruiker**, zoals uw initialen plus een nummer (bijvoorbeeld *bg1*), en die naam te gebruiken in de naam van de resourcegroep (bijvoorbeeld *wingtip-bg1*). De **gebruiker** parameter mag alleen letters, cijfers en afbreekstreepjes (zonder spaties). Voor het eerste en laatste teken moet u een letter of een cijfer gebruiken. Het wordt aanbevolen om alleen kleine letters te gebruiken.


1. **Implementeer de toepassing**.

    * Klik om de voorwaarden en bepalingen te accepteren.
    * Klik op **Kopen**.

1. Controleer de voortgang van de implementatie door te klikken op **Meldingen** (het belpictogram naast het zoekvak). Implementatie van de app Wingtip SaaS duurt ongeveer vijf minuten.

   ![implementatie gelukt](media/sql-database-saas-tutorial/succeeded.png)

## <a name="download-and-unblock-the-wingtip-saas-scripts"></a>Downloaden en de scripts Wingtip SaaS deblokkeren

Download de bron en het beheer scripts tijdens de implementatie van de toepassing.

> [!IMPORTANT]
> Uitvoerbare inhoud (scripts, dll-bestanden) mogelijk geblokkeerd door Windows als zip-bestanden van een externe bron wordt gedownload en uitgepakt. Bij het uitpakken van de scripts uit een zipbestand, volgt u onderstaande stappen voor het deblokkeren van het ZIP-bestand voor het uitpakken. Dit zorgt ervoor dat de scripts mogen worden uitgevoerd.

1. Blader naar [de github-repo Wingtip SaaS](https://github.com/Microsoft/WingtipSaaS).
1. Klik op **klonen of downloaden**.
1. Klik op **ZIP downloaden** en sla het bestand.
1. Met de rechtermuisknop op de **WingtipSaaS master.zip** bestand en selecteer **eigenschappen**.
1. Op de **algemene** tabblad **blokkering**, en klik op **toepassen**.
1. Klik op **OK**.
1. Pak de bestanden.

Scripts bevinden zich in de *... \\WingtipSaaS master\\Learning-Modules* map.

## <a name="update-the-configuration-file-for-this-deployment"></a>Het configuratiebestand voor deze implementatie bijwerken

Voordat u de scripts, stelt u de *resourcegroep* en *gebruiker* in waarden **UserConfig.psm1**. Deze variabelen ingesteld op de waarden die u tijdens de implementatie instellen.

1. Open ...\\Learning Modules\\*UserConfig.psm1* in de *PowerShell ISE*.
1. Update *ResourceGroupName* en *naam* met de specifieke waarden voor uw implementatie (op regels 10 en 11 alleen).
1. De wijzigingen opslaan!

Hier in te stellen gewoon voorkomt u moet deze implementatie-specifieke waarden in elk script bijwerken.

## <a name="run-the-application"></a>De toepassing uitvoeren

In de app worden venues gepresenteerd, zoals concertzalen, jazzclubs en stadions, die bepaalde evenementen organiseren. Plaatsen registreren als klanten (of tenants) van het platform Wingtip voor een eenvoudige manier om een lijst van gebeurtenissen en tickets verkopen. Elke venue krijgt een eigen web-app voor het beheren en vermelden van hun evenementen en het verkopen van tickets, onafhankelijk en gescheiden van andere tenants. Achter de opgehaald elke tenant een SQL-database die is geïmplementeerd in een elastische SQL-groep.

Een centrale **Events Hub** bevat een lijst met tenant-URL's die specifiek zijn voor uw implementatie.

1. Open de _gebeurtenissen Hub_ in uw webbrowser: http://events.wtp.&lt; GEBRUIKER&gt;. trafficmanager.net (vervangen door de naam van uw implementatie gebruiker):

    ![events hub](media/sql-database-saas-tutorial/events-hub.png)

1. Klik in de **Events Hub** op *Fabrikam Jazz Club*.

   ![Gebeurtenissen](./media/sql-database-saas-tutorial/fabrikam.png)


Om te bepalen van de distributie van inkomende aanvragen en het gebruik van de app [ *Azure Traffic Manager*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview). De pagina's van evenementen, die specifiek zijn voor een tenant, werken alleen als de namen van de tenants zijn opgenomen in de URL's. Alle tenant-URL's bevatten de unieke waarde voor *Gebruiker* die u eerder hebt opgegeven en hebben deze notatie: http://events.wtp.&lt;GEBRUIKER&gt;.trafficmanager.net/*fabrikamjazzclub*. De app parseert de tenantnaam uit de URL en gebruikt deze om een sleutel te maken en zo via [*Shard Map Manager*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-scale-shard-map-management) toegang te krijgen tot een catalogus. De sleutel van de catalogus wordt toegewezen aan de locatie van de database van de tenant. De **gebeurtenissen Hub** uitgebreide metagegevens in de catalogus gebruikt voor het ophalen van de tenantnaam die zijn gekoppeld aan elke database een van de lijst met URL's.

In een productieomgeving wordt er meestal een CNAME DNS-record gemaakt om [*het internetdomein van uw bedrijf te laten verwijzen*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-point-internet-domain) naar het profiel van Traffic Manager.

## <a name="start-generating-load-on-the-tenant-databases"></a>Workloads genereren voor de tenant-databases

Nu dat de app is geïmplementeerd, laten we aan de slag! De *Demo LoadGenerator* PowerShell-script wordt gestart van een werkbelasting die wordt uitgevoerd op alle databases van de tenant. De werkelijke belasting van veel SaaS-apps is doorgaans sporadisch onvoorspelbaar. Om te simuleren belasting, levert de generator belasting verdeeld over alle tenants, met willekeurige bursts op elke tenant optreedt op willekeurige intervallen. Hierdoor duurt het enkele minuten voor het patroon load te geven, dus het is raadzaam om te laten de generator uitvoeren voor ten minste drie tot vier minuten voor de bewaking van de belasting.

1. In de *PowerShell ISE*, open de... \\Learning-Modules\\hulpprogramma's\\*Demo LoadGenerator.ps1* script.
1. Druk op **F5** om het script uit te voeren en de load-generator te starten (gebruik voorlopig de standaardwaarden van het script).

> [!IMPORTANT]
> Voor het uitvoeren van andere scripts, een nieuw PowerShell ISE-venster te openen. De load-generator wordt uitgevoerd als een reeks taken in uw lokale PowerShell-sessie. De *Demo LoadGenerator.ps1* script serversysteemstatus van de daadwerkelijke laadbewerking generator-script, dat wordt uitgevoerd als een taak voorgrond plus een reeks achtergrond generatie load-taken. Een load-generator-taak wordt aangeroepen voor elke database die is geregistreerd in de catalogus. De taken worden uitgevoerd in uw lokale PowerShell-sessie, zodat u de PowerShell-sessie sluit alle taken stopt. Als u uw machine onderbreekt, kan load generatie is onderbroken en wordt hervat wanneer u uw machine ontwaken.

Zodra de load-generator generatie load-taken voor elke tenant roept, blijft de voorgrond-taak in een status-taak wordt aangeroepen, waarbij aanvullende achtergrondtaken wordt gestart voor een nieuwe tenants die vervolgens zijn ingericht. U kunt *Ctrl-C* of druk op de *stoppen* klikken om de voorgrond-taak stoppen, maar bestaande achtergrondtaken gaat verder met het genereren van belasting op elke database. Als u wilt bewaken en beheren van de achtergrondtaken, gebruikt u *Get-Job*, *Receive-Job* en *taak stoppen*. Terwijl de voorgrond-taak wordt uitgevoerd, kunt u dezelfde PowerShell-sessie niet gebruiken om uit te voeren van andere scripts. Voor het uitvoeren van andere scripts, een nieuw PowerShell ISE-venster te openen.

Als u de load-generator-sessie opnieuw starten wilt, bijvoorbeeld met andere parameters, u kunt de voorgrond-taak stoppen en voer vervolgens opnieuw de *Demo LoadGenerator.ps1* script. Opnieuw uit te voeren *Demo LoadGenerator.ps1* eerste stopt alle momenteel wordt uitgevoerd, taken en start vervolgens opnieuw de generator, serversysteemstatus van een nieuwe set taken met de huidige parameters.

Laat de load-generator uitgevoerd in de status van de taak wordt aangeroepen op dit moment.


## <a name="provision-a-new-tenant"></a>Een nieuwe tenant inrichten

De eerste implementatie maakt drie voorbeeld tenants, maar we een andere tenant om te zien hoe dit heeft gevolgen voor de gedistribueerde toepassing maken. De werkstroom van de inrichting tenants Wingtip SaaS wordt beschreven in de [inrichten en catalogus zelfstudie](sql-database-saas-tutorial-provision-and-catalog.md). In deze stap maakt maken u snel een nieuwe tenant.

1. Open ...\\Learning Modules\Provision and Catalog\\*Demo-ProvisionAndCatalog.ps1* in de *PowerShell ISE*.
1. Druk op **F5** het script uit te voeren (laat de standaardwaarden voor deze oefening).

   > [!NOTE]
   > Veel Wingtip SaaS-scripts gebruiken *$PSScriptRoot* om toe te staan navigeren door mappen voor het aanroepen van functies in andere scripts. Deze variabele wordt alleen beoordeeld als het script wordt uitgevoerd, door te drukken **F5**.  Syntaxismarkering en een selectie actief (**F8**) kunnen leiden tot fouten, indrukt **F5** wanneer het uitvoeren van scripts.

De nieuwe tenant-database wordt gemaakt in een elastische SQL-groep, geïnitialiseerd en geregistreerd in de catalogus. Na een geslaagde inrichting wordt de nieuwe tenant de ticket verkopen *gebeurtenissen* site wordt weergegeven in uw browser:

![Nieuwe tenant](./media/sql-database-saas-tutorial/red-maple-racing.png)

Vernieuw de *gebeurtenissen Hub* en de nieuwe tenant wordt nu weergegeven in de lijst.


## <a name="explore-the-servers-pools-and-tenant-databases"></a>Servers, pools en tenant-databases

Nu dat u hebt gestart met het uitvoeren van een werklast in de verzameling tenants, bekijken we enkele van de bronnen die zijn geïmplementeerd:

1. In de [Azure-portal](http://portal.azure.com), blader naar de lijst met SQL-servers en open de **catalogus -&lt;gebruiker&gt;**  server. De catalogusserver bevat twee databases: De **tenantcatalog**, en de **basetenantdb** (een lege *gouden* of sjabloondatabase die wordt gekopieerd voor het maken van nieuwe tenants).

   ![databases](./media/sql-database-saas-tutorial/databases.png)

1. Ga terug naar de lijst met SQL-servers en open de **tenants1 -&lt;gebruiker&gt;**  server die de tenant-databases bevat. Elke tenant-database is een _Standard elastische_ database in een standaard 50 eDTU-pool. Ook ziet er is een _Rode esdoorn race_ database, de tenant-database die u eerder hebt ingericht.

   ![server](./media/sql-database-saas-tutorial/server.png)

## <a name="monitor-the-pool"></a>De pool bewaken

Als de load-generator enkele minuten actief is, moeten er voldoende gegevens beschikbaar zijn om eens te kijken naar enkele bewakingsfuncties die zijn ingebouwd in pools en databases.

1. Blader naar de server **tenants1 -&lt;gebruiker&gt;**, en klik op **Pool1** om weer te geven Resourcegebruik voor de pool (de load-generator uitgevoerd in de volgende grafieken voor een uur):

   ![pool bewaken](./media/sql-database-saas-tutorial/monitor-pool.png)

Wat deze twee grafieken mooi laten zien, is dat elastische pools en SQL Database uitermate geschikt zijn voor workloads van SaaS-toepassingen. Vier databases die elk 'bursten' met maximaal 40 eDTU's kunnen makkelijk worden ondersteund in een pool van 50 eDTU. Als ze zijn ingericht als zelfstandige databases, zoals elke moeten een S2 (50 DTU) voor de ondersteuning van de bursts. De kosten van 4 zelfstandige S2 databases zijn bijna 3 keer de prijs van de groep en de toepassingen nog volop mogelijkheden voor veel meer databases. In de praktijk situaties worden SQL-Database klanten momenteel uitgevoerd maximaal 500 databases in 200 eDTU-toepassingen. Zie voor meer informatie de [zelfstudie over prestatiebewaking](sql-database-saas-tutorial-performance-monitoring.md).


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]

> * De Wingtip SaaS-toepassing implementeren
> * Informatie over de servers, pools en databases waaruit de app bestaat
> * Tenants worden via de *catalogus* toegewezen aan hun gegevens
> * Inrichten van nieuwe tenants
> * Poolgebruik bekijken om de activiteit in een tenant te controleren
> * Voorbeeldresources verwijderen om gerelateerde facturering te stoppen

U kunt nu [de zelfstudie over het inrichten en catalogiseren van tenants](sql-database-saas-tutorial-provision-and-catalog.md) volgen als u dat wilt.



## <a name="additional-resources"></a>Aanvullende bronnen

* Aanvullende [zelfstudies die zijn gebaseerd op de Wingtip SaaS-toepassing](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* Informatie over elastische pools vindt u in [*Wat is een elastische pool van Azure SQL?*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool).
* Informatie over elastische taken vindt u in [*Managing scaled-out cloud databases*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview) (Uitgeschaalde clouddatabases beheren).
* Informatie over SaaS-toepassingen voor meerdere tenants vindt u in [*Design patterns for multi-tenant SaaS applications*](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications) (Ontwerppatronen voor SaaS-toepassingen voor meerdere tenants).
