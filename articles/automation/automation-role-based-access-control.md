---
title: Op rollen gebaseerd toegangsbeheer in Azure Automation
description: Met op rollen gebaseerd toegangsbeheer (RBAC) beschikt u over toegangsbeheer voor Azure-resources. In dit artikel wordt beschreven hoe u RBAC instelt in Azure Automation.
keywords: automatisering rbac, rolgebaseerde toegangscontrole, azure rbac
services: automation
ms.service: automation
ms.component: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 05/17/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 538208c39d6436c15b95760133e00c980e2e8277
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52727899"
---
# <a name="role-based-access-control-in-azure-automation"></a>Op rollen gebaseerd toegangsbeheer in Azure Automation

Met op rollen gebaseerd toegangsbeheer (RBAC) beschikt u over toegangsbeheer voor Azure-resources. Met behulp van [RBAC](../role-based-access-control/overview.md), kunt u taken scheiden binnen uw team en alleen de mate van toegang verlenen aan gebruikers, groepen en toepassingen die ze nodig hebben om hun werk. Op rollen gebaseerde toegang kan worden verleend aan gebruikers via de Azure-portal, Azure-opdrachtregelprogramma's of Azure Management-API's.

## <a name="roles-in-automation-accounts"></a>Rollen in Automation-accounts

In Azure Automation wordt toegang verleend door de juiste RBAC-rol toe te wijzen aan gebruikers, groepen en toepassingen bij het Automation-accountbereik. Hieronder vindt u de ingebouwde rollen die worden ondersteund met een Automation-account:

| **Rol** | **Beschrijving** |
|:--- |:--- |
| Eigenaar |De rol eigenaar geeft toegang tot alle resources en acties binnen een Automation-account, inclusief het bieden van toegang aan andere gebruikers, groepen en toepassingen voor het beheren van het Automation-account. |
| Inzender |De rol van Bijdrager maakt het mogelijk om alles te beheren, behalve de toegangsrechten van andere gebruikers te wijzigen naar een Automation-account. |
| Lezer |Met de rol van Lezer kunt u alle resources in een Automation-account bekijken, maar niets wijzigen. |
| Automation-operator |De rol Automation-Operator kunt u de runbooknaam van het en eigenschappen weergeven en taken voor alle runbooks in een Automation-account maken en beheren. Deze rol is handig als u niet wilt dat uw Automation-accountresources zoals referentieassets en runbooks worden weergegeven of gewijzigd, maar u wel wilt toestaan dat leden van uw organisatie deze runbooks uitvoeren. |
|Operator voor Automation-taak|De rol Operator voor Automation kunt u taken voor alle runbooks in een Automation-account maken en beheren.|
|Operator voor Automation-runbook|De rol Operator voor Automation-Runbook kunt u de naam en de eigenschappen van een runbook weergeven.|
| Inzender van Log Analytics | De rol Inzender van Log Analytics kunt u alle controlegegevens lezen en bewerken van instellingen voor controle. Bewerken van instellingen voor controle houdt het toevoegen van de VM-extensie voor VM's, het lezen van opslagaccountsleutels om te kunnen verzamelen van Logboeken van Azure-opslag, het maken en configureren van Automation-accounts, oplossingen toe te voegen en Azure diagnostics configureren op alle Azure-resources.|
| Lezer van Log Analytics | De rol Lezer van Log Analytics kunt u bekijken en zoeken van alle bewaking gegevens, evenals controle-instellingen weergeven. Dit omvat de configuratie van Azure diagnostics op alle Azure-resources bekijken. |
| Controlebijdrager | De rol Inzender controle kunt u lezen alle bewakingsgegevens en controle-instellingen bijwerken.|
| Controlelezer | De rol van lezer controle kunt u alle controlegegevens lezen. |
| Beheerder van gebruikerstoegang |De beheerdersrol gebruiker toegang kunt u gebruikerstoegang tot Azure Automation-accounts beheren. |

## <a name="role-permissions"></a>Rolmachtigingen

De volgende tabellen beschrijven de specifieke machtigingen die aan elke rol. Dit kunnen bijvoorbeeld acties, welke machtigingen geven, en NotActions, die ze beperken.

