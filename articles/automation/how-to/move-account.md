---
title: Uw Azure Automation-account verplaatsen naar een ander abonnement
description: In dit artikel wordt beschreven hoe u uw Automation-account verplaatsen naar een ander abonnement
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/11/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 59d433bfb888eaa41cc8f66bdf3ad28c16efbe5c
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58225957"
---
# <a name="move-your-azure-automation-account-to-another-subscription"></a>Uw Azure Automation-account verplaatsen naar een ander abonnement

Azure biedt u de mogelijkheid bepaalde resources verplaatsen naar een nieuwe resourcegroep of abonnement. U kunt resources via Azure portal, PowerShell, de Azure CLI of de REST-API verplaatsen. Zie voor meer informatie over het proces, [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../../azure-resource-manager/resource-group-move-resources.md). 

Azure Automation-accounts zijn een van de resources die kunnen worden verplaatst. In dit artikel leert u de stappen voor het Automation-accounts verplaatsen naar een andere resourcegroep of abonnement.

De hoofdstappen voor het verplaatsen van uw Automation-account zijn:

1. Verwijder uw oplossingen.
2. Uw werkruimte ontkoppelen.
3. Verplaats het Automation-account.
4. Verwijderen en opnieuw maken van Run As-accounts.
5. Uw oplossingen opnieuw inschakelen.

## <a name="remove-solutions"></a>De oplossingen verwijderen

Als u wilt loskoppelen van uw werkruimte van uw Automation-account, moeten deze oplossingen worden verwijderd uit uw werkruimte:
- **Wijzigingen bijhouden en inventaris**
- **Updatebeheer** 
- **Starten/stoppen van VM's buiten kantooruren** 

Elke oplossing te vinden in de resourcegroep, en selecteer **verwijderen**. Op de **Resources verwijderen** pagina, Controleer de resources wilt verwijderen en selecteer **verwijderen**.

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

Voor de **VM's starten/stoppen** oplossing, moet u ook de regels voor waarschuwingen die zijn gemaakt door de oplossing te verwijderen.

In de Azure-portal, gaat u naar de resourcegroep en selecteer **bewaking** > **waarschuwingen** > **Waarschuwingsregels beheren**.

![Waarschuwingen van de pagina tonen selectie van regels voor waarschuwingen beheren](../media/move-account/alert-rules.png)

Op de **regels** pagina, ziet u een lijst van de waarschuwingen die zijn geconfigureerd in die resourcegroep. De **VM's starten/stoppen** oplossing maakt drie regels voor waarschuwingen:

* AutoStop_VM_Child
* ScheduledStartStop_Parent
* SequencedStartStop_Parent

Selecteer deze drie waarschuwingsregels en selecteer vervolgens **verwijderen**. Deze actie wordt deze regels voor waarschuwingen verwijderd.

![Regels pagina Bevestiging van verwijderen voor de geselecteerde regels voor aanvragen](../media/move-account/delete-rules.png)

> [!NOTE]
> Als u geen waarschuwingsregels niet ziet op de **regels** pagina, wijzigt u de **Status** om weer te geven **uitgeschakeld** van waarschuwingen, omdat u mogelijk hebt uitgeschakeld ze.

Wanneer de regels voor waarschuwingen zijn verwijderd, verwijdert u de actiegroep die is gemaakt voor de **VM's starten/stoppen** oplossing meldingen.

Selecteer in de Azure portal, **Monitor** > **waarschuwingen** > **actiegroepen beheren**.

Selecteer **StartStop_VM_Notification** in de lijst. Selecteer op de pagina van de groep actie **verwijderen**.

![Verwijderen van de actiepagina selecteren](../media/move-account/delete-action-group.png)

Op deze manier kunt u uw actiegroep verwijderen met behulp van PowerShell met de [Remove-AzureRmActionGroup](/powershell/module/azurerm.insights/remove-azurermactiongroup) cmdlet, zoals te zien is in het volgende voorbeeld:

```azurepowershell-interactive
Remove-AzureRmActionGroup -ResourceGroupName <myResourceGroup> -Name StartStop_VM_Notification
```

## <a name="unlink-your-workspace"></a>Uw werkruimte ontkoppelen

Selecteer in de Azure portal, **Automation-account** > **gerelateerde Resources** > **gekoppelde werkruimte**. Selecteer **werkruimte ontkoppelen** ontkoppelen van de werkruimte van uw Automation-account.

![Een werkruimte van een Automation-account ontkoppelen](../media/move-account/unlink-workspace.png)

## <a name="move-your-automation-account"></a>Uw Automation-account verplaatsen

Nadat de vorige items is verwijderd, kunt u doorgaan met het verwijderen van uw Automation-account en de runbooks. In de Azure-portal, blader naar de resourcegroep van uw Automation-account. Selecteer **verplaatsen** > **verplaatsen naar een ander abonnement**.

