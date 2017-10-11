---
title: Eigenaars- en gebruikers toevoegen in Azure DevTest Labs | Microsoft Docs
description: Eigenaars- en gebruikers toevoegen in Azure DevTest Labs met de Azure-portal of PowerShell
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 4f51d9a5-2702-45f0-a2d5-a3635b58c416
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: tarcher
ms.openlocfilehash: d67fa257574d6cb4ad4b18521900374fb51da290
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="add-owners-and-users-in-azure-devtest-labs"></a>Eigenaars- en gebruikers toevoegen in Azure DevTest Labs
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/How-to-set-security-in-your-DevTest-Lab/player]
> 
> 

Toegang in Azure DevTest Labs wordt beheerd door [gebaseerd toegangsbeheer (RBAC)](../active-directory/role-based-access-control-what-is.md). Met RBAC kunt kunt u taken scheiden binnen uw team in *rollen* waar u de hoeveelheid toegang nodig is voor gebruikers voor het uitvoeren van hun taken verlenen. Drie van deze RBAC-rollen zijn *eigenaar*, *DevTest Labs gebruiker*, en *Inzender*. In dit artikel leert u welke acties kunnen worden uitgevoerd in elk van de drie belangrijkste RBAC-rollen. Van daaruit u meer informatie over hoe u gebruikers toevoegen aan een lab - zowel via de portal en via een PowerShell-script en gebruikers toevoegen op het abonnementsniveau.

## <a name="actions-that-can-be-performed-in-each-role"></a>Acties die kunnen worden uitgevoerd in elke rol
Er zijn drie belangrijkste rollen kunt u een gebruiker toewijzen:

* Eigenaar
* DevTest Labs gebruiker
* Inzender

De volgende tabel ziet u de acties die kunnen worden uitgevoerd door gebruikers in elk van deze rollen:

| **Gebruikers met deze rol acties kunnen uitvoeren** | **DevTest Labs gebruiker** | **Eigenaar** | **Inzender** |
| --- | --- | --- | --- |
| **Taken voor het testlab** | | | |
| Gebruikers toevoegen aan een lab |Nee |Ja |Nee |
| Update-instellingen |Nee |Ja |Ja |
| **Basis VM-taken** | | | |
| Toevoegen en verwijderen van aangepaste installatiekopieën |Nee |Ja |Ja |
| Toevoegen, bijwerken en verwijderen van formules |Ja |Ja |Ja |
| Geaccepteerde Azure Marketplace-installatiekopieën |Nee |Ja |Ja |
| **VM-taken** | | | |
| Virtuele machines maken |Ja |Ja |Ja |
| Starten, stoppen en verwijderen van virtuele machines |Alleen virtuele machines die zijn gemaakt door de gebruiker |Ja |Ja |
| Beleidsregels voor virtuele machine bijwerken |Nee |Ja |Ja |
| Gegevensschijven van VM's toevoegen of verwijderen |Alleen virtuele machines die zijn gemaakt door de gebruiker |Ja |Ja |
| **Taken van artefacten** | | | |
| Toevoegen en verwijderen van artefacten opslagplaatsen |Nee |Ja |Ja |
| Toepassen van artefacten |Ja |Ja |Ja |

> [!NOTE]
> Wanneer een gebruiker een virtuele machine maakt, wordt die gebruiker automatisch toegewezen aan de **eigenaar** rol van de gemaakte virtuele machine.
> 
> 

