---
title: Database-per-tenant SaaS-zelfstudie - Azure SQL Database | Microsoft Docs
description: Implementeren en verkennen van de Wingtip Tickets SaaS multitenant-toepassing die laat zien van het patroon van de database-per-tenant en andere SaaS-patronen met behulp van Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: sstein
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 97570a16c7d87a3c8182909b61c04fde30b3fe9b
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58000200"
---
# <a name="deploy-and-explore-a-multitenant-saas-app-that-uses-the-database-per-tenant-pattern-with-sql-database"></a>Implementeren en een multitenant SaaS-app die gebruikmaakt van het patroon van de database-per-tenant met SQL Database verkennen

In deze zelfstudie, implementeren en verkennen van de Wingtip Tickets SaaS-toepassing van het database-per-tenant (Wingtip). De app maakt gebruik van een database-per-tenant-patroon voor het opslaan van de gegevens van meerdere tenants. De app is ontworpen om u te presenteren functies van Azure SQL Database die op een vereenvoudigde manier om in te schakelen van SaaS-scenario's.

Vijf minuten nadat u hebt geselecteerd **implementeren in Azure**, hebt u een multitenant SaaS-toepassing. De app bevat een SQL-database die wordt uitgevoerd in de cloud. De app wordt geïmplementeerd met drie voorbeeldtenants, elk met een eigen database. Alle databases die zijn geïmplementeerd in een elastische SQL-groep. De app wordt geïmplementeerd op uw Azure-abonnement. U hebt volledige toegang om te verkennen en werken met de afzonderlijke onderdelen van de app. De toepassing C#-broncode en het van beheerscripts zijn beschikbaar in de [WingtipTicketsSaaS DbPerTenant GitHub-opslagplaats][github-wingtip-dpt].

In deze zelfstudie komen deze onderwerpen aan bod:

> [!div class="checklist"]
> - Klik hier voor meer informatie over het implementeren van de Wingtip SaaS-toepassing.
> - Waar om op te halen van de toepassingsbron code en scripts.
> - Over de servers, pools en databases waaruit de app.
> - Hoe tenants worden toegewezen aan hun gegevens met de *catalogus*.
> - Klik hier voor meer informatie over het inrichten van een nieuwe tenant.
> - Klik hier voor meer informatie over het bewaken van activiteit in een tenant in de app.

Een [reeks gerelateerde zelfstudies](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials) biedt verschillende ontwerp- en -beheerpatronen voor SaaS verkennen. De zelfstudies bouwen buiten deze initiële implementatie. Wanneer u de zelfstudies, kunt u de bijgeleverde scripts om te zien hoe de verschillende SaaS-patronen worden geïmplementeerd kunt onderzoeken. De scripts laten zien hoe de functies van SQL Database de ontwikkeling van SaaS-toepassingen vereenvoudigen.

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie, zorg ervoor dat Azure PowerShell is geïnstalleerd. Zie [Aan de slag met Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps) voor meer informatie.

## <a name="deploy-the-wingtip-tickets-saas-application"></a>De Wingtip Tickets SaaS-toepassing implementeren

### <a name="plan-the-names"></a>De namen van plan bent

In de stappen van deze sectie bieden u een gebruikerswaarde die wordt gebruikt om ervoor resourcenamen wereldwijd uniek zijn. U ook opgeven een naam op voor de resourcegroep waarin alle resources die zijn gemaakt door een implementatie van de app. We raden voor fictieve personen met de naam Anne Finley:

- **Gebruiker**: *af1* bestaat uit de Anne Finley initialen plus een cijfer. Als u de app op een tweede keer implementeren, gebruikt u een andere waarde. Een voorbeeld is af2.
- **Resourcegroep**: *wingtip-dpt-af1* geeft aan dat dit is de database-per-tenant-app. Toevoegen van de gebruiker de naam af1 voor het correleren van de naam van de resourcegroep met de namen van de resources die deze bevat.

Kies de namen van de nu en schrijf ze op.

### <a name="steps"></a>Stappen

1. Als u de sjabloon van de implementatie van de database-per-tenant Wingtip Tickets SaaS in Azure portal, schakelt **implementeren in Azure**.

   <a href="https://aka.ms/deploywingtipdpt" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

