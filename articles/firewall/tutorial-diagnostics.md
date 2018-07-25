---
title: 'Zelfstudie: Azure Firewall-logboeken bewaken'
description: In deze zelfstudie leert u hoe u Azure Firewall-logboeken inschakelt en beheert.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.workload: infrastructure-services
ms.date: 7/11/2018
ms.author: victorh
ms.openlocfilehash: a4922fda80b957138a9929090f9d3c349348185d
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991861"
---
# <a name="tutorial-monitor-azure-firewall-logs"></a>Zelfstudie: Azure Firewall-logboeken bewaken

[!INCLUDE [firewall-preview-notice](../../includes/firewall-preview-notice.md)]

Bij de voorbeelden in de Azure Firewall-artikelen wordt ervan uitgegaan dat u de openbare preview van Azure Firewall al hebt ingeschakeld. Zie [De openbare preview van Azure Firewall inschakelen](public-preview.md) voor meer informatie.

U kunt Azure Firewall bewaken met behulp van firewall-logboeken. U kunt ook activiteitenlogboeken gebruiken om bewerkingen in Azure Firewall-resources te controleren.

Via de portal kunt u toegang verkrijgen tot sommige van deze logboeken. Logboeken kunnen worden verzonden naar [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), Storage en Event Hubs en kunnen worden geanalyseerd in Log Analytics of door verschillende hulpprogramma’s zoals Excel en Power BI.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Logboekregistratie inschakelen via de Azure-portal
> * Logboekregistratie inschakelen met PowerShell
> * Het activiteitenlogboek bekijken en analyseren
> * De logboeken voor netwerk- en toepassingsregels bekijken en analyseren

## <a name="diagnostic-logs"></a>Diagnostische logboeken

 De volgende diagnostische logboeken zijn beschikbaar voor Azure Firewall:

* **Logboek voor toepassingsregels**

   Het logboek voor toepassingsregels wordt alleen in een opslagaccount opgeslagen, naar Event Hubs gestreamd en/of naar Log Analytics verzonden als u het voor elke Azure Firewall hebt ingeschakeld. Elke nieuwe verbinding die overeenkomt met een van uw geconfigureerde toepassingsregels, resulteert in een logboek voor de geaccepteerde/geweigerde verbinding. De gegevens worden geregistreerd in JSON-indeling, zoals weergegeven in het volgende voorbeeld:

   ```
   Category: access logs are either application or network rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
   {
    "category": "AzureFirewallApplicationRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallApplicationRuleLog",
    "properties": {
        "msg": "HTTPS request from 10.1.0.5:55640 to mydestination.com:443. Action: Allow. Rule Collection: collection1000. Rule: rule1002"
    }
   }
   ```

* **Logboek voor netwerkregels**

   Het logboek voor netwerkregels wordt alleen in een opslagaccount opgeslagen, naar Event Hubs gestreamd en/of naar Log Analytics verzonden als u het voor elke Azure Firewall hebt ingeschakeld. Elke nieuwe verbinding die overeenkomt met een van uw geconfigureerde netwerkregels, resulteert in een logboek voor de geaccepteerde/geweigerde verbinding. De gegevens worden geregistreerd in JSON-indeling, zoals weergegeven in het volgende voorbeeld:

   ```
   Category: access logs are either application or network rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
  {
    "category": "AzureFirewallNetworkRule",
    "time": "2018-06-14T23:44:11.0590400Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallNetworkRuleLog",
    "properties": {
        "msg": "TCP request from 111.35.136.173:12518 to 13.78.143.217:2323. Action: Deny"
    }
   }

   ```

U hebt drie opties voor het opslaan van uw logboeken:

* **Opslagaccount**: opslagaccounts kunnen het best worden gebruikt wanneer logboeken voor een langere periode worden opgeslagen en moeten kunnen worden bekeken wanneer dat nodig is.
* **Event Hubs**: Event Hubs zijn een geweldige optie voor integratie met andere SIEM-hulpprogramma’s (Security Information and Event Management) om waarschuwingen over uw resources te krijgen.
* **Log Analytics**: Log Analytics kan het best worden gebruikt voor algemene realtime bewaking van uw toepassing of voor het bekijken van trends.

## <a name="activity-logs"></a>Activiteitenlogboeken

   Activiteitenlogboekitems worden standaard verzameld en kunnen in de Azure-portal worden bekeken.

   U kunt [Azure-activiteitenlogboeken](../azure-resource-manager/resource-group-audit.md) (voorheen bekend als operationele logboeken en auditlogboeken) gebruiken om alle bewerkingen te bekijken die naar uw Azure-abonnement worden verzonden.

## <a name="enable-diagnostic-logging-through-the-azure-portal"></a>Diagnostische logboekregistratie inschakelen via de Azure-portal

Nadat u deze procedure voor het inschakelen van diagnostische logboekregistratie hebt voltooid, kan het enkele minuten duren voordat de gegevens in uw logboeken verschijnen. Als u aanvankelijk niets ziet, controleert u het een paar minuten later opnieuw.

1. Open in de Azure-portal uw firewall-resourcegroep en klik op de firewall.
2. Klik onder **Bewaking** op **Diagnostische logboeken**.

   Voor Azure Firewall zijn er twee servicespecifieke logboeken beschikbaar:

   * Logboek voor toepassingsregels
   * Logboek voor netwerkregels

