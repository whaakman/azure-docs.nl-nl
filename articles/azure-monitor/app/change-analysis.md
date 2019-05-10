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
ms.openlocfilehash: b04bd57c66b52e9a2c14d43c9e89d7c54fc48ae2
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415866"
---
# <a name="azure-monitor-application-change-analysis-public-preview"></a>Azure Monitor-toepassing wijzigen analyse (preview-versie)

Wanneer er een probleem live site/uitval optreedt, is het essentieel dat u snel bepalen van de hoofdoorzaak te achterhalen. Standard bewakingsoplossingen kunt u snel vaststellen dat er een probleem is, en vaak ook welk onderdeel is mislukt. Maar dit wordt niet altijd leiden tot een onmiddellijke uitleg waarom de fout zich voordoet. Uw site gewerkt vijf minuten geleden, nu deze is verbroken. Wat er in de afgelopen vijf minuten gewijzigd? Dit is de vraag die de nieuwe functie van Azure Monitor-analyse voor wijziging van toepassing is ontworpen om te beantwoorden. Met het bouwen van op de kracht van de [Azure Resource Graph](https://docs.microsoft.com/azure/governance/resource-graph/overview) wijziging toepassingsanalyse biedt inzicht in uw Azure App Service-App-wijzigingen ter observability verhogen en verlagen MTTR (Mean Time-herstel).

> [!IMPORTANT]
> Analyse van Azure Monitor toepassing wijzigen is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

## <a name="how-do-i-use-application-change-analysis"></a>Hoe gebruik ik toepassingsanalyse wijzigen?

Analyse van Azure Monitor toepassing wijzigen is momenteel gratis ingebouwd in de self-service **vaststellen en oplossen van problemen met** ondervindt, die kunnen worden benaderd vanaf de **overzicht** sectie van uw Azure App Service toepassing:

![Schermopname van Azure App Service-Overzicht pagina met rode vakken om de knop Overzicht en vaststellen en oplossen van problemen met knop](./media/change-analysis/change-analysis.png)

### <a name="enable-change-analysis"></a>Wijziging analyse inschakelen

1. Selecteer **beschikbaarheid en prestaties**

    ![Schermafbeelding van de beschikbaarheid en prestaties opties voor probleemoplossing](./media/change-analysis/availability-and-performance.png)

2. Klik op de **toepassing vastloopt** tegel.

   ![Schermafbeelding met de toepassing vastloopt tegel](./media/change-analysis/application-crashes-tile.png)

3. Om in te schakelen **wijziging Analysis** Selecteer **nu inschakelen**.

   ![Schermafbeelding van de beschikbaarheid en prestaties opties voor probleemoplossing](./media/change-analysis/application-crashes.png)

4. Om te profiteren van de volledige analysis functionaliteit set wijzigen **analyse wijzigen**, **scannen op wijzigingen in de code**, en **altijd op** naar **op** en selecteer **opslaan**.

    ![Schermafbeelding van de wijziging analysis-gebruikersinterface van Azure App Service inschakelen](./media/change-analysis/change-analysis-on.png)

    Als **wijziging Analysis** is ingeschakeld, kunt u zich voor het detecteren van niveau wijzigingen van resources. Als **scannen op wijzigingen in de code** is ingeschakeld, u ook implementatiebestanden te zien en wijzigingen in de configuratie van site. Inschakelen van **altijd op** optimaliseert de prestaties scannen wijzigen, maar mogelijk extra kosten vanuit het oogpunt van de facturering.

5.  Als alles is ingeschakeld, selecteren **vaststellen en oplossen van problemen met** > **beschikbaarheid en prestaties** > **toepassing vastloopt** kunt u toegang tot de ervaring van de analyse wijzigen. De grafiek wordt het type van de wijzigingen die hebben plaatsgevonden na verloop van tijd, samen met details over deze wijzigingen summerize:

     ![Schermafbeelding van wijzigen diff weergeven](./media/change-analysis/change-view.png)

## <a name="troubleshooting"></a>Probleemoplossing

### <a name="unable-to-fetch-change-analysis-information"></a>Kan geen wijziging analyse-informatie ophalen.

Dit is een tijdelijk probleem met de huidige preview onboarding-ervaring. De oplossing bestaat uit een verborgen label instellen voor uw web-app en klikt u vervolgens de pagina te vernieuwen:

   ![Schermafbeelding van de tag wijzigen verborgen](./media/change-analysis/hidden-tag.png)

Om in te stellen de verborgen tag met behulp van PowerShell:

1. Open Azure Cloudshell:

    ![Schermopname van Azure Cloud Shell wijzigen](./media/change-analysis/cloud-shell.png)

2. Wijzig het type shell in PowerShell:

   ![Schermopname van Azure Cloud Shell wijzigen](./media/change-analysis/choose-powershell.png)

3. Voer de volgende opdracht uit:

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
