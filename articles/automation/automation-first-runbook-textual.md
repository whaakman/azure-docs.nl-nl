---
title: Mijn eerste PowerShell Workflow-runbook in Azure Automation
description: Zelfstudie die u helpt bij het maken, testen en publiceren van een eenvoudig tekstrunbook met behulp van PowerShell Workflow.
keywords: powershell-werkstroom, voorbeelden powershell-werkstroom, werkstroom powershell
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 32ad5ef4f297a64f0925febb247ccb117dffedf0
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37928587"
---
# <a name="my-first-powershell-workflow-runbook"></a>Mijn eerste PowerShell Workflow-runbook

> [!div class="op_single_selector"]
> * [Grafisch](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell-werkstroom](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)
> 
> 

In deze zelfstudie wordt stap voor stap het maken van een [PowerShell Workflow-runbook](automation-runbook-types.md#powershell-workflow-runbooks) in Azure Automation beschreven. U begint met een eenvoudig runbook dat u testen en publiceren terwijl we uitleggen hoe u kunt de status van de runbooktaak bijhouden. Vervolgens wijzigt u het runbook zodanig dat Azure-resources daadwerkelijk worden beheerd, in dit geval door een virtuele machine van Azure te starten. Ten slotte u het runbook krachtiger door runbookparameters toe te voegen.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Azure-abonnement. Als u nog geen abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of u aanmelden voor een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Automation-account](automation-offering-get-started.md) om het runbook te bevatten en te verifiëren voor Azure-resources.  Dit account moet machtigingen hebben om de virtuele machine te starten en stoppen.
* Een virtuele machine van Azure. u stopt en start deze machine, dus het mag geen productiemachine.

## <a name="step-1---create-new-runbook"></a>Stap 1: nieuw runbook maken
U begint met het maken van een eenvoudig runbook waarmee de tekst weergeeft *Hello World*.

1. Open uw Automation-account in Azure Portal.

   Op de Automation-accountpagina vindt u een beknopte weergave van de resources in dit account. U hebt als het goed is al enkele assets. De meeste daarvan zijn de modules die automatisch zijn opgenomen in een nieuw Automation-account. Ook moet u de referentieasset hebben die wordt genoemd in de [vereisten](#prerequisites).

1. Klik op **Runbooks** onder **procesautomatisering** om de lijst van runbooks te openen.
2. Maak een nieuw runbook door te klikken op de **+ toevoegen van een runbook** knop en vervolgens **een nieuw runbook maken**.
3. Geef het runbook de naam *MyFirstRunbook-Workflow*.
4. In dit geval u gaat maken een [PowerShell Workflow-runbook](automation-runbook-types.md#powershell-workflow-runbooks) , dus selecteer **Powershell-werkstroom** voor **runbooktype**.
5. Klik op **Maken** om het runbook te maken en de teksteditor te openen.

## <a name="step-2---add-code-to-the-runbook"></a>Stap 2: code toevoegen aan het runbook
U kunt de code rechtstreeks in het runbook typen of u kunt cmdlets, runbooks en assets selecteren in het besturingselement Bibliotheek en deze laten toevoegen aan het runbook met eventuele gerelateerde parameters. Voor dit scenario typt u rechtstreeks in het runbook.

1. Uw runbook is momenteel leeg met alleen de vereiste *werkstroom* #trefwoord, de naam van uw runbook en de accolades die de volledige werkstroom encases.

   ```powershell-interactive
   Workflow MyFirstRunbook-Workflow
   {
   }
   ```
2. Typ *Write-Output "Hallo wereld".* tussen de accolades.

   ```powershell-interactive
   Workflow MyFirstRunbook-Workflow
   {
   Write-Output "Hello World"
   }
   ```
3. Klik op **Opslaan** om het runbook op te slaan.

## <a name="step-3---test-the-runbook"></a>Stap 3: het runbook testen
Voordat u het runbook publiceert om het beschikbaar te maken in productie, wilt u het testen om er zeker van te zijn dat het goed werkt. Wanneer u een runbook test, voert u de **concept**versie uit en geeft u de uitvoer interactief weer.

1. Klik op **Testvenster** om het testvenster te openen.
2. Klik op **Start** om de test te starten. Dit moet de enige ingeschakelde optie zijn.
3. Een [runbooktaak](automation-runbook-execution.md) wordt gemaakt en de status ervan wordt weergegeven.

   In eerste instantie is de taakstatus *In de wachtrij geplaatst*. Hiermee wordt aangegeven dat er wordt gewacht tot in de cloud een runbook-werkrol beschikbaar is. Wordt verplaatst naar *vanaf* wanneer een werkrol de taak claimt en daarna *met* wanneer het runbook daadwerkelijk wordt uitgevoerd.  

1. Wanneer de runbooktaak is voltooid, wordt de uitvoer ervan weergegeven. In het geval is, ziet u *Hello World*.<br><br> ![Hello World](media/automation-first-runbook-textual/test-output-hello-world.png)
2. Sluit het testvenster om terug te gaan naar het papier.

## <a name="step-4---publish-and-start-the-runbook"></a>Stap 4: het runbook publiceren en starten
Het runbook dat u hebt gemaakt, bevindt zich nog steeds in de modus Concept. U moet publiceren voordat u deze in productie kan uitvoeren. Wanneer u een runbook publiceert, overschrijft u de bestaande gepubliceerde versie met de conceptversie. In het geval hebt u nog geen geen gepubliceerde versie omdat u het runbook zojuist hebt gemaakt.

1. Klik op **Publiceren** om het runbook te publiceren en klik vervolgens op **Ja** wanneer hierom wordt gevraagd.
2. Als u naar links schuift om het runbook in weer te geven de **Runbooks** deelvenster, het wordt nu een **ontwerpstatus** van **gepubliceerd**.
3. Schuif terug naar rechts om het deelvenster voor **MyFirstRunbook-Workflow** weer te geven.  
   Met de opties bovenaan kunnen we het runbook starten, plannen dat het op een bepaald moment in de toekomst start of een [webhook](automation-webhooks.md) maken zodat het kan worden gestart via een HTTP-aanroep.
4. u willen het runbook starten dus klikt u op **Start** en vervolgens **Ja** wanneer hierom wordt gevraagd.<br><br> ![Runbook starten](media/automation-first-runbook-textual/automation-runbook-controls-start.png)
5. Een taakdeelvenster wordt geopend voor de runbooktaak die u hebt gemaakt. u kunt dit deelvenster sluiten, maar in dit geval u laat deze geopend zodat u de voortgang van de taak kan bekijken.
6. De taakstatus wordt weergegeven in **taaksamenvatting** en komt overeen met de statussen die u hebt gezien wanneer u het runbook getest.<br><br> ![Taakoverzicht](media/automation-first-runbook-textual/job-pane-status-blade-jobsummary.png)
7. Zodra voor het runbook de status *Voltooid* wordt weergegeven, klikt u op **Uitvoer**. Het deelvenster Uitvoer wordt geopend en ziet u uw *Hello World*.<br><br> ![Taakoverzicht](media/automation-first-runbook-textual/job-pane-status-blade-outputtile.png)  
8. Sluit het deelvenster Uitvoer.
9. Klik op **Alle logboeken** om het deelvenster Streams voor de runbooktaak te openen. u ziet alleen *Hello World* in de uitvoerstroom, maar er kan weergegeven andere stromen voor een runbook-taak, zoals uitgebreid en fout als het runbook hiernaar wordt geschreven.<br><br> ![Taakoverzicht](media/automation-first-runbook-textual/job-pane-status-blade-alllogstile.png)
10. Sluit het deelvenster Streams en het deelvenster Taak om terug te gaan naar het deelvenster MyFirstRunbook.
11. Klik op **Taken** om het deelvenster Taken voor dit runbook te openen. Hiermee worden alle taken weergegeven die met dit runbook zijn gemaakt. u ziet alleen één weergegeven omdat u de taak slechts één keer uitgevoerd taak.<br><br> ![Taken](media/automation-first-runbook-textual/runbook-control-job-tile.png)
12. U kunt klikken op deze taak om de dezelfde taakdeelvenster dat u hebt bekeken toen u het runbook startten te openen. Hiermee kunt u teruggaan in de tijd en de details bekijken van elke taak die voor een bepaald runbook is gemaakt.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>Stap 5: verificatie toevoegen voor het beheren van Azure-resources
u hebt getest en gepubliceerd uw runbook, maar tot nu toe doet het nog niets nuttigs. wilt u deze Azure-resources beheren. Het is niet mogelijk om dat te doen maar tenzij u beschikt over het laten verifiëren met de referenties die wordt verwezen in de [vereisten](#prerequisites). doet u dat met de **Connect-AzureRmAccount** cmdlet.

1. Open de teksteditor door te klikken op **Bewerken** in het deelvenster MyFirstRunbook-Workflow.
2. u hoeft niet de **Write-Output** regel meer, dus gaan en deze verwijdert.
3. Plaats de cursor op een lege regel tussen de accolades.
4. Typ of kopieer en plak de volgende code waarmee de verificatie wordt uitgevoerd met uw Uitvoeren als-account voor Automation:

   ```powershell-interactive
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   ```

   > [!IMPORTANT]
   > **Add-AzureRmAccount** en **Login-AzureRmAccount** zijn nu aliassen voor **Connect-AzureRMAccount**. Als de **Connect-AzureRMAccount** cmdlet niet bestaat, kunt u **Add-AzureRmAccount** of **Login-AzureRmAccount**, of u kunt [uw-modules bijwerken ](automation-update-azure-modules.md) in uw Automation-Account naar de nieuwste versie. 

> [!NOTE]
> U moet mogelijk [uw-modules bijwerken](automation-update-azure-modules.md) ondanks dat u net een nieuw automation-account hebt gemaakt.

5. Klik op **testvenster** zodat u het runbook kan testen.
6. Klik op **Start** om de test te starten. Zodra deze is voltooid, ontvangt u uitvoer zoals hieronder afgebeeld, waarin basisinformatie van uw account wordt weergegeven. Hiermee wordt bevestigd dat de referentie geldig is.<br><br> ![Verifiëren](media/automation-first-runbook-textual/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>Stap 6: code toevoegen om een virtuele machine te starten
Nu dat uw runbook voor uw Azure-abonnement verifieert, kunt u resources kunt beheren. u toevoegen een opdracht voor het starten van een virtuele machine. U kunt een virtuele machine kiezen in uw Azure-abonnement en nu u hardcoderen in het runbook de naam zijn.

1. Na *Connect-AzureRmAccount*, type *Start-AzureRmVM-Name 'VMName' - ResourceGroupName 'NameofResourceGroup'* daarbij de naam en de resourcegroep de naam van de virtuele machine te starten.  

   ```powershell-interactive
   workflow MyFirstRunbook-Workflow
   {
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
   }
   ```
2. Sla het runbook en klik vervolgens op **testvenster** zodat u deze kunt testen.
3. Klik op **Start** om de test te starten. Nadat deze is voltooid, controleert u of de virtuele machine is gestart.

## <a name="step-7---add-an-input-parameter-to-the-runbook"></a>Stap 7: een invoerparameter toevoegen aan het runbook
uw runbook momenteel de virtuele machine gestart die u vastgelegd in het runbook zou, maar het nuttiger zijn als u de virtuele machine opgeven kunt wanneer het runbook wordt gestart. U toevoegen invoerparameters aan het runbook om deze functionaliteit te bieden.

1. Voeg parameters voor *VMName* en *ResourceGroupName* toe aan het runbook en gebruik deze variabelen met de cmdlet **Start-AzureRmVM**, zoals in het voorbeeld hieronder.

   ```powershell-interactive
   workflow MyFirstRunbook-Workflow
   {
    Param(
     [string]$VMName,
     [string]$ResourceGroupName
    )  
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
   }
   ```
2. Sla het runbook op en open het testvenster. U kunt nu waarden opgeven voor de twee invoervariabelen die in de test.
3. Sluit het testvenster.
4. Klik op **Publiceren** om de nieuwe versie van het runbook te publiceren.
5. Stop de virtuele machine die u in de vorige stap hebt gestart.
6. Klik op **Starten** om het runbook te starten. Typ waarden voor **VMName** en **ResourceGroupName** voor de virtuele machine die u wilt starten.<br><br> ![Runbook starten](media/automation-first-runbook-textual/automation-pass-params.png)<br>  
7. Wanneer het runbook is voltooid, controleert u of de virtuele machine is gestart.  

## <a name="next-steps"></a>Volgende stappen
* Zie [Mijn eerste grafische runbook](automation-first-runbook-graphical.md) om aan de slag te gaan met grafische runbooks
* Zie [Mijn eerste PowerShell-runbook](automation-first-runbook-textual-powershell.md) om aan de slag te gaan met PowerShell-runbooks
* Zie [Azure Automation-runbooktypen](automation-runbook-types.md) voor meer informatie over runbooktypen, hun voordelen en beperkingen
* Zie [Systeemeigen PowerShell-scriptondersteuning in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/) voor meer informatie over de functie voor PowerShelll-scriptondersteuning
