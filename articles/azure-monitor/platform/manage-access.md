---
title: Beheren van Log Analytics-werkruimten in Azure Monitor | Microsoft Docs
description: U kunt Log Analytics-werkruimten in Azure Monitor met behulp van verschillende beheertaken voor gebruikers, accounts, werkruimten en Azure-accounts beheren.
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
ms.date: 02/07/2019
ms.author: magoedte
ms.openlocfilehash: 6990bed4065183ecabb502ea90b5ddf26db563b4
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58500182"
---
# <a name="manage-log-data-and-workspaces-in-azure-monitor"></a>Logboekgegevens en toegang tot werkruimten in Azure Monitor beheren
Azure Monitor-winkels vastleggen gegevens in een Log Analytics-werkruimte is in wezen een container die gegevens en configuratie-informatie bevat. Voor het beheren van toegang tot gegevens vastleggen, kunt u verschillende beheertaken met betrekking tot werkruimten uitvoeren. U of andere leden van uw organisatie kunnen meerdere werkruimten gebruiken om verschillende gegevenssets te beheren die worden verzameld uit de gehele of delen van uw IT-infrastructuur.

In dit artikel wordt uitgelegd hoe u voor het beheren van toegang tot logboeken en voor het beheer van de werkruimten die ze bevatten. 

## <a name="create-a-workspace"></a>Een werkruimte maken
Voor het maken van een Log Analytics-werkruimte, moet u naar:

1. U dient een Azure-abonnement te hebben.
2. U dient een naam voor de werkruimte te kiezen.
3. De werkruimte koppelen aan een van uw abonnementen en resourcegroepen.
4. U dient een geografische locatie te kiezen.

Raadpleeg de volgende artikelen voor meer informatie over het maken van een werkruimte:

- [Een Log Analytics-werkruimte maken in Azure portal](../learn/quick-create-workspace.md)
- [Een Log Analytics-werkruimte maken met Azure CLI 2.0](../learn/quick-create-workspace-cli.md)
- [Een Log Analytics-werkruimte maken met Azure PowerShell](../learn/quick-create-workspace-posh.md)

## <a name="determine-the-number-of-workspaces-you-need"></a>Vaststellen hoeveel werkruimten u nodig hebt
Een Log Analytics-werkruimte is een Azure-resource en een container waaruit gegevens worden verzameld, samengevoegd, geanalyseerd en gepresenteerd in Azure Monitor. U kunt meerdere werkruimten per Azure-abonnement hebt en u toegang hebt tot meer dan één werkruimte, de mogelijkheid om eenvoudig query's uitvoeren voor deze. In deze sectie wordt beschreven wanneer het handig kan zijn om meer dan één werkruimte te maken.

Een Log Analytics-werkruimte biedt:

