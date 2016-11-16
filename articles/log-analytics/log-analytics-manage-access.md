---
title: Toegang tot Log Analytics beheren | Microsoft Docs
description: Toegang tot Log Analytics beheren met behulp van verschillende beheertaken voor gebruikers, accounts, OMS-werkruimten en Azure-accounts.
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: jwhit
editor: 
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/02/2016
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: fbd35f9fdd5165a2dd4bc7dd47bd70c890539e38


---
# <a name="manage-access-to-log-analytics"></a>Toegang tot Log Analytics beheren
Voor het beheren van toegang tot Log Analytics voert u verschillende beheertaken uit voor gebruikers, accounts, OMS-werkruimten en Azure-accounts. Als u in de Operations Management Suite (OMS) een werkruimte wilt maken, kiest u een werkruimtenaam, koppelt u deze aan uw account en kiest u een geografische locatie. Een werkruimte is in wezen een container met accountgegevens en eenvoudige configuratiegegevens voor het account. U of andere leden van uw organisatie kunnen meerdere OMS-werkruimten gebruiken om verschillende gegevenssets te beheren die worden verzameld uit de gehele of delen van uw IT-infrastructuur.

In het artikel [Aan de slag met Log Analytics](log-analytics-get-started.md) wordt uitgelegd hoe u snel aan de slag kunt gaan. In de rest van dit artikel worden veelgebruikte taken beschreven:

* Vaststellen hoeveel werkruimten u nodig hebt
* Accounts en gebruikers beheren
* Een groep toevoegen aan een bestaande werkruimte
* Een bestaande werkruimte koppelen aan een Azure-abonnement
* Een werkruimte upgraden naar een betaald data-abonnement
* Een data-abonnementtype wijzigen
* Een Azure Active Directory-organisatie toevoegen aan een bestaande werkruimte
* Uw OMS-werkruimte verwijderen

## <a name="determine-the-number-of-workspaces-you-need"></a>Vaststellen hoeveel werkruimten u nodig hebt
Een werkruimte is een Azure-resource die bestaat uit een container waarin gegevens worden verzameld, samengevoegd, geanalyseerd en gepresenteerd in de OMS-portal.

Het is mogelijk om meerdere OMS Log Analytics-werkruimten te maken en gebruikers kunnen toegang hebben tot een of meer werkruimten. Door het aantal werkruimten te minimaliseren, kunt u de meeste gegevens opvragen en correleren. In deze sectie wordt beschreven wanneer het handig kan zijn om meer dan één werkruimte te maken.

Op dit moment biedt een Log Analytics-werkruimte het volgende:

* Een geografische locatie voor de opslag van gegevens
* Details voor facturering
* Gegevensisolatie

Op basis van de bovenstaande kenmerken kunt u in de volgende gevallen meerdere werkruimten maken:

* U vertegenwoordigt een mondiaal bedrijf en moet de gegevens opslaan in specifieke regio’s ten behoeve van de onafhankelijkheid van de gegevens of om nalevingsredenen.
* U gebruikt Azure en wilt kosten voor de overdracht van uitgaande gegevens voorkomen door een Log Analytics-werkruimte in dezelfde regio te hebben als de Azure-resource die deze beheert.
* U wilt kosten toewijzen aan verschillende afdelingen of bedrijfsonderdelen op basis van hun gebruik. Wanneer u voor elke afdeling of bedrijfsgroep een eigen werkruimte maakt, worden op uw factuur- en gebruiksoverzicht van Azure de kosten voor elke werkruimte afzonderlijk weergegeven.
* U bent aanbieder van beheerde services en moet de logboekanalysegegevens voor elke klant geïsoleerd van de gegevens van andere klanten bewaren.
* U beheert meerdere klanten en wilt dat elke klant, afdeling of bedrijfsgroep de eigen gegevens kan bekijken, maar niet de gegevens van anderen.

Wanneer u agents gebruikt om gegevens te verzamelen, kunt u elke agent configureren om te rapporteren aan de vereiste werkruimte.

