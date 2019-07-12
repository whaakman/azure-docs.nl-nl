---
title: Over het beheren van een commerciële Marketplace-account in Partnercentrum
description: Informatie over het beheren van een commerciële Marketplace-account in de Partner Center.
author: mattwojo
manager: evansma
ms.author: parthp
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: e0c9f1fcf2d9d7e806645f1bdf9e8a6c74245a60
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2019
ms.locfileid: "67619325"
---
# <a name="how-to-manage-your-commercial-marketplace-account-in-partner-center"></a>Over het beheren van uw commerciële Marketplace-account in Partnercentrum 

Nadat u hebt [een Partner Center-account hebt gemaakt](./create-account.md), kunt u uw account en aanbiedingen met behulp van beheren de [commerciële Marketplace-dashboard](https://partner.microsoft.com/dashboard/commercial-marketplace/overview).

In dit artikel gaat we Duik in over het beheren van uw account Partner Center onder andere: 

- [Toegang tot de instellingen van uw Partner Center-account](#access-your-account-settings)
- [Uw uitgevers-ID, de verkoper-ID, de gebruikers-ID en het Azure AD-tenants vinden](#account-details)
- [Contactgegevens bijwerken](#contact-info)
- [Financiële gegevens (betalings-account, btw-profiel, payout wachtstatus) beheren](#financial-details)
- [Bijhouden van GUID's instellen voor het bewaken van klanten te worden gebruikt](#tracking-guids)
- [Gebruikers van beheer](#manage-users)
- [Manager-groepen](#manage-groups)
- [Beheer Azure AD-toepassingen](#manage-azure-ad-applications)
- [Gebruikersrollen en machtigingen definiëren](#define-user-roles-and-permissions)
- [Azure AD-tenants (werkaccounts) beheren](#manage-tenants)
- [Manager Partner Center overeenkomsten](#agreements)


## <a name="access-your-account-settings"></a>Toegang tot de instellingen van uw account

Als u hebt nog niet gedaan, u (of beheerder van uw organisatie) moet toegang tot de [Accountinstellingen](https://partner.microsoft.com/dashboard/account/management) voor uw account Partner Center om:
- accountstatus voor verificatie van uw bedrijf controleren
- Bevestig uw verkoper-ID, de MPN-ID, de uitgevers-ID en neem contact op met de informatie, met inbegrip van het bedrijf fiatteur en verkoper contact
- instellen van financiële gegevens van uw bedrijf, met inbegrip van vrijstelling van belasting, indien van toepassing
- Maak gebruikersaccounts maken voor iedereen die uw bedrijven-account wordt gebruikt in Partnercentrum

### <a name="open-developer-settings"></a>Open de instellingen voor ontwikkelaars

Accountinstellingen bevindt zich in de rechterbovenhoek van uw [commerciële Marketplace-dashboard](https://partner.microsoft.com/dashboard/commercial-marketplace) in Partnercentrum. Selecteer het tandwielpictogram (in de rechterbovenhoek van het dashboard) en selecteer vervolgens **-instellingen voor ontwikkelaars**. 

![Account-instellingen in het menu in Partnercentrum](./media/dashboard-developer-settings.png)

Binnen **Accountinstellingen**, kunt u zich om weer te geven uw:
- **Details van account**: Accounttype en status van Account
- **Uitgever-id's**: De verkoper-ID, gebruikers-ID, uitgevers-ID, Azure AD-tenants, enzovoort.
- **Contactgegevens**: Weergavenaam van de uitgever, neem contact op met de naam van de verkoper, e-mailadres, telefoonnummer en adres
- **Financiële gegevens**: Betalings-account, btw-profiel en de blokkeringsstatus Payout
- **apparaten**: Alle tests apparaten die zijn gekoppeld aan uw account
- **Bijhouden van GUID's**: Een tracering GUID's koppelen aan uw account

### <a name="account-details"></a>Accountdetails

In het gedeelte Account details ziet u de basisgegevens, zoals uw **accounttype** (bedrijf of individu) en de **verificatiestatus** van uw account. Deze instellingen wordt elke stap vereist, met inbegrip van e-mailverificatie, werkgelegenheid verificatie en verificatie van de zakelijke weergegeven tijdens uw verificatieproces account. U kunt ook hier uw e-mailadres bijwerken en de verificatie opnieuw, indien nodig. 

### <a name="publisher-ids"></a>Uitgever-id 's

In de sectie uitgever-id's, ziet u uw **verkoper**, **MPN ID**, en **uitgevers-ID**. Deze waarden worden toegewezen door Microsoft voor het aanduiden van uw developer-account en kunnen niet worden bewerkt.

### <a name="contact-info"></a>Contactgegevens

In de sectie Neem contact op met gegevens ziet u uw **Publisher weergavenaam**, **verkoper contactgegevens** (de Neem contact op met de naam, e-mailadres, telefoonnummer en adres voor de verkoper bedrijf), en de **bedrijf goedkeurder** (de naam, e-mailadres en telefoonnummer van de persoon met bevoegdheid om goed te keuren beslissingen voor het bedrijf). 

### <a name="financial-details"></a>Financiële gegevens

U kunt opgeven of bijwerken van uw financiële gegevens als u betaalde apps, invoegtoepassingen of services publiceren in de sectie financiële gegevens. 

Als u alleen om gratis aanbiedingen weer te geven, moet u geen betalings-account instellen of een btw-formulieren invullen. Als u later van gedachten veranderen en besluit dat u om te verkopen via Microsoft wilt, kunt u instellen van uw betalings-account en invullen van belastingformulieren op dat moment. 

#### <a name="payout-account"></a>Betalings-account

Een betalings-account is de opbrengst worden verzonden vanuit uw verkoop rekening. Dit account bank moet zich in hetzelfde land waar u uw account Partner Center hebt geregistreerd.

Als u wilt uw betalings-account hebt ingesteld, moet u **uw Microsoft-Account koppelen**:
1. In **Accountinstellingen**onder de **financiële gegevens** sectie, selecteer **uw Microsoft-Account koppelen**. 
2. Wanneer u hierom wordt gevraagd, meldt u zich aan met uw Microsoft-Account (MSA). Dit account kan niet al worden gekoppeld aan een andere Partner Center-account. 
3. U voltooit de installatie van uw betalings-account, log volledig buiten het Partnercentrum, meld u opnieuw aan met uw Microsoft-Account (in plaats van uw werkaccount). 

Nu dat uw Microsoft-Account gekoppeld is, om toe te voegen een betalings-account, gaat u te werk:
- **Kies een betalingsmethode**: Bank- of PayPal
- **Voeg betalingsgegevens**: Dit omvat mogelijk een accounttype (controleren of besparingen) te kiezen, geven de naam van houder, het account en routering, getal, factuuradres, telefoonnummer of e-mailadres of PayPal-nummer. \* Zie voor meer informatie over het gebruik of PayPal-nummer als betalingswijze voor uw account en wilt weten of deze wordt ondersteund in de regio van uw markt [PayPal info](https://docs.microsoft.com/windows/uwp/publish/setting-up-your-payout-account-and-tax-forms#paypal-info).

> [!IMPORTANT]
> Wijzigen van uw betalings-account, kan uw betalingen door maximaal één betalingscyclus uitstellen. Deze vertraging treedt op omdat we nodig hebt om te controleren of het account wijzigen, net zoals we doen wanneer het account payout eerst instelt. U hebt nog steeds te verkopen het volledige bedrag nadat uw account is geverifieerd; alle betalingen vervaldatum voor de huidige betaling cyclus wordt toegevoegd aan de volgende uit.  

#### <a name="tax-profile"></a>Btw-profiel

Controleer uw huidige belasting profielstatus, waaruit blijkt dat de juiste **entiteitstype** en **certificaat belastinggegevens** wordt weergegeven. Selecteer **bewerken** bijwerken of een vereiste formulieren.

Om te kunnen maken van uw btw-status, moet u opgeven welk land minderjarige en citizenship en voltooien van de juiste btw-formulieren die zijn gekoppeld aan uw land/regio.

Ongeacht welk land verblijfplaats of citizenship vult u Verenigde Staten belasting vormen geen aanbiedingen via Microsoft verkopen. Partners die voldoen aan bepaalde vereisten Verenigde Staten moeten een formulier IRS W-9 invullen. Andere partners buiten de Verenigde Staten moeten een formulier IRS W-8 invullen. U vult deze formulieren online als u klaar bent met uw btw-profiel.

Een afzonderlijke fiscaal id-nummer Verenigde Staten (of ITIN) is niet vereist voor het ontvangen van betalingen van Microsoft of te claimen van belasting Verdrag voordelen.

U kunt invullen en verzenden van uw btw-formulieren elektronisch in Partner Center; in de meeste gevallen moet u niet afdrukken en e-formulieren.

Verschillende landen en regio's hebt verschillende btw-vereisten. De exacte waarde die u in belastingen betalen moet, is afhankelijk van de landen en regio's waar het verkopen van uw aanbiedingen. Microsoft terugwijst verkoop- en belasting op uw naam in sommige landen. Deze landen worden aangeduid van de aanbieding van uw aanbieding. In andere landen, afhankelijk van waar u bent geregistreerd, moet u mogelijk afdragen over verkopen en belasting gebruiken voor uw verkoop rechtstreeks naar de lokale belastingautoriteit. Bovendien is de verkoop-opbrengst u ontvangt mogelijk fiscale als inkomsten. We raden u aan contact op met de instantie voor uw land of regio die het beste kunt u bepalen de juiste btw-gegevens voor uw Microsoft-verkoop transacties.

##### <a name="withholding-rates"></a>Tarieven inhouding
De informatie die u in uw btw-formulieren indient, bepaalt de snelheid van de juiste inhouding. De inhouding tarief geldt alleen voor verkoop die u in de Verenigde Staten aanbrengt; verkoop in niet-Amerikaanse locaties vallen niet onder inhouding. De inhouding tarieven verschillen, maar voor de meeste ontwikkelaars registreren buiten de Verenigde Staten, is de snelheid van de standaard 30%. U hebt de mogelijkheid om dit tarief als uw land/regio een Verdrag inkomstenbelasting heeft ingestemd met de Verenigde Staten.

##### <a name="tax-treaty-benefits"></a>Belasting Verdrag voordelen
Als u zich buiten de Verenigde Staten, is het mogelijk dat u kunt profiteren van de belasting Verdrag voordelen. Deze voordelen van land tot land verschillen en kunnen u de belastingen die Microsoft inhoudt te verminderen. U kunt belasting Verdrag voordelen door deel II van het formulier W 8BEN claim. Het is raadzaam dat u contact met de juiste bronnen in uw land of regio opnemen om te bepalen of deze voordelen op u van toepassing.

[Meer informatie over de btw-gegevens voor Windows-app/game-ontwikkelaars en Azure Marketplace-uitgevers](https://docs.microsoft.com/windows/uwp/publish/tax-details-for-paid-apps).

#### <a name="payout-hold-status"></a>De blokkeringsstatus payout

Standaard verzendt Microsoft betalingen op maandbasis. Maar u hebt de optie voor het plaatsen van uw betalingen in de wachtstand, waardoor betalingen te verzenden naar uw account. Als u ervoor uw betalingen in de wachtstand plaatsen kiest, we gaat verder met het registreren van eventuele inkomsten dat u verdient en geef de details in uw **Payout samenvatting**. Er wordt niet betalingen echter verzonden naar uw account totdat u de wachtstand verwijdert. 

Als u wilt uw betalingen in de wachtstand plaatsen, gaat u naar **Accountinstellingen**. Onder **financiële gegevens**, in de **Payout blokkeringsstatus** sectie, de schuifregelaar naar **op**. U kunt de status van uw betalings-blokkering op elk gewenst moment wijzigen, maar houd er rekening mee dat de volgende maandelijkse toekenning is van invloed op uw beslissing. Bijvoorbeeld, als u wilt voor het opslaan van April payout, zorg ervoor dat u de status van uw betalings-wachtstand ingesteld op **op** vóór het einde van maart.

Nadat u hebt ingesteld uw betalings houdt de status **op**, alle betalingen zijn in de wachtstand totdat u de schuifregelaar overzet naar **uit**. Als u dit doet, zult u inbegrepen bij de volgende maandelijkse payout cyclus (op voorwaarde dat alle toepasselijke betaling drempelwaarden wordt voldaan). Bijvoorbeeld, als u al uw betalingen in de wachtstand, maar zou hebben een payout gegenereerd in juni en zorg ervoor dat u het in-/ uitschakelen de toekenning houdt de status **uit** vóór het einde van mei.

> [!NOTE]
> Uw **Payout blokkeringsstatus** selectie van toepassing op **alle** omzet-bronnen die worden betaald via Microsoft Partner Center, met inbegrip van Azure Marketplace, AppSource, Microsoft Store, reclame, enz.). U kunt verschillende wachtstand statussen voor elke bron omzet niet selecteren.

### <a name="devices"></a>Apparaten

De instellingen voor Apparaatbeheer gelden alleen voor UWP-publicatie. [Meer informatie](https://docs.microsoft.com/windows/uwp/publish/manage-account-settings-and-profile#additional-settings-and-info).

### <a name="tracking-guids"></a>Bijhouden van GUID 's

Unieke id's (GUID's) zijn wereldwijd unieke referentie getallen (met 32 hexadecimale cijfers) die kunnen worden gebruikt voor het bijhouden van uw gebruik van Azure. 

Voor het maken van GUID's voor het bijhouden, moet u een GUID-generator. Het Azure Storage-team heeft gemaakt een [GUID generator formulier](https://aka.ms/StoragePartners) die e-mail een GUID van de juiste indeling en kunnen worden hergebruikt binnen de systemen verschillende bijhouden.

U wordt aangeraden u een unieke GUID voor elke aanbieding en de distributie-kanaal voor elk product. U kunt kiezen voor het gebruik van een enkele GUID voor meerdere distributiekanalen van het product als u niet wilt dat reporting om te worden verdeeld.

Als u een product implementeren met behulp van een sjabloon en deze beschikbaar op de Azure Marketplace en GitHub is, kunt u maken en registreren van 2 afzonderlijke GUID's:

*   Product A in Azure Marketplace
*   Product een op GitHub

Reporting wordt uitgevoerd door de partner-waarde (Microsoft Partner-ID) en de GUID's. Ook kunt u bijhouden van GUID's op een meer gedetailleerd niveau uitlijnen op elk abonnement binnen uw aanbieding.

Zie voor meer informatie de [Azure bijhouden van klanten te worden gebruikt met veelgestelde vragen over de GUID's](https://docs.microsoft.com/azure/marketplace/azure-partner-customer-usage-attribution#faq).



## <a name="multi-user-account-management"></a>Meerdere gebruikers accountbeheer

Partner Center maakt gebruik van [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) (Azure AD) voor meerdere gebruikers toegang tot en beheer. Uw organisatie Azure AD als onderdeel van het registratieproces automatisch gekoppeld aan uw Partner Center-account is. 

## <a name="manage-users"></a>Gebruikers beheren

De **gebruikers** sectie van de Partner Center (onder **Accountinstellingen**) gaan we het gebruik van Azure AD voor het beheren van de gebruikers, groepen en Azure AD-toepassingen die toegang tot uw account Partner Center hebben. Houd er rekening mee dat als u wilt beheren van gebruikers, moet u zijn aangemeld met uw [werkaccount](./company-work-accounts.md) (de bijbehorende Azure AD-tenant). Voor het beheren van gebruikers in een ander werkaccount / tenant, moet u zich afmelden en weer aanmelden als een gebruiker met **Manager** machtigingen op die werkaccount / tenant. 

Nadat u bent aangemeld met uw werkaccount (Azure AD-tenant), kunt u het volgende doen:
- [Gebruikers toevoegen of verwijderen](#add-or-remove-users)
- [Gebruikerswachtwoord wijzigen](#change-a-user-password)
- [Toevoegen of verwijderen van groepen](#add-or-remove-users)
- [Toevoegen of verwijderen van Azure AD-toepassingen](#add-new-azure-ad-applications)
- [Sleutels voor een Azure AD-toepassing beheren](#manage-keys-for-an-azure-ad-application)
- [Gebruikersrollen en machtigingen definiëren](#define-user-roles-and-permissions)


Houd er rekening mee dat alle Partnercentrum-gebruikers (met inbegrip van groepen en toepassingen voor Azure AD) moeten een actieve werkaccount hebben in een [Azure AD-tenant](#manage-tenants) dat is gekoppeld aan uw Partner Center-account. 

### <a name="add-or-remove-users"></a>Gebruikers toevoegen of verwijderen

Uw account moet beschikken over [ **Manager op serverniveau** ](#define-user-roles-and-permissions) machtigingen voor de [werkaccount (Azure AD-tenant)](./company-work-accounts.md) waarin u wilt toevoegen of bewerken van gebruikers.

#### <a name="add-existing-users"></a>Bestaande gebruikers toevoegen

Gebruikers toevoegen aan uw Partner Center-account die al bestaan in uw bedrijf [werkaccount (Azure AD-tenant)](./company-work-accounts.md):

1. Ga naar **gebruikers** (onder **Accountinstellingen**) en selecteer **gebruikers toevoegen**.
2. Selecteer een of meer gebruikers in de lijst die wordt weergegeven. U kunt het zoekvak gebruiken om te zoeken naar specifieke gebruikers.
\* Als u meer dan één gebruiker toevoegen aan uw Partner Center-account selecteert, moet u ze toewijzen dezelfde rol of aangepaste machtigingen. Als u wilt toevoegen van meerdere gebruikers met verschillende rolmachtigingen, Herhaal deze stappen voor elke functie of een set met aangepaste machtigingen.
3.  Wanneer u klaar bent met gebruikers kiezen, klikt u op **toevoegen geselecteerd**.
5.  In de **rollen** sectie, geeft u de rollen zijn of aangepaste machtigingen voor de geselecteerde gebruiker (s).
6.  Selecteer **Opslaan**.

#### <a name="create-new-users"></a>Nieuwe gebruikers maken

Voor het maken van nieuwe gebruikersaccounts, moet u een account met hebben [ **hoofdbeheerder** ](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) machtigingen. 

1. Ga naar **gebruikers** (onder **Accountinstellingen**), selecteer **gebruikers toevoegen**, en kies vervolgens **maken van nieuwe gebruikers**.
1. Voer een voornaam, achternaam en een gebruikersnaam voor elke nieuwe gebruiker. 
1. Als u wilt dat de nieuwe gebruiker aan een account voor globale beheerders hebben in de adreslijst van uw organisatie, schakel het selectievakje **zodat deze gebruiker een globale beheerder in uw Azure AD, met volledig beheer over alle mapresources**. Hiermee krijgt de gebruiker volledige toegang tot alle beheerfuncties in uw bedrijf van Azure AD. Ze zult kunnen toevoegen en beheren van gebruikers in uw organisatie werkaccount (Azure AD-tenant), maar niet in Partner Center, tenzij u het account de juiste rol/machtigingen verlenen. 
1. Als u het selectievakje ingeschakeld **zodat deze gebruiker een globale beheerder**, moet u voor een **wachtwoord recovery e** voor de gebruiker om te herstellen van het wachtwoord indien nodig.
1. In de **groepslidmaatschap** sectie, selecteer de groepen die u wilt dat de nieuwe gebruiker horen.
1. In de **rollen** sectie, geeft u de rollen zijn of aangepaste machtigingen voor de gebruiker.
1. Selecteer **Opslaan**.

Het maken van een nieuwe gebruiker in de Partner Center maakt ook een account voor die gebruiker het werkaccount (Azure AD-tenant) waaraan u bent aangemeld. Wijzigingen aanbrengen in de naam van een gebruiker in de Partner Center, zal de dezelfde wijzigingen aanbrengen in uw organisatie werkaccount (Azure AD-tenant).

#### <a name="invite-new-users-by-email"></a>Nieuwe gebruikers uitnodigen via e-mail

Als u wilt uitnodigen gebruikers die momenteel niet een deel van uw bedrijf werkaccount (Azure AD-tenant) via e-mail, moet u een account met hebben [ **hoofdbeheerder** ](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) machtigingen. 

1. Ga naar **gebruikers** (onder **Accountinstellingen**), selecteer **gebruikers toevoegen**, en kies vervolgens **gebruikers uitnodigen via e-mail**.
2. Voer een of meer e-mailadressen (maximaal tien), gescheiden door komma's of puntkomma's.
3. In de **rollen** sectie, geeft u de rollen zijn of aangepaste machtigingen voor de gebruiker.
4. Selecteer **Opslaan**.

De gebruikers die u hebt uitgenodigd krijgt een e-mailbericht naar het deelnemen aan uw Partner Center-account. Een nieuwe gastgebruiker account wordt in uw werkaccount (Azure AD-tenant) worden gemaakt. Elke gebruiker moet de uitnodiging accepteren voordat ze krijgen uw account tot toegang.

Als u een uitnodiging opnieuw verzenden wilt, gaat u naar de **gebruikers** pagina, de uitnodiging niet vinden in de lijst met gebruikers, selecteert u het e-mailadres (of de tekst waarin wordt gemeld *uitnodiging in behandeling*). Selecteer vervolgens aan de onderkant van de pagina **uitnodiging opnieuw verzenden**.
 

> [!NOTE]
> Als uw organisatie gebruikmaakt van [adreslijstintegratie](https://go.microsoft.com/fwlink/p/?LinkID=724033) voor de on-premises Active Directory met uw Azure AD worden gesynchroniseerd, kunt u zich niet aan het maken van nieuwe gebruikers, groepen of Azure AD-toepassingen in de Partner Center. U (of een andere beheerder in uw on-premises directory) moet rechtstreeks in de on-premises directory maken, voordat u zult kunnen zien en deze toevoegen in het Partnercentrum.

#### <a name="remove-a-user"></a>Een gebruiker verwijderen

Als u wilt een gebruiker verwijderen uit uw werkaccount (Azure AD-tenant), gaat u naar **gebruikers** (onder **Accountinstellingen**), selecteer de gebruiker die u wilt verwijderen met behulp van het selectievakje in de kolom helemaal rechts, kies vervolgens  **Verwijder** uit de beschikbare acties. Een pop-upvenster wordt weergegeven voor u om te bevestigen dat u wilt verwijderen van de geselecteerde gebruiker (s).

#### <a name="change-a-user-password"></a>Gebruikerswachtwoord wijzigen

Als een van uw gebruikers nodig heeft om hun wachtwoord te wijzigen, ze kunnen doen zich als u hebt opgegeven een **wachtwoord recovery e** bij het maken van het gebruikersaccount. U kunt ook het wachtwoord van een gebruiker bijwerken door de onderstaande stappen te volgen. Als u wilt wijzigen van het wachtwoord van een gebruiker in uw bedrijf work account (Azure AD-tenant), moet u zijn aangemeld op een account met [ **hoofdbeheerder** ](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) machtigingen. Houd er rekening mee dat Hiermee wijzigt u het wachtwoord van de gebruiker in uw Azure AD-tenant, samen met het wachtwoord die ze gebruiken voor toegang tot Partner Center.

1.  Uit de **gebruikers** pagina (onder **Accountinstellingen**), selecteer de naam van het gebruikersaccount dat u wilt bewerken.
2.  Selecteer de **wachtwoord opnieuw instellen** knop aan de onderkant van de pagina.
3.  Een bevestigingspagina wordt weergegeven met de aanmeldingsgegevens voor de gebruiker, met inbegrip van een tijdelijk wachtwoord. Als u niet mogelijk toegang tot het tijdelijke wachtwoord nadat u deze pagina verlaat, worden zorg ervoor dat u afdrukken of kopiëren van deze gegevens en deze aan de gebruiker opgeven.


## <a name="manage-groups"></a>Groepen beheren

Met behulp van groepen kunt u meerdere rollen en machtigingen samen kunt beheren.

#### <a name="add-an-existing-group"></a>Een bestaande groep toevoegen

Om toe te voegen aan dat een groep die al bestaat in uw organisatie werkaccount (Azure AD-tenant) geven aan uw account Partner Center: 

1.  Uit de **gebruikers** pagina (onder **Accountinstellingen**), selecteer **groepen toevoegen**.
2.  Selecteer een of meer groepen in de lijst die wordt weergegeven. U kunt het zoekvak gebruiken om te zoeken naar specifieke groepen.
Als u meer dan één groep toe te voegen aan uw Partner Center-account selecteert, moet u ze toewijzen dezelfde rol of aangepaste machtigingen. Als u wilt toevoegen van meerdere groepen met verschillende rolmachtigingen, Herhaal deze stappen voor elke functie of een set met aangepaste machtigingen.
3.  Wanneer u klaar bent met groepen te kiezen, klikt u op **toevoegen geselecteerd**.
4.  In de **rollen** sectie, geeft u de rollen zijn of aangepaste machtigingen voor de geselecteerde groepen. Alle leden van de groep zich toegang tot uw account Partner Center met de machtigingen dat u toepassen op de groep, ongeacht de rollen en machtigingen die zijn gekoppeld aan hun afzonderlijke account.
5.  Selecteer **Opslaan**.

Wanneer u een bestaande groep toevoegt, wordt elke gebruiker die lid is van die groep toegang hebben tot uw account Partner Center met de machtigingen die zijn gekoppeld aan de toegewezen rol van de groep zijn.

#### <a name="add-a-new-group"></a>Een nieuwe groep toevoegen

Een gloednieuwe groep toevoegen aan uw Partner Center-account: 

1.  Uit de **gebruikers** pagina (onder **Accountinstellingen**), selecteer **groepen toevoegen**.
2.  Selecteer op de volgende pagina **nieuwe groep**.
3.  Voer de weergavenaam voor de nieuwe groep.
4.  Geef de rollen zijn of aangepaste machtigingen voor de groep. Alle leden van de groep zich toegang tot uw account Partner Center met de machtigingen voor dat u zijn hier van toepassing, ongeacht de rolmachtigingen/hun afzonderlijke account zijn gekoppeld.
5.  Selecteer de gebruiker (s) voor de nieuwe groep in de lijst die wordt weergegeven. U kunt het zoekvak gebruiken om te zoeken naar specifieke gebruikers.
6.  Wanneer u klaar bent met gebruikers te selecteren, klikt u op **toevoegen geselecteerd** aan de nieuwe groep wilt toevoegen.
7.  Selecteer **Opslaan**.

Houd er rekening mee dat de nieuwe groep wordt gemaakt in uw organisatie werkaccount (Azure AD-tenant), niet alleen in uw Partner Center-account.

#### <a name="remove-a-group"></a>Een groep verwijderen

Als u wilt een groep verwijderen uit uw werkaccount (Azure AD-tenant), gaat u naar **gebruikers** (onder **Accountinstellingen**), selecteer de groep die u wilt verwijderen met behulp van het selectievakje in de kolom helemaal rechts en kies vervolgens  **Verwijder** uit de beschikbare acties. Een pop-upvenster wordt weergegeven voor u om te bevestigen dat u wilt verwijderen van de geselecteerde groepen.

## <a name="manage-azure-ad-applications"></a>Azure AD-toepassingen beheren

U kunt toepassingen of services die deel van uw bedrijf Azure uitmaken AD voor toegang tot uw account Partner Center. 

#### <a name="add-existing-azure-ad-applications"></a>Bestaande Azure AD-toepassingen toevoegen 

Als u wilt toevoegen aanwezig toepassingen die al in de Azure Active Directory van uw bedrijf: 

1.  Uit de **gebruikers** pagina (onder **Accountinstellingen**), selecteer **Azure AD-toepassingen toevoegen**.
2.  Selecteer een of meer Azure AD-toepassingen in de lijst die wordt weergegeven. U kunt het zoekvak gebruiken om te zoeken naar specifieke Azure AD-toepassingen. Als u meer dan één Azure AD-toepassing toevoegen aan uw Partner Center-account selecteert, moet u ze toewijzen dezelfde rol of aangepaste machtigingen. Als u wilt toevoegen van meerdere Azure AD-toepassingen met verschillende rolmachtigingen, Herhaal deze stappen voor elke functie of een set met aangepaste machtigingen.
3.  Wanneer u klaar bent met Azure AD-toepassingen te selecteren, klikt u op **toevoegen geselecteerd**.
5.  In de **rollen** sectie, geeft u de rollen zijn of aangepaste machtigingen voor de geselecteerde Azure AD-toepassingen.
6.  Selecteer **Opslaan**.

#### <a name="add-new-azure-ad-applications"></a>Toevoegen van nieuwe Azure AD-toepassingen 

Als u wilt dat Partner Center toegang verlenen tot een gloednieuwe Azure AD-toepassing-account, kunt u in de **gebruikers** sectie. Houd er rekening mee dat hiermee een nieuw account in uw bedrijf work account (Azure AD-tenant), niet alleen in uw Partner Center-account wordt gemaakt. Als u deze Azure AD-toepassing voornamelijk voor verificatie van de Partner Center gebruikt en hoeft geen gebruikers deze rechtstreeks toegang hebben tot, kunt u een geldig adres voor de **antwoord-URL** en **App ID URI**, zo lang Als deze waarden worden niet gebruikt door een andere Azure AD-toepassing in uw directory.

1.  Uit de **gebruikers** pagina (onder **Accountinstellingen**), selecteer **Azure AD-toepassingen toevoegen**.
2.  Selecteer op de volgende pagina **New Azure AD-toepassing**.
3.  Voer de **antwoord-URL** voor de nieuwe Azure AD-toepassing. Dit is de URL waar gebruikers kunnen zich aanmelden en gebruikt uw Azure AD-toepassing (soms ook wel bekend als de App-URL of de aanmeldings-URL genoemd). De **antwoord-URL** mag niet langer zijn dan 256 tekens bestaan en moet uniek zijn binnen uw directory.
4.  Voer de **App ID URI** voor de nieuwe Azure AD-toepassing. Dit is een logische id voor de Azure AD-toepassing die wordt weergegeven wanneer een aanvraag voor eenmalige aanmelding wordt verzonden naar Azure AD. Houd er rekening mee dat de **App ID URI** moet uniek zijn voor elke Azure AD-toepassing in uw directory. Deze ID kan niet langer zijn dan 256 tekens. Zie voor meer informatie over de URI van de App-ID [toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/quickstart-modify-supported-accounts#change-the-application-registration-to-support-different-accounts).
5.  In de **rollen** sectie, geeft u de rollen zijn of aangepaste machtigingen voor de Azure AD-toepassing.
6.  Selecteer **Opslaan**.

Na het toevoegen of een Azure AD-toepassing maken, kunt u terugkeren naar de **gebruikers** sectie en selecteer de naam van de toepassing om instellingen voor de toepassing, met inbegrip van de Tenant-ID, de Client-ID, de antwoord-URL en het App-ID-URI te controleren.

#### <a name="remove-an-application"></a>Een toepassing verwijderen

Als u wilt een toepassing verwijderen uit uw werkaccount (Azure AD-tenant), gaat u naar **gebruikers** (onder **Accountinstellingen**), selecteer de toepassing die u wilt verwijderen met behulp van het selectievakje in de kolom helemaal rechts Kies vervolgens **verwijderen** uit de beschikbare acties. Een pop-upvenster wordt weergegeven voor u om te bevestigen dat u wilt verwijderen van de geselecteerde toepassingen.

#### <a name="manage-keys-for-an-azure-ad-application"></a>Sleutels voor een Azure AD-toepassing beheren

Als uw Azure AD-toepassing gegevens leest en in Microsoft Azure AD schrijft, moet dit een sleutel. U kunt de sleutels voor een Azure AD-toepassing maken door de informatie in de Partner Center te bewerken. U kunt ook sleutels die zijn niet langer nodig verwijderen.

1.  Uit de **gebruikers** pagina (onder **Accountinstellingen**), selecteer de naam van de Azure AD-toepassing. U ziet alle actieve sleutels voor de Azure AD-toepassing, met inbegrip van de datum waarop de sleutel is gemaakt en wanneer het verloopt. 
2. Als u wilt verwijderen van een sleutel die is niet meer nodig hebt, selecteert u **verwijderen**.
3.  Selecteer om een nieuwe sleutel toe **nieuwe sleutel toevoegen**.
4.  U ziet een scherm van de **Client-ID** en **waarden sleutel**. Als u het niet mogelijk toegang tot het opnieuw nadat u deze pagina verlaat, worden zorg ervoor dat u afdrukken of kopiëren van deze informatie.
4.  Als u meer-sleutels maken wilt, selecteert u **toevoegen op een andere toets**.


### <a name="define-user-roles-and-permissions"></a>Gebruikersrollen en machtigingen definiëren

Gebruikers van uw bedrijf kunnen worden toegewezen, de volgende rollen en machtigingen in de commerciële Marketplace-programma op Partner Center. 

Houd er rekening mee Azure Active Directory (AAD) tenant functies omvatten globale beheerder, Gebruikerbeheerder en CSP-rollen. Niet-AAD-rollen worden de rollen die de tenant niet beheren en ze MPN admin, admin voor bedrijven-profiel, referral-beheerder, incentive admin en stimulerend gebruiker bevatten.


|**Rol**|**Machtigingen**|
|----------------------------------|:---------------------------------|
|Globale beheerder|• Toegang hebben tot alle Microsoft-account/services met volledige bevoegdheden
|      |• Maken van ondersteuningstickets voor het Partnercentrum
||• Weergave overeenkomsten, prijslijsten en aanbiedingen
||• Weergave maken en beheren van gebruikers van partnerorganisaties|
|Manager|• Toegang tot alle functies van Microsoft-account met uitzondering van de belasting en betalings-instellingen
|      |• Kunnen gebruikers, rollen, beheren en werkaccounts (tenants)|
|Developer|• Kunt uploaden pakketten, apps en invoegtoepassingen indienen en het gebruiksrapport voor telemetrie over weergeven
|      |• Geen toegang tot financiële informatie of account-instellingen|
|Inzender voor bedrijven|• Kunnen toegang krijgen tot financiële gegevens en stel prijsinformatie
|      |• Niet maken of nieuwe apps en invoegtoepassingen|
|Financieel medewerker|• Kunt betalings-rapporten weergeven
|      |• Kan geen wijzigingen aanbrengen aan apps of -instellingen|
|Marketer|• Kunnen reageren op beoordelingen van de klant en niet-financiële rapporten
|      |• Kan geen wijzigingen aanbrengen aan apps of -instellingen|

Zie voor meer informatie over het beheren van rollen en machtigingen in andere gebieden van de Partner Center, zoals Azure Active Directory (AD), Cloud Solution Provider (CSP), het deelvenster leverancier (CPV), gastgebruikers of Microsoft Partner Network (MPN), [toewijzen gebruikers, rollen en machtigingen in Partner Center](https://docs.microsoft.com/partner-center/permissions-overview).


## <a name="manage-tenants"></a>Tenants beheren

Een Azure Active Directory (AD)-tenant, ook wel de 'werkaccount' in deze documentatie is een weergave van uw organisatie instellen in Azure portal en helpt u bij het beheren van een specifiek exemplaar van Microsoft cloud-services voor uw interne en externe gebruikers. Als uw organisatie is geabonneerd op een Microsoft-cloudservice, zoals Azure, Microsoft Intune of Office 365, wordt een Azure AD-tenant is geladen voor u. 

U kunt meerdere tenants instellen voor gebruik met de Partner Center. Elke gebruiker met de **Manager** rol in de Partner Center-account hebben de mogelijkheid toevoegen en verwijderen van Azure AD-tenants uit het account.  

### <a name="add-an-existing-tenant"></a>Een bestaande tenant toevoegen

Een andere Azure AD-tenant koppelen aan uw Partner Center-account:

1.  Uit de **Tenants** pagina (onder **Accountinstellingen**), selecteer **koppelen van een andere Azure AD-tenant**.
2. Voer uw Azure AD-referenties voor de tenant die u wilt koppelen.
3. Controleer de naam van de organisatie en het domein voor uw Azure AD-tenant. Selecteer voor het voltooien van de koppeling **bevestigen**.

Als de koppeling geslaagd is, klikt u vervolgens moet u gereed voor toevoegen en beheren van gebruikers van de account in de **gebruikers** sectie in het Partnercentrum.

### <a name="create-a-new-tenant"></a>Een nieuwe tenant maken

Een merk nieuwe Azure AD-tenant met uw Partner Center-account maken:

1.  Uit de **Tenants** pagina (onder **Accountinstellingen**), selecteer **maken een nieuwe Azure AD-tenant**.
2. Voer in de directory-informatie voor uw nieuwe Azure AD:
    - **Domeinnaam**: De unieke naam die worden gebruikt voor uw Azure AD-domein, samen met '. onmicrosoft.com '. Bijvoorbeeld, als u "voorbeeld" hebt ingevoerd, is uw Azure AD-domein 'example.onmicrosoft.com'.
    - **Neem contact op met de e-mailbericht**: Een e-mailadres waar we kunnen contact met u opnemen over uw account indien nodig.
    - **Globale beheerder gebruikersaccountgegevens**: De voornaam, laatste naam, gebruikersnaam en wachtwoord die u wilt gebruiken voor het nieuwe account voor globale beheerder.
3. Selecteer **maken** om te bevestigen van de gegevens van de nieuwe domein en -account.
4. Meld u aan met uw nieuwe Azure AD-hoofdbeheerder gebruikersnaam en wachtwoord om te beginnen met [toevoegen en beheren van gebruikers](#manage-users).

Zie het artikel voor meer informatie over het maken van nieuwe tenants in uw Azure-portal, in plaats van via de portal Partnercentrum [een nieuwe tenant maken in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant).

### <a name="remove-a-tenant"></a>Een tenant verwijderen

Als u wilt verwijderen van een tenant van uw Partner Center-account, de naam worden gevonden op de **Tenants** pagina (in **Accountinstellingen**) en selecteer vervolgens **verwijderen**. U wordt gevraagd om te bevestigen dat u wilt verwijderen van de tenant. Zodra u dit doet, kunnen gebruikers in deze tenant zich kunt aanmelden bij de Partner Center-account en de machtigingen die u hebt geconfigureerd voor deze gebruikers worden verwijderd.

Wanneer u een tenant verwijdert, kunnen alle gebruikers die zijn toegevoegd aan de Partner Center-account van deze tenant is niet meer aanmelden bij het account.

> [!TIP]
> U kunt een tenant niet verwijderen als u bent momenteel aangemeld bij Partner Center met behulp van een account in dezelfde tenant. Als u wilt verwijderen van een tenant, moet u zich aanmelden bij de Partner Center als een **Manager** voor een andere tenant die is gekoppeld aan het account. Als er slechts één tenant die is gekoppeld aan het account, kunt die tenant alleen worden verwijderd na het aanmelden met het Microsoft-account dat het account wordt geopend.


## <a name="agreements"></a>Overeenkomsten

De **overeenkomsten** sectie van de Partner Center (onder **Accountinstellingen**) laten we u een lijst weergeven met de publicatie overeenkomsten die u hebt gemachtigd. Deze overeenkomsten worden weergegeven op basis van naam en versienummer, met inbegrip van de datum waarop die deze is geaccepteerd en de naam van de gebruiker die de overeenkomst geaccepteerd. 

**Acties die nodig zijn** boven aan deze pagina kan worden weergegeven als er updates overeenkomst die uw aandacht. Voor het accepteren van een bijgewerkte overeenkomst, eerst lezen van de gekoppelde versie van de overeenkomst, en selecteer vervolgens **overeenkomst te accepteren**. 

Voor informatie over de overeenkomsten in Partnercentrum Cloud Solution Provider (CSP), gaat u naar [Microsoft Cloud-overeenkomsten per regio en taal](https://docs.microsoft.com/partner-center/agreements).

## <a name="next-steps"></a>Volgende stappen

- [Een nieuwe SaaS-aanbieding maken](./create-new-saas-offer.md)
