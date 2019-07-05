---
title: Analyse van de toepassing wijzigen in Azure Monitor gebruiken om te vinden van problemen met web-app | Microsoft Docs
description: Analyse van de toepassing wijzigen in Azure Monitor om problemen van toepassing op live sites op Azure App Service te gebruiken.
services: application-insights
author: cawams
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: cawa
ms.openlocfilehash: 45df8f9e57223ea60a11c6af2187d362184cae2b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443345"
---
# <a name="use-application-change-analysis-preview-in-azure-monitor"></a>Analyse (preview) van de toepassing wijzigen in Azure Monitor gebruiken

Wanneer er een probleem voor live site of een storing optreedt, is het essentieel dat u snel de hoofdoorzaak vaststellen. Standard bewakingsoplossingen mogelijk u te waarschuwen voor een probleem. Ze kunnen zelfs aangeven welk onderdeel is mislukt. Maar deze waarschuwing wordt niet altijd onmiddellijk wordt uitgelegd van de fout veroorzaakt. U weet dat uw site vijf minuten geleden gewerkt en nu deze is verbroken. Wat er in de afgelopen vijf minuten gewijzigd? Dit is de vraag die analyse voor wijziging van toepassing is ontworpen om te beantwoorden in Azure Monitor.

Voortbouwend op de kracht van [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview), analyse van de wijziging geeft inzicht in uw Azure-toepassingswijzigingen aan observability verhogen en verlagen MTTR (gemiddelde tijd om te herstellen).

