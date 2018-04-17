---
title: Op rollen gebaseerd toegangsbeheer in Azure Automation
description: Met op rollen gebaseerd toegangsbeheer (RBAC) beschikt u over toegangsbeheer voor Azure-resources. In dit artikel wordt beschreven hoe u RBAC instelt in Azure Automation.
keywords: automatisering rbac, rolgebaseerde toegangscontrole, azure rbac
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: c9cdebd2fb7f650b042fa04f345ac440e0b41cb8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="role-based-access-control-in-azure-automation"></a>Op rollen gebaseerd toegangsbeheer in Azure Automation

Met op rollen gebaseerd toegangsbeheer (RBAC) beschikt u over toegangsbeheer voor Azure-resources. Met behulp van [RBAC](../role-based-access-control/role-assignments-portal.md), kunt u taken scheiden binnen uw team en de hoeveelheid toegang verlenen aan gebruikers, groepen en toepassingen die ze nodig hebben om hun taken uitvoeren. Op rollen gebaseerde toegang kan worden verleend aan gebruikers via de Azure-portal, Azure-opdrachtregelprogramma's of Azure Management-API's.

## <a name="roles-in-automation-accounts"></a>Rollen in Automation-accounts
In Azure Automation wordt toegang verleend door de juiste RBAC-rol toe te wijzen aan gebruikers, groepen en toepassingen bij het Automation-accountbereik. Hieronder vindt u de ingebouwde rollen die worden ondersteund met een Automation-account:

| **Rol** | **Beschrijving** |
|:--- |:--- |
| Eigenaar |De rol eigenaar geeft toegang tot alle bronnen en acties binnen een Automation-account met inbegrip van toegang verlenen aan andere gebruikers, groepen en toepassingen voor het beheren van het Automation-account. |
| Inzender |De rol van Bijdrager maakt het mogelijk om alles te beheren, behalve de toegangsrechten van andere gebruikers te wijzigen naar een Automation-account. |
| Lezer |Met de rol van Lezer kunt u alle resources in een Automation-account bekijken, maar niets wijzigen. |
| Automation-operator |De rol van Automation-operator geeft u de mogelijkheid om bijkomende taken, zoals starten, stoppen, opschorten, hernemen en plannen van taken uit te voeren. Deze rol is handig als u niet wilt dat uw Automation-accountresources zoals referentieassets en runbooks worden weergegeven of gewijzigd, maar u wel wilt toestaan dat leden van uw organisatie deze runbooks uitvoeren. |
|Operator voor Automation-taak|De rol Automation-taak Operator kunt u maken en beheren van taken met Automation-runbooks.|
|Operator voor Automation-runbook|De rol Automation-Runbook-Operator kunt u eigenschappen van runbook lezen. Bent u ook de taken van het runbook te maken.|
| Inzender van Log Analytics | De rol Inzender van Log Analytics kunt u alle bewakingsgegevens lezen en bewerken van controle-instellingen. Controle-instellingen bewerken bevat de VM-extensie toevoegen aan virtuele machines, toegangscodes voor opslag om te kunnen verzamelen van Logboeken van de Azure-opslag configureren lezen maken en configureren Automation-accounts, oplossingen toevoegen en configureren van Azure diagnostics op alle Azure-resources.|
| Lezer van Log Analytics | De rol logboekweergave Analytics kunt u weergeven en doorzoeken alle bewaking gegevens, evenals een weergave controle-instellingen. Dit omvat de configuratie van Azure diagnostics weergeven op alle Azure-resources. |
| Controlebijdrager | De rol Inzender bewaking kunt u alle controlegegevens en update controle-instellingen lezen.|
| Controlelezer | De rol Lezer Montioring kunt u alle bewakingsgegevens lezen. |
| Beheerder van gebruikerstoegang |De beheerdersrol gebruiker toegang kunt u gebruikerstoegang tot Azure Automation-accounts beheren. |

## <a name="role-permissions"></a>Rolmachtigingen

De volgende tabellen beschrijven de specifieke machtigingen toegekend aan elke rol. Dit kunnen bijvoorbeeld acties, waardoor machtigingen en NotActions die ze beperken.

### <a name="owner"></a>Eigenaar

Een eigenaar kan alles beheren, inclusief toegang. De volgende tabel ziet u de machtigingen voor de rol:

