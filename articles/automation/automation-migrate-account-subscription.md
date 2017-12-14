---
title: Automation-Account en Resources migreren | Microsoft Docs
description: In dit artikel wordt beschreven hoe een Automation-Account in Azure Automation en de bijbehorende bronnen van een abonnement te verplaatsen naar een andere.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 9c2db4a2-f324-48dc-8ce7-3343bf7230d5
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/29/2017
ms.author: magoedte
ms.openlocfilehash: c13ee767cc2a1fb7880e6d0491cd6a247c737c13
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="migrate-automation-account-and-resources"></a>Automation-Account en resources migreren
Voor het Automation-accounts en de bijbehorende bronnen (dat wil zeggen, assets, runbooks, modules, enzovoort) die u hebt gemaakt in de Azure portal en wilt migreren van een resourcegroep naar een andere of van een abonnement naar een andere, kunt u dit doen eenvoudig met de [verplaatsen van resources](../azure-resource-manager/resource-group-move-resources.md) functie is beschikbaar in de Azure-portal. Echter, voordat u doorgaat met deze actie moet u eerst nagaan de volgende [controlelijst voor het verplaatsen van resources](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) en daarnaast de volgende lijst specifiek zijn voor Automation.   

1. De bestemming/resourcegroep moet zich in dezelfde regio bevinden als de bron.  Wat betekent dat Automation-accounts niet verplaatsen tussen regio's.
2. Bij het verplaatsen van resources (bijvoorbeeld runbooks, taken, enzovoort), worden zowel de bronnengroep als de doelgroep vergrendeld voor de duur van de bewerking. Schrijven en verwijderen van bewerkingen op de groepen die worden geblokkeerd totdat de verplaatsing is voltooid.  
3. De runbooks of variabelen, die verwijst naar een resourcegroep of abonnement-ID uit het bestaande abonnement, moeten worden bijgewerkt nadat de migratie is voltooid.   

> [!NOTE]
> Deze functie ondersteunt geen bewegende klassieke automation-resources.
>
>

## <a name="to-move-the-automation-account-using-the-portal"></a>Verplaatsen van het Automation-Account via de portal
1. Van uw Automation-account, klikt u op **verplaatsen** boven aan de pagina.<br> ![Optie verplaatsen](media/automation-migrate-account-subscription/automation-menu-move.png)<br>
2. Op de **verplaatsen van resources** deelvenster, kennisgeving uit bronnen die betrekking hebben op uw Automation-account en uw groep(en) resource.  Selecteer de **abonnement** en **resourcegroep** uit de vervolgkeuzelijst, of Selecteer de optie **een nieuwe resourcegroep maken** en voert u een nieuwe Resourcegroepnaam in het veld dat is opgegeven.  
3. Bekijk en schakel het selectievakje in om te bevestigen dat u *over hulpprogramma's en scripts moet worden bijgewerkt om de nieuwe resource-id's gebruiken nadat resources zijn verplaatst* en klik vervolgens op **OK**.<br> ![Deelvenster voor Resources verplaatsen](media/automation-migrate-account-subscription/automation-move-resources-blade.png)<br>   

Dit kan enkele minuten duren.  In **meldingen**, krijgt u de status van elke actie die uitgevoerd - validatie, migratie wordt, en klik tot slot wanneer deze is voltooid.     

## <a name="to-move-the-automation-account-using-powershell"></a>Verplaatsen van het Automation-Account met behulp van PowerShell
U kunt bestaande Automation-resources verplaatsen naar een andere resourcegroep of abonnement met de **Get-AzureRmResource** cmdlet ophalen van de specifieke Automation-account en vervolgens **verplaatsen AzureRmResource** cmdlet uit te voeren van de verplaatsing.

Het eerste voorbeeld laat zien hoe een Automation-account verplaatsen naar een nieuwe resourcegroep.

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup"
   ```

Nadat u het bovenstaande voorbeeld uitvoert, wordt u gevraagd om te controleren of dat u wilt dat deze actie uit te voeren.  Nadat u op **Ja** en toestaan dat het script om door te gaan, ontvangt u geen meldingen tijdens het uitvoeren van de migratie.  

Als u wilt verplaatsen naar een nieuw abonnement, een waarde bevatten voor de *DestinationSubscriptionId* parameter.

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup" -DestinationSubscriptionId "SubscriptionId"
   ```

Net als bij het vorige voorbeeld, wordt u gevraagd om te bevestigen dat de verplaatsing.  

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het verplaatsen van resources naar de nieuwe resourcegroep of abonnement [resources verplaatsen naar de nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md)
* Zie [Op rollen gebaseerd toegangsbeheer in Azure Automation](automation-role-based-access-control.md) voor meer informatie over het op rollen gebaseerd toegangsbeheer in Azure Automation.
* Zie voor meer informatie over PowerShell-cmdlets voor het beheren van uw abonnement, [Azure PowerShell gebruiken met Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)
* Zie voor meer informatie over portal functies voor het beheren van uw abonnement, [om resources te beheren met behulp van de Azure-portal](../azure-resource-manager/resource-group-portal.md).
