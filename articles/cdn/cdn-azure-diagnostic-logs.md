---
title: Azure diagnostische logboeken | Microsoft Docs
description: De klant kunt logboekanalyse inschakelen voor Azure CDN.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2018
ms.author: v-deasim
ms.openlocfilehash: 98a7fc5c4607115811e17a7cf6acd4e867663833
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261301"
---
# <a name="azure-diagnostic-logs"></a>Diagnostische logboeken in Azure

U kunt met Azure diagnoselogboeken basisanalyse bekijken en deze opslaan in een of meer bestemmingen, met inbegrip van:

 - Azure-opslagaccount
 - Azure Event Hubs
 - [Log Analytics-werkruimte](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
Deze functie is beschikbaar op de CDN-eindpunten voor alle Prijscategorieën. 

Logboeken met diagnostische gegevens van Azure kunnen u basisgebruik metrische gegevens exporteren van uw CDN-eindpunt naar een groot aantal bronnen, zodat u ze in een aangepaste manier gebruiken kunt. U kunt bijvoorbeeld de volgende soorten gegevens exporteren doen:

- Exporteer gegevens voor blob-opslag, exporteren naar CSV en grafieken genereren in Excel.
- Gegevens exporteren naar Event Hubs en correleren met gegevens van andere Azure-services.
- Gegevens exporteren naar gegevens in uw eigen werkruimte voor logboekanalyse Log Analytics en weergeven

Het volgende diagram toont een typische CDN core analytics weergave van gegevens.

![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/01_OMS-workspace.png)

*Afbeelding 1: CDN core analytics weergeven*

Zie voor meer informatie over diagnostische logboeken [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="enable-logging-with-the-azure-portal"></a>Inschakelen van logboekregistratie met de Azure-portal

Volg deze stappen kunt logboekregistratie met CDN basisanalyse:

Meld u aan bij [Azure Portal](http://portal.azure.com). Als u niet dat u al CDN hebt ingeschakeld voor uw werkstroom [Maak een Azure CDN-profiel en een eindpunt](cdn-create-new-endpoint.md) voordat u doorgaat.

1. Navigeer in de Azure-portal naar **CDN-profiel**.

2. Zoeken naar een CDN-profiel in de Azure portal of Selecteer een van uw dashboard. Selecteer vervolgens het CDN-eindpunt waarvoor u wilt inschakelen van diagnostische logboeken.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. Selecteer **diagnostische logboeken** in de sectie bewaking.

   De **diagnostische logboeken** pagina wordt weergegeven.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a>Logboekregistratie met Azure Storage inschakelen

Een opslagaccount gebruiken voor het opslaan van de logboeken als volgt te werk:
    
1. Voor **naam**, voer een naam voor de instellingen diagnostische logboeken.
 
2. Selecteer **archiveren naar een opslagaccount**, selecteer daarna **CoreAnalytics**. 

2. Voor **bewaartermijn (dagen)**, kiest u het aantal dagen bewaren. Een bewaartermijn van nul dagen worden de logboeken voor onbepaalde tijd opgeslagen. 

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png) 

3. Selecteer **opslagaccount**.

    De **Selecteer een opslagaccount** pagina wordt weergegeven.

4. Selecteer een opslagaccount in de vervolgkeuzelijst en selecteer vervolgens **OK**.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/cdn-select-storage-account.png)

5. Nadat u de instellingen diagnostische logboeken hebt aangebracht, selecteert u **opslaan**.

### <a name="logging-with-log-analytics"></a>Logboekregistratie met logboekanalyse

Log Analytics gebruiken voor het opslaan van de logboeken als volgt te werk:

1. Van de **diagnostische logboeken** pagina **verzenden met logboekanalyse**. 

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. Selecteer **configureren** Log Analytics-logboekregistratie configureren. 

   De **OMS werkruimten** pagina wordt weergegeven.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. Selecteer **Maak nieuwe werkruimte**.

    De **OMS-werkruimte** pagina wordt weergegeven.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/07_Create-new.png)

