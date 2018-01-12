---
title: De verwijdering van resourcegroepen automatiseren | Microsoft Docs
description: PowerShell Workflow-versie van een Azure Automation-scenario, inclusief runbooks om alle resourcegroepen in uw abonnement te verwijderen.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: 
ms.assetid: b848e345-fd5d-4b9d-bc57-3fe41d2ddb5c
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: magoedte
ms.openlocfilehash: e1734bdd22ecfc4e54074f02582f5a8eca7d4f59
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="azure-automation-scenario---automate-removal-of-resource-groups"></a>Azure Automation-scenario: de verwijdering van resourcegroepen automatiseren
Veel klanten maken meer dan één resourcegroep. Sommige worden dan bijvoorbeeld gebruikt voor het beheer van productietoepassingen en andere als ontwikkelings-, test- en faseringsomgeving. Het automatiseren van de implementatie van deze resources is één ding, maar om een resourcegroep buiten gebruik te stellen met een enkele muisklik is heel wat anders. U kunt deze algemene beheertaak stroomlijnen met behulp van Azure Automation. Dit is handig als u werkt met een Azure-abonnement met een bestedingslimiet via een ledenaanbieding zoals MSDN of het programma Microsoft Partner Network Cloud Essentials.

Dit scenario is gebaseerd op een PowerShell-runbook en is ontworpen om een of meer resourcegroepen die u opgeeft uit uw abonnement te verwijderen. De standaardinstelling van het runbook is om een test uit te voeren voordat u doorgaat. Zo voorkomt u dat u de resourcegroep per ongeluk verwijdert voordat u er klaar voor bent om deze procedure te voltooien.   

## <a name="getting-the-scenario"></a>Het scenario ophalen
Dit scenario bestaat uit een PowerShell-runbook dat u kunt downloaden uit de [PowerShell Gallery](https://www.powershellgallery.com/packages/Remove-ResourceGroup/1.0/DisplayScript). U kunt het ook rechtstreeks vanuit de [Runbook-galerie](automation-runbook-gallery.md) in Azure Portal importeren.<br><br>

| Runbook | Beschrijving |
| --- | --- |
| Remove-ResourceGroup |Hiermee verwijdert u een of meer Azure-resourcegroepen en de daarbij behorende resources uit het abonnement. |

<br>
De volgende invoerparameters zijn voor dit runbook gedefinieerd:

| Parameter | Beschrijving |
| --- | --- |
| NameFilter (vereist) |Hiermee geeft u een naamfilter op, zodat u de resourcegroepen die u van plan bent te verwijderen, kunt beperken. U kunt meerdere waarden doorgeven met behulp van een met komma's gescheiden lijst.<br>Het filter is niet hoofdlettergevoelig en komt overeen met elke resourcegroep die de tekenreeks bevat. |
| PreviewMode (optioneel) |Hiermee voert u het runbook uit om te zien welke resourcegroepen er worden verwijderd, maar er wordt geen actie ondernomen.<br>De standaard is **waar**. Zo voorkomt u dat u onbedoeld een of meer resourcegroepen verwijdert die aan het runbook zijn doorgegeven. |

## <a name="install-and-configure-this-scenario"></a>Dit scenario installeren en configureren
### <a name="prerequisites"></a>Vereisten
Dit runbook verifieert het gebruik van het [Azure Uitvoeren als-account](automation-sec-configure-azure-runas-account.md).    

### <a name="install-and-publish-the-runbooks"></a>De runbooks installeren en publiceren
Nadat u het runbook hebt gedownload, kunt u dit importeren met behulp van de procedure in [Importing runbook procedures](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation) (Procedures voor het importeren van runbooks). Publiceer het runbook nadat dit is geïmporteerd in uw Automation-account.

## <a name="using-the-runbook"></a>Het runbook gebruiken
De volgende stappen helpen u bij het uitvoeren van dit runbook. Zo raakt u vertrouwd met hoe het werkt. In dit voorbeeld wordt het runbook alleen getest. De resourcegroep wordt niet daadwerkelijk verwijderd.  

1. Open uw Automation-account via Azure Portal en klik op **Runbooks**.
2. Selecteer het runbook **Remove-ResourceGroup** en klik op **Starten**.
3. Wanneer u het runbook start, wordt de blade **Runbook starten** geopend en kunt u de parameters configureren. Voer de namen in van de resourcegroepen in uw abonnement waarmee u wilt testen en waarvan het niet erg is als deze per ongeluk worden verwijderd.<br> ![Remove-ResouceGroup-parameters](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-input-parameters.png)

   > [!NOTE]
   > Zorg ervoor dat **Previewmode** is ingesteld op **true** om te voorkomen dat de geselecteerde resourcegroepen worden verwijderd.  **Opmerking:** Met dit runbook wordt de resourcegroep met het Automation-account dat dit runbook uitvoert, niet verwijderd.  
   >
   >
4. Nadat u alle parameterwaarden hebt geconfigureerd, klikt u op **OK**. Het runbook wordt in de wachtrij geplaatst om te worden uitgevoerd.  

Als u de details van de runbooktaak **Remove-ResourceGroup** in Azure Portal wilt weergeven, selecteert u **Taken** in het runbook. Het taakoverzicht geeft de invoerparameters en de uitvoerstroom weer, naast algemene informatie over de taak en eventuele uitzonderingen.<br> ![Taakstatus van het runbook Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-status.png).

Het **Taakoverzicht** bevat berichten van de uitvoer-, waarschuwings- en foutstromen. Selecteer **Uitvoer** om gedetailleerde resultaten van de uitvoering van het runbook weer te geven.<br> ![Uitvoerresultaten van het runbook Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-output.png)

## <a name="next-steps"></a>Volgende stappen
* Zie [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md) (Een runbook in Azure Automation maken of importeren) om uw eigen runbook te maken.
* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks.
