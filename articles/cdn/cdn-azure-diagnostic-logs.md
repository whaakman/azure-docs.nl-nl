---
title: Diagnostische logboeken in Azure | Microsoft Docs
description: Klant kunt logboekanalyse voor Azure CDN inschakelen.
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
ms.openlocfilehash: 0baa43977099af9c6c0d9c2e4c03abc121ec279d
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47097003"
---
# <a name="azure-diagnostic-logs"></a>Diagnostische logboeken in Azure

U kunt met diagnostische logboeken in Azure, core analytics bekijken en sla deze in een of meer doelen, met inbegrip van:

 - Azure Storage-account
 - Azure Event Hubs
 - [Log Analytics-werkruimte](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started)
 
Deze functie is beschikbaar op het CDN-eindpunten voor alle Prijscategorieën. 

Diagnostische logboeken van Azure kunnen u eenvoudige metrische gegevens exporteren van uw CDN-eindpunt naar een groot aantal bronnen, zodat u ze in een andere manier gebruiken kunt. U kunt bijvoorbeeld de volgende typen gegevens exporteren doen:

- Exporteer gegevens voor blob-opslag, exporteren naar CSV en grafieken in Excel te genereren.
- Gegevens exporteren naar Event Hubs en correleren met gegevens uit andere Azure-services.
- Gegevens exporteren naar Log Analytics en de weergave gegevens in uw eigen werkruimte van Log Analytics

Het volgende diagram toont een typische CDN core analytics-weergave van gegevens.

![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/01_OMS-workspace.png)

*Afbeelding 1: core analytics-weergave van CDN*

Zie voor meer informatie over diagnostische logboeken [diagnostische logboeken](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="enable-logging-with-the-azure-portal"></a>Logboekregistratie inschakelen met de Azure-portal

Volg deze stappen kunt registreren met CDN core analytics:

Meld u aan bij [Azure Portal](http://portal.azure.com). Als u niet dat u al CDN hebt ingeschakeld voor uw werkstroom [maken van een Azure CDN-profiel en een eindpunt](cdn-create-new-endpoint.md) voordat u doorgaat.

1. In de Azure-portal, gaat u naar **CDN-profiel**.

2. Zoeken naar een CDN-profiel in de Azure-portal, of Selecteer een van uw dashboard. Selecteer vervolgens het CDN-eindpunt waarvoor u wenst te logboeken met diagnostische gegevens inschakelen.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/02_Browse-to-Diagnostics-logs.png)

3. Selecteer **diagnoselogboeken** in de sectie bewaking.

   De **diagnoselogboeken** pagina wordt weergegeven.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/03_Diagnostics-logs-options.png)

### <a name="enable-logging-with-azure-storage"></a>Logboekregistratie inschakelen met Azure Storage

Als u een opslagaccount wilt opslaan van de logboeken, de volgende stappen uit:
    
1. Voor **naam**, voer een naam voor de instellingen diagnostische logboeken.
 
2. Selecteer **archiveren naar een opslagaccount**en selecteer vervolgens **CoreAnalytics**. 

2. Voor **bewaarperiode (dagen)**, kiest u het aantal dagen bewaren. Een bewaarperiode van nul dagen worden de logboeken voor onbepaalde tijd opgeslagen. 

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/04_Diagnostics-logs-storage.png) 

3. Selecteer **opslagaccount**.

    De **selecteert u een opslagaccount** pagina wordt weergegeven.

4. Selecteer een opslagaccount in de vervolgkeuzelijst en selecteer vervolgens **OK**.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/cdn-select-storage-account.png)

5. Nadat u uw instellingen voor diagnostische logboeken hebt aangebracht, selecteert u **opslaan**.

### <a name="logging-with-log-analytics"></a>Logboekregistratie met Log Analytics

Aan de Log Analytics gebruiken voor het opslaan van de logboeken, als volgt te werk:

1. Uit de **diagnoselogboeken** weergeeft, schakelt **verzenden naar Log Analytics**. 

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/05_Ready-to-Configure.png)    

2. Selecteer **configureren** Log Analytics-logboekregistratie configureren. 

   De **OMS-werkruimten** pagina wordt weergegeven.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/06_Choose-workspace.png)