## <a name="add-an-owner-or-user-at-the-lab-level"></a>Toevoegen van een eigenaar of gebruiker op het niveau van het lab
Eigenaars- en gebruikers kunnen worden toegevoegd op het niveau van het lab via de Azure-portal. Dit omvat externe gebruikers met een geldig [Microsoft-account (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account).
De volgende stappen begeleiden u bij het proces van een eigenaar of gebruiker toevoegen aan een lab in Azure DevTest Labs:

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer **Meer services** en selecteer in de lijst vervolgens **DevTest Labs**.
3. Selecteer de gewenste testomgeving uit de lijst van labs.
4. Selecteer op de labblade **configuratie**. 
5. Op de **configuratie** blade Selecteer **gebruikers**.
6. Op de **gebruikers** blade Selecteer **+ toevoegen**.
   
    ![Gebruiker toevoegen](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
7. Op de **Selecteer een rol** blade, selecteer de gewenste rol. De sectie [acties die kunnen worden uitgevoerd in elke rol](#actions-that-can-be-performed-in-each-role) staan de verschillende acties die kunnen worden uitgevoerd door gebruikers met de eigenaar, DevTest gebruiker en Inzender rollen.
8. Op de **gebruikers toevoegen** blade, voer het e-mailadres of de naam van de gebruiker die u wilt toevoegen in de rol die u hebt opgegeven. Als de gebruiker kan niet worden gevonden, wordt het probleem uitgelegd in een foutbericht weergegeven. Als de gebruiker wordt gevonden, wordt die gebruiker weergegeven en geselecteerd. 
9. Selecteer **Selecteer**.
10. Selecteer **OK** sluiten de **toegang toevoegen** blade.
11. Als u terugkeert naar de **gebruikers** blade de gebruiker is toegevoegd.  

## <a name="add-an-external-user-to-a-lab-using-powershell"></a>Een externe gebruiker toevoegen aan een testomgeving met behulp van PowerShell
Naast het toevoegen van gebruikers in de Azure portal, kunt u een externe gebruiker toevoegen aan uw testomgeving met een PowerShell-script. In het volgende voorbeeld wijzigt u gewoon de parameterwaarden onder de **waarden wijzigen** opmerking.
U kunt ophalen de `subscriptionId`, `labResourceGroup`, en `labName` waarden uit de labblade in de Azure portal.

> [!NOTE]
> Het voorbeeldscript wordt ervan uitgegaan dat de opgegeven gebruiker is als Gast toegevoegd aan de Active Directory en mislukt als dit niet het geval is. Als u wilt een gebruiker niet in de Active Directory toevoegt aan een lab, kunt u de Azure portal gebruiken voor de gebruiker toewijzen aan een rol, zoals geïllustreerd in de sectie [een eigenaar of gebruiker toevoegen op het niveau van het lab](#add-an-owner-or-user-at-the-lab-level).   
> 
> 

    # Add an external user in DevTest Labs user role to a lab
    # Ensure that guest users can be added to the Azure Active directory:
    # https://azure.microsoft.com/en-us/documentation/articles/active-directory-create-users/#set-guest-user-access-policies

    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource name here>"
    $labName = "<Enter lab name here>"
    $userDisplayName = "<Enter user's display name here>"

    # Log into your Azure account
    Login-AzureRmAccount

    # Select the Azure subscription that contains the lab. 
    # This step is optional if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Retrieve the user object
    $adObject = Get-AzureRmADUser -SearchString $userDisplayName

    # Create the role assignment. 
    $labId = ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    New-AzureRmRoleAssignment -ObjectId $adObject.Id -RoleDefinitionName 'DevTest Labs User' -Scope $labId

## <a name="add-an-owner-or-user-at-the-subscription-level"></a>Toevoegen van een eigenaar of gebruiker op het abonnementsniveau
Azure machtigingen zijn doorgegeven van bovenliggende bereik aan het onderliggende bereik in Azure. Eigenaren van een Azure-abonnement met labs worden dus automatisch eigenaren van deze labs. Ze ook eigenaar van de virtuele machines en andere resources die zijn gemaakt door gebruikers van de testomgeving en de service Azure DevTest Labs. 

U kunt aanvullende eigenaars toevoegen aan een lab via de labblade in de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). De toegevoegde eigenaar bereik van beheer is echter beperken dan bereik van de eigenaar van het abonnement. Bijvoorbeeld, de eigenaren van de toegevoegde geen volledige toegang tot sommige van de resources die door de service DevTest Labs in het abonnement worden gemaakt. 

Als u wilt een eigenaar toevoegen aan een Azure-abonnement, de volgende stappen uit:

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer **meer Services**, en selecteer vervolgens **abonnementen** uit de lijst.
3. Selecteer het gewenste abonnement.
4. Selecteer **toegang** pictogram. 
   
    ![-Gebruikers](./media/devtest-lab-add-devtest-user/access-users.png)
5. Op de **gebruikers** blade Selecteer **toevoegen**.
   
    ![Gebruiker toevoegen](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
6. Op de **Selecteer een rol** blade Selecteer **eigenaar**.
7. Op de **gebruikers toevoegen** blade, voer het e-mailadres of de naam van de gebruiker die u wilt toevoegen als een eigenaar. Als de gebruiker kan niet worden gevonden, krijgt u een foutbericht weergegeven waarin wordt uitgelegd van het probleem. Als de gebruiker wordt gevonden, wordt die gebruiker vermeld in de **gebruiker** in het tekstvak.
8. Selecteer de gebruikersnaam bevindt.
9. Selecteer **Selecteer**.
10. Selecteer **OK** sluiten de **toegang toevoegen** blade.
11. Als u terugkeert naar de **gebruikers** blade de gebruiker is toegevoegd als een eigenaar. Deze gebruiker is nu eigenaar van een labs gemaakt onder dit abonnement, en dus mogelijk eigenaar taken uit te voeren. 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