Als u System Center Operations Manager gebruikt, kan elke beheergroep uit Operations Manager worden verbonden met slechts één werkruimte. U kunt Microsoft Monitoring Agent installeren op computers die worden beheerd door Operations Manager en de agent laten rapporteren over zowel Operations Manager als een andere Log Analytics-werkruimte.

### <a name="workspace-information"></a>Werkruimtegegevens
In de OMS-portal kunt u uw werkruimtegegevens bekijken en kiezen of u informatie wilt ontvangen van Microsoft.

#### <a name="view-workspace-information"></a>Werkruimtegegevens bekijken
1. Klik in OMS op de tegel **Instellingen**.
2. Klik op het tabblad **Accounts**.
3. Klik op het tabblad **Werkruimtegegevens**.  
   ![Werkruimtegegevens](./media/log-analytics-manage-access/workspace-information.png)

## <a name="manage-accounts-and-users"></a>Accounts en gebruikers beheren
Aan elke werkruimte kunnen meerdere gebruikersaccounts zijn gekoppeld en elk gebruikersaccount (Microsoft-account of organisatieaccount) kan toegang hebben tot meerdere OMS-werkruimten.

Standaard wordt het Microsoft-account of organisatieaccount dat wordt gebruikt voor het maken van de werkruimte, de beheerder van de werkruimte. De beheerder kan vervolgens aanvullende Microsoft-accounts uitnodigen of gebruikers uit Azure Active Directory kiezen.

Het verlenen van toegang tot de OMS-werkruimte aan gebruikers wordt op twee locaties beheerd:

* In Azure kunt u op rollen gebaseerd toegangsbeheer gebruiken voor toegang tot het Azure-abonnement en de gekoppelde Azure-resources. Deze rechten worden ook gebruikt voor toegang tot PowerShell en de REST API.
* In de OMS-portal kan alleen toegang worden verleend tot de OMS-portal, niet tot het gekoppelde Azure-abonnement.

Gebruikers zien geen gegevens in de tegels van de oplossingen Backup en Site Recovery als u hen alleen deze toegang geeft tot de OMS-portal, maar niet tot het Azure-abonnement dat eraan is gekoppeld.
Als u alle gebruikers wilt toestaan om in deze oplossingen gegevens te bekijken, moet u ervoor zorgen dat zij ten minste **leesrechten** hebben voor de Back-upkluis en de Site Recovery-kluis die aan de OMS-werkruimte zijn gekoppeld.   

### <a name="managing-access-to-log-analytics-using-the-azure-portal"></a>Toegang tot Log Analytics beheren met behulp van de Azure Portal
Als u gebruikers toegang biedt tot de Log Analytics-werkruimte met behulp van Azure-machtigingen, bijvoorbeeld in de Azure Portal, hebben deze gebruikers ook toegang tot de Log Analytics-portal. Als gebruikers zich in de Azure Portal bevinden, kunnen ze naar de OMS-portal navigeren door te klikken op de taak **OMS Portal** wanneer ze de Log Analytics-werkruimteresource bekijken.

Een aantal punten met betrekking tot de Azure Portal waarmee u rekening moet houden:

* Dit is geen *op rollen gebaseerd toegangsbeheer*. Als u in de Azure Portal *leesrechten* hebt voor de Log Analytics-werkruimte, kunt u via de OMS-portal wijzigingen aanbrengen. De OMS-portal heeft een concept van Beheerder, Bijdrager en Gebruiker met alleen-lezentoegang. Als het account waarbij u bent aangemeld, zich in de Azure Active Directory bevindt die is gekoppeld aan de werkruimte, bent u in de OMS-portal een Beheerder. In andere gevallen bent u een Bijdrager.
* Als u zich bij de OMS-portal aanmeldt via http://mms.microsoft.com, wordt standaard de lijst **Een werkruimte selecteren** weergegeven. Deze lijst bevat alleen werkruimten die zijn toegevoegd met behulp van de OMS-portal. Als u de werkruimten wilt zien waartoe u toegang hebt met Azure-abonnementen, moet u een tenant als onderdeel van de URL opgeven. Bijvoorbeeld:
  
  `mms.microsoft.com/?tenant=contoso.com` De tenant-id is vaak het laatste deel van het e-mailadres waarmee u zich aanmeldt.
