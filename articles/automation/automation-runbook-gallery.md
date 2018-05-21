---
title: Runbook- en galerieën voor Azure Automation
description: Runbooks en modules van Microsoft en de community zijn beschikbaar voor u installeren en gebruiken in uw Azure Automation-omgeving.  Dit artikel wordt beschreven hoe u toegang hebt tot deze bronnen en bij te dragen van uw runbooks aan de galerie.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 61d267898bab5e5df394d4097eacb33ada809518
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="runbook-and-module-galleries-for-azure-automation"></a>Runbook- en galerieën voor Azure Automation
In plaats van uw eigen runbooks en modules in Azure Automation, kunt u toegang tot allerlei scenario's die al zijn gebouwd door Microsoft en de community.  U kunt deze scenario's zonder aanpassingen gebruiken of u kunt ze als uitgangspunt gebruiken en ze bewerken voor uw specifieke vereisten.

U krijgt runbooks uit de [Runbookgalerie](#runbooks-in-runbook-gallery) en -modules van de [PowerShell Gallery](#modules-in-powerShell-gallery).  U kunt ook bijdragen aan de community delen scenario's die u ontwikkelt.

## <a name="runbooks-in-runbook-gallery"></a>Runbooks in Runbookgalerie
De [Runbookgalerie](http://gallery.technet.microsoft.com/scriptcenter/site/search?f\[0\].Type=RootCategory&f\[0\].Value=WindowsAzure&f\[1\].Type=SubCategory&f\[1\].Value=WindowsAzure_automation&f\[1\].Text=Automation) biedt tal van runbooks van Microsoft en de community die u in Azure Automation importeren kunt. Kunt u een runbook downloaden uit de galerie, die wordt gehost in de [TechNet Script Center](https://gallery.technet.microsoft.com/scriptcenter/site/upload), of u kunt rechtstreeks runbooks importeren uit de galerie in de Azure portal.

U kunt alleen met de importeren rechtstreeks vanuit de Runbookgalerie met de Azure portal. U kunt deze functie met Windows PowerShell niet uitvoeren.

> [!NOTE]
> De inhoud van alle runbooks die u niet uit de Runbookgalerie ophalen en uiterst voorzichtig met het installeren en uit te voeren in een productieomgeving, moet u valideren.
> 
> 

### <a name="to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal"></a>Een runbook importeren vanuit de galerie Runbook met de Azure-portal
1. Open uw Automation-account in Azure Portal.
2. Onder **procesautomatisering**, klikt u op **Runbooks galerie**
3. Ga naar het galerij-item u wilt en selecteert u deze om de details ervan weer te geven. U kunt aanvullende zoekparameters invoeren voor de publisher en het type aan de linkerkant.
   
    ![Galerie bladeren](media/automation-runbook-gallery/browse-gallery.png)
5. Klik op **weergave bronproject** om weer te geven van het item in de [TechNet Script Center](http://gallery.technet.microsoft.com/).
6. Voor het importeren van een item, klikt u op de bijbehorende details weergeven en klik vervolgens op de **importeren** knop.
   
    ![Knop importeren](media/automation-runbook-gallery/gallery-item-detail.png)
7. U kunt desgewenst de naam van het runbook wijzigen en klik vervolgens op **OK** voor het importeren van het runbook.
8. Het runbook wordt weergegeven op de **Runbooks** tabblad voor het Automation-Account.

### <a name="adding-a-runbook-to-the-runbook-gallery"></a>Een runbook toe te voegen aan de runbookgalerie
Microsoft raadt u runbooks toevoegen aan de Runbook-galerie die u denkt dat handig kan zijn voor andere klanten.  U kunt een runbook door toevoegen [uploaden naar het Script Center](http://gallery.technet.microsoft.com/site/upload) rekening houdend met de volgende details.

* U moet opgeven *Windows Azure* voor de **categorie** en *Automation* voor de **subcategorie** voor het runbook moet worden weergegeven in de wizard.  
* Het uploaden moet één .ps1 of .graphrunbook-bestand.  Als het runbook is vereist voor alle modules, onderliggende runbooks of activa, moet klikt u aanbieden die in de beschrijving van de verzending en in het gedeelte met opmerkingen van het runbook.  Als u een scenario voor het vereisen van meerdere runbooks hebt, elk afzonderlijk uploaden en de namen van de gerelateerde runbooks in elk van de bijbehorende beschrijvingen. Zorg ervoor dat de dezelfde codes te gebruiken, zodat ze in dezelfde categorie worden weergegeven. Een gebruiker moet lees de beschrijving als u wilt weten dat er andere runbooks vereist zijn het scenario om te werken.
* Voeg het label 'GraphicalPS' als u wilt publiceren een **grafisch runbook** (geen grafische werkstroom). 
* Een PowerShell- of PowerShell Workflow codefragment invoegen in het gebruik van de beschrijving **code sectie invoegen** pictogram.
* De samenvatting voor het uploaden wordt weergegeven in de resultaten Runbookgalerie zodat u moet bieden gedetailleerde informatie waarmee een gebruiker die de functionaliteit van het runbook te identificeren.
* U moet één tot drie van de volgende codes toewijzen aan het uploaden.  Het runbook wordt vermeld in de wizard onder de categorieën die overeenkomen met de labels.  Alle tags niet op deze lijst worden genegeerd door de wizard. Als u overeenkomende labels niet opgeeft, wordt het runbook wordt vermeld onder de andere categorie.
  
  * Backup
  * Capaciteitsbeheer
  * Wijzigingsbeheer
  * Naleving
  * Dev / testen omgevingen
  * Herstel na noodgevallen
  * Bewaking
  * Patch toepassen
  * Inrichten
  * Herstel
  * Levenscyclusbeheer van virtuele machine
* De galerie-Automation updates eenmaal een uur, zodat u uw bijdragen onmiddellijk niet zien.

## <a name="modules-in-powershell-gallery"></a>Modules in PowerShell Gallery
PowerShell-modules bevatten cmdlets die u in uw runbooks gebruiken kunt en bestaande modules die u in Azure Automation installeren kunt zijn beschikbaar in de [PowerShell Gallery](http://www.powershellgallery.com).  U kunt deze galerie starten vanuit de Azure-portal en deze rechtstreeks in Azure Automation installeren of u kunt deze downloaden en deze handmatig installeren.  

### <a name="to-import-a-module-from-the-automation-module-gallery-with-the-azure-portal"></a>Een module importeren uit de galerie met Automation-Module met de Azure-portal
1. Open uw Automation-account in Azure Portal.
2. Selecteer **Modules** onder **gedeelde bronnen** openen van de lijst met modules.
4. Klik op **bladeren galerie** vanaf de bovenkant van de pagina.
   
    ![Modulegalerie](media/automation-runbook-gallery/modules-blade.png) <br>
5. Op de **bladeren galerie** pagina die u kunt zoeken op de volgende velden:
   
   * Modulenaam
   * Tags
   * Auteur
   * Naam van cmdlet/DSC-resource
6. Zoek een module die u geïnteresseerd bent in en selecteert u deze om de details ervan weer te geven.  
   Wanneer u een specifieke module inzoomen, kunt u meer informatie weergeven over de module, een koppeling naar de PowerShell-galerie, inclusief eventuele vereiste afhankelijkheden en alle van de cmdlets en/of de DSC-resources met de module.
   
    ![Details van de PowerShell-module](media/automation-runbook-gallery/gallery-item-details-blade.png) <br>
7. Voor het installeren van de module rechtstreeks in Azure Automation, klikt u op de **importeren** knop.
8. Als u op de knop importeren op de **importeren** deelvenster ziet u de naam van de module die u wilt importeren. Als alle afhankelijkheden zijn geïnstalleerd, de **OK** knop wordt geactiveerd. Als u afhankelijkheden ontbreekt, moet u die importeren voordat u deze module kunt importeren.
9. Op de **importeren** pagina, klikt u op **OK** de module te importeren. Terwijl Azure Automation importeert een module in uw account, pakt deze metagegevens over de module en de cmdlets. Dit kan enkele minuten duren, omdat elke activiteit moet worden geëxtraheerd.
10. U ontvangt een initiële melding dat de module wordt geïmplementeerd en nog een melding wanneer deze is voltooid.
11. Nadat de module is geïmporteerd, kunt u de beschikbare activiteiten bekijken en kunt u de resources in uw runbooks en Desired State Configuration.

> [!NOTE]
> Modules die alleen ondersteuning voor PowerShell core worden niet ondersteund in Azure Automation en kunnen niet worden geïmporteerd in de Azure portal of geïmplementeerd rechtstreeks vanuit de PowerShell-galerie.

## <a name="requesting-a-runbook-or-module"></a>Een runbook of de module aanvragen
U kunt aanvragen verzenden [User Voice](https://feedback.azure.com/forums/246290-azure-automation/).  Als u nodig hebt te schrijven van een runbook of een vraag over PowerShell hebt, een vraag te posten onze [forum](http://social.msdn.microsoft.com/Forums/windowsazure/en-US/home?forum=azureautomation&filter=alltypes&sort=lastpostdesc).

## <a name="next-steps"></a>Volgende stappen
* Om te beginnen met runbooks, Zie [maken of importeren van een runbook in Azure Automation](automation-creating-importing-runbook.md)
* Zie voor informatie over de verschillen tussen PowerShell en PowerShell Workflow met runbooks, [Learning PowerShell workflow](automation-powershell-workflow.md)