3. Selecteer **nieuwe werkruimte maken**.

    De **OMS-werkruimte** pagina wordt weergegeven.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/07_Create-new.png)

4. Voor **OMS-werkruimte**, voer de naam van een OMS-werkruimte. De naam van de OMS-werkruimte moet uniek zijn en bevatten alleen letters, cijfers en afbreekstreepjes bevatten; spaties en onderstrepingstekens zijn niet toegestaan. 

5. Voor **abonnement**, selecteer een bestaand abonnement in de vervolgkeuzelijst. 

6. Voor **resourcegroep**, maak een nieuwe resourcegroep of Selecteer een bestaande resourcegroep.

7. Voor **locatie**, selecteer een locatie in de lijst.

8. Selecteer **vastmaken aan dashboard** als u wilt opslaan van de configuratie aan uw dashboard. 

9. Selecteer **OK** om de configuratie te voltooien.

10. Nadat uw werkruimte is gemaakt, keert u terug naar de **diagnostische logboeken** pagina. Controleer of de naam van uw nieuwe Log Analytics-werkruimte.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/09_Return-to-logging.png)

11. Selecteer **CoreAnalytics**en selecteer vervolgens **opslaan**.

12. Als u de nieuwe Log Analytics-werkruimte, selecteert u **Core analytics** van de pagina van uw CDN-eindpunt.

    ![Portal - logboeken met diagnostische gegevens](./media/cdn-diagnostics-log/cdn-core-analytics-page.png) 

    Werkruimte van Log Analytics is nu klaar om gegevens te registreren. Om die gegevens gebruiken, moet u een [Log Analytics-oplossing](#consuming-diagnostics-logs-from-a-log-analytics-workspace), bedekt verderop in dit artikel.

Zie voor meer informatie over het logboek gegevens vertragingen [zich gegevens vertragingen](#log-data-delays).

## <a name="enable-logging-with-powershell"></a>Logboekregistratie inschakelen met PowerShell

Het volgende voorbeeld ziet het inschakelen van diagnostische logboeken via de Azure PowerShell-Cmdlets.

### <a name="enabling-diagnostic-logs-in-a-storage-account"></a>Inschakelen van diagnostische logboeken in een storage-account

1. Meld u en selecteer een abonnement:

    Connect-AzureRmAccount 

    SELECT-AzureSubscription - abonnements-id 

2. Om in te schakelen logboeken met diagnostische gegevens in een Storage-account, voer de volgende opdracht:

    ```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Cdn/profiles/{profileName}/endpoints/{endpointName}" -StorageAccountId "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicStorage/storageAccounts/{storageAccountName}" -Enabled $true -Categories CoreAnalytics
    ```

3. Om in te schakelen diagnostische logboeken in Log Analytics-werkruimte, voer de volgende opdracht:

    ```powershell
    Set-AzureRmDiagnosticSetting -ResourceId "/subscriptions/`{subscriptionId}<subscriptionId>
    .<subscriptionName>" -WorkspaceId "/subscriptions/<workspaceId>.<workspaceName>" -Enabled $true -Categories CoreAnalytics 
    ```

## <a name="consuming-diagnostics-logs-from-azure-storage"></a>Diagnostische logboeken uit Azure Storage gebruiken
Deze sectie beschrijft het schema van CDN core analytics, hoe deze binnen een Azure storage-account is georganiseerd en wordt een voorbeeldcode om de logboeken in een CSV-bestand te downloaden.

### <a name="using-microsoft-azure-storage-explorer"></a>Met behulp van Microsoft Azure Storage Explorer
Voordat u krijgt de core analytics-gegevens uit een Azure storage-account tot toegang, moet u eerst een hulpprogramma voor toegang tot de inhoud van een storage-account. Er zijn verschillende hulpprogramma's beschikbaar op de markt, is die het beste de Microsoft Azure Storage Explorer. Zie voor het downloaden van het hulpprogramma [Azure Storage Explorer](http://storageexplorer.com/). Na het downloaden en installeren van de software, moet u deze voor het gebruik van de Azure storage-account dat is geconfigureerd als een doel voor de diagnoselogboeken CDN configureren.

1.  Open **Microsoft Azure Storage Explorer**
2.  Zoek het opslagaccount
3.  Vouw de **Blobcontainers** knooppunt onder dit storage-account.
4.  Selecteer de container met de naam *insights-logs-coreanalytics*.
5.  Weergeven van resultaten in het rechter deelvenster, beginnen met het eerste niveau als *resourceId =*. Doorgaan met het selecteren van elk niveau totdat u het bestand *PT1H.json*. Zie voor een uitleg van het pad [Blob padindeling](cdn-azure-diagnostic-logs.md#blob-path-format).
6.  Elke blob *PT1H.json* bestand vertegenwoordigt de analytics-logboeken voor één uur voor een specifieke CDN-eindpunt of het aangepaste domein.
7.  Het schema van de inhoud van dit JSON-bestand wordt beschreven in de sectie schema van de core analytics-Logboeken.


#### <a name="blob-path-format"></a>De padindeling BLOB

Core analytics logboeken worden elk uur gegenereerd en de gegevens worden verzameld en opgeslagen binnen een enkel Azure-blob als een JSON-nettolading. Omdat het hulpprogramma Storage explorer interpreteert '/' als een mapscheidingsteken en toont de hiërarchie, het pad naar de Azure-blob wordt weergegeven als er een hiërarchische structuur is en de naam van de blob vertegenwoordigt. De naam van de blob volgt de volgende naamconventie gebruikt:   

```resourceId=/SUBSCRIPTIONS/{Subscription Id}/RESOURCEGROUPS/{Resource Group Name}/PROVIDERS/MICROSOFT.CDN/PROFILES/{Profile Name}/ENDPOINTS/{Endpoint Name}/ y={Year}/m={Month}/d={Day}/h={Hour}/m={Minutes}/PT1H.json```

**Beschrijving van de velden:**

|Waarde|Beschrijving|
|-------|---------|
|Abonnements-id    |ID van de Azure-abonnement in Guid-indeling.|
|Naam van brongroep |De naam van de resourcegroep waartoe de CDN-resources behoren.|
|Profielnaam |Naam van het CDN-profiel|
|De naam van eindpunt |Naam van het CDN-eindpunt|
|Jaar|  Weergave van vier cijfers van het jaar, bijvoorbeeld 2017|
|Maand| De weergave van de twee cijfers van het maandnummer. 01 januari =... 12 December =|
|Dag|   Twee cijfers weergave van de dag van de maand|
|PT1H.json| Werkelijke JSON-bestand waarin de analytics-gegevens is opgeslagen|

### <a name="exporting-the-core-analytics-data-to-a-csv-file"></a>De core analytics-gegevens exporteren naar een CSV-bestand

Als u eenvoudig toegang krijgen tot basisanalyse, wordt voorbeeldcode voor een hulpprogramma geleverd. Dit hulpprogramma kunt de JSON-bestanden downloaden naar een bestandsindeling met door komma's gescheiden plat, die kan worden gebruikt om grafieken of andere aggregaties te maken.

Dit is hoe u het hulpprogramma kunt gebruiken:

1.  Ga naar de github-koppeling: [https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv ](https://github.com/Azure-Samples/azure-cdn-samples/tree/master/CoreAnalytics-ExportToCsv)
2.  De code downloaden.
3.  Volg de instructies om te compileren en configureren.
4.  Het hulpprogramma uitvoeren.
5.  Het resulterende CSV-bestand bevat de analytics-gegevens in een eenvoudige, platte hiërarchie.

## <a name="consuming-diagnostics-logs-from-a-log-analytics-workspace"></a>Diagnostische logboeken van Log Analytics-werkruimte gebruiken
Log Analytics is een Azure-service die uw cloud en on-premises omgevingen voor het onderhouden van hun beschikbaarheid en prestaties. De service verzamelt gegevens afkomstig van resources in uw cloud- en on-premises omgevingen en van andere bewakingsprogramma's om analyse over meerdere resources aan te bieden. 

Voor het gebruik van Log Analytics, moet u [logboekregistratie inschakelen](#enable-logging-with-azure-storage) naar de Azure Log Analytics-werkruimte, die eerder in dit artikel wordt besproken.

### <a name="using-the-log-analytics-workspace"></a>Met behulp van de Log Analytics-werkruimte

 Het volgende diagram toont de architectuur van de invoer en uitvoer van de opslagplaats:

![Log Analytics-werkruimte](./media/cdn-diagnostics-log/12_Repo-overview.png)

*Afbeelding 3 - opslagplaats voor Log Analytics*

U kunt de gegevens op verschillende manieren weergeven met behulp van oplossingen voor beheer. U vindt oplossingen uit de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/monitoring-management?page=1&subcategories=management-solutions).

U kunt oplossingen voor installeren via Azure marketplace door te selecteren de **nu downloaden** koppelen aan de onderkant van elke oplossing.

### <a name="add-a-log-analytics-cdn-management-solution"></a>Een beheeroplossing voor Log Analytics-CDN toevoegen

Volg deze stappen voor het toevoegen van een beheeroplossing voor Log Analytics:

1.   Aanmelden bij Azure portal met uw Azure-abonnement en Ga naar uw dashboard.
    ![Azure-dashboard](./media/cdn-diagnostics-log/13_Azure-dashboard.png)

2. In de **nieuw** pagina onder **Marketplace**, selecteer **bewaking en beheer**.

    ![Marketplace](./media/cdn-diagnostics-log/14_Marketplace.png)

3. In de **bewaking en beheer** weergeeft, schakelt **alle**.

    ![Alles bekijken](./media/cdn-diagnostics-log/15_See-all.png)

4. Zoeken naar CDN in het zoekvak in.

    ![Alles bekijken](./media/cdn-diagnostics-log/16_Search-for.png)

5. Selecteer **basisanalyse van Azure CDN**. 

    ![Alles bekijken](./media/cdn-diagnostics-log/17_Core-analytics.png)

6. Nadat u hebt geselecteerd **maken**, wordt u gevraagd een nieuwe Log Analytics-werkruimte maken of gebruik een bestaande resourcegroep. 

    ![Alles bekijken](./media/cdn-diagnostics-log/18_Adding-solution.png)

7. Selecteer de werkruimte die u eerder hebt gemaakt. Vervolgens moet u een automation-account toevoegen.

    ![Alles bekijken](./media/cdn-diagnostics-log/19_Add-automation.png)

8. Het volgende scherm ziet u het formulier van de automation-account dat moet invullen. 

    ![Alles bekijken](./media/cdn-diagnostics-log/20_Automation.png)

9. Als u het automation-account hebt gemaakt, bent u gereed voor het toevoegen van uw oplossing. Selecteer de knop **Create** (Maken).

    ![Alles bekijken](./media/cdn-diagnostics-log/21_Ready.png)

10. Uw oplossing is nu is toegevoegd aan uw werkruimte. Ga terug naar uw Azure portal-dashboard.

    ![Alles bekijken](./media/cdn-diagnostics-log/22_Dashboard.png)

    Selecteer de Log Analytics-werkruimte die u gaat u naar uw werkruimte hebt gemaakt. 

11. Selecteer de **OMS-Portal** tegel voor uw nieuwe oplossing.

    ![Alles bekijken](./media/cdn-diagnostics-log/23_workspace.png)

12. De portal moet er nu uitzien zoals het volgende scherm:

    ![Alles bekijken](./media/cdn-diagnostics-log/24_OMS-solution.png)

    Selecteer een van de tegels om te zien van verschillende weergaven in uw gegevens.

    ![Alles bekijken](./media/cdn-diagnostics-log/25_Interior-view.png)

    U kunt bladeren links of rechts om te zien van verdere tegels voor afzonderlijke weergaven in de gegevens. 

    Selecteer een van de tegels voor meer informatie over uw gegevens.

     ![Alles bekijken](./media/cdn-diagnostics-log/26_Further-detail.png)

### <a name="offers-and-pricing-tiers"></a>Aanbiedingen en Prijscategorieën

U kunt zien, aanbiedingen en Prijscategorieën voor oplossingen voor het beheer [hier](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions#offers-and-pricing-tiers).

### <a name="customizing-views"></a>Weergaven aanpassen

U kunt de weergave aanpassen in uw gegevens met behulp van de **Weergaveontwerper**. Om te beginnen met het ontwerpen, gaat u naar uw Log Analytics-werkruimte en selecteer de **Weergaveontwerper** tegel.

![Designer weergeven](./media/cdn-diagnostics-log/27_Designer.png)

Slepen en neerzetten om de typen van grafieken en vul de gegevens details u wilt analyseren.

![Designer weergeven](./media/cdn-diagnostics-log/28_Designer.png)

    
## <a name="log-data-delays"></a>Logboek gegevens vertragingen

De volgende tabel toont logboek gegevens vertragingen voor **Azure CDN Standard van Microsoft**, **Azure CDN Standard van Akamai**, en **Azure CDN Standard/Premium van Verizon**.

Microsoft log gegevens vertragingen | Verizon logboek gegevens vertragingen | Akamai logboek gegevens vertragingen
--- | --- | ---
1 uur vertraging. | 1 uur vertraging en kan maximaal twee uur om te starten na voltooiing van de eindpunt-doorgifte weergegeven. | 24 uur; vertraagd Als er meer dan 24 uur geleden is gemaakt, duurt het tot 2 uur weergegeven. Als er onlangs is gemaakt, duurt het maximaal 25 uur voor de logboeken om te starten worden weergegeven.

## <a name="diagnostic-log-types-for-cdn-core-analytics"></a>Typen diagnostische logboeken voor basisanalyse van CDN

Microsoft biedt momenteel core analytics Logboeken alleen die metrische gegevens van die HTTP-antwoord statistieken en uitgaand verkeer zoals te zien van het CDN POP's / randen bevatten.

### <a name="core-analytics-metrics-details"></a>Core analytics metrische gegevens
De volgende tabel bevat een overzicht van metrische gegevens beschikbaar zijn in de core analytics-logboeken voor **Azure CDN Standard van Microsoft**, **Azure CDN Standard van Akamai**, en **Azure CDN Standard/Premium van Verizon**. Niet alle metrische gegevens zijn beschikbaar van alle providers, hoewel deze verschillen minimaal zijn. De tabel geeft ook aan of een bepaalde meetwaarde beschikbaar via een provider is. De metrische gegevens zijn beschikbaar voor de CDN-eindpunten die verkeer op deze hebben.


|Gegevens                     | Beschrijving | Microsoft | Verizon | Akamai |
|---------------------------|-------------|-----------|---------|--------|
| RequestCountTotal         | Totaal aantal treffers in de aanvraag tijdens deze periode. | Ja | Ja |Ja |
| RequestCountHttpStatus2xx | Telling van alle aanvragen dat heeft geresulteerd in een 2xx HTTP-code (bijvoorbeeld 200, 202). | Ja | Ja |Ja |
| RequestCountHttpStatus3xx | Telling van alle aanvragen dat heeft geresulteerd in een 3xx HTTP-code (bijvoorbeeld: 300, 302). | Ja | Ja |Ja |
| RequestCountHttpStatus4xx | Telling van alle aanvragen dat heeft geresulteerd in een 4xx HTTP-code (bijvoorbeeld: 400, 404). | Ja | Ja |Ja |
| RequestCountHttpStatus5xx | Telling van alle aanvragen dat heeft geresulteerd in een code van de HTTP-5xx (bijvoorbeeld, 500, 504). | Ja | Ja |Ja |
| RequestCountHttpStatusOthers | Telling van alle andere HTTP-codes (buiten 2xx-5xx). | Ja | Ja |Ja |
| RequestCountHttpStatus200 | Telling van alle aanvragen dat heeft geresulteerd in een HTTP-antwoord voor 200 code. | Ja | Nee  |Ja |
| RequestCountHttpStatus206 | Telling van alle aanvragen dat heeft geresulteerd in een HTTP-antwoord voor 206 code. | Ja | Nee  |Ja |
| RequestCountHttpStatus302 | Telling van alle aanvragen dat heeft geresulteerd in een HTTP-antwoord voor 302-code. | Ja | Nee  |Ja |
| RequestCountHttpStatus304 | Telling van alle aanvragen dat heeft geresulteerd in een HTTP-antwoord voor 304 code. | Ja | Nee  |Ja |
| RequestCountHttpStatus404 | Telling van alle aanvragen dat heeft geresulteerd in een HTTP-antwoord voor 404-code. | Ja | Nee  |Ja |
| RequestCountCacheHit | Telling van alle aanvragen dat heeft geresulteerd in een Cache is bereikt. De asset is rechtstreeks vanuit de pop-server uitgevoerd naar de client. | Ja | Ja | Nee  |
| RequestCountCacheMiss | Telling van alle aanvragen dat heeft geresulteerd in een Cache ontbreekt. Een Cache ontbreekt, betekent dat de asset is niet gevonden op de pop-server die het dichtst bij de client, en daarom is opgehaald van de oorsprong. | Ja | Ja | Nee |
| RequestCountCacheNoCache | Telling van alle aanvragen voor een asset die wordt voorkomen dat in de cache worden opgeslagen vanwege de Gebruikersconfiguratie van een aan de rand. | Ja | Ja | Nee |
| RequestCountCacheUncacheable | Telling van alle aanvragen naar activa die in de cache wordt opgeslagen door de Cache-Control en Expires-koppen, waarin wordt aangegeven voorkomen dat deze moet niet worden opgeslagen in de cache op een pop-server of door de HTTP-client van de asset. | Ja | Ja | Nee |
| RequestCountCacheOthers | Telling van alle aanvragen met de status van de cache niet wordt gedekt door hierboven. | Nee | Ja | Nee  |
| EgressTotal | Uitgaande gegevensoverdracht in GB | Ja |Ja |Ja |
| EgressHttpStatus2xx | Uitgaande gegevensoverdracht * voor antwoorden met 2xx HTTP-statuscodes in GB. | Ja | Ja | Nee  |
| EgressHttpStatus3xx | Uitgaande gegevensoverdracht voor antwoorden met 3xx HTTP-statuscodes in GB. | Ja | Ja | Nee  |
| EgressHttpStatus4xx | Uitgaande gegevensoverdracht voor antwoorden met 4xx HTTP-statuscodes in GB. | Ja | Ja | Nee  |
| EgressHttpStatus5xx | Uitgaande gegevensoverdracht voor antwoorden met 5xx HTTP-statuscodes in GB. | Ja | Ja | Nee |
| EgressHttpStatusOthers | Uitgaande gegevensoverdracht voor antwoorden met andere HTTP-statuscodes in GB. | Ja | Ja | Nee  |
| EgressCacheHit | Voor uitgaande gegevensoverdracht voor antwoorden die werden geleverd rechtstreeks vanuit de CDN-cache op de CDN POP's / randen. | Ja | Ja | Nee |
| EgressCacheMiss. | Uitgaande gegevensoverdracht voor antwoorden die zijn niet gevonden op de dichtstbijzijnde POP-server, en opgehaald uit de oorspronkelijke server. | Ja | Ja | Nee |
| EgressCacheNoCache | Uitgaande gegevensoverdracht voor activa die worden opgeslagen vanwege de Gebruikersconfiguratie van een aan de rand worden voorkomen. | Ja | Ja | Nee |
| EgressCacheUncacheable | Uitgaande gegevensoverdracht voor activa die voorkomen in de cache wordt opgeslagen door van de asset Cache-Control en/of Expires-koppen. Geeft aan dat deze moet niet worden opgeslagen in de cache op een pop-server of door de HTTP-client. | Ja | Ja | Nee |
| EgressCacheOthers | Uitgaande gegevensoverdracht voor andere scenario's met cache. | Nee | Ja | Nee |

* Uitgaande gegevensoverdracht verwijst naar verkeer van CDN POP-servers aan de client wordt geleverd.


### <a name="schema-of-the-core-analytics-logs"></a>Schema van de core analytics-Logboeken 

Alle logboeken worden opgeslagen in de JSON-indeling en elk item heeft tekenreeksvelden op basis van het volgende schema:

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

Waar *tijd* vertegenwoordigt de begintijd van de grens van het uur waarvoor de statistieken is gemeld. Wanneer een metrische waarde wordt niet ondersteund door een CDN-provider, in plaats van een Double-waarde of een geheel getal-waarde, is er een null-waarde. Deze null-waarde geeft aan dat de afwezigheid van een metrische waarde en wijkt af van de waarde 0. Er is een set van deze metrische gegevens per domein geconfigureerd op het eindpunt.

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

* [Diagnostische logboeken in Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Core analytics via de aanvullende portal van Azure CDN](https://docs.microsoft.com/azure/cdn/cdn-analyze-usage-patterns)
* [Azure Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)
* [Azure Log Analytics REST-API](https://docs.microsoft.com/rest/api/loganalytics)