|Acties|Beschrijving|
|---|---|
|Microsoft.Automation/automationAccounts/|Maken en beheren van bronnen van alle typen.|

### <a name="contributor"></a>Inzender

Een medewerker kan alles beheren behalve toegang. De volgende tabel ziet u de machtigingen verleend en geweigerd voor de rol:

|**Acties**  |**Beschrijving**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/|Maken en beheren van bronnen van alle typen|
|**Niet-acties**||
|Microsoft.Authorization/*/Delete| Rollen en roltoewijzingen verwijderen.       |
|Microsoft.Authorization/*/Write     |  Rollen en roltoewijzingen maken.       |
|Microsoft.Authorization/elevateAccess/Action    | Hiermee stelt u de mogelijkheid te maken van de beheerder van een gebruiker toegang geweigerd.       |

### <a name="reader"></a>Lezer

Een lezer zien alle resources in een Automation-account, maar geen wijzigingen kunt aanbrengen.

|**Acties**  |**Beschrijving**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/read|Alle resources in een Automation-account weergeven. |

### <a name="automation-job-operator"></a>Operator voor Automation-taak

Een Automation-Operator taak krijgt bij het Automation-accountbereik. Hierdoor kan de operatormachtigingen voor het beheren van taken in het account.

|**Acties**  |**Beschrijving**  |
|---------|---------|
|Microsoft.Authorization/*/read|Autorisatie worden gelezen.|
|Microsoft.Automation/automationAccounts/jobs/read|Lijst met taken van het runbook.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|Hervatten van een taak die is onderbroken.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|Annuleren van een taak wordt uitgevoerd.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|Lees de taak stromen en de uitvoer.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|Onderbreken van een taak wordt uitgevoerd.|
|Microsoft.Automation/automationAccounts/jobs/write|Taken maken.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |  Lezen van rollen en roltoewijzingen.       |
|Microsoft.Resources/deployments/*      |Maken en beheren van resourcegroepimplementaties.         |
|Microsoft.Insights/alertRules/*      | Maken en beheren van regels voor waarschuwingen.        |
|Microsoft.Support/* |Maak en ondersteuningstickets beheren.|

### <a name="automation-runbook-operator"></a>Operator voor Automation-runbook

Een rol Automation-Runbook-Operator wordt verleend op het bereik van het Runbook. Een Automation-Runbook-Operator, ziet de runbooknaam. Deze machtiging in combinatie met 'Automation-taak-Operator' bij het Automation-accountbereik kan de operator de Automation-Operator-acties uitvoeren voor een bepaald runbook. De volgende tabel ziet u de machtigingen voor de rol:

|**Acties**  |**Beschrijving**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/runbooks/read     | Lijst van de runbooks.        |
|Microsoft.Authorization/*/read      | Autorisatie worden gelezen.        |
|Microsoft.Resources/subscriptions/resourceGroups/read      |Lezen van rollen en roltoewijzingen.         |
|Microsoft.Resources/deployments/*      | Maken en beheren van resourcegroepimplementaties.         |
|Microsoft.Insights/alertRules/*      | Maken en beheren van regels voor waarschuwingen.        |
|Microsoft.Support/*      | Maak en ondersteuningstickets beheren.        |

### <a name="automation-operator"></a>Automation-operator

Er is een Automation-Operator kunnen starten, stoppen, onderbreken en hervatten van taken. De volgende tabel ziet u de machtigingen voor de rol:

|**Acties**  |**Beschrijving**  |
|---------|---------|
|Microsoft.Authorization/*/read|Autorisatie worden gelezen.|
|Microsoft.Automation/automationAccounts/jobs/read|Lijst met taken van het runbook.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|Hervatten van een taak die is onderbroken.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|Annuleren van een taak wordt uitgevoerd.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|Lees de taak stromen en de uitvoer.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|Onderbreken van een taak wordt uitgevoerd.|
|Microsoft.Automation/automationAccounts/jobs/write|Taken maken.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |Lezen van rollen en roltoewijzingen.         |
|Microsoft.Resources/deployments/*      |Maken en beheren van resourcegroepimplementaties.         |
|Microsoft.Insights/alertRules/*      | Maken en beheren van regels voor waarschuwingen.        |
|Microsoft.Support/* |Maak en ondersteuningstickets beheren.|

### <a name="log-analytics-contributor"></a>Inzender van Log Analytics

Log Analytics medewerker kunnen alle bewakingsgegevens lezen en bewerken van controle-instellingen. Controle-instellingen bewerken bevat de VM-extensie toevoegen aan virtuele machines; lezen van toegangscodes voor opslag om te kunnen verzamelen van Logboeken van Azure Storage; configureren maken en configureren van Automation-accounts; toevoegen van oplossingen; en het configureren van Azure diagnostics op alle Azure-resources. De volgende tabel ziet u de machtigingen voor de rol:

|**Acties**  |**Beschrijving**  |
|---------|---------|
|* / lezen|Bronnen van alle typen, met uitzondering van geheimen lezen.|
|Microsoft.Automation/automationAccounts/*|Automation-accounts beheren.|
|Microsoft.ClassicCompute/virtualMachines/extensions/*|Maken en beheren van uitbreidingen van de virtuele machine.|
|Microsoft.ClassicStorage/storageAccounts/listKeys/action|Lijst met klassieke opslagaccountsleutels.|
|Microsoft.Compute/virtualMachines/extensions/*|Maken en beheren van uitbreidingen voor klassieke virtuele machine.|
|Microsoft.Insights/alertRules/*|Waarschuwingsregels voor lezen, schrijven, verwijderen.|
|Microsoft.Insights/diagnosticSettings/*|Diagnostische instellingen voor lezen, schrijven, verwijderen.|
|Microsoft.OperationalInsights/*|Log Analytics beheren.|
|Microsoft.OperationsManagement/*|Oplossingen in werkruimten beheren.|
|Microsoft.Resources/deployments/*|Maken en beheren van resourcegroepimplementaties.|
|Microsoft.Resources/subscriptions/resourcegroups/deployments/*|Maken en beheren van resourcegroepimplementaties.|
|Microsoft.Storage/storageAccounts/listKeys/action|Toegangscodes voor opslag weergeven.|
|Microsoft.Support/*|Maak en ondersteuningstickets beheren.|


### <a name="log-analytics-reader"></a>Lezer van Log Analytics

Een Log Analytics-lezer kunt weergeven en zoeken van alle bewakingsgegevens en controle-instellingen, met inbegrip van de configuratie van Azure diagnostics weergeven op alle Azure-resources weergeven. De volgende tabel ziet u de machtigingen verleend of geweigerd voor de rol:

|**Acties**  |**Beschrijving**  |
|---------|---------|
|* / lezen|Bronnen van alle typen, met uitzondering van geheimen lezen.|
|Microsoft.OperationalInsights/workspaces/analytics/query/action|Query's in logboekanalyse beheren.|
|Microsoft.OperationalInsights/workspaces/search/action|Log Analytics gegevens zoeken.|
|Microsoft.Support/*|Maak en ondersteuningstickets beheren.|
|**Niet-acties**| |
|Microsoft.OperationalInsights/workspaces/sharedKeys/read|Kan niet lezen van de sleutels voor gedeelde toegang.|

### <a name="monitoring-contributor"></a>Controlebijdrager

Een inzender bewaking kan alle bewakingsgegevens lezen en bijwerken van controle-instellingen. De volgende tabel ziet u de machtigingen voor de rol:

|**Acties**  |**Beschrijving**  |
|---------|---------|
|* / lezen|Bronnen van alle typen, met uitzondering van geheimen lezen.|
|Microsoft.AlertsManagement/alerts/*|Waarschuwingen beheren.|
|Microsoft.AlertsManagement/alertsSummary/*|De waarschuwing dashboard beheren.|
|Microsoft.Insights/AlertRules/*|Waarschuwingsregels beheren.|
|Microsoft.Insights/components/*|Application Insights-onderdelen beheren.|
|Microsoft.Insights/DiagnosticSettings/*|Diagnostische instellingen beheren.|
|Microsoft.Insights/eventtypes/*|Activiteitenlogboek gebeurtenissen (management gebeurtenissen) in een abonnement weergeven. Deze machtiging is van toepassing op de portal zowel programmatische toegang tot het activiteitenlogboek.|
|Microsoft.Insights/LogDefinitions/*|Deze machtiging is vereist voor gebruikers die toegang nodig tot activiteitenlogboeken via de portal. Lijst met logboekbestanden categorieën in het activiteitenlogboek.|
|Microsoft.Insights/MetricDefinitions/*|Metrische definities (lijst met beschikbare metrische gegevens typen voor een resource) lezen.|
|Microsoft.Insights/Metrics/*|Metrische gegevens voor een resource lezen.|
|Microsoft.Insights/Register/Action|Registreer de provider Microsoft.Insights.|
|Microsoft.Insights/webtests/*|Application Insights-webtests beheren.|
|Microsoft.OperationalInsights/workspaces/intelligencepacks/*|Log Analytics-oplossing packs beheren.|
|Microsoft.OperationalInsights/workspaces/savedSearches/*|Log Analytics opgeslagen zoekopdrachten beheren.|
|Microsoft.OperationalInsights/workspaces/search/action|Log Analytics-werkruimten zoeken.|
|Microsoft.OperationalInsights/workspaces/sharedKeys/action|Lijst met sleutels voor een werkruimte voor logboekanalyse.|
|Microsoft.OperationalInsights/workspaces/storageinsightconfigs/*|Log Analytics inzicht opslagconfiguraties beheren.|
|Microsoft.Support/*|Maak en ondersteuningstickets beheren.|
|Microsoft.WorkloadMonitor/workloads/*|Werkbelasting moet worden beheerd.|

### <a name="monitoring-reader"></a>Controlelezer

Alle bewaking gegevens kunnen worden gelezen door een lezer bewaking. De volgende tabel ziet u de machtigingen voor de rol:

|**Acties**  |**Beschrijving**  |
|---------|---------|
|* / lezen|Bronnen van alle typen, met uitzondering van geheimen lezen.|
|Microsoft.OperationalInsights/workspaces/search/action|Log Analytics-werkruimten zoeken.|
|Microsoft.Support/*|Maken en ondersteuningstickets beheren|

### <a name="user-access-administrator"></a>Beheerder van gebruikerstoegang

Een beheerder voor gebruikerstoegang kunt gebruikerstoegang tot Azure-resources te beheren. De volgende tabel ziet u de machtigingen voor de rol:

|**Acties**  |**Beschrijving**  |
|---------|---------|
|* / lezen|Alle bronnen lezen|
|Microsoft.Authorization/*|Machtigingen beheren|
|Microsoft.Support/*|Maken en ondersteuningstickets beheren|

## <a name="onboarding"></a>Onboarding

De volgende tabellen tonen de minimaal vereiste machtigingen die nodig zijn voor onboarding virtuele machines voor het bijhouden van wijzigingen of beheeroplossingen bijwerken.

### <a name="onboarding-from-a-virtual-machine"></a>Voorbereiding van een virtuele machine

|**Actie**  |**Machtiging**  |**Minimale bereik**  |
|---------|---------|---------|
|Schrijven van nieuwe implementatie      | Microsoft.Resources/deployments/*          |Abonnement          |
|Schrijven van nieuwe resourcegroep      | Microsoft.Resources/subscriptions/resourceGroups/write        | Abonnement          |
|Nieuwe standaard werkruimte maken      | Microsoft.OperationalInsights/workspaces/write         | Resourcegroep         |
|Nieuw Account maken      |  Microsoft.Automation/automationAccounts/write        |Resourcegroep         |
|Koppeling werkruimte en -account      |Microsoft.OperationalInsights/workspaces/write</br>Microsoft.Automation/automationAccounts/read|Werkruimte</br>Automation-account
|Oplossing maken      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write |Resourcegroep          |
|De extensie MMA maken      | Microsoft.Compute/virtualMachines/write         | Virtuele machine         |
|Maken van opgeslagen zoekopdracht      | Microsoft.OperationalInsights/workspaces/write          | Werkruimte         |
|Configuratie van de scope maken      | Microsoft.OperationalInsights/workspaces/write          | Werkruimte         |
|Koppeling bereik config-oplossing      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write         | Oplossing         |
|Voorbereiden op status controleren - werkruimte lezen      | Microsoft.OperationalInsights/workspaces/read         | Werkruimte         |
|Controle van de installatiestatus onboarding - lezen eigenschap van de werkruimte van account gekoppeld     | Microsoft.Automation/automationAccounts/read      | Automation-account        |
|Voorbereiden op status controleren - oplossing lezen      | Microsoft.OperationalInsights/workspaces/intelligencepacks/read          | Oplossing         |
|Onboarding status check - virtuele machine lezen      | Microsoft.Compute/virtualMachines/read         | Virtuele machine         |
|Voorbereiden op status controleren - account lezen      | Microsoft.Automation/automationAccounts/read  |  Automation-account   |

### <a name="onboarding-from-automation-account"></a>Onboarding van Automation-account

|**Actie**  |**Machtiging** |**Minimale bereik**  |
|---------|---------|---------|
|Maak nieuwe implementatie     | Microsoft.Resources/deployments/*        | Abonnement         |
|Nieuwe resourcegroep maken     | Microsoft.Resources/subscriptions/resourceGroups/write         | Abonnement        |
|Blade AutomationOnboarding - nieuwe werkruimte maken     |Microsoft.OperationalInsights/workspaces/write           | Resourcegroep        |
|AutomationOnboarding blade - gekoppelde werkruimte lezen     | Microsoft.Automation/automationAccounts/read        | Automation-account       |
|AutomationOnboarding blade - oplossing     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read         | Oplossing        |
|AutomationOnboarding blade - werkruimte lezen     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read        | Werkruimte        |
|Koppeling voor de werkruimte en Account maken     | Microsoft.OperationalInsights/workspaces/write        | Werkruimte        |
|Account voor schoenendoos schrijven      | Microsoft.Automation/automationAccounts/write        | Account        |
|Oplossing maken      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write        | Resourcegroep         |
|Maken en bewerken opgeslagen zoekopdracht     | Microsoft.OperationalInsights/workspaces/write        | Werkruimte        |
|Bereik config maken/bewerken     | Microsoft.OperationalInsights/workspaces/write        | Werkruimte        |
|Koppeling bereik config-oplossing      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write         | Oplossing         |
|**Stap 2 - vrijgeven meerdere virtuele machines**     |         |         |
|VMOnboarding blade - extensie MMA maken     | Microsoft.Compute/virtualMachines/write           | Virtuele machine        |
|Maken / bewerken van de opgeslagen zoekopdracht     | Microsoft.OperationalInsights/workspaces/write           | Werkruimte        |
|Maken / bewerken bereik config  | Microsoft.OperationalInsights/workspaces/write   | Werkruimte|

## <a name="update-management"></a>Updatebeheer

Updatebeheer is bereikt op meerdere services om de service te bieden. De volgende tabel ziet u de machtigingen om de update management-implementaties beheren:

|**Resource**  |**Rol**  |**Bereik**  |
|---------|---------|---------|
|Automation-account     | Inzender van Log Analytics       | Automation-account        |
|Automation-account    | Inzender voor virtuele machines        | De resourcegroep voor het account        |
|Log Analytics-werkruimte     | Inzender van Log Analytics| Log Analytics-werkruimte        |
|Log Analytics-werkruimte |Lezer van Log Analytics| Abonnement|
|Oplossing     |Inzender van Log Analytics         | Oplossing|
|Virtuele machine     | Inzender voor virtuele machines        | Virtuele machine        |

## <a name="configure-rbac-for-your-automation-account-using-azure-portal"></a>RBAC configureren voor uw Automation-account met Azure portal
1. Meld u aan bij de [Azure-portal](https://portal.azure.com/) en open uw Automation-account via de pagina Automation-accounts.  
2. Klik op de **toegangsbeheer (IAM)** besturingselement in de linkerbovenhoek. Hiermee opent u de **toegangsbeheer (IAM)** pagina waar u kunt nieuwe gebruikers, groepen toevoegen en toepassingen voor het beheren van uw Automation-account en bestaande rollen die kunnen worden geconfigureerd voor het Automation-account weergeven.
   
   ![De knop Toegang](media/automation-role-based-access-control/automation-01-access-button.png)  

### <a name="add-a-new-user-and-assign-a-role"></a>Een nieuwe gebruiker toevoegen en een rol toewijzen
1. Van de **toegangsbeheer (IAM)** pagina, klikt u op **+ toevoegen** openen de **machtigingen toevoegen** pagina waar u kunt een gebruiker, groep of toepassing toevoegen en een rol aan ze toewijzen.  

2. Selecteer een rol in de lijst met beschikbare rollen. U kunt een van de beschikbare ingebouwde rol die ondersteuning biedt voor een Automation-account of elke aangepaste rol die u hebt gedefinieerd.

3. Typ de gebruikersnaam van de gebruiker die u wilt toekennen van machtigingen voor het in de **Selecteer** veld. Selecteer de gebruiker in de lijst en klikt u op **opslaan**.
   
   ![Gebruikers toevoegen](media/automation-role-based-access-control/automation-04-add-users.png)  
   
   Nu u de gebruiker is toegevoegd ziet aan de **gebruikers** pagina met de geselecteerde rol is toegewezen.  
   
   ![Gebruikers weergeven](media/automation-role-based-access-control/automation-05-list-users.png)  
   
   U kunt ook een rol aan de gebruiker toewijzen via de pagina **Rollen**. 
4. Klik op **rollen** van de **toegangsbeheer (IAM)** pagina openen de **rollen** pagina. Via deze pagina kunt u het volgende bekijken: de naam van de rol, het aantal gebruikers en de groepen die aan die rol zijn toegewezen.
   
    ![Rol toewijzen vanaf pagina Gebruikers](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)  
   
   > [!NOTE]
   > Op rollen gebaseerde toegangsbeheer kan alleen worden ingesteld bij het Automation-accountbereik en niet op een resource onder het Automation-account.

### <a name="remove-a-user"></a>Een gebruiker verwijderen
U kunt de toegangsrechten voor een gebruiker die het Automation-account niet beheert of die niet meer werkt voor de organisatie verwijderen. Hieronder vindt u de stappen voor het verwijderen van een gebruiker: 

1. Van de **toegangsbeheer (IAM)** pagina, selecteert u de gebruiker wilt verwijderen en klik op **verwijderen**.
2. Klik op de pagina met toewijzingsdetails op de knop **Verwijderen**.
3. Klik op **Ja** om het verwijderen te bevestigen.

   ![Gebruikers verwijderen](media/automation-role-based-access-control/automation-08-remove-users.png)

## <a name="role-assigned-user"></a>Rol toegewezen gebruiker

Wanneer een gebruiker die is toegewezen aan een functie zich bij Azure aanmelden en haar Automation-account selecteert, zien ze nu eigenaar van de account die worden vermeld in de lijst met **mappen**. Als een gebruiker het Automation-account wil bekijken waaraan hij/zij is toegevoegd, moet deze overschakelen van de standaardmap naar de standaardmap van de eigenaar.

### <a name="user-experience-for-automation-operator-role"></a>Gebruikerservaring voor de rol Automation-operator
Wanneer een gebruiker die is toegewezen aan de weergaven van de rol Automation-Operator het Automation-account dat ze worden toegewezen aan, ze kunnen alleen de lijst met runbooks, runbooktaken en planningen gemaakt in het Automation-account weergeven maar hun definitie niet weergeven. De gebruiker kan de runbooktaak starten, stoppen, onderbreken, hervatten of plannen. De gebruiker heeft geen toegang tot andere Automation-resources zoals configuraties, hybrid worker-groepen of DSC-knooppunten.

![Geen toegang tot bronnen](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)  

De gebruiker heeft toegang voor het weergeven en maken van planningen, maar heeft geen toegang tot andere assettypen.

Deze gebruiker heeft ook geen toegang tot de weergave van de webhooks die aan een runbook zijn gekoppeld.

![Geen toegang tot webhooks](media/automation-role-based-access-control/automation-13-no-access-to-webhooks.png)  

## <a name="configure-rbac-for-your-automation-account-using-azure-powershell"></a>RBAC configureren voor uw Automation-account met Azure PowerShell
Op rollen gebaseerde toegang kan ook worden geconfigureerd voor een Automation-account met de volgende [Azure PowerShell-cmdlets](../role-based-access-control/role-assignments-powershell.md):

• Met [Get-AzureRmRoleDefinition](https://msdn.microsoft.com/library/mt603792.aspx) wordt een lijst weergegeven van alle RBAC-rollen die beschikbaar zijn in Azure Active Directory. U kunt deze opdracht samen met de eigenschap **Naam** gebruiken om alle acties die kunnen worden uitgevoerd door een specifieke rol weer te geven.

```powershell-interactive
Get-AzureRmRoleDefinition -Name 'Automation Operator'
```

Hier volgt een voorbeeld van uitvoer:

```powershell
Name             : Automation Operator
Id               : d3881f73-407a-4167-8283-e981cbba0404
IsCustom         : False
Description      : Automation Operators are able to start, stop, suspend, and resume jobs
Actions          : {Microsoft.Authorization/*/read, Microsoft.Automation/automationAccounts/jobs/read, Microsoft.Automation/automationAccounts/jobs/resume/action, 
                   Microsoft.Automation/automationAccounts/jobs/stop/action...}
