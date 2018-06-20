---
title: Gebruik Microsoft Azure Traffic Manager eindpunt quotum in Language Understanding (LUIS) - Azure te verhogen | Microsoft Docs
description: Microsoft Azure Traffic Manager gebruiken voor het eindpunt quotum verdeeld over verschillende abonnementen in Language Understanding (LUIS) eindpunt quotum te verhogen
author: v-geberr
manager: kaiqb
services: cognitive-services
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/07/2018
ms.author: v-geberr
ms.openlocfilehash: 513d4395b1d3e631855c2f6e132d54331b3ddf8d
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266342"
---
# <a name="use-microsoft-azure-traffic-manager-to-manage-endpoint-quota-across-keys"></a>Microsoft Azure Traffic Manager gebruiken voor het beheren van endpoint-quotum via sleutels
Language Understanding (LUIS) biedt de mogelijkheid het quotum van de aanvraag eindpunt boven een enkele sleutel quotum te verhogen. Dit wordt gedaan door het maken van meer sleutels voor LUIS en ze toe te voegen aan de toepassing LUIS op de **publiceren** pagina in de **Resources en sleutels** sectie. 

De clienttoepassing heeft voor het beheren van het verkeer via de sleutels. LUIS doen dat niet. 

In dit artikel wordt uitgelegd hoe het verkeer via sleutels met Azure beheren [Traffic Manager][traffic-manager-marketing]. U moet een getraind en gepubliceerde LUIS-app al hebt. Als u geen abonnement hebt, volgt u de vooraf gedefinieerde domein [Quick Start](luis-get-started-create-app.md). 

