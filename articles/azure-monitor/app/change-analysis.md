---
title: Azure Monitor-toepassing wijzigen analysis - detecteren wijzigingen die mogelijk van invloed op live site problemen/storingen met Azure Monitor-toepassing wijzigen analyse | Microsoft Docs
description: Toepassing live siteproblemen oplossen in Azure App Services met Azure Monitor wijzigen toepassingsanalyse
services: application-insights
author: cawams
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: cawa
ms.openlocfilehash: 5bd3816e65398283de85b4551a137b3f97db4cc7
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66226275"
---
# <a name="application-change-analysis-public-preview"></a>Analyse van de wijziging toepassing (openbare preview)

Wanneer er een probleem live site/uitval optreedt, is het essentieel dat u snel bepalen van de hoofdoorzaak te achterhalen. Standard bewakingsoplossingen kunt u snel vaststellen dat er een probleem is, en vaak ook welk onderdeel is mislukt. Maar dit wordt niet altijd leiden tot een onmiddellijke uitleg waarom de fout zich voordoet. Uw site gewerkt vijf minuten geleden, nu deze is verbroken. Wat er in de afgelopen vijf minuten gewijzigd? Dit is de vraag die de nieuwe functie van Azure Monitor-analyse voor wijziging van toepassing is ontworpen om te beantwoorden. Met het bouwen van op de kracht van de [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) wijziging toepassingsanalyse biedt inzicht in uw Azure-toepassingswijzigingen aan observability verhogen en verlagen MTTR (Mean Time-herstel).

> [!IMPORTANT]
> Analyse van Azure Monitor toepassing wijzigen is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="overview-of-change-analysis-service"></a>Overzicht van de wijziging Analysis-service
De wijziging analysis-service detecteert de verschillende soorten wijzigingen uit infrastructuur laag helemaal tot aan de implementatie van toepassing. Het is een abonnement op Azure-resourceprovider waarmee gezocht naar wijzigingen van resources in het abonnement en gegevens voor verschillende diagnostische hulpprogramma's om te begrijpen welke wijzigingen gebruikers er de oorzaak van problemen.