* Een geografische locatie voor de opslag van gegevens.
* Gegevensisolatie voor het definiëren van toegangsrechten voor verschillende gebruikers in de werkruimte-georiënteerde modus. Niet relevant als u werkt in de resource-georiënteerde modus.
* Bereik voor de configuratie van instellingen, zoals [prijscategorie](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#changing-pricing-tier), [retentie](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period) en [gegevens beperking](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#daily-cap).
* Kosten met betrekking tot de gegevensopname en retentie zijn aangebracht op de bron van de werkruimte.

Uit oogpunt van verbruik, wordt u aangeraden dat u werkruimten zo weinig mogelijk maakt. Het maakt beheer en query's eenvoudiger en sneller. Maar op basis van de voorgaande kenmerken, kunt u meerdere werkruimten maken:

* U bent een mondiaal bedrijf en u moet zich aanmelden voor gegevens die zijn opgeslagen in specifieke regio's voor gegevens soevereiniteit of om nalevingsredenen.
* U gebruikt Azure en wilt kosten voor de overdracht van uitgaande gegevens voorkomen door een werkruimte in dezelfde regio te hebben als de Azure-resource die deze beheert.
* U bent aanbieder van beheerde services en moet de Log Analytics-gegevens voor elke klant geïsoleerd van de gegevens van andere klanten bewaren.
* U beheert meerdere klanten en wilt dat elke klant / afdeling of bedrijfsgroep om te zien van hun eigen gegevens, maar niet gegevens uit andere resources, en er is geen bedrijfsbehoefte voor een geconsolideerde cross klant / afdeling of bedrijfsgroep weergeven. '.

Wanneer u Windows-agents gebruikt om gegevens te verzamelen, kunt u [elke agent configureren om te rapporteren aan een of meer werkruimten](../../azure-monitor/platform/agent-windows.md).

Als u System Center Operations Manager gebruikt, kan elke beheergroep uit Operations Manager worden verbonden met slechts één werkruimte. U kunt Microsoft Monitoring Agent installeren op computers die worden beheerd door Operations Manager en de agent laten rapporteren over zowel Operations Manager als een andere Log Analytics-werkruimte.

Als de architectuur van de werkruimte is gedefinieerd, moet u dit beleid op Azure-resources afdwingen [Azure Policy](../../governance/policy/overview.md). Dit biedt een ingebouwde definitie die zouden automatisch worden toegepast op alle Azure-resources. U kunt bijvoorbeeld een beleid om ervoor te zorgen dat al uw Azure-resources in een bepaalde regio hun diagnostische logboeken naar een bepaalde werkruimte verzonden instellen.

## <a name="view-workspace-details"></a>Details van de werkruimte weergeven
Terwijl u gegevens in uw Log Analytics-werkruimte van analyseren de **Azure Monitor** in het menu in de Azure-portal maken en beheren van toegang tot werkruimten in de **Log Analytics-werkruimten** menu.
 

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en klikt u op **alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics** werkruimten.  

    ![Azure Portal](media/manage-access/azure-portal-01.png)  

3. Selecteer in de lijst met uw werkruimte.

4. Pagina van de werkruimte geeft details weer over de werkruimte, aan de slag, configuratie en koppelingen voor meer informatie.  

    ![Details van de werkruimte](./media/manage-access/workspace-overview-page.png)  


## <a name="workspace-permissions-and-scope"></a>Machtigingen voor de werkruimte en het bereik
De gegevens die een gebruiker toegang tot heeft worden bepaald door meerdere factoren die in de volgende tabel worden vermeld. In de onderstaande secties worden beschreven.

| Multi-factor Authentication | Description |
|:---|:---|
| [Toegangsmodus](#access-modes) | Methode waarmee de gebruiker toegang heeft tot de werkruimte.  Definieert het bereik van de beschikbare gegevens en de modus voor het beheer van toegang dat wordt toegepast. |
| [Access control-modus](#access-control-mode) | Instellen in de werkruimte waarmee wordt gedefinieerd of machtigingen worden toegepast op het niveau van de werkruimte of resource. |
| [Machtigingen](#manage-accounts-and-users) | Machtigingen voor afzonderlijke of groepen gebruikers voor de werkruimte of resource. Hiermee definieert u welke gegevens de gebruiker toegang tot hebben. |
| [Tabelniveau RBAC](#table-level-rbac) | Optionele gedetailleerde machtigingen die van toepassing op alle gebruikers, ongeacht hun toegang of toegang besturingselement modus. Hiermee definieert u welke gegevenstypen die een gebruiker toegang heeft. |



## <a name="access-modes"></a>Toegangsmodi in
De _toegangsmodus_ verwijst naar hoe een gebruiker toegang heeft tot een Log Analytics-werkruimte en definieert u het bereik van de toegang te krijgen tot gegevens. 

**Workspace-centric**: In deze modus kan een gebruiker alle logboeken in de werkruimte die ze gemachtigd zijn om te bekijken. Query's in deze modus zijn gericht op alle gegevens in alle tabellen in de werkruimte. Dit is de access-modus gebruikt wanneer u zich aanmeldt met de werkruimte worden geopend als het bereik, zoals wanneer u selecteert **logboeken** uit de **Azure Monitor** in het menu in de Azure-portal.

**Resource-georiënteerde**: Wanneer u toegang krijgen tot de werkruimte voor een bepaalde resource, zoals wanneer u selecteert **logboeken** vanuit een resourcemenu in Azure portal, kunt u Logboeken voor alleen die resource in alle tabellen die u toegang tot hebt bekijken. Query's in deze modus zijn gericht op alleen de gegevens die zijn gekoppeld aan deze resource. In deze modus kunt ook gedetailleerde op rollen gebaseerd toegangsbeheer (RBAC). 

> [!NOTE]
> Logboeken zijn beschikbaar voor de resource-georiënteerde query's alleen als ze goed gekoppeld aan de betreffende resource zijn. Op dit moment hebben de volgende bronnen beperkingen: 
> - Computers buiten Azure
> - Service Fabric
> - Application Insights
> - Containers
> - Aangepaste logboeken die zijn gemaakt door HTTP-gegevensverzamelaar-API
>
> U kunt testen als Logboeken goed gekoppeld aan de resource zijn door een query uit te voeren en inspecteren van de records u geïnteresseerd bent in. Als de juiste resource-ID in de [_ResourceId](log-standard-properties.md#_resourceid) eigenschap en klik vervolgens gegevens is beschikbaar voor de resource-georiënteerde query's.

### <a name="comparing-access-modes"></a>Vergelijking van de toegangsmodi in

De volgende tabel geeft een overzicht van de toegangsmodi in:

| | Werkruimte-centraal | Resource-centraal |
|:---|:---|:---|
| Voor wie is elk model bedoeld? | Centrale beheersite. Beheerders moeten configureren voor het verzamelen van gegevens en gebruikers die toegang nodig tot een groot aantal bronnen. Ook nodig op dat moment voor gebruikers die hebben voor toegang tot logboeken voor resources buiten Azure. | Toepassing teams. Beheerders van Azure-resources die worden bewaakt. |
| Wat is een gebruiker nodig om logboeken weer te geven? | Machtigingen voor de werkruimte. Zie **werkruimtemachtigingen** in [accounts en gebruikers beheren](#manage-accounts-and-users). | Leestoegang tot de resource. Zie **machtigingen voor resources** in [accounts en gebruikers beheren](#manage-accounts-and-users). Machtigingen kunnen worden overgenomen (zoals de betreffende resourcegroep) of rechtstreeks toegewezen aan de resource. Machtiging voor de logboeken voor de resource wordt automatisch toegewezen. |
| Wat is het bereik van machtigingen? | Werkruimte. Gebruikers met toegang tot de werkruimte kunnen alle logboeken in deze werkruimte uit tabellen die ze gemachtigd zijn om te zoeken. Zie [tabel toegangsbeheer](#table-level-rbac) | Azure resource. Gebruikers logboeken kan opvragen voor bronnen ze hebben toegang tot vanuit elke werkruimte, maar kan geen query uitvoeren voor logbestanden voor andere bronnen. |
| Hoe kan een gebruiker toegang tot logboeken? | Start **logboeken** van **Azure Monitor** menu of **Log Analytics-werkruimten**. | Start **logboeken** in het menu voor de Azure-resource. |


## <a name="access-control-mode"></a>Modus toegangsbeheer
De _besturingselement toegangsmodus_ is een instelling op elk werkruimten die definieert hoe machtigingen voor deze werkruimte worden bepaald.

**Machtigingen voor de werkruimte vereisen**:  In deze modus controle is niet toegestaan voor gedetailleerde RBAC. Voor een gebruiker voor toegang tot de werkruimte, moeten deze machtigingen aan de werkruimte of specifieke tabellen worden verleend. 

Als een gebruiker toegang heeft tot de werkruimte in de werkruimte-georiënteerde modus, wordt ze toegang tot alle gegevens die ze toegang tot gekregen hebben tabellen hebben. Als een gebruiker toegang heeft tot de werkruimte in de resource-georiënteerde modus, hebben ze toegang tot alleen gegevens voor die bron in tabellen die toegang tot hebt gekregen.

Dit is de standaardinstelling voor alle werkruimten die zijn gemaakt vóór maart 2019.

**Gebruik resource of werkruimte machtigingen**: In deze modus controle kunt gedetailleerde RBAC. Gebruikers worden krijgen toegang tot alleen de gegevens die zijn gekoppeld aan resources die ze kunnen bekijken via de Azure-machtigingen, resources waarvoor ze hebben `read` machtiging. 

Wanneer een gebruiker toegang heeft tot de werkruimte in de werkruimte-georiënteerde modus, gelden de volgende machtigingen voor de werkruimte. Wanneer een gebruiker toegang heeft tot de werkruimte in de resource-georiënteerde modus, alleen machtigingen voor resources moeten worden gecontroleerd en machtigingen voor de werkruimte worden genegeerd. RBAC voor een gebruiker door te verwijderen van machtigingen voor de werkruimte en hun machtigingen voor resources om te worden erkend inschakelen.

Dit is de standaardinstelling voor alle werkruimten die zijn gemaakt na maart 2019.

> [!NOTE]
> Als een gebruiker alleen machtigingen voor resources in de werkruimte, ze worden pas toegang krijgen tot de werkruimte met behulp van [Resource-georiënteerde modus](#access-modes).


### <a name="define-access-control-mode-in-azure-portal"></a>Besturingselement toegangsmodus definiëren in Azure portal
U kunt de huidige werkruimte access control-modus weergeven op de **overzicht** -pagina voor de werkruimte in de **Log Analytics-werkruimte** menu.

![Weergavemodus werkruimte access control](media/manage-access/view-access-control-mode.png)

U kunt deze instelling wijzigen op de **eigenschappen** pagina voor de werkruimte. Als u de instelling wijzigt worden, uitgeschakeld als u geen machtigingen voor het configureren van de werkruimte.

![Toegangsmodus voor werkruimte wijzigen](media/manage-access/change-access-control-mode.png)

### <a name="define-access-control-mode-in-powershell"></a>Besturingselement toegangsmodus definiëren in PowerShell

Gebruik de volgende opdracht om te controleren van het besturingselement toegangsmodus voor alle werkruimten in het abonnement:

```powershell
Get-AzResource -ResourceType Microsoft.OperationalInsights/workspaces -ExpandProperties | foreach {$_.Name + ": " + $_.Properties.features.enableLogAccessUsingOnlyResourcePermissions} 
```

Het volgende script gebruiken om in te stellen de controle-toegangsmodus voor een specifieke werkruimte:

```powershell
$WSName = "my-workspace"
$Workspace = Get-AzResource -Name $WSName -ExpandProperties
if ($Workspace.Properties.features.enableLogAccessUsingOnlyResourcePermissions -eq $null) 
    { $Workspace.Properties.features | Add-Member enableLogAccessUsingOnlyResourcePermissions $true -Force }
else 
    { $Workspace.Properties.features.enableLogAccessUsingOnlyResourcePermissions = $true }
Set-AzResource -ResourceId $Workspace.ResourceId -Properties $Workspace.Properties -Force
```

Het volgende script gebruiken om in te stellen van de modus voor het beheer van toegang voor alle werkruimten in het abonnement

```powershell
Get-AzResource -ResourceType Microsoft.OperationalInsights/workspaces -ExpandProperties | foreach {
if ($_.Properties.features.enableLogAccessUsingOnlyResourcePermissions -eq $null) 
    { $_.Properties.features | Add-Member enableLogAccessUsingOnlyResourcePermissions $true -Force }
else 
    { $_.Properties.features.enableLogAccessUsingOnlyResourcePermissions = $true }
Set-AzResource -ResourceId $_.ResourceId -Properties $_.Properties -Force
```

### <a name="define-access-mode-in-resource-manager-template"></a>Toegangsmodus definiëren in Resource Manager-sjabloon
Voor het configureren van de toegangsmodus voor in een Azure Resource Manager-sjabloon, stel de **enableLogAccessUsingOnlyResourcePermissions** functie vlag in de werkruimte aan een van de volgende waarden.

- **De waarde False**: De werkruimte aan werkruimte gerichte machtigingen instellen. Dit is de standaardinstelling als de vlag is niet ingesteld.
- **true**: Stel de werkruimte op resource-georiënteerde machtigingen.


## <a name="manage-accounts-and-users"></a>Accounts en gebruikers beheren
De machtigingen voor de werkruimte die worden toegepast op een bepaalde gebruiker zijn gedefinieerd door de toegangsmodus en de [besturingselement toegangsmodus](#access-control-mode) van de werkruimte. **Machtigingen voor de werkruimte** worden toegepast wanneer een gebruiker toegang heeft tot een werkruimte met behulp van **werkruimte gerichte** in [werkruimte gerichte modus](#access-modes). **Machtigingen voor resources** worden toegepast wanneer een gebruiker toegang heeft tot een werkruimte met **resource of werkruimte machtigingen gebruiken** [besturingselement toegangsmodus](#access-control-mode) met behulp van [resource-georiënteerde modus ](#access-modes).

### <a name="workspace-permissions"></a>Machtigingen voor de werkruimte
Elke werkruimte kunnen meerdere accounts worden gekoppeld, en elk account kan toegang hebben tot meerdere werkruimten. Toegang wordt beheerd [Azure op rollen gebaseerde toegang](../../role-based-access-control/role-assignments-portal.md). 


Voor de volgende activiteiten zijn ook Azure-machtigingen vereist:

| Bewerking                                                          | Azure-machtigingen nodig | Opmerkingen |
|-----------------------------------------------------------------|--------------------------|-------|
| Toevoegen en verwijderen van bewakingsoplossingen                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Deze machtigingen moeten worden toegekend op het niveau van de resourcegroep of het abonnement. |
| De prijscategorie wijzigen                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| Gegevens weergeven op de tegels *Back-up* en *Site Recovery* | Beheerder/medebeheerder | Heeft toegang tot resources die zijn geïmplementeerd met behulp van het klassieke implementatiemodel |
| Een werkruimte maken in Azure Portal                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


#### <a name="manage-access-to-log-analytics-workspace-using-azure-permissions"></a>Toegang tot Log Analytics-werkruimte met behulp van Azure-machtigingen beheren 
Volg de stappen in [Roltoewijzingen gebruiken voor het beheer van de toegang tot de resources van uw Azure-abonnement](../../role-based-access-control/role-assignments-portal.md) om toegang te verlenen tot de Log Analytics-werkruimte met behulp van Azure-machtigingen.

Azure heeft twee ingebouwde gebruikersrollen voor Log Analytics-werkruimten:
- Lezer van Log Analytics
- Inzender van Log Analytics

Leden van de rol *Lezer van Log Analytics* kunnen:
- Alle controlegegevens weergeven en doorzoeken 
- Controlegegevens weergeven, inclusief de configuratie van Azure Diagnostics voor alle Azure-resources.

De rol Lezer van Log Analytics bevat de volgende Azure acties:

| Type    | Machtiging | Description |
| ------- | ---------- | ----------- |
| Bewerking | `*/read`   | De mogelijkheid om alle Azure-resources en resourceconfiguratie weer te geven. Omvat: <br> Status van de VM-extensie <br> Configuratie van Azure Diagnostics voor resources <br> Alle eigenschappen en instellingen van alle resources |
| Bewerking | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Mogelijkheid om Zoeken in logboeken v2-query's uit te voeren |
| Bewerking | `Microsoft.OperationalInsights/workspaces/search/action` | Mogelijkheid om Zoeken in logboeken v1-query's uit te voeren |
| Bewerking | `Microsoft.Support/*` | Mogelijkheid ondersteuningsaanvragen te openen |
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
| `*/read`     | De mogelijkheid om alle resources en de resourceconfiguratie weer te geven. Omvat: <br> Status van de VM-extensie <br> Configuratie van Azure Diagnostics voor resources <br> Alle eigenschappen en instellingen van alle resources |
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

U kunt toewijzingen op het resourceniveau van de (werkruimte) om te verzekeren dat nauwkeurig toegangsbeheer moet uitvoeren.  Gebruik [aangepaste rollen](../../role-based-access-control/custom-roles.md) om rollen te maken met de specifieke machtigingen die nodig zijn.

### <a name="resource-permissions"></a>Machtigingen voor resources 
Als gebruikers query zich vanaf een werkruimte met behulp van resource-georiënteerde toegang aanmeldt, wordt ze de volgende machtigingen hebben op de resource:

| Machtiging | Description |
| ---------- | ----------- |
| `Microsoft.Insights/logs/<tableName>/read`<br><br>Voorbeelden:<br>`Microsoft.Insights/logs/*/read`<br>`Microsoft.Insights/logs/Heartbeat/read` | De mogelijkheid om alle gegevens voor de resource weer te geven.  |


Deze machtiging wordt meestal van een rol met verleend  _\*/lezen of_ _\*_ machtigingen, zoals de ingebouwde [lezer](../../role-based-access-control/built-in-roles.md#reader) en [ Inzender](../../role-based-access-control/built-in-roles.md#contributor) rollen. Houd er rekening mee dat aangepaste rollen met specifieke acties of toegewezen ingebouwde rollen niet advies bij deze machtiging inwinnen.

Zie [definiëren per tabel toegangsbeheer](#table-level-rbac) hieronder als u wilt maken van verschillende toegangsbeheer voor verschillende tabellen.


## <a name="table-level-rbac"></a>Tabelniveau RBAC
**Het niveau van RBAC tabel** kunt u meer gedetailleerde controle met gegevens in een Log Analytics-werkruimte naast de andere machtigingen bieden. Dit besturingselement kunt u specifieke gegevenstypen die alleen toegankelijk voor een specifieke set gebruikers zijn definiëren.

Implementeren van toegangsbeheer tabel met [Azure aangepaste rollen](../../role-based-access-control/custom-roles.md) verlenen of weigeren van toegang tot specifieke [tabellen](../log-query/log-query-overview.md#how-azure-monitor-log-data-is-organized) in de werkruimte. Deze rollen worden toegepast op werkruimten met werkruimte gericht of resource-georiënteerde [toegang tot de control-modi](#access-control-mode) ongeacht van de gebruiker [toegangsmodus](#access-modes).

Maak een [aangepaste rol](../../role-based-access-control/custom-roles.md) met de volgende acties te definiëren de toegang tot tabel toegangsbeheer.

- Om toegang te verlenen aan een tabel, opnemen in de **acties** sectie van de roldefinitie.
- Voor het weigeren van toegang tot een tabel, opnemen in de **NotActions** sectie van de roldefinitie.
- Gebruik * om op te geven van alle tabellen.

Als u bijvoorbeeld een rol maken met toegang tot de _Heartbeat_ en _AzureActivity_ tabellen, maakt u een aangepaste rol die met behulp van de volgende acties:

```
"Actions":  [
              "Microsoft.OperationalInsights/workspaces/query/Heartbeat/read",
              "Microsoft.OperationalInsights/workspaces/query/AzureActivity/read"
  ],
```

Een rol maken met toegang tot enige _SecurityBaseline_ en geen andere tabellen, maakt u een aangepaste rol die met behulp van de volgende acties:

```
    "Actions":  [
        "Microsoft.OperationalInsights/workspaces/query/*/read"
    ],
    "NotActions":  [
        "Microsoft.OperationalInsights/workspaces/query/SecurityBaseline/read"
    ],
```

### <a name="custom-logs"></a>Aangepaste logboeken
 Aangepaste logboeken worden gemaakt door gegevensbronnen, zoals aangepaste logboeken en HTTP Data Collector-API. De eenvoudigste manier om te identificeren van het type van het logboek is door het controleren van de tabellen die worden vermeld onder [aangepaste logboeken in het logboek schema](../log-query/get-started-portal.md#understand-the-schema).

 U kunt geen op dit moment verlenen of weigeren van toegang tot afzonderlijke aangepaste logboeken, maar u kunt verlenen of weigeren van toegang tot alle aangepaste logboeken. Voor het maken van een rol met toegang tot alle aangepaste logboeken, maakt u een aangepaste rol die met behulp van de volgende acties:

```
    "Actions":  [
        "Microsoft.OperationalInsights/workspaces/query/Tables.Custom/read"
    ],
```

### <a name="considerations"></a>Overwegingen

- Als een gebruiker is verleend globale leesmachtiging hebt met de standard-lezer of Inzender-rollen die de  _\*/lezen_ actie, overschrijft het per tabel access control en geven ze toegang tot alle logboekgegevens.
- Als een gebruiker is verleend per tabel toegang, maar er zijn geen andere machtigingen, is ze zou kunnen toegang krijgen tot logboekgegevens van de API, maar niet vanuit de Azure portal. Voor toegang tot de Azure-portal, lezer van Log Analytics te gebruiken als de basis-rol.
- Beheerders voor het abonnement heeft toegang tot alle gegevenstypen, ongeacht eventuele andere machtigingsinstellingen.
- Eigenaren van de werkruimte worden behandeld als voor andere gebruikers voor toegangsbeheer per tabel.
- U moet de rollen toewijzen aan beveiligingsgroepen in plaats van afzonderlijke gebruikers te verminderen van het aantal toewijzingen. Dit helpt u ook bestaande groep beheerhulpprogramma's gebruiken om te configureren en controleren of de toegang.




## <a name="next-steps"></a>Volgende stappen
* Zie [Log Analytics-agent overzicht](../../azure-monitor/platform/log-analytics-agent.md) voor het verzamelen van gegevens van computers in uw datacenter of andere cloudomgeving.
* Zie [Gegevens verzamelen over Azure Virtual Machines](../../azure-monitor/learn/quick-collect-azurevm.md) voor het configureren van het verzamelen van gegevens van Azure VM's.  

