---
title: Log Analytics-werk ruimten beheren in Azure Monitor | Microsoft Docs
description: U kunt Log Analytics-werk ruimten beheren in Azure Monitor met behulp van verschillende beheer taken voor gebruikers, accounts, werk ruimten en Azure-accounts.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/16/2019
ms.author: magoedte
ms.openlocfilehash: fbfbd8e26ab3e92f06194322be7ec2be2fb180fd
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68254453"
---
# <a name="manage-log-data-and-workspaces-in-azure-monitor"></a>Logboek gegevens en-werk ruimten in Azure Monitor beheren
Azure Monitor worden logboek gegevens opgeslagen in een Log Analytics-werk ruimte, wat in feite een container is die gegevens-en configuratie gegevens bevat. Als u de toegang tot logboek gegevens wilt beheren, voert u verschillende beheer taken uit voor werk ruimten. U of andere leden van uw organisatie kunnen meerdere werkruimten gebruiken om verschillende gegevenssets te beheren die worden verzameld uit de gehele of delen van uw IT-infrastructuur.

In dit artikel wordt uitgelegd hoe u de toegang tot logboeken beheert en hoe u de werk ruimten waarin deze zich bevinden beheert. 

## <a name="create-a-workspace"></a>Een werkruimte maken
Als u een Log Analytics-werk ruimte wilt maken, moet u het volgende doen:

1. U dient een Azure-abonnement te hebben.
2. U dient een naam voor de werkruimte te kiezen.
3. De werkruimte koppelen aan een van uw abonnementen en resourcegroepen.
4. U dient een geografische locatie te kiezen.

Raadpleeg de volgende artikelen voor meer informatie over het maken van een werk ruimte:

- [Een Log Analytics-werk ruimte maken in de Azure Portal](../learn/quick-create-workspace.md)
- [Een Log Analytics-werk ruimte maken met Azure CLI 2,0](../learn/quick-create-workspace-cli.md)
- [Een Log Analytics-werk ruimte maken met Azure PowerShell](../learn/quick-create-workspace-posh.md)

## <a name="determine-the-number-of-workspaces-you-need"></a>Vaststellen hoeveel werkruimten u nodig hebt
Een Log Analytics-werk ruimte is een Azure-resource en is een container waarin gegevens worden verzameld, geaggregeerd, geanalyseerd en weer gegeven in Azure Monitor. U kunt meerdere werk ruimten per Azure-abonnement hebben en u hebt toegang tot meer dan één werk ruimte, met de mogelijkheid om deze eenvoudig op te vragen. In deze sectie wordt beschreven wanneer het handig kan zijn om meer dan één werkruimte te maken.

Een Log Analytics-werk ruimte biedt:

* Een geografische locatie voor de opslag van gegevens.
* Gegevens isolatie om verschillende gebruikers toegangs rechten te definiëren in de werk ruimte-georiënteerde modus. Niet relevant bij het werken in de modus resource-georiënteerd.
* Het bereik voor de configuratie van instellingen, zoals de [prijs categorie](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#changing-pricing-tier), retentie en het beperken van [gegevens](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#daily-cap). [](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)
* De kosten voor gegevens opname en-retentie worden gemaakt op de werkruimte resource.

Uit oogpunt van verbruik, wordt u aangeraden dat u werkruimten zo weinig mogelijk maakt. Het maakt beheer en query's eenvoudiger en sneller. Maar op basis van de voorgaande kenmerken, kunt u meerdere werkruimten maken:

* U bent een wereld wijd bedrijf en u hebt logboek gegevens nodig die in specifieke regio's zijn opgeslagen voor gegevens-soevereiniteit of nalevings redenen.
* U gebruikt Azure en wilt kosten voor de overdracht van uitgaande gegevens voorkomen door een werkruimte in dezelfde regio te hebben als de Azure-resource die deze beheert.
* U bent aanbieder van beheerde services en moet de Log Analytics-gegevens voor elke klant geïsoleerd van de gegevens van andere klanten bewaren.
* U beheert meerdere klanten en u wilt dat elke klant/afdeling/bedrijfs groep de eigen gegevens ziet, maar niet de gegevens van anderen, en er is geen bedrijfs behoefte voor een geconsolideerde klant/afdeling/bedrijfs groep weer gave. ".

Wanneer u Windows-agents gebruikt om gegevens te verzamelen, kunt u [elke agent configureren om te rapporteren aan een of meer werkruimten](../../azure-monitor/platform/agent-windows.md).

Als u System Center Operations Manager gebruikt, kan elke beheergroep uit Operations Manager worden verbonden met slechts één werkruimte. U kunt Microsoft Monitoring Agent installeren op computers die worden beheerd door Operations Manager en de agent laten rapporteren over zowel Operations Manager als een andere Log Analytics-werkruimte.

Zodra de architectuur van de werk ruimte is gedefinieerd, moet u dit beleid afdwingen op Azure-resources met [Azure Policy](../../governance/policy/overview.md). Dit kan een ingebouwde definitie bevatten die automatisch wordt toegepast op alle Azure-resources. U kunt bijvoorbeeld een beleid instellen om ervoor te zorgen dat alle Azure-resources in een bepaalde regio al hun Diagnostische logboeken naar een bepaalde werk ruimte hebben verzonden.

## <a name="view-workspace-details"></a>Details van werk ruimte weer geven
Tijdens het analyseren van gegevens in uw Log Analytics-werk ruimte vanuit het menu **Azure monitor** in de Azure Portal, maakt en beheert u werk ruimten in het menu **log Analytics werk ruimten** .
 

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) en klik op **alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **log Analytics** -werk ruimten.  

    ![Azure Portal](media/manage-access/azure-portal-01.png)  

