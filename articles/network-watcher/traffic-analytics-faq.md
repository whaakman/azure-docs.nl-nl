---
title: Veelgestelde vragen over Azure traffic analytics | Microsoft Docs
description: Vind antwoorden op enkele veelgestelde vragen over traffic analytics.
services: network-watcher
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/08/2018
ms.author: jdial
ms.openlocfilehash: a4b87d92751c84d96bc70915d16adae7943c145e
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062874"
---
# <a name="traffic-analytics-frequently-asked-questions"></a>Verkeer analytics Veelgestelde vragen

In dit artikel verzamelt op één plek veel van de veelgestelde vragen over verkeer analyses in Azure-netwerk-Watcher.

## <a name="what-are-the-prerequisites-to-use-traffic-analytics"></a>Wat zijn de vereisten voor het verkeer analytics gebruiken?

Traffic analytics moet de volgende vereisten:

- Een abonnement van de netwerk-Watcher ingeschakeld.
- Netwerkbeveiligingsgroep (NSG) stroom Logboeken is ingeschakeld voor het nsg's die u wilt bewaken.
- Een Azure Storage-account voor het opslaan van onbewerkte flog Logboeken.
- Een Azure-logboekanalyse-werkruimte met lees- en schrijftoegang.

Uw account moet voldoen aan een van de volgende traffic analytics inschakelen:

- Uw account moet worden toegewezen aan een van de volgende functies op het abonnementsniveau: accountbeheerder, servicebeheerder of CO-beheerder.
- Uw account moet een van de volgende rollen gebaseerde toegang toegangsbeheer (RBAC) rollen hebben bij het abonnementsbereik: eigenaar, bijdrager, lezer of netwerk Inzender.
- Als uw account niet aan een van de eerder vermelde rollen toegewezen is, moet deze worden toegewezen aan een aangepaste rol die de volgende acties uit, op het abonnementsniveau is toegewezen.
            
    - Microsoft.Network/applicationGateways/read
    - Microsoft.Network/connections/read
    - Microsoft.Network/loadBalancers/read 
    - Microsoft.Network/localNetworkGateways/read 
    - Microsoft.Network/networkInterfaces/read 
    - Microsoft.Network/networkSecurityGroups/read 
    - Microsoft.Network/publicIPAddresses/read
    - Microsoft.Network/routeTables/read
    - Microsoft.Network/virtualNetworkGateways/read 
    - Microsoft.Network/virtualNetworks/read
        
Rollen die zijn toegewezen aan een gebruiker voor een abonnement controleren:

1. Aanmelden bij Azure met behulp van **Login-AzureRmAccount**. 

2. Selecteer het vereiste abonnement met behulp van **Select-AzureRmSubscription**. 

3. U kunt de rollen die zijn toegewezen aan een opgegeven gebruiker gebruiken **Get-AzureRmRoleAssignment - SignInName [e-mailadres gebruiker] - IncludeClassicAdministrators**. 

