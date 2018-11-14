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
ms.devlang: na
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: de464cfeca01e492139e8bf9679d8f9876eedda6
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51625620"
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
* Bereik voor de configuratie van instellingen, zoals bewaren en gegevens beperking

Uit oogpunt van verbruik, wordt u aangeraden dat u werkruimten zo weinig mogelijk maakt. Het maakt beheer en query's eenvoudiger en sneller. Maar op basis van de voorgaande kenmerken, kunt u meerdere werkruimten maken:

* U vertegenwoordigt een mondiaal bedrijf en moet de gegevens opslaan in specifieke regio’s ten behoeve van de onafhankelijkheid van de gegevens of om nalevingsredenen.
* U gebruikt Azure en wilt kosten voor de overdracht van uitgaande gegevens voorkomen door een werkruimte in dezelfde regio te hebben als de Azure-resource die deze beheert.
* U wilt kosten toewijzen aan verschillende afdelingen of bedrijfsonderdelen op basis van hun gebruik door het maken van een werkruimte voor elke afdeling of bedrijfsgroep in een eigen Azure-abonnement.
* U bent aanbieder van beheerde services en moet de Log Analytics-gegevens voor elke klant geïsoleerd van de gegevens van andere klanten bewaren.
* U beheert meerdere klanten en wilt dat elke klant / afdeling of bedrijfsgroep om te zien van hun eigen gegevens, maar niet de gegevens van anderen.

Wanneer u Windows-agents gebruikt om gegevens te verzamelen, kunt u [elke agent configureren om te rapporteren aan een of meer werkruimten](log-analytics-agent-windows.md).

Als u System Center Operations Manager gebruikt, kan elke beheergroep uit Operations Manager worden verbonden met slechts één werkruimte. U kunt Microsoft Monitoring Agent installeren op computers die worden beheerd door Operations Manager en de agent laten rapporteren over zowel Operations Manager als een andere Log Analytics-werkruimte.

## <a name="workspace-information"></a>Werkruimtegegevens

U kunt gegevens van uw werkruimte in Azure Portal bekijken. 