4. Voor **OMS-werkruimte**, voer de naam van een OMS-werkruimte. Naam van de OMS-werkruimte moet uniek zijn en mag alleen letters, cijfers en afbreekstreepjes; spaties en onderstrepingstekens bevatten, zijn niet toegestaan. 

5. Voor **abonnement**, selecteer een bestaand abonnement in de vervolgkeuzelijst. 

6. Voor **resourcegroep**, een nieuwe resourcegroep maken of een bestaande set selecteren.

7. Voor **locatie**, selecteer een locatie in de lijst.

8. Selecteer **vastmaken aan dashboard** als u wilt opslaan van de configuratie aan uw dashboard. 

9. Selecteer **OK** om de configuratie te voltooien.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/08_Workspace-resource.png)

10. Nadat uw werkruimte is gemaakt, keert u terug naar de **diagnostische logboeken** pagina. Controleer de naam van uw nieuwe werkruimte voor logboekanalyse.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/09_Return-to-logging.png)

11. Selecteer **CoreAnalytics**, selecteer daarna **opslaan**.

12. Als u wilt uw nieuwe werkruimte voor logboekanalyse weergeven, selecteert u **analytics Core** van uw CDN-eindpunt pagina.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/11_OMS-dashboard.png) 

    Uw werkruimte voor logboekanalyse is nu gereed om gegevens te registreren. Om die gegevens gebruiken, moet u een [Log Analytics-oplossing](#consuming-diagnostics-logs-from-a-log-analytics-workspace), gedekte verderop in dit artikel.

Zie voor meer informatie over het logboek gegevens vertragingen [meld gegevens vertragingen](#log-data-delays).

## <a name="enable-logging-with-powershell"></a>Inschakelen van logboekregistratie met PowerShell

Het volgende voorbeeld ziet het inschakelen van diagnostische logboeken via de Azure PowerShell-Cmdlets.

### <a name="enabling-diagnostic-logs-in-a-storage-account"></a>Inschakelen van diagnostische logboeken in een opslagaccount

1. Meld u bij en selecteer een abonnement:

    Verbinding maken met AzureRmAccount 

    SELECT-AzureSubscription - abonnements-id 

2. Om diagnostische logboeken in een opslagaccount, voer de volgende opdracht:

    ```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
    ```

3. Om diagnostische logboeken in een werkruimte voor logboekanalyse, voer de volgende opdracht:

    ```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
    ```

## <a name="consuming-diagnostics-logs-from-azure-storage"></a>Diagnostische logboeken van Azure Storage gebruiken
Deze sectie beschrijft het schema van CDN core analytics, hoe deze binnen een Azure storage-account is ingedeeld en wordt een voorbeeldcode voor het downloaden van de logboeken in een CSV-bestand.

### <a name="using-microsoft-azure-storage-explorer"></a>Met behulp van Microsoft Azure Opslagverkenner
Voordat u de core analytische gegevens vanaf een Azure storage-account openen kunt, moet u eerst een hulpprogramma voor toegang tot de inhoud in een opslagaccount. Er zijn verschillende hulpprogramma's beschikbaar in de markt, is die het wordt aangeraden de Microsoft Azure Storage Explorer. Zie voor het downloaden van het hulpprogramma [Azure Opslagverkenner](http://storageexplorer.com/). Na het downloaden en installeren van de software, configureert u het gebruik van de Azure storage-account dat is geconfigureerd als doel naar de CDN diagnostische logboeken.

1.  Open **Microsoft Azure Opslagverkenner**
2.  Zoek de storage-account
3.  Vouw de **Blob-Containers** knooppunt onder dit opslagaccount.
4.  Selecteer de container met de naam *insights-logboeken-coreanalytics*.
5.  Weergeven van resultaten in het rechterdeelvenster beginnen met het eerste niveau als *resourceId =*. Doorgaan met het selecteren van elk niveau totdat u het bestand *PT1H.json*. Zie de volgende *Blob padindeling* Opmerking voor een uitleg van het pad.
6.  Elke blob *PT1H.json* bestand vertegenwoordigt de logboeken analytics gedurende één uur voor een specifieke CDN-eindpunt of het aangepaste domein.
7.  Het schema van de inhoud van dit JSON-bestand wordt beschreven in de sectie schema van de core analytics-Logboeken.


> [!NOTE]
> **BLOB-padindeling**
> 
> Core analytics logboeken worden elk uur gegenereerd en de gegevens worden verzameld en opgeslagen binnen een enkele Azure blob-als een JSON-nettolading. Omdat het programma voor Storage explorer interpreteert '/' als een mapscheidingsteken en toont de hiërarchie, het pad naar de blob van Azure wordt weergegeven als er een hiërarchische structuur is en vertegenwoordigt de blob-naam. De naam van de blob volgt de volgende naamconventie: 
    
    resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json

**Beschrijving van velden:**

|Waarde|Beschrijving|
|-------|---------|
|Abonnements-id    |ID van het Azure-abonnement in Guid-indeling.|
|Naam van brongroep |Naam van de resourcegroep waartoe het CDN-resources behoren.|
|Profielnaam |Naam van het CDN-profiel|
|Naam van het eindpunt |Naam van het CDN-eindpunt|
|Jaar|  Weergave van vier cijfers van het jaar, bijvoorbeeld 2017|
|Maand| De weergave van de twee cijfers van het nummer van de maand. 01 januari =... 12 December =|
|Dag|   Twee cijfers weergave van de dag van de maand|
|PT1H.json| Werkelijke JSON-bestand waarin de analytische gegevens is opgeslagen|

### <a name="exporting-the-core-analytics-data-to-a-csv-file"></a>De core analytische gegevens exporteren naar een CSV-bestand

Als u eenvoudig toegang krijgen tot basisanalyse, is voorbeeldcode voor een hulpprogramma opgegeven. Dit hulpprogramma kunt downloaden van de JSON-bestanden in een platte door komma's gescheiden bestandsindeling, die kan worden gebruikt om grafieken of andere aggregaties te maken.

Dit is hoe u het hulpprogramma kunt gebruiken:

1.  Ga naar de github-koppeling: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv ](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv)
2.  Download de code.
3.  Volg de instructies voor het compileren en configureren.
4.  Voer het hulpprogramma.
5.  Het resulterende CSV-bestand bevat de analytische gegevens in een eenvoudige platte hiërarchie.

## <a name="consuming-diagnostics-logs-from-a-log-analytics-workspace"></a>Diagnostische logboeken van een werkruimte voor logboekanalyse gebruiken
Log Analytics is een Azure-service die wordt bewaakt uw cloud en on-premises omgevingen voor het onderhouden van de beschikbaarheid en prestaties. De service verzamelt gegevens afkomstig van resources in uw cloud- en on-premises omgevingen en van andere bewakingsprogramma's om analyse over meerdere resources aan te bieden. 

Als u wilt gebruiken Log Analytics, moet u [logboekregistratie inschakelen](#enable-logging-with-azure-storage) naar de Azure Log Analytics-werkruimte die eerder in dit artikel wordt besproken.

### <a name="using-the-log-analytics-workspace"></a>Met behulp van de werkruimte voor logboekanalyse

 Het volgende diagram toont de architectuur van de invoer en uitvoer van de opslagplaats:

![Log Analytics-werkruimte](./media/cdn-diagnostics-log/12_Repo-overview.png)

*Afbeelding 3 - Log Analytics-opslagplaats*

U kunt de gegevens op verschillende manieren weergeven met behulp van oplossingen voor het beheer. Kunt u oplossingen voor het beheer van de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).

Kunt u oplossingen voor het beheer vanuit Azure marketplace installeren met het selecteren van de **nu downloaden** koppeling aan de onderkant van elke oplossing.

### <a name="add-a-log-analytics-cdn-management-solution"></a>Toevoegen van een beheeroplossing Log Analytics CDN

Volg deze stappen voor het toevoegen van een Log Analytics Management-oplossing:

1.   Meld u aan bij de Azure portal met behulp van uw Azure-abonnement en Ga naar uw dashboard.
    ![Azure-dashboard](./media/cdn-diagnostics-log/13_Azure-dashboard.png)

2. In de **nieuw** pagina onder **Marketplace**, selecteer **bewaking + management**.

    ![Marketplace](./media/cdn-diagnostics-log/14_Marketplace.png)

3. In de **bewaking + management** pagina **alle**.

    ![Alles bekijken](./media/cdn-diagnostics-log/15_See-all.png)

4. Zoeken naar de CDN in het zoekvak.

    ![Alles bekijken](./media/cdn-diagnostics-log/16_Search-for.png)

5. Selecteer **Azure CDN basisanalyse**. 

    ![Alles bekijken](./media/cdn-diagnostics-log/17_Core-analytics.png)

6. Nadat u hebt geselecteerd **maken**, wordt u gevraagd te maken van een nieuwe werkruimte voor logboekanalyse of gebruik een bestaande. 

    ![Alles bekijken](./media/cdn-diagnostics-log/18_Adding-solution.png)

7. Selecteer de werkruimte die u hebt gemaakt voordat. Vervolgens moet u een automation-account toevoegen.

    ![Alles bekijken](./media/cdn-diagnostics-log/19_Add-automation.png)

8. Het volgende scherm ziet u het formulier van de automation-account dat moet invullen. 

    ![Alles bekijken](./media/cdn-diagnostics-log/20_Automation.png)

9. Nadat u het automation-account hebt gemaakt, bent u klaar voor het toevoegen van uw oplossing. Selecteer de knop **Create** (Maken).

    ![Alles bekijken](./media/cdn-diagnostics-log/21_Ready.png)

10. Uw oplossing is nu toegevoegd aan uw werkruimte. Ga terug naar uw Azure-portaldashboard.

    ![Alles bekijken](./media/cdn-diagnostics-log/22_Dashboard.png)

    Selecteer de werkruimte voor logboekanalyse die u gaat u naar uw werkruimte hebt gemaakt. 

11. Selecteer de **OMS-Portal** tegel voor uw nieuwe oplossing.

    ![Alles bekijken](./media/cdn-diagnostics-log/23_workspace.png)

12. Uw portal ziet er nu als het volgende scherm:

    ![Alles bekijken](./media/cdn-diagnostics-log/24_OMS-solution.png)

    Selecteer een van de tegels voor een overzicht van verschillende weergaven in uw gegevens.

    ![Alles bekijken](./media/cdn-diagnostics-log/25_Interior-view.png)

    U kunt bladeren links of rechts om te zien van verdere tegels die afzonderlijke weergaven in de gegevens. 

    Selecteer een van de tegels voor meer informatie over uw gegevens.

     ![Alles bekijken](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a>Aanbiedingen en Prijscategorieën

U kunt zien aanbiedingen en Prijscategorieën voor beheeroplossingen voor [hier](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).

### <a name="customizing-views"></a>Weergaven aanpassen

U kunt de weergave aanpassen in uw gegevens met behulp van de **ontwerper**. Om te beginnen ontwerpen, Ga naar de werkruimte voor logboekanalyse en selecteer de **ontwerper** tegel.

![Designer weergeven](./media/cdn-diagnostics-log/27_Designer.png)

Slepen en neerzetten beschrijft de soorten grafieken en vul de gegevens u wilt analyseren.

![Designer weergeven](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a>Logboek gegevens vertragingen

De volgende tabel toont logboek gegevens vertragingen zijn voor **Azure CDN Standard van Microsoft**, **Azure CDN Standard van Akamai**, en **Azure CDN Standard/Premium van Verizon**.

Microsoft logboek gegevens vertragingen | Verizon logboek gegevens vertragingen | Akamai logboek gegevens vertragingen
--- | --- | ---
Uitgesteld 1 uur. | 1 uur vertraging en kan maximaal twee uur om te starten na voltooiing van de endpoint-doorgifte verschijnen. | Vertraging 24 uur; Als deze meer dan 24 uur geleden is gemaakt, duurt het tot 2 uur starten verschijnen. Als u onlangs is gemaakt, kan deze maximaal 25 uur voor de Logboeken starten verschijnen duren.

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a>Diagnostische logboeken typen voor CDN-basisanalyse

Microsoft biedt momenteel core analytics Logboeken alleen die met HTTP-antwoord statistieken en uitgaande zoals gezien vanaf de CDN POP's / randen metrische gegevens bevatten.

### <a name="core-analytics-metrics-details"></a>Core analytics metrische gegevens
De volgende tabel bevat een overzicht van metrische gegevens beschikbaar zijn in de kern van het analytics-logboeken voor **Azure CDN Standard van Microsoft**, **Azure CDN Standard van Akamai**, en **Azure CDN Standard/Premium van Verizon**. Niet alle metrische gegevens zijn beschikbaar in alle providers, hoewel deze verschillen minimaal zijn. De tabel ook geeft aan of een metriek beschikbaar via een provider is. De metrische gegevens zijn beschikbaar voor deze CDN-eindpunten die verkeer op deze hebben.


|Gegevens                     | Beschrijving | Microsoft | Verizon | Akamai |
|---------------------------|-------------|-----------|---------|--------|
| RequestCountTotal         | Totaal aantal treffers in de aanvraag tijdens deze periode. | Ja | Ja |Ja |
| RequestCountHttpStatus2xx | Telling van alle aanvragen dat heeft geresulteerd in een 2xx HTTP-code (bijvoorbeeld, 200, 202). | Ja | Ja |Ja |
| RequestCountHttpStatus3xx | Telling van alle aanvragen dat heeft geresulteerd in een 3xx HTTP-code (bijvoorbeeld, 300, 302). | Ja | Ja |Ja |
| RequestCountHttpStatus4xx | Telling van alle aanvragen dat heeft geresulteerd in een 4xx HTTP-code (bijvoorbeeld, 400, 404). | Ja | Ja |Ja |
| RequestCountHttpStatus5xx | Telling van alle aanvragen dat heeft geresulteerd in een 5xx HTTP-code (bijvoorbeeld, 500, 504). | Ja | Ja |Ja |
| RequestCountHttpStatusOthers | Telling van alle andere HTTP-codes (buiten 2xx-5xx). | Ja | Ja |Ja |
| RequestCountHttpStatus200 | Telling van alle aanvragen dat heeft geresulteerd in een HTTP-antwoord voor 200 code. | Ja | Nee  |Ja |
| RequestCountHttpStatus206 | Telling van alle aanvragen dat heeft geresulteerd in een HTTP-antwoord voor 206 code. | Ja | Nee  |Ja |
| RequestCountHttpStatus302 | Telling van alle aanvragen dat heeft geresulteerd in een HTTP-antwoord voor 302 code. | Ja | Nee  |Ja |
| RequestCountHttpStatus304 | Telling van alle aanvragen dat heeft geresulteerd in een HTTP-antwoord voor 304 code. | Ja | Nee  |Ja |
| RequestCountHttpStatus404 | Telling van alle aanvragen dat heeft geresulteerd in een 404 HTTP-code-antwoord. | Ja | Nee  |Ja |
| RequestCountCacheHit | Telling van alle aanvragen dat heeft geresulteerd in een Cache heeft bereikt. De asset is geleverd rechtstreeks vanuit de pop-server naar de client. | Ja | Ja | Nee  |
| RequestCountCacheMiss | Telling van alle aanvragen dat heeft geresulteerd in een Cache ontbreekt. Een Cache ontbreekt, betekent de asset is niet gevonden op de pop-server die het dichtst bij de client en daarom is opgehaald van de oorsprong. | Ja | Ja | Nee |
| RequestCountCacheNoCache | Telling van alle aanvragen voor een asset die worden opgeslagen vanwege de Gebruikersconfiguratie van een op de rand worden voorkomen. | Ja | Ja | Nee |
| RequestCountCacheUncacheable | Telling van alle aanvragen voor de activa die niet worden opgeslagen door de Cache-Control en Expires-koppen die aangeven dat deze mag niet worden opgeslagen op een pop-server of door de HTTP-client de asset. | Ja | Ja | Nee |
| RequestCountCacheOthers | Telling van alle aanvragen met de status van de cache niet wordt gedekt door hierboven. | Nee | Ja | Nee  |
| EgressTotal | Uitgaande gegevensoverdracht in GB | Ja |Ja |Ja |
| EgressHttpStatus2xx | Uitgaande gegevens overdracht * voor antwoorden met 2xx HTTP-statuscodes in GB. | Ja | Ja | Nee  |
| EgressHttpStatus3xx | Uitgaande gegevensoverdracht voor antwoorden met 3xx HTTP-statuscodes in GB. | Ja | Ja | Nee  |
| EgressHttpStatus4xx | Uitgaande gegevensoverdracht voor antwoorden met 4xx HTTP-statuscodes in GB. | Ja | Ja | Nee  |
| EgressHttpStatus5xx | Uitgaande gegevensoverdracht voor antwoorden met 5xx HTTP-statuscodes in GB. | Ja | Ja | Nee |
| EgressHttpStatusOthers | Uitgaande gegevensoverdracht voor antwoorden met andere HTTP-statuscodes in GB. | Ja | Ja | Nee  |
| EgressCacheHit | Uitgaande gegevensoverdracht voor antwoorden die zijn geleverd rechtstreeks vanaf de CDN-cache op de CDN POP's / randen. | Ja | Ja | Nee |
| EgressCacheMiss. | Uitgaande gegevensoverdracht voor reacties waarbij zijn niet gevonden op de dichtstbijzijnde POP-server en opgehaald uit de bronserver. | Ja | Ja | Nee |
| EgressCacheNoCache | Uitgaande gegevensoverdracht voor bedrijfsmiddelen die niet worden opgeslagen vanwege de Gebruikersconfiguratie van een op de rand. | Ja | Ja | Nee |
| EgressCacheUncacheable | Uitgaande gegevensoverdracht voor bedrijfsmiddelen die niet worden opgeslagen door de asset Cache-Control en/of Expires-koppen. Hiermee wordt aangegeven dat deze mag niet worden opgeslagen op een pop-server of door de HTTP-client. | Ja | Ja | Nee |
| EgressCacheOthers | Uitgaande gegevensoverdracht voor andere scenario's met cache. | Nee | Ja | Nee |

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

Waar *tijd* vertegenwoordigt de starttijd van de uur-grens waarvoor de statistieken is gemeld. Wanneer een waarde wordt niet ondersteund door een CDN-provider, in plaats van een dubbel of integer-waarde is een null-waarde. Deze null-waarde geeft de afwezigheid van een metriek en verschilt van de waarde 0. Er is een set met deze metrische gegevens per domein geconfigureerd op het eindpunt.

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

## <a name="additional-resources"></a>Aanvullende resources

* [Azure diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Basisanalyse via de aanvullende portal Azure CDN](https://docs.microsoft.com/azure/cdn/cdn-analyze-usage-patterns)
* [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)
* [Azure-logboekanalyse REST-API](https://docs.microsoft.com/rest/api/loganalytics)