## <a name="connect-to-powershell-in-the-azure-portal"></a>Verbinding maken met PowerShell in de Azure portal
In de [Azure] [ azure-portal] portal, open de PowerShell-venster. Het pictogram voor het PowerShell-venster wordt de **> _** in de bovenste navigatiebalk. Ophalen van de meest recente versie van PowerShell en u bent geverifieerd met behulp van PowerShell vanuit de portal. PowerShell in de portal vereist een [Azure Storage](https://azure.microsoft.com/services/storage/) account. 

![Schermopname van Azure-portal met Powershell-venster openen](./media/traffic-manager/azure-portal-powershell.png)

De volgende secties Gebruik [Traffic Manager PowerShell-cmdlets](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/?view=azurermps-6.2.0#traffic_manager).

## <a name="create-azure-resource-group-with-powershell"></a>Azure-resourcegroep maken met PowerShell
Voordat u de Azure-resources maakt, moet u een resourcegroep te bevatten van alle resources maken. De resourcegroep een naam `luis-traffic-manager` en gebruik de regio is `West US`. De regio van de resourcegroep slaat metagegevens over de groep. Dit won't uw resources vertragen als ze in een andere regio. 

Maken van de resourcegroep met **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-6.2.0)** cmdlet:

```PowerShell
New-AzureRmResourceGroup -Name luis-traffic-manager -Location "West US"
```

## <a name="create-luis-keys-to-increase-total-endpoint-quota"></a>LUIS sleutels totale eindpunt quotum te verhogen maken
1. In de Azure-portal maakt twee **Language Understanding** sleutels in de `West US` en één in de `East US`. Bestaande resourcegroep gebruikt, in de vorige sectie hebt gemaakt met de naam `luis-traffic-manager`. 

    ![Schermopname van Azure-portal met twee LUIS sleutels in de resourcegroep luis-verkeer-manager](./media/traffic-manager/luis-keys.png)

2. In de [LUIS] [ LUIS] website op de **publiceren** pagina, sleutels toevoegen aan de app en publiceer de app opnieuw. 

    ![Schermopname van LUIS portal met twee LUIS sleutels op pagina publiceren](./media/traffic-manager/luis-keys-in-luis.png)

    De voorbeeld-URL in de **eindpunt** kolom wordt een aanvraag voor ophalen met de abonnementssleutel gebruikt als een queryparameter. Kopieer de twee nieuwe sleutels eindpunt-URL's. Ze worden gebruikt als onderdeel van het Traffic Manager-configuratie verderop in dit artikel.

## <a name="manage-luis-endpoint-requests-across-keys-with-traffic-manager"></a>LUIS endpoint-aanvragen via sleutels met Traffic Manager beheren
Traffic Manager maakt een nieuwe DNS-toegangspunt voor uw eindpunten. Deze komt niet fungeren als gateway of proxy, maar alleen op het niveau van DNS. In dit voorbeeld is de DNS-records niet gewijzigd. Dit maakt gebruik van een DNS-bibliotheek om te communiceren met Traffic Manager ophalen van het juiste eindpunt voor die specifieke aanvraag. _Elke_ aanvraag die bestemd zijn voor LUIS, moet u eerst een Traffic Manager-verzoek om te bepalen welk LUIS-eindpunt te gebruiken. 

### <a name="polling-uses-luis-endpoint"></a>LUIS eindpunt maakt gebruik van polling
Traffic Manager controleert of de eindpunten regelmatig te controleren of dat het eindpunt is nog steeds beschikbaar. Het Traffic Manager-URL moet toegankelijk zijn met een GET-aanvraag en retourneren een 200 gepeild. De eindpunt-URL op de **publiceren** pagina dit wordt uitgevoerd. Aangezien elke abonnementssleutel een andere route en querytekenreeksparameters heeft, moet elke abonnementssleutel een andere polling-pad. Elke keer dat het Traffic Manager worden opgevraagd, een quotumaanvraag kosten. De querytekenreeksparameter **q** van de LUIS-eindpunt is de utterance naar LUIS verzonden. Een utterance verzenden in plaats van deze parameter wordt gebruikt voor het Traffic Manager polling naar het logboek van LUIS eindpunt toevoegen als een techniek voor foutopsporing tijdens het ophalen van Traffic Manager is geconfigureerd.

Omdat elk eindpunt LUIS een eigen pad moet, moet een eigen Traffic Manager-profiel. Als u wilt beheren via profielen, maken een [ _geneste_ Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-nested-profiles) architectuur. Een profiel van de bovenliggende verwijst naar de onderliggende elementen profielen en beheren van verkeer ertussen.

Zodra de Traffic Manager is geconfigureerd, moet u het pad voor het gebruik van de logboekregistratie = false querytekenreeksparameter zodat uw logboek niet met polling terechtkomen.

## <a name="configure-traffic-manager-with-nested-profiles"></a>Traffic Manager configureren met geneste profielen
De volgende secties maken twee onderliggende profielen, één voor de sleutel Oost LUIS en één voor de sleutel West LUIS. Vervolgens een bovenliggende-profiel is gemaakt en de twee onderliggende profielen worden toegevoegd aan het profiel van de bovenliggende. 

### <a name="create-the-east-us-traffic-manager-profile-with-powershell"></a>VS-Oost Traffic Manager-profiel maken met PowerShell
Als u wilt het VS-Oost Traffic Manager-profiel maakt, zijn verschillende stappen: profiel maken, het eindpunt toevoegen en het eindpunt instellen. Een Traffic Manager-profiel kunt hebben veel eindpunten, maar elk eindpunt heeft hetzelfde validatiepad. Omdat de LUIS eindpunt-URL's voor de abonnementen Oost- en andere vanwege regio en abonnement sleutel, heeft elk eindpunt LUIS moet één eindpunt in het profiel. 

1. Maken van het profiel waarop **[nieuw AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/new-azurermtrafficmanagerprofile?view=azurermps-6.2.0)** cmdlet

    Gebruik de volgende cmdlet om het profiel te maken. Zorg ervoor dat u wijzigt de `appIdLuis` en `subscriptionKeyLuis`. De subscriptionKey is voor de sleutel Oost ons LUIS. Als het pad niet correct is is, met inbegrip van de LUIS app-ID en -abonnement sleutel, de polling Traffic Manager is een status van `degraded` omdat het eindpunt LUIS kan niet met succes verkeer beheren aanvragen. Zorg ervoor dat de waarde van `q` is `traffic-manager-east` zodat u deze waarde in de logboeken van de endpoint LUIS kunt zien.

    ```PowerShell
    $eastprofile = New-AzureRmTrafficManagerProfile -Name luis-profile-eastus -ResourceGroupName luis-traffic-manager -TrafficRoutingMethod Performance -RelativeDnsName luis-dns-eastus -Ttl 30 -MonitorProtocol HTTPS -MonitorPort 443 -MonitorPath "/luis/v2.0/apps/<appID>?subscription-key=<subscriptionKey>&q=traffic-manager-east"
    ```
    
    Deze tabel bevat uitleg over iedere variabele in de cmdlet:
    
    |Configuratieparameter|Naam of waarde variabele|Doel|
    |--|--|--|
    |-Naam|Luis-profiel-eastus|De naam van het Traffic Manager in Azure-portal|
    |-ResourceGroupName|Luis-verkeer-manager|In de vorige sectie hebt gemaakt|
    |-TrafficRoutingMethod|Prestaties|Zie voor meer informatie [methoden voor het doorsturen van Traffic Manager][routing-methods]. Als prestaties wordt gebruikt, moet de URL-aanvraag aan het Traffic Manager afkomstig zijn uit het gebied van de gebruiker. Als er via een chatbot of een andere toepassing, is de chatbot verantwoordelijkheid om na te bootsen de regio in de aanroep naar de Traffic Manager. |
    |-RelativeDnsName|Luis-dns-eastus|Dit is het subdomein voor de service: luis-dns-eastus.trafficmanager.net|
    |-Ttl|30|Polling-interval is 30 seconden|
    |-MonitorProtocol<BR>-MonitorPort|HTTPS<br>443|Poort en het protocol voor LUIS is HTTPS/443|
    |-MonitorPath|`/luis/v2.0/apps/<appIdLuis>?subscription-key=<subscriptionKeyLuis>&q=traffic-manager-east`|Vervang <appIdLuis> en <subscriptionKeyLuis> met uw eigen waarden.|
    
    De aanvraag is gelukt, heeft geen reactie.

2. Toevoegen van de VS-Oost-eindpunt met **[toevoegen AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/add-azurermtrafficmanagerendpointconfig?view=azurermps-6.2.0)** cmdlet

    ```PowerShell
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName luis-east-endpoint -TrafficManagerProfile $eastprofile -Type ExternalEndpoints -Target eastus.api.cognitive.microsoft.com -EndpointLocation "eastus" -EndpointStatus Enabled
    ```
    Deze tabel bevat uitleg over iedere variabele in de cmdlet:

    |Configuratieparameter|Naam of waarde variabele|Doel|
    |--|--|--|
    |-EndpointName|Luis-Oost-eindpunt|Eindpuntnaam weergegeven onder het profiel|
    |-TrafficManagerProfile|$eastprofile|Gebruik profile-object is gemaakt in stap 1|
    |-Type|ExternalEndpoints|Zie voor meer informatie [Traffic Manager-eindpunt][traffic-manager-endpoints] |
    |-Doel|eastus.API.cognitive.Microsoft.com|Dit is het domein voor het eindpunt LUIS.|
    |-EndpointLocation|'eastus'|De regio van het eindpunt|
    |-EndpointStatus|Ingeschakeld|Eindpunt inschakelen wanneer deze wordt gemaakt|

    Geslaagde reactie ziet eruit als:

    ```cmd
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-eastus
    Name                             : luis-profile-eastus
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-eastus
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /luis/v2.0/apps/<luis-app-id>?subscription-key=f0517d185bcf467cba5147d6260bb868&q=traffic-manager-east
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {luis-east-endpoint}
    ```

3. VS-Oost-eindpunt met instellen **[Set AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/set-azurermtrafficmanagerprofile?view=azurermps-6.2.0)** cmdlet

    ```PowerShell
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $eastprofile
    ```

    Een geslaagde reactie is hetzelfde antwoord als stap 2.

### <a name="create-the-west-us-traffic-manager-profile-with-powershell"></a>VS-West Traffic Manager-profiel maken met PowerShell
Volg dezelfde stappen voor het maken van de VS-West Traffic Manager-profiel: profiel maken, het eindpunt toevoegen en het eindpunt instellen.

1. Maken van het profiel waarop **[nieuw AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/New-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)** cmdlet

    Gebruik de volgende cmdlet om het profiel te maken. Zorg ervoor dat u wijzigt de `appIdLuis` en `subscriptionKeyLuis`. De subscriptionKey is voor de sleutel Oost ons LUIS. Als het pad niet correct met inbegrip van de sleutel LUIS app-ID en -abonnement is, de polling Traffic Manager is een status van `degraded` omdat het eindpunt LUIS kan niet met succes verkeer beheren aanvragen. Zorg ervoor dat de waarde van `q` is `traffic-manager-west` zodat u deze waarde in de logboeken van de endpoint LUIS kunt zien.

    ```PowerShell
    $westprofile = New-AzureRmTrafficManagerProfile -Name luis-profile-westus -ResourceGroupName luis-traffic-manager -TrafficRoutingMethod Performance -RelativeDnsName luis-dns-westus -Ttl 30 -MonitorProtocol HTTPS -MonitorPort 443 -MonitorPath "/luis/v2.0/apps/<appIdLuis>?subscription-key=<subscriptionKeyLuis>&q=traffic-manager-west"
    ```
    
    Deze tabel bevat uitleg over iedere variabele in de cmdlet:
    
    |Configuratieparameter|Naam of waarde variabele|Doel|
    |--|--|--|
    |-Naam|Luis-profiel-westus|De naam van het Traffic Manager in Azure-portal|
    |-ResourceGroupName|Luis-verkeer-manager|In de vorige sectie hebt gemaakt|
    |-TrafficRoutingMethod|Prestaties|Zie voor meer informatie [methoden voor het doorsturen van Traffic Manager][routing-methods]. Als prestaties wordt gebruikt, moet de URL-aanvraag aan het Traffic Manager afkomstig zijn uit het gebied van de gebruiker. Als er via een chatbot of een andere toepassing, is de chatbot verantwoordelijkheid om na te bootsen de regio in de aanroep naar de Traffic Manager. |
    |-RelativeDnsName|Luis-dns-westus|Dit is het subdomein voor de service: luis-dns-westus.trafficmanager.net|
    |-Ttl|30|Polling-interval is 30 seconden|
    |-MonitorProtocol<BR>-MonitorPort|HTTPS<br>443|Poort en het protocol voor LUIS is HTTPS/443|
    |-MonitorPath|`/luis/v2.0/apps/<appIdLuis>?subscription-key=<subscriptionKeyLuis>&q=traffic-manager-west`|Vervang <appId> en <subscriptionKey> met uw eigen waarden. Vergeet niet dat de abonnementssleutel voor dit is anders dan de sleutel van het abonnement Oost|
    
    De aanvraag is gelukt, heeft geen reactie.

2. Toevoegen van de VS-West eindpunt met **[toevoegen AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0)** cmdlet

    ```PowerShell
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName luis-west-endpoint -TrafficManagerProfile $westprofile -Type ExternalEndpoints -Target westus.api.cognitive.microsoft.com -EndpointLocation "westus" -EndpointStatus Enabled
    ```

    Deze tabel bevat uitleg over iedere variabele in de cmdlet:

    |Configuratieparameter|Naam of waarde variabele|Doel|
    |--|--|--|
    |-EndpointName|Luis-west-eindpunt|Eindpuntnaam weergegeven onder het profiel|
    |-TrafficManagerProfile|$westprofile|Gebruik profile-object is gemaakt in stap 1|
    |-Type|ExternalEndpoints|Zie voor meer informatie [Traffic Manager-eindpunt][traffic-manager-endpoints] |
    |-Doel|westus.API.cognitive.Microsoft.com|Dit is het domein voor het eindpunt LUIS.|
    |-EndpointLocation|'westus'|De regio van het eindpunt|
    |-EndpointStatus|Ingeschakeld|Eindpunt inschakelen wanneer deze wordt gemaakt|

    Geslaagde reactie ziet eruit als:

    ```cmd
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-westus
    Name                             : luis-profile-westus
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-westus
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /luis/v2.0/apps/c3fc5d1e-5187-40cc-af0f-fbde328aa16b?subscription-key=e3605f07e3cc4bedb7e02698a54c19cc&q=traffic-manager-west
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {luis-west-endpoint}
    ```

3. VS-West eindpunt met instellen **[Set AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Set-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)** cmdlet

    ```PowerShell
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $westprofile
    ```

    Een geslaagde reactie is hetzelfde antwoord als stap 2.

### <a name="create-parent-traffic-manager-profile"></a>Bovenliggende Traffic Manager-profiel maken
De bovenliggende Traffic Manager-profiel maken en twee onderliggende Traffic Manager-profielen koppelen aan het bovenliggende item.

1. Maken van de bovenliggende profiel met **[nieuw AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/New-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)** cmdlet

    ```PowerShell
    $parentprofile = New-AzureRmTrafficManagerProfile -Name luis-profile-parent -ResourceGroupName luis-traffic-manager -TrafficRoutingMethod Performance -RelativeDnsName luis-dns-parent -Ttl 30 -MonitorProtocol HTTPS -MonitorPort 443 -MonitorPath "/"
    ```

    Deze tabel bevat uitleg over iedere variabele in de cmdlet:

    |Configuratieparameter|Naam of waarde variabele|Doel|
    |--|--|--|
    |-Naam|Luis profiel bovenliggende|De naam van het Traffic Manager in Azure-portal|
    |-ResourceGroupName|Luis-verkeer-manager|In de vorige sectie hebt gemaakt|
    |-TrafficRoutingMethod|Prestaties|Zie voor meer informatie [methoden voor het doorsturen van Traffic Manager][routing-methods]. Als prestaties wordt gebruikt, moet de URL-aanvraag aan het Traffic Manager afkomstig zijn uit het gebied van de gebruiker. Als er via een chatbot of een andere toepassing, is de chatbot verantwoordelijkheid om na te bootsen de regio in de aanroep naar de Traffic Manager. |
    |-RelativeDnsName|Luis-dns-bovenliggende|Dit is het subdomein voor de service: luis-dns-parent.trafficmanager.net|
    |-Ttl|30|Polling-interval is 30 seconden|
    |-MonitorProtocol<BR>-MonitorPort|HTTPS<br>443|Poort en het protocol voor LUIS is HTTPS/443|
    |-MonitorPath|`/`|Dit pad is niet van belang omdat het onderliggende eindpunt paden in plaats daarvan worden gebruikt.|

    De aanvraag is gelukt, heeft geen reactie.

2. VS-Oost onderliggende profiel toevoegen aan bovenliggende met **[toevoegen AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0)** en **NestedEndpoints** type

    ```PowerShell
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint-useast -TrafficManagerProfile $parentprofile -Type NestedEndpoints -TargetResourceId $eastprofile.Id -EndpointStatus Enabled -EndpointLocation "eastus" -MinChildEndpoints 1
    ```

    Deze tabel bevat uitleg over iedere variabele in de cmdlet:

    |Configuratieparameter|Naam of waarde variabele|Doel|
    |--|--|--|
    |-EndpointName|onderliggende-eindpunt-useast|Oost-profiel|
    |-TrafficManagerProfile|$parentprofile|Profiel toewijzen dit eindpunt|
    |-Type|NestedEndpoints|Zie voor meer informatie [toevoegen AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0). |
    |-TargetResourceId|$eastprofile. ID|ID van het onderliggende profiel|
    |-EndpointStatus|Ingeschakeld|Status endpoint na het toevoegen aan de bovenliggende|
    |-EndpointLocation|'eastus'|[De naam van de Azure-regio](https://azure.microsoft.com/global-infrastructure/regions/) van bron|
    |-MinChildEndpoints|1|Minimumaantal onderliggende eindpunten|

    De weergave geslaagd antwoord, zoals het volgende en bevat de nieuwe `child-endpoint-useast` eindpunt:    

    ```cmd
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-parent
    Name                             : luis-profile-parent
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-parent
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {child-endpoint-useast}
    ```

3. VS-West onderliggende profiel toevoegen aan bovenliggende met **[toevoegen AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0)** cmdlet en **NestedEndpoints** type

    ```PowerShell
    Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint-uswest -TrafficManagerProfile $parentprofile -Type NestedEndpoints -TargetResourceId $westprofile.Id -EndpointStatus Enabled -EndpointLocation "westus" -MinChildEndpoints 1
    ```

    Deze tabel bevat uitleg over iedere variabele in de cmdlet:

    |Configuratieparameter|Naam of waarde variabele|Doel|
    |--|--|--|
    |-EndpointName|onderliggende-eindpunt-uswest|West profiel|
    |-TrafficManagerProfile|$parentprofile|Profiel toewijzen dit eindpunt|
    |-Type|NestedEndpoints|Zie voor meer informatie [toevoegen AzureRmTrafficManagerEndpointConfig](https://docs.microsoft.com/powershell/module/azurerm.trafficmanager/Add-AzureRmTrafficManagerEndpointConfig?view=azurermps-6.2.0). |
    |-TargetResourceId|$westprofile. ID|ID van het onderliggende profiel|
    |-EndpointStatus|Ingeschakeld|Status endpoint na het toevoegen aan de bovenliggende|
    |-EndpointLocation|'westus'|[De naam van de Azure-regio](https://azure.microsoft.com/global-infrastructure/regions/) van bron|
    |-MinChildEndpoints|1|Minimumaantal onderliggende eindpunten|

    De weergave geslaagd antwoord, zoals en omvat zowel de vorige `child-endpoint-useast` eindpunt en de nieuwe `child-endpoint-uswest` eindpunt:

    ```cmd
    Id                               : /subscriptions/<azure-subscription-id>/resourceGroups/luis-traffic-manager/providers/Microsoft.Network/trafficManagerProfiles/luis-profile-parent
    Name                             : luis-profile-parent
    ResourceGroupName                : luis-traffic-manager
    RelativeDnsName                  : luis-dns-parent
    Ttl                              : 30
    ProfileStatus                    : Enabled
    TrafficRoutingMethod             : Performance
    MonitorProtocol                  : HTTPS
    MonitorPort                      : 443
    MonitorPath                      : /
    MonitorIntervalInSeconds         : 30
    MonitorTimeoutInSeconds          : 10
    MonitorToleratedNumberOfFailures : 3
    Endpoints                        : {child-endpoint-useast, child-endpoint-uswest}
    ```

4. Instellen van de eindpunten met **[Set AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Set-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)** cmdlet 

    ```PowerShell
    Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $parentprofile
    ```

    Een geslaagde reactie is het antwoord van stap 3.

### <a name="powershell-variables"></a>PowerShell-variabelen
In de vorige secties drie PowerShell variabelen zijn gemaakt: `$eastprofile`, `$westprofile`, `$parentprofile`. Deze variabelen worden gebruikt aan het einde van de Traffic Manager-configuratie. Als u wilt maken van de variabelen niet of bent vergeten, of uw PowerShell-venster een optreedt time-out, kunt u de PowerShell-cmdlet  **[Get-AzureRmTrafficManagerProfile](https://docs.microsoft.com/powershell/module/AzureRM.TrafficManager/Get-AzureRmTrafficManagerProfile?view=azurermps-6.2.0)**, zodat het profiel opnieuw ophalen en toe te wijzen aan een variabele. 

Vervang de items in de vierkante haken, `<>`, met de juiste waarden voor elk van de drie profielen die u nodig hebt. 

```PowerShell
$<variable-name> = Get-AzureRmTrafficManagerProfile -Name <profile-name> -ResourceGroupName luis-traffic-manager
```

## <a name="verify-traffic-manager-works"></a>Controleer of het Traffic Manager werkt
Om te controleren of de Traffic Manager-profielen werken, de profielen moet beschikken over de status van `Online` deze status is gebaseerd op het polling-pad van het eindpunt. 

### <a name="view-new-profiles-in-the-azure-portal"></a>Nieuwe profielen weergeven in de Azure portal
U kunt controleren dat alle drie de profielen zijn gemaakt door te kijken naar de resources in de `luis-traffic-manager` resourcegroep.

![Schermopname van Azure resource group luis-verkeer-manager](./media/traffic-manager/traffic-manager-profiles.png)

### <a name="verify-the-profile-status-is-online"></a>Controleer of dat de status van het profiel is Online
Het Traffic Manager controleert of het pad van elk eindpunt om te controleren of dat deze online is. Als deze online is, wordt de status van de onderliggende profielen zijn `Online`. Dit wordt weergegeven op de **overzicht** van elk profiel. 

![Schermopname van Azure Traffic Manager-profiel weer met Monitor Status van Online overzicht](./media/traffic-manager/profile-status-online.png)

### <a name="validate-traffic-manager-polling-works"></a>Traffic Manager werkt polling valideren
Valideren van de traffic manager polling werkt op een andere manier is met de LUIS endpoint-Logboeken. Op de [LUIS] [ LUIS] lijst met apps van website pagina, exporteren van het logboek eindpunt voor de toepassing. Omdat het Traffic Manager vaak worden opgevraagd voor de twee eindpunten, zijn er vermeldingen in de logboeken zelfs als ze alleen op enkele minuten duren zijn. Vergeet niet om te zoeken naar gegevens waarbij de query met begint `traffic-manager-`.

```text
traffic-manager-west    6/7/2018 19:19  {"query":"traffic-manager-west","intents":[{"intent":"None","score":0.944767}],"entities":[]}
traffic-manager-east    6/7/2018 19:20  {"query":"traffic-manager-east","intents":[{"intent":"None","score":0.944767}],"entities":[]}
```

### <a name="validate-dns-response-from-traffic-manager-works"></a>DNS-antwoord van Traffic Manager werkt valideren
Aanvragen om te valideren dat de DNS-antwoord een eindpunt LUIS retourneert, het profiel voor het beheren van verkeer bovenliggende DNS met een DNS-clientbibliotheek. De DNS-naam voor het profiel van de bovenliggende `luis-dns-parent.trafficmanager.net`.

De volgende Node.js-code doet een aanvraag voor het profiel van de bovenliggende en een eindpunt LUIS geretourneerd:

```javascript
const dns = require('dns');

dns.resolveAny('luis-dns-parent.trafficmanager.net', (err, ret) => {
  console.log('ret', ret);
});
```

Het antwoord van de geslaagde met het eindpunt LUIS is:

```cmd
[
    {
        value: 'westus.api.cognitive.microsoft.com', 
        type: 'CNAME'
    }
]
```

## <a name="use-the-traffic-manager-parent-profile"></a>Het profiel van de bovenliggende Traffic Manager gebruiken
Om verkeer tussen de eindpunten te beheren, moet u een aanroep naar de DNS Traffic Manager vinden van het eindpunt LUIS invoegen. Deze aanroep is gemaakt voor elke aanvraag LUIS-eindpunt en moet de geografische locatie van de gebruiker van de clienttoepassing LUIS simuleren. De DNS-antwoordcode tussen uw clienttoepassing LUIS en de aanvraag aan LUIS voor de voorspelling eindpunt toevoegen 


## <a name="clean-up"></a>Opruimen
De twee LUIS abonnement sleutels, de drie Traffic Manager-profielen en de resourcegroep die deel uitmaakt van deze vijf resources verwijderen. Dit wordt gedaan via de Azure-portal. U kunt de vijf resources verwijderen uit de lijst met resources. Verwijder de resourcegroep. 

## <a name="next-steps"></a>Volgende stappen

Bekijk [middleware](https://docs.microsoft.com/azure/bot-service/bot-builder-create-middleware?view=azure-bot-service-4.0&tabs=csaddmiddleware%2Ccsetagoverwrite%2Ccsmiddlewareshortcircuit%2Ccsfallback%2Ccsactivityhandler) opties in BotFramework v4 om te begrijpen hoe deze management verkeer code kan worden toegevoegd aan een bot BotFramework. 

[traffic-manager-marketing]: https://azure.microsoft.com/services/traffic-manager/
[traffic-manager-docs]: https://docs.microsoft.com/azure/traffic-manager/
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
[azure-portal]: https://portal.azure.com/
[azure-storage]: https://azure.microsoft.com/services/storage/
[routing-methods]: https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods
[traffic-manager-endpoints]: https://docs.microsoft.com/azure/traffic-manager/traffic-manager-endpoint-types
