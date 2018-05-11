---
title: Verwijderen van resourcegroepen met Azure Automation automatiseren
description: PowerShell Workflow-versie van een Azure Automation-scenario, inclusief runbooks om alle resourcegroepen in uw abonnement te verwijderen.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: c8fcb7abbc28aef06c43b6ece89eb7bd6916694e
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
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
| NameFilter (vereist) |Hiermee geeft u een naamfilter op, zodat u de resourcegroepen die u van plan bent te verwijderen, kunt beperken. U kunt meerdere waarden doorgeven met behulp van een met komma's gescheiden lijst.<br>Het filter is niet hoofdlettergevoelig en overeenkomt met een resourcegroep die de tekenreeks. |
| PreviewMode (optioneel) |Hiermee voert u het runbook uit om te zien welke resourcegroepen er worden verwijderd, maar er wordt geen actie ondernomen.<br>De standaard is **waar**. Zo voorkomt u dat u onbedoeld een of meer resourcegroepen verwijdert die aan het runbook zijn doorgegeven. |

## <a name="install-and-configure-this-scenario"></a>Dit scenario installeren en configureren
### <a name="prerequisites"></a>Vereisten
Dit runbook verifieert het gebruik van het [Azure Uitvoeren als-account](automation-sec-configure-azure-runas-account.md).    

### <a name="install-and-publish-the-runbooks"></a>De runbooks installeren en publiceren
Nadat u het runbook hebt gedownload, kunt u dit importeren met behulp van de procedure in [Importing runbook procedures](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation) (Procedures voor het importeren van runbooks). Publiceer het runbook nadat dit is geïmporteerd in uw Automation-account.

## <a name="using-the-runbook"></a>Het runbook gebruiken
De volgende stappen maakt u de uitvoering van dit runbook en de helpen die u meer vertrouwd raken met hoe het werkt. U wilt het runbook testen in dit voorbeeld wordt de resourcegroep niet werkelijk te verwijderen.  

1. Open uw Automation-account via Azure Portal en klik op **Runbooks**.
2. Selecteer het runbook **Remove-ResourceGroup** en klik op **Starten**.
3. Wanneer u het runbook start de **Runbook starten** pagina wordt geopend en u kunt de parameters configureren. Geef de namen van resourcegroepen in uw abonnement dat u voor het testen gebruiken kunt en zorgt ervoor geen kwaad dat als per ongeluk worden verwijderd.

   > [!NOTE]
   > Zorg ervoor dat **Previewmode** is ingesteld op **true** om te voorkomen dat de geselecteerde resourcegroepen worden verwijderd. Dit runbook wordt niet verwijderd van de resourcegroep die het Automation-account met dit runbook bevat.  
   >
   >
1. Nadat u alle parameterwaarden hebt geconfigureerd, klikt u op **OK**. Het runbook wordt in de wachtrij geplaatst om te worden uitgevoerd.  

De details van de **verwijderen ResourceGroup** runbook taak in de Azure-portal onder **Resource**, selecteer **taken** in het runbook. Selecteer, de taak die u wilt weergeven. Het taakoverzicht geeft de invoerparameters en de uitvoerstroom weer, naast algemene informatie over de taak en eventuele uitzonderingen.<br> ![Taakstatus van het runbook Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-status.png).

Het **Taakoverzicht** bevat berichten van de uitvoer-, waarschuwings- en foutstromen. Selecteer **Uitvoer** om gedetailleerde resultaten van de uitvoering van het runbook weer te geven.<br> ![Uitvoerresultaten van het runbook Remove-ResourceGroup](media/automation-scenario-remove-resourcegroup/remove-resourcegroup-runbook-job-output.png)

## <a name="next-steps"></a>Volgende stappen
* Zie [Creating or importing a runbook in Azure Automation](automation-creating-importing-runbook.md) (Een runbook in Azure Automation maken of importeren) om uw eigen runbook te maken.
* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks.
