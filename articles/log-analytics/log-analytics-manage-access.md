---
title: De werkruimten in Azure Log Analytics beheren | Microsoft Docs
description: U kunt werkruimten in Azure Log Analytics beheren met behulp van verschillende beheertaken voor gebruikers, accounts, werkruimten en Azure-accounts.
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/12/2017
ms.author: magoedte
ms.openlocfilehash: d9f86ac19044fd13e77d35d6c3dd9964c3852001
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-workspaces"></a>Werkruimten beheren

Voor het beheren van toegang tot Log Analytics, voert u verschillende beheertaken uit voor werkruimten. In dit artikel worden adviezen en aanbevolen procedures beschreven voor het beheren van werkruimten. Een werkruimte is in wezen een container met accountgegevens en eenvoudige configuratiegegevens voor het account. U of andere leden van uw organisatie kunnen meerdere werkruimten gebruiken om verschillende gegevenssets te beheren die worden verzameld uit de gehele of delen van uw IT-infrastructuur.

Het volgende is nodig om een werkruimte te maken:

1. U dient een Azure-abonnement te hebben.
2. U dient een naam voor de werkruimte te kiezen.
3. U dient de werkruimte aan uw abonnement te koppelen.
4. U dient een geografische locatie te kiezen.

## <a name="determine-the-number-of-workspaces-you-need"></a>Vaststellen hoeveel werkruimten u nodig hebt
Een werkruimte is een Azure-resource die bestaat uit een container waarin gegevens worden verzameld, samengevoegd, geanalyseerd en gepresenteerd in Azure Portal.

U kunt per Azure-abonnement beschikken over meerdere werkruimten en toegang hebben tot meer dan één werkruimte. Door het aantal werkruimten te minimaliseren, kunt u voor de meeste gegevens query's uitvoeren en de gegevens aan elkaar relateren, aangezien het niet mogelijk is om query's voor meerdere werkruimten uit te voeren. In deze sectie wordt beschreven wanneer het handig kan zijn om meer dan één werkruimte te maken.

Een werkruimte biedt momenteel het volgende:

* Een geografische locatie voor de opslag van gegevens
* Details voor facturering
* Gegevensisolatie
* Bereik voor configuratie

Op basis van de voorgaande kenmerken kunt u in de volgende gevallen meerdere werkruimten maken:

* U vertegenwoordigt een mondiaal bedrijf en moet de gegevens opslaan in specifieke regio’s ten behoeve van de onafhankelijkheid van de gegevens of om nalevingsredenen.
* U gebruikt Azure en wilt kosten voor de overdracht van uitgaande gegevens voorkomen door een werkruimte in dezelfde regio te hebben als de Azure-resource die deze beheert.
* U wilt kosten toewijzen aan verschillende afdelingen of bedrijfsonderdelen op basis van hun gebruik. Wanneer u voor elke afdeling of bedrijfsgroep een eigen werkruimte maakt, worden op uw factuur- en gebruiksoverzicht van Azure de kosten voor elke werkruimte afzonderlijk weergegeven.
* U bent aanbieder van beheerde services en moet de Log Analytics-gegevens voor elke klant geïsoleerd van de gegevens van andere klanten bewaren.
* U beheert meerdere klanten en wilt dat elke klant, afdeling of bedrijfsgroep de eigen gegevens kan bekijken, maar niet de gegevens van anderen.

Wanneer u agents gebruikt om gegevens te verzamelen, kunt u [elke agent configureren om te rapporteren aan een of meer werkruimten](log-analytics-windows-agents.md).

Als u System Center Operations Manager gebruikt, kan elke beheergroep uit Operations Manager worden verbonden met slechts één werkruimte. De Microsoft Monitoring Agent (MMA) op de computer kan echter zo worden geconfigureerd dat zowel aan Operations Manager als een andere Log Analytics-werkruimte wordt gerapporteerd.  

### <a name="workspace-information"></a>Werkruimtegegevens

U kunt gegevens van uw werkruimte in Azure Portal bekijken. 

#### <a name="view-workspace-information-in-the-azure-portal"></a>Werkruimtegegevens weergeven in Azure Portal

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **Meer services** in de linkerbenedenhoek van Azure Portal.  Typ in de lijst met resources **Log Analytics**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Klik op **Log Analytics**.  
    ![Azure-hub](./media/log-analytics-manage-access/hub.png)  
3. Selecteer een werkruimte op de blade Abonnementen in Log Analytics.
4. De blade van de werkruimte geeft gegevens over de werkruimte en koppelingen voor meer informatie weer.  
    ![details van de werkruimte](./media/log-analytics-manage-access/workspace-details.png)  


