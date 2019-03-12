---
title: Uw Azure Automation-Account verplaatsen naar een ander abonnement
description: In dit artikel wordt beschreven hoe u uw Automation-Account verplaatsen naar een ander abonnement
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/11/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 01f3995a80375f2deada13c36c48600243a17623
ms.sourcegitcommit: 1902adaa68c660bdaac46878ce2dec5473d29275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/11/2019
ms.locfileid: "57733244"
---
# <a name="move-your-automation-account-to-another-subscription"></a>Uw Automation-Account verplaatsen naar een ander abonnement

Azure biedt u de mogelijkheid bepaalde resources verplaatsen naar een nieuwe resourcegroep of abonnement met dezelfde tenant systeemeigen via de Azure portal, PowerShell, Azure CLI of REST-API. Zie voor meer informatie over het proces, [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../../azure-resource-manager/resource-group-move-resources.md). Automation-Accounts zijn een van de resources die kunnen worden verplaatst, maar er zijn speciale stappen die nodig zijn bij het verplaatsen van uw Automation-Account.

De hoofdstappen voor het verplaatsen van uw Automation-Account te zijn:

* Uw oplossingen verwijderen
* Uw werkruimte ontkoppelen
* Het Automation-Account verplaatsen
* Verwijderen en opnieuw maken van het Run As-Accounts
* Uw oplossingen opnieuw in te schakelen

## <a name="remove-solutions"></a>De oplossingen verwijderen

Als u wilt loskoppelen van uw werkruimte van uw Automation-Account, de wijzigingen en inventaris, worden updatebeheer en het starten/stoppen van VM's uit uur oplossingen verwijderd uit uw werkruimte.

In de resourcegroep, selecteert u elk **oplossing** en klikt u op **verwijderen**. Op de **Resources verwijderen** pagina, Controleer de resources wilt verwijderen, en klikt u op **verwijderen**.

![Oplossingen verwijderen uit de Azure portal](../media/move-account/delete-solutions.png)

U kunt doen met dezelfde taak de [Remove-AzureRmResource](/powershell/module/azurerm.resources/remove-azurermresource) cmdlet, zoals wordt weergegeven in het volgende voorbeeld:

```azurepowershell-interactive
$workspaceName = <myWorkspaceName>
$resourceGroupName = <myResourceGroup>
Remove-AzureRmResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "ChangeTracking($workspaceName)" -ResourceGroupName $resourceGroupName
Remove-AzureRmResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Updates($workspaceName)" -ResourceGroupName $resourceGroupName
Remove-AzureRmResource -ResourceType 'Microsoft.OperationsManagement/solutions' -ResourceName "Start-Stop-VM($workspaceName)" -ResourceGroupName $resourceGroupName
```

### <a name="additional-steps-for-startstop-vms"></a>Aanvullende stappen voor VM's starten/stoppen

Voor de **starten/stoppen van VM** oplossing, moet u ook de regels voor waarschuwingen die zijn gemaakt door de oplossing te verwijderen.

In de Azure-portal, gaat u naar de resourcegroep en selecteer **waarschuwingen** onder **bewaking**. Op de **waarschuwingen** weergeeft, schakelt **Waarschuwingsregels beheren**

![Waarschuwingen-pagina waarbij die u op de regels voor waarschuwingen beheren klikken](../media/move-account/alert-rules.png)

Op de **regels** pagina, ziet u een lijst met alle van de waarschuwingen die zijn geconfigureerd in die resourcegroep. De **VM's starten/stoppen** oplossing maakt 3 waarschuwingsregels

* AutoStop_VM_Child
* ScheduledStartStop_Parent
* SequencedStartStop_Parent

Selecteer deze 3 regels voor waarschuwingen en klik op **verwijderen**. Deze actie wordt deze regels voor waarschuwingen verwijderd.

![Regels voor de pagina met regels die zijn geselecteerd en wordt verwijderd](../media/move-account/delete-rules.png)

> [!NOTE]
> Als u geen waarschuwingsregels niet ziet op de **regels** pagina, wijzigt u de **Status** om weer te geven **uitgeschakeld** waarschuwt wanneer u mogelijk hebt uitgeschakeld ze.

Wanneer de regels voor waarschuwingen zijn verwijderd, moet u de actiegroep die is gemaakt voor meldingen voor de virtuele machine starten/stoppen oplossing verwijderen.

Ga in de Azure-portal naar **Monitor**, selecteer **waarschuwingen**, en klikt u op **actiegroepen beheren**.

De actiegroep selecteren in de lijst, moet deze de naam van de **StartStop_VM_Notification**. Klik op de pagina actie groepen **verwijderen**

![Actie groepspagina op verwijderen te klikken](../media/move-account/delete-action-group.png)

Op deze manier kunt u uw actiegroep met PowerShell verwijderen. Deze actie wordt uitgevoerd met de [Remove-AzureRmActionGroup](/powershell/module/azurerm.insights/remove-azurermactiongroup) cmdlet zoals te zien is in het volgende voorbeeld:

```azurepowershell-interactive
Remove-AzureRmActionGroup -ResourceGroupName <myResourceGroup> -Name StartStop_VM_Notification
```

## <a name="unlink-your-workspace"></a>Uw werkruimte ontkoppelen

In de Azure-portal, gaat u naar uw **Automation-Account**. Onder **gerelateerde Resources**, klikt u op **gekoppelde werkruimte**. Klik op **werkruimte ontkoppelen** ontkoppelen van de werkruimte van uw Automation-Account.

