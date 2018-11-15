---
title: Faseringsomgevingen voor web-apps in Azure App Service instellen | Microsoft Docs
description: Leer hoe u gefaseerde publicatie gebruiken voor web-apps in Azure App Service.
services: app-service
documentationcenter: ''
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: e224fc4f-800d-469a-8d6a-72bcde612450
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
ms.openlocfilehash: b5a06cff653007568b4ab2b44624b6314413f8a6
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636064"
---
# <a name="set-up-staging-environments-in-azure-app-service"></a>Faseringsomgevingen in Azure App Service instellen
<a name="Overview"></a>

Wanneer u uw web-app, web-app op Linux-, mobiele back-end- en API-app implementeert [App Service](https://go.microsoft.com/fwlink/?LinkId=529714), u kunt implementeren op een afzonderlijke implementatiesite in plaats van de standaard-productiesite bij uitvoering in de **Standard**, **Premium**, of **geïsoleerd** laag voor App Service-plan. Implementatiesleuven zijn daadwerkelijk live apps met hun eigen hostnamen. App-inhoud en configuratie-elementen kunnen worden gewisseld tussen twee implementatiesites, inclusief de productiesite. Implementeren van uw toepassing naar een implementatiesite heeft de volgende voordelen:

* U kunt app-wijzigingen in een gefaseerde installatie implementatiesleuf valideren voordat deze wisselen met de productiesite.
* Zorgt ervoor dat alle exemplaren van de site zijn opgewarmd voordat ze in productie wordt komen eerst een app implementeren op een site en deze in productie komt. Dit veel minder uitvaltijd wanneer u uw app implementeert. Het omleiden van verkeer is naadloos en er zijn geen aanvragen worden verwijderd als gevolg van bewerkingen voor wisselen. Deze volledige werkstroom kan worden geautomatiseerd door het configureren van [automatisch wisselen](#Auto-Swap) wanneer vooraf swap-validatie is niet nodig.
* De site met een eerder voorbereide app heeft nu de vorige productie-app na een wisselen. Als de wijzigingen in de productiesite gewisseld zijn niet zoals u verwacht, kunt u de dezelfde swap onmiddellijk als u uw 'laatst bekende goede site' uitvoeren terug.

Elke laag van App Service-plan ondersteunt een verschillend aantal implementatiesites. Om erachter te komen het aantal sleuven van uw app-laag ondersteunt, Zie [limieten van App Service](https://docs.microsoft.com/azure/azure-subscription-service-limits#app-service-limits). Als u wilt schalen van uw app naar een andere laag, moet de doel-laag ondersteuning voor het aantal sleuven die uw app al gebruikmaakt. Bijvoorbeeld, als uw app meer dan 5 sleuven heeft, u kunt geen de schaal omlaag naar **Standard** tier, omdat **Standard** laag biedt alleen ondersteuning voor 5 implementatiesites.

<a name="Add"></a>

## <a name="add-a-deployment-slot"></a>Een implementatiesite toevoegen
De app moet worden uitgevoerd de **Standard**, **Premium**, of **geïsoleerd* laag zodat u meerdere implementatiesites inschakelen.

1. In de [Azure Portal](https://portal.azure.com/), opent u van uw app [resourceblade](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Kies de **implementatiesites** optie en klik vervolgens op **sleuf toevoegen**.
   
    ![Een nieuwe implementatiesite toevoegen][QGAddNewDeploymentSlot]
   
   > [!NOTE]
   > Als de app nog niet in de **Standard**, **Premium**, of **geïsoleerd* laag, ontvangt u een bericht weergegeven dat aangeeft van de ondersteunde lagen voor het inschakelen van gefaseerd publiceren. Op dit moment hebt u de optie te selecteren **Upgrade** en navigeer naar de **schaal** tab van uw app voordat u doorgaat.
   > 
   > 
3. In de **een sleuf toevoegen** blade, Geef een naam op voor de site en selecteer of u wilt klonen appconfiguratie vanaf een andere bestaande implementatiesite. Klik op het vinkje om door te gaan.
   
    ![Configuratiebron][ConfigurationSource1]
   
    De eerste keer dat u een site toevoegt, alleen hebt u twee opties: de configuratie van de kloon van de standaard-sleuf in productie of helemaal niet.
    Nadat u verschillende sleuven hebt gemaakt, kunt u zich voor het klonen van de configuratie van een site dan de regio in de productieomgeving:
   
    ![Configuratie van bronnen][MultipleConfigurationSources]
4. Klik in de resource-blade van uw app, op **implementatiesites**, klikt u vervolgens op een implementatiesite als u wilt dat de sleuf van resource-blade geopend met een set metrische gegevens en configuratie net als elke andere app. De naam van de site wordt weergegeven aan de bovenkant van de blade om u te herinneren dat u de implementatiesleuf bekijkt.
   
    ![Titel van de implementatie-sleuf][StagingTitle]
5. Klik op de URL van de app in de blade van de site. U ziet de implementatiesite heeft een eigen hostnaam en is ook een live-app. Als u wilt beperken openbare toegang tot de implementatiesleuf, Zie [App Service Web-App-blok webtoegang tot niet-productie-implementatiesleuven](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

Er is geen inhoud na implementatie sleuf maken. U kunt implementeren op de site van een andere opslagplaats vertakking, of een geheel andere opslagplaats. U kunt ook wijzigen van de site-configuratie. Gebruik het publiceren profiel of de implementatie-referenties die zijn gekoppeld aan de implementatiesite voor updates van inhoud.  U kunt bijvoorbeeld [publiceren naar deze sleuf met git](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>

## <a name="which-settings-are-swapped"></a>Welke instellingen worden gewisseld?
Wanneer u de configuratie van een andere implementatiesite kloont, wordt de configuratie van de gekloonde kan worden bewerkt. Bepaalde configuratie-elementen wordt bovendien de inhoud volgen via een wisselbestand (kan niet in sleuf worden specifieke) terwijl andere configuratie-elementen in dezelfde sleuf na een (sleuf specifieke) voor wisselen blijven. De volgende lijsten ziet u de instellingen die wijzigen wanneer u sleuven.

**Instellingen die worden gewisseld**:

* Algemene instellingen -, framework-versie, 64-32-bits, zoals Web sockets
* App-instellingen (kan worden geconfigureerd voor het gekozen voor een site)
* Verbindingsreeksen (kan worden geconfigureerd voor het gekozen voor een site)
* Handlertoewijzingen
* Instellingen voor controle en diagnose
* WebJobs-inhoud

**Instellingen die niet zijn gewisseld**:

* Publicatie-eindpunten
* Aangepaste domeinnamen
* SSL-certificaten en -bindingen
* Schaalinstellingen
* WebJobs-planners

Voor het configureren van een app-instelling of de verbindingsreeks aan een site (niet wisselen), toegang krijgen tot de **toepassingsinstellingen** blade voor een specifieke site, selecteer vervolgens de **site-instelling** in voor de configuratie elementen die behouden in de sleuf blijven moeten. Een configuratie-element als sleuf specifieke markeert het effect van het tot stand brengen van dat element als niet swappable over alle implementatiesites die zijn gekoppeld aan de app heeft.

![Site-instellingen][SlotSettings]

<a name="Swap"></a>

## <a name="swap-deployment-slots"></a>Implementatiesites wisselen 
U kunt implementatie sleuven in de **overzicht** of **implementatiesites** weergave van de resource-blade van uw app.

> [!IMPORTANT]
> Voordat u een app uit een implementatiesite naar productie wisselen, zorg ervoor dat alle niet-sleuf specifieke instellingen zijn geconfigureerd, precies zoals u wilt dat dit in de doel-wisselen.
> 
> 

1. Als u wilt implementatiesites wisselen, klikt u op de **wisselen** knop in de opdrachtbalk van de app of in de opdrachtbalk van een implementatiesite.
   
    ![Knop wisselen][SwapButtonBar]

2. Zorg ervoor dat het wisselen van bron- en wisselen correct zijn ingesteld. Het doel van het wisselbestand is meestal de productiesite. Klik op **OK** om de bewerking te voltooien. Wanneer de bewerking is voltooid, zijn de implementatiesites zijn gewisseld.

    ![Wisseling voltooien](./media/web-sites-staged-publishing/SwapImmediately.png)

    Voor de **wisselen met preview** type wisseling, Zie [wisselen met Preview-versie (meerdere fase swap)](#Multi-Phase).  

<a name="Multi-Phase"></a>

## <a name="swap-with-preview-multi-phase-swap"></a>Wisselen met Preview-versie (meerdere fase swap)

Wisselen met preview of meerdere fase wisselen, Vereenvoudig de validatie van de site-specifieke configuratie-elementen, zoals verbindingsreeksen.
Voor uw essentiële workloads, u wilt valideren dat de app zich gedraagt zoals verwacht als de productiesite configuratie is toegepast, en moet u deze validatie uitvoeren *voordat* de app in productie wordt gewisseld. Wisselen met Preview-versie is wat u nodig hebt.

> [!NOTE]
> Wisselen met Preview-versie wordt niet ondersteund in web App on Linux.

Wanneer u gebruikt de **wisseling met voorbeeld** optie (Zie [implementatiesites wisselen](#Swap)), App Service doet het volgende:

- Houdt de bestemmingssleuf niet worden gewijzigd, zodat bestaande werkbelasting op dat de sleuf (zoals een productieomgeving) wordt niet negatief beïnvloed.
- Geldt de configuratie-elementen van de doelsleuf voor de bron-sleuf, met inbegrip van de site-specifieke verbindingsreeksen en app-instellingen.
- De werkprocessen op de bron-site met behulp van deze elementen van de hiervoor genoemde configuratie opnieuw is opgestart.
- Wanneer u de wisseling voltooien: de bron van de pre-warmed-up-sleuf is verplaatst naar de doelsleuf. De doelsleuf is verplaatst naar de bron-sleuf zoals in een handmatige wisselen.
- Wanneer u de wisseling annuleren: de configuratie-elementen van de bron-sleuf opnieuw naar de bronsite.

U kunt een voorbeeld precies hoe de app wordt uitgevoerd met de configuratie van de doelsleuf. Nadat u de validatie is voltooid, kunt u de wisseling in een aparte stap voltooien. Deze stap heeft het bijkomende voordeel dat de bron-sleuf al is opgewarmd met de gewenste configuratie en clients geen geen downtime.  

Voorbeelden voor Azure PowerShell-cmdlets beschikbaar voor meerdere fase wisselen zijn opgenomen in de Azure PowerShell-cmdlets voor implementatie sleuven sectie.

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a>Automatisch wisselen configureren
Automatisch wisselen stroomlijnt DevOps-scenario's waar u uw app met nul koude start en zonder downtime continu implementeren voor eindgebruikers van de app. Wanneer een implementatiesite is geconfigureerd voor automatisch wisselen in productie nemen, telkens wanneer u de update-code naar die site pushen, wordt App Service automatisch de app wisselen in productie nadat deze al in de sleuf is opgewarmd.

> [!IMPORTANT]
> Wanneer u automatisch wisselen voor een site inschakelt, zorg er dan voor dat de configuratie van de implementatiesite is exact de configuratie die bestemd zijn voor de doelsleuf (meestal de productiesite).
> 
> 

> [!NOTE]
> Automatisch wisselen wordt niet ondersteund in web App on Linux.

Configureren van automatische wisselen voor een site is eenvoudig. Volg deze stappen:

1. In **Implementatiesites**, selecteert u een niet-productie-site en kies **toepassingsinstellingen** op de resourceblade van die site.  
   
    ![][Autoswap1]
2. Selecteer **op** voor **automatisch wisselen**, selecteer de gewenste doelsleuf in **sleuf voor automatisch wisselen**, en klikt u op **opslaan** in de opdrachtbalk. Zorg ervoor dat de configuratie voor de sleuf is exact de configuratie die bestemd zijn voor de doelsleuf.
   
    De **meldingen** tabblad knippert een groene **SUCCES** zodra de bewerking voltooid is.
   
    ![][Autoswap2]
   
   > [!NOTE]
   > Als u wilt testen automatisch wisselen voor uw app, kunt u eerst een niet-productie-doelsleuf in selecteren **sleuf voor automatisch wisselen** om vertrouwd te raken met de functie.  
   > 
   > 
3. Een code pushen naar deze implementatiesite worden uitgevoerd. Automatisch wisselen gebeurt er na een korte periode en de update wordt weergegeven op de URL van de doelsleuf.

<a name="Rollback"></a>

## <a name="roll-back-a-production-app-after-swap"></a>Terugdraaien van een productie-app na wisselen
Als er fouten worden geïdentificeerd in de productieomgeving na een wisselen van sleuf, terugdraaien de sleuven op hun status vooraf wisselen door de dezelfde twee sleuven onmiddellijk wisselen.

<a name="Warm-up"></a>

## <a name="custom-warm-up-before-swap"></a>Aangepaste warmen voordat wisselen
Sommige apps mogelijk aangepaste warmen acties. De `applicationInitialization` configuratie-element in web.config kunt u opgeven de initialisatie van de aangepaste acties worden uitgevoerd voordat een aanvraag wordt ontvangen. De wisselbewerking moet wachten voor deze aangepaste warmen om te voltooien. Hier volgt een voorbeeld van web.config fragment.

    <system.webServer>
        <applicationInitialization>
            <add initializationPage="/" hostName="[app hostname]" />
            <add initializationPage="/Home/About" hostname="[app hostname]" />
        </applicationInitialization>
    </system.webServer>

## <a name="monitor-swap-progress"></a>De voortgang van de wisseling

Wisselen van de duurt soms even om te voltooien, zoals wanneer de app die wordt gewisseld bestaat uit een tijd lang warmen. Krijgt u meer informatie over het wisselen van bewerkingen in de [activiteitenlogboek](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) in de [Azure-portal](https://portal.azure.com).

Selecteer in de app-pagina van de portal, in de navigatiebalk links **activiteitenlogboek**.

Een wisselbewerking wordt weergegeven in het logboekquery als `Slotsswap`. U kunt deze uitvouwen en selecteer een van de suboperations of fouten om de details te bekijken.

![Activiteitenlogboek voor wisselen van sleuf](media/web-sites-staged-publishing/activity-log.png)

<a name="Delete"></a>

## <a name="delete-a-deployment-slot"></a>Een implementatiesite verwijderen
In de blade voor een implementatiesite, de implementatiesleuf-blade te openen, klikt u op **overzicht** (de standaardpagina), en klikt u op **verwijderen** in de opdrachtbalk.  

![Een Implementatiesite verwijderen][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="automate-with-azure-powershell"></a>Automatiseren met Azure PowerShell

Azure PowerShell is een module die cmdlets voor het beheren van Azure via Windows PowerShell, inclusief ondersteuning voor het beheren van implementatiesites gebruiken in Azure App Service biedt.

* Zie voor informatie over het installeren en configureren van Azure PowerShell, en verificatie van Azure PowerShell met uw Azure-abonnement, [installeren en configureren van Microsoft Azure PowerShell](/powershell/azure/overview).  

- - -
### <a name="create-a-web-app"></a>Een webtoepassing maken
```PowerShell
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

- - -
### <a name="create-a-deployment-slot"></a>Een implementatiesite maken
```PowerShell
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

- - -
### <a name="initiate-a-swap-with-preview-multi-phase-swap-and-apply-destination-slot-configuration-to-source-slot"></a>Starten van een wisseling met Preview-versie (meerdere fase swap) en doel-siteconfiguratie van toepassing op de bronsite
```PowerShell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a>Annuleren van een in behandeling (wisselen met controleren) voor wisselen en bron siteconfiguratie herstellen
```PowerShell
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

- - -
### <a name="swap-deployment-slots"></a>Implementatiesites wisselen
```PowerShell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

### <a name="monitor-swap-events-in-the-activity-log"></a>Swap-gebeurtenissen in het activiteitenlogboek controleren
```PowerShell
Get-AzureRmLog -ResourceGroup [resource group name] -StartTime 2018-03-07 -Caller SlotSwapJobProcessor  
```

- - -
### <a name="delete-deployment-slot"></a>Implementatiesleuf verwijderen
```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

- - -
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="automate-with-azure-cli"></a>Automatiseren met Azure CLI

Voor [Azure CLI](https://github.com/Azure/azure-cli) opdrachten voor implementatiesites, Zie [az webapp deployment slot](/cli/azure/webapp/deployment/slot).

## <a name="next-steps"></a>Volgende stappen
[Azure App Service Web App-web-toegang tot niet-productie-implementatiesleuven blokkeren](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)  
[Inleiding tot App Service on Linux](../app-service/containers/app-service-linux-intro.md)  
[Gratis proefversie van Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png