### <a name="owner"></a>Eigenaar

Een eigenaar kan alles beheren, inclusief toegang. De volgende tabel ziet u de machtigingen voor de rol:

|Acties|Beschrijving|
|---|---|
|Microsoft.Automation/automationAccounts/|Maken en beheren van resources van alle typen.|

### <a name="contributor"></a>Inzender

Een medewerker kan alles beheren behalve toegang. De volgende tabel ziet u de machtigingen verleend en wordt geweigerd voor de rol:

|**Acties**  |**Beschrijving**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/|Maken en beheren van resources van alle typen|
|**Geen acties**||
|Microsoft.Authorization/*/Delete| Rollen en roltoewijzingen verwijderen.       |
|Microsoft.Authorization/*/Write     |  Maak rollen en roltoewijzingen.       |
|Microsoft.Authorization/elevateAccess/Action    | De mogelijkheid om te maken van een Administrator voor gebruikerstoegang weigert.       |

### <a name="reader"></a>Lezer

Een lezer kunt alle resources weergeven in een Automation-account, maar geen wijzigingen kunt aanbrengen.

|**Acties**  |**Beschrijving**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/read|Alle resources in een Automation-account weergeven. |

### <a name="automation-operator"></a>Automation-operator

Er is een Automation-Operator kunt maken en beheren van taken en runbooknamen en de eigenschappen voor alle runbooks in een Automation-account lezen.  Opmerking: Als u wilt voor het beheren van beheerderstoegang tot afzonderlijke runbooks en vervolgens niet instellen van deze rol, en in plaats daarvan de rollen 'Operator voor Automation-taak' en 'Operator voor Automation-Runbook' gebruiken in combinatie. De volgende tabel ziet u de machtigingen voor de rol:

