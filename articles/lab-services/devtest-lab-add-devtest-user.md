---
title: Eigenaars en gebruikers toevoegen in Azure DevTest Labs | Microsoft Docs
description: Eigenaars en gebruikers toevoegen in Azure DevTest Labs met behulp van de Azure portal of PowerShell
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: 4f51d9a5-2702-45f0-a2d5-a3635b58c416
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2018
ms.author: spelluru
ms.openlocfilehash: 558df3fa70989aaf9ba182df3a918994c7dc9db6
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51243706"
---
# <a name="add-owners-and-users-in-azure-devtest-labs"></a>Eigenaars en gebruikers toevoegen in Azure DevTest Labs
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/How-to-set-security-in-your-DevTest-Lab/player]
> 
> 

Toegang in Azure DevTest Labs wordt beheerd door [op rollen gebaseerd toegangsbeheer (RBAC)](../role-based-access-control/overview.md). Met RBAC, kunt u taken scheiden binnen uw team in *rollen* waarbij u alleen de mate van toegang voor gebruikers om hun werk te verlenen. Drie van deze RBAC-rollen zijn *eigenaar*, *DevTest Labs-gebruiker*, en *Inzender*. In dit artikel leert u welke acties kunnen worden uitgevoerd in elk van de drie belangrijkste RBAC-rollen. Van daaruit leert u hoe u gebruikers toevoegen aan een lab - zowel via de portal en via een PowerShell-script, en hoe u gebruikers toevoegt op het abonnementsniveau.

## <a name="actions-that-can-be-performed-in-each-role"></a>Acties die kunnen worden uitgevoerd in elke rol
Er zijn drie primaire rollen kunt u een gebruiker toewijzen:

* Eigenaar
* DevTest Labs-gebruiker
* Inzender

De volgende tabel ziet u de acties die kunnen worden uitgevoerd door gebruikers in elk van deze rollen:

| **Acties van gebruikers in deze rol kunnen uitvoeren** | **DevTest Labs-gebruiker** | **Eigenaar** | **Inzender** |
| --- | --- | --- | --- |
| **Taken voor het testlab** | | | |
| Gebruikers toevoegen aan een lab |Nee |Ja |Nee |
| Kosten-instellingen bijwerken |Nee |Ja |Ja |
| **Basis VM-taken** | | | |
| Toevoegen en verwijderen van aangepaste installatiekopieën |Nee |Ja |Ja |
| Toevoegen, bijwerken en verwijderen van formules |Ja |Ja |Ja |
| Geaccepteerde Azure Marketplace-installatiekopieën |Nee |Ja |Ja |
| **VM-taken** | | | |
| Virtuele machines maken |Ja |Ja |Ja |
| Starten, stoppen en verwijderen van virtuele machines |Alleen virtuele machines die zijn gemaakt door de gebruiker |Ja |Ja |
| Beleid voor virtuele machines bijwerken |Nee |Ja |Ja |
| Gegevensschijven van virtuele machines toevoegen/verwijderen |Alleen virtuele machines die zijn gemaakt door de gebruiker |Ja |Ja |
| **Artefact taken** | | | |
| Toevoegen en verwijderen van opslagplaatsen artefact |Nee |Ja |Ja |
| Artefacten toepassen |Ja |Ja |Ja |

> [!NOTE]
> Wanneer een gebruiker een virtuele machine maakt, wordt die gebruiker automatisch toegewezen aan de **eigenaar** rol van de gemaakte virtuele machine.
> 
> 

