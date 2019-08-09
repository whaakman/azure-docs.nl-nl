---
title: Toepassings wijzigings analyse in Azure Monitor gebruiken om problemen met web-apps op te sporen | Microsoft Docs
description: Toepassings wijzigingen in Azure Monitor gebruiken om problemen met toepassingen op Live sites in Azure App Service op te lossen.
services: application-insights
author: cawams
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: cawa
ms.openlocfilehash: a08fc7d7822b4aeddafb588fdb73e86559ce2b12
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2019
ms.locfileid: "68849170"
---
# <a name="use-application-change-analysis-preview-in-azure-monitor"></a>Toepassings wijzigings analyse (preview) gebruiken in Azure Monitor

Wanneer er een probleem is met de live site of de storing optreedt, is het snel bepalen van de hoofd oorzaak van cruciaal belang. Met oplossingen voor standaard bewaking wordt u mogelijk op een probleem gewaarschuwd. Ze kunnen zelfs aangeven welk onderdeel mislukt. Deze waarschuwing legt echter niet altijd onmiddellijk de oorzaak van de fout uit. U weet dat uw site vijf minuten geleden heeft gewerkt en nu is verbroken. Wat is er in de afgelopen vijf minuten gewijzigd? Dit is de vraag of de analyse van toepassings wijzigingen is ontworpen om in Azure Monitor te beantwoorden.