|**Acties**  |**Beschrijving**  |
|---------|---------|
|Microsoft.Authorization/*/read|Autorisatie worden gelezen.|
|Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read|Resources voor de Hybrid Runbook Worker gelezen.|
|Microsoft.Automation/automationAccounts/jobs/read|Lijst met taken van het runbook.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|Hervat een taak die is onderbroken.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|Een taak wordt geannuleerd.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|Lees de Taakstromen en de uitvoer.|
|Microsoft.Automation/automationAccounts/jobs/output/read|De uitvoer van een taak ophalen.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|Onderbreken van een taak wordt uitgevoerd.|
|Microsoft.Automation/automationAccounts/jobs/write|Taken maken.|
|Microsoft.Automation/automationAccounts/jobSchedules/read|Een Azure Automation-taakschema ophalen.|
|Microsoft.Automation/automationAccounts/jobSchedules/write|Maak een Azure Automation-taakschema.|
|Microsoft.Automation/automationAccounts/linkedWorkspace/read|Ophalen van de werkruimte die is gekoppeld aan het automation-account.|
|Microsoft.Automation/automationAccounts/read|Maak een Azure Automation-account.|
|Microsoft.Automation/automationAccounts/runbooks/read|Ophalen van een Azure Automation-runbook.|
|Microsoft.Automation/automationAccounts/schedules/read|Een Azure Automation-planningsasset ophalen.|
|Microsoft.Automation/automationAccounts/schedules/write|Maken of bijwerken van een Azure Automation-planningsasset.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |Rollen en roltoewijzingen lezen.         |
|Microsoft.Resources/deployments/*      |Maken en beheren van implementaties van resource-groep.         |
|Microsoft.Insights/alertRules/*      | Maken en beheren van regels voor waarschuwingen.        |
|Microsoft.Support/* |Maak en ondersteuningstickets beheren.|

### <a name="automation-job-operator"></a>Operator voor Automation-taak

Een rol Operator voor Automation krijgt bij het Automation-accountbereik. Hiermee wordt de operatormachtigingen voor het maken en beheren van taken voor alle runbooks in het account. De volgende tabel ziet u de machtigingen voor de rol:

|**Acties**  |**Beschrijving**  |
|---------|---------|
|Microsoft.Authorization/*/read|Autorisatie worden gelezen.|
|Microsoft.Automation/automationAccounts/jobs/read|Lijst met taken van het runbook.|
|Microsoft.Automation/automationAccounts/jobs/resume/action|Hervat een taak die is onderbroken.|
|Microsoft.Automation/automationAccounts/jobs/stop/action|Een taak wordt geannuleerd.|
|Microsoft.Automation/automationAccounts/jobs/streams/read|Lees de Taakstromen en de uitvoer.|
|Microsoft.Automation/automationAccounts/jobs/suspend/action|Onderbreken van een taak wordt uitgevoerd.|
|Microsoft.Automation/automationAccounts/jobs/write|Taken maken.|
|Microsoft.Resources/subscriptions/resourceGroups/read      |  Rollen en roltoewijzingen lezen.       |
|Microsoft.Resources/deployments/*      |Maken en beheren van implementaties van resource-groep.         |
|Microsoft.Insights/alertRules/*      | Maken en beheren van regels voor waarschuwingen.        |
|Microsoft.Support/* |Maak en ondersteuningstickets beheren.|

### <a name="automation-runbook-operator"></a>Operator voor Automation-runbook

Een Operator voor Automation-Runbook-rol wordt verleend op het bereik van het Runbook. Een Operator voor Automation-Runbook kunt weergeven, de naam en de eigenschappen van het runbook.  Deze rol in combinatie met de rol 'Automation-taak Operator' kunt de operator op die ook maken en beheren van taken voor het runbook. De volgende tabel ziet u de machtigingen voor de rol:

|**Acties**  |**Beschrijving**  |
|---------|---------|
|Microsoft.Automation/automationAccounts/runbooks/read     | Lijst met runbooks.        |
|Microsoft.Authorization/*/read      | Autorisatie worden gelezen.        |
|Microsoft.Resources/subscriptions/resourceGroups/read      |Rollen en roltoewijzingen lezen.         |
|Microsoft.Resources/deployments/*      | Maken en beheren van implementaties van resource-groep.         |
|Microsoft.Insights/alertRules/*      | Maken en beheren van regels voor waarschuwingen.        |
|Microsoft.Support/*      | Maak en ondersteuningstickets beheren.        |

### <a name="log-analytics-contributor"></a>Inzender van Log Analytics

Een inzender van Log Analytics kan alle controlegegevens lezen en bewerken van instellingen voor controle. Bewerken van instellingen voor controle houdt het toevoegen van de VM-extensie voor virtuele machines; lezen van opslagaccountsleutels om te kunnen verzamelen van Logboeken van Azure Storage; configureren het maken en configureren van Automation-accounts; toevoegen van oplossingen en Azure diagnostics configureren op alle Azure-resources. De volgende tabel ziet u de machtigingen voor de rol:

|**Acties**  |**Beschrijving**  |
|---------|---------|
|* / lezen|Bronnen van alle typen, met uitzondering van geheimen worden gelezen.|
|Microsoft.Automation/automationAccounts/*|Automation-accounts beheren.|
|Microsoft.ClassicCompute/virtualMachines/extensions/*|Maken en beheren van extensies voor virtuele machines.|
|Microsoft.ClassicStorage/storageAccounts/listKeys/action|Lijst met sleutels voor klassieke opslagaccount.|
|Microsoft.Compute/virtualMachines/extensions/*|Maken en beheren van extensies voor klassieke virtuele machines.|
|Microsoft.Insights/alertRules/*|Waarschuwingsregels voor lezen/schrijven/verwijderen.|
|Microsoft.Insights/diagnosticSettings/*|Diagnostische instellingen lezen, schrijven en verwijderen.|
|Microsoft.OperationalInsights/*|Log Analytics beheren.|
|Microsoft.OperationsManagement/*|Beheren van oplossingen in werkruimten.|
|Microsoft.Resources/deployments/*|Maken en beheren van implementaties van resource-groep.|
|Microsoft.Resources/subscriptions/resourcegroups/deployments/*|Maken en beheren van implementaties van resource-groep.|
|Microsoft.Storage/storageAccounts/listKeys/action|Lijst met sleutels voor het opslagaccount.|
|Microsoft.Support/*|Maak en ondersteuningstickets beheren.|

### <a name="log-analytics-reader"></a>Lezer van Log Analytics

Een lezer van Log Analytics kunt bekijken en zoeken van alle bewakingsgegevens en de controle-instellingen, inclusief het weergeven van de configuratie van Azure diagnostics op alle Azure-resources weergeven. De volgende tabel ziet u de machtigingen verleend of geweigerd voor de rol:

|**Acties**  |**Beschrijving**  |
|---------|---------|
|* / lezen|Bronnen van alle typen, met uitzondering van geheimen worden gelezen.|
|Microsoft.OperationalInsights/workspaces/analytics/query/action|Query's in Log Analytics beheren.|
|Microsoft.OperationalInsights/workspaces/search/action|Zoeken naar Log Analytics-gegevens.|
|Microsoft.Support/*|Maak en ondersteuningstickets beheren.|
|**Geen acties**| |
|Microsoft.OperationalInsights/workspaces/sharedKeys/read|Kan de gedeelde toegangssleutel gelezen.|

### <a name="monitoring-contributor"></a>Controlebijdrager

Een bijdrager bewaking kan alle controlegegevens lezen en controle-instellingen bijwerken. De volgende tabel ziet u de machtigingen voor de rol:

|**Acties**  |**Beschrijving**  |
|---------|---------|
|* / lezen|Bronnen van alle typen, met uitzondering van geheimen worden gelezen.|
|Microsoft.AlertsManagement/alerts/*|Waarschuwingen beheren.|
|Microsoft.AlertsManagement/alertsSummary/*|De waarschuwing dashboard beheren.|
|Microsoft.Insights/AlertRules/*|Regels voor waarschuwingen beheren.|
|Microsoft.Insights/components/*|Application Insights-onderdelen beheren.|
|Microsoft.Insights/DiagnosticSettings/*|Diagnostische instellingen beheren.|
|Microsoft.Insights/eventtypes/*|Lijst met gebeurtenissen in activiteitenlogboeken (van Beheergebeurtenissen) in een abonnement. Deze machtiging is van toepassing op zowel programmatische als portal toegang tot het activiteitenlogboek.|
|Microsoft.Insights/LogDefinitions/*|Deze machtiging is vereist voor gebruikers die toegang nodig tot activiteitenlogboeken via de portal. Logboekcategorieën lijst in het activiteitenlogboek.|
|Microsoft.Insights/MetricDefinitions/*|Metrische definities (lijst met beschikbare metrische gegevens typen voor een resource) lezen.|
|Microsoft.Insights/Metrics/*|De metrische gegevens voor een resource lezen.|
|Microsoft.Insights/Register/Action|De Microsoft.Insights-provider hebt geregistreerd.|
|Microsoft.Insights/webtests/*|Application Insights-webtests beheren.|
|Microsoft.OperationalInsights/workspaces/intelligencepacks/*|Oplossingspakketten Log Analytics beheren.|
|Microsoft.OperationalInsights/workspaces/savedSearches/*|Log Analytics opgeslagen zoekopdrachten beheren.|
|Microsoft.OperationalInsights/workspaces/search/action|Zoeken naar Log Analytics-werkruimten.|
|Microsoft.OperationalInsights/workspaces/sharedKeys/action|Lijst met sleutels voor een Log Analytics-werkruimte.|
|Microsoft.OperationalInsights/workspaces/storageinsightconfigs/*|Log Analytics insight opslagconfiguraties beheren.|
|Microsoft.Support/*|Maak en ondersteuningstickets beheren.|
|Microsoft.WorkloadMonitor/workloads/*|Workloads beheren.|

### <a name="monitoring-reader"></a>Controlelezer

Een lezer bewaking kan alle controlegegevens lezen. De volgende tabel ziet u de machtigingen voor de rol:

|**Acties**  |**Beschrijving**  |
|---------|---------|
|* / lezen|Bronnen van alle typen, met uitzondering van geheimen worden gelezen.|
|Microsoft.OperationalInsights/workspaces/search/action|Zoeken naar Log Analytics-werkruimten.|
|Microsoft.Support/*|Maken en ondersteuningstickets beheren|

### <a name="user-access-administrator"></a>Beheerder van gebruikerstoegang

Een Administrator voor gebruikerstoegang beheren de gebruikerstoegang tot Azure-resources. De volgende tabel ziet u de machtigingen voor de rol:

|**Acties**  |**Beschrijving**  |
|---------|---------|
|* / lezen|Lezen van alle resources|
|Microsoft.Authorization/*|Machtigingen beheren|
|Microsoft.Support/*|Maken en ondersteuningstickets beheren|

## <a name="onboarding"></a>Onboarding

De volgende tabellen de minimaal vereiste machtigingen die nodig zijn voor onboarding van virtuele machines voor het bijhouden van weergeven of bijwerken van oplossingen voor beheer.

### <a name="onboarding-from-a-virtual-machine"></a>Onboarding van een virtuele machine

|**Actie**  |**Machtiging**  |**Minimale bereik**  |
|---------|---------|---------|
|Schrijven van nieuwe implementatie      | Microsoft.Resources/deployments/*          |Abonnement          |
|Schrijven van nieuwe resourcegroep      | Microsoft.Resources/subscriptions/resourceGroups/write        | Abonnement          |
|Nieuwe standaard werkruimte maken      | Microsoft.OperationalInsights/workspaces/write         | Resourcegroep         |
|Nieuw Account maken      |  Microsoft.Automation/automationAccounts/write        |Resourcegroep         |
|Koppeling werkruimte en account      |Microsoft.OperationalInsights/workspaces/write</br>Microsoft.Automation/automationAccounts/read|Werkruimte</br>Automation-account
|Oplossing maken      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write |Resourcegroep          |
|Maken van de MMA-extensie      | Microsoft.Compute/virtualMachines/write         | Virtuele machine         |
|Opgeslagen zoekactie maken      | Microsoft.OperationalInsights/workspaces/write          | Werkruimte         |
|Bereik-configuratie maken      | Microsoft.OperationalInsights/workspaces/write          | Werkruimte         |
|Koppeling oplossing voor het bereik config      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write         | Oplossing         |
|Onboardingstatus van de werkruimte worden gecontroleerd: lezen      | Microsoft.OperationalInsights/workspaces/read         | Werkruimte         |
|Controleren van de onboarding - lezen eigenschap van de werkruimte van account gekoppeld     | Microsoft.Automation/automationAccounts/read      | Automation-account        |
|Onboardingstatus van de controleren - oplossing lezen      | Microsoft.OperationalInsights/workspaces/intelligencepacks/read          | Oplossing         |
|Controleren van de onboarding - lezen-VM      | Microsoft.Compute/virtualMachines/read         | Virtuele machine         |
|Onboardingstatus van de controleren - account lezen      | Microsoft.Automation/automationAccounts/read  |  Automation-account   |

### <a name="onboarding-from-automation-account"></a>Onboarding van Automation-account

|**Actie**  |**Machtiging** |**Minimale bereik**  |
|---------|---------|---------|
|Nieuwe implementatie maken     | Microsoft.Resources/deployments/*        | Abonnement         |
|Nieuwe resourcegroep maken     | Microsoft.Resources/subscriptions/resourceGroups/write         | Abonnement        |
|Blade AutomationOnboarding - nieuwe werkruimte maken     |Microsoft.OperationalInsights/workspaces/write           | Resourcegroep        |
|AutomationOnboarding blade - gekoppelde werkruimte lezen     | Microsoft.Automation/automationAccounts/read        | Automation-account       |
|AutomationOnboarding blade - oplossing     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read         | Oplossing        |
|AutomationOnboarding blade - werkruimte te lezen     | Microsoft.OperationalInsights/workspaces/intelligencepacks/read        | Werkruimte        |
|Koppeling voor werkruimte en Account maken     | Microsoft.OperationalInsights/workspaces/write        | Werkruimte        |
|Account voor schoenendoos schrijven      | Microsoft.Automation/automationAccounts/write        | Account        |
|Oplossing maken      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write        | Resourcegroep         |
|Maak/opgeslagen zoekopdracht bewerken     | Microsoft.OperationalInsights/workspaces/write        | Werkruimte        |
|Bereik config maken/bewerken     | Microsoft.OperationalInsights/workspaces/write        | Werkruimte        |
|Koppeling oplossing voor het bereik config      | Microsoft.OperationalInsights/workspaces/intelligencepacks/write         | Oplossing         |
|**Stap 2: onboarding meerdere virtuele machines**     |         |         |
|VMOnboarding blade - extensie van de MMA maken     | Microsoft.Compute/virtualMachines/write           | Virtuele machine        |
|Maken / bewerken van opgeslagen zoekopdracht     | Microsoft.OperationalInsights/workspaces/write           | Werkruimte        |
|Maken / bewerken van bereik config  | Microsoft.OperationalInsights/workspaces/write   | Werkruimte|

## <a name="update-management"></a>Updatebeheer

Updatebeheer bereikt over meerdere services om de service te bieden. De volgende tabel ziet u de machtigingen die nodig zijn voor management-update-implementaties beheren:

|**Resource**  |**Rol**  |**Bereik**  |
|---------|---------|---------|
|Automation-account     | Inzender van Log Analytics       | Automation-account        |
|Automation-account    | Inzender voor virtuele machines        | Resourcegroep voor het account        |
|Log Analytics-werkruimte     | Inzender van Log Analytics| Log Analytics-werkruimte        |
|Log Analytics-werkruimte |Lezer van Log Analytics| Abonnement|
|Oplossing     |Inzender van Log Analytics         | Oplossing|
|Virtuele machine     | Inzender voor virtuele machines        | Virtuele machine        |

## <a name="configure-rbac-for-your-automation-account"></a>RBAC configureren voor uw Automation-account

In de volgende sectie ziet u hoe u RBAC configureren voor uw Automation-Account via de [portal](#configure-rbac-using-the-azure-portal) en [PowerShell](#configure-rbac-using-powershell)

### <a name="configure-rbac-using-the-azure-portal"></a>RBAC met behulp van de Azure-portal configureren

1. Meld u aan bij de [Azure-portal](https://portal.azure.com/) en open uw Automation-account via de pagina Automation-accounts.
2. Klik op de **toegangsbeheer (IAM)** besturingselement in de linkerbovenhoek. Hiermee opent u de **toegangsbeheer (IAM)** pagina waar u nieuwe gebruikers, groepen, kunt toevoegen en toepassingen voor het beheren van uw Automation-account en bestaande rollen die kunnen worden geconfigureerd voor het Automation-account kunt weergeven.
3. Klik op de **roltoewijzingen** tabblad.

   ![De knop Toegang](media/automation-role-based-access-control/automation-01-access-button.png)

#### <a name="add-a-new-user-and-assign-a-role"></a>Een nieuwe gebruiker toevoegen en een rol toewijzen

1. Uit de **toegangsbeheer (IAM)** pagina, klikt u op **+ roltoewijzing toevoegen** openen de **roltoewijzing toevoegen** pagina waar u kunt een gebruiker, groep of toepassing toevoegen en een rol toewijzen naar deze.

2. Selecteer een rol in de lijst met beschikbare rollen. U kunt een van de beschikbare ingebouwde rollen die ondersteuning biedt voor een Automation-account of een aangepaste rol die u hebt gedefinieerd.

3. Typ de gebruikersnaam van de gebruiker die u wilt machtigen voor in de **Selecteer** veld. Selecteer de gebruiker in de lijst en klikt u op **opslaan**.

   ![Gebruikers toevoegen](media/automation-role-based-access-control/automation-04-add-users.png)

   Nu u de gebruiker is toegevoegd ziet aan de **gebruikers** pagina met de geselecteerde rol

   ![Gebruikers weergeven](media/automation-role-based-access-control/automation-05-list-users.png)

   U kunt ook een rol aan de gebruiker toewijzen via de pagina **Rollen**.
4. Klik op **rollen** uit de **toegangsbeheer (IAM)** pagina wordt geopend de **rollen** pagina. Via deze pagina kunt u het volgende bekijken: de naam van de rol, het aantal gebruikers en de groepen die aan die rol zijn toegewezen.

    ![Rol toewijzen vanaf pagina Gebruikers](media/automation-role-based-access-control/automation-06-assign-role-from-users-blade.png)

   > [!NOTE]
   > Op rollen gebaseerd toegangsbeheer kan alleen worden ingesteld op het Automation-accountbereik en niet op een resource onder het Automation-account.

#### <a name="remove-a-user"></a>Een gebruiker verwijderen

De toegangsmachtigingen voor een gebruiker die het Automation-account niet beheert, of die niet meer werkt voor de organisatie, kunt u verwijderen. Hieronder vindt u de stappen voor het verwijderen van een gebruiker:

1. Uit de **toegangsbeheer (IAM)** pagina, selecteert u de gebruiker wilt verwijderen en klik op **verwijderen**.
2. Klik op de pagina met toewijzingsdetails op de knop **Verwijderen**.
3. Klik op **Ja** om het verwijderen te bevestigen.

   ![Gebruikers verwijderen](media/automation-role-based-access-control/automation-08-remove-users.png)

### <a name="configure-rbac-using-powershell"></a>RBAC met behulp van PowerShell configureren

Rollen gebaseerde toegang kan ook worden geconfigureerd voor een Automation-account met behulp van de volgende [Azure PowerShell-cmdlets](../role-based-access-control/role-assignments-powershell.md):

[Get-AzureRmRoleDefinition](https://msdn.microsoft.com/library/mt603792.aspx) geeft een lijst van alle RBAC-rollen die beschikbaar in Azure Active Directory zijn. U kunt deze opdracht samen met de eigenschap **Naam** gebruiken om alle acties die kunnen worden uitgevoerd door een specifieke rol weer te geven.

```azurepowershell-interactive
Get-AzureRmRoleDefinition -Name 'Automation Operator'
```

Hier volgt de voorbeelduitvoer:

```azurepowershell
Name             : Automation Operator
Id               : d3881f73-407a-4167-8283-e981cbba0404
IsCustom         : False
Description      : Automation Operators are able to start, stop, suspend, and resume jobs
Actions          : {Microsoft.Authorization/*/read, Microsoft.Automation/automationAccounts/jobs/read, Microsoft.Automation/automationAccounts/jobs/resume/action,
                   Microsoft.Automation/automationAccounts/jobs/stop/action...}