NotActions       : {}
AssignableScopes : {/}
``` 

• Met [Get-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt619413.aspx) worden Azure AD RBAC-roltoewijzingen weergegeven bij het opgegeven bereik. Zonder parameters worden met deze opdracht alle roltoewijzingen geretourneerd die onder het abonnement zijn gemaakt. Gebruik de parameter **ExpandPrincipalGroups** om toegangstoewijzingen voor de opgegeven gebruiker weer te geven en voor de groepen waarvan de gebruiker lid is.  
    **Voorbeeld:** gebruik de volgende opdracht om alle gebruikers en de bijbehorende rollen binnen een Automation-account weer te geven.

```powershell-interactive
Get-AzureRMRoleAssignment -scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Hier volgt een voorbeeld van uitvoer:

```powershell
RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Automation/automationAccounts/myAutomationAccount/provid
                     ers/Microsoft.Authorization/roleAssignments/cc594d39-ac10-46c4-9505-f182a355c41f
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Automation/automationAccounts/myAutomationAccount
DisplayName        : admin@contoso.com
SignInName         : admin@contoso.com
RoleDefinitionName : Automation Operator
RoleDefinitionId   : d3881f73-407a-4167-8283-e981cbba0404
ObjectId           : 15f26a47-812d-489a-8197-3d4853558347
ObjectType         : User
```