1. Meld u aan bij de [Azure-portal](https://portal.azure.com) als u dat nog niet hebt gedaan.

2. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.  

    ![Azure Portal](media/log-analytics-manage-access/azure-portal-01.png)  

3. Selecteer in het deelvenster voor abonnementen van Log Analytics een werkruimte.

4. Pagina van de werkruimte geeft details weer over de aan de slag, configuratie en koppelingen voor meer informatie.  

    ![Details van de werkruimte](./media/log-analytics-manage-access/workspace-overview-page.png)  

## <a name="manage-accounts-and-users"></a>Accounts en gebruikers beheren
Elke werkruimte kunnen meerdere accounts worden gekoppeld, en elk account kan toegang hebben tot meerdere werkruimten. Toegang wordt beheerd [Azure op rollen gebaseerde toegang](../role-based-access-control/role-assignments-portal.md). Deze toegangsrechten is van toepassing op de Azure-portal en de API-toegang.


Voor de volgende activiteiten zijn ook Azure-machtigingen vereist:

| Bewerking                                                          | Azure-machtigingen nodig | Opmerkingen |
|-----------------------------------------------------------------|--------------------------|-------|
| Beheeroplossingen toevoegen en verwijderen                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Deze machtigingen moeten worden toegekend op het niveau van de resourcegroep of het abonnement. |
| De prijscategorie wijzigen                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| Gegevens weergeven op de tegels *Back-up* en *Site Recovery* | Beheerder/medebeheerder | Heeft toegang tot resources die zijn geïmplementeerd met behulp van het klassieke implementatiemodel |
| Een werkruimte maken in Azure Portal                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


### <a name="managing-access-to-log-analytics-using-azure-permissions"></a>Toegang tot Log Analytics beheren met behulp van Azure-machtigingen
Volg de stappen in [Roltoewijzingen gebruiken voor het beheer van de toegang tot de resources van uw Azure-abonnement](../role-based-access-control/role-assignments-portal.md) om toegang te verlenen tot de Log Analytics-werkruimte met behulp van Azure-machtigingen.

Azure heeft twee ingebouwde gebruikersrollen voor Log Analytics:
- Lezer van Log Analytics
- Inzender van Log Analytics

Leden van de rol *Lezer van Log Analytics* kunnen:
- Alle controlegegevens weergeven en doorzoeken 
- Controlegegevens weergeven, inclusief de configuratie van Azure Diagnostics voor alle Azure-resources.

De rol Lezer van Log Analytics bevat de volgende Azure acties:

| Type    | Machtiging | Beschrijving |
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

| Machtiging | Beschrijving |
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

Het wordt aangeraden om toewijzingen uit te voeren op resourceniveau (werkruimte) omdat dan een nauwkeurig toegangsbeheer mogelijk is.  Gebruik [aangepaste rollen](../role-based-access-control/custom-roles.md) om rollen te maken met de specifieke machtigingen die nodig zijn.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Een bestaande werkruimte koppelen aan een Azure-abonnement
Alle werkruimten die zijn gemaakt na 26 september 2016 moeten op het moment van maken zijn gekoppeld aan een Azure-abonnement. Werkruimten die vóór deze datum zijn gemaakt, moeten bij aanmelding worden gekoppeld aan een werkruimte. Wanneer u de werkruimte maakt via Azure Portal of uw werkruimte koppelt aan een Azure-abonnement, wordt uw Azure Active Directory als uw organisatieaccount gekoppeld.

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Een werkruimte in de Azure Portal koppelen aan een Azure-abonnement
1. Klik in Azure Portal op **Alle services**. Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Log Analytics**.  

2. Klik in het deelvenster voor abonnementen van Log Analytics op **toevoegen**.  

    ![lijst met werkruimten](./media/log-analytics-manage-access/workspace-link-existing-01.png)

3. Uit de **Log Analytics-werkruimte** deelvenster, klikt u op **bestaande koppelen**.  

4. Klik op **Vereiste instellingen configureren**.  

5. Hier ziet u de lijst met werkruimten die nog niet zijn gekoppeld aan uw Azure-account. Selecteer de werkruimte.  
   
6. Indien nodig kunt u de waarden van de volgende items wijzigen:
   * Abonnement
   * Resourcegroep
   * Locatie
   * Prijscategorie  

7. Klik op **OK**. De werkruimte is nu gekoppeld aan uw Azure-account.

> [!NOTE]
> Als u de te koppelen werkruimte niet ziet, heeft uw Azure-abonnement geen toegang tot de werkruimte die u hebt gemaakt met behulp van de OMS-portal.  Zie [Een gebruiker toevoegen aan een bestaande werkruimte](#add-a-user-to-an-existing-workspace) om dit account toegang te verlenen vanuit de OMS-portal.
>
>

## <a name="upgrade-a-workspace-to-a-paid-plan"></a>Een werkruimte upgraden naar een betaald abonnement
Er zijn drie typen werkruimteabonnementen voor OMS: **Gratis**, **Zelfstandig** en **OMS**.  Als u het *gratis* abonnement hebt, geldt een limiet van 500 MB aan gegevens dat per dag naar Log Analytics wordt verzonden.  Als u deze hoeveelheid overschrijdt, moet u uw werkruimte wijzigen naar een betaald abonnement om te voorkomen dat gegevens buiten deze limiet niet worden verzameld. U kunt op elk gewenst moment uw type abonnement wijzigen.  Zie [Prijsgegevens](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-pricing) voor meer informatie over de tarieven voor OMS.

### <a name="using-entitlements-from-an-oms-subscription"></a>Rechten van een OMS-abonnement gebruiken
Als u de rechten wilt gebruiken die horen bij de aanschaf van OMS E1, OMS E2 OMS of de OMS-invoegtoepassing voor System Center, kiest u het *OMS*-abonnement van OMS Log Analytics.

Wanneer u een OMS-abonnement koopt, worden de rechten toegevoegd aan uw Enterprise-overeenkomst. Elk Azure-abonnement dat onder deze overeenkomst wordt gemaakt, kan gebruikmaken van deze rechten. Alle werkruimten in deze abonnementen gebruiken de OMS-rechten.

Doe het volgende om ervoor te zorgen dat gebruik van een werkruimte wordt toegepast op uw rechten voor het OMS-abonnement:

1. Maak uw werkruimte in een Azure-abonnement dat deel uitmaakt van de Enterprise-overeenkomst die het OMS-abonnement omvat

2. Selecteer het *OMS*-abonnement voor de werkruimte

> [!NOTE]
> Als uw werkruimte is gemaakt vóór 26 september 2016 en uw Log Analytics-abonnement *Premium* is, maakt deze werkruimte gebruik van de rechten van de OMS-invoegtoepassing voor System Center. U kunt uw rechten ook gebruiken door over te schakelen naar de *OMS*-prijscategorie.
>
>

De rechten van de OMS-abonnement zijn niet zichtbaar in de Azure-portal. U kunt de rechten en het gebruik wel in de Enterprise Portal zien.  

Als u het Azure-abonnement waaraan uw werkruimte is gekoppeld, wilt wijzigen, kunt u de Azure PowerShell-cmdlet [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) gebruiken.

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Azure Commitment gebruiken via een Enterprise-overeenkomst
Als u geen OMS-abonnement hebt, betaalt u voor elk onderdeel van OMS afzonderlijk en wordt het gebruik weergegeven op uw Azure-factuur.

Als u een Azure-betalingsverplichting hebt voor de Enterprise-inschrijving waaraan uw Azure-abonnementen zijn gekoppeld, wordt gebruik van Log Analytics automatisch verrekend met de resterende betalingsverplichting.

Als u het Azure-abonnement waaraan de werkruimte is gekoppeld, wilt wijzigen, kunt u de Azure PowerShell-cmdlet [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) gebruiken.  

### <a name="change-a-workspace-to-a-paid-pricing-tier-in-the-azure-portal"></a>Een werkruimte wijzigen in een betaalde prijscategorie in Azure Portal
1. Selecteer in de Azure-portal in het deelvenster voor abonnementen van Log Analytics een werkruimte.

2. In het werkruimtedeelvenster onder **algemene**, selecteer **prijscategorie**.  

3. Onder **prijscategorie**, selecteer een prijscategorie en klik vervolgens op **Selecteer**.  
    ![Prijsplan geselecteerd](./media/log-analytics-manage-access/workspace-pricing-tier-info.png)

> [!NOTE]
> Als de werkruimte is gekoppeld aan een Automation-account, moet u vóórdat u de prijscategorie *Zelfstandig (per GB)* kunt selecteren eerst alle oplossingen **Automation and Control** verwijderen en het Automation-account loskoppelen. Klik op de blade van de werkruimte onder **Algemeen** op **Oplossingen** om oplossingen te bekijken en te verwijderen. Klik op de blade **Prijscategorie** op de naam van het Automation-account om het Automation-account los te koppelen.
>
>

### <a name="change-a-workspace-to-a-paid-pricing-tier-in-the-oms-portal"></a>Een werkruimte wijzigen in een betaalde prijscategorie in de OMS-portal

Als u de prijscategorie wilt wijzigen via de OMS-portal, moet u een Azure-abonnement hebben.

1. Klik in de OMS-portal op de tegel **Instellingen**.

2. Klik op het tabblad **Accounts** en vervolgens op het tabblad **Azure-abonnement en data-abonnement**.

3. Klik op de prijscategorie die u wilt gebruiken.

4. Klik op **Opslaan**.  

    ![abonnement en data-abonnementen](./media/log-analytics-manage-access/subscription-tab.png)

Uw nieuwe data-abonnement wordt weergegeven in het lint van de OMS-portal boven aan de webpagina.

![OMS-lint](./media/log-analytics-manage-access/data-plan-changed.png)

## <a name="next-steps"></a>Volgende stappen
* Zie [Log Analytics-agent overzicht](log-analytics-agent-overview.md) voor het verzamelen van gegevens van computers in uw datacenter of andere cloudomgeving.
* Zie [Gegevens verzamelen over Azure Virtual Machines](log-analytics-quick-collect-azurevm.md) voor het configureren van het verzamelen van gegevens van Azure VM's.  
* [Log Analytics-oplossingen uit de galerie met oplossingen toevoegen](../monitoring/monitoring-solutions.md) om functionaliteit toe te voegen en gegevens te verzamelen.

