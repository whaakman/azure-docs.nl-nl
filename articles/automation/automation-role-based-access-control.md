---
title: Op rollen gebaseerd toegangsbeheer in Azure Automation | Microsoft Docs
description: Met op rollen gebaseerd toegangsbeheer (RBAC) beschikt u over toegangsbeheer voor Azure-resources. In dit artikel wordt beschreven hoe u RBAC instelt in Azure Automation.
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
keywords: automatisering rbac, rolgebaseerde toegangscontrole, azure rbac
ms.assetid: 04b5625e-0ee8-4b5b-85cd-7734c1b3d4a3
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/30/2016
ms.author: magoedte;sngun
ms.openlocfilehash: 9a115301e173b3d5cb3a4ac527ed1b01bc9cc421
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="role-based-access-control-in-azure-automation"></a>Op rollen gebaseerd toegangsbeheer in Azure Automation
## <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer
Met op rollen gebaseerd toegangsbeheer (RBAC) beschikt u over toegangsbeheer voor Azure-resources. Met [RBAC](../active-directory/role-based-access-control-configure.md) kunt u taken scheiden binnen uw team en alleen de mate van toegang verlenen aan gebruikers, groepen en toepassingen die nodig is om de taken uit te voeren. Op rollen gebaseerde toegang kan worden verleend aan gebruikers via de Azure-portal, Azure-opdrachtregelprogramma's of Azure Management-API's.

## <a name="rbac-in-automation-accounts"></a>RBAC in Automation-accounts
In Azure Automation wordt toegang verleend door de juiste RBAC-rol toe te wijzen aan gebruikers, groepen en toepassingen bij het Automation-accountbereik. Hieronder vindt u de ingebouwde rollen die worden ondersteund met een Automation-account:  

| **Rol** | **Beschrijving** |
|:--- |:--- |
| Eigenaar |De rol van Eigenaar maakt het mogelijk om alle resources en acties binnen een Automation-account te openen, inclusief toegang tot andere gebruikers, groepen en toepassingen om het Automation-account te beheren. |
| Inzender |De rol van Bijdrager maakt het mogelijk om alles te beheren, behalve de toegangsrechten van andere gebruikers te wijzigen naar een Automation-account. |
| Lezer |Met de rol van Lezer kunt u alle resources in een Automation-account bekijken, maar niets wijzigen. |
| Automation-operator |De rol van Automation-operator geeft u de mogelijkheid om bijkomende taken, zoals starten, stoppen, opschorten, hernemen en plannen van taken uit te voeren. Deze rol is handig als u niet wilt dat uw Automation-accountresources zoals referentieassets en runbooks worden weergegeven of gewijzigd, maar u wel wilt toestaan dat leden van uw organisatie deze runbooks uitvoeren. |
| Beheerder van gebruikerstoegang |De beheerdersrol gebruiker toegang kunt u gebruikerstoegang tot Azure Automation-accounts beheren. |

> [!NOTE]
> U kunt geen toegangsrechten verlenen aan een specifiek runbook of specifieke runbooks, alleen de resources en acties binnen het Automation-account.  
> 
> 

Dit artikel geeft een stapsgewijze beschrijving van het instellen van RBAC in Azure Automation. Maar eerst vindt u hier een beschrijving van de individuele machtigingen die worden verleend aan de Bijdrager, Lezer, Automation-operator en Beheerder van gebruikerstoegang, zodat u dit goed begrijpt voordat u rechten toekent aan het Automation-account.  Anders kan dit leiden tot onverwachte of ongewenste consequenties.     

## <a name="contributor-role-permissions"></a>Machtigingen voor de rol van Bijdrager
De volgende tabel geeft de acties die kunnen worden uitgevoerd door de rol Bijdrager in Automatisering.

| **Resourcetype** | **Lezen** | **Schrijven** | **Verwijderen** | **Andere acties** |
|:--- |:--- |:--- |:--- |:--- |
| Azure Automation Account |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation Certificate Asset |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation Connection Asset |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation Connection Type Asset |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation Credential Asset |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation Schedule Asset |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation Variable Asset |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation Desired State Configuration | | | |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |
| Hybrid Runbook Worker Resource Type |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | |
| Azure Automation Job |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |
| Automation Job Stream |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation Job Schedule |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | |
| Automation Module |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | |
| Azure Automation Runbook |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |
| Automation Runbook Draft |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |
| Automation Runbook Draft Test Job |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |
| Automation Webhook |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |

