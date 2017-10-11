---
title: Toegangslogboeken, Prestatielogboeken, back-end-status en metrische gegevens voor Application Gateway bewaken | Microsoft Docs
description: Meer informatie over het inschakelen en beheren van toegang en Prestatielogboeken voor Application Gateway
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: tysonn
tags: azure-resource-manager
ms.assetid: 300628b8-8e3d-40ab-b294-3ecc5e48ef98
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: amitsriva
ms.openlocfilehash: 12c252340b82aba5ee69b12db83353750782e7c5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="back-end-health-diagnostic-logs-and-metrics-for-application-gateway"></a>Back-end-status, diagnostische logboeken en metrische gegevens voor Application Gateway

U kunt met behulp van Azure Application Gateway resources bewaken in de volgende manieren:

* [Status van de back-end](#back-end-health): Application Gateway biedt de mogelijkheid voor het bewaken van de status van de servers in de back-end-adresgroepen via de Azure-portal en via PowerShell. U kunt ook de status van de back-end-adresgroepen via de prestaties van diagnostische logboeken vinden.

* [Logboeken](#diagnostic-logs): Logboeken toestaan voor prestaties, toegang en andere gegevens worden opgeslagen of gebruikt van een resource voor controledoeleinden.

* [Metrische gegevens](#metrics): Application Gateway is momenteel een waarde. Met deze metriek wordt de doorvoer van de toepassingsgateway in bytes per seconde.

## <a name="back-end-health"></a>Back-end-status

Toepassingsgateway biedt de mogelijkheid voor het bewaken van de status van afzonderlijke onderdelen van de back-end-adresgroepen via de portal, PowerShell en de opdrachtregelinterface (CLI). U kunt ook een geaggregeerde status vinden samenvatting van back-end-groepen via de diagnostische logboeken van de prestaties. 

Het back-end-statusrapport weerspiegelt de uitvoer van de statuscontrole van de Application Gateway naar de back-end-exemplaren. Bij het zoeken is voltooid en de back-end kan verkeer ontvangen, als in orde wordt beschouwd. Anders wordt deze orde beschouwd.

> [!IMPORTANT]
> Als er een netwerkbeveiligingsgroep (NSG) op een Application Gateway-subnet, opent u poortbereiken 65503 65534 op het subnet voor Application Gateway voor binnenkomend verkeer. Deze poorten zijn vereist voor de back-end-status API om te werken.


### <a name="view-back-end-health-through-the-portal"></a>Status van de back-end via de portal weergeven

In de portal wordt automatisch back-end health verstrekt. Selecteer in een bestaande toepassingsgateway **bewaking** > **back-end health**. 

Elk lid in de back-end-pool wordt vermeld op deze pagina (of het is een NIC, IP of FQDN). De naam van de back-endpool, poort, HTTP-instellingen voor de naam van back-end en gezondheidsstatus worden weergegeven. Geldige waarden voor gezondheidsstatus zijn **goed**, **niet in orde**, en **onbekende**.

> [!NOTE]
> Als er een back-end-gezondheidsstatus van **onbekende**, zorg ervoor dat de toegang tot de back-end niet wordt geblokkeerd door een regel voor het NSG, een door de gebruiker opgegeven routes (UDR) of een aangepaste DNS-server in het virtuele netwerk.

![Back-end-status][10]

### <a name="view-back-end-health-through-powershell"></a>Status van de back-end via PowerShell weergeven

De volgende PowerShell-code laat zien hoe de gezondheid van de back-end weergeven met behulp van de `Get-AzureRmApplicationGatewayBackendHealth` cmdlet:

```powershell
Get-AzureRmApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

### <a name="view-back-end-health-through-azure-cli-20"></a>Status van de back-end via Azure CLI 2.0 weergeven

```azurecli
az network application-gateway show-backend-health --resource-group AdatumAppGatewayRG --name AdatumAppGateway
```

### <a name="results"></a>Resultaten

Het volgende fragment toont een voorbeeld van het antwoord:

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

U kunt verschillende typen logboeken in Azure beheren en problemen oplossen Toepassingsgateways. U kunt sommige van deze logboeken openen via de portal. Alle logboeken kunnen worden opgehaald uit Azure Blob storage en weergegeven in de verschillende hulpprogramma's, zoals [logboekanalyse](../log-analytics/log-analytics-azure-networking-analytics.md), Excel en Power BI. U kunt meer informatie over de verschillende typen logboeken in de volgende lijst:

* **Activiteitenlogboek**: U kunt [Azure activiteitenlogboeken](../monitoring-and-diagnostics/insights-debugging-with-events.md) (voorheen bekend als operationele logboeken en controlelogboeken) om alle bewerkingen die worden verzonden naar uw Azure-abonnement en hun status weer te geven. Logboekvermeldingen activiteit worden standaard verzameld en u kunt ze weergeven in de Azure portal.
* **Toegangslogboek**: U kunt dit logboek Application Gateway toegangspatronen weergeven en analyseren van informatie, waaronder de aanroeper IP, aangevraagde URL antwoord latentie, retourcode en bytes in en uit. Een toegangslogboek verzameld om de 300 seconden. Dit logboek bevat één record per exemplaar van de toepassingsgateway. Het exemplaar Application Gateway kan worden geïdentificeerd door de exemplaar-id-eigenschap.
* **Prestatielogboek**: U kunt dit logboek gebruiken om weer te geven hoe Application Gateway-exemplaren uitvoert. Dit logboek bevat informatie over de prestaties voor elk exemplaar, met inbegrip van totaal aantal aanvragen die worden geleverd, doorvoer in bytes, totaal aantal aanvragen dat wordt geleverd, aantal mislukte aanvragen en in orde en slecht backend-exemplaren. Een prestatielogboek worden verzameld om de 60 seconden.
* **Firewall-logboek**: U kunt dit logboek gebruiken om de aanvragen die zijn geregistreerd via de modus voor detectie of ter voorkoming van een toepassingsgateway die is geconfigureerd met de web application firewall weer te geven.

> [!NOTE]
> Logboeken zijn alleen beschikbaar voor resources geïmplementeerd in het Azure Resource Manager-implementatiemodel. U kunt Logboeken niet gebruiken voor resources in het klassieke implementatiemodel. Zie voor een beter begrip van de twee modellen, de [Understanding Resource Manager-implementatie en klassieke implementatie](../azure-resource-manager/resource-manager-deployment-model.md) artikel.

Hebt u drie opties voor het opslaan van uw logboeken:

* **Storage-account**: Storage-accounts beste kunnen worden gebruikt voor Logboeken wanneer logboeken worden opgeslagen voor een langere duur en gelezen wanneer deze nodig is.
* **Event hubs**: Event hubs zijn een goede optie voor het integreren met andere informatie over beveiliging en beheer van gebeurtenissen (SEIM) hulpmiddelen voor waarschuwingen over uw resources.
* **Meld u Analytics**: Log Analytics is het meest geschikt voor algemene realtime-controle van uw toepassing of trends kijken.

### <a name="enable-logging-through-powershell"></a>Schakel logboekregistratie in via PowerShell

Activiteit-logboekregistratie is standaard ingeschakeld voor elke Resource Manager-resource. U moet toegang en prestaties van logboekregistratie voor het starten van de gegevens beschikbaar zijn via deze logboeken verzamelen inschakelen. Als logboekregistratie wilt inschakelen, gebruikt u de volgende stappen uit:

1. Houd er rekening mee uw storage-account resource-ID, waar de logboekgegevens wordt opgeslagen. Deze waarde is van het formulier: /subscriptions/\<subscriptionId\>/resourceGroups/\<Resourcegroepnaam\>/providers/Microsoft.Storage/storageAccounts/\<opslagaccountnaam\>. U kunt opslagaccount gebruiken in uw abonnement. U kunt de Azure portal gebruiken om deze informatie te vinden.

    ![Portal: resource-ID voor het opslagaccount](./media/application-gateway-diagnostics/diagnostics1.png)

2. Houd er rekening mee resource-ID van de toepassingsgateway van uw waarvoor logboekregistratie is ingeschakeld. Deze waarde is van het formulier: /subscriptions/\<subscriptionId\>/resourceGroups/\<Resourcegroepnaam\>/providers/Microsoft.Network/applicationGateways/\<gateway toepassingsnaam\>. U kunt de portal gebruiken om deze informatie te vinden.

    ![Portal: resource-ID voor de toepassingsgateway](./media/application-gateway-diagnostics/diagnostics2.png)

3. Diagnostische logboekregistratie inschakelen met behulp van de volgende PowerShell-cmdlet:

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```
    
> [!TIP] 
>Activiteitenlogboeken vereisen een afzonderlijke opslagaccount niet. Het gebruik van opslag voor toegangs- en prestatieregistratie kosten in rekening worden gebracht.

### <a name="enable-logging-through-the-azure-portal"></a>Schakel logboekregistratie in via de Azure-portal

1. Uw bron vinden in de Azure portal en klikt u op **diagnostische logboeken**.

   Er zijn drie logboeken beschikbaar voor Application Gateway:

   * Toegangslogboek
   * Prestatielogboek
   * Firewall-logboek

2. Als u wilt verzamelen van gegevens, klikt u op **diagnostische gegevens inschakelen**.

   ![Diagnostische gegevens inschakelen][1]

3. De **diagnostische instellingen** blade bevat de instellingen voor diagnostische logboeken. In dit voorbeeld worden de logboeken met logboekanalyse opgeslagen. Klik op **configureren** onder **logboekanalyse** voor het configureren van uw werkruimte. U kunt ook event hubs en storage-account gebruiken om op te slaan van de diagnostische logboeken.

   ![Het configuratieproces wordt gestart][2]

4. Kies een bestaande werkruimte van Operations Management Suite (OMS) of een nieuwe maken. In dit voorbeeld wordt een bestaande.

   ![Opties voor OMS werkruimten][3]

5. Bevestig de instellingen en klik op **opslaan**.

   ![Diagnostische instellingenblade met selecties][4]

### <a name="activity-log"></a>Activiteitenlogboek

Standaard genereert Azure het activiteitenlogboek. De logboeken gedurende 90 dagen bewaard in de gebeurtenislogboeken van Azure-opslag. Meer informatie over deze logboeken door te lezen de [weergeven van gebeurtenissen en het activiteitenlogboek](../monitoring-and-diagnostics/insights-debugging-with-events.md) artikel.

### <a name="access-log"></a>Toegangslogboek

Het toegangslogboek wordt alleen gegenereerd als u deze op elk exemplaar van de toepassingsgateway, zoals beschreven in de voorgaande stappen hebt ingeschakeld. De gegevens worden opgeslagen in het opslagaccount dat u hebt opgegeven als u de logboekregistratie ingeschakeld. Elke toegang van Application Gateway wordt geregistreerd in JSON-indeling, zoals wordt weergegeven in het volgende voorbeeld:


|Waarde  |Beschrijving  |
|---------|---------|
|Exemplaar-id     | Gateway-instantie die de aanvraag wordt uitgevoerd.        |
|client-IP     | Oorspronkelijke IP-adres voor de aanvraag.        |
|enterpriseclient     | Poort voor de aanvraag afkomstig is.       |
|HttpMethod     | HTTP-methode die wordt gebruikt door de aanvraag.       |
|requestUri     | De URI van de aanvraag ontvangen.        |
|RequestQuery     | **Server-gerouteerd**: exemplaar van de Back-end-adresgroep die de aanvraag is verzonden. </br> **X-AzureApplicationGateway-logboek-ID**: correlatie-ID van de aanvraag gebruikt. Het kan worden gebruikt om op te lossen problemen met de verkeer voor de back-endservers. </br>**STATUS van de SERVER**: HTTP-antwoordcode Application Gateway van de back-end ontvangen.       |
|UserAgent     | De gebruikersagent van de HTTP-aanvraag-header.        |
|httpStatus     | HTTP-statuscode geretourneerd naar de client van de toepassingsgateway.       |
|Httpversie     | HTTP-versie van de aanvraag.        |
|ReceivedBytes     | Grootte van het pakket ontvangen in bytes.        |
|SentBytes| Grootte van het pakket dat is verzonden, in bytes.|
|timeTaken| Totale tijdsduur (in milliseconden) die het duurt voordat een aanvraag te verwerken en de reactie daarop worden verzonden. Dit wordt berekend als het interval van de tijd wanneer Application Gateway ontvangt de eerste byte van een HTTP-aanvraag aan de tijd wanneer de bewerking is voltooid voor het verzenden van het antwoord. Het is belangrijk te weten het veld tijd omvat gewoonlijk het tijdstip waarop de aanvraag en antwoord-pakketten worden overgebracht via het netwerk. |
|sslEnabled| Communicatie met de back-end-adresgroepen wordt aangegeven of SSL gebruikt. Geldige waarden zijn in- of uitschakelen.|
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

### <a name="performance-log"></a>Prestatielogboek

De prestatielogboek wordt alleen gegenereerd als u deze op elk exemplaar van de toepassingsgateway, zoals beschreven in de voorgaande stappen hebt ingeschakeld. De gegevens worden opgeslagen in het opslagaccount dat u hebt opgegeven als u de logboekregistratie ingeschakeld. De prestatielogboekgegevens wordt gegenereerd met intervallen van 1 minuut. De volgende gegevens worden geregistreerd:


|Waarde  |Beschrijving  |
|---------|---------|
|Exemplaar-id     |  Application Gateway-exemplaren waarvoor gegevens wordt gegenereerd. Er is een rij per exemplaar voor een toepassingsgateway met meerdere-exemplaar.        |
|healthyHostCount     | Het aantal orde hosts in de groep back-end.        |
|unHealthyHostCount     | Het aantal beschadigde hosts in de groep back-end.        |
|requestCount     | Het aantal geleverde aanvragen.        |
|Latentie | Latentie (in milliseconden) van aanvragen van het exemplaar naar de back-end die de aanvragen fungeert. |
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
> Latentie wordt berekend op basis van de tijd wanneer de eerste byte van de HTTP-aanvraag wordt ontvangen op het tijdstip waarop de laatste byte van het HTTP-antwoord is verzonden. Het is de som van de verwerkingstijd van de toepassingsgateway plus de netwerkverbindingskosten voor de back-end, plus de tijd die de back-end nodig heeft om de aanvraag te verwerken.

### <a name="firewall-log"></a>Firewall-logboek

De firewall-logboek is gegenereerd, alleen als u deze voor elke application gateway, zoals beschreven in de voorgaande stappen hebt ingeschakeld. Dit logboek is ook vereist dat de web application firewall op een application gateway is geconfigureerd. De gegevens worden opgeslagen in het opslagaccount dat u hebt opgegeven als u de logboekregistratie ingeschakeld. De volgende gegevens worden geregistreerd:


|Waarde  |Beschrijving  |
|---------|---------|
|Exemplaar-id     | Gateway toepassingsexemplaar voor welke firewall worden gegevens gegenereerd. Er is een rij per exemplaar voor een toepassingsgateway met meerdere-exemplaar.         |
|client-IP     |   Oorspronkelijke IP-adres voor de aanvraag.      |
|enterpriseclient     |  Poort voor de aanvraag afkomstig is.       |
|requestUri     | De URL van de aanvraag ontvangen.       |
|ruleSetType     | Type van de regelset. De beschikbare waarde is OWASP.        |
|ruleSetVersion     | Regelset versie die wordt gebruikt. Beschikbare waarden zijn 2.2.9 en 3.0.     |
|ruleId     | Regel-ID van de activerende gebeurtenis.        |
|Bericht     | Gebruiksvriendelijke bericht voor de triggergebeurtenis. Meer informatie vindt u in de detailsectie.        |
|Actie     |  Actie op die op de aanvraag. Beschikbare waarden zijn geblokkeerd en toegestaan.      |
|Site     | De site waarvoor het logboek is gegenereerd. Alleen globale wordt momenteel, omdat er regels zijn globale vermeld.|
|Meer informatie     | Details van de activerende gebeurtenis.        |
|Details.Message     | Beschrijving van de regel.        |
|Details.Data     | Specifieke gegevens gevonden in de aanvraag die overeenkomt met de regel.         |
|Details.File     | Configuratiebestand die deel uitmaakt van de regel.        |
|Details.line     | Regelnummer in het configuratiebestand dat de gebeurtenis.       |

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

### <a name="view-and-analyze-the-activity-log"></a>Weergeven en analyseren van het activiteitenlogboek

U kunt weergeven en analyseren van logboekgegevens van de activiteit met behulp van de volgende methoden:

* **Azure-hulpprogramma's**: gegevens ophalen uit het activiteitenlogboek via Azure PowerShell, de Azure CLI, de REST-API van Azure of de Azure-portal. Stapsgewijze instructies voor elke methode zijn aangegeven in de [activiteit bewerkingen met Resource Manager](../azure-resource-manager/resource-group-audit.md) artikel.
* **Power BI**: als u nog geen hebt een [Power BI](https://powerbi.microsoft.com/pricing) account, kunt u proberen deze gratis. Met behulp van de [activiteitenlogboeken Azure pack voor Power BI inhoud](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), u kunt uw gegevens analyseren met vooraf geconfigureerde dashboards die u gebruiken kunt of aanpassen.

### <a name="view-and-analyze-the-access-performance-and-firewall-logs"></a>Weergeven en analyseren van de toegang, prestaties en firewall-Logboeken

Azure [logboekanalyse](../log-analytics/log-analytics-azure-networking-analytics.md) kan de teller en de event log-bestanden verzamelen van uw Blob storage-account. Dit omvat visualisaties en krachtige zoekmogelijkheden hebben voor het analyseren van uw Logboeken.

U kunt ook verbinding maken met uw opslagaccount en ophalen van de JSON-logboekvermeldingen voor toegang en prestaties van Logboeken. Nadat u de JSON-bestanden hebt gedownload, kunt u deze converteren naar CSV en ze weergeven in Excel, Power BI of een ander hulpprogramma voor de weergave van gegevens.

> [!TIP]
> Als u bekend met Visual Studio en de basisconcepten bent van het wijzigen van waarden voor de constanten en variabelen in C#, kunt u de [Meld converter extra](https://github.com/Azure-Samples/networking-dotnet-log-converter) beschikbaar is via GitHub.
> 
> 

## <a name="metrics"></a>Metrische gegevens

Metrische gegevens zijn een functie voor bepaalde waar u prestatiemeteritems kunt bekijken in de portal voor Azure-resources. Voor Application Gateway is een waarde nu beschikbaar. Met deze metriek wordt de doorvoer en kunt u deze bekijken in de portal. Blader naar een application gateway en klikt u op **metrische gegevens**. Als u wilt weergeven van de waarden, selecteer doorvoer in de **beschikbare metrische gegevens** sectie. In de volgende afbeelding ziet u een voorbeeld met de filters die u kunt de gegevens worden weergegeven in andere bereiken.

![Metrische weergave met filters][5]

Zie voor een actuele lijst met metrische gegevens [ondersteund met een Azure-Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).

### <a name="alert-rules"></a>Regels voor waarschuwingen

U kunt regels voor waarschuwingen op basis van de metrische gegevens voor een bron op te starten. Bijvoorbeeld, een waarschuwing een webhook aanroepen of e-beheerder als de doorvoer van de toepassingsgateway boven, onder of met een drempelwaarde voor een opgegeven periode is.

Het volgende voorbeeld wordt u begeleid bij het maken van een waarschuwingsregel die een e-mailbericht naar een beheerder na schendingen van doorvoer een drempelwaarde verzendt:

1. Klik op **metrische waarschuwing toevoegen** openen de **regel toevoegen** blade. U kunt ook deze blade van de metrische gegevens blade bereiken.

   ![Knop 'Metrische waarschuwing toevoegen'][6]

2. Op de **regel toevoegen** blade invullen van de naam van de voorwaarde, en secties melden en op **OK**.

   * In de **voorwaarde** selector, selecteert u een van de vier waarden: **groter is dan**, **groter dan of gelijk**, **minder dan**, of **kleiner dan of gelijk zijn aan**.

   * In de **periode** selector, selecteert u een periode van 5 minuten en 6 uur.

   * Als u selecteert **e-eigenaren, bijdragers en lezers**, het e-mailbericht worden dynamisch op basis van de gebruikers die toegang tot deze resource hebben. Anders kunt u opgeven van een door komma's gescheiden lijst met gebruikers in de **aanvullende beheerder email(s)** vak.

   ![Blade regel toevoegen][7]

Als de drempelwaarde wordt overschreden, ontvangt een e-mailbericht is vergelijkbaar met de structuur in de volgende afbeelding:

![E-mailadres voor geschonden drempelwaarde][8]

Een lijst met waarschuwingen wordt weergegeven nadat u een waarschuwing voor een metriek maken. Deze biedt een overzicht van alle regels voor waarschuwingen.

![Lijst met waarschuwingen en regels][9]

Zie voor meer informatie over waarschuwingsmeldingen, [meldingen van waarschuwingen ontvangen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).

Voor meer informatie over webhooks en hoe u ze kunt gebruiken met waarschuwingen, gaat u naar [een webhook configureren op een Azure metrische waarschuwing](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

## <a name="next-steps"></a>Volgende stappen

* Teller en de gebeurtenislogboeken visualiseren met behulp van [logboekanalyse](../log-analytics/log-analytics-azure-networking-analytics.md).
* [Uw Azure activiteitenlogboek met Power BI visualiseren](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) blogbericht.
* [Weergeven en analyseren van Azure activiteitenlogboeken in Power BI en meer](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) blogbericht.

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