NotActions       : {}
AssignableScopes : {/}
```

[Get-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt619413.aspx) worden Azure AD RBAC-roltoewijzingen bij het opgegeven bereik. Zonder parameters worden met deze opdracht alle roltoewijzingen geretourneerd die onder het abonnement zijn gemaakt. Gebruik de parameter **ExpandPrincipalGroups** om toegangstoewijzingen voor de opgegeven gebruiker weer te geven en voor de groepen waarvan de gebruiker lid is.
    **Voorbeeld:** gebruik de volgende opdracht om alle gebruikers en de bijbehorende rollen binnen een Automation-account weer te geven.

```azurepowershell-interactive
Get-AzureRMRoleAssignment -scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Hier volgt de voorbeelduitvoer:

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

[Nieuwe-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603580.aspx) toegang toewijzen aan gebruikers, groepen en toepassingen naar een bepaald bereik.
    **Voorbeeld:** gebruik van de volgende opdracht om toe te wijzen de rol 'Automation-Operator' voor een gebruiker in het Automation-accountbereik.

```azurepowershell-interactive
New-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to grant access> -RoleDefinitionName 'Automation operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Hier volgt de voorbeelduitvoer:

```azurepowershell
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

Gebruik [Remove-AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt603781.aspx) toegang van een opgegeven gebruiker, groep of toepassing verwijderen uit een bepaald bereik.
    **Voorbeeld:** gebruik van de volgende opdracht om de gebruiker verwijderen uit de rol 'Automation-Operator' in het Automation-accountbereik.

