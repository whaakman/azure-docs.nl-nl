<properties
    pageTitle="Toegang tot Log Analytics beheren | Microsoft Azure"
    description="Toegang tot Log Analytics beheren met behulp van verschillende beheertaken voor gebruikers, accounts, OMS-werkruimten en Azure-accounts."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="banders"/>


# Toegang tot Log Analytics beheren

Voor het beheren van toegang tot Log Analytics gebruikt u verschillende beheertaken voor gebruikers, accounts, OMS-werkruimten en Azure-accounts. Als u in de Operations Management Suite (OMS) een nieuwe werkruimte wilt maken, kiest u een werkruimtenaam, koppelt u deze aan uw account en kiest u een geografische locatie. Een werkruimte is in wezen een container met accountgegevens en eenvoudige configuratiegegevens voor het account. U of andere leden van uw organisatie kunnen meerdere OMS-werkruimten gebruiken om verschillende gegevenssets te beheren die worden verzameld uit de gehele of delen van uw IT-infrastructuur.

In het artikel [Aan de slag met Log Analytics](log-analytics-get-started.md) leert u hoe u snel aan de slag kunt gaan. In de rest van dit artikel worden sommige van de acties die u moet uitvoeren voor het beheren van toegang tot OMS, gedetailleerder beschreven.

Hoewel u mogelijk niet elke beheertaak meteen hoeft uit te voeren, komen de meest voorkomende taken die u mogelijk gebruikt, in de volgende secties aan de orde:

- Vaststellen hoeveel werkruimten u nodig hebt
- Accounts en gebruikers beheren
- Een groep toevoegen aan een bestaande werkruimte
- Een bestaande werkruimte koppelen aan een Azure-abonnement
- Een werkruimte upgraden naar een betaald data-abonnement
- Een data-abonnementtype wijzigen
- Een Azure Active Directory-organisatie toevoegen aan een bestaande werkruimte
- Uw OMS-werkruimte sluiten

## Vaststellen hoeveel werkruimten u nodig hebt

Een werkruimte is een Azure-resource die bestaat uit een container waarin gegevens worden verzameld, samengevoegd, geanalyseerd en gepresenteerd in de OMS-portal.

Het is mogelijk om meerdere OMS Log Analytics-werkruimten te maken en gebruikers kunnen toegang hebben tot een of meer werkruimten. Over het algemeen is het verstandig om het aantal werkruimten te beperken, omdat dit u in staat stelt om query’s uit te voeren op en relaties te zoeken tussen de meeste gegevens. In deze sectie wordt beschreven wanneer het handig kan zijn om meer dan één werkruimte te maken.

Op dit moment biedt een Log Analytics-werkruimte het volgende:

- Een geografische locatie voor de opslag van gegevens
- Details voor facturering
- Gegevensisolatie

Op basis van de bovenstaande kenmerken kunt u in de volgende gevallen meerdere werkruimten maken:

- U vertegenwoordigt een mondiaal bedrijf en moet de gegevens opslaan in specifieke regio’s ten behoeve van de onafhankelijkheid van de gegevens of om nalevingsredenen.
- U gebruikt Azure en wilt kosten voor de overdracht van uitgaande gegevens voorkomen door een Log Analytics-werkruimte in dezelfde regio te hebben als de Azure-resource die deze beheert.
- U wilt kosten toewijzen aan verschillende afdelingen of bedrijfsonderdelen op basis van hun gebruik. Wanneer u voor elke afdeling of bedrijfsgroep een eigen werkruimte maakt, worden op uw factuur- en gebruiksoverzicht van Azure de kosten voor elke werkruimte afzonderlijk weergegeven.
- U bent aanbieder van beheerde services en moet de logboekanalysegegevens voor elke klant geïsoleerd van de gegevens van andere klanten bewaren.
- U beheert meerdere klanten en wilt dat elke klant, afdeling of bedrijfsgroep de eigen gegevens kan bekijken, maar niet de gegevens van andere klanten, afdelingen of bedrijfsgroepen.