![Bezig met ontkoppelen van een werkruimte van een Automation-Account](../media/move-account/unlink-workspace.png)

## <a name="move-your-automation-account"></a>Uw Automation-Account verplaatsen

Wanneer de vorige items hebt verwijderd, kunt u doorgaan met het verwijderen van uw Automation-Account en de runbooks. Navigeer in de Azure-portal naar de resourcegroep van uw Automation-Account. Selecteer **verplaatsen** en vervolgens **verplaatsen naar een ander abonnement**.

![Pagina resourcegroep verplaatsen naar een ander abonnement selecteren](../media/move-account/move-resources.png)

Selecteer de resources in uw resourcegroep die u wilt verplaatsen. Zorg ervoor dat u uw **Automation-Account**, **Runbook**, en **Log Analytics-werkruimte** resources.

Nadat de verplaatsing voltooid is, zijn er extra stappen beschreven die moeten worden genomen om op te halen alles werkt.

## <a name="recreate-run-as-accounts"></a>Opnieuw uitvoeren als-Accounts maken

[Run As-Accounts](../manage-runas-account.md) maken van een service-principal in Azure Active Directory om te verifiëren met Azure-resources. Wanneer u abonnementen wijzigt, is de bestaande uitvoeren als-account niet meer kan worden gebruikt door het Automation-Account.

Navigeer naar uw Automation-Account in het nieuwe abonnement en selecteer **Run as-accounts** onder **Accountinstellingen**. U ziet dat de Run As-Accounts worden nu als onvolledig weergegeven.

![Run As-Accounts weergegeven als onvolledig](../media/move-account/run-as-accounts.png)

Klik op elk uitvoeren als-Account en klik op de **eigenschappen** pagina, klikt u op **verwijderen** verwijderen van de Run As-Account.

> ! [OPMERKING] Als u bent niet gemachtigd om te maken of weergeven van de Run As-accounts ziet u het volgende bericht `You do not have permissions to create an Azure Run As account (service principal) and grant the Contributor role to the service principal.`. Zie voor meer informatie over de vereiste machtigingen voor het configureren van een uitvoeren als-account, [machtigingen nodig voor het configureren van uitvoeren als-accounts](../manage-runas-account.md#permissions).

Nadat het Run As-Accounts zijn verwijderd, klikt u op **maken** op de **Azure uitvoeren als-Account**. Op de **uitvoeren als-Account toevoegen** pagina, klikt u op **maken** de uitvoeren als-Account en service-principal te maken. Herhaal de voorgaande stappen met het **klassiek uitvoeren als-Account**.

## <a name="enable-solutions"></a>Oplossingen inschakelen

Nadat het Run As-Accounts opnieuw gemaakt is, schakelt u de oplossingen die u voordat u de verplaatsing verwijdert opnieuw. Om in te schakelen **wijzigingen bijhouden en inventaris** en **updatebeheer**, selecteert u de betreffende functie in uw Automation-Account. Kies de Log Analytics-werkruimte die u verplaatst en klik op **inschakelen**.

![Oplossingen in uw verplaatste Automation-Account opnieuw in te schakelen](../media/move-account/reenable-solutions.png)

De machines die toegevoegd met uw oplossingen zijn wordt opnieuw weergegeven omdat u verbinding maakt met de bestaande Log Analytics-werkruimte.

Als u wilt opnieuw inschakelen van de VM's starten/stoppen buiten kantooruren oplossing, moet u om de oplossing opnieuw te implementeren. Onder **gerelateerde Resources**, selecteer **starten/stoppen van VM**. Klik op **meer informatie over en inschakelen van de oplossing** en klikt u op **maken** implementatie te starten.

Op de **oplossing toevoegen** pagina, kies de Log Analytics-werkruimte en het Automation-Account.  

![Run As-Accounts weergegeven als onvolledig](../media/move-account/add-solution-vm.png)

Zie voor gedetailleerde instructies over het configureren van de oplossing [VM's starten/stoppen buiten kantooruren oplossing in Azure Automation](../automation-solution-vm-management.md)

## <a name="post-move-verification"></a>Bericht verplaatsen verificatie

Nadat de verplaatsing voltooid is, Controleer de verschillende scenario's in uw Automation-Account om te controleren of dat alles werkt zoals verwacht. De volgende tabel ziet u een lijst met taken die moeten worden gecontroleerd nadat de verplaatsing is voltooid:

|Mogelijkheid|Tests|Koppeling voor probleemoplossing|
|---|---|---|
|Runbooks|Een Runbook kan uitvoeren en verbinding maken met Azure-resources.|[Problemen met runbooks oplossen](../troubleshoot/runbooks.md)
| Broncodebeheer|U kunt een handmatige synchronisatie uitvoeren op uw opslagplaats van het besturingselement.|[Integratie van bronbeheer](../source-control-integration.md)|
|Wijzigingen bijhouden en Inventaris|Controleer of dat u huidige inventarisgegevens van uw machines zien.|[Wijzigingen bijhouden oplossen](../troubleshoot/change-tracking.md)|
|Updatebeheer|Controleer of u uw machines ziet en ze in orde</br>Voer een test-software-update-implementatie.|[Problemen met Update Management oplossen](../troubleshoot/update-management.md)|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het verplaatsen van resources in Azure, [verplaatsen van resources in Azure](../../azure-resource-manager/move-support-resources.md)