1. Voer waarden in de sjabloon voor de vereiste parameters.

    > [!IMPORTANT]
    > Sommige firewalls verificatie en de server zijn opzettelijk onbeveiligde voor demonstratiedoeleinden te gebruiken. U wordt aangeraden dat u een nieuwe resourcegroep maken. Gebruik geen bestaande resourcegroepen, servers of pools. Gebruik deze toepassing, scripts en alle geïmplementeerde resources niet voor productie. Verwijder deze resourcegroep wanneer u klaar met de toepassing bent om gerelateerde facturering te stoppen.

    - **Resourcegroep**: Selecteer **nieuw**, en geef de unieke naam die u hebt gekozen eerder voor de resourcegroep.
    - **Locatie**: Selecteer een locatie in de vervolgkeuzelijst.
    - **Gebruiker**: Gebruik de gebruiker de naam-waarde die u eerder hebt gekozen.

1. De toepassing implementeren.

    a. Selecteer om de voorwaarden en bepalingen te accepteren.

    b. Selecteer **Aankoop**.

1. Voor het controleren van de status van implementatie, selecteer **meldingen** (het belpictogram aan de rechterkant van het zoekvak). Implementatie van de Wingtip Tickets SaaS-app duurt ongeveer vijf minuten.

   ![Implementatie is voltooid](media/saas-dbpertenant-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-wingtip-tickets-management-scripts"></a>Downloaden en de blokkering opheffen van de Wingtip Tickets-management-scripts

Terwijl de toepassing is geïmplementeerd, moet u de bron-code en scripts downloaden.

> [!IMPORTANT]
> Uitvoerbare inhoud (scripts en DLL's) mogelijk geblokkeerd door Windows als ZIP-bestanden worden gedownload vanuit een externe bron en uitgepakt. Volg de stappen voor het deblokkeren van het ZIP-bestand voordat u de scripts hebt uitgepakt. Opheffen van de blokkering wordt in dat de scripts mogen worden uitgevoerd.

1. Blader naar de [WingtipTicketsSaaS DbPerTenant GitHub-opslagplaats][github-wingtip-dpt].
1. Selecteer **Klonen of downloaden**.
1. Selecteer **ZIP downloaden**, en sla het bestand.
1. Met de rechtermuisknop op de **WingtipTicketsSaaS-DbPerTenant-master.zip** bestand en selecteer vervolgens **eigenschappen**.
1. Op de **algemene** tabblad **opheffen van blokkeringen** > **toepassen**.
1. Selecteer **OK**, en pak de bestanden

Scripts bevinden zich in de... \\WingtipTicketsSaaS-DbPerTenant-master\\map Learning-Modules.

## <a name="update-the-user-configuration-file-for-this-deployment"></a>Het configuratiebestand van de gebruiker voor deze implementatie bijwerken

Voordat u alle scripts uitvoeren, moet u de waarden voor resource-groep en gebruiker in het configuratiebestand van de gebruiker bijgewerkt. Deze variabelen instellen op de waarden die u hebt gebruikt tijdens de implementatie.

1. Open in de PowerShell ISE... \\Learning Modules\\**UserConfig.psm1**
1. Update **ResourceGroupName** en **naam** met de specifieke waarden voor uw implementatie (op regel 10 en 11 alleen).
1. Sla de wijzigingen op.

Deze waarden wordt verwezen in vrijwel elk script.

## <a name="run-the-application"></a>De toepassing uitvoeren

De app worden venues die als host gebeurtenissen fungeren gepresenteerd. Locatietypen bevatten concertzalen, jazzclubs stadions en sport stadions. Wingtip Tickets, worden venues geregistreerd als tenants. Een tenant heeft een locatie die een gemakkelijke manier evenementen en tickets kunnen verkopen aan hun klanten. Elke venue krijgt een aangepaste website om de gebeurtenissen weer te geven en te verkopen van tickets.

In de app krijgt intern elke tenant een SQL-database geïmplementeerd in een elastische SQL-groep.

Een centraal **Events Hub** pagina bevat een lijst met koppelingen naar de tenants in uw implementatie.

1. Gebruik de URL naar de Hub-gebeurtenissen in uw webbrowser te openen: http://events.wingtip-dpt.&lt; gebruiker&gt;. trafficmanager.net. Vervang &lt;gebruiker&gt; met de waarde van de gebruiker van uw implementatie.

    ![Events Hub](media/saas-dbpertenant-get-started-deploy/events-hub.png)

2. Selecteer **Fabrikam Jazz Club** in de Events Hub.

    ![Gebeurtenissen](./media/saas-dbpertenant-get-started-deploy/fabrikam.png)

### <a name="azure-traffic-manager"></a>Azure Traffic Manager

Maakt gebruik van de toepassing Wingtip [*Azure Traffic Manager* ](../traffic-manager/traffic-manager-overview.md) om de verdeling van inkomende aanvragen te beheren. De URL voor toegang tot de pagina gebeurtenissen voor een specifieke tenant maakt gebruik van de volgende indeling:

- http://events.wingtip-dpt.&lt;user&gt;.trafficmanager.net/fabrikamjazzclub

    De onderdelen van de voorgaande indeling worden in de volgende tabel beschreven.

    | URL-onderdeel        | Description       |
    | :-------------- | :---------------- |
    | http://events.wingtip-dpt | De onderdelen van de gebeurtenissen van de Wingtip-app.<br /><br /> *-dpt* onderscheidt de *database-per-tenant* uitvoering van de Wingtip Tickets van andere implementaties. Voorbeelden zijn de *één* app-per-tenant (*-sa*) of *multitenant-database* (*- mt*) implementaties. |
    | .*&lt;user&gt;* | *af1* in het voorbeeld. |
    | .trafficmanager.net/ | Traffic Manager, basis-URL. |
    | fabrikamjazzclub | Hiermee geeft u de tenant met de naam Fabrikam Jazz Club. |
    | &nbsp; | &nbsp; |

- De tenantnaam wordt afgeleid uit de URL door de app.
- Naam van de tenant wordt gebruikt om een sleutel te maken.
- De sleutel wordt gebruikt voor toegang tot de catalogus om op te halen van de locatie van de database van de tenant.
  - De catalogus wordt geïmplementeerd met behulp van *shard-Toewijzingsbeheer*.
- De Events Hub maakt gebruik van uitgebreide metagegevens in de catalogus te maken van de URL van de pagina van de lijst van gebeurtenissen voor elke tenant.

In een productieomgeving, doorgaans maakt u een CNAME-DNS-record [*internetdomein van een bedrijf*](../traffic-manager/traffic-manager-point-internet-domain.md) naar de Traffic Manager-DNS-naam.

> [!NOTE]
> Deze mogelijk niet direct duidelijk wat het gebruik van traffic manager is in deze zelfstudie. Het doel van deze reeks zelfstudies is te presenteren patronen die de schaal van een complexe productie-omgeving kunnen verwerken. Bijvoorbeeld, in dat geval hebt u meerdere web-apps, verdeeld over de hele wereld, CO-locatie met databases en moet u traffic manager om te routeren tussen deze instanties.
Er zijn echter een andere reeks zelfstudies die laat zien van het gebruik van traffic manager de [geo-herstel](saas-dbpertenant-dr-geo-restore.md) en de [geo-replicatie](saas-dbpertenant-dr-geo-replication.md) zelfstudies. In deze zelfstudie wordt traffic manager gebruikt om u te helpen bij het overschakelen naar een exemplaar van het herstel van de SaaS-app in het geval van een regionale onderbreking.

## <a name="start-generating-load-on-the-tenant-databases"></a>Workloads genereren voor de tenant-databases

Nu dat de app is geïmplementeerd, laten we om te werken.

De *Demo-LoadGenerator* PowerShell-script wordt een werkbelasting die wordt uitgevoerd op alle tenantdatabases. De werkelijke belasting van veel SaaS-apps is het sporadisch en onvoorspelbaar. Als u wilt dit type load simuleren, produceert de generator een belasting met willekeurige pieken of pieken van activiteit op elke tenant. De pieken optreden met willekeurige intervallen. Het duurt enkele minuten voor het patroon load te geven. Moet de generator voor ten minste drie of vier minuten voordat u de belasting controleren worden uitgevoerd.

1. Open in de PowerShell ISE, de... \\Learning Modules\\hulpprogramma's voor\\*Demo-LoadGenerator.ps1* script.
2. Druk op F5 op het script uitvoert en de load-generator te starten. Laat de standaardwaarde parameterwaarden nu.
3. Meld u aan bij uw Azure-account en selecteer het abonnement dat u gebruiken wilt, indien nodig.

De load-generator-script wordt gestart een achtergrondtaak voor elke database in de catalogus en wordt het gestopt. Als u de load-generator-script opnieuw uitvoeren, stopt alle achtergrondtaken die worden uitgevoerd voordat er begonnen wordt met nieuwe labels.

### <a name="monitor-the-background-jobs"></a>De achtergrondtaken bewaken

Als u wilt beheren en controleren van de achtergrondtaken, gebruikt u de volgende cmdlets:

- `Get-Job`
- `Receive-Job`
- `Stop-Job`

### <a name="demo-loadgeneratorps1-actions"></a>Demo-LoadGenerator.ps1 acties

*Demo-LoadGenerator.ps1* lijkt op een actieve werkbelasting van de klanttransacties. De volgende stappen beschrijven de reeks acties die *Demo-LoadGenerator.ps1* initieert:

1. *Demo-LoadGenerator.ps1* begint *LoadGenerator.ps1* op de voorgrond is geplaatst.

    - Beide .ps1-bestanden worden opgeslagen onder de mappen Learning Modules\\hulpprogramma's voor\\.

2. *LoadGenerator.ps1* lussen via alle tenant-databases in de catalogus.

3. *LoadGenerator.ps1* een achtergrondtaak van PowerShell voor elke tenantdatabase begint:

    - De achtergrondtaken worden standaard uitgevoerd gedurende 120 minuten.
    - Elke taak een op basis van CPU-belasting zorgt ervoor dat op één tenant-database door te voeren *sp_CpuLoadGenerator*. De intensiteit en de duur van de belasting varieert, afhankelijk van `$DemoScenario`.
    - *sp_CpuLoadGenerator* lussen om een SQL SELECT-instructie die ervoor zorgt een hoog CPU-belasting dat. Het tijdsinterval tussen problemen van de SELECT varieert op basis van de parameterwaarden te maken van een instelbare CPU-belasting. De belastingsniveaus en intervallen zijn ingedeeld als u wilt simuleren realistischer geladen.
    - Deze .sql-bestanden worden opgeslagen onder *WingtipTenantDB\\dbo\\StoredProcedures\\*.

4. Als `$OneTime = $false`, de load-generator begint de achtergrondtaken en vervolgens om uit te voeren. Elke 10 seconden, bewaakt het voor alle nieuwe tenants die zijn ingericht. Als u `$OneTime = $true`, de LoadGenerator begint de achtergrondtaken en vervolgens niet meer werkt op de voorgrond is geplaatst. Voor deze zelfstudie laat u `$OneTime = $false`.

   Ctrl + C of stoppen bewerking Ctrl-Break gebruiken als u wilt stoppen of opnieuw starten van de load-generator.

   Als u de load-generator die worden uitgevoerd in de voorgrond is geplaatst, gebruikt u een ander exemplaar van de PowerShell ISE andere PowerShell-scripts uit te voeren.

&nbsp;

Voordat u met de volgende sectie doorgaat, laat u de load-generator in de status van de taak wordt uitgevoerd.

## <a name="provision-a-new-tenant"></a>Een nieuwe tenant inrichten

De eerste implementatie er drie voorbeeldtenants gemaakt. Nu maken u een andere tenant om te zien van de impact op de geïmplementeerde toepassing. In de app Wingtip, de werkstroom voor het inrichten van nieuwe tenants wordt uitgelegd in de [inrichten en catalogiseren zelfstudie](saas-dbpertenant-provision-and-catalog.md). In deze fase maakt u een nieuwe tenant, waarbij minder dan één minuut.

1. Open een nieuwe PowerShell ISE.
2. Open... \\Learning Modules\Provision and Catalog\\*Demo-ProvisionAndCatalog.ps1*.
3. Druk op F5 om het script uitvoert. Gebruik voorlopig de standaardwaarden.

   > [!NOTE]
   > Veel Wingtip SaaS-scripts gebruiken *$PSScriptRoot* om te bladeren in mappen voor het aanroepen van functies in andere scripts. Deze variabele wordt geëvalueerd wanneer het volledige script is uitgevoerd door op F5 te drukken. Markeren en uitvoeren van een selectie met F8 kunnen leiden tot fouten. Druk op F5 om het uitvoeren van de scripts.

De database van de nieuwe tenant is:

- In een elastische SQL-pool gemaakt.
- Geïnitialiseerd.
- Geregistreerd in de catalogus.

Inrichting is voltooid, de *gebeurtenissen* site van de nieuwe tenant wordt weergegeven in uw browser.

![Nieuwe tenant](./media/saas-dbpertenant-get-started-deploy/red-maple-racing.png)

Vernieuw de Events Hub zodat de nieuwe tenant weergegeven in de lijst.

## <a name="explore-the-servers-pools-and-tenant-databases"></a>Servers, pools en tenant-databases

Nu u bent begonnen met het uitvoeren van een werklast in de verzameling van tenants, laten we bekijken enkele van de resources die zijn geïmplementeerd.

1. In de [Azure-portal](https://portal.azure.com), blader naar de lijst met SQL-servers. Open vervolgens de **catalogus-dpt -&lt;gebruiker&gt;** server.
    - De catalogusserver bevat twee databases: **tenantcatalog** en **basetenantdb** (een sjabloondatabase die voor het maken van nieuwe tenants worden gekopieerd).

   ![Databases](./media/saas-dbpertenant-get-started-deploy/databases.png)

2. Ga terug naar de lijst met SQL-servers.

3. Open de **tenants1-dpt -&lt;gebruiker&gt;** server die de tenantdatabases bevat.

4. Zie de volgende items:

    - Elke tenant-database is een **Standard-elastisch** database in een standaardpool van 50 eDTU.
    - De database Red Maple Racing is de database van de tenant die u eerder hebt ingericht.

   ![Server-databases](./media/saas-dbpertenant-get-started-deploy/server.png)

## <a name="monitor-the-pool"></a>De pool bewaken

Na *LoadGenerator.ps1* uitvoeringen voor enkele minuten, voldoende gegevens beschikbaar moeten zijn om te kijken naar enkele mogelijkheden voor bewaking starten. Deze mogelijkheden zijn ingebouwd in pools en databases.

Blader naar de server **tenants1-dpt -&lt;gebruiker&gt;**, en selecteer **Pool1** om Resourcegebruik voor de groep weer te geven. In de volgende grafieken, wordt de load-generator uitgevoerd gedurende één uur.

   ![Pool bewaken](./media/saas-dbpertenant-get-started-deploy/monitor-pool.png)

- De eerste grafiek met het label **Resourcegebruik**, toont pool-eDTU-gebruik.
- Het tweede diagram toont de eDTU-gebruik van de vijf meest actieve databases in de groep.

De twee grafieken ziet u dat elastische pools en SQL-Database goed geschikt voor werkbelastingen van onvoorspelbare SaaS-toepassingen zijn. De grafieken weergegeven dat vier databases worden elke bursting tot maar liefst 40 edtu's en nog comfortabel alle databases worden ondersteund door een pool van 50 eDTU. De 50 eDTU-groep kan zelfs zwaardere werkbelastingen te ondersteunen. Als de databases worden ingericht als individuele databases, moet elk criterium een S2 (50 DTU) voor de ondersteuning van de pieken. De kosten van vier afzonderlijke S2-databases zijn bijna drie keer de prijs van de groep. In praktijksituaties Voer SQL-Database-klanten maximaal 500 databases in pools van 200 eDTU. Zie voor meer informatie de [zelfstudie over prestatiebewaking](saas-dbpertenant-performance-monitoring.md).

## <a name="additional-resources"></a>Aanvullende resources

- Zie voor meer informatie, extra [zelfstudies die op de database-per-tenant-toepassing Wingtip Tickets SaaS voortbouwen](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials).
- Zie voor meer informatie over elastische pools, [wat is een elastische pool van Azure SQL?](sql-database-elastic-pool.md).
- Zie voor meer informatie over elastische taken, [beheren uitgeschaalde clouddatabases](sql-database-elastic-jobs-overview.md).
- Zie voor meer informatie over multitenant SaaS-toepassingen, [ontwerppatronen voor multitenant SaaS-toepassingen](saas-tenancy-app-design-patterns.md).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> - Klik hier voor meer informatie over het implementeren van de Wingtip Tickets SaaS-toepassing.
> - Over de servers, pools en databases waaruit de app.
> - Hoe tenants worden toegewezen aan hun gegevens met de *catalogus*.
> - Klik hier voor meer informatie over het inrichten van nieuwe tenants.
> - Klik hier voor meer informatie over het gebruik van de groep van toepassingen voor het controleren van activiteit in een tenant weergeven.
> - Klik hier voor meer informatie over het verwijderen van de voorbeeldresources om gerelateerde facturering te stoppen.

Probeer vervolgens de [inrichten en catalogiseren zelfstudie](saas-dbpertenant-provision-and-catalog.md).

<!-- Link references. -->

[github-wingtip-dpt]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant
