---
title: Faseringsomgevingen voor web-apps in Azure App Service instellen | Microsoft Docs
description: Leer hoe u gefaseerde publicatie gebruiken voor web-apps in Azure App Service.
services: app-service
documentationcenter: ''
author: cephalin
writer: cephalin
manager: jpconnoc
editor: mollybos
ms.assetid: e224fc4f-800d-469a-8d6a-72bcde612450
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2019
ms.author: cephalin
ms.openlocfilehash: 7c12b34f6d735579326d4ccdd95e7831fbb777d6
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56181419"
---
# <a name="set-up-staging-environments-in-azure-app-service"></a>Faseringsomgevingen in Azure App Service instellen
<a name="Overview"></a>

> [!NOTE]
> In deze gebruiksaanwijzing wordt beschreven hoe productiesites met behulp van een nieuwe pagina voor Preview-versie management beheren. Gebruikt voor de beheerpagina voor bestaande klanten kunnen echter ook doorgaan met de bestaande sleuf beheren pagina als voordat. 
>

Wanneer u uw web-app, web-app op Linux-, mobiele back-end- en API-app implementeert [App Service](https://go.microsoft.com/fwlink/?LinkId=529714), u kunt implementeren op een afzonderlijke implementatiesite in plaats van de standaard-productiesite bij uitvoering in de **Standard**, **Premium**, of **geïsoleerd** laag voor App Service-plan. Implementatiesleuven zijn daadwerkelijk live apps met hun eigen hostnamen. App-inhoud en configuratie-elementen kunnen worden gewisseld tussen twee implementatiesites, inclusief de productiesite. Implementeren van uw toepassing naar een niet-productiesite heeft de volgende voordelen:

* U kunt app-wijzigingen in een gefaseerde installatie implementatiesleuf valideren voordat deze wisselen met de productiesite.
* Eerst een app implementeren op een site en deze in productie komt ervoor zorgt dat alle exemplaren van de site zijn opgewarmd voordat ze in productie wordt komen. Dit veel minder uitvaltijd wanneer u uw app implementeert. Het omleiden van verkeer is naadloos en er zijn geen aanvragen worden verwijderd vanwege swap-bewerkingen. Deze volledige werkstroom kan worden geautomatiseerd door het configureren van [automatisch wisselen](#Auto-Swap) wanneer vooraf swap-validatie is niet nodig.
* De site met een eerder voorbereide app heeft nu de vorige productie-app na een wisselen. Als de wijzigingen in de productiesite gewisseld niet zoals verwacht, kunt u de dezelfde swap onmiddellijk als u uw 'laatst bekende goede site' uitvoeren terug.

Elke laag van App Service-plan ondersteunt een verschillend aantal implementatiesites. Om erachter te komen het aantal sleuven van uw app-laag ondersteunt, Zie [limieten van App Service](https://docs.microsoft.com/azure/azure-subscription-service-limits#app-service-limits). Als u wilt schalen van uw app naar een andere laag, moet de doel-laag ondersteuning voor het aantal sleuven die uw app al gebruikmaakt. Bijvoorbeeld, als uw app meer dan vijf sleuven heeft, u kunt geen de schaal omlaag naar **Standard** tier, omdat **Standard** laag biedt alleen ondersteuning voor vijf implementatiesites.

<a name="Add"></a>

## <a name="add-slot"></a>Sleuf toevoegen
De app moet worden uitgevoerd de **Standard**, **Premium**, of **geïsoleerd** laag zodat u meerdere implementatiesites inschakelen.

1. In de [Azure-portal](https://portal.azure.com/), opent u van uw app [resourcepagina](../azure-resource-manager/resource-group-portal.md#manage-resources).

2. Kies in het linkernavigatievenster de **implementatiesites (Preview)** optie en klik vervolgens op **sleuf toevoegen**.
   
    ![Een nieuwe implementatiesite toevoegen](./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png)
   
   > [!NOTE]
   > Als de app nog niet in de **Standard**, **Premium**, of **geïsoleerd** laag, ontvangt u een bericht weergegeven dat aangeeft van de ondersteunde lagen voor het inschakelen van gefaseerd publiceren. Op dit moment hebt u de optie te selecteren **Upgrade** en navigeer naar de **schaal** tab van uw app voordat u doorgaat.
   > 

3. In de **een sleuf toevoegen** dialoogvenster, Geef een naam op voor de site en selecteer of u wilt klonen appconfiguratie vanaf een andere bestaande implementatiesite. Klik op **toevoegen** om door te gaan.
   
    ![Configuratiebron](./media/web-sites-staged-publishing/ConfigurationSource1.png)
   
    U kunt de configuratie van een bestaande sleuf klonen. Instellingen die kunnen worden gekloond omvatten app-instellingen, verbindingsreeksen, taalversies van het framework, websockets, HTTP-versie en platform bitness.

4. Nadat de site wordt toegevoegd, klikt u op **sluiten** om het dialoogvenster te sluiten. De nieuwe site wordt nu weergegeven in de **implementatiesites (Preview)** pagina. Standaard de **verkeer %** is ingesteld op 0 voor de nieuwe site, met alle klantenverkeer doorgestuurd naar de productiesite.

5. Klik op de nieuwe implementatiesite om dat de sleuf van resource-pagina te openen.
   
    ![Titel van de implementatie-sleuf](./media/web-sites-staged-publishing/StagingTitle.png)

    De staging-site is een beheerpagina net als elke andere App Service-app. U kunt van de site-configuratie wijzigen. De naam van de site wordt weergegeven boven aan de pagina om u te herinneren dat u de implementatiesleuf bekijkt.

6. Klik op de URL van de app in de resourcepagina van de site. De implementatiesleuf heeft een eigen hostnaam en is ook een live-app. Als u wilt beperken openbare toegang tot de implementatiesleuf, Zie [IP-beperkingen voor Azure App Service](app-service-ip-restrictions.md).

De nieuwe implementatiesite bevat geen inhoud, zelfs als u de instellingen van een andere sleuf klonen. U kunt bijvoorbeeld [publiceren naar deze sleuf met git](app-service-deploy-local-git.md). U kunt implementeren op de site van een andere opslagplaats vertakking of een andere opslagplaats. 

<a name="AboutConfiguration"></a>

## <a name="which-settings-are-swapped"></a>Welke instellingen worden gewisseld?
Wanneer u de configuratie van een andere implementatiesite kloont, wordt de configuratie van de gekloonde kan worden bewerkt. Bepaalde configuratie-elementen volgen bovendien de inhoud via een wisselbestand (kan niet in sleuf worden specifieke) terwijl andere configuratie-elementen in dezelfde sleuf na een (sleuf specifieke) voor wisselen blijven. De volgende lijsten ziet u de instellingen die wijzigen wanneer u sleuven.

**Instellingen die worden gewisseld**:

* Algemene instellingen -, framework-versie, 64-32-bits, zoals Web sockets
* App-instellingen (kan worden geconfigureerd voor het gekozen voor een site)
* Verbindingsreeksen (kan worden geconfigureerd voor het gekozen voor een site)
* Handlertoewijzingen
* Instellingen voor controle en diagnose
* Openbare certificaten
* WebJobs-inhoud
* Hybride verbindingen

**Instellingen die niet worden gewisseld**:

* Publicatie-eindpunten
* Aangepaste domeinnamen
* Persoonlijke certificaten en SSL-bindingen
* Schaalinstellingen
* WebJobs-planners

<!-- VNET, IP restrictions, CORS, hybrid connections? -->

Voor het configureren van een app-instelling of de verbindingsreeks aan een specifieke site (niet wisselen), gaat u naar de **toepassingsinstellingen** voor dat de sleuf pagina en selecteer vervolgens de **site-instelling** vak voor de configuratie-elementen die behouden in de sleuf blijven moeten. Een configuratie-element markeren als sleuf specifieke vertelt App Service is niet swappable.

![Instelling voor site](./media/web-sites-staged-publishing/SlotSetting.png)

<a name="Swap"></a>

## <a name="swap-two-slots"></a>Twee sleuven 
U kunt implementatiesites wisselen van uw App **implementatiesites (Preview)** pagina. 

U kunt ook sleuven wissen uit de **overzicht** en **implementatiesites** pagina's, maar momenteel kunt u de oude ervaring. Deze handleiding laat zien hoe u de nieuwe gebruikersinterface in de **implementatiesites (Preview)** pagina.

> [!IMPORTANT]
> Voordat u een app uit een implementatiesite naar productie wisselen, zorg ervoor dat alle instellingen zijn geconfigureerd, precies zoals u wilt dat dit in de doel-wisselen.
> 
> 

Als u wilt implementatiesites wisselen, de volgende stappen uit:

1. Navigeer naar uw app **implementatiesites (Preview)** pagina en klik op **wisselen**.
   
    ![Knop wisselen](./media/web-sites-staged-publishing/SwapButtonBar.png)

    De **wisselen** dialoogvenster bevat de instellingen van de geselecteerde bron- en doelsleuf die wordt gewijzigd.

2. Selecteer de gewenste **bron** en **doel** sleuven. Het doel is meestal de productiesite. Ook het geval is, klikt u op de **wijzigingen in gegevensbron** en **doel wijzigingen** tabbladen en controleer of dat de wijzigingen in de configuratie worden verwacht. Wanneer u klaar bent, u kunt wisselen de sleuven onmiddellijk door te klikken op **wisselen**.

    ![Wisseling voltooien](./media/web-sites-staged-publishing/SwapImmediately.png)

    Als u wilt zien hoe uw doelsleuf wordt uitgevoerd met de nieuwe instellingen voordat de wisseling daadwerkelijk gebeurt, klikt u op **wisselen**, maar de instructies in [wisselen met preview](#Multi-Phase).

3. Wanneer u klaar bent, sluit u het dialoogvenster door te klikken op **sluiten**.

<a name="Multi-Phase"></a>

### <a name="swap-with-preview-multi-phase-swap"></a>Wisselen met Preview-versie (meerdere fase swap)

> [!NOTE]
> Wisselen met Preview-versie wordt niet ondersteund in web App on Linux.

Voordat het wisselen in productie als de doelsleuf, controleert u de app wordt uitgevoerd met de verwisseld instellingen voordat de wisseling gebeurt. De bron-sleuf is ook opgewarmd voordat het wisselen is voltooid, die ook wenselijk voor bedrijfskritische toepassingen.

Wanneer u een wisseling met preview uitvoeren, doet de App Service het volgende bij het starten van de wisseling:

- De doelsleuf dus geen invloed op de bestaande werkbelasting op dat de sleuf (zoals een productieomgeving) ongewijzigd blijft.
- Geldt de configuratie-elementen van de doelsleuf voor de bron-sleuf, met inbegrip van de site-specifieke verbindingsreeksen en app-instellingen.
- Start opnieuw op de werkprocessen op de bron-site met behulp van deze configuratie-elementen. U kunt bladeren van de bron-sleuf en de App uitvoeren met de configuratiewijzigingen.

Als u de wisseling in een aparte stap hebt voltooid, wordt de sleuf warm gelopen bron in de doelsleuf en de doelsleuf van App Service verplaatst in de sleuf van de bron. Als u de wisseling annuleren, opnieuw App Service de configuratie-elementen van de bron-sleuf naar de bronsite.

Volg deze stappen om te wisselen met Preview-versie.

1. Volg de stappen in [implementatiesites wisselen](#Swap) maar selecteer **wisselen met preview uitvoeren**.

    ![Wisseling met voorbeeld](./media/web-sites-staged-publishing/SwapWithPreview.png)

    Het dialoogvenster ziet u hoe de configuratie in de sleuf van de bron wordt gewijzigd in fase 1, en hoe de bron en doel-sleuf in fase 2 wijzigen.

2. Wanneer deze gereed is voor de wisseling starten, klikt u op **wisseling starten**.

    Wanneer de fase 1 is voltooid, krijgt u een melding in het dialoogvenster. Een voorbeeld van de wisseling in de bron-sleuf door te navigeren naar `https://<app_name>-<source-slot-name>.azurewebsites.net`. 

3. Wanneer u klaar bent om uit te voeren van de wisseling in behandeling, selecteer **wisseling voltooien** in **Wisselactie** en klikt u op **wisseling voltooien**.

    Als u wilt een wisseling in behandeling annuleren, selecteert u **wisseling annuleren** in plaats daarvan en klik op **wisseling annuleren**.

4. Wanneer u klaar bent, sluit u het dialoogvenster door te klikken op **sluiten**.

Zie voor het automatiseren van een wisselbestand meerdere fasen, automatiseren met PowerShell.

<a name="Rollback"></a>

## <a name="roll-back-swap"></a>Wisselen terugdraaien
Als er fouten in de doelsleuf (bijvoorbeeld de productiesite) na een wisselen van sleuf optreden, herstelt de sleuven vooraf wisselen door de dezelfde twee sleuven onmiddellijk wisselen.

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a>Automatisch wisselen configureren

> [!NOTE]
> Automatisch wisselen wordt niet ondersteund in web App on Linux.

Automatisch wisselen stroomlijnt DevOps-scenario's waar u voor het implementeren van uw app continu met nul koude start en uitvaltijd voor eindgebruikers van de app. Wanneer een sleuf autoswaps in productie nemen, telkens wanneer u uw code pushen wordt gewijzigd naar die site, App Service automatisch Hiermee wisselt u de app in productie nadat deze in de sleuf van de bron wordt opgewarmd.

   > [!NOTE]
   > Voordat u voor de productiesite configureert automatisch wisselen, kunt u testen automatisch wisselen op een niet-productie doelsleuf eerst.
   > 

Als u wilt configureren voor automatisch wisselen, de volgende stappen uit:

1. Navigeer naar de resourcepagina van uw app. Selecteer **implementatiesites (Preview)** > *\<gewenste bronsite >* > **toepassingsinstellingen**.
   
2. In **automatisch wisselen**, selecteer **op**, selecteer vervolgens de gewenste doelsleuf in **sleuf voor automatisch wisselen**, en klikt u op **opslaan** in de opdrachtbalk. 
   
    ![](./media/web-sites-staged-publishing/AutoSwap02.png)

3. Voer een code-push naar de bronsite. Automatisch wisselen gebeurt er na een korte periode en de update wordt weergegeven op de URL van de doelsleuf.

<a name="Warm-up"></a>

## <a name="custom-warm-up"></a>Aangepaste warmen
Bij het gebruik van [automatisch wisselen](#Auto-Swap), sommige apps kunnen aangepaste warmen acties voor de wisseling vereisen. De `applicationInitialization` configuratie-element in web.config kunt u opgeven de initialisatie van de aangepaste acties worden uitgevoerd. De wisselbewerking moet wachten voor deze aangepaste warmen voltooid voordat het wisselen met de doelsleuf. Hier volgt een voorbeeld van web.config fragment.

    <system.webServer>
        <applicationInitialization>
            <add initializationPage="/" hostName="[app hostname]" />
            <add initializationPage="/Home/About" hostName="[app hostname]" />
        </applicationInitialization>
    </system.webServer>

U kunt ook het gedrag warmen met een of meer van de volgende [app-instellingen](https://github.com/MicrosoftDocs/azure-docs-pr/pull/web-sites-configure.md):

- `WEBSITE_SWAP_WARMUP_PING_PATH`: Het pad naar het pingen voor opwarming uw site. Deze app-instelling toevoegen door een aangepast pad dat begint met een slash als de waarde op te geven. Bijvoorbeeld `/statuscheck`. De standaardwaarde is `/`. 
- `WEBSITE_SWAP_WARMUP_PING_STATUSES`: Geldige HTTP-responscodes voor de bewerking warmen. Deze app-instelling met een door komma's gescheiden lijst met HTTP-codes toevoegen. Bijvoorbeeld: `200,202` . Als de geretourneerde statuscode zich niet in de lijst, worden de bewerkingen opwarmtijd en wisselen gestopt. Standaard zijn alle responscodes geldig.

## <a name="monitor-swap"></a>Monitor voor wisselen

Als de wisselbewerking lang duurt om te voltooien, krijgt u informatie over de wisselbewerking opnieuw uit in de [activiteitenlogboek](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

Selecteer in de resourcepagina van uw app in de portal, in de navigatiebalk links **activiteitenlogboek**.

Een wisselbewerking wordt weergegeven in het logboekquery als `Swap Web App Slots`. U kunt deze uitvouwen en selecteer een van de suboperations of fouten om de details te bekijken.

## <a name="route-traffic"></a>Verkeer routeren

Standaard worden alle aanvragen van clients naar de productie-URL van de app (`http://<app_name>.azurewebsites.net`) worden gerouteerd naar de productiesite. U kunt een gedeelte van het verkeer routeren naar een andere sleuf. Deze functie is handig als u feedback van gebruikers voor een nieuwe update nodig, maar u kunt nu niet vrijgeven naar productie.

### <a name="route-production-traffic-automatically"></a>Productieverkeer automatisch doorsturen

Om productieverkeer te routeren automatisch, de volgende stappen uit:

1. Navigeer naar de resourcepagina van uw app en selecteer **implementatiesites (Preview)**.

2. In de **verkeer %** kolom van de site die u wilt doorsturen naar, Geef een percentage (tussen 0 en 100) staat voor het bedrag van het totale verkeer dat u wilt routeren. Klik op **Opslaan**.

    ![](./media/web-sites-staged-publishing/RouteTraffic.png)

Wanneer de instelling is opgeslagen, wordt het opgegeven percentage van clients willekeurig doorgestuurd naar de niet-productiesite. 

Zodra een client wordt automatisch doorgestuurd naar een specifieke site, de 'vastgemaakte' aan dat de sleuf voor de duur van die clientsessie. Op de browser van de client, ziet u welke site de sessie is vastgemaakt aan door te kijken de `x-ms-routing-name` cookie in uw HTTP-headers. Een aanvraag die wordt doorgestuurd naar de site "staging" heeft de cookie `x-ms-routing-name=staging`. Een aanvraag wordt gerouteerd naar de productiesite heeft de cookie `x-ms-routing-name=self`.

### <a name="route-production-traffic-manually"></a>Handmatig productieverkeer routeren

Naast de automatische verkeersroutering, kan de App Service aanvragen versturen naar een specifieke site. Dit is handig als u wilt dat uw gebruikers kunnen opt-in-of opt-out van uw bèta-app. Om productieverkeer te routeren handmatig, gebruikt u de `x-ms-routing-name` queryparameter.

Zodat gebruikers afmelden voor uw bèta-app kunt u bijvoorbeeld deze koppeling in de web-pagina plaatsen:

```HTML
<a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>
```

De tekenreeks `x-ms-routing-name=self` Hiermee geeft u de productiesite. Zodra de browser van de client toegang heeft tot de koppeling, niet alleen wordt deze doorgestuurd naar de productiesite, maar elke volgende aanvraag heeft de `x-ms-routing-name=self` cookie die kledingwinkelketen van de sessie naar de productiesite.

Als gebruikers wilt laten deelnemen aan uw app Bèta, stelt u dezelfde queryparameter voor de op de naam van de niet-productie-sleuf, bijvoorbeeld:

```
<webappname>.azurewebsites.net/?x-ms-routing-name=staging
```

<a name="Delete"></a>

## <a name="delete-slot"></a>Sleuf verwijderen

Navigeer naar de resourcepagina van uw app. Selecteer **implementatiesites (Preview)** > *\<sleuf verwijderen >* > **overzicht**. Klik op **verwijderen** in de opdrachtbalk.  

![Een Implementatiesite verwijderen](./media/web-sites-staged-publishing/DeleteStagingSiteButton.png)

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="automate-with-powershell"></a>Automatiseren met PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure PowerShell is een module die cmdlets voor het beheren van Azure via Windows PowerShell, inclusief ondersteuning voor het beheren van implementatiesites gebruiken in Azure App Service biedt.

Zie voor informatie over het installeren en configureren van Azure PowerShell, en verificatie van Azure PowerShell met uw Azure-abonnement, [installeren en configureren van Microsoft Azure PowerShell](/powershell/azure/overview).  

- - -
### <a name="create-web-app"></a>Een web-app maken
```PowerShell
New-AzWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

- - -
### <a name="create-slot"></a>Sleuf maken
```PowerShell
New-AzWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

- - -
### <a name="initiate-swap-with-preview-multi-phase-swap-and-apply-destination-slot-configuration-to-source-slot"></a>Wisselen met Preview-versie (meerdere fase swap) starten en doel-siteconfiguratie van toepassing op de bronsite
```PowerShell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="cancel-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a>Annuleren in behandeling (wisselen met controleren) voor wisselen en bron siteconfiguratie herstellen
```PowerShell
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

- - -
### <a name="swap-deployment-slots"></a>Implementatiesites wisselen
```PowerShell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

### <a name="monitor-swap-events-in-the-activity-log"></a>Swap-gebeurtenissen controleren in de activiteits-logboek
```PowerShell
Get-AzLog -ResourceGroup [resource group name] -StartTime 2018-03-07 -Caller SlotSwapJobProcessor  
```

- - -
### <a name="delete-slot"></a>Sleuf verwijderen
```powershell
Remove-AzResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

- - -
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="automate-with-cli"></a>Automatiseren met CLI

Voor [Azure CLI](https://github.com/Azure/azure-cli) opdrachten voor implementatiesites, Zie [az webapp deployment slot](/cli/azure/webapp/deployment/slot).

## <a name="next-steps"></a>Volgende stappen
[Toegang blokkeren tot niet-productie-sleuven](app-service-ip-restrictions.md)