```azurepowershell-interactive
Remove-AzureRmRoleAssignment -SignInName <sign-in Id of a user you wish to remove> -RoleDefinitionName 'Automation Operator' -Scope '/subscriptions/<SubscriptionID>/resourcegroups/<Resource Group Name>/Providers/Microsoft.Automation/automationAccounts/<Automation account name>'
```

Vervang in de voorgaande voorbeelden **Id voor aanmelden**, **abonnements-Id**, **groepsnaam voor accountresources**, en **Automation-accountnaam** met uw accountgegevens. Kies **Ja** om te bevestigen voordat u verdergaat met het verwijderen van de roltoewijzing voor de gebruiker.

### <a name="user-experience-for-automation-operator-role---automation-account"></a>Gebruikerservaring voor de rol Automation-operator - Automation-Account

Wanneer een gebruiker, het Automation-account dat ze worden toegewezen aan die is toegewezen aan de rol Automation-Operator in het bereik Automation-Account bekijkt, ze kunnen alleen de lijst met runbooks, runbooktaken, weergeven en schema's gemaakt in het Automation-account, maar niet de hun de definitie. De gebruiker kan de runbooktaak starten, stoppen, onderbreken, hervatten of plannen. De gebruiker heeft geen toegang tot andere Automation-resources, zoals configuraties, hybrid worker-groepen of DSC-knooppunten.

