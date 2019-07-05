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
ms.date: 06/18/2019
ms.author: cephalin
ms.openlocfilehash: fd488d475e24bc1aeebfa49b9d81b04ebae449ff
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445602"
---
# <a name="set-up-staging-environments-in-azure-app-service"></a>Faseringsomgevingen in Azure App Service instellen
<a name="Overview"></a>

Wanneer u uw web-app, web-app op Linux-, mobiele back-end- of API-app implementeert [Azure App Service](https://go.microsoft.com/fwlink/?LinkId=529714), kunt u een afzonderlijke implementatiesite in plaats van de standaard-productiesite wanneer u uitvoert de **Standard**, **Premium**, of **geïsoleerd** laag voor App Service-plan. Implementatiesleuven zijn live apps met hun eigen hostnamen. App-inhoud en configuratie-elementen kunnen worden gewisseld tussen twee implementatiesites, inclusief de productiesite. 

Implementeren van uw toepassing naar een niet-productiesite heeft de volgende voordelen:

* U kunt app-wijzigingen in een gefaseerde installatie implementatiesleuf valideren voordat deze wisselen met de productiesite.
* Eerst een app implementeren op een site en deze in productie komt ervoor zorgt dat alle exemplaren van de site zijn opgewarmd voordat ze in productie wordt komen. Dit veel minder uitvaltijd wanneer u uw app implementeert. Het omleiden van verkeer is naadloos en er zijn geen aanvragen worden verwijderd vanwege swap-bewerkingen. U kunt deze volledige werkstroom automatiseren door het configureren van [automatisch wisselen](#Auto-Swap) wanneer vooraf swap-validatie is niet nodig.
* De site met een eerder voorbereide app heeft nu de vorige productie-app na een wisselen. Als de wijzigingen in de productiesite gewisseld niet zoals verwacht, kunt u de dezelfde swap onmiddellijk als u uw 'laatst bekende goede site' uitvoeren terug.

Elke laag van App Service-plan ondersteunt een verschillend aantal implementatiesites. Er is geen extra kosten voor het gebruik van implementatiesites. Om erachter te komen het aantal sleuven van uw app-laag ondersteunt, Zie [limieten voor App Service](https://docs.microsoft.com/azure/azure-subscription-service-limits#app-service-limits). 

Schaal uw app naar een andere laag, zorg ervoor dat de doel-laag biedt ondersteuning voor het aantal sleuven uw app al gebruikt. Bijvoorbeeld, als uw app meer dan vijf sleuven heeft, u kunt geen de schaal omlaag naar de **Standard** tier, omdat de **Standard** laag ondersteunt maximaal vijf implementatiesites. 

<a name="Add"></a>

## <a name="add-a-slot"></a>Een sleuf toevoegen
De app moet worden uitgevoerd de **Standard**, **Premium**, of **geïsoleerd** laag zodat u meerdere implementatiesites inschakelen.

1. In de [Azure-portal](https://portal.azure.com/), opent u van uw app [resourcepagina](../azure-resource-manager/manage-resources-portal.md#manage-resources).

2. Selecteer in het linkerdeelvenster **implementatiesites** > **sleuf toevoegen**.
   
    ![Een nieuwe implementatiesite toevoegen](./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png)
   
   > [!NOTE]
   > Als de app nog niet in de **Standard**, **Premium**, of **geïsoleerd** laag, ontvangt u een bericht weergegeven dat aangeeft van de ondersteunde lagen voor het inschakelen van gefaseerd publiceren. Op dit moment hebt u de optie te selecteren **Upgrade** en Ga naar de **schaal** tab van uw app voordat u doorgaat.
   > 

3. In de **een sleuf toevoegen** in het dialoogvenster, Geef een naam op voor de site en selecteer of voor het klonen van een app-configuratie van een andere implementatiesleuf. Selecteer **toevoegen** om door te gaan.
   
    ![Configuratiebron](./media/web-sites-staged-publishing/ConfigurationSource1.png)
   
    U kunt een configuratie van een bestaande sleuf klonen. Instellingen die kunnen worden gekloond omvatten app-instellingen, verbindingsreeksen, taalversies van het framework, websockets, HTTP-versie en platform bitness.

4. Nadat de site wordt toegevoegd, selecteert u **sluiten** om het dialoogvenster te sluiten. De nieuwe site wordt nu weergegeven op de **implementatiesites** pagina. Standaard **verkeer %** is ingesteld op 0 voor de nieuwe site, met alle klantenverkeer doorgestuurd naar de productiesite.

5. Selecteer de nieuwe implementatiesite om dat de sleuf van resource-pagina te openen.
   
    ![Titel van de implementatie-sleuf](./media/web-sites-staged-publishing/StagingTitle.png)

    De staging-site is een beheerpagina net als elke andere App Service-app. U kunt van de site-configuratie wijzigen. De naam van de site wordt weergegeven boven aan de pagina om u te herinneren dat u de implementatiesleuf bekijkt.

6. Selecteer de URL van de app op de pagina van de resource van de site. De implementatiesite heeft een eigen hostnaam en is ook een live-app. Als u wilt beperken openbare toegang tot de implementatiesleuf, Zie [Azure App Service-IP-beperkingen](app-service-ip-restrictions.md).

De nieuwe implementatiesite bevat geen inhoud, zelfs als u de instellingen van een andere sleuf klonen. U kunt bijvoorbeeld [publiceren naar deze sleuf met Git](app-service-deploy-local-git.md). U kunt implementeren op de site van een andere opslagplaats vertakking of een andere opslagplaats. 

<a name="AboutConfiguration"></a>

## <a name="what-happens-during-a-swap"></a>Wat gebeurt er tijdens een wisselen

### <a name="swap-operation-steps"></a>Bewerkingsstappen wisselen

Wanneer u de twee sleuven (meestal van een staging-site naar de productiesite) wisselen, App Service de doet het volgende om ervoor te zorgen dat de doelsleuf niet downtime treedt:

1. De volgende instellingen van de doelsleuf (bijvoorbeeld de productiesite) van toepassing op alle exemplaren van de bron-site: 
    - [Site-specifieke](#which-settings-are-swapped) app-instellingen en verbindingsreeksen, indien van toepassing.
    - [Continue implementatie](deploy-continuous-deployment.md) -instellingen, als ingeschakeld.
    - [App Service-verificatie](overview-authentication-authorization.md) -instellingen, als ingeschakeld.
    
    Al deze gevallen-trigger voor alle exemplaren in de sleuf van de bron op te starten. Tijdens de [wisselen met preview](#Multi-Phase), dit markeert het einde van de eerste fase. De wisselbewerking wordt onderbroken en u kunt valideren dat de bron-sleuf correct met instellingen voor de doelsleuf werkt.

1. Wachten op elk exemplaar in de bron-sleuf om uit te voeren van het opnieuw opstarten. Als een willekeurig exemplaar niet wordt gestart, wordt de wisselbewerking opnieuw uit alle wijzigingen naar de bronsite ongedaan maken en stopt de bewerking.

1. Als [lokale cache](overview-local-cache.md) is ingeschakeld, de initialisatie van de lokale cache activeren door een HTTP-aanvraag naar de hoofdmap van de toepassing ('/ ') op elk exemplaar van de bron-site. Wacht totdat elke instantie een HTTP-antwoord geretourneerd. Initialisatie van de lokale cache weer opnieuw wordt opgestart, wordt op elk exemplaar.

1. Als [automatisch wisselen](#Auto-Swap) is ingeschakeld met [aangepaste warmen](#Warm-up), trigger [toepassing inleiding](https://docs.microsoft.com/iis/get-started/whats-new-in-iis-8/iis-80-application-initialization) door een HTTP-aanvraag naar de hoofdmap van de toepassing ('/ ') op elk exemplaar van de bron sleuf.

    Als `applicationInitialization` is niet opgegeven, een HTTP-aanvraag naar de hoofdmap van de bron-sleuf op elk exemplaar van de toepassing activeren. 
    
    Als een exemplaar van een HTTP-antwoord geretourneerd, heeft deze beschouwd als opgewarmd.

1. Als u alle exemplaren van de bron-sleuf zijn opgewarmd, wisselen de twee sleuven door over te schakelen de routeringsregels voor de twee sleuven. De doelsleuf (bijvoorbeeld de productiesite) heeft na deze stap wordt de app die eerder in de sleuf van de bron wordt opgewarmd.

1. Nu dat de bron-sleuf eerder het wisselen van pre-app in de doelsleuf heeft, moet u de dezelfde bewerking uitvoeren op alle instellingen toepassen en opnieuw starten van de exemplaren.

Op elk gewenst moment van de wisselbewerking opnieuw uit, wordt al het werk van het initialiseren van de verwisseld apps gebeurt op de bron-sleuf. De doelsleuf online blijft terwijl de bron-sleuf wordt voorbereid en opgewarmd, ongeacht waar de wisseling slaagt of mislukt. Het wisselen van een staging-site met de productiesite, zorg ervoor dat de productiesite altijd de doelsleuf. Op deze manier de wisselbewerking heeft geen invloed op uw productie-app.

### <a name="which-settings-are-swapped"></a>Welke instellingen worden gewisseld?
Wanneer u de configuratie van een andere implementatiesite kloont, wordt de configuratie van de gekloonde kan worden bewerkt. Bepaalde configuratie-elementen volgt u de inhoud via een wisselbestand (geen sleuf specifieke), terwijl andere configuratie-elementen in dezelfde sleuf na een (sleuf specifieke) voor wisselen blijven. De volgende lijsten ziet u de instellingen die wijzigen wanneer u sleuven.

**Instellingen die worden gewisseld**:

* Algemene instellingen, zoals framework-versie, 64-32-bits, websockets
* App-instellingen (kan worden geconfigureerd voor het gekozen voor een site)
* Verbindingsreeksen (kan worden geconfigureerd voor het gekozen voor een site)
* Handlertoewijzingen
* Instellingen voor controle en diagnose
* Openbare certificaten
* WebJobs-inhoud
* Hybride verbindingen *
* Integratie van virtuele netwerken *
* Service-eindpunten *
* Azure Content Delivery Network *

Functies die zijn gemarkeerd met een sterretje (*) zijn gepland om te worden aangebracht in de sleuf sticky. 

**Instellingen die niet worden gewisseld**:

* Publicatie-eindpunten
* Aangepaste domeinnamen
* Persoonlijke certificaten en SSL-bindingen
* Schaalinstellingen
* WebJobs-planners
* IP-beperkingen
* AlwaysOn
* Instellingen-protocol (HTTPS, TLS-versie, clientcertificaten)
* Instellingen voor diagnostische logboeken
* Cross-origin resource sharing (CORS)

<!-- VNET and hybrid connections not yet sticky to slot -->

Voor het configureren van een app-instelling of de verbindingsreeks aan een specifieke site (niet wisselen), gaat u naar de **configuratie** -pagina voor die site. Toevoegen of bewerken van een instelling en selecteer vervolgens **implementatie van site-instelling**. Als dit selectievakje vertelt App Service de instelling is niet swappable. 

![Site-instelling](./media/web-sites-staged-publishing/SlotSetting.png)

<a name="Swap"></a>

## <a name="swap-two-slots"></a>Twee sleuven 
U kunt implementatiesites wisselen op van uw app **implementatiesites** pagina en de **overzicht** pagina. Zie voor technische informatie over het wisselen van sleuf [wat er gebeurt tijdens het overplaatsen](#AboutConfiguration).

> [!IMPORTANT]
> Voordat u een app uit een implementatiesite naar productie wisselen, zorg ervoor dat productie de doelsleuf is en dat alle instellingen in de bron-sleuf precies zoals u wilt dat ze in productie zijn geconfigureerd.
> 
> 

Implementatiesites wisselen:

1. Ga naar uw app **implementatiesites** pagina en selecteer **wisselen**.
   
    ![Knop wisselen](./media/web-sites-staged-publishing/SwapButtonBar.png)

    De **wisselen** in het dialoogvenster ziet u instellingen in de geselecteerde bron- en doelsleuf die wordt gewijzigd.

2. Selecteer de gewenste **bron** en **doel** sleuven. Het doel is meestal de productiesite. Schakel ook de **wijzigingen in gegevensbron** en **doel wijzigingen** tabbladen en controleer of dat de wijzigingen in de configuratie worden verwacht. Wanneer u klaar bent, u kunt wisselen de sleuven onmiddellijk door te selecteren **wisselen**.

    ![Wisseling voltooien](./media/web-sites-staged-publishing/SwapImmediately.png)

    Als u wilt zien hoe uw doelsleuf wordt uitgevoerd met de nieuwe instellingen voordat de wisseling daadwerkelijk gebeurt, selecteer niet **wisselen**, maar de instructies in [wisselen met preview](#Multi-Phase).

3. Wanneer u klaar bent, sluit u het dialoogvenster selecteert **sluiten**.

Als u problemen hebt, raadpleegt u [oplossen swaps](#troubleshoot-swaps).

<a name="Multi-Phase"></a>

### <a name="swap-with-preview-multi-phase-swap"></a>Wisselen met Preview-versie (meerdere fase swap)

> [!NOTE]
> Wisselen met Preview-versie wordt niet ondersteund in web App on Linux.

Voordat u in productie als de doelsleuf wisselen, valideren dat de app wordt uitgevoerd met de verwisseld instellingen. De bron-sleuf is ook opgewarmd voordat het wisselen is voltooid, dat is het wenselijk zijn voor uw essentiële toepassingen.

Wanneer u een wisseling met preview uitvoeren, App Service voert dezelfde [wisselen](#AboutConfiguration) , maar na de eerste stap wordt onderbroken. U kunt vervolgens het resultaat van de staging-site controleren alvorens de wisseling te voltooien. 

Als u de wisseling annuleert, wordt App Service-configuratie-elementen naar de bronsite opnieuw toegepast.

Wisselen met Preview-versie:

1. Volg de stappen in [implementatiesites wisselen](#Swap) maar selecteer **wisselen met preview uitvoeren**.

    ![Wisselen met Preview-versie](./media/web-sites-staged-publishing/SwapWithPreview.png)

    In het dialoogvenster ziet u hoe de configuratie in de sleuf van de bron wordt gewijzigd in fase 1, en hoe de bron en doel-sleuf in fase 2 wijzigen.

2. Wanneer u klaar bent om de wisseling starten, selecteert u **wisseling starten**.

    Wanneer de fase 1 is voltooid, krijgt u een melding in het dialoogvenster. Voorbeeld van de wisseling in de bron-sleuf door te gaan naar `https://<app_name>-<source-slot-name>.azurewebsites.net`. 

3. Wanneer u klaar bent om de in behandeling wisseling voltooien, selecteert u **wisseling voltooien** in **Wisselactie** en selecteer **wisseling voltooien**.

    Als u wilt een wisseling in behandeling annuleren, selecteert u **wisseling annuleren** in plaats daarvan.

4. Wanneer u klaar bent, sluit u het dialoogvenster selecteert **sluiten**.

Als u problemen hebt, raadpleegt u [oplossen swaps](#troubleshoot-swaps).

Zie voor het automatiseren van een wisselbestand meerdere fasen, [automatiseren met PowerShell](#automate-with-powershell).

<a name="Rollback"></a>

## <a name="roll-back-a-swap"></a>Terugdraaien van een wisselbestand
Als er fouten in de doelsleuf (bijvoorbeeld de productiesite) na een wisselen van sleuf optreden, herstelt de sleuven vooraf wisselen door de dezelfde twee sleuven onmiddellijk wisselen.

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a>Automatisch wisselen configureren

> [!NOTE]
> Automatisch wisselen wordt niet ondersteund in web App on Linux.

Automatisch wisselen stroomlijnt Azure DevOps-scenario's waar u uw app continu met nul koude start en zonder downtime voor klanten van de app te implementeren. Wanneer automatisch wisselen is ingeschakeld in een sleuf in productie nemen, telkens wanneer u push de codewijzigingen naar dat de sleuf, App Service automatisch [Hiermee wisselt u de app in productie](#swap-operation-steps) nadat deze in de sleuf van de bron wordt opgewarmd.

   > [!NOTE]
   > Voordat u automatisch wisselen voor de productiesite configureert, moet u overwegen automatisch wisselen op een niet-productie doelsleuf testen.
   > 

Automatisch wisselen configureren:

1. Ga naar de resourcepagina van uw app. Selecteer **implementatiesites** >  *\<gewenste bronsite >*  > **configuratie**  >  **Algemene instellingen**.
   
2. Voor **automatisch wisselen ingeschakeld**, selecteer **op**. Selecteer vervolgens de gewenste doelsleuf voor **implementatie voor automatisch wisselen**, en selecteer **opslaan** op de opdrachtbalk. 
   
    ![Selecties voor het configureren van automatisch wisselen](./media/web-sites-staged-publishing/AutoSwap02.png)

3. Voer een code-push naar de bronsite. Automatisch wisselen gebeurt er na een korte periode en de update wordt weergegeven op de URL van de doelsleuf.

Als u problemen hebt, raadpleegt u [oplossen swaps](#troubleshoot-swaps).

<a name="Warm-up"></a>

## <a name="specify-custom-warm-up"></a>Aangepaste warmen opgeven
Wanneer u [automatisch wisselen](#Auto-Swap), sommige apps mogelijk aangepaste warmen acties voor het wisselbestand. De `applicationInitialization` configuratie-element in web.config kunt u aangepaste initialisatie acties opgeven. De [wisselen](#AboutConfiguration) wacht tot deze aangepaste warmen voltooid voordat u wisselen met de doelsleuf. Hier volgt een voorbeeld van web.config fragment.

    <system.webServer>
        <applicationInitialization>
            <add initializationPage="/" hostName="[app hostname]" />
            <add initializationPage="/Home/About" hostName="[app hostname]" />
        </applicationInitialization>
    </system.webServer>

Voor meer informatie over het aanpassen van de `applicationInitialization` -element, Zie [meest voorkomende sleuf wisselen fouten bij de implementatie en het oplossen van deze](https://ruslany.net/2017/11/most-common-deployment-slot-swap-failures-and-how-to-fix-them/).

U kunt ook het gedrag warmen met een of beide van de volgende [app-instellingen](configure-common.md):

- `WEBSITE_SWAP_WARMUP_PING_PATH`: Het pad naar het pingen om te oefenen van uw site. Deze app-instelling toevoegen door een aangepast pad dat begint met een slash als de waarde op te geven. Een voorbeeld is `/statuscheck`. De standaardwaarde is `/`. 
- `WEBSITE_SWAP_WARMUP_PING_STATUSES`: Geldige HTTP-responscodes voor de bewerking warmen. Deze app-instelling met een door komma's gescheiden lijst met HTTP-codes toevoegen. Een voorbeeld is `200,202` . Als de geretourneerde statuscode niet in de lijst, worden de bewerkingen opwarmtijd en wisselen gestopt. Standaard zijn alle responscodes geldig.

Als u problemen hebt, raadpleegt u [oplossen swaps](#troubleshoot-swaps).

## <a name="monitor-a-swap"></a>Een wisseling controleren

Als de [wisselen](#AboutConfiguration) lang duurt om te voltooien, krijgt u informatie over de wisselbewerking opnieuw uit in de [activiteitenlogboek](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

Selecteer op de resourcepagina van uw app in de portal, in het linkerdeelvenster **activiteitenlogboek**.

Een wisselbewerking wordt weergegeven in het logboekquery als `Swap Web App Slots`. U kunt deze uitvouwen en selecteer een van de suboperations of fouten om de details te bekijken.

## <a name="route-traffic"></a>Verkeer routeren

Standaard worden alle aanvragen van clients naar de productie-URL van de app (`http://<app_name>.azurewebsites.net`) worden gerouteerd naar de productiesite. U kunt een gedeelte van het verkeer routeren naar een andere sleuf. Deze functie is handig als u feedback van gebruikers voor een nieuwe update nodig, maar u kunt nu niet vrijgeven naar productie.

### <a name="route-production-traffic-automatically"></a>Productieverkeer automatisch doorsturen

Om productieverkeer te routeren automatisch:

1. Ga naar de resourcepagina van uw app en selecteer **implementatiesites**.

2. In de **verkeer %** kolom van de site die u wilt doorsturen naar, Geef een percentage (tussen 0 en 100) staat voor het bedrag van het totale verkeer dat u wilt routeren. Selecteer **Opslaan**.

    ![Instellen van een percentage verkeer](./media/web-sites-staged-publishing/RouteTraffic.png)

Nadat de instelling wordt opgeslagen, wordt het opgegeven percentage van clients willekeurig doorgestuurd naar de niet-productiesite. 

Nadat een client wordt automatisch doorgestuurd naar een specifieke site, de 'vastgemaakte' aan dat de sleuf voor de duur van die clientsessie. Op de browser van de client, ziet u welke site de sessie is vastgemaakt aan door te kijken de `x-ms-routing-name` cookie in uw HTTP-headers. Een aanvraag die wordt doorgestuurd naar de site "staging" heeft de cookie `x-ms-routing-name=staging`. Een aanvraag wordt gerouteerd naar de productiesite heeft de cookie `x-ms-routing-name=self`.

### <a name="route-production-traffic-manually"></a>Handmatig productieverkeer routeren

Naast de automatische verkeersroutering, kan de App Service aanvragen versturen naar een specifieke site. Dit is handig als u wilt dat uw gebruikers kunnen zich aanmelden of afmelden voor uw bèta-app. Om productieverkeer te routeren handmatig, gebruikt u de `x-ms-routing-name` queryparameter.

Als u wilt dat gebruikers afmelden voor uw app Bèta, kunt u bijvoorbeeld deze koppeling op uw webpagina plaatsen:

```HTML
<a href="<webappname>.azurewebsites.net/?x-ms-routing-name=self">Go back to production app</a>
```

De tekenreeks `x-ms-routing-name=self` Hiermee geeft u de productiesite. Nadat de browser van de client toegang heeft tot de koppeling, wordt die omgeleid naar de productiesite. Elke volgende aanvraag heeft de `x-ms-routing-name=self` cookie die kledingwinkelketen van de sessie naar de productiesite.

Zodat gebruikers kunnen meldt zich aan voor uw app Bèta, stelt u dezelfde queryparameter voor de op de naam van de niet-productiesite. Hier volgt een voorbeeld:

```
<webappname>.azurewebsites.net/?x-ms-routing-name=staging
```

Nieuwe sleuven krijgen standaard een regel voor het doorsturen van `0%`, zoals weergegeven in grijs. Als u expliciet deze waarde instelt op `0%` (weergegeven in zwarte tekst), uw gebruikers hebben toegang tot de staging-site handmatig met behulp van de `x-ms-routing-name` queryparameter. Maar deze wordt niet worden doorgestuurd naar de site automatisch omdat de routering percentage is ingesteld op 0. Dit is een geavanceerd scenario waar u "' de staging-site van het publiek verbergen kunt terwijl interne teams om wijzigingen op de site te testen.

<a name="Delete"></a>

## <a name="delete-a-slot"></a>Een site verwijderen

Ga naar de resourcepagina van uw app. Selecteer **implementatiesites** >  *\<sleuf verwijderen >*  > **overzicht**. Selecteer **verwijderen** op de opdrachtbalk.  

![Een implementatiesite verwijderen](./media/web-sites-staged-publishing/DeleteStagingSiteButton.png)

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="automate-with-powershell"></a>Automatiseren met PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure PowerShell is een module die cmdlets voor het beheren van Azure via Windows PowerShell, inclusief ondersteuning voor het beheren van implementatiesites gebruiken in Azure App Service biedt.

Zie voor informatie over het installeren en configureren van Azure PowerShell, en verificatie van Azure PowerShell met uw Azure-abonnement, [installeren en configureren van Microsoft Azure PowerShell](/powershell/azure/overview).  

---
### <a name="create-a-web-app"></a>Een webtoepassing maken
```powershell
New-AzWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

---
### <a name="create-a-slot"></a>Een sleuf maken
```powershell
New-AzWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

---
### <a name="initiate-a-swap-with-a-preview-multi-phase-swap-and-apply-destination-slot-configuration-to-the-source-slot"></a>Starten van een wisselen met een Preview-versie (meerdere fase swap) en doel-siteconfiguratie van toepassing op de bron-sleuf
```powershell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

---
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-the-source-slot-configuration"></a>Annuleren van een in behandeling (wisselen met controleren) voor wisselen en de configuratie van de implementatiesite bron herstellen
```powershell
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

---
### <a name="swap-deployment-slots"></a>Implementatiesites wisselen
```powershell
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

### <a name="monitor-swap-events-in-the-activity-log"></a>Swap-gebeurtenissen in het activiteitenlogboek controleren
```powershell
Get-AzLog -ResourceGroup [resource group name] -StartTime 2018-03-07 -Caller SlotSwapJobProcessor  
```

---
### <a name="delete-a-slot"></a>Een site verwijderen
```powershell
Remove-AzResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

---
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="automate-with-the-cli"></a>Automatiseren met de CLI

Voor [Azure CLI](https://github.com/Azure/azure-cli) opdrachten voor implementatiesites, Zie [az webapp deployment slot](/cli/azure/webapp/deployment/slot).

## <a name="troubleshoot-swaps"></a>Swaps oplossen

Als er een fout optreedt tijdens een [wisselen van sleuf](#AboutConfiguration), ze wordt vastgelegd *D:\home\LogFiles\eventlog.xml*. Dit wordt ook vastgelegd in het foutenlogboek van toepassingsspecifieke.

Hier volgen enkele algemene swap-fouten:

- Een HTTP-aanvraag naar de hoofdmap van de toepassing is een time-out opgetreden. De wisselbewerking wacht gedurende 90 seconden voor elke HTTP-aanvraag en nieuwe pogingen tot 5 keer. Als u alle nieuwe pogingen een time-out opgetreden, is de wisselbewerking gestopt.

- De initialisatie van de lokale cache kan mislukken als de app-inhoud groter is dan de lokale schijfquotum opgegeven voor de lokale cache. Zie voor meer informatie, [overzicht van lokale cache](overview-local-cache.md).

- Tijdens de [aangepaste warmen](#Warm-up), de HTTP-aanvragen worden gedaan intern (zonder tussenkomst van de externe URL). Ze kunnen mislukken met bepaalde herschrijvingsregels voor URL's in *Web.config*. Bijvoorbeeld regels voor het omleiden van domeinnamen of afdwingen van HTTPS kunnen te voorkomen dat warmen aanvragen van de app-code is bereikt. Wijzig uw herschrijvingsregels door toe te voegen van de volgende twee voorwaarden om dit probleem omzeilen:

    ```xml
    <conditions>
      <add input="{WARMUP_REQUEST}" pattern="1" negate="true" />
      <add input="{REMOTE_ADDR}" pattern="^100?\." negate="true" />
      ...
    </conditions>
    ```
- De herschrijvingsregels voor URL's zonder een aangepaste warmen kunnen nog steeds HTTP-aanvragen blokkeren. Om dit probleem omzeilen door uw herschrijvingsregels te wijzigen door de volgende voorwaarde toe te voegen:

    ```xml
    <conditions>
      <add input="{REMOTE_ADDR}" pattern="^100?\." negate="true" />
      ...
    </conditions>
    ```
- Sommige [IP-beperking regels](app-service-ip-restrictions.md) mogelijk te voorkomen dat het wisselen van HTTP-aanvragen verzendt naar uw app. IPv4-adresbereiken die beginnen met `10.` en `100.` intern zijn voor uw implementatie. U moet ze verbinding maken met uw app toestaan.

## <a name="next-steps"></a>Volgende stappen
[Toegang blokkeren tot niet-productie-sleuven](app-service-ip-restrictions.md)
