---
title: Mijn eerste grafische runbook in Azure Automation | Microsoft Docs
description: Zelfstudie die u helpt bij het maken, testen en publiceren van een eenvoudig grafisch runbook.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: 
keywords: runbook, runbook-sjabloon, runbook-automatisering, azure-runbook
ms.assetid: dcb88f19-ed2b-4372-9724-6625cd287c8a
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 948510eaaf55854bbc14d49bf78a8584c43f182d
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="my-first-graphical-runbook"></a>Mijn eerste grafische runbook

> [!div class="op_single_selector"]
> * [Grafisch](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell-werkstroom](automation-first-runbook-textual.md)
> * [Python](automation-first-runbook-textual-python2.md)
> 

In deze zelfstudie wordt stap voor stap het maken van een [grafisch runbook](automation-runbook-types.md#graphical-runbooks) in Azure Automation beschreven. We beginnen met een eenvoudig runbook dat we testen en publiceren terwijl wordt uitgelegd hoe u de status van de runbooktaak kunt bijhouden. Vervolgens wijzigt u het runbook zodanig dat Azure-resources daadwerkelijk worden beheerd, in dit geval door een virtuele machine van Azure te starten. Daarna rondt u de zelfstudie af door het runbook geavanceerder te maken. Dit doet u door runbookparameters en voorwaardelijke koppelingen toe te voegen.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Azure-abonnement. Als u nog geen abonnement hebt, kunt u [uw voordelen als MSDN-abonnee activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of u aanmelden voor een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Automation-account](automation-offering-get-started.md) om het runbook te bevatten en te verifiëren voor Azure-resources. Dit account moet machtigingen hebben om de virtuele machine te starten en stoppen.
* Een virtuele machine van Azure. U stopt en start deze machine, dus het mag geen productiemachine zijn.

## <a name="create-runbook"></a>Runbook maken

Begin met het maken van een eenvoudig runbook waarmee de tekst *Hallo wereld* als uitvoer wordt gegeven.

1. Open uw Automation-account in Azure Portal.

   Op de Automation-accountpagina vindt u een beknopte weergave van de resources in dit account. U zou al enkele assets moeten hebben. De meeste van deze assets zijn de modules die automatisch zijn opgenomen in een nieuw Automation-account. Ook moet u de referentieasset hebben die wordt genoemd in de [vereisten](#prerequisites).

2. Selecteer **Runbooks** onder **Procesbeheer** om de lijst van runbooks te openen.
3. Maak een nieuw runbook door te selecteren **+ een runbook toevoegen**, klikt u vervolgens op **een nieuw runbook maken**.
4. Geef het runbook de naam *MyFirstRunbook-Graphical*.
5. In dit geval gaat u een [grafisch runbook](automation-graphical-authoring-intro.md) maken, dus selecteer **Grafisch** voor **Runbooktype**.<br> ![Nieuw runbook](media/automation-first-runbook-graphical/create-new-runbook.png)<br>
6. Klik op **Maken** om het runbook te maken en de grafische editor te openen.

## <a name="add-activities"></a>Activiteiten toevoegen

Met het besturingselement Bibliotheek aan de linkerkant van de editor kunt u activiteiten selecteren die u aan uw runbook wilt toevoegen. U gaat een cmdlet **Write-Output** toevoegen om tekst als uitvoer van het runbook te krijgen.

1. Klik in het besturingselement Bibliotheek in het zoektekstvak en typ **Write-Output**. De zoekresultaten worden weergegeven op de volgende afbeelding: <br> ![Microsoft.PowerShell.Utility](media/automation-first-runbook-graphical/search-powershell-cmdlet-writeoutput.png)
1. Schuif omlaag naar de onderkant van de lijst. U kunt met de rechtermuisknop klikken op **Write-Output** en **Toevoegen aan papier** selecteren of klikken op de knop met de drie puntjes naast de cmdlet en **Toevoegen aan papier** selecteren.
1. Klik op de activiteit **Write-Output** op het papier. Deze actie wordt de pagina configuratie van besturingselement waarmee u de activiteit configureren geopend.
1. Het **label** krijgt standaard de naam van de cmdlet, maar u kunt dit wijzigen in een duidelijkere beschrijving. Wijzig dit in *Schrijf Hallo Wereld naar uitvoer*.
1. Klik op **Parameters** om waarden op te geven voor de parameters van de cmdlet.

   Sommige cmdlets hebben meerdere parametersets. U moet dan selecteren welke u wilt gebruiken. In dit geval heeft **Write-Output** slechts één parameterset, dus u hoeft er geen te selecteren.

1. Selecteer de parameter **InputObject**. Dit is de parameter waarin u de tekst opgeeft die naar de uitvoerstroom moet worden verzonden.
1. Selecteer in de vervolgkeuzelijst **Gegevensbron** de optie **PowerShell-expressie**. In de vervolgkeuzelijst **Gegevensbron** staan verschillende bronnen die u kunt gebruiken om een parameterwaarde te vullen.

   U kunt uitvoer van deze bronnen gebruiken, zoals een andere activiteit, een Automation-asset of een PowerShell-expressie. In dit geval is de uitvoer gewoon *Hallo wereld*. U kunt een PowerShell-expressie gebruiken en een tekenreeks opgeven.<br>

1. Typ in het vak **Expressie** de tekst *"Hallo wereld"* en klik vervolgens tweemaal op **OK** om terug te gaan naar het papier.
1. Klik op **Opslaan** om het runbook op te slaan.

## <a name="test-the-runbook"></a>Het runbook testen

Voordat u het runbook publiceert om het beschikbaar te maken in productie, wilt u het testen om er zeker van te zijn dat het goed werkt. Wanneer u een runbook test, voert u de **concept**versie uit en geeft u de uitvoer interactief weer.

1. Selecteer **testvenster** om de pagina van de Test te openen.
1. Klik op **Start** om de test te starten. Dit moet de enige ingeschakelde optie zijn.
1. Een [runbooktaak](automation-runbook-execution.md) wordt gemaakt en de status ervan wordt in het venster weergegeven.

   In eerste instantie is de taakstatus *In de wachtrij geplaatst*. Hiermee wordt aangegeven dat er wordt gewacht tot er in de cloud een runbook worker beschikbaar is. De taakstatus verandert daarna in *Starten* wanneer een werkrol de taak claimt en daarna in *Wordt uitgevoerd* wanneer het runbook daadwerkelijk wordt uitgevoerd.

1. Wanneer de runbooktaak is voltooid, wordt de uitvoer ervan weergegeven. In dit geval ziet u *Hallo wereld*.<br> ![Hello World](media/automation-first-runbook-graphical/runbook-test-results.png)
1. Sluit de pagina Test om terug te keren naar het canvas.

## <a name="publish-and-start-the-runbook"></a>Publiceren en het runbook starten

Het runbook dat u hebt gemaakt, bevindt zich nog steeds in de modus Concept. Het moet worden gepubliceerd voordat u het in productie kan uitvoeren. Wanneer u een runbook publiceert, overschrijft u de bestaande gepubliceerde versie met de conceptversie. In dit geval hebt u nog geen gepubliceerde versie omdat het runbook zojuist is gemaakt.

1. Selecteer **publiceren** om het runbook te publiceren en vervolgens **Ja** wanneer u wordt gevraagd.
1. Als u naar links schuift om weer te geven van het runbook in de **Runbooks** pagina bevat een **ontwerpstatus** van **gepubliceerde**.
1. Schuif terug naar rechts om weer te geven van de pagina voor **MyFirstRunbook**.

   Met de opties bovenaan kunnen we het runbook starten, plannen dat het op een bepaald moment in de toekomst start of een [webhook](automation-webhooks.md) maken zodat het kan worden gestart via een HTTP-aanroep.

1. Selecteer **Start** en vervolgens **Ja** wanneer u wordt gevraagd om het runbook te starten.
1. Een pagina van de taak is geopend voor de runbooktaak die is gemaakt. Controleer of de **taakstatus** **Voltooid** aangeeft.
1. Zodra voor het runbook de status *Voltooid* wordt weergegeven, klikt u op **Uitvoer**. De **uitvoer** pagina wordt geopend en ziet u de *Hallo wereld* in het deelvenster.
1. Sluit de uitvoer-pagina.
1. Klik op **alle logboeken** om de pagina Streams voor de runbooktaak te openen. U zou alleen *Hallo wereld* moeten zien in de uitvoerstroom, maar er kunnen ook andere stromen voor een runbooktaak worden weergegeven, zoals Uitgebreid en Fout als hiernaar wordt geschreven met het runbook.
1. Sluit de pagina alle logboeken en de pagina van de taak om terug te keren naar de pagina MyFirstRunbook.
1. Voor het weergeven van alle taken voor het runbook sluit de **taak** pagina en selecteer **taken** onder **RESOURCES**. Op deze blade worden alle taken weergegeven die met dit runbook zijn gemaakt. U zou slechts één weergegeven taak moeten zien, aangezien de taak slechts eenmaal is uitgevoerd.
1. U kunt op deze taak klikken om hetzelfde taakvenster te openen dat u hebt bekeken toen u het runbook startte. Hiermee kunt u teruggaan in de tijd en de details bekijken van elke taak die voor een bepaald runbook is gemaakt.

## <a name="create-variable-assets"></a>Variabele assets maken

U hebt het runbook getest en gepubliceerd, maar tot nu toe doet het nog niets nuttigs. U wilt dat er Azure-resources mee worden beheerd. Voordat u het runbook configureert voor verificatie, maakt u een variabele die het abonnements-id moet bevatten en u verwijst hiernaar nadat u de activiteit hebt ingesteld om te verifiëren in stap 6 hieronder. Door een verwijzing naar de abonnementscontext op te nemen, kunt u eenvoudig werken met verschillende abonnementen. Voordat u verdergaat, kopieert u uw abonnements-id uit de optie Abonnementen in het deelvenster Navigatie.

1. Selecteer op de pagina Automation-Accounts **variabelen** onder **gedeelde bronnen**.
1. Selecteer **toevoegen van een variabele**.
1. In de nieuwe variabele pagina in de **naam** Voer **AzureSubscriptionId** en in de **waarde** vak uw abonnement-id invoeren. Behoud *tekenreeks* voor het **type** en de standaardwaarde voor **Versleuteling**.
1. Klik op **Maken** om de variabele te maken.

## <a name="add-authentication"></a>Verificatie toevoegen

Nu u een variabele hebt die onze abonnements-id kan bevatten, kunt u uw runbook configureren voor verificatie met de Uitvoeren als-referenties waarnaar wordt verwezen in de [vereisten](#prerequisites). Dit doet u door de Uitvoeren als-verbinding voor Azure **Asset** en de cmdlet **Add-AzureRMAccount** toe te voegen aan het papier.

1. Ga terug naar uw runbook en selecteer **bewerken** op de pagina MyFirstRunbook.
1. U hoeft niet de **Schrijf Hallo wereld naar uitvoer** voordoet, dus klik op de weglatingstekens (...) en selecteer **verwijderen**.
1. Vouw in het besturingselement Bibliotheek **Assets**, **Verbindingen** uit en voeg **AzureRunAsConnection** toe aan het papier door **Toevoegen aan papier** te selecteren.
1. Typ in het besturingselement Bibliotheek **Add-AzureRmAccount** in het zoektekstvak.
1. Voeg **Add-AzureRmAccount** toe aan het papier.
1. Beweeg de muisaanwijzer over **Uitvoeren als-verbinding ophalen** totdat een cirkel wordt weergegeven aan de onderkant van de vorm. Klik op de cirkel en sleep de pijl naar **Add-AzureRmAccount**. De pijl die u hebt gemaakt, is een *koppeling*. Het runbook wordt gestart met **Uitvoeren als-verbinding ophalen** en vervolgens wordt **Add-AzureRmAccount** uitgevoerd.<br> ![Koppeling tussen activiteiten maken](media/automation-first-runbook-graphical/runbook-link-auth-activities.png)
1. Selecteer op het papier **Add-AzureRmAccount** en typ in het besturingselementvenster Configuratie **Aanmelden bij Azure** in het tekstvak **Label**.
1. Klik op **Parameters** en de pagina Parameterconfiguratie van activiteit wordt weergegeven.
1. **Add-AzureRmAccount** bevat meerdere parametersets, dus moet u er een selecteren voordat u parameterwaarden kunt opgeven. Klik op **Parameterset** en selecteer vervolgens de parameterset **ServicePrincipalCertificate**.
1. Als u de parameterset hebt geselecteerd, worden de parameters weergegeven op de pagina Parameterconfiguratie van activiteit. Klik op **APPLICATIONID**.<br> ![Parameters voor Azure RM-account toevoegen](media/automation-first-runbook-graphical/add-azurermaccount-params.png)
1. Selecteer op de pagina parameterwaarde **uitvoer van activiteit** voor de **gegevensbron** en selecteer **uitvoeren als-verbinding ophalen** uit de lijst in de **pad naar veld** textbox type **ApplicationId**, en klik vervolgens op **OK**. U geeft de naam van de eigenschap voor het pad naar het veld op omdat de uitvoer van de activiteit een object met meerdere eigenschappen bevat.
1. Klik op **CERTIFICATETHUMBPRINT**, en selecteer in de pagina parameterwaarde **uitvoer van activiteit** voor de **gegevensbron**. Selecteer **Uitvoeren als-verbinding ophalen** in de lijst, typ in het tekstvak **Pad naar veld** **CertificateThumbprint** en klik vervolgens op **OK**.
1. Klik op **SERVICEPRINCIPAL**, en selecteer in de pagina parameterwaarde **ConstantValue** voor de **gegevensbron**, klik op de optie **True**, en klik vervolgens op **OK**.
1. Klik op **TENANTID**, en selecteer in de pagina parameterwaarde **uitvoer van activiteit** voor de **gegevensbron**. Selecteer **Uitvoeren als-verbinding ophalen** in de lijst, typ in het tekstvak **Pad naar veld** **TenantId** en klik vervolgens tweemaal op **OK**.
1. Typ in het besturingselement Bibliotheek **Set-AzureRmContext** in het zoektekstvak.
1. Voeg **Set-AzureRmContext** toe aan het papier.
1. Selecteer op het papier **Set-AzureRmContext** en typ in het besturingselementvenster Configuratie **Abonnements-id opgeven** in het tekstvak **Label**.
1. Klik op **Parameters** en de pagina Parameterconfiguratie van activiteit wordt weergegeven.
1. **Set-AzureRmContext** bevat meerdere parametersets, dus moet u er een selecteren voordat u parameterwaarden kunt opgeven. Klik op **Parameterset** en selecteer vervolgens de parameterset **SubscriptionId**.
1. Als u de parameterset hebt geselecteerd, worden de parameters weergegeven op de pagina Parameterconfiguratie van activiteit. Klik op **SubscriptionID**.
1. Selecteer op de pagina parameterwaarde **Variabelenactivum** voor de **gegevensbron** en selecteer **AzureSubscriptionId** uit de lijst en klik vervolgens op **OK** twee keer.
1. Beweeg de muisaanwijzer over **Aanmelden bij Azure** totdat een cirkel wordt weergegeven aan de onderkant van de vorm. Klik op de cirkel en sleep de pijl naar **Abonnements-id opgeven**.

Uw runbook zou er op dit punt als volgt moeten uitzien: <br>![Configuratie runbookverificatie](media/automation-first-runbook-graphical/runbook-auth-config.png)

## <a name="add-activity-to-start-a-vm"></a>Activiteit voor het starten van een virtuele machine toevoegen

Hier voegt u een activiteit **Start-AzureRmVM** toe om een virtuele machine te starten. U kunt elke virtuele machine in uw Azure-abonnement selecteren en voorlopig hardcoderen we deze naam in de cmdlet.

1. Typ in het besturingselement Bibliotheek **Start-AzureRm** in het zoektekstvak.
2. Voeg **Start-AzureRmVM** toe aan het papier, klik erop en sleep het onder **Abonnements-id opgeven**.
1. Beweeg de muisaanwijzer over **Abonnements-id opgeven** totdat een cirkel wordt weergegeven aan de onderkant van de vorm. Klik op de cirkel en sleep de pijl naar **Start-AzureRmVM**.
1. Selecteer **Start-AzureRmVM**. Klik op **Parameters** en vervolgens op **Parameterset** om de sets voor **Start-AzureRmVM** weer te geven. Selecteer de parameterset **ResourceGroupNameParameterSetName**. Naast **ResourceGroupName** en **Name** staan uitroeptekens. Hiermee wordt aangegeven dat dit vereiste parameters zijn. Voor beide parameters worden tekenreekswaarden verwacht.
1. Selecteer **Name**. Selecteer **PowerShell-expressie** bij **Gegevensbron** en typ tussen dubbele aanhalingstekens de naam van de virtuele machine die u met dit runbook gaat starten. Klik op **OK**.
1. Selecteer **ResourceGroupName**. Gebruik **PowerShell-expressie** voor de **gegevensbron** en typ de naam van de resourcegroep tussen dubbele aanhalingstekens. Klik op **OK**.
1. Klik op Testvenster zodat u het runbook kunt testen.
1. Klik op **Start** om de test te starten. Nadat deze is voltooid, controleert u of de virtuele machine is gestart.

Uw runbook zou er op dit punt als volgt moeten uitzien: <br>![Configuratie runbookverificatie](media/automation-first-runbook-graphical/runbook-startvm.png)

## <a name="add-additional-input-parameters"></a>Aanvullende invoerparameters toevoegen

Met ons runbook wordt op dit moment de virtuele machine gestart in de resourcegroep die u hebt opgegeven in de cmdlet **Start-AzureRmVM**. Het runbook zou nuttiger zijn als we beide namen konden opgeven op het moment dat het runbook wordt gestart. U gaat daarom invoerparameters toevoegen aan het runbook om deze functionaliteit te bieden.

1. Open de grafische editor door te klikken op **Bewerken** in het deelvenster **MyFirstRunbook**.
1. Selecteer **invoer en uitvoer** en vervolgens **invoer toevoegen** om het deelvenster invoerparameter van Runbook te openen.
1. Geef *VMName* op voor de **Naam**. Behoud *tekenreeks* voor het **type**, maar wijzig **Verplicht** in *Ja*. Klik op **OK**.
1. Maak een tweede verplichte invoerparameter met de naam *ResourceGroupName* en klik vervolgens op **OK** om het venster **Invoer en uitvoer** te sluiten.<br> ![Invoerparameters voor runbook](media/automation-first-runbook-graphical/start-azurermvm-params-outputs.png)
1. Selecteer de activiteit **Start-AzureRmVM** en klik vervolgens op **Parameters**.
1. Wijzig de **gegevensbron** voor **Name** in **Runbookinvoer** en selecteer vervolgens **VMName**.
1. Wijzig de **gegevensbron** voor **ResourceGroupName** in **Runbookinvoer** en selecteer vervolgens **ResourceGroupName**.<br> ![Start-AzureVM-parameters](media/automation-first-runbook-graphical/start-azurermvm-params-runbookinput.png)
1. Sla het runbook op en open het testvenster. U kunt nu waarden opgeven voor de twee invoervariabelen die in de test worden gebruikt.
1. Sluit het testvenster.
1. Klik op **Publiceren** om de nieuwe versie van het runbook te publiceren.
1. Stop de virtuele machine die u in de vorige stap hebt gestart.
1. Klik op **Starten** om het runbook te starten. Typ waarden voor **VMName** en **ResourceGroupName** voor de virtuele machine die u wilt starten.
1. Wanneer het runbook is voltooid, controleert u of de virtuele machine is gestart.

## <a name="create-a-conditional-link"></a>Een voorwaardelijke koppeling maken

U gaat het runbook nu wijzigen zodat alleen wordt geprobeerd de virtuele machine te starten als deze nog niet is gestart. We doen dit door een cmdlet **Get-AzureRmVM** toe te voegen aan het runbook waarmee de exemplaarniveaustatus van de virtuele machine wordt opgehaald. We voegen vervolgens een PowerShell Workflow-codemodule genaamd **Status ophalen** toe met een PowerShell-codefragment om te bepalen of de status van de virtuele machine 'actief' of 'gestopt' is. Met een voorwaardelijke koppeling van de module **Status ophalen** wordt **Start-AzureRmVM** alleen uitgevoerd als de huidige uitvoeringsstatus 'gestopt' is. Ten slotte zorgt u dat een bericht wordt weergegeven waarin staat of de virtuele machine is gestart of niet, met de PowerShell-cmdlet Write-Output.

1. Open **MyFirstRunbook** in de grafische editor.
1. Verwijder de koppeling tussen **Abonnements-id opgeven** en **Start-AzureRmVM** door erop te klikken en vervolgens te drukken op de toets *Delete*.
1. Typ in het besturingselement Bibliotheek **Get-AzureRm** in het zoektekstvak.
1. Voeg **Get-AzureRmVM** toe aan het papier.
1. Selecteer **Get-AzureRmVM** en vervolgens **Parameterset** om de sets voor **Get-AzureRmVM** weer te geven. Selecteer de parameterset **GetVirtualMachineInResourceGroupNameParamSet**. Naast **ResourceGroupName** en **Name** staan uitroeptekens. Hiermee wordt aangegeven dat dit vereiste parameters zijn. Voor beide parameters worden tekenreekswaarden verwacht.
1. Selecteer onder **Gegevensbron** voor **Name** de optie **Runbookinvoer** en selecteer vervolgens **VMName**. Klik op **OK**.
1. Selecteer onder **Gegevensbron** voor **ResourceGroupName** de optie **Runbookinvoer** en selecteer vervolgens **ResourceGroupName**. Klik op **OK**.
1. Selecteer onder **Gegevensbron** voor **Status** de optie **Constante waarde** en klik vervolgens op **Waar**. Klik op **OK**.
1. Maak een koppeling van **Abonnements-id opgeven** naar **Get-AzureRmVM**.
1. Vouw in het besturingselement Bibliotheek **Runbookbesturing** uit en voeg **Code** toe aan het papier.  
1. Maak een koppeling van **Get-AzureRmVM** naar **Code**.  
1. Klik op **Code** en wijzig in het deelvenster Configuratie het label in **Status ophalen**.
1. Selecteer **Code** parameter, en de **Code-Editor** pagina wordt weergegeven.  
1. Plak het volgende codefragment in de code-editor:

    ```powershell-interactive
     $StatusesJson = $ActivityOutput['Get-AzureRmVM'].StatusesText
     $Statuses = ConvertFrom-Json $StatusesJson
     $StatusOut =""
     foreach ($Status in $Statuses){
     if($Status.Code -eq "Powerstate/running"){$StatusOut = "running"}
     elseif ($Status.Code -eq "Powerstate/deallocated") {$StatusOut = "stopped"}
     }
     $StatusOut
     ```

1. Maak een koppeling van **Status ophalen** naar **Start-AzureRmVM**.<br> ![Runbook met codemodule](media/automation-first-runbook-graphical/runbook-startvm-get-status.png)  
1. Selecteer de koppeling en wijzig in het deelvenster Configuratie **Voorwaarde toepassen** in **Ja**. De koppeling wordt een gestreepte lijn waarmee wordt aangegeven dat de doelactiviteit alleen wordt uitgevoerd als de voorwaarde 'waar' is.  
1. Voor **Expressie van voorwaarde** typt u *$ActivityOutput['Get Status'] -eq "Stopped"*. **Start-AzureRmVM** wordt nu alleen uitgevoerd als de virtuele machine is gestopt.
1. Vouw in het besturingselement Bibliotheek **Cmdlets** uit en vervolgens **Microsoft.PowerShell.Utility**.
1. Voeg **Write-Output** tweemaal aan het papier toe.
1. In het eerste besturingselement **Write-Output** klikt u op **Parameters** en wijzigt u de waarde voor **Label** in *VM gestart melden*.
1. Voor **InputObject** wijzigt u **Gegevensbron** in **PowerShell-expressie** en typt u de expressie *"$VMName successfully started."*.
1. In het tweede besturingselement **Write-Output** klikt u op **Parameters** en wijzigt u de waarde voor **Label** in *Starten VM mislukt melden*.
1. Voor **InputObject** wijzigt u **Gegevensbron** in **PowerShell-expressie** en typt u de expressie *"$VMName could not start."*.
1. Maak een koppeling van **Start-AzureRmVM** naar **VM gestart melden** en **Starten VM mislukt melden**.
1. Selecteer de koppeling naar **VM gestart melden** en wijzig **Voorwaarde toepassen** in **Waar**.
1. Voor **Expressie van voorwaarde** typt u *$ActivityOutput['Start-AzureRmVM'].IsSuccessStatusCode -eq $true*. Dit besturingselement Write-Output wordt nu alleen uitgevoerd als de virtuele machine is gestart.
1. Selecteer de koppeling naar **Starten VM mislukt melden** en wijzig **Voorwaarde toepassen** in **Waar**.
1. Voor **Expressie van voorwaarde** typt u *$ActivityOutput['Start-AzureRmVM'].IsSuccessStatusCode -ne $true*. Dit besturingselement Write-Output wordt nu alleen uitgevoerd als de virtuele machine niet is gestart. Uw runbook moet eruitzien als in de volgende afbeelding: <br> ![Runbook met Write-Output](media/automation-first-runbook-graphical/runbook-startazurermvm-complete.png)
1. Sla het runbook op en open het testvenster.
1. Start het runbook als de virtuele machine is gestopt; deze zou moeten starten.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over grafisch ontwerpen [Grafisch ontwerpen in Azure Automation](automation-graphical-authoring-intro.md)
* Zie [Mijn eerste PowerShell-runbook](automation-first-runbook-textual-powershell.md) om aan de slag te gaan met PowerShell-runbooks
* Zie [Mijn eerste PowerShell Workflow-runbook](automation-first-runbook-textual.md) om aan de slag te gaan met PowerShell Workflow-runbooks