![Geen toegang tot resources](media/automation-role-based-access-control/automation-10-no-access-to-resources.png)

## <a name="configure-rbac-for-runbooks"></a>RBAC configureren voor Runbooks

Azure Automation kunt u RBAC toewijzen aan specifieke runbooks. Hiervoor voert u het volgende script voor een gebruiker toevoegen aan een specifiek runbook. Het volgende script kan worden uitgevoerd door een Automation-accountbeheerder of Tenantbeheerder.

```azurepowershell-interactive
$rgName = "<Resource Group Name>" # Resource Group name for the Automation Account
$automationAccountName ="<Automation Account Name>" # Name of the Automation Account
$rbName = "<Name of Runbook>" # Name of the runbook
$userId = "<User ObjectId>" # Azure Active Directory (AAD) user's ObjectId from the directory

# Gets the Automation Account resource
$aa = Get-AzureRmResource -ResourceGroupName $rgName -ResourceType "Microsoft.Automation/automationAccounts" -ResourceName $automationAccountName

# Get the Runbook resource
$rb = Get-AzureRmResource -ResourceGroupName $rgName -ResourceType "Microsoft.Automation/automationAccounts/runbooks" -ResourceName "$automationAccountName/$rbName"

# The Automation Job Operator role only needs to be ran once per user.
New-AzureRmRoleAssignment -ObjectId $userId -RoleDefinitionName "Automation Job Operator" -Scope $aa.ResourceId

# Adds the user to the Automation Runbook Operator role to the Runbook scope
New-AzureRmRoleAssignment -ObjectId $userId -RoleDefinitionName "Automation Runbook Operator" -Scope $rb.ResourceId
```