* Als het account waarmee u zich aanmeldt, een account in de tenant Azure Active Directory is, bent u in de OMS-portal een *Beheerder*. Dit is meestal het geval, tenzij u zich aanmeldt als een CSP.  Als uw account zich niet in de tenant Azure Active Directory bevindt, bent u in de OMS-portal een *Gebruiker*.
* Als u rechtstreeks wilt navigeren naar een portal waartoe u toegang hebt via Azure-machtigingen, moet u de resource als onderdeel van de URL opgeven. Het is mogelijk om deze URL op te halen met behulp van PowerShell.
  
  Bijvoorbeeld `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.
  
  De URL ziet er als volgt uit: `https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`

### <a name="managing-users-in-the-oms-portal"></a>Gebruikers beheren in de OMS-portal
U beheert gebruikers en groepen op het tabblad **Gebruikers beheren** onder het tabblad **Accounts** op de pagina Instellingen.   

![gebruikers beheren](./media/log-analytics-manage-access/setup-workspace-manage-users.png)

#### <a name="add-a-user-to-an-existing-workspace"></a>Een gebruiker toevoegen aan een bestaande werkruimte
Voer de volgende stappen uit om een gebruiker of groep toe te voegen aan een OMS-werkruimte.

1. Klik in OMS op de tegel **Instellingen**.
2. Klik op het tabblad **Accounts** en vervolgens op het tabblad **Gebruikers beheren**.
3. Kies in de sectie **Gebruikers beheren** het accounttype dat u wilt toevoegen: **Organisatieaccount**, **Microsoft-account** of **Microsoft-ondersteuning**.
   
   * Als u Microsoft-account kiest, typt u het e-mailadres in van de gebruiker die is gekoppeld aan het Microsoft-account.
   * Als u Organisatieaccount kiest, kunt u een deel van de naam of e-mailalias van de gebruiker of groep opgeven. Er wordt dan een lijst met overeenkomende gebruikers en groepen weergegeven in een vervolgkeuzevak. Selecteer een gebruiker of groep.
   * Gebruik Microsoft-ondersteuning om een medewerker van Microsoft-ondersteuning of andere medewerker van Microsoft tijdelijk toegang te verlenen tot uw werkruimte om u te helpen bij het oplossen van problemen.
     
     > [!NOTE]
     > Voor optimale prestaties kunt u het aantal Active Directory-groepen dat aan één OMS-account is gekoppeld, het beste tot drie beperken: één voor beheerders, één voor bijdragers en één voor gebruikers met alleen-lezentoegang. Gebruik van meer groepen kan de prestaties van Log Analytics beïnvloeden.
     > 
     > 
4. Kies het type gebruiker of groep dat u wilt toevoegen: **Beheerder**, **Bijdrager** of **Gebruiker met alleen-lezentoegang**.  
5. Klik op **Add**.
   
   Als u een Microsoft-account toevoegt, wordt er een uitnodiging om lid te worden van de werkruimte verzonden naar het e-mailadres dat u hebt opgegeven. Nadat de gebruiker de instructies in de uitnodiging voor OMS heeft gevolgd, heeft de gebruiker toegang tot deze OMS-werkruimte.
   Als u een organisatieaccount toevoegt, heeft de gebruiker onmiddellijk toegang tot Log Analytics.  
   ![uitnodigingse-mail](./media/log-analytics-manage-access/setup-workspace-invitation-email.png)

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
1. Klik in OMS op de tegel **Instellingen**.
2. Klik op het tabblad **Accounts** en vervolgens op het tabblad **Gebruikers beheren**.
3. Selecteer de rol van de gebruiker die u wilt wijzigen.
4. Klik in het bevestigingsdialoogvenster op **Ja**.

### <a name="remove-a-user-from-an-oms-workspace"></a>Een gebruiker uit een OMS-werkruimte verwijderen
Voer de volgende stappen uit om een gebruiker te verwijderen uit een OMS-werkruimte. Met het verwijderen van de gebruiker wordt de werkruimte niet gesloten. In plaats daarvan wordt de koppeling tussen die gebruiker en de werkruimte verwijderd. Als een gebruiker is gekoppeld aan meerdere werkruimten, kan die gebruiker zich nog wel aanmelden bij OMS en de andere werkruimten zien.

1. Klik in OMS op de tegel **Instellingen**.
2. Klik op het tabblad **Accounts** en vervolgens op het tabblad **Gebruikers beheren**.
3. Klik op **Verwijderen** naast de naam van de gebruiker die u wilt verwijderen.
4. Klik in het bevestigingsdialoogvenster op **Ja**.

### <a name="add-a-group-to-an-existing-workspace"></a>Een groep toevoegen aan een bestaande werkruimte
1. Volg de bovenstaande stappen 1-4 in 'Een gebruiker toevoegen aan een bestaande werkruimte'.
2. Selecteer onder **Gebruiker/groep kiezen** de optie **Groep**.
   ![een groep toevoegen aan een bestaande werkruimte](./media/log-analytics-manage-access/add-group.png)
3. Voer de weergavenaam of het e-mailadres in voor de groep die u wilt toevoegen.
4. Selecteer de groep in de lijst met resultaten en klik op **Toevoegen**.

## <a name="link-an-existing-workspace-to-an-azure-subscription"></a>Een bestaande werkruimte koppelen aan een Azure-abonnement
Alle werkruimten die zijn gemaakt na 26 september 2016 moeten op het moment van maken zijn gekoppeld aan een Azure-abonnement. Werkruimten die vóór deze datum zijn gemaakt, moeten de volgende keer dat u zich aanmeldt aan een werkruimte worden gekoppeld. 

> [!IMPORTANT]
> U kunt een werkruimte alleen koppelen als uw Azure-account al toegang heeft tot deze werkruimte.  Met andere woorden: het account dat u gebruikt voor toegang tot de Azure Portal, moet **hetzelfde** zijn als het account dat u gebruikt voor toegang tot uw OMS-werkruimte. Zie [Een gebruiker toevoegen aan een bestaande werkruimte](#add-a-user-to-an-existing-workspace) als dit niet het geval is.
> 
> 

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-oms-portal"></a>Een werkruimte koppelen aan een Azure-abonnement in de OMS-portal
Als u een werkruimte wilt koppelen aan een Azure-abonnement in de OMS-portal, moet de aangemelde gebruiker al een betaald Azure-account hebben.

1. Klik in OMS op de tegel **Instellingen**.
2. Klik op het tabblad **Accounts** en vervolgens op het tabblad **Azure-abonnement en data-abonnement**.
3. Klik op het data-abonnement dat u wilt gebruiken.
4. Klik op **Opslaan**.  
   ![abonnement en data-abonnementen](./media/log-analytics-manage-access/subscription-tab.png)

Uw nieuwe data-abonnement wordt weergegeven in het lint van de OMS-portal boven aan de webpagina.

![OMS-lint](./media/log-analytics-manage-access/data-plan-changed.png)

### <a name="to-link-a-workspace-to-an-azure-subscription-in-the-azure-portal"></a>Een werkruimte in de Azure Portal koppelen aan een Azure-abonnement
1. Meld u aan bij de [Azure Portal](http://portal.azure.com).
2. Blader naar **Log Analytics (OMS)** en selecteer dit.
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
8. Klik op **Maken**. De werkruimte is nu gekoppeld aan uw Azure-account.

> [!NOTE]
> Als u de werkruimte die u wilt koppelen, niet ziet, heeft uw Azure-abonnement geen toegang tot de OMS-werkruimte die u hebt gemaakt met behulp van de OMS-website.  U moet toegang verlenen tot deze account via de OMS-portal. Zie [Een gebruiker toevoegen aan een bestaande werkruimte](#add-a-user-to-an-existing-workspace) voor meer informatie hierover.
> 
> 

## <a name="upgrade-a-workspace-to-a-paid-plan"></a>Een werkruimte upgraden naar een betaald abonnement
Er zijn drie typen werkruimteabonnementen voor OMS: **Gratis**, **Zelfstandig** en **OMS**.  Als u het *gratis* abonnement hebt, geldt een limiet van 500 MB aan gegevens dat per dag naar Log Analytics wordt verzonden.  Als u deze hoeveelheid overschrijdt, moet u uw werkruimte wijzigen naar een betaald abonnement om te voorkomen dat gegevens buiten deze limiet niet worden verzameld. U kunt op elk gewenst moment uw type abonnement wijzigen.  Zie [Prijsgegevens](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-pricing) voor meer informatie over de tarieven voor OMS.

### <a name="using-entitlements-from-an-oms-subscription"></a>Rechten van een OMS-abonnement gebruiken
Als u de rechten wilt gebruiken die horen bij de aanschaf van OMS E1, OMS E2 OMS of de OMS-invoegtoepassing voor System Center, kiest u het *OMS*-abonnement van OMS Log Analytics.

Wanneer u een OMS-abonnement koopt, worden de rechten toegevoegd aan uw Enterprise-overeenkomst. Elk Azure-abonnement dat onder deze overeenkomst wordt gemaakt, kan gebruikmaken van deze rechten. Hiermee kunt u bijvoorbeeld meerdere OMS-werkruimten hebben die gebruikmaken van de rechten van de OMS-abonnementen.

Doe het volgende om ervoor te zorgen dat gebruik van een OMS-werkruimte wordt toegepast op uw rechten voor het OMS-abonnement:

1. Maak uw OMS-werkruimte in een Azure-abonnement dat deel uitmaakt van de Enterprise-overeenkomst die het OMS-abonnement omvat
2. Selecteer het *OMS*-abonnement voor de werkruimte

> [!NOTE]
> Als uw werkruimte is gemaakt vóór 26 september 2016 en uw Log Analytics-abonnement *Premium* is, maakt deze werkruimte gebruik van de rechten van de OMS-invoegtoepassing voor System Center. U kunt uw rechten ook gebruiken door over te schakelen naar de *OMS*-prijscategorie. 
> 
> 

De rechten van het OMS-abonnement zijn niet zichtbaar in de Azure Portal of OMS-portal. U kunt de rechten en het gebruik wel in de Enterprise Portal zien.  

Als u het Azure-abonnement waaraan uw OMS-werkruimte is gekoppeld, wilt wijzigen, kunt u de Azure PowerShell-cmdlet [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) gebruiken.

### <a name="using-azure-commitment-from-an-enterprise-agreement"></a>Azure Commitment gebruiken via een Enterprise-overeenkomst
Als u geen OMS-abonnement hebt, betaalt u voor elk onderdeel van OMS afzonderlijk en wordt het gebruik weergegeven op uw Azure-factuur.

Als u een Azure-betalingsverplichting hebt voor de Enterprise-inschrijving waaraan uw Azure-abonnementen zijn gekoppeld, wordt gebruik van Log Analytics automatisch verrekend met een eventuele resterende betalingsverplichting.

Als u het Azure-abonnement waaraan de OMS-werkruimte is gekoppeld, wilt wijzigen, kunt u de Azure PowerShell-cmdlet [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) gebruiken.  

### <a name="change-a-workspace-to-a-paid-data-plan"></a>Een werkruimte wijzigen in een betaald data-abonnement
1. Meld u aan bij de [Azure Portal](http://portal.azure.com).
2. Blader naar **Log Analytics** en selecteer dit.
3. U ziet de lijst met bestaande werkruimten. Selecteer een werkruimte.  
   ![lijst met werkruimten](./media/log-analytics-manage-access/manage-access-change-plan01.png)
4. Klik onder **Instellingen** op **Prijscategorie**.  
   ![prijscategorie](./media/log-analytics-manage-access/manage-access-change-plan02.png)
5. Selecteer onder **Prijscategorie** een data-abonnement en klik vervolgens op **Selecteren**.  
   ![abonnement selecteren](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6. Wanneer u uw weergave in de Azure Portal vernieuwt, ziet u dat **Prijscategorie** is bijgewerkt met het abonnement dat u hebt geselecteerd.  
   ![prijscategorie bijwerken](./media/log-analytics-manage-access/manage-access-change-plan04.png)

## <a name="add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Een Azure Active Directory-organisatie toevoegen aan een bestaande werkruimte
U kunt uw Log Analytics-werkruimte koppelen aan een Azure Active Directory-domein en gebruikers uit uw map aan uw OMS-werkruimte koppelen.

Wanneer u de werkruimte maakt via Azure Portal of uw werkruimte koppelt aan een Azure-abonnement, wordt uw Azure Active Directory als uw organisatieaccount gekoppeld.

Wanneer u de werkruimte maakt via de OMS-portal, wordt u gevraagd om een Azure-abonnement en een organisatieaccount te koppelen voordat het Azure-abonnement aan de Log Analytics-werkruimte wordt gekoppeld.

### <a name="to-add-an-azure-active-directory-organization-to-an-existing-workspace"></a>Een Azure Active Directory-organisatie toevoegen aan een bestaande werkruimte
Door een Azure Active Directory-organisatie toe te voegen, kunt u gebruikers en groepen uit die map toevoegen aan de werkruimte.

1. Klik op de pagina Instellingen in OMS op **Accounts** en klik vervolgens op het tabblad **Gebruikers beheren**.  
2. Lees de informatie over organisatieaccounts en klik vervolgens op **Organisatie toevoegen**.  
    ![organisatie toevoegen](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Voer de identiteitsgegevens in van de beheerder van uw Azure Active Directory-domein. Vervolgens wordt er een bevestiging weergegeven waarin staat dat uw werkruimte is gekoppeld aan uw Azure Active Directory-domein.
    ![bevestiging van gekoppelde werkruimte](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

Als u gebruikers moet toevoegen vanuit een andere map, klikt u op de knop *Organisatie wijzigen* om de werkruimte te koppelen aan een andere map.

## <a name="delete-your-log-analytics-workspace"></a>Uw Log Analytics-werkruimte verwijderen
Wanneer u een OMS-werkruimte verwijdert, worden alle gegevens die betrekking hebben op uw werkruimte binnen 30 dagen uit de OMS-service verwijderd.

Als u een beheerder bent en er aan de werkruimte meerdere gebruikers zijn gekoppeld, wordt de koppeling tussen gebruikers en de werkruimte verbroken. Als de gebruikers zijn gekoppeld aan andere werkruimten, kunnen ze OMS blijven gebruiken met die andere werkruimten. Als ze echter niet zijn gekoppeld aan andere werkruimten, moeten ze een werkruimte maken voor het gebruik van OMS.

### <a name="to-delete-an-oms-workspace"></a>Een OMS-werkruimte verwijderen
1. Klik in OMS op de tegel **Instellingen**.
2. Klik op het tabblad **Accounts** en vervolgens op het tabblad **Werkruimtegegevens**.
3. Klik op **Werkruimte sluiten**.
4. Selecteer een van de redenen voor het sluiten van uw werkruimte of voer in het tekstvak een andere reden in.
5. Klik op **Werkruimte sluiten**.

## <a name="next-steps"></a>Volgende stappen
* Zie [Windows-computers verbinden met Log Analytics](log-analytics-windows-agents.md) voor informatie over het toevoegen van agents en verzamelen van gegevens.
* [Log Analytics-oplossingen uit de galerie met oplossingen toevoegen](log-analytics-add-solutions.md) om functionaliteit toe te voegen en gegevens te verzamelen.
* [Proxy- en firewallinstellingen configureren in Log Analytics](log-analytics-proxy-firewall.md) als uw organisatie een proxyserver of firewall gebruikt, zodat agents kunnen communiceren met de Log Analytics-service.




<!--HONumber=Nov16_HO2-->