![De pagina van resourcegroep verplaatsen naar een ander abonnement](../media/move-account/move-resources.png)

Selecteer de resources in uw resourcegroep die u wilt verplaatsen. Zorg ervoor dat u uw **Automation-account**, **Runbook**, en **Log Analytics-werkruimte** resources.

Nadat de verplaatsing voltooid is, zijn er extra stappen die nodig zijn om te controleren alles werkt.

## <a name="re-create-run-as-accounts"></a>Opnieuw uitvoeren als-accounts maken

[Run As-accounts](../manage-runas-account.md) maken van een service-principal in Azure Active Directory om te verifiëren met Azure-resources. Wanneer u abonnementen wijzigt, wordt het Automation-account niet meer gebruikt bestaande uitvoeren als-account.

Ga naar uw Automation-account in het nieuwe abonnement en selecteer **Run as-accounts** onder **Accountinstellingen**. U ziet dat de Run As-accounts worden nu als onvolledig weergegeven.

![Run As-accounts zijn onvolledig](../media/move-account/run-as-accounts.png)

Selecteer elke Run As-account. Op de **eigenschappen** weergeeft, schakelt **verwijderen** verwijderen uitvoeren als-account.

> [!NOTE]
> Als u bent niet gemachtigd om te maken of weergeven van de Run As-accounts, ziet u het volgende bericht: `You do not have permissions to create an Azure Run As account (service principal) and grant the Contributor role to the service principal.` Zie voor meer informatie over de vereiste machtigingen voor het configureren van een uitvoeren als-account, [machtigingen nodig voor het configureren van uitvoeren als-accounts](../manage-runas-account.md#permissions).

Nadat de Run As-accounts zijn verwijderd, schakelt u **maken** onder **uitvoeren als-account**. Op de **toevoegen uitvoeren als-account** weergeeft, schakelt **maken** de uitvoeren als-account en service-principal maken. Herhaal de voorgaande stappen met het **klassieke uitvoeren als-account**.

## <a name="enable-solutions"></a>Oplossingen inschakelen

Nadat u opnieuw het Run As-accounts hebt gemaakt, schakelt u de oplossingen die u hebt verwijderd voordat de verplaatsing opnieuw. Om in te schakelen **wijzigingen bijhouden en inventaris** en **updatebeheer**, selecteert u de betreffende functie in uw Automation-account. Kies de Log Analytics-werkruimte die u verplaatst en selecteer **inschakelen**.

![Oplossingen in uw verplaatste Automation-account opnieuw in te schakelen](../media/move-account/reenable-solutions.png)

Computers die toegevoegd met uw oplossingen zijn zijn zichtbaar wanneer u verbinding hebt gemaakt met de bestaande Log Analytics-werkruimte.

Om in te schakelen op de **VM's starten/stoppen** tijdens buiten kantooruren oplossing, moet u om de oplossing opnieuw te implementeren. Onder **gerelateerde Resources**, selecteer **VM's starten/stoppen** > **meer informatie over en inschakelen van de oplossing** > **maken** implementatie te starten.

Op de **oplossing toevoegen** pagina, kies uw Log Analytics-werkruimte en het Automation-account.  

![Menu van de oplossing toevoegen](../media/move-account/add-solution-vm.png)

Zie voor gedetailleerde instructies over het configureren van de oplossing [VM's starten/stoppen buiten kantooruren oplossing in Azure Automation](../automation-solution-vm-management.md).

## <a name="post-move-verification"></a>Na verificatie verplaatsen

Wanneer de verplaatsing voltooid is, controleert u de volgende lijst met taken die moeten worden gecontroleerd:

|Mogelijkheid|Tests|Koppeling voor probleemoplossing|
|---|---|---|
|Runbooks|Een runbook kan uitvoeren en verbinding maken met Azure-resources.|[Problemen met runbooks oplossen](../troubleshoot/runbooks.md)
| Broncodebeheer|U kunt een handmatige synchronisatie uitvoeren op uw opslagplaats van het besturingselement.|[Integratie van bronbeheer](../source-control-integration.md)|
|Wijzigingen bijhouden en inventaris|Controleer of dat u huidige inventarisgegevens van uw machines zien.|[Wijzigingen bijhouden oplossen](../troubleshoot/change-tracking.md)|
|Updatebeheer|Controleer of u uw machines bekijken en ze in orde.</br>Voer een test-software-update-implementatie.|[Problemen met update management oplossen](../troubleshoot/update-management.md)|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het verplaatsen van resources in Azure, [verplaatsen van resources in Azure](../../azure-resource-manager/move-support-resources.md).