Eenmaal is uitgevoerd, laat de gebruiker zich aanmeldt bij de Azure-portal en de weergave **alle Resources**. In de lijst zien ze het Runbook dat ze zijn toegevoegd als een **Operator voor Automation-Runbook** voor.

![Runbook RBAC in de portal](./media/automation-role-based-access-control/runbook-rbac.png)

### <a name="user-experience-for-automation-operator-role---runbook"></a>Gebruikerservaring voor de rol Automation - Runbook

Wanneer een gebruiker, die aan de rol Automation-Operator in het bereik instellen voor Runbook een Runbook dat ze worden toegewezen aan is toegewezen, kunnen ze alleen het runbook starten en de runbooktaken weergeven.

![Heeft alleen toegang tot het starten](media/automation-role-based-access-control/automation-only-start.png)

## <a name="next-steps"></a>Volgende stappen

* Raadpleeg [RBAC met Azure PowerShell beheren](../role-based-access-control/role-assignments-powershell.md) voor informatie over verschillende manieren om RBAC voor Azure Automation te configureren.
* Zie [Een runbook starten](automation-starting-a-runbook.md) voor informatie over verschillende manieren om een runbook te starten.
* Raadpleeg [Azure Automation-runbooktypen](automation-runbook-types.md) voor informatie over verschillende typen runbooks
