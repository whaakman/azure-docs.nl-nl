---
title: Runbook- en galerieën voor Azure Automation
description: Runbooks en modules van Microsoft en de community zijn beschikbaar voor u installeren en gebruiken in uw Azure Automation-omgeving.  Dit artikel wordt beschreven hoe u toegang hebt tot deze resources en bij te dragen van uw runbooks in de galerie.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/11/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 68eb3f3e5c568bb518251aca2a4e76932ce2eee5
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56416204"
---
# <a name="runbook-and-module-galleries-for-azure-automation"></a>Runbook- en galerieën voor Azure Automation

In plaats van het maken van uw eigen runbooks en modules in Azure Automation, kunt u scenario's die al zijn gebouwd door Microsoft en de community openen.

Krijgt u runbooks uit de [Runbookgalerie](#runbooks-in-runbook-gallery) en -modules uit de [PowerShell Gallery](#modules-in-powerShell-gallery).  U kunt ook bijdragen aan de community door sharing-scenario's die u hebt ontwikkeld, Zie [een runbook toe te voegen aan de galerie](automation-runbook-gallery.md#adding-a-runbook-to-the-runbook-gallery)

## <a name="runbooks-in-runbook-gallery"></a>Runbooks in de Runbook-galerie

De [Runbookgalerie](https://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=RootCategory&f\[0\].Value=WindowsAzure&f\[1\].Type=SubCategory&f\[1\].Value=WindowsAzure_automation&f\[1\].Text=Automation) biedt tal van runbooks van Microsoft en de community die u in Azure Automation importeren kunt. Om een te gebruiken, downloadt u een runbook vanuit de galerie, die wordt gehost in de [TechNet Script Center](https://gallery.technet.microsoft.com/scriptcenter/site/upload), of u kunt runbooks rechtstreeks importeren uit de galerie in Azure portal.

U kunt alleen met de importeren rechtstreeks vanuit de Runbook-galerie met Azure portal. U kunt deze functie met behulp van Windows PowerShell niet uitvoeren.

> [!NOTE]
> De inhoud van alle runbooks ophalen uit de Runbook Gallery uit te voeren en moet u bijzonder voorzichtig bij het installeren en uitvoeren van deze in een productieomgeving, moet u valideren.

### <a name="to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal"></a>Een runbook importeren vanuit de Runbook-galerie met Azure portal

1. Open uw Automation-account in Azure Portal.
2. Onder **procesautomatisering**, klikt u op **runbookgalerie**
3. Ga naar het galerie-item u wilt en selecteert u deze om de details ervan weer te geven. U kunt extra zoekparameters invoeren voor de uitgever en het type aan de linkerkant.

   ![Door galerie bladeren](media/automation-runbook-gallery/browse-gallery.png)

4. Klik op **weergave SourceProject** om weer te geven van het item in de [TechNet Script Center](https://gallery.technet.microsoft.com/).
5. Voor het importeren van een item, klikt u op de details ervan te bekijken en klik vervolgens op de **importeren** knop.

   ![Knop importeren](media/automation-runbook-gallery/gallery-item-detail.png)

6. (Optioneel) Wijzig de naam van het runbook en klik vervolgens op **OK** voor het importeren van het runbook.
7. Het runbook wordt weergegeven op de **Runbooks** tabblad voor het Automation-Account.

### <a name="adding-a-runbook-to-the-runbook-gallery"></a>Een runbook toe te voegen aan de runbookgalerie

Microsoft raadt u runbooks toevoegen aan de Runbook-galerie waarvan u denkt dat nuttig is voor andere klanten.  U kunt een runbook door toevoegen [uploaden naar het Script Center](https://gallery.technet.microsoft.com/site/upload) rekening houdend met de volgende details.

* U moet opgeven *Windows Azure* voor de **categorie** en *Automation* voor de **subcategorie** voor het runbook moet worden weergegeven de de wizard.  
* Het uploaden moet één `.ps1` of `.graphrunbook` bestand.  Als het runbook is vereist voor alle modules, onderliggende runbooks of assets, moet u deze items in de beschrijving van het verzenden en in de sectie met opmerkingen van het runbook weergeven.  Hebt u een scenario voor het vereisen van meerdere runbooks, upload vervolgens elk afzonderlijk en de namen van de gerelateerde runbooks in elk van de bijbehorende beschrijvingen. Zorg ervoor dat u de dezelfde codes gebruiken, zodat ze in dezelfde categorie worden weergegeven. Een gebruiker moet lees de beschrijving om te weten dat andere runbooks vereist zijn het scenario om te werken.
* De tag 'GraphicalPS' toevoegen als u wilt publiceren een **grafisch runbook** (niet een grafische Workflow).
* Een PowerShell- of PowerShell-werkstroom codefragment invoegen in de beschrijving via **invoegen sectie met voorbeeldcode** pictogram.
* De samenvatting voor het uploaden wordt weergegeven in de Runbook Gallery-resultaten, zodat u gedetailleerde informatie waarmee een gebruiker identificeren van de functionaliteit van het runbook moet opgeven.
* U moet één tot drie van de volgende codes toewijzen aan het uploaden.  Het runbook wordt vermeld in de wizard onder de categorieën die overeenkomen met de labels.  Alle tags niet op deze lijst worden genegeerd door de wizard. Als u alle overeenkomende labels niet opgeeft, wordt het runbook wordt vermeld onder de andere categorie.
  
  * Backup
  * Capaciteitsbeheer
  * Het besturingselement wijzigen
  * Naleving
  * Dev / Test-omgevingen
  * Herstel na noodgevallen
  * Bewaking
  * Patch toepassen
  * Inrichten
  * Herstel
  * Levenscyclusbeheer van virtuele machine

* Automation werkt de galerie eenmaal per uur, zodat u kunt uw bijdragen niet onmiddellijk weergegeven.

## <a name="modules-in-powershell-gallery"></a>Modules in PowerShell Gallery

PowerShell-modules bevatten cmdlets die u in uw runbooks gebruiken kunt en bestaande modules die u in Azure Automation installeren kunt zijn beschikbaar in de [PowerShell Gallery](https://www.powershellgallery.com). U kunt deze galerie starten vanuit de Azure-portal en installeer ze rechtstreeks in Azure Automation. U kunt ook deze te downloaden en deze handmatig installeren.  

### <a name="to-import-a-module-from-the-automation-module-gallery-with-the-azure-portal"></a>Een module importeren uit de galerie met Automation-Module met de Azure-portal

1. Open uw Automation-account in Azure Portal.
2. Selecteer **Modules** onder **gedeelde bronnen** om de lijst van de modules te openen.
3. Klik op **bladeren in galerie** vanaf de bovenkant van de pagina.

   ![Modulegalerie](media/automation-runbook-gallery/modules-blade.png)

4. Op de **bladeren in galerie** pagina die u kunt zoeken op de volgende velden:

   * Modulenaam
   * Tags
   * Auteur
   * De naam van de cmdlet/DSC-resource

5. Een module die u wilt zoeken en selecteren om de details ervan weer te geven.  

   Als u op een specifieke module inzoomen, vindt u meer informatie. Deze informatie bevat een koppeling terug naar de PowerShell Gallery, vereiste afhankelijkheden, en alle van de cmdlets of DSC-resources die de module bevat.

   ![Details van de PowerShell-module](media/automation-runbook-gallery/gallery-item-details-blade.png)

6. Voor het installeren van de module rechtstreeks in Azure Automation, klikt u op de **importeren** knop.
7. Als u de knop importeren klikt op de **importeren** deelvenster ziet u de naam van de module die u gaat importeren. Als alle afhankelijkheden zijn geïnstalleerd, de **OK** knop is geactiveerd. Als u afhankelijkheden ontbreekt, moet u die afhankelijkheden importeren voordat u deze module kunt importeren.
8. Op de **importeren** pagina, klikt u op **OK** om de module te importeren. Terwijl Azure Automation een module in uw account importeert, pakt deze metagegevens over de module en de cmdlets. Deze actie kan enkele minuten duren nadat elke activiteit moet worden geëxtraheerd.
9.  U ontvangen een initiële melding dat de module wordt geïmplementeerd en nog een melding wanneer deze is voltooid.
10. Nadat de module is geïmporteerd, kunt u de beschikbare activiteiten zien. U kunt de resources in uw runbooks en Desired State Configuration gebruiken.

> [!NOTE]
> Modules die alleen ondersteuning voor PowerShell core worden niet ondersteund in Azure Automation en kunnen niet worden geïmporteerd in Azure portal of rechtstreeks vanuit de PowerShell Gallery geïmplementeerd.

## <a name="python-runbooks"></a>Python-Runbooks

Python-Runbooks zijn beschikbaar in de [Script Center galerie](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B1%5D.Type=ProgrammingLanguage&f%5B1%5D.Value=Python&f%5B1%5D.Text=Python&sortBy=Date&username=). U kunt bijdragen Python-runbooks aan de galerie Script Center. Wanneer u dit doet, zorg ervoor dat u de tag toevoegen **Python** tijdens het uploaden van uw inzending.

## <a name="requesting-a-runbook-or-module"></a>Een runbook of de module aanvragen

U kunt aanvragen verzenden [User Voice](https://feedback.azure.com/forums/246290-azure-automation/).  Als u hulp nodig bij het schrijven van een runbook of een vraag over PowerShell hebt, stel een vraag aan onze [forum](https://social.msdn.microsoft.com/Forums/windowsazure/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc).

## <a name="next-steps"></a>Volgende stappen

* Om te beginnen met runbooks, Zie [runbook in Azure Automation beheren](manage-runbooks.md)
* Zie voor meer informatie over de verschillen tussen PowerShell en PowerShell-werkstroom met runbooks, [Learning PowerShell-werkstroom](automation-powershell-workflow.md)