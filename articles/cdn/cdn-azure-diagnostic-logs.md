---
title: Azure diagnostische logboeken | Microsoft Docs
description: De klant kunt logboekanalyse inschakelen voor Azure CDN.
services: cdn
documentationcenter: 
author: 
manager: 
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: v-deasim
ms.openlocfilehash: 7bb4eebc80d1c0fdcb9fb5d0f6bb7aeeeb3cb08d
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-diagnostic-logs"></a>Diagnostische logboeken in Azure

U kunt met Azure diagnoselogboeken basisanalyse bekijken en deze opslaan in een of meer bestemmingen, met inbegrip van:

 - Azure-opslagaccount
 - Azure Event Hubs
 - [De opslagplaats OMS Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
Deze functie is beschikbaar voor alle CDN-eindpunten die behoren tot Verizon (standaard en Premium) en CDN-profielen van Akamai (standaard). 

Logboeken met diagnostische gegevens van Azure kunnen u basisgebruik metrische gegevens exporteren van uw CDN-eindpunt naar een groot aantal bronnen, zodat u ze in een aangepaste manier gebruiken kunt. U kunt bijvoorbeeld de volgende soorten gegevens exporteren doen:

- Exporteer gegevens voor blob-opslag, exporteren naar CSV en grafieken genereren in Excel.
- Gegevens exporteren naar Event Hubs en correleren met gegevens van andere Azure-services.
- Gegevens exporteren om te melden analytics en gegevens in uw eigen OMS-werkruimte weergeven

De volgende afbeelding toont een typische CDN core analytics weergave van gegevens.

![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/01_OMS-workspace.png)

*Afbeelding 1: CDN core analytics weergeven*

Zie voor meer informatie over diagnostische logboeken [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="enable-logging-with-azure-portal"></a>Inschakelen van logboekregistratie met Azure-portal

Volg deze stappen kunt logboekregistratie met CDN basisanalyse:

Meld u aan bij [Azure Portal](http://portal.azure.com). Als u nog niet ingeschakeld voor uw werkstroom CDN hebt [Azure CDN inschakelen](cdn-create-new-endpoint.md) voordat u doorgaat.

1. Navigeer in de portal naar **CDN-profiel**.
2. Selecteer een CDN-profiel en vervolgens het CDN-eindpunt dat u wilt inschakelen **diagnostische logboeken**.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. Selecteer **diagnostische logboeken** in de **bewaking** sectie.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a>Logboekregistratie met Azure Storage inschakelen
    
1. Als u Azure-opslag voor het opslaan van de logboeken, schakelt **archiveren naar een opslagaccount**, selecteer **CoreAnalytics**, en kies vervolgens het aantal dagen bewaren onder **bewaartermijn (dagen)**. Een bewaartermijn van nul dagen worden de logboeken voor onbepaalde tijd opgeslagen. 
2. Voer een naam voor de instelling en klik vervolgens op **opslagaccount**. Nadat u een opslagaccount hebt geselecteerd, klikt u op **opslaan**.

![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png)

*Afbeelding 2 - logboekregistratie met Azure Storage*

### <a name="logging-with-oms-log-analytics"></a>Logboekregistratie met OMS Log Analytics

Gebruik OMS Log Analytics voor het opslaan van de logboeken, als volgt te werk:

1. Van de **diagnostische logboeken** blade Selecteer **verzenden met logboekanalyse**. 

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. Klik op **configureren** log analytics logboekregistratie configureren. In het dialoogvenster OMS werkruimten kunt u een vorige werkruimte selecteren of een nieuwe maken.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. Klik op **Maak nieuwe werkruimte**.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/07_Create-new.png)

4. Voer de naam van een nieuwe OMS-werkruimte. De naam van een OMS-werkruimte moet uniek zijn en mag alleen letters, cijfers en afbreekstreepjes; spaties en onderstrepingstekens bevatten, zijn niet toegestaan. 
5. Selecteer vervolgens een bestaand abonnement, de resourcegroep (nieuwe of bestaande), de locatie en de prijscategorie. U hebt ook de mogelijkheid om deze configuratie aan uw dashboard vastmaken. Klik op **OK** om de configuratie te voltooien.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/08_Workspace-resource.png)

5.  Nadat uw werkruimte is gemaakt, kunt u keert terug naar uw windows-logboeken met diagnostische gegevens. Controleer de naam van uw nieuwe log analytics-werkruimte.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/09_Return-to-logging.png)

    Nadat u de configuratie van analytics hebt ingesteld, moet u controleren dat u hebt geselecteerd **CoreAnalytics**.

