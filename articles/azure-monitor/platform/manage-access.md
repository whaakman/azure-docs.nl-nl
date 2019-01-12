---
title: De werkruimten in Azure Log Analytics en de OMS-portal beheren | Microsoft Docs
description: U kunt de toegang tot werkruimten in Azure Log Analytics en de OMS-portal beheren met behulp van verschillende beheertaken voor gebruikers, accounts, werkruimten en Azure-accounts.
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
ms.date: 11/13/2018
ms.author: magoedte
ms.openlocfilehash: 6c8f48ce71e11d1de0c28b4dab5327ab03e54f28
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54231781"
---
# <a name="manage-workspaces"></a>Werkruimten beheren

Voor het beheren van toegang tot Log Analytics, voert u verschillende beheertaken uit voor werkruimten. In dit artikel biedt adviezen en procedures voor het beheren van werkruimten. Een werkruimte is in wezen een container met accountgegevens en eenvoudige configuratiegegevens voor het account. U of andere leden van uw organisatie kunnen meerdere werkruimten gebruiken om verschillende gegevenssets te beheren die worden verzameld uit de gehele of delen van uw IT-infrastructuur.

Het volgende is nodig om een werkruimte te maken:

1. U dient een Azure-abonnement te hebben.
2. U dient een naam voor de werkruimte te kiezen.
3. De werkruimte koppelen aan een van uw abonnementen en resourcegroepen.
4. U dient een geografische locatie te kiezen.

## <a name="determine-the-number-of-workspaces-you-need"></a>Vaststellen hoeveel werkruimten u nodig hebt
Een werkruimte is een Azure-resource die bestaat uit een container waarin gegevens worden verzameld, samengevoegd, geanalyseerd en gepresenteerd in Azure Portal.

U kunt per Azure-abonnement meerdere werkruimten hebben en toegang hebben tot meer dan één werkruimte, met de mogelijkheid om op verschillende werkruimten tegelijk een query uit te voeren. In deze sectie wordt beschreven wanneer het handig kan zijn om meer dan één werkruimte te maken.

Een werkruimte biedt momenteel het volgende:

* Een geografische locatie voor de opslag van gegevens
* Gegevensisolatie toegangsrechten voor verschillende gebruikers definiëren
* Bereik voor de configuratie van instellingen, zoals [prijscategorie](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/manage-cost-storage#changing-pricing-tier), [retentie](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period) en [gegevens beperking](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/manage-cost-storage#daily-cap) 

Uit oogpunt van verbruik, wordt u aangeraden dat u werkruimten zo weinig mogelijk maakt. Het maakt beheer en query's eenvoudiger en sneller. Maar op basis van de voorgaande kenmerken, kunt u meerdere werkruimten maken:

* U vertegenwoordigt een mondiaal bedrijf en moet de gegevens opslaan in specifieke regio’s ten behoeve van de onafhankelijkheid van de gegevens of om nalevingsredenen.
* U gebruikt Azure en wilt kosten voor de overdracht van uitgaande gegevens voorkomen door een werkruimte in dezelfde regio te hebben als de Azure-resource die deze beheert.
* U wilt kosten toewijzen aan verschillende afdelingen of bedrijfsonderdelen op basis van hun gebruik door het maken van een werkruimte voor elke afdeling of bedrijfsgroep in een eigen Azure-abonnement.
* U bent aanbieder van beheerde services en moet de Log Analytics-gegevens voor elke klant geïsoleerd van de gegevens van andere klanten bewaren.
* U beheert meerdere klanten en wilt dat elke klant / afdeling of bedrijfsgroep om te zien van hun eigen gegevens, maar niet de gegevens van anderen.

Wanneer u Windows-agents gebruikt om gegevens te verzamelen, kunt u [elke agent configureren om te rapporteren aan een of meer werkruimten](../../azure-monitor/platform/agent-windows.md).

Als u System Center Operations Manager gebruikt, kan elke beheergroep uit Operations Manager worden verbonden met slechts één werkruimte. U kunt Microsoft Monitoring Agent installeren op computers die worden beheerd door Operations Manager en de agent laten rapporteren over zowel Operations Manager als een andere Log Analytics-werkruimte.

## <a name="workspace-information"></a>Werkruimtegegevens

U kunt gegevens van uw werkruimte in Azure Portal bekijken. 

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) als u dat nog niet hebt gedaan.

2. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.  

    ![Azure Portal](media/manage-access/azure-portal-01.png)  

3. Selecteer in het deelvenster voor abonnementen van Log Analytics een werkruimte.

4. Pagina van de werkruimte geeft details weer over de aan de slag, configuratie en koppelingen voor meer informatie.  

    ![Details van de werkruimte](./media/manage-access/workspace-overview-page.png)  

## <a name="manage-accounts-and-users"></a>Accounts en gebruikers beheren
Elke werkruimte kunnen meerdere accounts worden gekoppeld, en elk account kan toegang hebben tot meerdere werkruimten. Toegang wordt beheerd [Azure op rollen gebaseerde toegang](../../role-based-access-control/role-assignments-portal.md). Deze toegangsrechten is van toepassing op de Azure-portal en de API-toegang.


Voor de volgende activiteiten zijn ook Azure-machtigingen vereist:

| Bewerking                                                          | Azure-machtigingen nodig | Opmerkingen |
|-----------------------------------------------------------------|--------------------------|-------|
| Beheeroplossingen toevoegen en verwijderen                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Deze machtigingen moeten worden toegekend op het niveau van de resourcegroep of het abonnement. |
| De prijscategorie wijzigen                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| Gegevens weergeven op de tegels *Back-up* en *Site Recovery* | Beheerder/medebeheerder | Heeft toegang tot resources die zijn geïmplementeerd met behulp van het klassieke implementatiemodel |
| Een werkruimte maken in Azure Portal                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


### <a name="managing-access-to-log-analytics-using-azure-permissions"></a>Toegang tot Log Analytics beheren met behulp van Azure-machtigingen
Volg de stappen in [Roltoewijzingen gebruiken voor het beheer van de toegang tot de resources van uw Azure-abonnement](../../role-based-access-control/role-assignments-portal.md) om toegang te verlenen tot de Log Analytics-werkruimte met behulp van Azure-machtigingen.

Azure heeft twee ingebouwde gebruikersrollen voor Log Analytics:
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

Het wordt aangeraden om toewijzingen uit te voeren op resourceniveau (werkruimte) omdat dan een nauwkeurig toegangsbeheer mogelijk is.  Gebruik [aangepaste rollen](../../role-based-access-control/custom-roles.md) om rollen te maken met de specifieke machtigingen die nodig zijn.

## <a name="next-steps"></a>Volgende stappen
* Zie [Log Analytics-agent overzicht](../../azure-monitor/platform/log-analytics-agent.md) voor het verzamelen van gegevens van computers in uw datacenter of andere cloudomgeving.
* Zie [Gegevens verzamelen over Azure Virtual Machines](../../azure-monitor/learn/quick-collect-azurevm.md) voor het configureren van het verzamelen van gegevens van Azure VM's.  
* [Log Analytics-oplossingen uit de galerie met oplossingen toevoegen](../../azure-monitor/insights/solutions.md) om functionaliteit toe te voegen en gegevens te verzamelen.