Wanneer u agents gebruikt om gegevens te verzamelen, kunt u elke agent configureren om te rapporteren aan de vereiste werkruimte.

Als u System Center Operations Manager gebruikt, kan elke beheergroep uit Operations Manager worden verbonden met slechts één werkruimte. U kunt Microsoft Monitoring Agent installeren op computers die worden beheerd door Operations Manager en de agent laten rapporteren over zowel Operations Manager als een andere Log Analytics-werkruimte.

### Werkruimtegegevens

In de OMS-portal kunt u uw werkruimtegegevens bekijken en kiezen of u informatie wilt ontvangen van Microsoft.

#### Werkruimtegegevens bekijken

1. Klik in OMS op de tegel **Instellingen**.
2. Klik op het tabblad **Accounts**.
3. Klik op het tabblad **Werkruimtegegevens**.  
  ![Werkruimtegegevens](./media/log-analytics-manage-access/workspace-information.png)

## Accounts en gebruikers beheren

Aan elke werkruimte kunnen meerdere gebruikersaccounts zijn gekoppeld en elk gebruikersaccount (Microsoft-account of organisatieaccount) kan toegang hebben tot meerdere OMS-werkruimten.

Standaard wordt het Microsoft-account of organisatieaccount dat wordt gebruikt voor het maken van de werkruimte, de beheerder van de werkruimte. De beheerder kan vervolgens aanvullende Microsoft-accounts uitnodigen of gebruikers uit Azure Active Directory kiezen.

Het verlenen van toegang tot de OMS-werkruimte aan gebruikers wordt op twee locaties beheerd:

- In Azure kunt u op rollen gebaseerd toegangsbeheer gebruiken voor toegang tot het Azure-abonnement en de gekoppelde Azure-resources. Dit wordt ook gebruikt voor toegang tot PowerShell en de REST API.
- In de OMS-portal kan alleen toegang worden verleend tot de OMS-portal, niet tot het gekoppelde Azure-abonnement.

Als u gebruikers toegang verleent tot de OMS-portal maar niet tot het Azure-abonnement waaraan het is gekoppeld, worden op de tegels van de oplossingen Automation, Back-up en Site Recovery geen gegevens getoond aan gebruikers wanneer ze zich aanmelden bij de OMS-portal.

Als u alle gebruikers wilt toestaan om in deze oplossingen gegevens te bekijken, moet u ervoor zorgen dat zij ten minste **leesrechten** hebben voor het Automation-account, de Back-upkluis en de Site Recovery-kluis die aan de OMS-werkruimte zijn gekoppeld.   

### Toegang tot Log Analytics beheren met behulp van de Azure Portal

Als u gebruikers toegang biedt tot de Log Analytics-werkruimte met behulp van Azure-machtigingen, bijvoorbeeld in de Azure Portal, hebben deze gebruikers ook toegang tot de Log Analytics-portal. Als gebruikers zich in de Azure Portal bevinden, kunnen ze naar de OMS-portal navigeren door te klikken op de taak **OMS Portal** wanneer ze de Log Analytics-werkruimteresource bekijken.

Een aantal punten met betrekking tot de Azure Portal waarmee u rekening moet houden:

- Dit is geen *op rollen gebaseerd toegangsbeheer*. Als u in de Azure Portal *leesrechten* hebt voor de Log Analytics-werkruimte, kunt u via de OMS-portal wijzigingen aanbrengen. De OMS-portal heeft een concept van Beheerder, Bijdrager en Gebruiker met alleen-lezentoegang. Als het account waarbij u bent aangemeld, zich in de Azure Active Directory bevindt die is gekoppeld aan de werkruimte, bent u in de OMS-portal een Beheerder. In andere gevallen bent u een Bijdrager.