6. Klik op **Opslaan**.

7. Uw nieuwe OMS-werkruimte weergeven, gaat u naar uw Azure-portaldashboard en klik op de naam van uw log analytics-werkruimte. Klik op de tegel OMS-Portal om uw werkruimte in de OMS-opslagplaats weer te geven. 

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/11_OMS-dashboard.png) 

    De OMS-opslagplaats is nu gereed om gegevens te registreren. Om die gegevens gebruiken, moet u een [OMS oplossing](#consuming-oms-log-analytics-data), gedekte verderop in dit artikel.

Zie voor meer informatie over het logboek gegevens vertragingen [meld gegevens vertragingen](#log-data-delays).

## <a name="enable-logging-with-powershell"></a>Inschakelen van logboekregistratie met PowerShell

Het volgende voorbeeld ziet het inschakelen van diagnostische logboeken via de Azure PowerShell-Cmdlets.

###<a name="enabling-diagnostic-logs-in-a-storage-account"></a>Inschakelen van diagnostische logboeken in een Opslagaccount

Meldt u zich eerst en selecteer een abonnement:

    Login-AzureRmAccount 

    Select-AzureSubscription -SubscriptionId 


Om te schakelen diagnostische logboeken in een Opslagaccount, gebruikt u deze opdracht:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
```
In de inschakelen diagnostische logboeken in een OMS-werkruimte, gebruikt u deze opdracht:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
```



## <a name="consuming-diagnostics-logs-from-azure-storage"></a>Diagnostische logboeken van Azure Storage gebruiken
Deze sectie beschrijft het schema van CDN core analytics, hoe deze binnen een Azure storage-account is ingedeeld en wordt een voorbeeldcode voor het downloaden van de logboeken in een CSV-bestand.

### <a name="using-microsoft-azure-storage-explorer"></a>Met behulp van Microsoft Azure Opslagverkenner
Voordat u de core analytische gegevens vanaf de Azure Storage-Account openen kunt, moet u eerst een hulpprogramma voor toegang tot de inhoud in een opslagaccount. Er zijn verschillende hulpprogramma's beschikbaar in de markt, is die het wordt aangeraden de Microsoft Azure Storage Explorer. Zie voor het downloaden van het hulpprogramma [Azure Opslagverkenner](http://storageexplorer.com/). Na het downloaden en installeren van de software, configureert u het gebruik van de Azure storage-account dat is geconfigureerd als doel naar de CDN diagnostische logboeken.

1.  Open **Microsoft Azure Opslagverkenner**
2.  Zoek de storage-account
3.  Ga naar de **'Blob-Containers'** knooppunt onder deze storage-account in en vouw het knooppunt
4.  Selecteer de container met de naam **'insights-logboeken-coreanalytics'** en dubbelklik erop
5.  Weergeven van resultaten in het rechterdeelvenster beginnen met het eerste niveau, ziet eruit als **' resourceId = "**. Blijf klikken helemaal tot u het bestand **PT1H.json**. Zie de opmerking hieronder voor uitleg van het pad.
6.  Elke blob **PT1H.json** vertegenwoordigt de logboeken analytics gedurende één uur voor een specifieke CDN-eindpunt of het aangepaste domein.
7.  Het schema van de inhoud van dit JSON-bestand wordt beschreven in de sectie Schema van de core analytics-Logboeken


> [!NOTE]
> **BLOB-padindeling**
> 
> Core analytics logboeken worden elk uur gegenereerd en de gegevens worden verzameld en opgeslagen binnen een enkele Azure blob-als een JSON-nettolading. Omdat het programma voor Storage explorer interpreteert '/' als een mapscheidingsteken en toont de hiërarchie, het pad naar de blob van Azure wordt weergegeven als er een hiërarchische structuur is en vertegenwoordigt de blob-naam. De naam van de blob volgt de volgende naamconventie: 
    
    resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json

**Beschrijving van velden:**

|waarde|description|
|-------|---------|
|Abonnements-id    |ID van het Azure-abonnement in Guid-indeling.|
|Resource |Naam van de naam van de resourcegroep waartoe het CDN-resources behoren.|
|Profielnaam |Naam van het CDN-profiel|
|Naam van het eindpunt |Naam van het CDN-eindpunt|
|Jaar|  4-cijferige representatie van het jaar, bijvoorbeeld 2017|
|Maand| 2 cijfers weergave van het nummer van de maand. 01 januari =... 12 December =|
|Dag|   2 cijfers weergave van de dag van de maand|
|PT1H.JSON| Werkelijke JSON-bestand waarin de analytische gegevens is opgeslagen|

### <a name="exporting-the-core-analytics-data-to-a-csv-file"></a>De core analytische gegevens exporteren naar een CSV-bestand

Als u eenvoudig toegang krijgen tot basisanalyse, is voorbeeldcode voor een hulpprogramma opgegeven. Dit hulpprogramma kunt downloaden van de JSON-bestanden in een platte door komma's gescheiden bestandsindeling, die kan worden gebruikt om te grafieken of andere aggregaties eenvoudig te maken.

Dit is hoe u het hulpprogramma kunt gebruiken:

1.  Ga naar de github-link: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv )
2.  Download de code.
3.  Volg de instructies voor het compileren en configureren.
4.  Voer het hulpprogramma.
5.  Het resulterende CSV-bestand bevat de analytische gegevens in een eenvoudige platte hiërarchie.

## <a name="consuming-diagnostics-logs-from-an-oms-log-analytics-repository"></a>Diagnostische logboeken van een opslagplaats OMS Log Analytics gebruiken
Log Analytics is een service in Operations Management Suite (OMS) die wordt bewaakt uw cloud en on-premises omgevingen voor het onderhouden van de beschikbaarheid en prestaties. De service verzamelt gegevens afkomstig van resources in uw cloud- en on-premises omgevingen en van andere bewakingsprogramma's om analyse over meerdere resources aan te bieden. 

Als u wilt gebruiken Log Analytics, moet u [logboekregistratie inschakelen](#enable-logging-with-azure-storage) in de Azure-logboekanalyse OMS-opslagplaats die eerder in dit artikel wordt besproken.

### <a name="using-the-oms-repository"></a>Met behulp van de OMS-opslagplaats

 Het volgende diagram toont de architectuur van de invoer en uitvoer van de opslagplaats:

![OMS Log Analytics-opslagplaats](./media/cdn-diagnostics-log/12_Repo-overview.png)

*Afbeelding 3 - Log Analytics-opslagplaats*

U kunt de gegevens op verschillende manieren weergeven met behulp van oplossingen voor het beheer. Kunt u oplossingen voor het beheer van de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).

U kunt beheeroplossingen vanuit Azure marketplace installeren door te klikken op de **nu downloaden** koppeling aan de onderkant van elke oplossing.

### <a name="adding-an-oms-cdn-management-solution"></a>Toevoegen van een oplossing voor het beheer van OMS CDN

Volg deze stappen voor het toevoegen van een oplossing voor:

1.   Als u dit nog niet hebt gedaan, meld u aan bij de Azure portal met behulp van uw Azure-abonnement en gaat u naar uw Dashboard.
    ![Azure-Dashboard](./media/cdn-diagnostics-log/13_Azure-dashboard.png)

2. In de **nieuw** blade onder **Marketplace**, selecteer **bewaking + management**.

    ![Marketplace](./media/cdn-diagnostics-log/14_Marketplace.png)

3. In de **bewaking + management** blade, klikt u op **alle**.

    ![Alles bekijken](./media/cdn-diagnostics-log/15_See-all.png)

4.  Zoeken naar de CDN in het zoekvak.

    ![Alles bekijken](./media/cdn-diagnostics-log/16_Search-for.png)

5.  Selecteer **Azure CDN basisanalyse**. 

    ![Alles bekijken](./media/cdn-diagnostics-log/17_Core-analytics.png)

6.  Wanneer u op **maken**, zult u gevraagd een nieuwe OMS-werkruimte maken of gebruiken van een bestaande. 

    ![Alles bekijken](./media/cdn-diagnostics-log/18_Adding-solution.png)

7.  Selecteer de werkruimte die u hebt gemaakt voordat. Vervolgens moet u een automation-account toevoegen.

    ![Alles bekijken](./media/cdn-diagnostics-log/19_Add-automation.png)

8. Het volgende scherm ziet u het formulier van de automation-account dat moet invullen. 

    ![Alles bekijken](./media/cdn-diagnostics-log/20_Automation.png)

9. Nadat u het automation-account hebt gemaakt, bent u klaar voor het toevoegen van uw oplossing. Klik op de knop **Maken**.

    ![Alles bekijken](./media/cdn-diagnostics-log/21_Ready.png)

10. Uw oplossing is nu toegevoegd aan uw werkruimte. Ga terug naar uw Azure-portaldashboard.

    ![Alles bekijken](./media/cdn-diagnostics-log/22_Dashboard.png)

    Klik op de werkruimte voor logboekanalyse die u gaat u naar uw werkruimte hebt gemaakt. 

11. Klik op de **OMS-Portal** tegel voor uw nieuwe oplossing in de OMS-portal.

    ![Alles bekijken](./media/cdn-diagnostics-log/23_workspace.png)

12. De OMS-portal ziet er nu als het volgende scherm:

    ![Alles bekijken](./media/cdn-diagnostics-log/24_OMS-solution.png)

    Klik op een van de tegels voor een overzicht van verschillende weergaven in uw gegevens.

    ![Alles bekijken](./media/cdn-diagnostics-log/25_Interior-view.png)

    U kunt bladeren links of rechts om te zien van verdere tegels die afzonderlijke weergaven in de gegevens. 

    Op een van de tegels te klikken, kunt u meer informatie over uw gegevens.

     ![Alles bekijken](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a>Aanbiedingen en Prijscategorieën

U kunt zien aanbiedingen en Prijscategorieën voor OMS-beheeroplossingen voor [hier](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).

### <a name="customizing-views"></a>Weergaven aanpassen

U kunt de weergave aanpassen in uw gegevens met behulp van de **ontwerper**. Om te beginnen ontwerpen, gaat u naar de OMS-werkruimte en klik op de **ontwerper** tegel.

![Designer weergeven](./media/cdn-diagnostics-log/27_Designer.png)

U kunt slepen en neerzetten van de soorten grafieken en vult u de Gegevensdetails van de die u wilt analyseren.

![Designer weergeven](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a>Logboek gegevens vertragingen

Verizon logboek gegevens vertragingen | Akamai logboek gegevens vertragingen
--- | ---
Logboekgegevens van Verizon is 1 uur vertraging en tot 2 uur duren om te starten na voltooiing van de endpoint-doorgifte verschijnen. | Logboekgegevens Akamai is vertraagd 24 uur; Als deze meer dan 24 uur geleden is gemaakt, duurt het tot 2 uur starten verschijnen. Als u onlangs is gemaakt, kan deze maximaal 25 uur voor de Logboeken starten verschijnen duren.

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a>Diagnostische logboeken typen voor CDN-basisanalyse

Momenteel bieden we alleen core analytics logboeken weergegeven van HTTP-antwoord statistieken en uitgaande zoals gezien vanaf de CDN POP's / randen metrische gegevens bevatten.

### <a name="core-analytics-metrics-details"></a>Core analytics metrische gegevens
De volgende tabel bevat een overzicht van metrische gegevens beschikbaar zijn in de logboeken van de core analytics. Niet alle metrische gegevens zijn beschikbaar in alle providers, hoewel deze verschillen minimaal zijn. De volgende tabel wordt ook weergegeven als een metriek beschikbaar via een provider is. Houd er rekening mee dat de metrische gegevens beschikbaar zijn voor de CDN-eindpunten die verkeer op deze hebben.


|Gegevens                     | Beschrijving   | Verizon  | Akamai 
|---------------------------|---------------|---|---|
| RequestCountTotal         |Totaal aantal treffers aanvraag tijdens deze periode| Ja  |Ja   |
| RequestCountHttpStatus2xx |Telling van alle aanvragen dat heeft geresulteerd in een 2xx HTTP-code (bijvoorbeeld, 200, 202)              | Ja  |Ja   |
| RequestCountHttpStatus3xx | Telling van alle aanvragen dat heeft geresulteerd in een 3xx HTTP-code (bijvoorbeeld, 300, 302)              | Ja  |Ja   |
| RequestCountHttpStatus4xx |Telling van alle aanvragen dat heeft geresulteerd in een 4xx HTTP-code (bijvoorbeeld, 400, 404)               | Ja   |Ja   |
| RequestCountHttpStatus5xx | Telling van alle aanvragen dat heeft geresulteerd in een 5xx HTTP-code (bijvoorbeeld, 500, 504)              | Ja  |Ja   |
| RequestCountHttpStatusOthers |  Telling van alle andere HTTP-codes (buiten 2xx-5xx) | Ja  |Ja   |
| RequestCountHttpStatus200 | Telling van alle aanvragen dat heeft geresulteerd in een HTTP-antwoord voor 200 code              |Nee   |Ja   |
| RequestCountHttpStatus206 | Telling van alle aanvragen dat heeft geresulteerd in een HTTP-antwoord voor 206 code              |Nee   |Ja   |
| RequestCountHttpStatus302 | Telling van alle aanvragen dat heeft geresulteerd in een HTTP-antwoord voor 302 code              |Nee   |Ja   |
| RequestCountHttpStatus304 |  Telling van alle aanvragen dat heeft geresulteerd in een HTTP-antwoord voor 304 code             |Nee   |Ja   |
| RequestCountHttpStatus404 | Telling van alle aanvragen dat heeft geresulteerd in een 404 HTTP-code-antwoord              |Nee   |Ja   |
| RequestCountCacheHit |Telling van alle aanvragen dat heeft geresulteerd in een treffers in de Cache. De asset is geleverd rechtstreeks vanuit de pop-server naar de client.               | Ja  |Nee   |
| RequestCountCacheMiss | Telling van alle aanvragen dat heeft geresulteerd in een Cache ontbreekt. Dit betekent dat de asset is niet gevonden op de pop-server die het dichtst bij de client en daarom is opgehaald uit de oorsprong.              |Ja   | Nee  |
| RequestCountCacheNoCache | Telling van alle aanvragen voor een asset die worden opgeslagen vanwege de Gebruikersconfiguratie van een op de rand worden voorkomen.              |Ja   | Nee  |
| RequestCountCacheUncacheable | Telling van alle aanvragen voor de activa die niet worden opgeslagen door de asset Cache-Control en Expires-koppen die aangeven dat deze mag niet worden opgeslagen op een pop-server of door de HTTP-client                |Ja   |Nee   |
| RequestCountCacheOthers | Telling van alle aanvragen met de status van de cache niet wordt gedekt door hierboven.              |Ja   | Nee  |
| EgressTotal | Uitgaande gegevensoverdracht in GB              |Ja   |Ja   |
| EgressHttpStatus2xx | Uitgaande gegevens overdracht * voor antwoorden met 2xx HTTP-statuscodes in GB            |Ja   |Nee   |
| EgressHttpStatus3xx | Uitgaande gegevensoverdracht voor antwoorden met 3xx HTTP-statuscodes in GB              |Ja   |Nee   |
| EgressHttpStatus4xx | Uitgaande gegevensoverdracht voor antwoorden met 4xx HTTP-statuscodes in GB               |Ja   | Nee  |
| EgressHttpStatus5xx | Uitgaande gegevensoverdracht voor antwoorden met 5xx HTTP-statuscodes in GB               |Ja   |  Nee |
| EgressHttpStatusOthers | Uitgaande gegevensoverdracht voor antwoorden met andere HTTP-statuscodes in GB                |Ja   |Nee   |
| EgressCacheHit |  Uitgaande gegevensoverdracht voor antwoorden die rechtstreeks van de CDN-cache op de CDN POP's / randen zijn verzonden  |Ja   |  Nee |
| EgressCacheMiss | Uitgaande gegevensoverdracht voor reacties waarbij zijn niet gevonden op de dichtstbijzijnde POP-server en opgehaald uit de oorspronkelijke server              |Ja   |  Nee |
| EgressCacheNoCache | Uitgaande gegevensoverdracht voor bedrijfsmiddelen die niet worden opgeslagen vanwege de Gebruikersconfiguratie van een op de rand.                |Ja   |Nee   |
| EgressCacheUncacheable | Uitgaande gegevensoverdracht voor bedrijfsmiddelen die niet worden opgeslagen door de asset Cache-Control en/of Expires-koppen. Hiermee wordt aangegeven dat deze mag niet worden opgeslagen op een pop-server of door de HTTP-client.                   |Ja   | Nee  |
| EgressCacheOthers |  Uitgaande gegevensoverdracht voor andere scenario's met cache.             |Ja   | Nee  |

* Uitgaande gegevensoverdracht verwijst naar verkeer van CDN POP-servers aan de client geleverd.


### <a name="schema-of-the-core-analytics-logs"></a>Schema van de core analytics-Logboeken 

Alle logboeken worden opgeslagen in de JSON-indeling en elk item heeft tekenreeksvelden volgens het volgende schema:

```json
    "records": [
        {
            "time": "2017-04-27T01:00:00",
            "resourceId": "<ARM Resource Id of the CDN Endpoint>",
            "operationName": "Microsoft.Cdn/profiles/endpoints/contentDelivery",
            "category": "CoreAnalytics",
            "properties": {
                "DomainName": "<Name of the domain for which the statistics is reported>",
                "RequestCountTotal": integer value,
                "RequestCountHttpStatus2xx": integer value,
                "RequestCountHttpStatus3xx": integer value,
                "RequestCountHttpStatus4xx": integer value,
                "RequestCountHttpStatus5xx": integer value,
                "RequestCountHttpStatusOthers": integer value,
                "RequestCountHttpStatus200": integer value,
                "RequestCountHttpStatus206": integer value,
                "RequestCountHttpStatus302": integer value,
                "RequestCountHttpStatus304": integer value,
                "RequestCountHttpStatus404": integer value,
                "RequestCountCacheHit": integer value,
                "RequestCountCacheMiss": integer value,
                "RequestCountCacheNoCache": integer value,
                "RequestCountCacheUncacheable": integer value,
                "RequestCountCacheOthers": integer value,
                "EgressTotal": double value,
                "EgressHttpStatus2xx": double value,
                "EgressHttpStatus3xx": double value,
                "EgressHttpStatus4xx": double value,
                "EgressHttpStatus5xx": double value,
                "EgressHttpStatusOthers": double value,
                "EgressCacheHit": double value,
                "EgressCacheMiss": double value,
                "EgressCacheNoCache": double value,
                "EgressCacheUncacheable": double value,
                "EgressCacheOthers": double value,
            }
        }

    ]
}
```

De tijd' ' vertegenwoordigt waarop de starttijd van de uur-grens waarvoor de statistieken is gemeld. Wanneer een waarde wordt niet ondersteund door een CDN-provider, in plaats van een dubbel of integer-waarde is een null-waarde. Deze null-waarde geeft de afwezigheid van een metriek en verschilt van de waarde 0. Er is een set met deze metrische gegevens per domein geconfigureerd op het eindpunt.

Van de Voorbeeldeigenschappen:

```json
{
     "DomainName": "manlingakamaitest2.azureedge.net",
     "RequestCountTotal": 480,
     "RequestCountHttpStatus2xx": 480,
     "RequestCountHttpStatus3xx": 0,
     "RequestCountHttpStatus4xx": 0,
     "RequestCountHttpStatus5xx": 0,
     "RequestCountHttpStatusOthers": 0,
     "RequestCountHttpStatus200": 480,
     "RequestCountHttpStatus206": 0,
     "RequestCountHttpStatus302": 0,
     "RequestCountHttpStatus304": 0,
     "RequestCountHttpStatus404": 0,
     "RequestCountCacheHit": null,
     "RequestCountCacheMiss": null,
     "RequestCountCacheNoCache": null,
     "RequestCountCacheUncacheable": null,
     "RequestCountCacheOthers": null,
     "EgressTotal": 0.09,
     "EgressHttpStatus2xx": null,
     "EgressHttpStatus3xx": null,
     "EgressHttpStatus4xx": null,
     "EgressHttpStatus5xx": null,
     "EgressHttpStatusOthers": null,
     "EgressCacheHit": null,
     "EgressCacheMiss": null,
     "EgressCacheNoCache": null,
     "EgressCacheUncacheable": null,
     "EgressCacheOthers": null
}

```

## <a name="additional-resources"></a>Aanvullende bronnen

* [Azure diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Basisanalyse via de aanvullende portal Azure CDN](https://docs.microsoft.com/azure/cdn/cdn-analyze-usage-patterns)
* [OMS Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)
* [Azure-logboekanalyse REST-API](https://docs.microsoft.com/rest/api/loganalytics)