3. Klik op **Diagnostische gegevens inschakelen** om te beginnen met het verzamelen van gegevens.
4. Op de pagina **Diagnostische instellingen** staan de instellingen voor de diagnostische logboeken. 
5. In dit voorbeeld worden de logboeken in Log Analytics opgeslagen, dus typ **Firewall Log Analytics** als naam.
6. Klik op **Verzenden naar Log Analytics** om uw werkruimte te configureren. U kunt ook Event Hubs en een opslagaccount gebruiken om de diagnostische logboeken op te slaan.
7. Klik onder **Log Analytics** op **Configureren**.
8. Klik op de pagina OMS-werkruimten op **Nieuwe werkruimte maken**.
9. Op de pagina **Log Analytics-werkruimte** typt u **firewall-oms** als naam voor de nieuwe **OMS-werkruimte**.
10. Selecteer uw abonnement, gebruik de bestaande firewall-resourcegroep (**Test-FW-RG**), selecteer **VS - oost** als locatie en selecteer de prijscategorie **Gratis**.
11. Klik op **OK**.
   ![Het configuratieproces starten][1]
12. Klik onder **Logboek** op **AzureFirewallApplicationRule** en **AzureFirewallNetworkRule** om logboeken voor toepassings- en netwerkregels te verzamelen.
   ![Diagnostische instellingen opslaan][2]
13. Klik op **Opslaan**.

## <a name="enable-logging-with-powershell"></a>Logboekregistratie inschakelen met PowerShell

Activiteitenlogboekregistratie is automatisch ingeschakeld voor elke Resource Manager-resource. Diagnostische logboekregistratie moet worden ingeschakeld om te beginnen met het verzamelen van de gegevens die beschikbaar zijn via die logboeken.

Volg de onderstaande stappen om diagnostische logboekregistratie in te schakelen:

1. Noteer de resource-ID van uw opslagaccount waar de logboekgegevens worden opgeslagen. Deze waarde heeft de volgende indeling: */abonnementen/\<subscriptionId\>/resourceGroups/\<resourcegroepnaam\>/providers/Microsoft.Storage/storageAccounts/\<opslagaccountnaam\>*.

   U kunt elk opslagaccount in uw abonnement gebruiken. U kunt de Azure-portal gebruiken om deze informatie te vinden. De informatie staat op de pagina **Eigenschap** van de resource.

2. Noteer de resource-ID van uw firewall waarvoor logboekregistratie is ingeschakeld. Deze waarde heeft de volgende indeling: */abonnementen/\<subscriptionId\>/resourceGroups/\<resourcegroepnaam\>/providers/Microsoft.Network/azureFirewalls/\<firewallnaam\>*.

   U kunt de portal gebruiken om deze informatie te vinden.

3. Schakel diagnostische logboekregistratie in door de volgende PowerShell-cmdlet te gebruiken:

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/azureFirewalls/<Firewall name> `
   -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> `
   -Enabled $true     
    ```
    
> [!TIP] 
>Voor diagnostische logboeken is geen apart opslagaccount nodig. Voor het gebruik van opslag voor toegangs- en prestatielogboeken worden servicekosten in rekening gebracht.

## <a name="view-and-analyze-the-activity-log"></a>Het activiteitenlogboek bekijken en analyseren

U kunt activiteitenlogboekgegevens bekijken en analyseren via een van de volgende methoden:

* **Azure-hulpprogramma’s**: Haal informatie uit het activiteitenlogboek op via Azure PowerShell, de Azure CLI, de Azure REST-API of de Azure-portal. In het artikel [Activiteitsbewerkingen met Resource Manager](../azure-resource-manager/resource-group-audit.md) staan stapsgewijze instructies voor elke methode.
* **Power BI**: Als u nog geen [Power BI](https://powerbi.microsoft.com/pricing)-account hebt, kunt u het gratis uitproberen. Door het [inhoudspakket voor Azure-activiteitenlogboeken voor Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/) te gebruiken, kunt u uw gegevens analyseren met vooraf geconfigureerde dashboards die u kunt gebruiken zoals geleverd of kunt aanpassen.

## <a name="view-and-analyze-the-network-and-application-rule-logs"></a>De logboeken voor netwerk- en toepassingsregels bekijken en analyseren

Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) verzamelt de teller- en gebeurtenislogboekbestanden. Het omvat visualisaties en krachtige zoekmogelijkheden om uw logboeken te analyseren.

U kunt ook verbinding maken met uw opslagaccount en de JSON-logboekitems voor toegangs- en prestatielogboeken ophalen. Nadat u de JSON-bestanden hebt gedownload, kunt u ze naar de CSV-indeling converteren en in Excel, Power BI of een ander hulpprogramma voor gegevensvisualisatie bekijken.

> [!TIP]
> Als u bekend bent met Visual Studio en de basisconcepten van het wijzigen van waarden voor constanten en variabelen in C#, kunt u de [logboekconversieprogramma’s](https://github.com/Azure-Samples/networking-dotnet-log-converter) gebruiken die beschikbaar zijn in GitHub.


## <a name="next-steps"></a>Volgende stappen

Nu u uw firewall hebt geconfigureerd om logboeken te verzamelen, kunt u Log Analytics verkennen om uw gegevens te bekijken.

> [!div class="nextstepaction"]
> [Netwerkbewakingsoplossingen in Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md)

[1]: ./media/tutorial-diagnostics/figure1.png
[2]: ./media/tutorial-diagnostics/figure2.png
