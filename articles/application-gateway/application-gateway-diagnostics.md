---
title: Monitor voor toegang tot logboeken, Prestatielogboeken, back-endstatus en metrische gegevens voor Azure Application Gateway
description: Meer informatie over het inschakelen en beheren van toegang en Prestatielogboeken voor Azure Application Gateway
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 3/28/2019
ms.author: amitsriva
ms.openlocfilehash: 367da8a1948b9feb42bc82d85762ae314fe165a0
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58620873"
---
# <a name="back-end-health-diagnostic-logs-and-metrics-for-application-gateway"></a>Back-endstatus, diagnostische logboeken en metrische gegevens voor Application Gateway

Met behulp van Azure Application Gateway, kunt u resources kunt controleren op de volgende manieren:

* [Back-endstatus](#back-end-health): Application Gateway biedt de mogelijkheid om te controleren van de status van de servers in de back-end-adresgroepen via Azure portal en PowerShell. U vindt hier ook de status van de back-end-pools tot en met de prestaties van diagnostische logboeken.

* [Logs](#diagnostic-logging): Logboeken toestaan voor prestaties, toegang en andere gegevens worden opgeslagen of gebruikt vanaf een resource voor bewakingsdoeleinden.

* [Metrische gegevens](#metrics): Application Gateway biedt momenteel zeven metrische gegevens om prestatiemeteritems weer te geven.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="back-end-health"></a>Back-endstatus

Application Gateway biedt de mogelijkheid om te controleren van de status van de afzonderlijke leden van de back-end-adresgroepen via de portal, PowerShell en de opdrachtregelinterface (CLI). U vindt hier ook een health geaggregeerde overzicht van back-end-pools tot en met de prestaties van diagnostische logboeken. 

Het back-end-statusrapport geeft de uitvoer van de statustest van Application Gateway naar de back-end-exemplaren. Bij het scannen is voltooid en de back-end kan verkeer ontvangen, als in orde wordt beschouwd. Anders wordt deze als slecht beschouwd.

> [!IMPORTANT]
> Als er een netwerkbeveiligingsgroep (NSG) voor een Application Gateway-subnet is, opent u poortbereiken 65503 65534 op de Application Gateway-subnet voor binnenkomend verkeer. Dit poortbereik is vereist voor communicatie met Azure-infrastructuur. Ze zijn beveiligd (vergrendeld) met Azure-certificaten. Zonder de juiste certificaten kunnen zich externe entiteiten, inclusief de klanten van deze gateways, niet kunnen initiëren wijzigingen op de eindpunten.


### <a name="view-back-end-health-through-the-portal"></a>Status van de back-end via de portal bekijken

In de portal, wordt automatisch back-endstatus verstrekt. Selecteer in een bestaande toepassingsgateway **bewaking** > **back-endstatus**. 

Elk lid van de back-endpool wordt vermeld op deze pagina (of het is een NIC, IP- of FQDN-naam). De naam van de back-end-pool, poort, de naam van een back-end HTTP-instellingen en status van worden weergegeven. Geldige waarden voor de integriteitsstatus van de zijn **orde**, **niet in orde**, en **onbekende**.

> [!NOTE]
> Als u de status van een back-endstatus van ziet **onbekende**, zorg ervoor dat de toegang tot de back-end niet wordt geblokkeerd door een NSG-regel, een door de gebruiker gedefinieerde route (UDR) of een aangepaste DNS-server in het virtuele netwerk.

![Back-endstatus][10]

### <a name="view-back-end-health-through-powershell"></a>Weergave back-endstatus via PowerShell

De volgende PowerShell-code laat zien hoe u back-endstatus weergeven met behulp van de `Get-AzApplicationGatewayBackendHealth` cmdlet:

```powershell
Get-AzApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

### <a name="view-back-end-health-through-azure-cli"></a>Weergave back-endstatus via Azure CLI

```azurecli
az network application-gateway show-backend-health --resource-group AdatumAppGatewayRG --name AdatumAppGateway
```

### <a name="results"></a>Resultaten

Het volgende codefragment toont een voorbeeld van het antwoord:

```json
{
"BackendAddressPool": {
    "Id": "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/appGatewayBackendPool"
},
"BackendHttpSettingsCollection": [
    {
    "BackendHttpSettings": {
        "Id": "/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings"
    },
    "Servers": [
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        },
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        }
    ]
    }
]
}
```

## <a name="diagnostic-logging"></a>Diagnostische logboeken

U kunt verschillende soorten logboeken in Azure gebruiken om te beheren en problemen oplossen Toepassingsgateways. Via de portal kunt u toegang verkrijgen tot sommige van deze logboeken. Alle logboeken kunnen worden opgehaald uit Azure Blob-opslag en bekeken in verschillende hulpprogramma's, zoals [logboeken van Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md), Excel en Power BI. U kunt meer informatie over de verschillende typen logboeken in de volgende lijst:

* **Activiteitenlogboek**: U kunt [Azure-activiteitenlogboeken](../monitoring-and-diagnostics/insights-debugging-with-events.md) (voorheen bekend als de operationele logboeken en auditlogboeken) om alle bewerkingen die worden verzonden naar uw Azure-abonnement en hun status weer te geven. Activiteitenlogboekitems worden standaard verzameld en kunnen in de Azure-portal worden bekeken.
* **Toegangslogboek**: U kunt dit logboek patronen voor databasetoegang van Application Gateway weergeven en analyseren van belangrijke informatie. Dit omvat van de oproepende functie IP, aangevraagde URL, wachttijd van het antwoord, retourcode en bytes in en uit. Aanmeldgegevens voor de toegang is verzameld om de 300 seconden. Dit logboek bevat één record per exemplaar van Application Gateway. De toepassingsgateway-exemplaar wordt geïdentificeerd door de eigenschap instanceId.
* **Prestatielogboek**: U kunt dit logboek gebruiken om weer te geven hoe Application Gateway-instanties worden uitgevoerd. Dit logboek bevat informatie over de prestaties voor elk exemplaar, met inbegrip van totaal aantal aanvragen dat plaatsvindt, doorvoer in bytes, totaal aantal aanvragen dat plaatsvindt, aantal mislukte aanvragen en het aantal back-end-exemplaren in orde is en niet in orde. Een prestatielogboek worden verzameld van elke 60 seconden.
* **Firewall-logboek**: U kunt dit logboek gebruiken om de aanvragen die zijn geregistreerd via detectie of preventie modus van een application gateway die is geconfigureerd met de web application firewall weer te geven.

> [!NOTE]
> Logboeken zijn alleen beschikbaar voor resources die zijn geïmplementeerd in het Azure Resource Manager-implementatiemodel. U kunt Logboeken niet gebruiken voor resources in het klassieke implementatiemodel. Zie voor een beter begrip van de twee modellen, de [Understanding Resource Manager-implementatie en klassieke implementatie](../azure-resource-manager/resource-manager-deployment-model.md) artikel.

U hebt drie opties voor het opslaan van uw logboeken:

* **Opslagaccount**: Storage-accounts zijn beste worden gebruikt voor Logboeken als Logboeken worden opgeslagen voor een langere duur en gecontroleerd wanneer dit nodig is.
* **Eventhubs**: Eventhubs zijn een goede optie voor het integreren met andere security information en event management (SEIM) hulpprogramma's voor waarschuwingen over uw resources.
* **Logboeken in Azure Monitor**: Logboeken in Azure Monitor is best gebruikt voor algemene realtime-controle van uw toepassing of trends kijken.

### <a name="enable-logging-through-powershell"></a>Schakel logboekregistratie in via PowerShell

Activiteitenlogboekregistratie is automatisch ingeschakeld voor elke Resource Manager-resource. U moet toegang en prestaties logboekregistratie om te beginnen met het verzamelen van de gegevens beschikbaar zijn via deze logboeken inschakelen. Als u wilt logboekregistratie inschakelen, gebruikt u de volgende stappen uit:

1. Noteer de resource-ID van uw opslagaccount waar de logboekgegevens worden opgeslagen. Deze waarde is van het formulier: /subscriptions/\<subscriptionId\>/resourceGroups/\<groepsnaam voor accountresources\>/providers/Microsoft.Storage/storageAccounts/\<opslagaccountnaam\>. U kunt elk opslagaccount in uw abonnement gebruiken. U kunt de Azure-portal gebruiken om deze informatie te vinden.

    ![-Portal: resource-ID voor storage-account](./media/application-gateway-diagnostics/diagnostics1.png)

2. Houd er rekening mee resource-ID van de toepassingsgateway waarvoor logboekregistratie is ingeschakeld. Deze waarde is van het formulier: /subscriptions/\<subscriptionId\>/resourceGroups/\<groepsnaam voor accountresources\>/providers/Microsoft.Network/applicationGateways/\<gateway toepassingsnaam \>. U kunt de portal gebruiken om deze informatie te vinden.

    ![-Portal: resource-ID voor application gateway](./media/application-gateway-diagnostics/diagnostics2.png)

3. Schakel diagnostische logboekregistratie in door de volgende PowerShell-cmdlet te gebruiken:

    ```powershell
    Set-AzDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```
    
> [!TIP] 
>Activiteitenlogboeken vereisen geen een afzonderlijk opslagaccount. Voor het gebruik van opslag voor toegangs- en prestatielogboeken worden servicekosten in rekening gebracht.

### <a name="enable-logging-through-the-azure-portal"></a>Logboekregistratie inschakelen via de Azure-portal

1. Uw resource gevonden in de Azure-portal en selecteer **diagnostische instellingen**.

   Voor Application Gateway zijn de drie logboeken beschikbaar:

   * Toegangslogboek
   * Logboekbestand voor prestaties
   * Firewall-logboek

2. Selecteer eerst het verzamelen van gegevens **diagnostische gegevens inschakelen**.

   ![Diagnostische gegevens inschakelen][1]

3. Op de pagina **Diagnostische instellingen** staan de instellingen voor de diagnostische logboeken. In dit voorbeeld worden de logboeken met Log Analytics opgeslagen. U kunt ook Event Hubs en een opslagaccount gebruiken om de diagnostische logboeken op te slaan.

   ![Het configuratieproces starten][2]

5. Typ een naam voor de instellingen, bevestig de instellingen en selecteer **opslaan**.

### <a name="activity-log"></a>Activiteitenlogboek

Standaard genereert Azure het activiteitenlogboek. De logboeken worden gedurende 90 dagen bewaard in de logboeken voor Azure store. Meer informatie over deze logboeken vindt de [gebeurtenissen en activiteitenlogboek bekijken](../monitoring-and-diagnostics/insights-debugging-with-events.md) artikel.

### <a name="access-log"></a>Toegangslogboek

De access-logboek is gegenereerd, alleen als u deze op elk toepassingsgateway-exemplaar, zoals beschreven in de voorgaande stappen hebt ingeschakeld. De gegevens worden opgeslagen in de storage-account dat u hebt opgegeven toen u de logboekregistratie hebt ingeschakeld. Elke toegang van Application Gateway is vastgelegd in JSON-indeling, zoals wordt weergegeven in het volgende voorbeeld:


|Value  |Description  |
|---------|---------|
|instanceId     | Toepassingsgateway-exemplaar dat de aanvraag wordt uitgevoerd.        |
|clientIP     | Oorspronkelijke IP-adres voor de aanvraag.        |
|clientPort     | Oorspronkelijke poort voor de aanvraag.       |
|HttpMethod     | HTTP-methode die wordt gebruikt door de aanvraag.       |
|requestUri     | De URI van de aanvraag ontvangen.        |
|RequestQuery     | **Server-Routed**: Back-end-pool-exemplaar dat de aanvraag is verzonden.</br>**X-AzureApplicationGateway-LOG-ID**: Correlatie-ID die wordt gebruikt voor de aanvraag. Het kan worden gebruikt voor het oplossen van problemen met verkeer op de back-endservers. </br>**SERVER-STATUS**: HTTP-responscode die Application Gateway van de back-end ontvangen.       |
|UserAgent     | Gebruikersagent van de header van de HTTP-aanvraag.        |
|httpStatus     | HTTP-statuscode is geretourneerd naar de client van Application Gateway.       |
|httpVersion     | HTTP-versie van de aanvraag.        |
|receivedBytes     | Grootte van het pakket ontvangen, in bytes.        |
|sentBytes| Grootte van het pakket dat is verzonden, in bytes.|
|timeTaken| De lengte van de tijd (in milliseconden) die het duurt voordat een aanvraag om te worden verwerkt en de reactie wordt verzonden. Dit wordt berekend als het interval van de tijd die Application Gateway ontvangt wanneer de eerste byte van een HTTP-aanvraag naar de tijd waarop de bewerking is voltooid voor het verzenden van het antwoord. Het is belangrijk te weten dat het veld tijd meestal bevat de tijd die de aanvraag en respons pakketten worden overgebracht via het netwerk. |
|sslEnabled| Communicatie met de back-end-adresgroepen gebruikt of SSL. Geldige waarden zijn in- en uitschakelen.|
```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/PEERINGTEST/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayAccess",
    "time": "2017-04-26T19:27:38Z",
    "category": "ApplicationGatewayAccessLog",
    "properties": {
        "instanceId": "ApplicationGatewayRole_IN_0",
        "clientIP": "191.96.249.97",
        "clientPort": 46886,
        "httpMethod": "GET",
        "requestUri": "/phpmyadmin/scripts/setup.php",
        "requestQuery": "X-AzureApplicationGateway-CACHE-HIT=0&SERVER-ROUTED=10.4.0.4&X-AzureApplicationGateway-LOG-ID=874f1f0f-6807-41c9-b7bc-f3cfa74aa0b1&SERVER-STATUS=404",
        "userAgent": "-",
        "httpStatus": 404,
        "httpVersion": "HTTP/1.0",
        "receivedBytes": 65,
        "sentBytes": 553,
        "timeTaken": 205,
        "sslEnabled": "off"
    }
}
```

### <a name="performance-log"></a>Logboekbestand voor prestaties

Het logboekbestand voor prestaties is gegenereerd, alleen als u deze op elk toepassingsgateway-exemplaar, zoals beschreven in de voorgaande stappen hebt ingeschakeld. De gegevens worden opgeslagen in de storage-account dat u hebt opgegeven toen u de logboekregistratie hebt ingeschakeld. De prestatielogboekgegevens wordt gegenereerd in intervallen van 1 minuut. De volgende gegevens worden geregistreerd:


|Value  |Description  |
|---------|---------|
|instanceId     |  Toepassingsgateway-exemplaar waarvoor gegevens wordt gegenereerd. Er is één rij per exemplaar voor een toepassingsgateway met meerdere instanties.        |
|healthyHostCount     | Het aantal veilige hosts in de back-endpool.        |
|unHealthyHostCount     | Aantal slechte hosts in de back-endpool.        |
|requestCount     | Het aantal aanvragen.        |
|latentie | Gemiddelde latentie (in milliseconden) van aanvragen van het exemplaar naar de back-end die een registratiesysteem de aanvragen vormt. |
|failedRequestCount| Het aantal mislukte aanvragen.|
|Doorvoer| Gemiddelde doorvoersnelheid sinds de laatste logboek, gemeten in bytes per seconde.|

```json
{
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
    "operationName": "ApplicationGatewayPerformance",
    "time": "2016-04-09T00:00:00Z",
    "category": "ApplicationGatewayPerformanceLog",
    "properties":
    {
        "instanceId":"ApplicationGatewayRole_IN_1",
        "healthyHostCount":"4",
        "unHealthyHostCount":"0",
        "requestCount":"185",
        "latency":"0",
        "failedRequestCount":"0",
        "throughput":"119427"
    }
}
```

> [!NOTE]
> Latentie wordt berekend vanaf het moment waarop de eerste byte van de HTTP-aanvraag wordt ontvangen op het tijdstip waarop de laatste byte van het HTTP-antwoord is verzonden. Het is de som van de verwerkingstijd van de toepassingsgateway, plus de netwerkverbindingskosten voor de back-end, plus de tijd met de back-end om de aanvraag te verwerken.

### <a name="firewall-log"></a>Firewall-logboek

De firewall-logboek is gegenereerd, alleen als u deze voor elke application gateway, zoals beschreven in de voorgaande stappen hebt ingeschakeld. Dit logboek is ook vereist dat de web application firewall is geconfigureerd in een toepassingsgateway. De gegevens worden opgeslagen in de storage-account dat u hebt opgegeven toen u de logboekregistratie hebt ingeschakeld. De volgende gegevens worden geregistreerd:


|Value  |Description  |
|---------|---------|
|instanceId     | Toepassingsgateway-exemplaar voor de firewall van welke gegevens wordt gegenereerd. Er is één rij per exemplaar voor een toepassingsgateway met meerdere instanties.         |
|clientIp     |   Oorspronkelijke IP-adres voor de aanvraag.      |
|clientPort     |  Oorspronkelijke poort voor de aanvraag.       |
|requestUri     | URL van de aanvraag ontvangen.       |
|ruleSetType     | Type van de regelset. De beschikbare waarde is OWASP.        |
|ruleSetVersion     | Regelset versie die wordt gebruikt. Beschikbare waarden zijn 2.2.9 en 3.0.     |
|ruleId     | Regel-ID van de activerende gebeurtenis.        |
|message     | Gebruiksvriendelijke bericht voor de triggergebeurtenis. Meer informatie vindt u in de detailsectie.        |
|action     |  De actie die wordt uitgevoerd op de aanvraag. Beschikbare waarden zijn geblokkeerd en toegestaan.      |
|site     | De site waarvoor het logboek is gegenereerd. Op dit moment alleen Global vermeld omdat er regels zijn van toepassing.|
|details     | De details van de triggergebeurtenis.        |
|Details.Message     | Beschrijving van de regel.        |
|details.data     | Specifieke gegevens gevonden in de aanvraag die overeenkomen met de regel.         |
|Details.File     | Configuratiebestand dat de regel.        |
|details.line     | Regelnummer in het configuratiebestand dat de gebeurtenis.       |

```json
{
  "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/{applicationGatewayName}",
  "operationName": "ApplicationGatewayFirewall",
  "time": "2017-03-20T15:52:09.1494499Z",
  "category": "ApplicationGatewayFirewallLog",
  "properties": {
    "instanceId": "ApplicationGatewayRole_IN_0",
    "clientIp": "104.210.252.3",
    "clientPort": "4835",
    "requestUri": "/?a=%3Cscript%3Ealert(%22Hello%22);%3C/script%3E",
    "ruleSetType": "OWASP",
    "ruleSetVersion": "3.0",
    "ruleId": "941320",
    "message": "Possible XSS Attack Detected - HTML Tag Handler",
    "action": "Blocked",
    "site": "Global",
    "details": {
      "message": "Warning. Pattern match \"<(a|abbr|acronym|address|applet|area|audioscope|b|base|basefront|bdo|bgsound|big|blackface|blink|blockquote|body|bq|br|button|caption|center|cite|code|col|colgroup|comment|dd|del|dfn|dir|div|dl|dt|em|embed|fieldset|fn|font|form|frame|frameset|h1|head|h ...\" at ARGS:a.",
      "data": "Matched Data: <script> found within ARGS:a: <script>alert(\\x22hello\\x22);</script>",
      "file": "rules/REQUEST-941-APPLICATION-ATTACK-XSS.conf",
      "line": "865"
    }
  }
} 

