---
title: Ontwerp van hybride identiteit - strategie voor gegevensbescherming Azure | Microsoft Docs
description: Definieert u de strategie voor gegevensbescherming voor uw oplossing voor hybride identiteit om te voldoen aan de bedrijfsvereisten die u hebt gedefinieerd.
documentationcenter: 
services: active-directory
author: billmath
manager: mtillman
editor: 
ms.assetid: e76fd1f4-340a-492a-84d9-e05f3b7cc396
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/13/2017
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: d43be976f9b3fae7f3cbec1a0033f1a401ede896
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>Definieer de strategie voor gegevensbescherming voor uw oplossing voor hybride identiteit
In deze taak definieert u de strategie voor gegevensbescherming voor uw oplossing voor hybride identiteit om te voldoen aan de bedrijfsvereisten die u hebt gedefinieerd in:

* [Bepalen van de beveiligingsvereisten voor gegevens](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
* [Vereisten voor inhoudsbeheer bepalen](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
* [Vereisten voor toegangsbeheer bepalen](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
* [Vereisten voor respons op incidenten bepalen](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>Definieer de opties voor beveiliging
Zoals uitgelegd in [directory-synchronisatievereisten bepalen](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md), Microsoft Azure AD kan worden gesynchroniseerd met uw lokale Active Directory Domain Services (AD DS). Deze integratie kan organisaties met Azure AD om te controleren of de referenties van gebruikers wanneer zij probeert toegang tot bedrijfsbronnen. U kunt dit doen voor beide scenario's: de gegevens op rest lokaal en in de cloud. Toegang tot gegevens in Azure AD vereist verificatie van de gebruiker via een beveiligingstokenservice (STS).

Eenmaal is geverifieerd, wordt de UPN (user Principal name) lezen uit het verificatietoken. Vervolgens bepaalt het autorisatiesysteem het gerepliceerde partitie en de container die overeenkomt met het domein van de gebruiker. Klik op de bestaan, de ingeschakelde status en de rol van de gebruiker informatie helpt de autorisatiesysteem bepalen of de toegang tot de doel-tenant in die sessie voor de gebruiker is gemachtigd. Bepaalde geautoriseerde acties (in het bijzonder maken en het wachtwoord opnieuw instellen) maken van een audittrail die een tenantbeheerder wordt vervolgens gebruikt om naleving inspanningen of onderzoek te beheren.

Zwevend gegevens uit uw on-premises datacentrum in Azure Storage via een internetverbinding altijd mogelijk niet uitvoerbaar is als gevolg van gegevensvolume, beschikbare bandbreedte of andere overwegingen. De [Azure Storage Import/Export-Service](../storage/common/storage-import-export-service.md) biedt een optie op basis van hardware voor plaatsen/ophalen van grote hoeveelheden gegevens in blob storage. Hiermee kunt u verzenden [BitLocker-versleuteling](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2) harde schijven rechtstreeks naar een Azure-datacenter waar cloudoperators de inhoud uploaden naar uw storage-account of ze kunnen uw Azure gegevens downloaden naar uw stations naar u terug te keren. Alleen versleutelde schijven worden geaccepteerd voor dit proces (met een BitLocker-sleutel die is gegenereerd door de service zelf tijdens de installatie van de taak). De BitLocker-sleutel wordt geleverd aan Azure afzonderlijk, zodat de buiten-band sleutel delen.

Omdat gegevens die onderweg kunnen worden uitgevoerd in verschillende scenario's, is ook relevant te weten dat gebruikmaakt van Microsoft Azure [virtuele netwerken](https://azure.microsoft.com/documentation/services/virtual-network/) voor het isoleren van verkeer van de tenants van elkaar, zoals host - en gastniveau maatregelen nemen firewalls, pakketfilters IP-poorten blokkeren en HTTPS-eindpunten. De meeste interne communicatie van Azure, inclusief infrastructuur-infrastructuur- en infrastructuur klant (lokaal), zijn echter ook versleuteld. Een ander belangrijk scenario is de communicatie binnen de Azure-datacenters; Microsoft beheert netwerken om ervoor te zorgen dat er geen VM kunt imiteren of op het IP-adres van een andere afluisteren. TLS/SSL wordt gebruikt bij het openen van Azure Storage of de SQL-Databases, of wanneer u verbinding maakt met Cloud-Services. In dit geval wordt is de beheerder van de klant verantwoordelijk voor het verkrijgen van een TLS/SSL-certificaat en geïmplementeerd op de tenant-infrastructuur. Gegevensverkeer tussen virtuele Machines in dezelfde implementatie of verplaatst tussen de tenants in een enkele implementatie via Microsoft Azure Virtual Network kan worden beveiligd via protocollen zoals HTTPS, SSL/TLS of anderen gecodeerde communicatie.

Afhankelijk van hoe u de vragen in beantwoord [gegevensbeveiligingsvereisten bepalen](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md), moet u kunnen bepalen hoe u wilt uw gegevens beschermen en hoe de oplossing voor hybride identiteit kan u helpen bij dit proces. De volgende tabel bevat de ondersteund door Azure opties die beschikbaar voor elk scenario van data protection zijn.

| Opties voor beveiliging van gegevens | In rust in de cloud | Op de rest lokale | Onderweg |
| --- | --- | --- | --- |
| BitLocker-stationsversleuteling |X |X | |
| SQL Server voor het versleutelen van databases |X |X | |
| VM-VM-versleuteling | | |X |
| SSL/TLS | | |X |
| VPN | | |X |

> [!NOTE]
> Lees [naleving door functie](https://azure.microsoft.com/support/trust-center/services/) op [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) voor meer informatie over de certificaten die voldoet aan elke Azure-service.
> Omdat de opties voor bescherming van een gelaagde benadering, vergelijking tussen deze opties zijn niet geldig voor deze taak. Zorg ervoor dat u gebruik van alle opties die beschikbaar zijn voor elke status van de gegevens.
>
>

## <a name="define-content-management-options"></a>Definieer de opties voor inhoudbeheer
Een voordeel van het gebruik van Azure AD voor het beheren van een infrastructuur voor hybride identiteit is dat het proces volledig transparant vanuit het perspectief van de eindgebruiker is. De gebruiker probeert te krijgen tot een gedeelde bron, de resource vereist verificatie, de gebruiker heeft een verificatieaanvraag naar Azure AD om de token verkrijgen en toegang tot de bron te verzenden. Dit gehele proces wordt uitgevoerd op de achtergrond, zonder tussenkomst van de gebruiker. Het is ook mogelijk machtiging verlenen om te een [groep](active-directory-manage-groups.md#getting-started-with-access-management) gebruikers zodat zij bepaalde algemene acties uit te voeren.

Organisaties die zorgen te maken over de privacy van gegevens meestal zijn vereisen gegevensclassificatie voor hun oplossing. Als de huidige on-premises infrastructuur gegevensclassificatie al wordt gebruikt, is het mogelijk te gebruiken van Azure AD als de belangrijkste opslagplaats voor de identiteit van de gebruiker. Een algemene hulpprogramma dat het gebruikte lokaal is voor gegevensclassificatie heet [Data Classification Toolkit](https://msdn.microsoft.com/library/Hh204743.aspx) voor Windows Server 2012 R2. Dit hulpprogramma kunt u identificeren, classificeren en beveiligen van gegevens op bestandsservers in uw privécloud. Het is ook mogelijk gebruik van de [automatische Bestandsclassificatie](https://technet.microsoft.com/library/hh831672.aspx) in Windows Server 2012 deze taak uit te voeren.

Als uw organisatie geen gegevensclassificatie in plaats, maar gevoelige bestanden beveiligen moet zonder nieuwe Servers lokale toe te voegen, kunnen ze gebruiken Microsoft [Azure Rights Management Service](https://technet.microsoft.com/library/JJ585026.aspx).  Azure RMS gebruikt voor versleuteling, identiteit en autorisatiebeleid om te helpen uw bestanden en e-mail beveiligen en het werkt op meerdere apparaten, telefoons, tablets en pc's. Omdat Azure RMS een cloudservice is, hoeft niet expliciet vertrouwensrelaties met andere organisaties configureren voordat u beveiligde inhoud met hen kunt delen. Als ze al een Office 365 of Azure AD-directory, wordt samenwerking tussen verschillende organisaties automatisch ondersteund. Ook kunt u alleen de mapkenmerken die Azure RMS ondersteuning bieden voor een algemene identiteit voor uw lokale Active Directory-accounts moet met behulp van Azure Active Directory-Synchronisatieservices (AAD Sync) of Azure AD Connect synchroniseren.

Een essentieel onderdeel van inhoudsbeheer is om te begrijpen wie welke resource, een uitgebreide logboekregistratie kunnen is daarom belangrijk voor de oplossing voor identiteitsbeheer. Azure AD levert logboek meer dan 30 dagen, met inbegrip van:

* Wijzigingen in het rollidmaatschap (ex: gebruiker is toegevoegd aan de rol globale beheerder)
* Referentie-updates (ex: wachtwoordwijzigingen)
* Domeinbeheer (ex: een aangepast domein, verwijderen van een domein controleren)
* Het toevoegen of verwijderen van toepassingen
* Gebruikersbeheer (ex: toevoegen, verwijderen, het bijwerken van een gebruiker)
* Het toevoegen of verwijderen van licenties

> [!NOTE]
> Lees [Microsoft Azure-beveiliging en Audit Log-beheer](http://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf) voor meer informatie over mogelijkheden voor logboekregistratie in Azure.
> Afhankelijk van hoe u de vragen in beantwoord [inhoudsbeheer vereisten bepalen](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md), moet u kunnen bepalen hoe wilt u de inhoud die moet worden beheerd in uw oplossing voor hybride identiteit. Hoewel alle opties die beschikbaar zijn in tabel 6 kan worden geïntegreerd met Azure AD, is het belangrijk om te bepalen die geschikter zijn voor uw bedrijfsbehoeften.
>
>

| Opties voor inhoudbeheer | Voordelen | Nadelen |
| --- | --- | --- |
| Gecentraliseerd op locatie (Active Directory Rights Management Server) |Volledige controle over de infrastructuur van de server die verantwoordelijk is voor het classificeren van de gegevens <br> Ingebouwde mogelijkheden in Windows Server is niet nodig om extra licentie of abonnement <br> Kan worden geïntegreerd met Azure AD in een hybride scenario <br> Ondersteunt mogelijkheden voor information rights management (IRM) in Microsoft Online services, zoals Exchange Online en SharePoint Online, evenals Office 365 <br> Biedt ondersteuning voor lokale Microsoft server-producten, zoals Exchange Server, SharePoint Server en bestandsservers waarop Windows Server en infrastructuur voor Bestandsclassificatie (FCI). |Hogere onderhoud (gelijke tred houden met updates, configuratie en mogelijke upgrades), sinds IT eigenaar is van de Server <br> Vereist een serverinfrastructuur lokale<br> Mogelijkheden van Azure niet systeemeigen gebruikmaken |
| Gecentraliseerd in de cloud (Azure RMS) |Gemakkelijker te beheren in vergelijking met de on-premises-oplossing <br> Kan worden geïntegreerd met AD DS in een hybride scenario <br>  Volledig geïntegreerd met Azure AD <br> Een server on-premises, voor het implementeren van de service nodig niet <br> Ondersteunt on-premises Microsoft server-producten zoals Exchange Server, SharePoint Server en bestandsservers waarop Windows Server en File Classification, Infrastructure (FCI) <br> IT, kan volledige controle over hun tenantsleutel met BYOK-functionaliteit hebben. |Uw organisatie moet een cloudabonnement hebben met RMS ondersteunt <br> Uw organisatie moet een Azure Active directory voor de ondersteuning van gebruikersverificatie voor RMS hebben |
| Hybride (Azure RMS is geïntegreerd met, On-Premises Active Directory Rights Management Server) |Dit scenario een tijdje staan de voordelen van beide, gecentraliseerd op locatie en in de cloud. |Uw organisatie moet een cloudabonnement hebben met RMS ondersteunt <br> Uw organisatie moet een Azure AD-directory ter ondersteuning van gebruikersverificatie voor RMS, hebben <br> Vereist een verbinding tussen Azure-cloudservice en on-premises infrastructuur |

## <a name="define-access-control-options"></a>Definieer de opties voor toegangsbeheer
Dankzij het gebruik van verificatie, autorisatie en access control mogelijkheden beschikbaar zijn in Azure AD kunt u uw bedrijf gebruik van een centrale identiteitsopslagplaats en gebruikers tegelijkertijd inschakelen en partners gebruik van eenmalige aanmelding (SSO) zoals weergegeven in de volgende afbeelding:

![](./media/hybrid-id-design-considerations/centralized-management.png)

Gecentraliseerd beheer en volledig integratie met andere directory 's

Azure Active Directory biedt eenmalige aanmelding tot duizenden SaaS-toepassingen en lokale webtoepassingen. Zie de [federatiecompatibiliteitslijst van Azure Active Directory: onafhankelijke id-providers die kunnen worden gebruikt voor het implementeren van eenmalige aanmelding](https://msdn.microsoft.com/library/azure/jj679342.aspx) artikel voor meer informatie over de SSO van derden die door Microsoft zijn getest. Deze mogelijkheid kan organisatie voor het implementeren van verschillende B2B-scenario's terwijl de controle van het beheer van identiteits- en toegangsbeheer. Tijdens de B2B is ontwerpen proces echter belangrijk dat u inzicht in de verificatiemethode die wordt gebruikt door de partner en valideren van deze methode wordt ondersteund door Azure. De volgende methoden worden momenteel ondersteund door Azure AD:

* Security Assertion Markup Language (SAML)
* OAuth
* Kerberos
* Tokens
* Certificaten

> [!NOTE]
> Lees [Azure Active Directory-verificatieprotocollen](https://msdn.microsoft.com/library/azure/dn151124.aspx) meer gedetailleerde informatie over elk protocol en de mogelijkheden ervan in Azure.
>
>

Met de ondersteuning van Azure AD, mobile business-toepassingen dezelfde eenvoudig Mobile Services-verificatie-ervaring zodat werknemers aan te melden met hun mobiele toepassingen met hun zakelijke Active Directory-referenties gebruiken. Azure AD wordt met deze functie wordt ondersteund als een id-provider in Mobile Services naast de andere identiteitsproviders al ondersteund (waaronder Microsoft-Accounts, Facebook-ID, ID van Google en Twitter-ID). Als de apps lokale referentie van de gebruiker zich bevindt op van het bedrijf AD DS, moet de toegang van partners en gebruikers die afkomstig zijn van de cloud niet transparant zijn. U kunt beheren van de gebruiker voorwaardelijke toegang tot webtoepassingen (cloud-gebaseerd), web-API, Microsoft cloud-services, SaaS-toepassingen van derden en systeemeigen (mobiele)-clienttoepassingen en hebben de voordelen van beveiliging, controle en de rapportage-alles in één plaats. Het verdient echter de implementatie in een niet-productieomgeving of met een beperkt aantal gebruikers te valideren.

> [!TIP]
> het is belangrijk te vermelden dat Azure AD geen groepsbeleid heeft zoals AD DS heeft. Als u wilt afdwingen van beleid voor apparaten, moet u een beheersysteem voor mobiele apparaten, zoals [Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx).
>
>

Als de gebruiker is geverifieerd met behulp van Azure AD, is het belangrijk om te bepalen het niveau van toegang dat de gebruiker heeft. Het niveau van toegang dat de gebruiker over een bron heeft kan variëren. Azure AD kunt u een extra beveiligingslaag toevoegen door het beheren van toegang tot bepaalde bronnen, houd er rekening mee dat de bron zelf ook een eigen toegangsbeheerlijst afzonderlijk, zoals het toegangsbeheer voor bestanden op een bestandsserver hebben kan. De volgende afbeelding bevat een overzicht van de niveaus van toegangsbeheer dat u in een hybride scenario opnemen kunt:

![](./media/hybrid-id-design-considerations/accesscontrol.png)

Elke interactie in het diagram weergegeven zoals in afbeelding X vertegenwoordigt een access control-scenario die kan worden volstaan met Azure AD. Hieronder hebt u een beschrijving van elk scenario:

1. Voorwaardelijke toegang tot toepassingen die worden gehost op de lokale: U kunt geregistreerde apparaten gebruiken met toegangsbeleid voor toepassingen die zijn geconfigureerd voor gebruik van AD FS met Windows Server 2012 R2. Zie [Setting up On-premises Conditional Access using Azure Active Directory Device Registration](active-directory-conditional-access-azure-portal.md) (Engelstalig) voor meer informatie over het instellen van on-premises voorwaardelijke toegang.

2. Beheer van toegang tot de Azure-portal: Azure kunt u ook toegang tot de portal beheren met behulp van op rollen gebaseerde toegangsbeheer (RBAC)). Deze methode kunt het bedrijf om het aantal bewerkingen die een persoon in de Azure portal uitvoeren kunt te beperken. Met behulp RBAC voor het beheren van toegang tot de portal, kunnen IT-beheerders toegang delegeren met behulp van de volgende methoden voor access management:

    * Op basis van een groep roltoewijzing: U kunt toegang toewijzen aan Azure AD-groepen die kunnen worden gesynchroniseerd vanuit uw lokale Active Directory. Hiermee kunt u gebruikmaken van bestaande investeringen in uw organisatie heeft doorgevoerd in tooling en processen voor het beheren van groepen. U kunt ook de functie voor het beheer van gedelegeerd group van Azure AD Premium.
    * Ingebouwde rollen in Azure gebruiken: U kunt drie rollen: eigenaar, bijdrager en Reader om ervoor te zorgen dat gebruikers en groepen gemachtigd bent alleen de taken die ze nodig hebben om hun werk te doen.
    * Gedetailleerde toegang tot bronnen: U kunt rollen toewijzen aan gebruikers en groepen voor een bepaald abonnement, resourcegroep of een afzonderlijke Azure resource, zoals een website of de database. Op deze manier kunt u ervoor zorgen dat gebruikers hebben toegang tot alle bronnen die ze nodig hebben en geen toegang tot bronnen die ze niet hoeven te beheren.

> [!NOTE]
> Als u toepassingen bouwt en wilt aanpassen van het toegangsbeheer voor hen, is het ook mogelijk gebruik van Azure AD-toepassing-functies voor autorisatie. Bekijk dit [WebApp-RoleClaims-DotNet voorbeeld](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) over het bouwen van uw app om het gebruik van deze mogelijkheid.
>
>

3. Voorwaardelijke toegang voor Office 365-toepassingen met Microsoft Intune: IT-beheerders kunnen apparaatbeleid voor voorwaardelijke toegang voor het beveiligen van bedrijfsbronnen, terwijl tegelijkertijd waardoor de informatiewerkers op compatibele apparaten toegang krijgen tot de services inrichten. Zie [Conditional Access Device Policies for Office 365 services (Engelstalig)](active-directory-conditional-access-device-policies.md) voor meer informatie.

4. Voorwaardelijke toegang voor Saas-apps: [deze functie](http://blogs.technet.com/b/ad/archive/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work.aspx) kunt u regels voor toegang per toepassing multi-factor authentication-server en de mogelijkheid om te blokkeren van toegang voor gebruikers niet op een vertrouwd netwerk configureren. U kunt de multi-factor authentication-regels toepassen op alle gebruikers die zijn toegewezen aan de toepassing of alleen voor gebruikers binnen de opgegeven beveiligingsgroepen. Gebruikers kunnen worden uitgesloten van de vereiste voor meervoudige verificatie als ze toegang hebben tot de toepassing van een IP-adres dat in binnen de organisatie-netwerk.

Omdat de opties voor toegangsbeheer gebruiken een gelaagde benadering, vergelijking tussen deze opties zijn niet geldig voor deze taak. Zorg ervoor dat u gebruik van alle beschikbare opties voor elk scenario dat vereist dat u toegang tot uw resources.

## <a name="define-incident-response-options"></a>Definieer de opties voor respons op incidenten
Azure AD kan helpen IT identiteit potentiële beveiligingsrisico's in de omgeving door de bewaking van gebruikersactiviteit. IT Azure AD Access kunt en gebruiksrapporten meer inzicht verkrijgen in de integriteit en beveiliging van de directory van uw organisatie. Met deze informatie kunt kan IT-beheerder beter bepalen waar mogelijk beveiligingsrisico's mogelijk liggen zodat ze voldoende plannen kunnen die risico's te beperken.  [Azure AD Premium-abonnement](active-directory-get-started-premium.md) beschikt over een reeks beveiligingsrapporten waardoor IT om deze informatie te verkrijgen. [Azure AD-rapporten](active-directory-view-access-usage-reports.md) als volgt worden ingedeeld:

* **Rapporten voor afwijkingsdetectie**: aanmelden gebeurtenissen die zijn gevonden zijn afwijkende bevatten. Het doel is u rekening houden met deze activiteit en kunt u bepalen of een gebeurtenis verdacht is.
* **Rapport van de toepassing geïntegreerde**: biedt inzicht in hoe cloud-toepassingen in uw organisatie worden gebruikt. Azure Active Directory biedt integratie met duizenden cloud-toepassingen.
* **Foutenrapporten**: fouten die zich voordoen kunnen bij het inrichten van accounts kunnen externe toepassingen aangeven.
* **Rapporten van de gebruiker gedefinieerde**: apparaat/teken weergeven in de activiteitsgegevens voor een specifieke gebruiker.
* **Activiteitenlogboeken**: bevatten een record van alle controlegebeurtenissen binnen de afgelopen 24 uur, afgelopen 7 dagen of afgelopen 30 dagen, evenals wijzigingen in groep activiteiten en activiteit voor wachtwoord opnieuw instellen en registratie.

> [!TIP]
> Een ander rapport waarmee u kunt ook het Incident Response-team werkt aan een aanvraag is de [gebruiker met gelekte referenties](http://blogs.technet.com/b/ad/archive/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials.aspx) rapport. Dit rapport geeft alle resultaten weer tussen de lijst gelekte aanmeldingsreferenties en uw tenant.
>
>

Andere belangrijke ingebouwde rapporten in Azure AD die kan worden gebruikt tijdens een onderzoek respons op incidenten en zijn:

* **Activiteit voor wachtwoordherstel**: admin voorzien van inzicht in hoe vaak opnieuw instellen van wachtwoorden in de organisatie wordt gebruikt.
* **Registratie-activiteit wachtwoord opnieuw instellen**: biedt inzicht in dat gebruikers hun methoden voor het wachtwoord opnieuw instellen, hebt geregistreerd en welke methoden ze hebt geselecteerd.
* **Groep activiteit**: biedt een overzicht van wijzigingen aan de groep (ex: gebruikers toegevoegd of verwijderd) die zijn gestart in het deelvenster toegang.

Naast de core reporting mogelijkheden van Azure AD Premium die u ook tijdens een Incident Response onderzoek, kan IT gebruiken kunt profiteren van het rapport Audit om informatie te verkrijgen, zoals:

* Wijzigingen in het rollidmaatschap (bijvoorbeeld gebruiker toegevoegd aan de rol globale beheerder)
* Referentie-updates (bijvoorbeeld wachtwoordwijzigingen)
* Domeinbeheer (bijvoorbeeld een aangepast domein, verwijderen van een domein controleren)
* Het toevoegen of verwijderen van toepassingen
* Beheer van gebruikers (bijvoorbeeld toevoegen, verwijderen, het bijwerken van een gebruiker)
* Het toevoegen of verwijderen van licenties

Omdat de opties voor respons op incidenten een gelaagde benadering, geldt vergelijking tussen deze opties niet voor deze taak. Zorg ervoor dat u gebruik van alle beschikbare opties voor elk scenario dat vereist dat u Azure AD-rapportagemogelijkheid gebruiken als onderdeel van uw bedrijf respons op incidenten proces.

## <a name="next-steps"></a>Volgende stappen
[Beheertaken voor hybride identiteit bepalen](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](active-directory-hybrid-identity-design-considerations-overview.md)