• [New AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603580.aspx) toegang toewijzen aan gebruikers, groepen en toepassingen naar een bepaald bereik.  
    **Voorbeeld:** gebruik van de volgende opdracht om toe te wijzen de rol 'Automation-Operator' voor een gebruiker in het Automation-accountbereik.

```powershell-interactive
New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName 'Automation operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Hier volgt een voorbeeld van uitvoer:

```powershell
RoleAssignmentId   : /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/myResourceGroup/Providers/Microsoft.Automation/automationAccounts/myAutomationAccount/provid
                     ers/Microsoft.Authorization/roleAssignments/25377770-561e-4496-8b4f-7cba1d6fa346
Scope              : /subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/myResourceGroup/Providers/Microsoft.Automation/automationAccounts/myAutomationAccount
DisplayName        : admin@contoso.com
SignInName         : admin@contoso.com
RoleDefinitionName : Automation Operator
RoleDefinitionId   : d3881f73-407a-4167-8283-e981cbba0404
ObjectId           : f5ecbe87-1181-43d2-88d5-a8f5e9d8014e
ObjectType         : User
```

• Gebruik [Remove-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603781.aspx) toegang van een opgegeven gebruiker, groep of toepassing verwijderen uit een bepaald bereik.  
    **Voorbeeld:** gebruik van de volgende opdracht om de gebruiker verwijderen uit de rol 'Automation-Operator' in het Automation-accountbereik.

```powershell-interactive
Remove-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to remove> -RoleDefinitionName 'Automation Operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Vervang in de voorgaande voorbeelden **Id aanmelden**, **abonnements-Id**, **Resourcegroepnaam**, en **Automation-accountnaam** met uw accountdetails. Kies **Ja** om te bevestigen voordat u verdergaat met het verwijderen van de roltoewijzing voor de gebruiker.   

## <a name="next-steps"></a>Volgende stappen
* Raadpleeg [RBAC met Azure PowerShell beheren](../role-based-access-control/role-assignments-powershell.md) voor informatie over verschillende manieren om RBAC voor Azure Automation te configureren.
* Zie [Een runbook starten](automation-starting-a-runbook.md) voor informatie over verschillende manieren om een runbook te starten.
* Raadpleeg [Azure Automation-runbooktypen](automation-runbook-types.md) voor informatie over verschillende typen runbooks