```

### <a name="view-and-analyze-the-activity-log"></a>Het activiteitenlogboek bekijken en analyseren

U kunt activiteitenlogboekgegevens bekijken en analyseren via een van de volgende methoden:

* **Azure-hulpprogramma's**: haal informatie uit het activiteitenlogboek op via Azure PowerShell, de Azure CLI, de Azure REST-API of de Azure-portal. In het artikel [Activiteitsbewerkingen met Resource Manager](../azure-resource-manager/resource-group-audit.md) staan stapsgewijze instructies voor elke methode.
* **Power BI**: als u nog geen [Power BI](https://powerbi.microsoft.com/pricing)-account hebt, kunt u het gratis uitproberen. Door het [inhoudspakket voor Azure-activiteitenlogboeken voor Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/) te gebruiken, kunt u uw gegevens analyseren met vooraf geconfigureerde dashboards die u kunt gebruiken zoals geleverd of kunt aanpassen.

### <a name="view-and-analyze-the-access-performance-and-firewall-logs"></a>Bekijk en analyseer de toegang, prestaties en firewall-Logboeken

[Logboeken in Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md) kan de bestanden gebeurtenislogboeken en prestatiemeteritems verzamelen van uw Blob storage-account. Het omvat visualisaties en krachtige zoekmogelijkheden om uw logboeken te analyseren.

U kunt ook verbinding maken met uw opslagaccount en de JSON-logboekitems voor toegangs- en prestatielogboeken ophalen. Nadat u de JSON-bestanden hebt gedownload, kunt u ze naar de CSV-indeling converteren en in Excel, Power BI of een ander hulpprogramma voor gegevensvisualisatie bekijken.

> [!TIP]
> Als u bekend bent met Visual Studio en de basisconcepten van het wijzigen van waarden voor constanten en variabelen in C#, kunt u de [logboekconversieprogramma’s](https://github.com/Azure-Samples/networking-dotnet-log-converter) gebruiken die beschikbaar zijn in GitHub.
> 
> 

#### <a name="analyzing-access-logs-through-goaccess"></a>Toegang tot logboeken via GoAccess analyseren

We hebben een Resource Manager-sjabloon die wordt geïnstalleerd en wordt uitgevoerd de populaire gepubliceerd [GoAccess](https://goaccess.io/) analyzer aanmelden voor Application Gateway toegang tot logboeken. GoAccess biedt waardevolle HTTP-verkeer-statistieken, zoals het unieke bezoekers, bestanden aangevraagd, Hosts, besturingssystemen, Browsers, HTTP-statuscodes en nog veel meer. Zie voor meer informatie de [Leesmij-bestand in de map met het Resource Manager-sjablonen in GitHub](https://aka.ms/appgwgoaccessreadme).

## <a name="metrics"></a>Metrische gegevens

Metrische gegevens zijn een functie voor bepaalde Azure-resources waar u prestatiemeteritems in de portal bekijken kunt. De volgende metrische gegevens zijn beschikbaar voor Application Gateway:

- **Huidige verbindingen**
- **Mislukte aanvragen**
- **Aantal in orde Backendpool**

   U kunt filteren op een per back-end-pool basis om weer te geven in orde/slecht hosts in een specifieke back-endpool.


- **Reactiestatus**

   De distributie van antwoord status code kan verder worden gecategoriseerd om antwoorden in 2xx, 3xx of 4xx of 5xx categorieën weer te geven.

- **Doorvoer**
- **Totaal aantal aanvragen**
- **Aantal niet in orde Backendpool**

   U kunt filteren op een per back-end-pool basis om weer te geven in orde/slecht hosts in een specifieke back-endpool.

Blader naar een toepassingsgateway onder **bewaking** Selecteer **metrische gegevens**. Om de beschikbare waarden te zien, selecteert u de vervolgkeuzelijst **METRISCH**.

In de volgende afbeelding ziet u een voorbeeld met drie metrische gegevens weergegeven voor de laatste 30 minuten:

[![](media/application-gateway-diagnostics/figure5.png "Metrische gegevens weergeven")](media/application-gateway-diagnostics/figure5-lb.png#lightbox)

Zie voor een huidige lijst met metrische gegevens [ondersteunde metrische gegevens met Azure Monitor](../azure-monitor/platform/metrics-supported.md).

### <a name="alert-rules"></a>Waarschuwingsregels

U kunt regels voor waarschuwingen op basis van metrische gegevens voor een resource op te starten. Bijvoorbeeld, een waarschuwing een webhook aanroepen of e-beheerder als de doorvoer van de toepassingsgateway boven, onder of met een drempelwaarde voor een opgegeven periode is.

Het volgende voorbeeld wordt beschreven hoe u een waarschuwingsregel die een e-mailbericht naar een beheerder na schendingen van de doorvoer van een drempelwaarde verzendt:

1. Selecteer **metrische waarschuwing toevoegen** openen de **regel toevoegen** pagina. Ook kunt u deze pagina van de metrische gegevens over pagina bereiken.

   ![Knop 'Waarschuwing voor metrische gegevens toevoegen'][6]

2. Op de **regel toevoegen** pagina, vul de naam van de voorwaarde, en op de hoogte stellen secties en selecteer **OK**.

   * In de **voorwaarde** selector, selecteert u een van de vier waarden: **Groter dan**, **groter dan of gelijk**, **minder dan**, of **kleiner dan of gelijk zijn aan**.

   * In de **periode** selector, selecteer een periode van vijf minuten tot zes uur.

   * Als u selecteert **e-eigenaren, bijdragers en lezers**, het e-mailbericht worden dynamisch op basis van de gebruikers die toegang tot die resource hebben. Anders krijgt u een door komma's gescheiden lijst van gebruikers in de **beheerder email(s)** vak.

   ![Regel pagina toevoegen][7]

Als de drempelwaarde is geschonden, ontvangt een e-mailbericht dat vergelijkbaar is met het scherm in de volgende afbeelding:

![E-mailadres voor geschonden drempelwaarde][8]

Een lijst met waarschuwingen wordt weergegeven nadat u een waarschuwing voor metrische gegevens maken. Het biedt een overzicht van alle regels voor de waarschuwingen.

![Lijst met waarschuwingen en regels][9]

Zie voor meer informatie over waarschuwingsmeldingen, [meldingen van waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

Voor meer informatie over webhooks en hoe u ze kunt gebruiken met waarschuwingen, gaat u naar [een webhook configureren voor een Azure metrische waarschuwing](../azure-monitor/platform/alerts-webhooks.md).

## <a name="next-steps"></a>Volgende stappen

* Prestatiemeteritems en gebeurtenislogboeken visualiseren met behulp van [logboeken van Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md).
* [Uw Azure-activiteitenlogboek met Power BI visualiseren](https://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogbericht.
* [Bekijk en analyseer Azure-activiteitenlogboeken in Power BI en meer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogbericht.

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png
[10]: ./media/application-gateway-diagnostics/figure10.png