Wijzigingen die zijn gebaseerd op de kracht van de [Azure-resource grafiek](https://docs.microsoft.com/azure/governance/resource-graph/overview), bieden inzicht in de veranderingen in de Azure-toepassing om de waarneembaarheid te verg Roten en MTTR te verminderen (gemiddelde tijd om te herstellen).

> [!IMPORTANT]
> De analyse van wijzigingen is momenteel beschikbaar als preview-versie. Deze preview-versie is beschikbaar zonder een service overeenkomst. Deze versie wordt niet aanbevolen voor productie werkbelastingen. Sommige functies worden mogelijk niet ondersteund of hebben mogelijk beperkte mogelijkheden. Zie voor meer informatie [aanvullende gebruiks voorwaarden voor Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)-previews.

## <a name="overview"></a>Overzicht

Met wijzigings analyse worden verschillende soorten wijzigingen gedetecteerd, van de laag van de infra structuur voor de implementatie van toepassingen. Het is een Azure-resource provider op abonnements niveau waarmee wijzigingen in de resource in het abonnement worden gecontroleerd. Wijzigings analyse biedt gegevens voor verschillende diagnostische hulpprogram ma's waarmee gebruikers kunnen begrijpen welke wijzigingen problemen hebben veroorzaakt.

In het volgende diagram ziet u de architectuur van de wijzigings analyse:

![Architectuur diagram van de manier waarop wijzigingen in de analyse wijzigings gegevens worden opgehaald en biedt client hulpprogramma's](./media/change-analysis/overview.png)

De huidige wijzigings analyse is geïntegreerd in de ervaring voor het **vaststellen en oplossen van problemen** in de app service Web-app. Als u detectie van wijzigingen wilt inschakelen en wijzigingen in de Web-App wilt weer geven, raadpleegt u de sectie *wijzigings analyse voor het onderdeel web apps* verderop in dit artikel.

### <a name="azure-resource-manager-deployment-changes"></a>Implementatie wijzigingen Azure Resource Manager

Met behulp van de [resource grafiek van Azure](https://docs.microsoft.com/azure/governance/resource-graph/overview)kunt u een historisch overzicht krijgen van de manier waarop de Azure-resources die als host voor uw toepassing fungeren, na verloop van tijd worden gewijzigd. Met wijzigings analyse kunnen bijvoorbeeld wijzigingen in IP-configuratie regels, beheerde identiteiten en SSL-instellingen worden gedetecteerd. Dus als een tag wordt toegevoegd aan een web-app, wordt de wijziging in de analyse van wijzigingen weer gegeven. Deze informatie is beschikbaar zolang de `Microsoft.ChangeAnalysis` resource provider is ingeschakeld in het Azure-abonnement.

### <a name="changes-in-web-app-deployment-and-configuration"></a>Wijzigingen in de implementatie en configuratie van de web-app

Met de wijzigings analyse worden de implementatie-en configuratie status van een toepassing elke 4 uur vastgelegd. Het kan bijvoorbeeld wijzigingen in de omgevings variabelen van de toepassing detecteren. Het hulp programma berekent de verschillen en geeft aan wat er is gewijzigd. In tegens telling tot Resource Manager-wijzigingen is de wijzigings informatie voor code-implementatie mogelijk niet onmiddellijk beschikbaar in het hulp programma. Als u de meest recente wijzigingen in de analyse van wijzigingen wilt weer geven, selecteert u **wijzigingen nu scannen**.

![Scherm opname van de knop "wijzigingen nu scannen"](./media/change-analysis/scan-changes.png)

### <a name="dependency-changes"></a>Wijzigingen van afhankelijkheden

Wijzigingen in bron afhankelijkheden kunnen ook problemen veroorzaken in een web-app. Als een web-app bijvoorbeeld aanroept in een redis-cache, kan de redis-cache-SKU invloed hebben op de prestaties van de web-app. Als u wijzigingen in afhankelijkheden wilt detecteren, controleert u of de DNS-record van de web-app is gewijzigd. Op deze manier worden wijzigingen in alle app-onderdelen geïdentificeerd die problemen kunnen veroorzaken.
Momenteel worden de volgende afhankelijkheden ondersteund:
- Web Apps
- Azure Storage
- Azure SQL


## <a name="change-analysis-for-the-web-apps-feature"></a>De analyse van de functie Web Apps wijzigen

In Azure Monitor is de analyse van wijzigingen op dit moment ingebouwd in de ervaring van de self-service om **problemen op te lossen** . Toegang tot deze ervaring via de pagina **overzicht** van uw app service-toepassing.

![Scherm afbeelding van de knop overzicht en de knop problemen vaststellen en oplossen](./media/change-analysis/change-analysis.png)

### <a name="enable-change-analysis-in-the-diagnose-and-solve-problems-tool"></a>Wijzigingen in de analyse inschakelen in het hulp programma problemen vaststellen en oplossen

1. Selecteer **Beschik baarheid en prestaties**.

    ![Scherm afbeelding van de opties voor het oplossen van problemen met Beschik baarheid en prestaties](./media/change-analysis/availability-and-performance.png)

1. Selecteer **toepassings wijzigingen**. Het is niet mogelijk dat de functie ook beschikbaar is in **toepassings crashes**.

   ![Scherm opname van de knop ' toepassings crashes '](./media/change-analysis/application-changes.png)

1. Selecteer **nu inschakelen**om de analyse van wijzigingen in te scha kelen.

   ![Scherm opname van de opties ' toepassings crashes '](./media/change-analysis/enable-changeanalysis.png)

1. Schakel de **analyse van wijzigingen** in en selecteer **Opslaan**.

    ![Scherm opname van de gebruikers interface voor het inschakelen van een analyse](./media/change-analysis/change-analysis-on.png)


1. Als u de wijzigings analyse wilt openen, selecteert u **diagnoses en problemen** > oplossen beschik**baarheid en** > crashes van**toepassing**. U ziet een grafiek met een overzicht van het type wijzigingen in de loop van de tijd, samen met details over deze wijzigingen:

     ![Scherm opname van de weer gave diff wijzigen](./media/change-analysis/change-view.png)


### <a name="enable-change-analysis-at-scale"></a>Wijzigings analyse op schaal inschakelen

Als uw abonnement een groot aantal web-apps bevat, kan het inschakelen van de service op het niveau van de web-app inefficiënt zijn. In dit geval volgt u deze alternatieve instructies.

### <a name="register-the-change-analysis-resource-provider-for-your-subscription"></a>Registreer de resource provider voor de wijzigings analyse voor uw abonnement

1. De functie vlag voor het wijzigen van analyse registreren (preview). Omdat de functie vlag in preview is, moet u deze registreren om deze zichtbaar te maken voor uw abonnement:

   1. Open [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/).

      ![Scherm afbeelding van wijzigings Cloud Shell](./media/change-analysis/cloud-shell.png)

   1. Wijzig het shell type in **Power shell**.

      ![Scherm afbeelding van wijzigings Cloud Shell](./media/change-analysis/choose-powershell.png)

   1. Voer de volgende PowerShell-opdracht uit:

        ``` PowerShell
        Set-AzContext -Subscription <your_subscription_id> #set script execution context to the subscription you are trying to enable
        Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.ChangeAnalysis" -ListAvailable #Check for feature flag availability
        Register-AzureRmProviderFeature -FeatureName PreviewAccess -ProviderNamespace Microsoft.ChangeAnalysis #Register feature flag
        ```

1. Registreer de resource provider voor het wijzigen van de analyse voor het abonnement.

   - Ga naar **abonnementen**en selecteer het abonnement dat u wilt inschakelen in de wijzigings service. Selecteer vervolgens resource providers:

        ![Scherm afbeelding die laat zien hoe u de resource provider voor de wijzigings analyse registreert](./media/change-analysis/register-rp.png)

       - Selecteer **micro soft. ChangeAnalysis**. Selecteer vervolgens boven aan de pagina **registreren**.

       - Nadat de resource provider is ingeschakeld, kunt u een verborgen tag in de web-app instellen om wijzigingen op het niveau van de implementatie te detecteren. Als u een verborgen tag wilt instellen, volgt u de instructies onder **kan geen wijzigings analyse gegevens ophalen**.

   - U kunt ook een Power shell-script gebruiken om de resource provider te registreren:

        ```PowerShell
        Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState #Check if RP is ready for registration

        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis" #Register the Change Analysis RP
        ```

        Als u Power shell wilt gebruiken om een verborgen tag in een web-app in te stellen, voert u de volgende opdracht uit:

        ```powershell
        $webapp=Get-AzWebApp -Name <name_of_your_webapp>
        $tags = $webapp.Tags
        $tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
        Set-AzResource -ResourceId <your_webapp_resourceid> -Tag $tag
        ```

     > [!NOTE]
     > Nadat u de verborgen tag hebt toegevoegd, moet u mogelijk nog steeds tot 4 uur wachten voordat u de wijzigingen gaat bekijken. De resultaten worden uitgesteld omdat wijzigings analyse alleen om de 4 uur uw web-app scant. Het schema van 4 uur beperkt de gevolgen voor de prestaties van de scan.

## <a name="next-steps"></a>Volgende stappen

- Schakel Application Insights in voor [Azure-app Services-apps](azure-web-apps.md).
- Schakel Application Insights in voor [Azure VM-en Azure virtual machine Scale set door IIS gehoste apps](azure-vm-vmss-apps.md).
- Meer informatie over [Azure resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview), waarmee u de analyse van energie wijzigingen kunt aanbrengen.