> [!IMPORTANT]
> Analyse van de wijziging is momenteel in preview. Deze preview-versie wordt geleverd zonder een service level agreement. Deze versie wordt niet aanbevolen voor productieworkloads. Sommige functies mogelijk niet ondersteund of mogelijk beperkt. Zie voor meer informatie, [aanvullende gebruiksvoorwaarden voor Microsoft Azure previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="overview"></a>Overzicht

Analyse van de wijziging detecteert verschillende soorten wijzigingen uit de laag infrastructuur helemaal tot aan de implementatie van toepassing. Het is een abonnement op Azure-resourceprovider waarmee wordt gecontroleerd of de wijzigingen van resources in het abonnement. Wijziging analyse levert gegevens voor verschillende diagnostische hulpprogramma's waarmee gebruikers te begrijpen wat verandert er de oorzaak van problemen.

Het volgende diagram illustreert de architectuur van de analyse van wijziging:

![Architectuurdiagram van hoe analyse wijzigen wijzigingsgegevens worden opgehaald en geeft de client-hulpprogramma 's](./media/change-analysis/overview.png)

Op dit moment wijziging analyse is geïntegreerd in de **vaststellen en oplossen van problemen met** -ervaring in de App Service-web-app. Als u wilt wijzigen zodat en wijzigingen in de web-app weergeven, Zie de *analyse wijzigen voor de functie Web Apps* verderop in dit artikel.

### <a name="azure-resource-manager-deployment-changes"></a>Wijzigingen van Azure Resource Manager-implementatie

Met behulp van [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview), wijziging analyse levert een historisch overzicht van hoe de Azure-resources die uw toepassing wordt gehost na verloop van tijd zijn veranderd. Wijziging analyse kunt detecteren, bijvoorbeeld: wijzigingen in IP-configuratie van regels, beheerde identiteiten en SSL-instellingen. Dus als een label wordt toegevoegd aan een web-app, analyse wijzigen de wijziging wordt weergegeven. Deze informatie is beschikbaar als de `Microsoft.ChangeAnalysis` resourceprovider is ingeschakeld in het Azure-abonnement.

### <a name="changes-in-web-app-deployment-and-configuration"></a>Wijzigingen in de web-app-implementatie en configuratie

Analyse van de wijziging wordt de status van de implementatie en configuratie van een toepassing elke 4 uur vastgelegd. Deze kan worden gedetecteerd, bijvoorbeeld: wijzigingen in de omgevingsvariabelen van de toepassing. Het hulpprogramma berekent de verschillen en geeft op wat er is gewijzigd. In tegenstelling tot Resource Manager-wijzigingen, informatie over de wijziging van de code implementatie mogelijk niet beschikbaar onmiddellijk in het hulpprogramma. Als u wilt de meest recente wijzigingen in analyse van de wijziging weergeven, selecteert u **Scan verandert nu**.

![Schermopname van de knop 'Scan verandert nu'](./media/change-analysis/scan-changes.png)

### <a name="dependency-changes"></a>Wijzigingen van de afhankelijkheid

Wijzigingen in de afhankelijkheden van resources kunnen ook problemen veroorzaken in een web-app. Bijvoorbeeld, als een web-app naar een Redis-cache aanroepen, de Redis-cache SKU kan van invloed zijn op de prestaties van de web-apps. Voor het detecteren van wijzigingen in de afhankelijkheden, controleert de wijziging Analysis DNS-record voor de web-app. Op deze manier verwijst naar wijzigingen in alle app-componenten die problemen kunnen veroorzaken.

## <a name="change-analysis-for-the-web-apps-feature"></a>Analyse van de functie Web Apps wijzigen

In Azure Monitor, analyse van de wijziging op dit moment is ingebouwd in de self-service **vaststellen en oplossen van problemen** optreden. Toegang tot deze ervaring van de **overzicht** pagina van uw App Service-toepassing.

![Schermopname van de knop 'Overzicht' en ' vaststellen en oplossen van problemen ' knop](./media/change-analysis/change-analysis.png)

### <a name="enable-change-analysis-in-the-diagnose-and-solve-problems-tool"></a>Inschakelen van de analyse van de wijziging in de vaststellen en oplossen van problemen met hulpprogramma

1. Selecteer **beschikbaarheid en prestaties**.

    ![Schermafbeelding van de 'beschikbaarheid en prestaties' opties voor probleemoplossing](./media/change-analysis/availability-and-performance.png)

1. Selecteer **wijzigingen in de toepassing**. Niet dat de functie is ook beschikbaar in **toepassing vastloopt**.

   ![Schermopname van de knop "Toepassing vastloopt"](./media/change-analysis/application-changes.png)

1. Als u Analysis wijzigen, schakelt **nu inschakelen**.

   ![Schermafbeelding van de opties "Toepassing vastloopt"](./media/change-analysis/enable-changeanalysis.png)

1. Schakel **wijziging Analysis** en selecteer **opslaan**.

    ![Schermafbeelding van de gebruikersinterface "Wijziging analyse inschakelen"](./media/change-analysis/change-analysis-on.png)


1. Voor toegang tot analyse wijzigen, selecteert u **vaststellen en oplossen van problemen met** > **beschikbaarheid en prestaties** > **toepassing vastloopt**. Hier ziet u een grafiek met een overzicht van het type van wijzigingen na verloop van tijd, samen met details over deze wijzigingen:

     ![Schermafbeelding van de wijziging diff-weergave](./media/change-analysis/change-view.png)


### <a name="enable-change-analysis-at-scale"></a>Analyse van de wijziging op schaal inschakelen

Als uw abonnement veel web-apps bevat, is het inschakelen van de service op het niveau van de web-app is inefficiënt. In dit geval, volgt u deze alternatieve instructies.

### <a name="register-the-change-analysis-resource-provider-for-your-subscription"></a>Registreer de resourceprovider Analysis wijzigen voor uw abonnement

1. Registreer de functievlag wijziging analyse (preview). Omdat de functievlag in preview is, moet u registreren, zodat deze zichtbaar is voor uw abonnement:

   1. Open [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/).

      ![Schermafbeelding van Cloud Shell wijzigen](./media/change-analysis/cloud-shell.png)

   1. Wijzigen van de shell type **PowerShell**.

      ![Schermafbeelding van Cloud Shell wijzigen](./media/change-analysis/choose-powershell.png)

   1. Voer de volgende PowerShell-opdracht uit:

        ``` PowerShell
        Set-AzContext -Subscription <your_subscription_id> #set script execution context to the subscription you are trying to enable
        Get-AzureRmProviderFeature -ProviderNamespace "Microsoft.ChangeAnalysis" -ListAvailable #Check for feature flag availability
        Register-AzureRmProviderFeature -FeatureName PreviewAccess -ProviderNamespace Microsoft.ChangeAnalysis #Register feature flag
        ```

1. Registreer de resourceprovider Analysis wijzigen voor het abonnement.

   - Ga naar **abonnementen**, en selecteer het abonnement dat u wilt inschakelen in de service wijzigen. Selecteer vervolgens de resourceproviders:

        ![Schermopname die laat zien hoe u om de wijziging Analysis-resourceprovider te registreren](./media/change-analysis/register-rp.png)

       - Select **Microsoft.ChangeAnalysis**. Selecteer vervolgens aan de bovenkant van de pagina **registreren**.

       - Nadat de resourceprovider is ingeschakeld, kunt u een verborgen label instellen op de web-app voor het detecteren van wijzigingen op het niveau van de implementatie. Om in te stellen een verborgen code, volg de instructies onder **kan niet worden opgehaald van de wijziging analysegegevens**.

   - U kunt ook een PowerShell-script gebruiken om de resourceprovider te registreren:

        ```PowerShell
        Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState #Check if RP is ready for registration

        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.ChangeAnalysis" #Register the Change Analysis RP
        ```

        Voor het gebruik van PowerShell een verborgen label instellen voor een web-app, voer de volgende opdracht:

        ```powershell
        $webapp=Get-AzWebApp -Name <name_of_your_webapp>
        $tags = $webapp.Tags
        $tags[“hidden-related:diagnostics/changeAnalysisScanEnabled”]=$true
        Set-AzResource -ResourceId <your_webapp_resourceid> -Tag $tag
        ```

     > [!NOTE]
     > Nadat u de verborgen tag toevoegt, moet u mogelijk nog steeds te wachten tot 4 uur voordat u wijzigingen zien. Resultaten worden vertraagd omdat wijziging Analysis uw web-app alleen elke 4 uur scant. De planning van 4 uur beperkt de gevolgen voor de prestaties van de scan.

## <a name="next-steps"></a>Volgende stappen

- App Service bewaken effectiever door [Application Insights-functies inschakelen](azure-web-apps.md) in Azure Monitor.
- Meer informatie over [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview), die helpt power wijziging analyse.