## <a name="reader-role-permissions"></a>Machtigingen voor de rol van Lezer
De volgende tabel geeft de acties die kunnen worden uitgevoerd door de rol van lezer in Automation.

| **Resourcetype** | **Lezen** | **Schrijven** | **Verwijderen** | **Andere acties** |
|:--- |:--- |:--- |:--- |:--- |
| Klassiek abonnement beheerder |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Vergrendeling voor beheer |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Machtiging |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Provider-bewerkingen |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Nieuwe roltoewijzing |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Roldefinitie ophalen |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |

## <a name="automation-operator-role-permissions"></a>Rolmachtigingen Automation-operator
De volgende tabel geeft de specifieke acties die kunnen worden uitgevoerd door de rol van operator in Automation.

| **Resourcetype** | **Lezen** | **Schrijven** | **Verwijderen** | **Andere acties** |
|:--- |:--- |:--- |:--- |:--- |
| Azure Automation Account |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation Certificate Asset | | | | |
| Automation Connection Asset | | | | |
| Automation Connection Type Asset | | | | |
| Automation Credential Asset | | | | |
| Automation Schedule Asset |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | |
| Automation Variable Asset | | | | |
| Automation Desired State Configuration | | | | |
| Hybrid Runbook Worker Resource Type | | | | |
| Azure Automation Job |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |
| Automation Job Stream |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation Job Schedule |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | |
| Automation Module | | | | |
| Azure Automation Runbook |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation Runbook Draft | | | | |
| Automation Runbook Draft Test Job | | | | |
| Automation Webhook | | | | |