- Als u zich bij de OMS-portal aanmeldt via http://mms.microsoft.com, wordt standaard de lijst **Een werkruimte selecteren** weergegeven. Deze lijst bevat alleen werkruimten die zijn toegevoegd met behulp van de OMS-portal. Als u de werkruimten wilt zien waartoe u toegang hebt met Azure-abonnementen, moet u een tenant als onderdeel van de URL opgeven. Bijvoorbeeld:

  `mms.microsoft.com/?tenant=contoso.com` De tenant-id is vaak het laatste deel van het e-mailadres waarmee u zich aanmeldt.

- Als het account waarmee u zich aanmeldt, een account in de tenant Azure Active Directory is (dit is meestal het geval, tenzij u zich aanmeldt als een CSP), bent u in de OMS-portal een *Beheerder*. Als uw account zich niet in de tenant Azure Active Directory bevindt, bent u in de OMS-portal een *Gebruiker*.

- Als u rechtstreeks wilt navigeren naar een portal waartoe u toegang hebt via Azure-machtigingen, moet u de resource als onderdeel van de URL opgeven. Het is mogelijk om deze URL op te halen met behulp van PowerShell.

  Bijvoorbeeld `(Get-AzureRmOperationalInsightsWorkspace).PortalUrl`.

  De URL ziet er als volgt uit:
`https://eus.mms.microsoft.com/?tenant=contoso.com&resource=%2fsubscriptions%2faaa5159e-dcf6-890a-a702-2d2fee51c102%2fresourcegroups%2fdb-resgroup%2fproviders%2fmicrosoft.operationalinsights%2fworkspaces%2fmydemo12`


### Gebruikers beheren in de OMS-portal

U beheert gebruikers en groepen op het tabblad **Gebruikers beheren** onder het tabblad **Accounts** op de pagina Instellingen. Hier kunt u de taken uitvoeren in de volgende secties.  

![gebruikers beheren](./media/log-analytics-manage-access/setup-workspace-manage-users.png)

#### Een gebruiker toevoegen aan een bestaande werkruimte

Voer de volgende stappen uit om een gebruiker of groep toe te voegen aan een OMS-werkruimte. De gebruiker of groep kan alle waarschuwingen die aan deze werkruimte zijn gekoppeld, bekijken en er actie voor ondernemen.