Als u geen uitvoer niet ziet, moet u contact op met de abonnementsbeheerder van de respectieve als u wilt toegang krijgen tot de opdrachten uitvoeren. Zie voor meer informatie [rollen gebaseerd toegangsbeheer met Azure PowerShell beheren](https://docs.microsoft.com/en-us/azure/role-based-access-control/role-assignments-powershell).


## <a name="in-which-azure-regions-are-traffic-analytics-available"></a>In welke Azure gebieden worden traffic analytics?

U kunt verkeer analytics gebruiken voor het nsg's in een van de volgende ondersteunde regio's: West-Centraal VS, VS-Oost, VS-Oost 2, Noordelijk Centraal, VS, Zuid-centraal VS, VS-midden, VS-West, VS-West 2, West-Europa, Noord-Europa, VK West, VK Zuid, Australië-Oost, Australië-Zuidoost en Zuidoost-Azië. De werkruimte voor logboekanalyse moet bestaan in de West-Centraal VS, VS-Oost, West-Europa, VK Zuid, Australië-Zuidoost of de regio Zuidoost-Azië.

## <a name="can-the-nsgs-i-enable-flow-logs-for-be-in-different-regions-than-my-workspace"></a>Kan het nsg's ik stroom inschakelen Logboeken voor zich in verschillende regio's dan mijn werkruimte?

Ja, deze nsg's kunnen zich in verschillende regio's dan uw werkruimte voor logboekanalyse.

## <a name="can-multiple-nsgs-be-configured-within-a-single-workspace"></a>Kunnen meerdere nsg's worden geconfigureerd in een enkel werkruimte?

Ja.

## <a name="can-i-use-an-existing-workspace"></a>Kan ik een bestaande werkruimte gebruiken?

Ja. Als u een bestaande werkruimte selecteert, ervoor zorgen dat deze is gemigreerd naar de nieuwe querytaal. Als u niet bijwerken van de werkruimte wilt, moet u een nieuwe maken. Zie voor meer informatie over de nieuwe querytaal [Azure-logboekanalyse bijwerken naar de nieuwe logboek zoekopdracht](../log-analytics/log-analytics-log-search-upgrade.md).

## <a name="can-my-azure-storage-account-be-in-one-subscription-and-my-operations-management-suite-workspace-be-in-a-different-subscription"></a>Kan mijn Azure Storage-Account in een abonnement en Mijn Operations Management Suite-werkruimte in een ander abonnement?

Ja, Azure Storage-account kan zich in een abonnement en de Operations Management Suite-werkruimte kan zich in een ander abonnement.

## <a name="can-i-store-raw-logs-in-a-different-subscription"></a>Kan ik onbewerkte Logboeken opslaan in een ander abonnement

Nee. U kunt de onbewerkte Logboeken opslaan in een opslagaccount waarvoor een NSG voor logboeken van de stroom is ingeschakeld. Zowel het opslagaccount en de logboeken van de onbewerkte moeten wel in hetzelfde abonnement en dezelfde regio.

## <a name="what-if-i-cant-configure-an-nsg-for-traffic-analytics-due-to-a-not-found-error"></a>Wat gebeurt er als ik kan een NSG voor traffic analytics vanwege een fout 'Niet gevonden' niet configureren?

Selecteer een ondersteunde regio. Als u een niet-ondersteunde regio selecteert, ontvangt u een 'Niet gevonden'-fout. De ondersteunde regio's worden eerder in dit artikel vermeld.

## <a name="what-if-i-am-getting-the-status-failed-to-load-under-the-nsg-flow-logs-page"></a>Wat gebeurt er als ik krijg de status 'kan niet laden,' onder de NSG stroom logboeken pagina?

De provider Microsoft.Insights moet worden geregistreerd voor logboekregistratie werkt alleen goed als stroom. Als u niet zeker of de provider Microsoft.Insights is geregistreerd voor uw abonnement weet, vervangt u *xxxxx-xxxxx-xxxxxx-xxxx* in de volgende opdracht en voer de volgende opdrachten vanuit PowerShell:

```powershell-interactive
**Select-AzureRmSubscription** -SubscriptionId xxxxx-xxxxx-xxxxxx-xxxx
**Register-AzureRmResourceProvider** -ProviderNamespace Microsoft.Insights
```

## <a name="i-have-configured-the-solution-why-am-i-not-seeing-anything-on-the-dashboard"></a>Ik hebt de oplossing geconfigureerd. Waarom krijg ik niet zien alles op het dashboard?

Het dashboard kan worden weergegeven voor het eerst tot 30 minuten duren. De oplossing moet voldoende gegevens voor het afleiden van betekenisvolle inzichten eerst statistische berekening uitvoeren. Vervolgens worden rapporten gegenereerd. 

## <a name="what-if-i-get-this-message-we-could-not-find-any-data-in-this-workspace-for-selected-time-interval-try-changing-the-time-interval-or-select-a-different-workspace"></a>Wat gebeurt er als ik dit bericht ontvangt: "Er is niet gevonden geen gegevens in deze werkruimte voor het geselecteerde tijdsinterval. Wijzig het tijdsinterval of Selecteer een andere werkruimte. '?

Voer de volgende opties:
- Wijzig het tijdsinterval in de bovenste balk.
- Selecteer een andere werkruimte voor logboekanalyse in de bovenste balk.
- Probeer toegang tot traffic analytics na 30 minuten als deze onlangs is ingeschakeld.
    
Als het probleem blijft bestaan, vraagtekens in de [voice-gebruikersforum](https://feedback.azure.com/forums/217313-networking?category_id=195844).

## <a name="what-if-i-get-this-message-analyzing-your-nsg-flow-logs-for-the-first-time-this-process-may-take-20-30-minutes-to-complete-check-back-after-some-time-2-if-the-above-step-doesnt-work-and-your-workspace-is-under-the-free-sku-then-check-your-workspace-usage-here-to-validate-over-quota-else-refer-to-faqs-for-further-information"></a>Wat gebeurt er als ik dit bericht ontvangt: "analyseren van uw NSG logboeken voor de eerste keer stromen. Dit proces kan 20-30 minuten duren. Controleer na enige tijd opnieuw. 2) als de vorige stap niet werkt en uw werkruimte ligt onder de gratis SKU, Controleer uw werkruimte-gebruik voor het valideren van overschreden, anders Raadpleeg Veelgestelde vragen voor meer informatie. "?

U ziet dit bericht omdat:
- Verkeer analytics onlangs is ingeschakeld en kan niet nog zijn geaggregeerd voldoende gegevens voor het afleiden van betekenisvolle inzichten.
- Gebruik de gratis versie van de Operations Management Suite-werkruimte en de quotalimieten is overschreden. Mogelijk moet u een werkruimte te gebruiken met een grotere capaciteit.
    
Als het probleem blijft bestaan, vraagtekens in de [voice-gebruikersforum](https://feedback.azure.com/forums/217313-networking?category_id=195844).
    
## <a name="what-if-i-get-this-message-looks-like-we-have-resources-data-topology-and-no-flows-information-meanwhile-click-here-to-see-resources-data-and-refer-to-faqs-for-further-information"></a>Wat gebeurt er als ik dit bericht ontvangt: "lijkt te hebben we resources (topologie) en er zijn geen gegevens stromen. Ondertussen Klik hier om te zien van bronnen gegevens en Raadpleeg de veelgestelde vragen voor meer informatie. "?

U ziet de bronneninformatie op het dashboard; Er zijn echter geen stroom-gegevens aanwezig. Vanwege geen overdrachten van communicatie tussen de bronnen kan geen gegevens aanwezig zijn. Wacht tot 60 minuten en status controleren. Als het probleem zich blijft voordoen en u zeker weet dat overdrachten van communicatie tussen bronnen bestaan, vraagtekens in de [voice-gebruikersforum](https://feedback.azure.com/forums/217313-networking?category_id=195844).

## <a name="can-i-configure-traffic-analytics-using-powershell-or-an-azure-resource-manager-template-or-client"></a>Kan ik configureren traffic analytics met PowerShell of een Azure Resource Manager-sjabloon of de client?

U kunt verkeer analytics configureren met behulp van Windows PowerShell versie 6.2.1 en hoger. Zie configureren van stroom logboekregistratie en analyse van verkeer voor een specifieke NSG met de cmdlet Set [Set AzureRmNetworkWatcherConfigFlowLog](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermnetworkwatcherconfigflowlog?view=azurermps-6.3.0). Als u de logboekregistratie van stroom en verkeer analytics status voor een specifieke NSG, Zie [Get-AzureRmNetworkWatcherFlowLogStatus](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkwatcherflowlogstatus?view=azurermps-6.3.0).

U niet op dit moment wordt een Azure Resource Manager-sjabloon gebruiken voor het configureren van traffic analytics.

Zie de volgende voorbeelden traffic analytics configureren met behulp van een Azure Resource Manager-client.

**Voorbeeld van de cmdlet Set:**
```
#Requestbody parameters
$TAtargetUri ="/subscriptions/<NSG subscription id>/resourceGroups/<NSG resource group name>/providers/Microsoft.Network/networkSecurityGroups/<name of NSG>"
$TAstorageId = "/subscriptions/<storage subscription id>/resourcegroups/<storage resource group name> /providers/microsoft.storage/storageaccounts/<storage account name>"
$networkWatcherResourceGroupName = "<network watcher resource group name>"
$networkWatcherName = "<network watcher name>"

$requestBody = 
@"
{
    'targetResourceId': '${TAtargetUri}',
    'properties': 
    {
        'storageId': '${TAstorageId}',
        'enabled': '<true to enable flow log or false to disable flow log>',
        'retentionPolicy' : 
        {
            days: <enter number of days like to retail flow logs in storage account>,
            enabled: <true to enable retention or false to disable retention>
        }
    },
    'flowAnalyticsConfiguration':
    {
                'networkWatcherFlowAnalyticsConfiguration':
      {
        'enabled':,<true to enable traffic analytics or false to disable traffic analytics>
        'workspaceId':'bbbbbbbb-bbbb-bbbb-bbbb-bbbbbbbbbbbb',
        'workspaceRegion':'<workspace region>',
        'workspaceResourceId':'/subscriptions/<workspace subscription id>/resourcegroups/<workspace resource group name>/providers/microsoft.operationalinsights/workspaces/<workspace name>'
        
      }

    }
}
"@
$apiversion = "2016-09-01"

armclient login
armclient post "https://management.azure.com/subscriptions/<NSG subscription id>/resourceGroups/<network watcher resource group name>/providers/Microsoft.Network/networkWatchers/<network watcher name>/configureFlowlog?api-version=${apiversion}" $requestBody
```
**Voorbeeld van een cmdlet ophalen:**
```
#Requestbody parameters
$TAtargetUri ="/subscriptions/<NSG subscription id>/resourceGroups/<NSG resource group name>/providers/Microsoft.Network/networkSecurityGroups/<NSG name>"


$requestBody = 
@"
{
    'targetResourceId': '${TAtargetUri}'
}
“@


armclient login
armclient post "https://management.azure.com/subscriptions/<NSG subscription id>/resourceGroups/<network watcher resource group name>/providers/Microsoft.Network/networkWatchers/<network watcher name>/queryFlowLogStatus?api-version=${apiversion}" $requestBody
```



## <a name="how-is-traffic-analytics-priced"></a>Hoe wordt verkeer analytics prijs?

Verkeer analytics datalimiet geldt. De meting is gebaseerd op de verwerking van de logboekgegevens van de stroom door de service en opslaan van de resulterende verbeterde Logboeken in een werkruimte voor logboekanalyse. Zie voor meer informatie de [prijzen plan](https://azure.microsoft.com/en-us/pricing/details/network-watcher/). 

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-geo-map-view"></a>Hoe kan ik navigeren met het toetsenbord in de overzichtsweergave geo?

De pagina van de kaart geo bevat twee hoofdsecties:
    
- **Banner**: de banner aan de bovenkant van de kaart geo voorziet in knoppen voor het verkeer distributie-filters (bijvoorbeeld, implementatie, het verkeer van landen en kwaadaardig) selecteren. Wanneer u een knop selecteert, wordt het respectieve filter wordt toegepast op de kaart. Als u de actieve knop selecteert, markeert de kaart de actieve datacentra in uw implementatie.
- **Kaart**: onder de banner ziet u de sectie map distributie van verkeer tussen Azure-datacenters en andere landen.
    
### <a name="keyboard-navigation-on-the-banner"></a>Toetsenbordnavigatie op de koptekst
    
- Standaard is de selectie op de pagina van de kaart geo voor het logo van het filter 'Azure DC's '.
- Als u wilt verplaatsen naar een ander filter, gebruiken de `Tab` of de `Right arrow` sleutel. Als u wilt terug verplaatsen, gebruikt de `Shift+Tab` of de `Left arrow` sleutel. Navigatie is links naar rechts, gevolgd door boven naar beneden.
- Druk op `Enter` of de `Down` pijltoets het geselecteerde filter wilt toepassen. Op basis van filterselectie en implementatie, zijn een of meer knooppunten onder de sectie map gemarkeerd.
- Als u wilt overschakelen tussen banner en kaart, drukt u op `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-map"></a>Toetsenbordnavigatie op de kaart
    
- Nadat u hebt geselecteerd een filter op de koptekst en ingedrukt `Ctrl+F6`, focus naar een van de gemarkeerde knooppunten (**Azure-datacenter** of **land/regio**) in de overzichtsweergave.
- Als u wilt verplaatsen naar andere gemarkeerde knooppunten in de kaart, gebruiken `Tab` of de `Right arrow` voor voorwaartse beweging. Gebruik `Shift+Tab` of de `Left arrow` sleutel voor neerwaartse verkeer.
- U selecteert een gemarkeerde knooppunt in de kaart, met de `Enter` of `Down arrow` sleutel.
- Op de selectie van eventuele dergelijke knooppunten, focus naar de **informatie werkset** voor het knooppunt. Standaard focus naar de gesloten knop op de **informatie werkset**. Verder verplaatsen binnen de **vak** geven, gebruikt u `Right arrow` en `Left arrow` sleutels verplaatsen voorwaarts en achterwaarts, respectievelijk. Drukken `Enter` heeft hetzelfde effect als de knop gerichte in de **informatie werkset**.
- Wanneer u drukt op `Tab` terwijl de focus op de **informatie werkset**, de focus naar de eindpunten in het hetzelfde continent als het geselecteerde knooppunt. Gebruik de `Right arrow` en `Left arrow` sleutels om deze eindpunten te doorlopen.
- U kunt verplaatsen naar andere stroom eindpunten of continent clusters met `Tab` voor voorwaartse beweging en `Shift+Tab` voor neerwaartse verkeer.
- Wanneer de focus is **Continent clusters**, gebruiken de `Enter` of `Down` pijltoetsen om te markeren de eindpunten in het continent cluster. Als u wilt verplaatsen door middel van eindpunten en de knop sluiten op de box informatie van het cluster continent, gebruiken de `Right arrow` of `Left arrow` sleutel voor voorwaarts en achterwaarts verkeer, respectievelijk. U kunt gebruiken op een willekeurig eindpunt `Shift+L` overschakelen naar de regel voor verbinding van het geselecteerde knooppunt naar het eindpunt. U kunt ook op `Shift+L` opnieuw te verplaatsen naar het geselecteerde eindpunt.
        
### <a name="keyboard-navigation-at-any-stage"></a>Toetsenbordnavigatie in elke fase
    
- `Esc` de uitgebreide selectie wordt samengevouwen.
- De `Up arrow` sleutel voert dezelfde actie als `Esc`. De `Down arrow` sleutel voert dezelfde actie als `Enter`.
- Gebruik `Shift+Plus` inzoomen, en `Shift+Minus` om uit te zoomen.

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-virtual-network-topology-view"></a>Hoe kan ik navigeren met het toetsenbord in de weergave van de topologie virtuele netwerk?

De pagina van de topologie virtuele netwerken bevat twee hoofdsecties:
    
- **Banner**: de banner aan de bovenkant van de topologie van de virtuele netwerken voorziet in knoppen voor het verkeer Distributiefilters (bijvoorbeeld verbonden virtuele netwerken, niet-verbonden virtuele netwerken en openbare IP-adressen) selecteren. Wanneer u een knop selecteert, wordt de desbetreffende filter wordt toegepast op de topologie. Als u de actieve knop selecteert, markeert de topologie de actieve virtuele netwerken in uw implementatie.
- **Topologie**: onder de banner toont de topologie sectie distributie van verkeer tussen virtuele netwerken.
    
### <a name="keyboard-navigation-on-the-banner"></a>Toetsenbordnavigatie op de koptekst
    
- Standaard is de selectie op de pagina van de topologie virtuele netwerken voor het logo van het filter 'VNets verbonden'.
- U kunt verplaatsen naar een ander filter met de `Tab` sleutel om te gaan. U kunt terug verplaatsen met de `Shift+Tab` sleutel. Navigatie is links naar rechts, gevolgd door boven naar beneden.
- Druk op `Enter` het geselecteerde filter wilt toepassen. Op basis van de filterselectie van het en de implementatie, worden een of meerdere knooppunten (virtueel netwerk) onder de sectie topologie gemarkeerd.
- Tussen de banner en de topologie, drukt u op `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-topology"></a>Toetsenbordnavigatie van de topologie
    
- Nadat u hebt geselecteerd een filter op de koptekst en ingedrukt `Ctrl+F6`, focus naar een van de gemarkeerde knooppunten (**VNet**) in de Topologieweergave.
- U kunt verplaatsen naar andere gemarkeerde knooppunten in de Topologieweergave met de `Shift+Right arrow` voor voorwaartse beweging. 
- Op de gemarkeerde knooppunten focus naar de **informatie werkset** voor het knooppunt. Standaard focus naar de **meer details** knop op de **informatie werkset**. Verder verplaatsen binnen de **vak** geven, gebruikt u de `Right arrow` en `Left arrow` sleutels verplaatsen voorwaarts en achterwaarts, respectievelijk. Drukken `Enter` heeft hetzelfde effect als de knop gerichte in de **informatie werkset**.
- Op de selectie van eventuele dergelijke knooppunten, vindt u alle verbindingen, één voor één, door te drukken de `Shift+Left arrow` sleutel. Focus naar de **informatie werkset** van die verbinding. Op elk gewenst moment de focus kan worden verplaatst naar het knooppunt door te drukken `Shift+Right arrow` opnieuw.
    

## <a name="how-can-i-navigate-by-using-the-keyboard-in-the-subnet-topology-view"></a>Hoe kan ik Ga via het toetsenbord in de weergave van de topologie subnet

De pagina van de topologie virtuele subnetwerken bevat twee hoofdsecties:
    
- **Banner**: de banner aan de bovenkant van de topologie virtuele subnetwerken voorziet in knoppen voor het verkeer distributie-filters (bijvoorbeeld actief, middelgroot en Gateway subnetten) selecteren. Wanneer u een knop selecteert, wordt de desbetreffende filter wordt toegepast op de topologie. Als u de actieve knop selecteert, markeert de topologie de actieve virtuele subnetwerk in uw implementatie.
- **Topologie**: onder de banner in de sectie topologie distributie van verkeer tussen virtuele subnetwerken ziet.
    
### <a name="keyboard-navigation-on-the-banner"></a>Toetsenbordnavigatie op de koptekst
    
- De selectie op de pagina virtuele subnetwerken topologie voor de banner is standaard het filter 'Subnetten'.
- U kunt verplaatsen naar een ander filter met de `Tab` sleutel om te gaan. U kunt terug verplaatsen met de `Shift+Tab` sleutel. Navigatie is links naar rechts, gevolgd door boven naar beneden.
- Druk op `Enter` het geselecteerde filter wilt toepassen. Op basis van filterselectie en implementatie, worden een of meerdere knooppunten (Subnet) onder de sectie topologie gemarkeerd.
- Tussen de banner en de topologie, drukt u op `Ctrl+F6`.
        
### <a name="keyboard-navigation-on-the-topology"></a>Toetsenbordnavigatie van de topologie
    
- Nadat u hebt geselecteerd een filter op de koptekst en ingedrukt `Ctrl+F6`, focus naar een van de gemarkeerde knooppunten (**Subnet**) in de Topologieweergave.
- U kunt verplaatsen naar andere gemarkeerde knooppunten in de Topologieweergave met de `Shift+Right arrow` voor voorwaartse beweging. 
- Op de gemarkeerde knooppunten focus naar de **informatie werkset** voor het knooppunt. Standaard focus naar de **meer details** knop op de **informatie werkset**. Verder verplaatsen binnen de **vak** geven, gebruikt u `Right arrow` en `Left arrow` sleutels verplaatsen voorwaarts en achterwaarts, respectievelijk. Drukken `Enter` heeft hetzelfde effect als de knop gerichte in de **informatie werkset**.
- Op de selectie van eventuele dergelijke knooppunten, vindt u alle verbindingen, één voor één, door te drukken `Shift+Left arrow` sleutel. Focus naar de **informatie werkset** van die verbinding. Op elk gewenst moment de focus kan worden verplaatst naar het knooppunt door te drukken `Shift+Right arrow` opnieuw.    