## <a name="manage-accounts-and-users"></a>Accounts en gebruikers beheren
Aan elke werkruimte kunnen meerdere accounts worden gekoppeld en elk account (Microsoft-account of organisatieaccount) kan toegang hebben tot meerdere werkruimten.

Standaard wordt het Microsoft-account of organisatieaccount dat wordt gebruikt voor het maken van de werkruimte, de beheerder van de werkruimte.

Er zijn twee machtigingsmodellen die de toegang tot een Log Analytics-werkruimte beheren:

1. Verouderde Log Analytics-gebruikersrollen
2. [Toegang op basis van rollen in Azure](../active-directory/role-based-access-control-configure.md)

In de volgende tabel ziet u de toegang die met elk machtigingsmodel kan worden ingesteld:

|                          | Log Analytics-portal | Azure Portal | API (inclusief PowerShell) |
|--------------------------|----------------------|--------------|----------------------------|
| Log Analytics-gebruikersrollen | Ja                  | Nee           | Nee                         |
| Toegang op basis van rollen in Azure  | Ja                  | Ja          | Ja                        |

> [!NOTE]
> Log Analytics zal in plaats van Log Analytics-gebruikersrollen toegang op basis van rollen in Azure gaan gebruiken als machtigingsmodel.
>
>

De oude Log Analytics-gebruikersrollen beheren alleen de toegang tot activiteiten die worden uitgevoerd in de [Log Analytics-portal](https://mms.microsoft.com).

Voor de volgende activiteiten zijn ook Azure-machtigingen vereist:

| Actie                                                          | Azure-machtigingen nodig | Opmerkingen |
|-----------------------------------------------------------------|--------------------------|-------|
| Beheeroplossingen toevoegen en verwijderen                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | |
| De prijscategorie wijzigen                                       | `Microsoft.OperationalInsights/workspaces/*/write` | |
| Gegevens weergeven op de tegels *Back-up* en *Site Recovery* | Beheerder/medebeheerder | Heeft toegang tot resources die zijn geïmplementeerd met behulp van het klassieke implementatiemodel |
| Een werkruimte maken in Azure Portal                        | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||


### <a name="managing-access-to-log-analytics-using-azure-permissions"></a>Toegang tot Log Analytics beheren met behulp van Azure-machtigingen
Volg de stappen in [Roltoewijzingen gebruiken voor het beheer van de toegang tot de resources van uw Azure-abonnement](../active-directory/role-based-access-control-configure.md) om toegang te verlenen tot de Log Analytics-werkruimte met behulp van Azure-machtigingen.

Azure heeft twee ingebouwde gebruikersrollen voor Log Analytics:
- Lezer van Log Analytics
- Inzender van Log Analytics

Leden van de rol *Lezer van Log Analytics* kunnen:
- Alle controlegegevens weergeven en doorzoeken 
- Controlegegevens weergeven, inclusief de configuratie van Azure Diagnostics voor alle Azure-resources.

| Type    | Machtiging | Beschrijving |
| ------- | ---------- | ----------- |
| Bewerking | `*/read`   | De mogelijkheid om alle resources en de resourceconfiguratie weer te geven. Omvat: <br> Status van de VM-extensie <br> Configuratie van Azure Diagnostics voor resources <br> Alle eigenschappen en instellingen van alle resources |
| Bewerking | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Mogelijkheid om Zoeken in logboeken v2-query's uit te voeren |
| Bewerking | `Microsoft.OperationalInsights/workspaces/search/action` | Mogelijkheid om Zoeken in logboeken v1-query's uit te voeren |
| Bewerking | `Microsoft.Support/*` | Mogelijkheid ondersteuningsaanvragen te openen |
|Geen bewerking | `Microsoft.OperationalInsights/workspaces/sharedKeys/read` | Zorgt ervoor dat de werkruimtesleutel die is vereist om de gegevensverzameling-API te gebruiken en agents te installeren, niet kan worden gelezen |


Leden van de rol *Inzender van Log Analytics* kunnen:
- Alle controlegegevens lezen 
- Automation-accounts maken en configureren
- Beheeroplossingen toevoegen en verwijderen
- Opslagaccountsleutels lezen 
- Verzameling met logboeken uit Azure Storage configureren
- De controle-instellingen voor Azure-resources bewerken, inclusief
  - De VM-extensie toevoegen aan virtuele machines
  - Azure Diagnostics configureren op alle Azure-resources

> [!NOTE] 
> U kunt de mogelijkheid om een VM-extensie toe te voegen aan een virtuele machine, gebruiken om volledige controle over een virtuele machine te krijgen.

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

Gebruik [aangepaste rollen](../active-directory/role-based-access-control-custom-roles.md) om rollen te maken met de specifieke machtigingen die nodig zijn.

### <a name="azure-user-roles-and-log-analytics-portal-user-roles"></a>Gebruikersrollen in Azure en in Log Analytics-portal
Als u minimaal Azure-leesmachtiging hebt in de Log Analytics-werkruimte, kunt u de OMS-portal openen door op de taak **OMS-portal** te klikken wanneer de Log Analytics-werkruimte wordt weergegeven.

Bij het openen van de OMS-portal schakelt u over op het gebruik van de verouderde Log Analytics-gebruikersrollen. Als u niet beschikt over een roltoewijzing in de Log Analytics-portal, [worden de Azure-machtigingen waarover u beschikt, in de werkruimte gecontroleerd](https://docs.microsoft.com/rest/api/authorization/permissions#Permissions_ListForResource).
De roltoewijzing in de OMS-portal wordt als volgt bepaald:

| Voorwaarden                                                   | Toegewezen Log Analytics-gebruikersrol | Opmerkingen |
|--------------------------------------------------------------|----------------------------------|-------|
| Uw account behoort tot een verouderde Log Analytics-gebruikersrol     | De opgegeven Log Analytics-gebruikersrol | |
| Uw account behoort niet tot een verouderde Log Analytics-gebruikersrol <br> Volledige Azure-machtigingen voor de werkruimte (`*` machtiging <sup>1</sup>) | Beheerder ||
| Uw account behoort niet tot een verouderde Log Analytics-gebruikersrol <br> Volledige Azure-machtigingen voor de werkruimte (`*` machtiging <sup>1</sup>) <br> *geen acties* van `Microsoft.Authorization/*/Delete` en `Microsoft.Authorization/*/Write` | Inzender ||
| Uw account behoort niet tot een verouderde Log Analytics-gebruikersrol <br> Azure-leesmachtiging | Alleen-lezen ||
| Uw account behoort niet tot een verouderde Log Analytics-gebruikersrol <br> Azure-machtigingen zijn niet begrepen | Alleen-lezen ||
| Voor door Cloud Solution Provider (CSP) beheerde abonnementen <br> Het account waarbij u bent aangemeld, bevindt zich in de Azure Active Directory die aan de werkruimte is gekoppeld | Beheerder | Doorgaans de klant van een CSP |
| Voor door Cloud Solution Provider (CSP) beheerde abonnementen <br> Het account waarbij u bent aangemeld, bevindt zich niet in de Azure Active Directory die aan de werkruimte is gekoppeld | Inzender | Doorgaans de CSP |

<sup>1</sup> Raadpleeg [Azure-machtigingen](../active-directory/role-based-access-control-custom-roles.md) voor meer informatie over roldefinities. Bij het evalueren van rollen is een actie van `*` niet equivalent aan `Microsoft.OperationalInsights/workspaces/*`.

Een aantal punten met betrekking tot de Azure Portal waarmee u rekening moet houden:

* Als u zich bij de OMS-portal aanmeldt via http://mms.microsoft.com, wordt de lijst **Een werkruimte selecteren** weergegeven. Deze lijst bevat alleen werkruimten waarin u een Log Analytics-gebruikersrol hebt. Als u de werkruimten wilt zien waartoe u toegang hebt met Azure-abonnementen, moet u een tenant als onderdeel van de URL opgeven. Bijvoorbeeld `mms.microsoft.com/?tenant=contoso.com`. De tenant-id is vaak het laatste deel van het e-mailadres waarmee u zich aanmeldt.
* Als u rechtstreeks wilt navigeren naar een portal waartoe u toegang hebt via Azure-machtigingen, moet u de resource als onderdeel van de URL opgeven. Het is mogelijk om deze URL op te halen met behulp van PowerShell.

  Bijvoorbeeld `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  De URL ziet er als volgt uit: `https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`

### <a name="managing-users-in-the-oms-portal"></a>Gebruikers beheren in de OMS-portal
U beheert gebruikers en groepen op het tabblad **Gebruikers beheren** onder het tabblad **Accounts** op de pagina Instellingen.   

![gebruikers beheren](./media/log-analytics-manage-access/setup-workspace-manage-users.png)


#### <a name="add-a-user-to-an-existing-workspace"></a>Een gebruiker toevoegen aan een bestaande werkruimte
Voer de volgende stappen uit om een gebruiker of groep toe te voegen aan een werkruimte.

1. Klik in de OMS-portal op de tegel **Instellingen**.
2. Klik op het tabblad **Accounts** en vervolgens op het tabblad **Gebruikers beheren**.
3. Kies in de sectie **Gebruikers beheren** het accounttype dat u wilt toevoegen: **Organisatieaccount**, **Microsoft-account** of **Microsoft-ondersteuning**.

   * Als u Microsoft-account kiest, typt u het e-mailadres in van de gebruiker die is gekoppeld aan het Microsoft-account.
   * Als u Organisatieaccount kiest, geeft u een deel van de naam of e-mailalias van de gebruiker/groep op. Er wordt dan een lijst met overeenkomende gebruikers en groepen weergegeven in een vervolgkeuzevak. Selecteer een gebruiker of groep.
   * Gebruik Microsoft-ondersteuning om een medewerker van Microsoft-ondersteuning of andere medewerker van Microsoft tijdelijk toegang te verlenen tot uw werkruimte om u te helpen bij het oplossen van problemen.

     > [!NOTE]
     > Voor optimale prestaties kunt u het aantal Active Directory-groepen dat aan één OMS-account is gekoppeld, het beste tot drie beperken: één voor beheerders, één voor bijdragers en één voor gebruikers met alleen-lezentoegang. Gebruik van meer groepen kan de prestaties van Log Analytics beïnvloeden.
     >
     >
4. Kies het type gebruiker of groep dat u wilt toevoegen: **Beheerder**, **Bijdrager** of **Gebruiker met alleen-lezentoegang**.  
5. Klik op **Add**.

   Als u een Microsoft-account toevoegt, wordt er een uitnodiging om lid te worden van de werkruimte verzonden naar het e-mailadres dat u hebt opgegeven. Nadat de gebruiker de instructies in de uitnodiging voor OMS heeft gevolgd, heeft de gebruiker toegang tot deze werkruimte.
   Als u een organisatieaccount toevoegt, heeft de gebruiker onmiddellijk toegang tot Log Analytics.  

#### <a name="edit-an-existing-user-type"></a>Een bestaand gebruikerstype bewerken
U kunt de accountrol van een gebruiker die aan uw OMS-account is gekoppeld, wijzigen. De volgende rollen zijn beschikbaar:

* *Beheerder*: kan gebruikers beheren, alle waarschuwingen weergeven en er actie voor ondernemen, en servers toevoegen en verwijderen
* *Bijdrager*: kan alle waarschuwingen weergeven en er actie voor ondernemen, en servers toevoegen en verwijderen
* *Gebruiker met alleen-lezentoegang*: gebruikers die zijn gemarkeerd als alleen-lezen kunnen het volgende niet:

  1. Oplossingen toevoegen/verwijderen. De oplossingengalerie is verborgen.
  2. Tegels toevoegen/wijzigen/verwijderen in **Mijn dashboard**.
  3. De pagina’s met **instellingen** bekijken. De pagina's zijn verborgen.
  4. In de zoekweergave zijn taken voor Power BI-configuratie, opgeslagen zoekopdrachten en waarschuwingen verborgen.

#### <a name="to-edit-an-account"></a>Een account bewerken
1. Klik in de OMS-portal op de tegel **Instellingen**.
2. Klik op het tabblad **Accounts** en vervolgens op het tabblad **Gebruikers beheren**.
3. Selecteer de rol van de gebruiker die u wilt wijzigen.
4. Klik in het bevestigingsdialoogvenster op **Ja**.

### <a name="remove-a-user-from-a-workspace"></a>Een gebruiker uit een werkruimte verwijderen
Voer de volgende stappen uit om een gebruiker te verwijderen uit een werkruimte. Met het verwijderen van de gebruiker wordt de werkruimte niet gesloten. In plaats daarvan wordt de koppeling tussen die gebruiker en de werkruimte verwijderd. Als een gebruiker is gekoppeld aan meerdere werkruimten, kan die gebruiker zich nog wel aanmelden bij OMS en de andere werkruimten zien.

1. Klik in de OMS-portal op de tegel **Instellingen**.
2. Klik op het tabblad **Accounts** en vervolgens op het tabblad **Gebruikers beheren**.
3. Klik op **Verwijderen** naast de naam van de gebruiker die u wilt verwijderen.
4. Klik in het bevestigingsdialoogvenster op **Ja**.

### <a name="add-a-group-to-an-existing-workspace"></a>Een groep toevoegen aan een bestaande werkruimte
1. Volg stap 1-4 in het gedeelte 'Een gebruiker toevoegen aan een bestaande werkruimte' hierboven.
2. Selecteer onder **Gebruiker/groep kiezen** de optie **Groep**.  
   ![een groep toevoegen aan een bestaande werkruimte](./media/log-analytics-manage-access/add-group.png)
3. Voer de weergavenaam of het e-mailadres in voor de groep die u wilt toevoegen.
4. Selecteer de groep in de lijst met resultaten en klik op **Toevoegen**.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Een bestaande werkruimte koppelen aan een Azure-abonnement
Alle werkruimten die zijn gemaakt na 26 september 2016 moeten op het moment van maken zijn gekoppeld aan een Azure-abonnement. Werkruimten die vóór deze datum zijn gemaakt, moeten bij aanmelding worden gekoppeld aan een werkruimte. Wanneer u de werkruimte maakt via Azure Portal of uw werkruimte koppelt aan een Azure-abonnement, wordt uw Azure Active Directory als uw organisatieaccount gekoppeld.

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>Een werkruimte koppelen aan een Azure-abonnement in de OMS-portal

- Wanneer u zich aanmeldt bij de OMS-portal, wordt u gevraagd om een Azure-abonnement te selecteren. Selecteer het abonnement dat u aan uw werkruimte wilt koppelen en klik vervolgens op **Koppelen**.  
    ![Azure-abonnement koppelen](./media/log-analytics-manage-access/required-link.png)

    > [!IMPORTANT]
    > U kunt een werkruimte alleen koppelen als uw Azure-account al toegang heeft tot deze werkruimte.  Met andere woorden: het account dat u gebruikt voor toegang tot Azure Portal, moet **hetzelfde** zijn als het account dat u gebruikt voor toegang tot de werkruimte. Zie [Een gebruiker toevoegen aan een bestaande werkruimte](#add-a-user-to-an-existing-workspace) als dit niet het geval is.

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Een werkruimte in de Azure Portal koppelen aan een Azure-abonnement
1. Meld u aan bij de [Azure Portal](http://portal.azure.com).
2. Blader naar **Log Analytics** en selecteer dit.
3. U ziet de lijst met bestaande werkruimten. Klik op **Add**.  
   ![lijst met werkruimten](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4. Klik onder **OMS-werkruimte** op **Of bestaande koppelen**.  
   ![bestaande koppelen](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5. Klik op **Vereiste instellingen configureren**.  
   ![vereiste instellingen configureren](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6. Hier ziet u de lijst met werkruimten die nog niet zijn gekoppeld aan uw Azure-account. Selecteer een werkruimte.  
   ![werkruimten selecteren](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7. Indien nodig kunt u de waarden van de volgende items wijzigen:
   * Abonnement
   * Resourcegroep
   * Locatie
   * Prijscategorie  
     ![waarden wijzigen](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8. Klik op **OK**. De werkruimte is nu gekoppeld aan uw Azure-account.

> [!NOTE]
> Als u de te koppelen werkruimte niet ziet, heeft uw Azure-abonnement geen toegang tot de werkruimte die u hebt gemaakt met behulp van de OMS-portal.  Zie [Een gebruiker toevoegen aan een bestaande werkruimte](#add-a-user-to-an-existing-workspace) om dit account toegang te verlenen vanuit de OMS-portal.
>
>

## <a name="change-an-azure-active-directory-organization-for-a-workspace"></a>Een Azure Active Directory-organisatie wijzigen voor een werkruimte

U kunt de Azure Active Directory-organisatie van een werkruimte wijzigen. Door de Azure Active Directory-organisatie te wijzigen, kunt u gebruikers en groepen uit die map toevoegen aan de werkruimte.

### <a name="to-change-the-azure-active-directory-organization-for-a-workspace"></a>De Azure Active Directory-organisatie wijzigen voor een werkruimte

1. Klik op de pagina Instellingen in de OMS-portal op **Accounts** en klik vervolgens op het tabblad **Gebruikers beheren**.  
2. Controleer de informatie over organisatieaccounts en klik vervolgens op **Organisatie wijzigen**.  
    ![organisatie wijzigen](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Voer de identiteitsgegevens in van de beheerder van uw Azure Active Directory-domein. Vervolgens wordt er een bevestiging weergegeven waarin staat dat uw werkruimte is gekoppeld aan uw Azure Active Directory-domein.  
    ![bevestiging van gekoppelde werkruimte](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

## <a name="next-steps"></a>Volgende stappen
* Zie [Gegevensgebruik begrijpen](log-analytics-usage.md) voor informatie over het analyseren van de hoeveelheid gegevens die door oplossingen worden verzameld en vanaf computers worden verzonden.
* [Log Analytics-beheeroplossingen uit Azure Marketplace toevoegen](log-analytics-add-solutions.md) om functionaliteit toe te voegen en gegevens te verzamelen.