>[AZURE.NOTE] Als u een gebruiker of groep uit uw Azure Active Directory-organisatieaccount wilt toevoegen, moet u eerst controleren of u uw OMS-account hebt gekoppeld aan uw Active Directory-domein. Zie [Een Azure Active Directory-organisatie toevoegen aan een bestaande werkruimte](#add-an-azure-active-directory-organization-to-an-existing-workspace).

1. Klik in OMS op de tegel **Instellingen**.
2. Klik op het tabblad **Accounts** en vervolgens op het tabblad **Gebruikers beheren**.
3. Kies in de sectie **Gebruikers beheren** het accounttype dat u wilt toevoegen: **Organisatieaccount**, **Microsoft-account** of **Microsoft-ondersteuning**.
    - Als u Microsoft-account kiest, typt u het e-mailadres in van de gebruiker die is gekoppeld aan het Microsoft-account.
    - Als u Organisatieaccount kiest, kunt u een deel van de naam of e-mailalias van de gebruiker of groep opgeven. Er wordt dan een lijst met gebruikers en groepen weergegeven. Selecteer een gebruiker of groep.
    - Gebruik Microsoft-ondersteuning om een medewerker van Microsoft-ondersteuning tijdelijk toegang te verlenen tot uw werkruimte om u te helpen bij het oplossen van problemen.

    >[AZURE.NOTE] Voor optimale prestaties kunt u het aantal Active Directory-groepen dat aan één OMS-account is gekoppeld, het beste tot drie beperken: één voor beheerders, één voor bijdragers en één voor gebruikers met alleen-lezentoegang. Gebruik van meer groepen kan de prestaties van Log Analytics beïnvloeden.

5. Kies het type gebruiker of groep dat u wilt toevoegen: **Beheerder**, **Bijdrager** of **Gebruiker met alleen-lezentoegang**.  
6. Klik op **Toevoegen**.

  Als u een Microsoft-account toevoegt, wordt er een uitnodiging om lid te worden van de werkruimte verzonden naar het e-mailadres dat u hebt opgegeven. Nadat de gebruiker de instructies in de uitnodiging heeft gevolgd om lid te worden van OMS, kan de gebruiker de waarschuwingen en accountgegevens voor het OMS-account bekijken en kunt u de gebruikersgegevens op het tabblad **Accounts** van de pagina **Instellingen** bekijken.
  Als u een organisatieaccount toevoegt, heeft de gebruiker onmiddellijk toegang tot Log Analytics.  
  ![uitnodigingse-mail](./media/log-analytics-manage-access/setup-workspace-invitation-email.png)

#### Een bestaand gebruikerstype bewerken

U kunt de accountrol van een gebruiker die aan uw OMS-account is gekoppeld, wijzigen. De volgende rollen zijn beschikbaar:

 - *Beheerder*: kan gebruikers beheren, alle waarschuwingen weergeven en er actie voor ondernemen, en servers toevoegen en verwijderen

 - *Bijdrager*: kan alle waarschuwingen weergeven en er actie voor ondernemen, en servers toevoegen en verwijderen

 - *Gebruiker met alleen-lezentoegang*: gebruikers die zijn gemarkeerd als alleen-lezen kunnen het volgende niet:
   1. Oplossingen toevoegen/verwijderen. De oplossingengalerie is verborgen.
   2. Tegels toevoegen/wijzigen/verwijderen in **Mijn dashboard**.
   3. De pagina’s met **instellingen** bekijken. De pagina's zijn verborgen.
   4. In de zoekweergave zijn taken voor Power BI-configuratie, opgeslagen zoekopdrachten en waarschuwingen verborgen.


#### Een account bewerken

1. Klik in OMS op de tegel **Instellingen**.
2. Klik op het tabblad **Accounts** en vervolgens op het tabblad **Gebruikers beheren**.
3. Selecteer de rol van de gebruiker die u wilt wijzigen.
2. Klik in het bevestigingsdialoogvenster op **Ja**.

### Een gebruiker uit een OMS-werkruimte verwijderen

Voer de volgende stappen uit om een gebruiker te verwijderen uit een OMS-werkruimte. Hiermee wordt de werkruimte van de gebruiker niet gesloten. In plaats daarvan wordt de koppeling tussen die gebruiker en de werkruimte verwijderd. Als een gebruiker is gekoppeld aan meerdere werkruimten, kan die gebruiker zich nog wel aanmelden bij OMS en de andere werkruimten zien.

1. Klik in OMS op de tegel **Instellingen**.
2. Klik op het tabblad **Accounts** en vervolgens op het tabblad **Gebruikers beheren**.
3. Klik op **Verwijderen** naast de naam van de gebruiker die u wilt verwijderen.
4. Klik in het bevestigingsdialoogvenster op **Ja**.


### Een groep toevoegen aan een bestaande werkruimte

1.  Volg de bovenstaande stappen 1-4 in 'Een gebruiker toevoegen aan een bestaande werkruimte'.
2.  Selecteer onder **Gebruiker/groep kiezen** de optie **Groep**.
    ![een groep toevoegen aan een bestaande werkruimte](./media/log-analytics-manage-access/add-group.png)
3.  Voer de weergavenaam of het e-mailadres in voor de groep die u wilt toevoegen.
4.  Selecteer de groep in de lijst met resultaten en klik op **Toevoegen**.

## Een bestaande werkruimte koppelen aan een Azure-abonnement

Het is mogelijk een werkruimte te maken via de website [microsoft.com/oms](https://microsoft.com/oms).  Er bestaan echter bepaalde limieten voor deze werkruimten, waarvan de meest opvallende wordt gevormd door een limiet van 500 MB aan gegevensuploads per dag als u een gratis account gebruikt. Als u in deze werkruimte wijzigingen wilt aanbrengen, moet u *uw bestaande werkruimte koppelen aan een Azure-abonnement*.

>[AZURE.IMPORTANT] U kunt een werkruimte alleen koppelen als uw Azure-account al toegang heeft tot deze werkruimte.  Met andere woorden: het account dat u gebruikt voor toegang tot de Azure Portal, moet **hetzelfde** zijn als het account dat u gebruikt voor toegang tot uw OMS-werkruimte. Als dit niet het geval is, raadpleegt u [Een gebruiker toevoegen aan een bestaande werkruimte](#add-a-user-to-an-existing-workspace).

### Een werkruimte koppelen aan een Azure-abonnement in de OMS-portal

Als u een werkruimte wilt koppelen aan een Azure-abonnement in de OMS-portal, moet de aangemelde gebruiker al een betaald Azure-account hebben. De werkruimte die u actief gebruikt, wordt gekoppeld aan het Azure-account.

1. Klik in OMS op de tegel **Instellingen**.
2. Klik op het tabblad **Accounts** en vervolgens op het tabblad **Azure-abonnement en data-abonnement**.
3. Klik op het data-abonnement dat u wilt gebruiken.
4. Klik op **Opslaan**.  
  ![abonnement en data-abonnementen](./media/log-analytics-manage-access/subscription-tab.png)

Uw nieuwe data-abonnement wordt weergegeven in het lint van de OMS-portal boven aan de webpagina.

![OMS-lint](./media/log-analytics-manage-access/data-plan-changed.png)

### Een werkruimte in de Azure Portal koppelen aan een Azure-abonnement

1.  Meld u aan bij de [Azure Portal](http://portal.azure.com).
2.  Blader naar **Log Analytics (OMS)** en selecteer dit.
3.  U ziet de lijst met bestaande werkruimten. Klik op **Toevoegen**.  
    ![lijst met werkruimten](./media/log-analytics-manage-access/manage-access-link-azure01.png)
4.  Klik onder **OMS-werkruimte** op **Of bestaande koppelen**.  
    ![bestaande koppelen](./media/log-analytics-manage-access/manage-access-link-azure02.png)
5.  Klik op **Vereiste instellingen configureren**.  
    ![vereiste instellingen configureren](./media/log-analytics-manage-access/manage-access-link-azure03.png)
6.  Hier ziet u de lijst met werkruimten die nog niet zijn gekoppeld aan uw Azure-account. Selecteer een werkruimte.  
    ![werkruimten selecteren](./media/log-analytics-manage-access/manage-access-link-azure04.png)
7.  Indien nodig kunt u de waarden van de volgende items wijzigen:
    - Abonnement
    - Resourcegroep
    - Locatie
    - Prijscategorie  
        ![waarden wijzigen](./media/log-analytics-manage-access/manage-access-link-azure05.png)
8.  Klik op **Maken**. De werkruimte is nu gekoppeld aan uw Azure-account.

>[AZURE.NOTE] Als u de werkruimte die u wilt koppelen, niet ziet, heeft uw Azure-abonnement geen toegang tot de OMS-werkruimte die u hebt gemaakt met behulp van de OMS-website.  U moet vanuit uw OMS-werkruimte via de OMS-website toegang verlenen tot dit account. Zie [Een gebruiker toevoegen aan een bestaande werkruimte](#add-a-user-to-an-existing-workspace) voor meer informatie hierover.



## Een werkruimte upgraden naar een betaald data-abonnement

Er zijn drie typen werkruimtedata-abonnementen voor OMS: **Gratis**, **Standard** en **Premium**.  Als u een *Gratis* abonnement hebt, hebt u mogelijk uw datalimiet van 500 MB bereikt.  U moet uw werkruimte upgraden naar een ***abonnement waarbij u betaald op basis van gebruik*** als u boven deze limiet gegevens wilt verzamelen. U kunt op elk gewenst moment uw type abonnement wijzigen.  Zie [Prijsgegevens](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx) voor meer informatie over de tarieven voor OMS.

>[AZURE.IMPORTANT] Werkruimteabonnementen kunnen alleen worden gewijzigd als ze zijn *gekoppeld* aan een Azure-abonnement.  Als u uw werkruimte in Azure hebt gemaakt of als u uw werkruimte *al* hebt gekoppeld, kunt u dit bericht negeren.  Als u uw werkruimte hebt gemaakt met de [OMS-website](http://www.microsoft.com/oms), moet u de stappen van [Een bestaande werkruimte koppelen aan een Azure-abonnement](#link-an-existing-workspace-to-an-azure-subscription) volgen.

### Rechten gebruiken via de OMS-invoegtoepassing voor System Center

De OMS-invoegtoepassing voor System Center biedt rechten voor het Premium-abonnement van OMS Log Analytics, zoals beschreven in [Prijzen voor OMS](https://www.microsoft.com/en-us/server-cloud/operations-management-suite/pricing.aspx).

Wanneer u de OMS-invoegtoepassing voor System Center aanschaft, wordt de OMS-invoegtoepassing voor System Center als een recht toegevoegd aan uw System Center-overeenkomst. Elk Azure-abonnement dat onder deze overeenkomst wordt gemaakt, kan gebruikmaken van dit recht. Hiermee kunt u bijvoorbeeld meerdere OMS-werkruimten hebben die gebruikmaken van de rechten voor de OMS-invoegtoepassing.

Doe het volgende om ervoor te zorgen dat gebruik van een OMS-werkruimte wordt toegepast op uw rechten voor de OMS-invoegtoepassing:

1. Uw OMS-werkruimte koppelen aan een Azure-abonnement dat deel uitmaakt van de Enterprise-overeenkomst die zowel gebruik van de aangeschafte OMS-invoegtoepassing als het Azure-abonnement omvat
2. Het Premium-abonnement selecteren voor de werkruimte

Als u uw gebruik in de Azure Portal of OMS-portal bekijkt, worden de rechten voor de OMS-invoegtoepassing niet weergegeven. U kunt de rechten echter wel in de Enterprise Portal zien.  

Als u het Azure-abonnement waaraan uw OMS-werkruimte is gekoppeld, wilt wijzigen, kunt u de Azure PowerShell-cmdlet [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) gebruiken.

### Azure Commitment gebruiken via een Enterprise-overeenkomst

Als u kiest voor het gebruik van afzonderlijke tarieven voor OMS-onderdelen, betaalt u voor elk onderdeel van OMS afzonderlijk en wordt het gebruik weergegeven op uw Azure-factuur.

Als u een Azure-betalingsverplichting hebt voor de Enterprise-inschrijving waaraan uw Azure-abonnementen zijn gekoppeld, wordt gebruik van Log Analytics automatisch verrekend met een eventuele resterende betalingsverplichting.

Als u het Azure-abonnement waaraan de OMS-werkruimte is gekoppeld, wilt wijzigen, kunt u de Azure PowerShell-cmdlet [Move-AzureRmResource](https://msdn.microsoft.com/library/mt652516.aspx) gebruiken.  



### Een werkruimte wijzigen in een betaald data-abonnement

1.  Meld u aan bij de [Azure Portal](http://portal.azure.com).
2.  Blader naar **Log Analytics (OMS)** en selecteer dit.
3.  U ziet de lijst met bestaande werkruimten. Selecteer een werkruimte.  
    ![lijst met werkruimten](./media/log-analytics-manage-access/manage-access-change-plan01.png)
4.  Klik onder **Instellingen** op **Prijscategorie**.  
    ![prijscategorie](./media/log-analytics-manage-access/manage-access-change-plan02.png)
5.  Selecteer onder **Prijscategorie** een data-abonnement en klik vervolgens op **Selecteren**.  
    ![abonnement selecteren](./media/log-analytics-manage-access/manage-access-change-plan03.png)
6.  Wanneer u uw weergave in de Azure Portal vernieuwt, ziet u dat **Prijscategorie** is bijgewerkt met het abonnement dat u hebt geselecteerd.  
    ![prijscategorie bijwerken](./media/log-analytics-manage-access/manage-access-change-plan04.png)

U kunt nu gegevens buiten de limiet voor gratis gegevens verzamelen.


## Een Azure Active Directory-organisatie toevoegen aan een bestaande werkruimte

U kunt uw Log Analytics-werkruimte (OMS) koppelen aan een Azure Active Directory-domein. Hiermee kunt u gebruikers uit Active Directory rechtstreeks aan uw OMS-werkruimte toevoegen zonder een apart Microsoft-account.

Wanneer u de werkruimte maakt via de Azure Portal of uw werkruimte koppelt aan een Azure-abonnement, wordt uw Azure Active Directory als uw organisatieaccount gekoppeld.

Wanneer u de werkruimte maakt via de OMS-portal, wordt u gevraagd om een Azure-abonnement en een organisatieaccount te koppelen.

### Een Azure Active Directory-organisatie toevoegen aan een bestaande werkruimte

1. Klik op de pagina Instellingen in OMS op **Accounts** en klik vervolgens op het tabblad **Werkruimtegegevens**.  
2. Lees de informatie over organisatieaccounts en klik vervolgens op **Organisatie toevoegen**.  
    ![organisatie toevoegen](./media/log-analytics-manage-access/manage-access-add-adorg01.png)
3. Voer de identiteitsgegevens in van de beheerder van uw Azure Active Directory-domein. Vervolgens wordt er een bevestiging weergegeven waarin staat dat uw werkruimte is gekoppeld aan uw Azure Active Directory-domein.
    ![bevestiging van gekoppelde werkruimte](./media/log-analytics-manage-access/manage-access-add-adorg02.png)

>[AZURE.NOTE] Nadat uw account is gekoppeld aan een organisatieaccount, kan deze koppeling niet meer worden verwijderd of gewijzigd.

## Uw OMS-werkruimte sluiten

Wanneer u een OMS-werkruimte sluit, worden alle prestatiegegevens die betrekking hebben op uw werkruimte, binnen 30 dagen na het sluiten van de werkruimte uit de OMS-service verwijderd.

Als u een beheerder bent en er aan de werkruimte meerdere gebruikers zijn gekoppeld, wordt de koppeling tussen gebruikers en de werkruimte verbroken. Als de gebruikers zijn gekoppeld aan andere werkruimten, kunnen ze OMS blijven gebruiken met die andere werkruimten. Als ze echter niet zijn gekoppeld aan andere werkruimten, moeten ze een nieuwe werkruimte maken voor het gebruik van OMS.

### Een OMS-werkruimte sluiten

1. Klik in OMS op de tegel **Instellingen**.
2. Klik op het tabblad **Accounts** en vervolgens op het tabblad **Werkruimtegegevens**.
3. Klik op **Werkruimte sluiten**.
4. Selecteer een van de redenen voor het sluiten van uw werkruimte of voer in het tekstvak een andere reden in.
5. Klik op **Werkruimte sluiten**.

## Volgende stappen

- Zie [Windows-computers verbinden met Log Analytics](log-analytics-windows-agents.md) voor informatie over het toevoegen van agents en verzamelen van gegevens.
- [Log Analytics-oplossingen uit de galerie met oplossingen toevoegen](log-analytics-add-solutions.md) om functionaliteit toe te voegen en gegevens te verzamelen.
- [Proxy- en firewallinstellingen configureren in Log Analytics](log-analytics-proxy-firewall.md) als uw organisatie een proxyserver of firewall gebruikt, zodat agents kunnen communiceren met de Log Analytics-service.



<!--HONumber=Sep16_HO3-->


