---
title: Faseringsomgevingen voor web-apps in Azure App Service instellen | Microsoft Docs
description: Informatie over het gebruiken van voorbereide publiceren voor web-apps in Azure App Service.
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
ms.openlocfilehash: 2fabf0d61ffd2f526fab49816eab36a86497a358
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="set-up-staging-environments-in-azure-app-service"></a>Faseringsomgevingen in Azure App Service instellen
<a name="Overview"></a>

Wanneer u uw web-app, web-app op Linux-, mobiele back-end- en API-app implementeert [App Service](http://go.microsoft.com/fwlink/?LinkId=529714), u kunt implementeren op een afzonderlijke implementatiesleuf in plaats van de productiesite standaard bij uitvoering in de **standaard** of **Premium** laag van App Service-plan. Implementatiesites zijn daadwerkelijk live apps met hun eigen hostnamen. App-inhoud en configuratie-elementen worden ingewisseld tussen twee implementatiesites, met inbegrip van de productiesite. Uw toepassing om een implementatiesleuf te implementeren, heeft de volgende voordelen:

* U kunt appwijzigingen in een gefaseerde installatie implementatiesleuf valideren voordat het wisselen met de productiesite.
* Zorgt ervoor dat alle exemplaren van de sleuf zijn opgewarmd voordat gewisseld naar de productie implementeert eerst een app in een sleuf en deze in productie te wisselen. Hierdoor is er uitvaltijd wanneer u uw app implementeert. Het verkeer omleiden naadloze is en er zijn geen aanvragen worden verwijderd als gevolg van de wisseling bewerkingen. Deze volledige werkstroom kan worden geautomatiseerd door configureren [automatisch wisselen](#Auto-Swap) wanneer vooraf swap-validatie is niet nodig.
* Als een wisseling heeft de sleuf met eerder voorbereide app de vorige productie-app. Als de gewisseld naar de productiesite wijzigingen zijn niet zoals u verwacht, kunt u de dezelfde swap onmiddellijk om uw "laatst bekende goede site" uitvoeren terug.

Elke laag van App Service plan ondersteunt een verschillend aantal implementatiesites. Sleuven om erachter te komen het nummer van uw app-laag ondersteunt, Zie [App Servicelimieten](https://docs.microsoft.com/azure/azure-subscription-service-limits#app-service-limits). Als u wilt schalen van uw app met een andere tier, moet de doel-laag het nummer van uw app gebruikmaakt van al sleuven ondersteunen. Bijvoorbeeld als uw app meer dan 5 sleuven heeft, u niet kunt schalen omlaag naar **standaard** laag, omdat **standaard** laag ondersteunt alleen 5 implementatiesites.

<a name="Add"></a>

## <a name="add-a-deployment-slot"></a>Een implementatiesleuf toevoegen
De app moet worden uitgevoerd in de **standaard** of **Premium** laag zodat u meerdere implementatiesites inschakelen.

1. In de [Azure Portal](https://portal.azure.com/), opent u uw app [resourceblade](../azure-resource-manager/resource-group-portal.md#manage-resources).
2. Kies de **implementatiesites** optie en klik vervolgens op **sleuf toevoegen**.
   
    ![Voeg een nieuwe implementatiesleuf][QGAddNewDeploymentSlot]
   
   > [!NOTE]
   > Als de app nog niet in de **standaard** of **Premium** laag, ontvangt u een bericht met de ondersteunde lagen voor het inschakelen van voorbereide publiceren. Op dit moment hebt u de optie te selecteren **Upgrade** en navigeer naar de **Scale** tabblad van uw app voordat u doorgaat.
   > 
   > 
3. In de **toevoegen van een sleuf** blade een naam opgeven voor de sleuf en selecteren of u de configuratie van de app uit een andere bestaande implementatiesleuf klonen. Klik op het vinkje om door te gaan.
   
    ![Configuratiebron][ConfigurationSource1]
   
    De eerste keer dat u een site toevoegt, alleen hebt u twee mogelijkheden: configuratie van de kloon van de standaard-sleuf in productie of helemaal niet.
    Wanneer u meerdere sleuven hebt gemaakt, kunt u zich kunt kloon configuratie uit een sleuf dan die in productie:
   
    ![Configuratie van bronnen][MultipleConfigurationSources]
4. Klik in de resourceblade van de app, op **implementatiesites**, klikt u vervolgens op een implementatiesleuf die sleuf resource om blade te openen, met een set van metrische gegevens en configuratie net als elke andere app. De naam van de site wordt weergegeven boven aan de blade om eraan te herinneren dat u de implementatiesleuf bekijkt.
   
    ![Implementatiesleuf titel][StagingTitle]
5. Klik op de app-URL in de sleuf blade. U ziet de implementatiesleuf heeft een eigen hostnaam en is ook een live-app. Als u wilt beperken voor openbare toegang tot de implementatiesleuf, Zie [App Service Web-App – blok webtoegang tot niet-productieve implementatiesites](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

Er is geen inhoud nadat de implementatiesleuf is gemaakt. U kunt implementeren op de sleuf van een andere opslagplaats vertakking of een geheel andere opslagplaats. U kunt ook de sleuf configuratie wijzigen. Publiceren of de implementatie referenties gebruiken die zijn gekoppeld aan de implementatiesleuf voor updates van inhoud.  U kunt bijvoorbeeld [publiceren naar deze sleuf met git](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>

## <a name="which-settings-are-swapped"></a>Welke instellingen worden omgewisseld?
Als u een configuratie uit een andere implementatiesleuf kloon, kan de configuratie van de gekloonde worden bewerkt. Bepaalde configuratie-elementen wordt bovendien de inhoud via een wisseling (kan niet in sleuf worden specifieke) gevolgd terwijl andere configuratie-elementen staan in dezelfde sleuf zijn na een wisseling (sleuf specifieke blijft). De volgende lijsten ziet de instellingen die wijzigen wanneer u sleuven wisselen.

**Instellingen die worden omgewisseld**:

* Algemene instellingen - zoals framework-versie, 64-32-bits, Web-sockets
* App-instellingen (kan worden geconfigureerd om te blijven hangen in een sleuf)
* Verbindingsreeksen (kan worden geconfigureerd om te blijven hangen in een sleuf)
* Handlertoewijzingen
* Instellingen voor controle en diagnose
* WebJobs-inhoud

**Instellingen die niet worden omgewisseld**:

* Publicatie-eindpunten
* Aangepaste domeinnamen
* SSL-certificaten en bindingen
* Schaalinstellingen
* WebJobs planners

Voor het configureren van een app-instelling of verbindingstekenreeks aan een site (niet verwisseld) toegang krijgen tot de **toepassingsinstellingen** blade voor een specifieke site, selecteer vervolgens de **sleuf instelling** in voor de configuratie elementen die de sleuf moeten houden. Een configuratie-element markeren als sleuf specifieke heeft het effect van het tot stand brengen van dat element als niet verwisselbare over alle implementatiesites die zijn gekoppeld aan de app.

![Sleufinstellingen][SlotSettings]

<a name="Swap"></a>

## <a name="swap-deployment-slots"></a>Wisselen implementatiesites 
U kunt wisselen implementatiesites in de **overzicht** of **implementatiesites** weergave van de resource-blade van uw app.

> [!IMPORTANT]
> Voordat u een app van een implementatiesleuf naar de productie wisselen, zorg ervoor dat alle niet-specifieke sleufinstellingen precies zoals u wilt dat dit in de doel-wisseling zijn geconfigureerd.
> 
> 

1. Het wisselen van implementatiesites, klikt u op de **wisselen** knop in de opdrachtbalk van de app of in de opdrachtbalk van een implementatiesleuf.
   
    ![Knop wisselen][SwapButtonBar]

2. Zorg ervoor dat de wisseling bron en het doel van de wisseling correct zijn ingesteld. Het doel van de wisselruimte is meestal de productiesite. Klik op **OK** om de bewerking te voltooien. Wanneer de bewerking is voltooid, hebben de implementatiesites zijn gewisseld.

    ![Wisseling voltooien](./media/web-sites-staged-publishing/SwapImmediately.png)

    Voor de **wisseling met voorbeeld** type wisselen, Zie [wisseling met voorbeeld (meerdere fase swap)](#Multi-Phase).  

<a name="Multi-Phase"></a>

## <a name="swap-with-preview-multi-phase-swap"></a>Wisselen met voorbeeld (meerdere fase swap)

Wisselen met voorbeeld of meerdere stap wisseling, validatie van de site-specifieke configuratie-elementen, zoals verbindingsreeksen vereenvoudigen.
Bedrijfskritieke werkbelastingen, u wilt valideren dat de app werkt zoals verwacht wanneer de productiesite configuratie is toegepast en moet u deze validatie uitvoeren *voordat* de app wordt gewisseld naar de productie. Wisseling met voorbeeld is wat u nodig hebt.

> [!NOTE]
> Wisseling met voorbeeld wordt niet ondersteund in web-apps op Linux.

Wanneer u gebruikt de **wisselen met voorbeeld** optie (Zie [implementatiesites wisselen](#Swap)), App Service doet het volgende:

- Houdt de doelsleuf ongewijzigd zodat bestaande werkbelasting op die site (zoals productie) wordt niet negatief beïnvloed.
- Geldt de configuratie-elementen van de doelsleuf naar de bronsite, met inbegrip van de site-specifieke verbindingsreeksen en app-instellingen.
- De werkprocessen op de bronsite met behulp van deze elementen van de hiervoor genoemde configuratie opnieuw is opgestart.
- Wanneer u de wisseling voltooien: de bron van de pre-warmed-up sleuf is verplaatst naar de doelsleuf. De doelsleuf wordt verplaatst naar de bronsite in een handmatige wisseling.
- Wanneer u de swap annuleert: past u de configuratie-elementen van de bronsite naar de bronsite.

U kunt een voorbeeld precies hoe de app wordt uitgevoerd met de configuratie van de doelsleuf. Nadat de validatie is voltooid, kunt u de wisselruimte in een aparte stap voltooien. Deze stap is de toegevoegde voordeel dat de bron-sleuf al met de gewenste configuratie opgewarmd en clients niet ondervinden geen downtime.  

Voorbeelden voor de Azure PowerShell-cmdlets beschikbaar voor meerdere stap wisseling zijn opgenomen in de Azure PowerShell-cmdlets voor implementatie sleuven sectie.

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a>Automatisch wisselen configureren
Automatisch wisselen stroomlijnt DevOps-scenario's waar u uw app met nul koude start en uitvaltijd continu implementeren voor klanten einde van de app. Wanneer een implementatiesleuf is geconfigureerd voor automatisch wisselen naar de productie, elke keer dat u de update-code pushen naar die sleuf, wordt App Service automatisch de app wisselen naar de productie nadat deze al in de sleuf is opgewarmd.

> [!IMPORTANT]
> Wanneer u automatisch wisselen voor een site inschakelt, zorg ervoor dat de configuratie van de implementatiesite exact de configuratie die bestemd zijn voor de doel-sleuf (meestal de productiesite).
> 
> 

> [!NOTE]
> Automatisch wisselen wordt niet ondersteund in web-apps op Linux.

Automatisch wisselen configureren voor een sleuf is eenvoudig. Volg deze stappen:

1. In **Implementatiesites**, selecteert u een niet-productiesite en kiest u **toepassingsinstellingen** in die sleuf resource-blade.  
   
    ![][Autoswap1]
2. Selecteer **op** voor **automatisch wisselen**, selecteer de gewenste doel sleuf in **automatisch wisselen sleuf**, en klik op **opslaan** in de opdrachtbalk. Zorg ervoor dat de configuratie voor de sleuf exact de configuratie die bestemd zijn voor de doel-sleuf.
   
    De **meldingen** tabblad knippert een groene **geslaagd** nadat de bewerking voltooid is.
   
    ![][Autoswap2]
   
   > [!NOTE]
   > Als u wilt testen automatisch wisselen voor uw app, kunt u eerst een niet-productieve doel sleuf in selecteren **automatisch wisselen sleuf** om vertrouwd te raken met de functie.  
   > 
   > 
3. Een code te pushen naar de uitrolfase die worden uitgevoerd. Automatisch wisselen gebeurt er na een korte periode en de update wordt weergegeven op de doel-sleuf URL.

<a name="Rollback"></a>

## <a name="roll-back-a-production-app-after-swap"></a>Terugdraaien van een productie-app nadat wisselen
Als er fouten worden geïdentificeerd in het nadat een wisseling sleuf, terugdraaien de sleuven naar de staat van vóór wisselen door de dezelfde twee sleuven onmiddellijk wisselen.

<a name="Warm-up"></a>

## <a name="custom-warm-up-before-swap"></a>Aangepaste opgewarmd voordat wisselen
Sommige apps mogelijk aangepaste opgewarmd acties. De `applicationInitialization` configuratie-element in web.config-bestand kunt u opgeven van aangepaste initialisatie met acties worden uitgevoerd voordat een aanvraag wordt ontvangen. De wisselbewerking wacht tot deze aangepaste opgewarmd om te voltooien. Hier volgt een voorbeeld web.config fragment.

    <applicationInitialization>
        <add initializationPage="/" hostName="[app hostname]" />
        <add initializationPage="/Home/About" hostname="[app hostname]" />
    </applicationInitialization>

## <a name="monitor-swap-progress"></a>Voortgang van de wisseling bewaken

Soms duurt wisselen van de enige tijd hebt voltooid, zoals wanneer de app die zijn uitgewisseld een tijd lang opgewarmd heeft. U kunt meer informatie krijgen over swap-bewerkingen in de [activiteitenlogboek](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) in de [Azure-portal](https://portal.azure.com).

Selecteer in de app-pagina van de portal in de linkernavigatiebalk **activiteitenlogboek**.

Een wisselbewerking wordt weergegeven in de query logboek als `Slotsswap`. U kunt deze uitvouwen en selecteer een van de suboperations of fouten om de details te bekijken.

![Activiteitenlogboek voor sleuf wisselen](media/web-sites-staged-publishing/activity-log.png)

<a name="Delete"></a>

## <a name="delete-a-deployment-slot"></a>Een implementatiesleuf verwijderen
In de blade voor een implementatiesleuf de implementatiesleuf blade geopend, klikt u op **overzicht** (de standaardpagina), en klik op **verwijderen** in de opdrachtbalk.  

![Een Implementatiesleuf verwijderen][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="automate-with-azure-powershell"></a>Automatiseren met Azure PowerShell

Azure PowerShell is een module die cmdlets voor het beheren van Azure via Windows PowerShell, inclusief ondersteuning voor het beheren van implementatiesites in Azure App Service biedt.

* Zie voor informatie over het installeren en configureren van Azure PowerShell, en Azure PowerShell verifiëren met uw Azure-abonnement, [installeren en configureren van Microsoft Azure PowerShell](/powershell/azure/overview).  

- - -
### <a name="create-a-web-app"></a>Een webtoepassing maken
```PowerShell
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

- - -
### <a name="create-a-deployment-slot"></a>Een implementatiesleuf te maken
```PowerShell
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

- - -
### <a name="initiate-a-swap-with-preview-multi-phase-swap-and-apply-destination-slot-configuration-to-source-slot"></a>Start een wisseling met voorbeeld (meerdere fase swap) en doel-siteconfiguratie van toepassing op de bronsite
```PowerShell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a>Annuleren van een wisseling in behandeling (swap met revisie) en bron siteconfiguratie herstellen
```PowerShell
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

- - -
### <a name="swap-deployment-slots"></a>Wisselen implementatiesites
```PowerShell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

### <a name="monitor-swap-events-in-the-activity-log"></a>Monitor voor wisselen gebeurtenissen in het activiteitenlogboek
```PowerShell
Get-AzureRmLog -ResourceGroup [resource group name] -StartTime 2018-03-07 -Caller SlotSwapJobProcessor  
```

- - -
### <a name="delete-deployment-slot"></a>Implementatiesite verwijderen
```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

- - -
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="automate-with-azure-cli"></a>Automatiseren met Azure CLI

Voor [Azure CLI](https://github.com/Azure/azure-cli) -opdrachten voor implementatiesites, Zie [az webapp implementatiesleuf](/cli/azure/webapp/deployment/slot).

## <a name="next-steps"></a>Volgende stappen
[Azure App Service-Web App – blok webtoegang tot niet-productieve implementatiesites](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)  
[Inleiding tot op Linux-App Service](../app-service/containers/app-service-linux-intro.md)  
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