Het volgende diagram illustreert de architectuur van wijziging analysis-service: ![Architectuurdiagram voor hoe wijziging analysis-service verkrijgt wijzigen van gegevens en gegevens beschikbaar stellen voor clienthulpprogramma 's](./media/change-analysis/overview.png)

Het hulpprogramma is momenteel geïntegreerd in de App-Services-web-app vaststellen en oplossen van problemen ervaring. Zie *wijziging Analysis service voor Web-App van App Services* sectie over het inschakelen en weergeven van wijzigingen in een web-app.

### <a name="azure-resource-manager-deployment-changes"></a>Wijzigingen van Azure Resource Manager-implementatie
Gebruik te maken van [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) de wijziging analyseprogramma biedt een historisch overzicht van hoe de Azure-resources die uw toepassing wordt gehost na verloop van tijd zijn veranderd. Bijvoorbeeld, als een web-app een tag toegevoegd heeft, de wijziging wordt doorgevoerd in het hulpmiddel wijzigen.
Deze informatie is altijd beschikbaar zolang de `Microsoft.ChangeAnalysis` resourceprovider is toegevoegd aan Azure-abonnement.

### <a name="web-application-deployment-and-configuration-changes"></a>Web-implementatie en configuratie wijzigingen in de toepassing
Implementatie en configuratie van de status van een toepassing elke 4 uur vastgelegd wijziging analyseprogramma voor het berekenen van de verschillen en presenteren wat is gewijzigd. Voorbeelden van dergelijke wijzigingen zijn variabele wijzigingen in de omgeving voor de toepassing, wijzigingen in IP-configuratie-regel, wijzigingen van de beheerde Service-identiteit, wijzigingen in de SSL, enzovoort.
In tegenstelling tot Resource Manager-wijzigingen, dit type gegevens over wijziging mogelijk niet beschikbaar onmiddellijk in het hulpprogramma. Als u wilt weergeven van de meest recente wijzigingen, gebruikt u de knop 'Scan verandert nu' in het hulpprogramma.

![Schermafbeelding van de scan voor wijzigingen nu knop in vaststellen en oplossen van problemen hulpprogramma met de analysis-integratie wijzigen voor app service web-app](./media/change-analysis/scan-changes.png)

### <a name="dependency-changes"></a>Wijzigingen van de afhankelijkheid
Afhankelijkheden resource kan ook worden de oorzaak van problemen. Bijvoorbeeld, als een web-app naar een Redis-cache aanroepen, kan de prestaties van de web-apps kan worden beïnvloed door Redis-cache SKU. Wijzig analysis service toont ook de afhankelijkheden informatie over wijzigingen in alle onderdelen van een app die kan zijn veroorzaakt problemen identificeren door te kijken naar de DNS-record van de web-app.


## <a name="change-analysis-service-for-app-services-web-app"></a>Analyseservice voor App Services-Web-App wijzigen

Analyse van Azure Monitor toepassing wijzigen is momenteel gratis ingebouwd in de self-service **vaststellen en oplossen van problemen met** ondervindt, die kunnen worden benaderd vanaf de **overzicht** sectie van uw Azure App Service toepassing:

![Schermopname van Azure App Service-Overzicht pagina met rode vakken om de knop Overzicht en vaststellen en oplossen van problemen met knop](./media/change-analysis/change-analysis.png)

### <a name="enable-change-analysis-in-diagnose-and-solve-problems-tool"></a>Inschakelen van de analyse van de wijziging in vaststellen en oplossen van problemen met hulpprogramma

1. Selecteer **beschikbaarheid en prestaties**

    ![Schermafbeelding van de beschikbaarheid en prestaties opties voor probleemoplossing](./media/change-analysis/availability-and-performance.png)

2. Klik op de **toepassing vastloopt** tegel.

   ![Schermafbeelding met de toepassing vastloopt tegel](./media/change-analysis/application-crashes-tile.png)

3. Om in te schakelen **wijziging Analysis** Selecteer **nu inschakelen**.

   ![Schermafbeelding van de beschikbaarheid en prestaties opties voor probleemoplossing](./media/change-analysis/application-crashes.png)

4. Om te profiteren van de volledige analysis functionaliteit set wijzigen **analyse wijzigen**, **scannen op wijzigingen in de code**, en **altijd op** naar **op** en selecteer **opslaan**.

    ![Schermafbeelding van de wijziging analysis-gebruikersinterface van Azure App Service inschakelen](./media/change-analysis/change-analysis-on.png)

    Als **wijziging Analysis** is ingeschakeld, kunt u zich voor het detecteren van niveau wijzigingen van resources. Als **scannen op wijzigingen in de code** is ingeschakeld, u ook implementatiebestanden te zien en wijzigingen in de configuratie van site. Inschakelen van **altijd op** optimaliseert de prestaties scannen wijzigen, maar mogelijk extra kosten vanuit het oogpunt van de facturering.

5.  Als alles is ingeschakeld, selecteren **vaststellen en oplossen van problemen met** > **beschikbaarheid en prestaties** > **toepassing vastloopt** kunt u toegang tot de ervaring van de analyse wijzigen. De grafiek wordt het type van de wijzigingen die hebben plaatsgevonden na verloop van tijd, samen met details over deze wijzigingen worden samengevat:

     ![Schermafbeelding van wijzigen diff weergeven](./media/change-analysis/change-view.png)


### <a name="enable-change-analysis-service-at-scale"></a>Wijziging Analysis-service op schaal inschakelen
Als u veel web-apps in uw abonnement hebt, worden de service per niveau van de web-app inschakelen inefficiënt. Hier volgen enkele andere onboarding-instructies.

#### <a name="registering-change-analysis-resource-provider-for-your-subscription"></a>De resourceprovider Analysis wijzigen voor uw abonnement registreren

1. Wijziging Analysis preview functievlag registreren

    Aangezien deze functie in preview is, moet u eerst registreren functievlag voor deze zichtbaar zijn voor uw abonnement.
    - Open [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/).

    ![Schermopname van Azure Cloud Shell wijzigen](./media/change-analysis/cloud-shell.png)

    - Wijzig het type shell in PowerShell:

    ![Schermopname van Azure Cloud Shell wijzigen](./media/change-analysis/choose-powershell.png)

    - Voer de volgende PowerShell-opdracht uit:

    ``` PowerShell

    Set-AzContext -Subscription <your_subscription_id> #set script execution context to the subscription you are trying to onboard
    Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.ChangeAnalysis" -ListAvailable #Check for feature flag availability
    Register-AzureRmProviderFeature -FeatureName PreviewAccess -ProviderNamespace Microsoft.ChangeAnalysis #Register feature flag

    ```

2. Wijziging Analysis-Resourceprovider registreren voor het abonnement

    - Navigeer naar de abonnementen, selecteer het abonnement dat u vrijgeven de service wijzigen wilt en klik vervolgens op van resourceproviders:

        ![Schermafbeelding voor het registreren van wijziging Analysis RP van blade abonnementen](./media/change-analysis/register-rp.png)

    - Selecteer *Microsoft.ChangeAnalysis* en klikt u op *registreren* boven aan de pagina.

    - Zodra de Resource Provider is geïmplementeerd, volg de instructies uit *kan niet worden opgehaald van analysegegevens wijzigen* hieronder verborgen tag instellen voor de web-app voor implementatie inschakelen het niveau van detectie te wijzigen op de web-app.

3. U kunt ook kunt u naar stap 2 hierboven, ook registreren voor de Resourceprovider via PowerShell-script:

    ```PowerShell
    Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState #Check if RP is ready for registration

    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis" #Register the Change Analysis RP
    ```

4. Als u wilt een verborgen-tag is ingesteld op een web-app met behulp van PowerShell, voer de volgende opdracht:

    ```powershell
    $webapp=Get-AzWebApp -Name <name_of_your_webapp>
    $tags = $webapp.Tags
    $tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
    Set-AzResource -ResourceId <your_webapp_resourceid> -Tag $tag
    ```

> [!NOTE]
> Zodra de verborgen tag wordt toegevoegd, moet u mogelijk nog steeds in eerste instantie wachten tot 4 uur als u eerst om wijzigingen te bekijken. Dit komt door de freqeuncy 4 uur dat de wijziging analysis-service gebruikt voor het scannen van uw web-app tijdens de invloed op de prestaties van de scan te beperken.

## <a name="next-steps"></a>Volgende stappen

- Verbetering van de bewaking van Azure App Services [door de Application Insights-functies ingeschakeld](azure-web-apps.md) van Azure Monitor.
- Vergroot u uw kennis van de [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) waarmee power Azure Monitor-toepassing wijzigen analyse.