Voor meer details geeft [Acties Automation-operator](../active-directory/role-based-access-built-in-roles.md#automation-operator) de acties ondersteund door de rol van Automation-operator op het Automation-account en zijn resources.

## <a name="user-access-administrator-role-permissions"></a>Machtigingen van de rol Beheerder van de Gebruikerstoegang
De volgende tabel geeft de specifieke acties die kunnen worden uitgevoerd door de rol van operator in Automation.

| **Resourcetype** | **Lezen** | **Schrijven** | **Verwijderen** | **Andere acties** |
|:--- |:--- |:--- |:--- |:--- |
| Azure Automation Account |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation Certificate Asset |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation Connection Asset |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation Connection Type Asset |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation Credential Asset |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation Schedule Asset |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation Variable Asset |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation Desired State Configuration | | | | |
| Hybrid Runbook Worker Resource Type |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Azure Automation Job |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation Job Stream |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation Job Schedule |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation Module |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Azure Automation Runbook |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation Runbook Draft |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation Runbook Draft Test Job |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |
| Automation Webhook |![Groene Status](media/automation-role-based-access-control/green-checkmark.png) | | | |

## <a name="configure-rbac-for-your-automation-account-using-azure-portal"></a>RBAC configureren voor uw Automation-account met Azure-portal
1. Meld u aan bij de [Azure-portal](https://portal.azure.com/) en open uw Automation-account via de pagina Automation-accounts.  
2. Klik op het besturingselement **Toegang** in de rechterbovenhoek. Hiermee wordt de pagina **Gebruikers** geopend, waar u nieuwe gebruikers, groepen en toepassingen kunt toevoegen om uw Automation-account te beheren en bestaande rollen kunt weergeven die kunnen worden geconfigureerd voor het Automation-account.  
   
   ![De knop Toegang](media/automation-role-based-access-control/automation-01-access-button.png)  

> [!NOTE]
> **Abonnementsbeheerders** bestaat al als de standaardgebruiker. De Active Directory-groep Abonnementsbeheerders bevat de servicebeheerder(s) en medebeheerder(s) voor uw Azure-abonnement. De servicebeheerder is de eigenaar van uw Azure-abonnement en de bijhorende resources en neemt ook de eigenaarsrol over voor de Automation-accounts. Dit betekent dat de toegang wordt **overgenomen** voor **servicebeheerders en medebeheerders** van een abonnement en dat deze wordt **toegewezen** voor alle andere gebruikers. Klik op **Abonnementsbeheerders** om meer details over hun machtigingen weer te geven.  
> 
> 

### <a name="add-a-new-user-and-assign-a-role"></a>Een nieuwe gebruiker toevoegen en een rol toewijzen
1. Klik op de pagina Gebruikers op **Toevoegen** om de pagina **Toegang toevoegen** te openen waar u een gebruiker, groep of toepassing kunt toevoegen, en hieraan een rol kunt toewijzen.  
   
   ![Gebruiker toevoegen](media/automation-role-based-access-control/automation-02-add-user.png)  
2. Selecteer een rol in de lijst met beschikbare rollen. We kiezen de rol **Lezer**, maar u kunt elke beschikbare ingebouwde rol kiezen die met een Automation-account wordt ondersteund of elke aangepaste rol die u mogelijk hebt gedefinieerd.  
   
   ![Rol selecteren](media/automation-role-based-access-control/automation-03-select-role.png)  
3. Klik op **Gebruikers toevoegen** om de pagina **Gebruikers toevoegen** te openen. Als u gebruikers, groepen of toepassingen hebt toegevoegd om uw abonnement te beheren, dan worden die gebruikers weergegeven en kunt u deze selecteren om toegang toe te voegen. Als er geen gebruikers worden weergegeven of als de gebruiker die u wilt toevoegen, niet wordt weergegeven, klikt u op **Uitnodigen** om de pagina **Een gast uitnodigen** te openen, waar u een gebruiker met een geldig e-mailadres voor een Microsoft-account, zoals Outlook.com, OneDrive of Xbox Live-id, kunt uitnodigen. Nadat u het e-mailadres van de gebruiker hebt ingevoerd, klikt u op **Selecteren** om de gebruiker toe te voegen en klikt u vervolgens op **OK**. 
   
   ![Gebruikers toevoegen](media/automation-role-based-access-control/automation-04-add-users.png)  
   
   Nu moet u zien dat de gebruiker is toegevoegd aan de pagina **Gebruikers** en dat de rol **Lezer** is toegewezen.  
   
   ![Gebruikers weergeven](media/automation-role-based-access-control/automation-05-list-users.png)  
   
   U kunt ook een rol aan de gebruiker toewijzen via de pagina **Rollen**. 
4. Klik op **Rollen** op de pagina Gebruikers om de pagina **Rollen** te openen. Via deze pagina kunt u het volgende bekijken: de naam van de rol, het aantal gebruikers en de groepen die aan die rol zijn toegewezen.
   
    ![Rol toewijzen vanaf pagina Gebruikers](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)  
   
   > [!NOTE]
   > Op rollen gebaseerd toegangsbeheer kan alleen worden ingesteld op het niveau van het Automation-account en niet op een resource onder het Automation-account.
   > 
   > 
   
    U kunt meer dan één rol toewijzen aan een gebruiker, groep of toepassing. Als we bijvoorbeeld de rol **Automation-operator** samen met de rol **Lezer** toevoegen aan de gebruiker, dan kan deze alle Automation-resources bekijken, en ook de runbooktaken uitvoeren. U kunt de vervolgkeuzelijst uitvouwen om een lijst met rollen weer te geven die aan de gebruiker zijn toegewezen.  
   
    ![Meerdere rollen weergeven](media/automation-role-based-access-control/automation-07-view-multiple-roles.png)  

### <a name="remove-a-user"></a>Een gebruiker verwijderen
U kunt de toegangsmachtigingen voor een gebruiker verwijderen die het Automation-account niet beheert, of die niet meer voor de organisatie werkt. Hieronder vindt u de stappen voor het verwijderen van een gebruiker: 

1. Selecteer op de pagina **Gebruikers** de roltoewijzing die u wilt verwijderen.
2. Klik op de pagina met toewijzingsdetails op de knop **Verwijderen**.
3. Klik op **Ja** om het verwijderen te bevestigen. 
   
   ![Gebruikers verwijderen](media/automation-role-based-access-control/automation-08-remove-users.png)  

## <a name="role-assigned-user"></a>Gebruiker met toegewezen rol
Wanneer een gebruiker die aan een rol is toegewezen, zich aanmeldt bij zijn/haar Automation-account, ziet deze het account van de eigenaar weergegeven in de lijst met **Standaardmappen**. Als een gebruiker het Automation-account wil bekijken waaraan hij/zij is toegevoegd, moet deze overschakelen van de standaardmap naar de standaardmap van de eigenaar.  

![Standaardmap](media/automation-role-based-access-control/automation-09-default-directory-in-role-assigned-user.png)  

### <a name="user-experience-for-automation-operator-role"></a>Gebruikerservaring voor de rol Automation-operator
Wanneer een gebruiker, aan wie de rol Automation-operator is toegewezen, het Automation-account bekijkt waaraan hij/zij is toegewezen, kan de gebruiker alleen de lijst met runbooks, runbooktaken en planningen weergeven die in het Automation-account zijn gemaakt, maar niet de bijbehorende definitie. De gebruiker kan de runbooktaak starten, stoppen, onderbreken, hervatten of plannen. De gebruiker heeft geen toegang tot andere Automation-resources, zoals configuraties, Hybrid Worker-groepen of DSC-knooppunten.  

![Geen toegang tot resources](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)  

Wanneer de gebruiker op het runbook klikt, worden de opdrachten voor het weergeven van de bron of het bewerken van het runbook niet aangeboden, omdat de rol Automation-operator geen toegang tot de opdrachten toestaat.  

![Geen toegang om runbook te bewerken](media/automation-role-based-access-control/automation-11-no-access-to-edit-runbook.png)  

De gebruiker heeft toegang voor het weergeven en maken van planningen, maar heeft geen toegang tot andere assettypen.  

![Geen toegang tot assets](media/automation-role-based-access-control/automation-12-no-access-to-assets.png)  

Deze gebruiker heeft ook geen toegang tot de weergave van de webhooks die aan een runbook zijn gekoppeld.

![Geen toegang tot webhooks](media/automation-role-based-access-control/automation-13-no-access-to-webhooks.png)  

## <a name="configure-rbac-for-your-automation-account-using-azure-powershell"></a>RBAC configureren voor uw Automation-account met Azure PowerShell
Op rollen gebaseerde toegang kan ook worden geconfigureerd voor een Automation-Account met de volgende [Azure PowerShell-cmdlets](../active-directory/role-based-access-control-manage-access-powershell.md).

• Met [Get-AzureRmRoleDefinition](https://msdn.microsoft.com/library/mt603792.aspx) wordt een lijst weergegeven van alle RBAC-rollen die beschikbaar zijn in Azure Active Directory. U kunt deze opdracht samen met de eigenschap **Naam** gebruiken om alle acties die kunnen worden uitgevoerd door een specifieke rol weer te geven.  
    **Voorbeeld:**  
    ![Roldefinitie ophalen](media/automation-role-based-access-control/automation-14-get-azurerm-role-definition.png)  

• Met [Get-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt619413.aspx) worden Azure AD RBAC-roltoewijzingen weergegeven bij het opgegeven bereik. Zonder parameters worden met deze opdracht alle roltoewijzingen geretourneerd die onder het abonnement zijn gemaakt. Gebruik de parameter **ExpandPrincipalGroups** om toegangstoewijzingen voor de opgegeven gebruiker weer te geven en voor de groepen waarvan de gebruiker lid is.  
    **Voorbeeld:** gebruik de volgende opdracht om alle gebruikers en de bijbehorende rollen binnen een Automation-account weer te geven.

    Get-AzureRMRoleAssignment -scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>” 

![Roltoewijzing ophalen](media/automation-role-based-access-control/automation-15-get-azurerm-role-assignment.png)

• [New AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603580.aspx) om toegang te verlenen aan gebruikers, groepen en toepassingen bij een bepaald bereik.  
    **Voorbeeld:** gebruik de volgende opdracht om een nieuwe rol 'Automation-operator' toe te wijzen voor een gebruiker bij het Automation-accountbereik.

    New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName "Automation operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”  

![Nieuwe roltoewijzing](media/automation-role-based-access-control/automation-16-new-azurerm-role-assignment.png)

• Gebruik [Remove-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603781.aspx) om toegang van de opgegeven gebruiker, groep of toepassing te verwijderen uit een bepaald bereik.  
    **Voorbeeld:** gebruik de volgende opdracht om een de gebruiker te verwijderen uit de rol van “Automation Operator” in het bereik Automation Account.

    Remove-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to remove> -RoleDefinitionName "Automation Operator" -Scope “/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation Account Name>”

Vervang in de bovenstaande voorbeelden de **aanmeldnaam**, de **abonnements-id**, de **resourcegroepnaam** en de **Automation-accountnaam** door uw accountgegevens. Kies **Ja** om te bevestigen voordat u verdergaat met het verwijderen van de roltoewijzing voor de gebruiker.   

## <a name="next-steps"></a>Volgende stappen
* Raadpleeg [RBAC met Azure PowerShell beheren](../active-directory/role-based-access-control-manage-access-powershell.md) voor informatie over verschillende manieren om RBAC voor Azure Automation te configureren.
* Zie [Een runbook starten](automation-starting-a-runbook.md) voor informatie over verschillende manieren om een runbook te starten.
* Raadpleeg [Azure Automation-runbooktypen](automation-runbook-types.md) voor informatie over verschillende typen runbooks