## <a name="add-an-owner-or-user-at-the-lab-level"></a>Een eigenaar of gebruiker toevoegen op het niveau van het lab
Eigenaars en gebruikers kunnen worden toegevoegd op het niveau van het lab via Azure portal. Een gebruiker kan een externe gebruiker met een geldig zijn [Microsoft-account (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account).
De volgende stappen begeleiden u bij het proces van het toevoegen van een gebruiker of eigenaar aan een lab in Azure DevTest Labs:

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer **alle services**, en selecteer vervolgens **DevTest Labs** in de lijst.
3. Selecteer de gewenste lab in de lijst met labs.
4. Selecteer op de blade van de testomgeving **configuratie en het beleid**. 
5. Op de **configuratie en het beleid** weergeeft, schakelt **toegangsbeheer (IAM)** in het menu aan de linkerkant. 
6. Selecteer **toevoegen** op de werkbalk om een gebruiker toevoegen aan een rol.

    ![Gebruiker toevoegen](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
1. In de **machtigingen toevoegen** venster de volgende acties uitvoeren: 
    1. Selecteer een rol (bijvoorbeeld: DevTest Labs-gebruiker). De sectie [acties die kunnen worden uitgevoerd in elke rol](#actions-that-can-be-performed-in-each-role) geeft een lijst van de verschillende acties die kunnen worden uitgevoerd door gebruikers in de rollen eigenaar, DevTest-gebruiker en Inzender.
    2. Selecteer de gebruiker moet worden toegevoegd aan de rol. 
    3. Selecteer **Opslaan**. 

        ![Gebruiker toevoegen aan de rol](./media/devtest-lab-add-devtest-user/add-user.png) 
11. Als u terugkeert naar de **gebruikers** blade, de gebruiker is toegevoegd.  

## <a name="add-an-external-user-to-a-lab-using-powershell"></a>Een externe gebruiker toevoegen aan een lab met behulp van PowerShell
Naast het toevoegen van gebruikers in Azure portal, kunt u een externe gebruiker toevoegen aan uw testomgeving met een PowerShell-script. Wijzig in het volgende voorbeeld wordt de parameterwaarden onder de **waarden wijzigen** opmerking.
U vindt de `subscriptionId`, `labResourceGroup`, en `labName` waarden uit de labblade in Azure portal.

> [!NOTE]
> Script in het voorbeeld wordt ervan uitgegaan dat de opgegeven gebruiker als gast is toegevoegd aan de Active Directory en mislukt als dit niet het geval is. Als u wilt een gebruiker niet in Active Directory toevoegt aan een lab, gebruikt u de Azure-portal de gebruiker toewijzen aan een rol, zoals wordt geïllustreerd in de sectie [een eigenaar of gebruiker toevoegen op het niveau van het lab](#add-an-owner-or-user-at-the-lab-level).   
> 
> 

    # Add an external user in DevTest Labs user role to a lab
    # Ensure that guest users can be added to the Azure Active directory:
    # https://azure.microsoft.com/documentation/articles/active-directory-create-users/#set-guest-user-access-policies

    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource name here>"
    $labName = "<Enter lab name here>"
    $userDisplayName = "<Enter user's display name here>"

    # Log into your Azure account
    Connect-AzureRmAccount

    # Select the Azure subscription that contains the lab. 
    # This step is optional if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Retrieve the user object
    $adObject = Get-AzureRmADUser -SearchString $userDisplayName

    # Create the role assignment. 
    $labId = ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    New-AzureRmRoleAssignment -ObjectId $adObject.Id -RoleDefinitionName 'DevTest Labs User' -Scope $labId

## <a name="add-an-owner-or-user-at-the-subscription-level"></a>Een eigenaar of gebruiker toevoegen op het abonnementsniveau
Azure-machtigingen worden doorgevoerd van bovenliggend bereik aan het onderliggende bereik in Azure. Eigenaren van een Azure-abonnement met labs worden dus automatisch eigenaars van de labs. Deze tevens eigenaar van de virtuele machines en andere resources die zijn gemaakt door gebruikers van de testomgeving, en de service Azure DevTest Labs. 

U kunt extra eigenaren toevoegen aan een lab via de blade van de testomgeving in de [Azure-portal](https://go.microsoft.com/fwlink/p/?LinkID=525040). Bereik van beheer van de eigenaar van de toegevoegde is echter meer gedetailleerde dan de scope van de eigenaar van het abonnement. Bijvoorbeeld, de toegevoegde eigenaars geen volledige toegang tot een aantal van de resources die in het abonnement zijn gemaakt door de DevTest Labs-service. 

Eigenaar toevoegen aan een Azure-abonnement, de volgende stappen uit:

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer **alle Services**, en selecteer vervolgens **abonnementen** in de lijst.
3. Selecteer het gewenste abonnement.
4. Selecteer **toegang** pictogram. 
   
    ![-Gebruikers](./media/devtest-lab-add-devtest-user/access-users.png)
5. Op de **gebruikers** Selecteer **toevoegen**.
   
    ![Gebruiker toevoegen](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
6. Op de **Selecteer een rol** blade, selecteer **eigenaar**.
7. Op de **gebruikers toevoegen** blade, voer het e-mailadres of naam van de gebruiker die u wilt toevoegen als een eigenaar. Als de gebruiker kan niet worden gevonden, krijgt u een foutbericht weergegeven waarin wordt uitgelegd van het probleem. Als de gebruiker wordt gevonden, dat de gebruiker wordt vermeld onder de **gebruiker** in het tekstvak.
8. Selecteer de naam van de gebruiker bevindt.
9. Selecteer **Selecteer**.
10. Selecteer **OK** sluiten de **toegang toevoegen** blade.
11. Als u terugkeert naar de **gebruikers** blade, de gebruiker is toegevoegd als een eigenaar. Deze gebruiker is nu eigenaar van een labs gemaakt op basis van dit abonnement en is dus eigenaar taken uit te voeren. 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

