---
title: Mijn eerste PowerShell-runbook in Azure Automation
description: Zelfstudie die u helpt bij het maken, testen en publiceren van een eenvoudig PowerShell-runbook.
keywords: azure powershell, zelfstudie powershell-script, powershell-automatisering
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8f3185a2c7633ba0cb5a9b266bcddf023d3c36e1
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166449"
---
# <a name="my-first-powershell-runbook"></a>Mijn eerste PowerShell-runbook

> [!div class="op_single_selector"]
> * [Grafisch](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell-werkstroom](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)

In deze zelfstudie wordt stap voor stap het maken van een [PowerShell-runbook](automation-runbook-types.md#powershell-runbooks) in Azure Automation beschreven. U begint met een eenvoudig runbook dat u testen en publiceren terwijl u leert hoe u de status van de runbooktaak volgen. Vervolgens wijzigt u het runbook zodanig dat Azure-resources daadwerkelijk worden beheerd, in dit geval door een virtuele machine van Azure te starten. Ten slotte wordt u het runbook krachtiger door runbookparameters toe te voegen.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Azure-abonnement. Als u nog geen abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of u aanmelden voor een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Automation-account](automation-offering-get-started.md) om het runbook te bevatten en te verifiëren voor Azure-resources. Dit account moet machtigingen hebben om de virtuele machine te starten en stoppen.
* Een virtuele machine van Azure. U stopt en start deze machine, dus het mag geen productiemachine zijn.

## <a name="step-1---create-new-runbook"></a>Stap 1: nieuw runbook maken
U begint met het maken van een eenvoudig runbook waarmee de tekst weergeeft *Hello World*.

1. Open uw Automation-account in Azure Portal.

   Op de Automation-accountpagina vindt u een beknopte weergave van de resources in dit account. U hebt als het goed is al enkele assets. De meeste daarvan zijn de modules die automatisch zijn opgenomen in een nieuw Automation-account. Ook moet u de referentieasset hebben die wordt genoemd in de [vereisten](#prerequisites).

1. Klik op **Runbooks** onder **procesautomatisering** om de lijst van runbooks te openen.
2. Maak een nieuw runbook door te klikken op de **+ toevoegen van een runbook** knop en vervolgens **een nieuw runbook maken**.
3. Geef het runbook de naam *MyFirstRunbook-PowerShell*.
4. In dit geval u gaat maken een [PowerShell-runbook](automation-runbook-types.md#powershell-runbooks) , dus selecteer **Powershell** voor **runbooktype**.
5. Klik op **Maken** om het runbook te maken en de teksteditor te openen.

## <a name="step-2---add-code-to-the-runbook"></a>Stap 2: code toevoegen aan het runbook
U kunt de code rechtstreeks in het runbook typen of u kunt cmdlets, runbooks en assets selecteren in het besturingselement Bibliotheek en deze laten toevoegen aan het runbook met eventuele gerelateerde parameters. Voor dit scenario typt u rechtstreeks in het runbook.

1. Uw runbook is momenteel leeg, typ *Write-Output "Hallo wereld".* in de hoofdtekst van het script.<br><br> ![Hello World](media/automation-first-runbook-textual-powershell/automation-helloworld.png)  
2. Klik op **Opslaan** om het runbook op te slaan.

## <a name="step-3---test-the-runbook"></a>Stap 3: het runbook testen
Voordat u het runbook publiceert om het beschikbaar te maken in productie, wilt u het testen om er zeker van te zijn dat het goed werkt. Wanneer u een runbook test, voert u de **concept**versie uit en geeft u de uitvoer interactief weer.

1. Klik op **Testvenster** om het testvenster te openen.
2. Klik op **Start** om de test te starten. Dit moet de enige ingeschakelde optie zijn.
3. Een [runbooktaak](automation-runbook-execution.md) wordt gemaakt en de status ervan wordt weergegeven.

   In eerste instantie is de taakstatus *In de wachtrij geplaatst*. Hiermee wordt aangegeven dat er wordt gewacht tot er in de cloud een runbook worker beschikbaar is. Wordt verplaatst naar *vanaf* wanneer een werkrol de taak claimt en daarna *met* wanneer het runbook daadwerkelijk wordt uitgevoerd.  

1. Wanneer de runbooktaak is voltooid, wordt de uitvoer ervan weergegeven. In het geval is, ziet u *Hello World*.<br><br> ![Uitvoer testvenster](media/automation-first-runbook-textual-powershell/automation-testpane-output.png)  
2. Sluit het testvenster om terug te gaan naar het papier.

## <a name="step-4---publish-and-start-the-runbook"></a>Stap 4: het runbook publiceren en starten
Het runbook dat u hebt gemaakt, bevindt zich nog steeds in de modus Concept. U moet publiceren voordat u deze in productie kan uitvoeren.  Wanneer u een runbook publiceert, overschrijft u de bestaande gepubliceerde versie met de conceptversie. In het geval hebt u nog geen geen gepubliceerde versie omdat u het runbook zojuist hebt gemaakt.

1. Klik op **Publiceren** om het runbook te publiceren en klik vervolgens op **Ja** wanneer hierom wordt gevraagd.
2. Als u naar links schuift om het runbook in weer te geven de **Runbooks** deelvenster, het wordt nu een **ontwerpstatus** van **gepubliceerd**.
3. Schuif terug naar rechts om het deelvenster voor **MyFirstRunbook-PowerShell** weer te geven.  
   Met de opties bovenaan kunnen we het runbook starten, het runbook weergeven, plannen dat het op een bepaald moment in de toekomst start of een [webhook](automation-webhooks.md) maken, zodat het kan worden gestart via een HTTP-aanroep.
4. U wilt het runbook starten, dus klik op **Start** en klik vervolgens op **Ok** wanneer de pagina Runbook starten wordt geopend.
5. De pagina van een taak wordt geopend voor de runbooktaak die u hebt gemaakt. u kunt dit deelvenster sluiten, maar in dit geval u laat deze geopend zodat u de voortgang van de taak kan bekijken.
6. De taakstatus wordt weergegeven in **taaksamenvatting** en komt overeen met de statussen die u hebt gezien wanneer u het runbook getest.<br><br> ![Taakoverzicht](media/automation-first-runbook-textual-powershell/job-pane-status-blade-jobsummary.png)<br>  
7. Zodra het runbook de status bevat *voltooid*onder **overzicht** klikt u op **uitvoer**. Het deelvenster Uitvoer wordt geopend en ziet u uw *Hello World*.<br><br> ![Taakuitvoer](media/automation-first-runbook-textual-powershell/job-pane-status-blade-outputtile.png)<br> 
8. Sluit u de uitvoer-pagina.
9. Klik op **Alle logboeken** om het deelvenster Streams voor de runbooktaak te openen. U zou alleen *Hallo wereld* moeten zien in de uitvoerstroom, maar er kunnen ook andere stromen voor een runbooktaak worden weergegeven, zoals Uitgebreid en Fout als hiernaar wordt geschreven met het runbook.<br><br> ![Alle logboeken](media/automation-first-runbook-textual-powershell/job-pane-status-blade-alllogstile.png)<br>   
10. Sluit de pagina stromen en de pagina van de taak om terug te keren naar de pagina MyFirstRunbook-PowerShell.
11. Onder **Details**, klikt u op **taken** om het deelvenster Taken voor dit runbook te openen. Hiermee worden alle taken weergegeven die met dit runbook zijn gemaakt. U zou slechts één weergegeven taak moeten zien, aangezien de taak slechts eenmaal is uitgevoerd.<br><br> ![Takenlijst](media/automation-first-runbook-textual-powershell/runbook-control-job-tile.png)  
12. U kunt op deze taak klikken om hetzelfde taakvenster te openen dat u hebt bekeken toen u het runbook startte. Hiermee kunt u teruggaan in de tijd en de details bekijken van elke taak die voor een bepaald runbook is gemaakt.

## <a name="step-5---add-authentication-to-manage-azure-resources"></a>Stap 5: verificatie toevoegen voor het beheren van Azure-resources
U hebt het runbook getest en gepubliceerd, maar tot nu toe doet het nog niets nuttigs. U wilt dat er Azure-resources mee worden beheerd. Het is niet kunt doen die al is, tenzij u hebt het laten verifiëren met de referenties die wordt verwezen in de [vereisten](#prerequisites). doet u dat met de **Connect-AzureRmAccount** cmdlet. Als u bij het beheren van resources voor meerdere abonnementen die u wilt gebruiken de **- AzureRmContext** parameter samen met [Get-AzureRmContext](/powershell/module/azurerm.profile/get-azurermcontext).

   ```powershell
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
-ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

   Get-AzureRmVM -ResourceGroupName myResourceGroup -AzureRmContext $AzureContext
   ```

1. Open de teksteditor door te klikken op **bewerken** op de pagina MyFirstRunbook-PowerShell.
2. u hoeft niet de **Write-Output** regel meer, dus gaan en deze verwijdert.
3. Typ of kopieer en plak de volgende code waarmee de verificatie wordt uitgevoerd met uw Uitvoeren als-account voor Automation:

   ```powershell
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   ```

   > [!IMPORTANT]
   > **Add-AzureRmAccount** en **Login-AzureRmAccount** zijn nu aliassen voor **Connect-AzureRMAccount**. Als de **Connect-AzureRMAccount** cmdlet niet bestaat, kunt u **Add-AzureRmAccount** of **Login-AzureRmAccount**, of u kunt uw modules bijwerken in uw Automation Account naar de nieuwste versie.

4. Klik op **testvenster** zodat u het runbook kan testen.
5. Klik op **Start** om de test te starten. Zodra deze is voltooid, ontvangt u uitvoer zoals hieronder afgebeeld, waarin basisinformatie van uw account wordt weergegeven. Hiermee wordt bevestigd dat de referentie geldig is.<br><br> ![Verifiëren](media/automation-first-runbook-textual-powershell/runbook-auth-output.png)

## <a name="step-6---add-code-to-start-a-virtual-machine"></a>Stap 6: code toevoegen om een virtuele machine te starten
Nu dat uw runbook voor uw Azure-abonnement verifieert, kunt u resources kunt beheren. u toevoegen een opdracht voor het starten van een virtuele machine. U kunt een virtuele machine kiezen in uw Azure-abonnement en voor nu u hardcoderen we deze naam in het runbook.

1. Na *Connect-AzureRmAccount*, type *Start-AzureRmVM-Name 'VMName' - ResourceGroupName 'NameofResourceGroup'* daarbij de naam en de resourcegroep de naam van de virtuele machine te starten.  
   
   ```
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
   ```
   <br>
2. Sla het runbook en klik vervolgens op **testvenster** zodat u deze kunt testen.
3. Klik op **Start** om de test te starten. Nadat deze is voltooid, controleert u of de virtuele machine is gestart.

## <a name="step-7---add-an-input-parameter-to-the-runbook"></a>Stap 7: een invoerparameter toevoegen aan het runbook
Uw runbook momenteel de virtuele machine gestart die u vastgelegd in het runbook zou, maar het nuttiger zijn als u de virtuele machine opgeven wanneer het runbook wordt gestart. U toevoegen invoerparameters aan het runbook om deze functionaliteit te bieden.

1. Voeg parameters voor *VMName* en *ResourceGroupName* aan het runbook en gebruik deze variabelen met de **Start-AzureRmVM** cmdlet zoals in het volgende voorbeeld.

   ```
   Param(
    [string]$VMName,
    [string]$ResourceGroupName
   )
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
   ```
   <br>
1. Sla het runbook op en open het testvenster. U kunt nu waarden opgeven voor de twee invoervariabelen die in de test worden gebruikt.
2. Sluit het testvenster.
3. Klik op **Publiceren** om de nieuwe versie van het runbook te publiceren.
4. Stop de virtuele machine die u in de vorige stap hebt gestart.
5. Klik op **OK** om het runbook te starten. Typ waarden voor **VMName** en **ResourceGroupName** voor de virtuele machine die u wilt starten.<br><br> ![Parameter doorgeven](media/automation-first-runbook-textual-powershell/automation-pass-params.png)<br>  
6. Wanneer het runbook is voltooid, controleert u of de virtuele machine is gestart.

## <a name="differences-from-powershell-workflow"></a>Verschillen met PowerShell Workflow
PowerShell-runbooks hebben dezelfde levenscyclus, mogelijkheden en beheer als PowerShell Workflow-runbooks, maar er zijn enkele verschillen en beperkingen:

1. PowerShell-runbooks worden snel uitgevoerd vergeleken met PowerShell Workflow-runbooks omdat ze geen compilatiestap hebben.
2. Met PowerShell Workflow-runbooks worden controlepunten ondersteund. Met behulp van controlepunten kunnen PowerShell Workflow-runbooks vanaf elk punt in het runbook worden hervat, terwijl PowerShell-runbooks alleen vanaf het begin kunnen worden hervat.
3. Met PowerShell Workflow-runbooks worden parallelle en seriële uitvoering ondersteund, terwijl met PowerShell-runbooks opdrachten alleen serieel kunnen worden uitgevoerd.
4. In een PowerShell Workflow-runbook kan een activiteit, een opdracht of een scriptblok een eigen runspace hebben, terwijl in een PowerShell-runbook alles in een script in één runspace wordt uitgevoerd. Er zijn ook een aantal [syntactische verschillen](https://technet.microsoft.com/magazine/dn151046.aspx) tussen een systeemeigen PowerShell-runbook en een PowerShell Workflow-runbook.

## <a name="next-steps"></a>Volgende stappen
* Zie [Mijn eerste grafische runbook](automation-first-runbook-graphical.md) om aan de slag te gaan met grafische runbooks
* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks
* Zie [Azure Automation-runbooktypen](automation-runbook-types.md) voor meer informatie over runbooktypen, hun voordelen en beperkingen
* Zie [Systeemeigen PowerShell-scriptondersteuning in Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/) voor meer informatie over de functie voor PowerShelll-scriptondersteuning