3. Selecteer uw werk ruimte in de lijst.

4. Op de pagina werk ruimte worden details over de werk ruimte weer gegeven, aan de slag, configuratie en koppelingen voor aanvullende informatie.  

    ![Details van de werkruimte](./media/manage-access/workspace-overview-page.png)  


## <a name="workspace-permissions-and-scope"></a>Werkruimte machtigingen en-bereik
De gegevens waartoe een gebruiker toegang heeft, worden bepaald door meerdere factoren die in de volgende tabel worden weer gegeven. Elk wordt beschreven in de volgende secties.

| Multi-factor Authentication | Description |
|:---|:---|
| [Toegangs modus](#access-modes) | De methode die de gebruiker gebruikt voor toegang tot de werk ruimte.  Hiermee definieert u het bereik van de beschik bare gegevens en de toegangs beheer modus die wordt toegepast. |
| [Toegangs beheer modus](#access-control-mode) | Instelling in de werk ruimte die definieert of machtigingen worden toegepast op het niveau van de werk ruimte of de resource. |
| [Machtigingen](#manage-accounts-and-users) | Machtigingen die worden toegepast op afzonderlijke of groepen gebruikers voor de werk ruimte of resource. Hiermee definieert u welke gegevens de gebruiker toegang heeft. |
| [RBAC op tabel niveau](#table-level-rbac) | Optionele gedetailleerde machtigingen die van toepassing zijn op alle gebruikers, ongeacht de toegangs modus of de toegangs beheer modus. Hiermee definieert u welke gegevens typen een gebruiker kan openen. |



## <a name="access-modes"></a>Toegangs modi
De _toegangs modus_ verwijst naar hoe een gebruiker toegang heeft tot een log Analytics-werk ruimte en definieert het bereik van de gegevens waartoe ze toegang hebben. 

**Werk ruimte-gericht**: In deze modus kan een gebruiker alle logboeken weer geven in de werk ruimte waarvoor ze machtigingen hebben. Query's in deze modus zijn gericht op alle gegevens in alle tabellen in de werk ruimte. Dit is de toegangs modus die wordt gebruikt wanneer logboeken worden geopend met de werk ruimte als het bereik, bijvoorbeeld **** wanneer u Logboeken selecteert in het **Azure monitor** menu in de Azure Portal.

**Resource gericht**: Wanneer u de werk ruimte voor een bepaalde resource opent, bijvoorbeeld wanneer u **Logboeken** selecteert in een resource menu in het Azure Portal, kunt u alleen logboeken voor die resource in alle tabellen weer geven waartoe u toegang hebt. Query's in deze modus zijn alleen van toepassing op gegevens die aan die resource zijn gekoppeld. In deze modus wordt ook granulaire op rollen gebaseerd toegangs beheer (RBAC) ingeschakeld. 

> [!NOTE]
> Logboeken zijn alleen beschikbaar voor resource gerichte query's als ze goed zijn gekoppeld aan de betreffende resource. Momenteel hebben de volgende resources beperkingen: 
> - Computers buiten Azure
> - Service Fabric
> - Application Insights
> - Containers
>
> U kunt testen of Logboeken goed zijn gekoppeld aan hun resource door een query uit te voeren en de records te controleren waarin u bent geïnteresseerd. Als de juiste resource-ID voor komt in de eigenschap [_ResourceId](log-standard-properties.md#_resourceid) , zijn de gegevens beschikbaar voor resource gerichte query's.

### <a name="comparing-access-modes"></a>De toegangs modi vergelijken

De volgende tabel bevat een overzicht van de toegangs modi:

| | Werk ruimte-gericht | Resource gericht |
|:---|:---|:---|
| Voor wie is elk model bedoeld? | Centraal beheer. Beheerders die gegevens verzameling en gebruikers moeten configureren die toegang nodig hebben tot een groot aantal verschillende bronnen. Dit is momenteel vereist voor gebruikers die toegang moeten hebben tot logboeken voor bronnen buiten Azure. | Toepassings teams. Beheerders van Azure-resources die worden bewaakt. |
| Wat heeft een gebruiker nodig om logboeken weer te geven? | Machtigingen voor de werk ruimte. Zie **machtigingen voor werk ruimten** in [accounts en gebruikers beheren](#manage-accounts-and-users). | Lees toegang tot de resource. Zie **resource machtigingen** in [accounts en gebruikers beheren](#manage-accounts-and-users). Machtigingen kunnen worden overgenomen (bijvoorbeeld van de container resource groep) of rechtstreeks worden toegewezen aan de resource. De machtigingen voor de logboeken voor de resource worden automatisch toegewezen. |
| Wat is het bereik van machtigingen? | Werk ruimte. Gebruikers met toegang tot de werk ruimte kunnen alle logboeken in die werk ruimte opvragen van tabellen waarvoor ze machtigingen hebben. Zie [Table Access Control](#table-level-rbac) | Azure-resource. De gebruiker kan Logboeken zoeken voor bronnen waartoe ze toegang hebben vanuit een wille keurige werk ruimte, maar kunnen geen logboeken voor andere resources zoeken. |
| Hoe kan de gebruiker toegang krijgen tot logboeken? | Start **Logboeken** vanuit **Azure Monitor** menu of **log Analytics werk ruimten**. | Start **Logboeken** vanuit het menu voor de Azure-resource. |


## <a name="access-control-mode"></a>Toegangs beheer modus
De _Access Control-modus_ is een instelling voor elke werk ruimte die definieert hoe machtigingen voor die werk ruimte worden bepaald.

**Werkruimte machtigingen vereisen**:  In deze controle modus is granulaire RBAC niet toegestaan. Een gebruiker heeft alleen toegang tot de werk ruimte als deze machtigingen voor de werk ruimte of specifieke tabellen hebben. 

Als een gebruiker de werk ruimte in de werk ruimte gecentreerde modus opent, hebben ze toegang tot alle tabellen waartoe ze toegang hebben gekregen. Als een gebruiker toegang heeft tot de werk ruimte in de resource-georiënteerde modus, hebben ze alleen toegang tot gegevens voor die resource in tabellen waartoe ze toegang hebben gekregen.

Dit is de standaard instelling voor alle werk ruimten die zijn gemaakt vóór 2019 maart.

**Resource-of werkruimte machtigingen gebruiken**: In deze besturings modus is granulaire RBAC toegestaan. Gebruikers krijgen alleen toegang tot gegevens die zijn gekoppeld aan resources die ze kunnen weer geven via Azure-machtigingen, resources waarvoor `read` ze zijn gemachtigd. 

Wanneer een gebruiker de werk ruimte in de werk ruimte-gecentreerde modus opent, zijn de werkruimte machtigingen van toepassing. Wanneer een gebruiker de werk ruimte in de resource-georiënteerde modus opent, worden alleen de resource machtigingen gecontroleerd en worden de machtigingen voor de werk ruimte genegeerd. Schakel RBAC voor een gebruiker in door deze uit de werkruimte machtigingen te verwijderen en de machtigingen van de resource te herkennen.

Dit is de standaard instelling voor alle werk ruimten die na maart 2019 zijn gemaakt.

> [!NOTE]
> Als een gebruiker alleen resource machtigingen heeft voor de werk ruimte, hebben ze alleen toegang tot de werk ruimte via de [resource-gecentreerde modus](#access-modes).


### <a name="define-access-control-mode-in-azure-portal"></a>Toegangs beheer modus definiëren in Azure Portal
U kunt de huidige toegangs beheer modus voor de werk ruimte weer geven op de pagina **overzicht** van de werk ruimte in het menu van de **log Analytics werk ruimte** .

![Toegangs beheer modus voor werk ruimte weer geven](media/manage-access/view-access-control-mode.png)

U kunt deze instelling wijzigen op de pagina **Eigenschappen** van de werk ruimte. Het wijzigen van de instelling wordt uitgeschakeld als u geen machtigingen hebt om de werk ruimte te configureren.

![Toegangs modus voor werk ruimte wijzigen](media/manage-access/change-access-control-mode.png)

### <a name="define-access-control-mode-in-powershell"></a>Toegangs beheer modus definiëren in Power shell

Gebruik de volgende opdracht om de toegangs beheer modus voor alle werk ruimten in het abonnement te controleren:

```powershell
Get-AzResource -ResourceType Microsoft.OperationalInsights/workspaces -ExpandProperties | foreach {$_.Name + ": " + $_.Properties.features.enableLogAccessUsingOnlyResourcePermissions} 
```

Gebruik het volgende script om de toegangs beheer modus voor een specifieke werk ruimte in te stellen:

```powershell
$WSName = "my-workspace"
$Workspace = Get-AzResource -Name $WSName -ExpandProperties
if ($Workspace.Properties.features.enableLogAccessUsingOnlyResourcePermissions -eq $null) 
    { $Workspace.Properties.features | Add-Member enableLogAccessUsingOnlyResourcePermissions $true -Force }
else 
    { $Workspace.Properties.features.enableLogAccessUsingOnlyResourcePermissions = $true }
Set-AzResource -ResourceId $Workspace.ResourceId -Properties $Workspace.Properties -Force
```

Gebruik het volgende script om de toegangs beheer modus voor alle werk ruimten in het abonnement in te stellen

```powershell
Get-AzResource -ResourceType Microsoft.OperationalInsights/workspaces -ExpandProperties | foreach {
if ($_.Properties.features.enableLogAccessUsingOnlyResourcePermissions -eq $null) 
    { $_.Properties.features | Add-Member enableLogAccessUsingOnlyResourcePermissions $true -Force }
else 
    { $_.Properties.features.enableLogAccessUsingOnlyResourcePermissions = $true }
Set-AzResource -ResourceId $_.ResourceId -Properties $_.Properties -Force
```

### <a name="define-access-mode-in-resource-manager-template"></a>De toegangs modus definiëren in de Resource Manager-sjabloon
Als u de toegangs modus in een Azure Resource Manager sjabloon wilt configureren, stelt u de **enableLogAccessUsingOnlyResourcePermissions** -functie vlag in de werk ruimte in op een van de volgende waarden.

- **Onwaar**: Stel de werk ruimte in op werk ruimte gerichte machtigingen. Dit is de standaard instelling als de vlag niet is ingesteld.
- **waar**: Stel de werk ruimte in op resource gerichte machtigingen.


## <a name="manage-accounts-and-users"></a>Accounts en gebruikers beheren
De machtigingen voor de werk ruimte die worden toegepast op een bepaalde gebruiker, worden gedefinieerd door de toegangs modus en de [toegangs beheer modus](#access-control-mode) van de werk ruimte. **Werkruimte machtigingen** worden toegepast wanneer een gebruiker toegang krijgt tot elke werk ruimte met behulp van **werk ruimte-** georiënteerd in de [werk ruimte gecentreerde modus](#access-modes). **Bron machtigingen** worden toegepast wanneer een gebruiker toegang heeft tot een werk ruimte met behulp van **resource-of werkruimte machtigingen** [toegangs beheer modus](#access-control-mode) met [resource-georiënteerde modus](#access-modes).

### <a name="workspace-permissions"></a>Werkruimte machtigingen
Elke werkruimte kunnen meerdere accounts worden gekoppeld, en elk account kan toegang hebben tot meerdere werkruimten. Toegang wordt beheerd [Azure op rollen gebaseerde toegang](../../role-based-access-control/role-assignments-portal.md). 


Voor de volgende activiteiten zijn ook Azure-machtigingen vereist:

||Bewerking |Azure-machtigingen nodig |Opmerkingen |
|-------|-------------------------|------|
| Bewakings oplossingen toevoegen en verwijderen | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Deze machtigingen moeten worden toegekend op het niveau van de resourcegroep of het abonnement. |
| De prijscategorie wijzigen | `Microsoft.OperationalInsights/workspaces/*/write` | |
| Gegevens weergeven op de tegels *Back-up* en *Site Recovery* | Beheerder/medebeheerder | Heeft toegang tot resources die zijn geïmplementeerd met behulp van het klassieke implementatiemodel |
| Een werkruimte maken in Azure Portal | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||
| Basis eigenschappen van werk ruimte weer geven en de Blade werk ruimte in de portal invoeren | `Microsoft.OperationalInsights/workspaces/read` ||
| Query logboeken met een wille keurige interface | `Microsoft.OperationalInsights/workspaces/query/read` ||
| Toegang tot alle logboek typen met behulp van query's | `Microsoft.OperationalInsights/workspaces/query/*/read` ||
| Toegang tot een specifieke logboek tabel | `Microsoft.OperationalInsights/workspaces/query/<table_name>/read` ||
| De werkruimte sleutels voor het verzenden van logboeken naar deze werk ruimte lezen | `Microsoft.OperationalInsights/workspaces/sharedKeys/action` ||



#### <a name="manage-access-to-log-analytics-workspace-using-azure-permissions"></a>Toegang tot Log Analytics werk ruimte beheren met Azure-machtigingen 
Volg de stappen in [Roltoewijzingen gebruiken voor het beheer van de toegang tot de resources van uw Azure-abonnement](../../role-based-access-control/role-assignments-portal.md) om toegang te verlenen tot de Log Analytics-werkruimte met behulp van Azure-machtigingen.

Azure heeft twee ingebouwde gebruikers rollen voor Log Analytics-werk ruimten:
- Lezer van Log Analytics
- Inzender van Log Analytics

Leden van de rol *Lezer van Log Analytics* kunnen:
- Alle controlegegevens weergeven en doorzoeken 
- Controlegegevens weergeven, inclusief de configuratie van Azure Diagnostics voor alle Azure-resources.

De rol Lezer van Log Analytics bevat de volgende Azure acties:

| Type    | Machtiging | Description |
| ------- | ---------- | ----------- |
| Bewerking | `*/read`   | De mogelijkheid om alle Azure-resources en resourceconfiguratie weer te geven. Omvat: <br> Status van de VM-extensie <br> Configuratie van Azure Diagnostics voor resources <br> Alle eigenschappen en instellingen van alle resources. <br> Voor werk ruimten kunnen volledige onbeperkte machtigingen de werk ruimte-instellingen lezen en query's uitvoeren op de gegevens. Bekijk meer gedetailleerde opties hierboven. |
| Action | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Afgeschaft, u hoeft ze niet aan gebruikers toe te wijzen. s |
| Action | `Microsoft.OperationalInsights/workspaces/search/action` | Afgeschaft, u hoeft ze niet aan gebruikers toe te wijzen. |
| Action | `Microsoft.Support/*` | Mogelijkheid ondersteuningsaanvragen te openen |
|Geen bewerking | `Microsoft.OperationalInsights/workspaces/sharedKeys/read` | Hiermee voorkomt u dat het lezen van de werkruimte sleutel die nodig is om te gebruiken van de gegevensverzameling-API en om agents te installeren. Hiermee voorkomt u dat de gebruiker van het toevoegen van nieuwe resources in de werkruimte |


Leden van de rol *Inzender van Log Analytics* kunnen:
- Alle controlegegevens lezen als lezer van Log Analytics  
- Automation-accounts maken en configureren  
- Beheeroplossingen toevoegen en verwijderen    
    > [!NOTE] 
    > Wilt u is de laatste twee acties uitvoeren, moet deze machtiging worden verleend op het niveau van de resource-groep of abonnement.  

- Opslagaccountsleutels lezen   
- Verzameling met logboeken uit Azure Storage configureren  
- De controle-instellingen voor Azure-resources bewerken, inclusief
  - De VM-extensie toevoegen aan virtuele machines
  - Azure Diagnostics configureren op alle Azure-resources

> [!NOTE] 
> U kunt de mogelijkheid om een VM-extensie toe te voegen aan een virtuele machine, gebruiken om volledige controle over een virtuele machine te krijgen.

De rol Inzender van Log Analytics bevat de volgende Azure acties:

| Machtiging | Description |
| ---------- | ----------- |
| `*/read`     | De mogelijkheid om alle Azure-resources en resourceconfiguratie weer te geven. Omvat: <br> Status van de VM-extensie <br> Configuratie van Azure Diagnostics voor resources <br> Alle eigenschappen en instellingen van alle resources. <br> Voor werk ruimten kunnen volledige onbeperkte machtigingen de werk ruimte-instellingen lezen en query's uitvoeren op de gegevens. Bekijk meer gedetailleerde opties hierboven. |
| `Microsoft.Automation/automationAccounts/*` | De mogelijkheid om Azure Automation-accounts te maken en te configureren, inclusief het toevoegen en bewerken van runbooks |
| `Microsoft.ClassicCompute/virtualMachines/extensions/*` <br> `Microsoft.Compute/virtualMachines/extensions/*` | VM-extensies toevoegen, bijwerken en verwijderen, met inbegrip van de Microsoft Monitoring Agent-extensie en de OMS Agent for Linux-extensie |
| `Microsoft.ClassicStorage/storageAccounts/listKeys/action` <br> `Microsoft.Storage/storageAccounts/listKeys/action` | Geef de opslagaccountsleutel weer. Vereist om Log Analytics te configureren om logboeken uit Azure-opslagaccounts te lezen |
| `Microsoft.Insights/alertRules/*` | Waarschuwingsregels toevoegen, bijwerken en verwijderen |
| `Microsoft.Insights/diagnosticSettings/*` | Diagnostische instellingen bij Azure-resources toevoegen, bijwerken en verwijderen |
| `Microsoft.OperationalInsights/*` | Configuratie voor Log Analytics-werkruimten toevoegen, bijwerken en verwijderen |
| `Microsoft.OperationsManagement/*` | Beheeroplossingen toevoegen en verwijderen |
| `Microsoft.Resources/deployments/*` | Maak en verwijder implementaties. Vereist voor het toevoegen en verwijderen van oplossingen, werkruimten en Automation-accounts |
| `Microsoft.Resources/subscriptions/resourcegroups/deployments/*` | Maak en verwijder implementaties. Vereist voor het toevoegen en verwijderen van oplossingen, werkruimten en Automation-accounts |

Als u gebruikers wilt toevoegen aan en verwijderen uit een gebruikersrol, moet u beschikken over machtigingen voor `Microsoft.Authorization/*/Delete` en `Microsoft.Authorization/*/Write`.

Gebruik deze rollen om gebruikers toegang te geven op verschillende niveaus:
- Abonnement: toegang tot alle werkruimten in het abonnement
- Resourcegroep: toegang tot alle werkruimten in de resourcegroep
- Resource: alleen toegang tot de opgegeven werkruimte

U moet toewijzingen op het niveau van de resource (werk ruimte) uitvoeren om nauw keurig toegangs beheer te garanderen.  Gebruik [aangepaste rollen](../../role-based-access-control/custom-roles.md) om rollen te maken met de specifieke machtigingen die nodig zijn.

### <a name="resource-permissions"></a>Resource machtigingen 
Wanneer gebruikers een query uitvoeren op Logboeken vanuit een werk ruimte met behulp van resource gerichte toegang, hebben ze de volgende machtigingen voor de resource:

| Machtiging | Description |
| ---------- | ----------- |
| `Microsoft.Insights/logs/<tableName>/read`<br><br>Voorbeelden:<br>`Microsoft.Insights/logs/*/read`<br>`Microsoft.Insights/logs/Heartbeat/read` | De mogelijkheid om alle logboek gegevens voor de resource weer te geven.  |
| `Microsoft.Insights/diagnosticSettings/write ` | De mogelijkheid om Diagnostische instellingen te configureren om Logboeken in te stellen voor deze bron. |

Deze machtiging wordt doorgaans verleend vanuit een rol die  _\*/Read of_ _\*_ machtigingen bevat, zoals de ingebouwde functie voor [lezer](../../role-based-access-control/built-in-roles.md#reader) en [Inzender](../../role-based-access-control/built-in-roles.md#contributor) . Houd er rekening mee dat aangepaste rollen die specifieke acties of speciale ingebouwde rollen bevatten, deze machtiging mogelijk niet bevatten.

Zie [definiëren per-tabel toegangs beheer](#table-level-rbac) hieronder als u een ander toegangs beheer voor verschillende tabellen wilt maken.


## <a name="table-level-rbac"></a>RBAC op tabel niveau
Met **RBAC op tabel niveau** kunt u naast de andere machtigingen meer nauw keurige controle geven aan gegevens in een log Analytics-werk ruimte. Met dit besturings element kunt u specifieke gegevens typen definiëren die alleen toegankelijk zijn voor een specifieke groep gebruikers.

U implementeert Table Access Control met [aangepaste Azure-rollen](../../role-based-access-control/custom-roles.md) voor het verlenen of weigeren van toegang tot specifieke [tabellen](../log-query/logs-structure.md) in de werk ruimte. Deze rollen worden toegepast op werk ruimten met werk ruimte gerichte of op resource gerichte [toegangs beheer modi](#access-control-mode) , ongeacht de [toegangs modus](#access-modes)van de gebruiker.

Maak een [aangepaste rol](../../role-based-access-control/custom-roles.md) met de volgende acties om toegang te definiëren tot Table Access Control.

- Als u toegang wilt verlenen aan een tabel, neemt u deze op in de sectie **acties** van de roldefinitie.
- Als u de toegang tot een tabel wilt weigeren, neemt u deze op in de sectie **intact** van de roldefinitie.
- Gebruik * om alle tabellen op te geven.

Als u bijvoorbeeld een rol wilt maken met toegang tot de tabellen _heartbeat_ en _AzureActivity_ , maakt u een aangepaste rol met behulp van de volgende acties:

```
"Actions":  [
              "Microsoft.OperationalInsights/workspaces/query/Heartbeat/read",
              "Microsoft.OperationalInsights/workspaces/query/AzureActivity/read"
  ],
```

Als u een rol wilt maken die alleen toegang heeft tot _Security Baseline Baseline_ en geen andere tabellen, maakt u een aangepaste rol met behulp van de volgende acties:

```
    "Actions":  [
        "Microsoft.OperationalInsights/workspaces/query/SecurityBaseline/read"
    ],
    "NotActions":  [
        "Microsoft.OperationalInsights/workspaces/query/*/read"
    ],
```

### <a name="custom-logs"></a>Aangepaste logboeken
 Aangepaste logboeken worden gemaakt door gegevens bronnen, zoals aangepaste logboeken en HTTP data collector API. De eenvoudigste manier om het type logboek te identificeren, is door de tabellen die worden vermeld onder [aangepaste Logboeken in het logboek schema](../log-query/get-started-portal.md#understand-the-schema)te controleren.

 U kunt momenteel geen toegang verlenen of weigeren voor afzonderlijke aangepaste logboeken, maar u hebt toegang tot alle aangepaste Logboeken. Als u een rol wilt maken met toegang tot alle aangepaste logboeken, maakt u een aangepaste rol met behulp van de volgende acties:

```
    "Actions":  [
        "Microsoft.OperationalInsights/workspaces/query/Tables.Custom/read"
    ],
```

### <a name="considerations"></a>Overwegingen

- Als een gebruiker algemene Lees machtigingen heeft met de Standard Reader-of Inzender rollen die de  _\*/Read_ -actie bevatten, wordt het toegangs beheer per tabel overschreven en hebben ze toegang tot alle logboek gegevens.
- Als een gebruiker toegang verleent per tabel, maar geen andere machtigingen heeft, zouden ze toegang kunnen krijgen tot logboek gegevens vanuit de API, maar niet van de Azure Portal. Als u toegang wilt krijgen tot de Azure Portal, gebruikt u Log Analytics Reader als basis functie.
- Beheerders voor het abonnement hebben toegang tot alle gegevens typen, ongeacht andere machtigings instellingen.
- Werkruimte eigenaren worden beschouwd als elke andere gebruiker voor toegangs beheer per tabel.
- U moet rollen toewijzen aan beveiligings groepen in plaats van afzonderlijke gebruikers om het aantal toewijzingen te verminderen. Hiermee kunt u ook bestaande hulpprogram ma's voor groeps beheer gebruiken om de toegang te configureren en te controleren.




## <a name="next-steps"></a>Volgende stappen
* Zie [Log Analytics-agent overzicht](../../azure-monitor/platform/log-analytics-agent.md) voor het verzamelen van gegevens van computers in uw datacenter of andere cloudomgeving.
* Zie [Gegevens verzamelen over Azure Virtual Machines](../../azure-monitor/learn/quick-collect-azurevm.md) voor het configureren van het verzamelen van gegevens van Azure VM's.  

