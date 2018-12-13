---
title: Hybride identiteit ontwerp - gegevensbeveiligingsstrategie Azure | Microsoft Docs
description: U definieert de strategie voor gegevensbescherming voor uw oplossing voor hybride identiteit om te voldoen aan de bedrijfsvereisten die u hebt gedefinieerd.
documentationcenter: ''
services: active-directory
author: billmath
manager: mtillman
editor: ''
ms.assetid: e76fd1f4-340a-492a-84d9-e05f3b7cc396
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/13/2017
ms.component: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 20782679a4e73c029d6c83fd3bc57914b4d28691
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52865043"
---
# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>Strategie voor gegevensbescherming voor uw oplossing voor hybride identiteit definiëren
In deze taak definieert u de strategie voor gegevensbescherming voor uw oplossing voor hybride identiteit om te voldoen aan de bedrijfsvereisten die u hebt gedefinieerd in:

* [Bepalen van de beveiligingsvereisten voor gegevens](plan-hybrid-identity-design-considerations-dataprotection-requirements.md)
* [Bepaal de vereisten voor inhoudsbeheer](plan-hybrid-identity-design-considerations-contentmgt-requirements.md)
* [Vereisten voor toegangsbeheer bepalen](plan-hybrid-identity-design-considerations-accesscontrol-requirements.md)
* [Bepaal de vereisten voor respons op incidenten](plan-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>Opties voor beveiliging van gegevens definiëren
Zoals uitgelegd in [adreslijstsynchronisatie bepalen](plan-hybrid-identity-design-considerations-directory-sync-requirements.md), Microsoft Azure AD kan worden gesynchroniseerd met uw on-premises Active Directory Domain Services (AD DS). Deze integratie kunt organisaties gebruik Azure AD om te controleren of de referenties van gebruikers wanneer ze willen toegang tot bedrijfsbronnen. U kunt dit doen voor beide scenario's: gegevens op rest on-premises en in de cloud. Toegang tot gegevens in Azure AD is vereist verificatie van de gebruiker via een beveiligingstokenservice (STS).

Eenmaal is geverifieerd, wordt de UPN (User Principal Name) lezen van het verificatietoken. Het autorisatiesysteem bepaalt vervolgens het gerepliceerde partitie en de container die overeenkomt met het domein van de gebruiker. Klik vervolgens op de bestaan, de ingeschakelde status en de rol van de gebruiker informatie helpt het autorisatiesysteem te bepalen of toegang tot de doel-tenant is gemachtigd voor de gebruiker in die sessie. Bepaalde gemachtigde acties (met name maken en het wachtwoord opnieuw instellen) voor het maken van een audittrail die vervolgens door een tenantbeheerder zijn gebruikt voor het beheren van inspanningen of onderzoek.

Gegevens te verplaatsen van uw on-premises datacenter naar Azure Storage via een internetverbinding mogelijk niet altijd haalbaar vanwege gegevensvolume, beschikbare bandbreedte of andere overwegingen. De [Azure Storage Import/Export-Service](../../storage/common/storage-import-export-service.md) biedt een optie op basis van hardware voor het plaatsen van/ophalen van grote hoeveelheden gegevens in blob-opslag. Hiermee kunt u voor het verzenden van [BitLocker-versleuteling](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2) hardeschijfstations rechtstreeks naar een Azure-datacenter waar cloudoperators upload de inhoud naar uw storage-account, of ze kunnen uw Azure-gegevens downloaden naar uw stations om terug te keren naar u. Alleen versleutelde schijven worden geaccepteerd voor dit proces (met behulp van een BitLocker-sleutel die is gegenereerd door de service zelf tijdens de installatie van de taak). De BitLocker-sleutel wordt geleverd aan Azure afzonderlijk manier wordt perimeterbeveiliging geboden buiten-band belangrijkste delen.

Omdat gegevens die onderweg zijn plaatsvinden in verschillende scenario's, is ook belangrijk te weten hoeveel dat gebruikmaakt van Microsoft Azure [virtuele netwerken](https://azure.microsoft.com/documentation/services/virtual-network/) voor het isoleren van verkeer van de tenants van elkaar, die gebruikmaakt van metingen, zoals host - en gastniveau firewalls, IP-pakket te filteren, poort blokkeren en HTTPS-eindpunten. De meeste interne communicatie van Azure, inclusief infrastructuur-infrastructuur- en infrastructuur klant (on-premises), zijn echter ook versleuteld. Een ander belangrijk scenario is de communicatie tussen Azure-datacenters; Microsoft beheert netwerken om ervoor te zorgen dat er geen virtuele machine kan imiteren of op IP-adres van een andere afluisteren. TLS/SSL wordt gebruikt bij het openen van Azure Storage of SQL-Databases, of bij het verbinden met Cloud Services. In dit geval is de beheerder van de klant verantwoordelijk voor het verkrijgen van een TLS/SSL-certificaat en deze implementeert in de infrastructuur van hun tenant. Gegevensverkeer tussen virtuele Machines in dezelfde implementatie of verplaatsen tussen tenants in een enkele implementatie via Microsoft Azure Virtual Network kan worden beveiligd via versleutelde communicatieprotocollen, zoals HTTPS, SSL/TLS of anderen.

Afhankelijk van hoe u de vragen in beantwoord [beveiligingsvereisten voor gegevens bepalen](plan-hybrid-identity-design-considerations-dataprotection-requirements.md), zou het mogelijk om te bepalen hoe u wilt beveiligen van uw gegevens en hoe de oplossing voor hybride identiteit kunt u kan helpen bij dit proces. De volgende tabel ziet u de opties die worden ondersteund door Azure die beschikbaar voor elk scenario voor gegevensbeveiliging zijn.

| Opties voor beveiliging van gegevens | AT-rest in de cloud | Op de rest-on-premises | In-transit |
| --- | --- | --- | --- |
| BitLocker-stationsversleuteling |X |X | |
| SQL Server voor het versleutelen van databases |X |X | |
| VM-VM-versleuteling | | |X |
| SSL/TLS | | |X |
| VPN | | |X |

> [!NOTE]
> Lezen [naleving per functie](https://azure.microsoft.com/support/trust-center/services/) op [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) voor meer informatie over de certificaten die compatibel zijn met verschillende Azure-Services.
> Omdat de opties voor de beveiliging van gegevens een gelaagde benadering, vergelijking tussen deze opties zijn niet van toepassing voor deze taak. Zorg ervoor dat u gebruik van alle opties die beschikbaar zijn voor elke status van de gegevens.
>
>

## <a name="define-content-management-options"></a>Opties voor inhoudbeheer definiëren

Eén voordeel van het gebruik van Azure AD voor het beheren van een infrastructuur voor hybride identiteit is dat het proces volledig transparant vanuit het perspectief van de eindgebruiker van is. De gebruiker probeert te krijgen tot een gedeelde resource, de resource vereist verificatie, de gebruiker heeft een verificatieaanvraag verzonden naar Azure AD om te kunnen ophalen van het token en toegang tot de resource. Dit hele proces wordt uitgevoerd op de achtergrond, zonder tussenkomst van de gebruiker. 

Organisaties die de bezorgdheid over de privacy van gegevens, meestal zijn vereisen gegevensclassificatie voor hun oplossing. Als gegevensclassificatie maakt al gebruik van hun huidige on-premises infrastructuur, is het mogelijk om te gebruiken van Azure AD als de hoofdopslagplaats voor de identiteit van de gebruiker. Een veelgebruikt hulpprogramma dat gebruikt on-premises is voor gegevensclassificatie heet [Data Classification Toolkit](https://msdn.microsoft.com/library/Hh204743.aspx) voor Windows Server 2012 R2. Dit hulpprogramma kunt identificeren, classificeren en beveiligen van gegevens op bestandsservers in uw privécloud. Het is ook mogelijk met gebruik van de [automatische Bestandsclassificatie](https://technet.microsoft.com/library/hh831672.aspx) in Windows Server 2012 deze taak te voltooien.

Als uw organisatie geen classificatie van dashboardgegevens in plaats, maar moet bij de bescherming van gevoelige bestanden zonder het toevoegen van nieuwe Servers on-premises, Microsoft kan worden gebruikt [Azure Rights Management-Service](https://technet.microsoft.com/library/JJ585026.aspx).  Azure RMS gebruikt autorisatiebeleidsregels voor versleuteling, identiteit en voor de beveiliging van uw bestanden en e-mailadres en het werkt op meerdere apparaten, telefoons, tablets en pc's. Omdat Azure RMS een cloudservice is, is er niet nodig om te configureren expliciet vertrouwensrelaties met andere organisaties voordat u beveiligde inhoud met hen kunt delen. Als ze al een Office 365 of Azure AD-adreslijst hebben, wordt samenwerking tussen verschillende organisaties automatisch ondersteund. U kunt ook alleen de mapkenmerken die Azure RMS ondersteuning bieden voor een gemeenschappelijke identiteit voor uw on-premises Active Directory-accounts moet met behulp van Azure Active Directory-Synchronisatieservices (AAD Sync) of Azure AD Connect synchroniseren.

Een essentieel onderdeel van inhoudsbeheer is om te begrijpen wie welke resource, een uitgebreide logboekregistratie is daarom belangrijk voor de oplossing voor identiteitsbeheer. Azure AD biedt log meer dan 30 dagen, met inbegrip van:

* Wijzigingen in het rollidmaatschap (ex: gebruiker is toegevoegd aan de rol van globale beheerder)
* Updates voor referenties (ex: wachtwoord te wijzigen)
* Domeinbeheer van (ex: een aangepast domein, verwijderen van een domein controleren)
* Toevoegen of verwijderen van toepassingen
* Beheer van gebruikers (ex: toevoegen, verwijderen, bijwerken van een gebruiker)
* Toevoegen of verwijderen van licenties

> [!NOTE]
> Lezen [Microsoft Azure-beveiliging en Controlelogboekbeheer](https://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf) voor meer informatie over mogelijkheden voor logboekregistratie in Azure.
> Afhankelijk van hoe u de vragen in beantwoord [Bepaal de vereisten voor inhoudsbeheer](plan-hybrid-identity-design-considerations-contentmgt-requirements.md), zou het mogelijk om te bepalen hoe u wilt dat de inhoud in uw oplossing voor hybride identiteit wordt beheerd. Hoewel alle opties die beschikbaar zijn in tabel 6 kan worden geïntegreerd met Azure AD, is het belangrijk om te bepalen die geschikter zijn voor uw bedrijfsbehoeften.
>
>

| Opties voor inhoudbeheer | Voordelen | Nadelen |
| --- | --- | --- |
| Gecentraliseerde on-premises (Active Directory Rights Management Server) |Volledige controle over de infrastructuur van de server die verantwoordelijk is voor het classificeren van de gegevens <br> Ingebouwde functie in Windows Server, hoeven niet extra licentie of abonnement <br> Kan worden geïntegreerd met Azure AD in een hybride scenario <br> Ondersteunt mogelijkheden voor information rights management (IRM) in Microsoft Online services zoals Exchange Online en SharePoint Online, evenals Office 365 <br> Biedt ondersteuning voor on-premises serverproducten van Microsoft, zoals Exchange Server, SharePoint Server en bestandsservers waarop Windows Server en infrastructuur voor Bestandsclassificatie (FCI) worden uitgevoerd. |Hogere onderhoud (gelijke tred houden met updates, configuratie en mogelijke upgrades), aangezien IT is eigenaar van de Server <br> Vereist een serverinfrastructuur on-premises<br> Mogelijkheden van Azure biedt geen systeemeigen gebruiken |
| Gecentraliseerd in de cloud (Azure RMS) |Gemakkelijker te beheren in vergelijking met de on-premises-oplossing <br> Kan worden geïntegreerd met AD DS in een hybride scenario <br>  Volledig geïntegreerd met Azure AD <br> Er zijn vereist om een on-premises server om de service implementeren <br> Ondersteunt on-premises serverproducten van Microsoft, zoals Exchange Server, SharePoint Server en bestandsservers waarop Windows Server en File Classification, Infrastructure (FCI) worden uitgevoerd <br> IT, volledige controle over hun tenantsleutel met BYOK-mogelijkheid kan hebben. |Uw organisatie moet beschikken over een cloudabonnement voor RMS ondersteunt <br> Uw organisatie moet beschikken over een Azure AD-directory voor de ondersteuning van gebruikersverificatie voor RMS |
| Hybride (Azure RMS is geïntegreerd met, On-Premises Active Directory Rights Management Server) |In dit scenario worden de voordelen van beide, gecentraliseerde on-premises en in de cloud bij elkaar opgeteld. |Uw organisatie moet beschikken over een cloudabonnement voor RMS ondersteunt <br> Uw organisatie moet beschikken over een Azure AD-directory voor de ondersteuning van gebruikersverificatie voor RMS <br> Vereist een verbinding tussen Azure-cloudservice en on-premises infrastructuur |

## <a name="define-access-control-options"></a>Opties voor toegangsbeheer definiëren
Door gebruik te maken van de verificatie, autorisatie en mogelijkheden voor toegangsbeheer toegang beschikbaar in Azure AD kunt u uw bedrijf gebruik van een centrale identiteitsopslagplaats terwijl u gebruikers en partners gebruik van eenmalige aanmelding (SSO) zoals weergegeven in de volgende afbeelding:

![](./media/plan-hybrid-identity-design-considerations/centralized-management.png)

Gecentraliseerd beheer en volledig integratie met andere directory 's

Azure Active Directory biedt single sign-on bij duizenden SaaS-toepassingen en on-premises webtoepassingen. Zie de [federatiecompatibiliteitslijst van Azure Active Directory: externe id-providers die kunnen worden gebruikt voor het implementeren van eenmalige aanmelding](how-to-connect-fed-compatibility.md) artikel voor meer informatie over de eenmalige aanmelding van derden die zijn getest door Microsoft. Deze mogelijkheid kan een organisatie voor het implementeren van tal van B2B-scenario's terwijl de controle over het beheer van identiteits- en toegangsbeheer. Tijdens de B2B is het ontwerpen van proces echter belangrijk te begrijpen van de verificatiemethode die wordt gebruikt door de partner en valideren of deze methode wordt ondersteund door Azure. De volgende methoden worden momenteel ondersteund door Azure AD:

* Security Assertion Markup Language (SAML)
* OAuth
* Kerberos
* Tokens
* Certificaten

> [!NOTE]
> Lees [Azure Active Directory-verificatieprotocollen](https://msdn.microsoft.com/library/azure/dn151124.aspx) graag meer informatie over elk protocol en de mogelijkheden in Azure.
>
>

Met behulp van de Azure AD-ondersteuning, mobiele, zakelijke toepassingen dezelfde eenvoudig Mobile Services-verificatie-ervaring waarmee werknemers zich aanmelden bij hun mobiele toepassingen met hun bedrijfsreferenties van Active Directory gebruiken. Azure AD wordt met deze functie wordt ondersteund als een id-provider in Mobile Services naast de andere id-providers al ondersteund (waaronder Microsoft-Accounts, Facebook-ID, Google-ID en Twitter-ID). Als de on-premises toepassingen de gebruikersreferenties te vinden op van het bedrijf AD DS gebruiken, moet de toegang van partners en gebruikers die afkomstig zijn van de cloud transparant zijn. U kunt beheren van de gebruiker voorwaardelijk toegangsbeheer webtoepassingen (cloud-gebaseerd), web-API, Microsoft cloud-services, SaaS-toepassingen van derden en clienttoepassingen met systeemeigen (mobiele) en zijn de voordelen van beveiliging, controle en de rapportage-alles in één plaats. Het wordt echter aanbevolen om te valideren van de implementatie in een niet-productieomgeving of met een beperkt aantal gebruikers.

> [!TIP]
> het is belangrijk om te vermelden dat Azure AD geen groepsbeleid als AD DS. Als u wilt afdwingen van beleid voor apparaten, moet u een beheeroplossing voor mobiele apparaten zoals [Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx).
>
>

Zodra de gebruiker is geverifieerd met behulp van Azure AD, is het belangrijk om te bepalen het niveau van toegang dat de gebruiker heeft. Het niveau van toegang dat de gebruiker over een bron heeft kan variëren. Azure AD kunt u een extra beveiligingslaag toevoegen door het beheren van toegang tot bepaalde bronnen, houd er rekening mee dat de resource zelf ook een eigen toegangsbeheerlijst afzonderlijk, zoals het toegangsbeheer voor bestanden in een bestandsserver hebben kan. De volgende afbeelding bevat een overzicht van het niveau van toegangsbeheer dat u kunt in een hybride scenario:

![](./media/plan-hybrid-identity-design-considerations/accesscontrol.png)

Elke interactie in het diagram weergegeven zoals in afbeelding X vertegenwoordigt een scenario van de access control die kan worden volstaan met Azure AD. Hieronder bevat een beschrijving van elk scenario:

  1. Voorwaardelijke toegang voor toepassingen die on-premises gehost: U kunt geregistreerde apparaten gebruiken met beleid voor toegang voor toepassingen die zijn geconfigureerd voor gebruik van AD FS met Windows Server 2012 R2.

  2. Beheer van toegang tot de Azure-portal: Azure kunt u ook toegang tot de portal beheren met behulp van op rollen gebaseerd toegangsbeheer (RBAC)). Deze methode kan het bedrijf om te beperken van het aantal bewerkingen die een persoon in de Azure-portal kunt uitvoeren. Met behulp van RBAC voor het beheren van toegang tot de portal, kunnen IT-beheerders toegang delegeren met behulp van de volgende methoden voor access management:

   - Groep op basis van roltoewijzing: U kunt toegang toewijzen aan Azure AD-groepen die kunnen worden gesynchroniseerd vanuit uw lokale Active Directory. Hiermee kunt u de bestaande investeringen die uw organisatie heeft aangebracht in de hulpprogramma's en processen voor het beheren van groepen. U kunt ook de functie voor het beheer van groepen met gemachtigden van Azure AD Premium gebruiken.
   - Gebruik van ingebouwde rollen in Azure: kunt u drie rollen: eigenaar, Inzender en lezer, om ervoor te zorgen dat gebruikers en groepen gemachtigd alleen de taken die ze nodig hebben om hun werk te doen.
   -  Gedetailleerde toegang tot resources: U kunt rollen toewijzen aan gebruikers en groepen voor een bepaald abonnement, resourcegroep of een afzonderlijke Azure-resource, zoals een website of de database. Op deze manier kunt u ervoor zorgen dat gebruikers hebben toegang tot alle bronnen die ze nodig hebben en geen toegang tot resources die ze niet hoeven te beheren.

   > [!NOTE]
   > Als u toepassingen bouwen en wilt aanpassen van het toegangsbeheer voor hen, is het ook mogelijk met Azure AD-toepassingsrollen voor autorisatie. Controleer deze [WebApp-RoleClaims-DotNet-voorbeeld](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) van het bouwen van uw app voor het gebruik van deze mogelijkheid.


  3. Voorwaardelijke toegang voor Office 365-toepassingen met Microsoft Intune: IT-beheerders kunnen apparaatbeleid voor voorwaardelijke toegang voor het beveiligen van bedrijfsbronnen, terwijl op hetzelfde moment ze IT-medewerkers op compatibele apparaten toegang krijgen tot de services inrichten. 
  
  4. Voorwaardelijke toegang voor Saas-apps: [deze functie](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work/) kunt u regels voor toegang per toepassing meervoudige verificatie en de mogelijkheid om te blokkeren van toegang voor gebruikers niet op een vertrouwd netwerk configureren. U kunt de multi-factor authentication-regels toepassen op alle gebruikers die zijn toegewezen aan de toepassing of alleen voor gebruikers binnen de opgegeven beveiligingsgroepen. Gebruikers kunnen worden uitgesloten van de vereiste voor meervoudige verificatie als ze de toepassing openen vanaf een IP-adres dat in binnen de organisatie het netwerk.

Omdat de opties voor toegangsbeheer gebruiken een gelaagde benadering, vergelijking tussen deze opties zijn niet van toepassing voor deze taak. Zorg ervoor dat u gebruik van alle opties die beschikbaar zijn voor elk scenario dat vereist dat u voor het beheren van toegang tot uw resources.

## <a name="define-incident-response-options"></a>Opties voor respons op incidenten definiëren
Azure AD kan helpen IT aan identiteit potentiële beveiligingsrisico's in de omgeving door de bewaking van de activiteit van de gebruiker. IT kunt Azure AD-toegang en gebruiksrapporten meer inzicht verkrijgen in de integriteit en beveiliging van de adreslijst van uw organisatie. Met deze informatie bepalen IT-beheerder beter waar mogelijke beveiligingsrisico's schuilen zodat ze voldoende plannen maken kunnen die risico's te beperken.  [Azure AD Premium-abonnement](../fundamentals/active-directory-get-started-premium.md) bevat een set beveiligingsrapporten die mogelijk IT om deze informatie te verkrijgen. [Azure AD-rapporten](../reports-monitoring/overview-reports.md) als volgt zijn onderverdeeld:

* **Anomaliedetectie rapporten**: aanmeldingsgebeurtenissen die gevonden worden afwijkende bevatten. Het doel is om u op de hoogte van deze activiteit maken en kunt u bepalen of een gebeurtenis verdacht is.
* **Rapport van de toepassing geïntegreerde**: biedt inzicht in hoe cloud-toepassingen in uw organisatie worden gebruikt. Azure Active Directory biedt integratie met duizenden cloudtoepassingen.
* **Foutenrapporten**: duiden op fouten die optreden bij het inrichten van accounts voor externe toepassingen.
* **Rapporten voor specifieke gebruikers**: apparaat/teken weergeven in de activiteitsgegevens voor een specifieke gebruiker.
* **Activiteitenlogboeken**: bevatten een record van alle gecontroleerde gebeurtenissen binnen de afgelopen 24 uur, afgelopen 7 dagen of afgelopen 30 dagen, evenals groep is gewijzigd en het wachtwoord opnieuw instellen en registratie-activiteit.

> [!TIP]
> Een ander rapport waarmee u kunt ook het Incident Response-team werkt aan een aanvraag is de [gebruiker met de referenties zijn gelekt](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials/) rapport. Dit rapport toont alle overeenkomsten tussen de lijst met de referenties zijn gelekt en uw tenant.
>


Andere belangrijke ingebouwde rapporten in Azure AD die kan worden gebruikt tijdens een onderzoek incidenten en zijn:

* **Activiteit voor wachtwoord opnieuw instellen**: de beheerder voorzien van inzicht in hoe vaak voor wachtwoord opnieuw instellen in de organisatie wordt gebruikt.
* **Activiteit van de registratie voor wachtwoordherstel**: biedt inzicht in welke gebruikers hun methoden voor wachtwoord opnieuw instellen, hebt geregistreerd en welke methoden die ze hebben geselecteerd.
* **Groep activiteit**: biedt een overzicht van wijzigingen aan de groep (bijvoorbeeld: gebruikers toegevoegd of verwijderd) die zijn gestart in het toegangsvenster.

Naast de core rapportagefunctionaliteit van Azure AD Premium die u ook tijdens een Incident Response onderzoeksproces, kan IT gebruiken kunt profiteren van het controlerapport om informatie te verkrijgen, zoals:

* Wijzigingen in het rollidmaatschap (bijvoorbeeld gebruiker toegevoegd aan de rol globale beheerder)
* Referentie-updates (bijvoorbeeld wachtwoord te wijzigen)
* Domeinbeheer (bijvoorbeeld een aangepast domein, verwijderen van een domein controleren)
* Toevoegen of verwijderen van toepassingen
* Beheer van gebruikers (bijvoorbeeld toevoegen, verwijderen, bijwerken van een gebruiker)
* Toevoegen of verwijderen van licenties

Omdat de opties voor respons op incidenten een gelaagde benadering, is vergelijking tussen deze opties niet van toepassing voor deze taak. Zorg ervoor dat u gebruik van alle opties die beschikbaar zijn voor elk scenario dat vereist dat u de rapportagefunctionaliteit van Azure AD gebruiken als onderdeel van het proces voor reacties op incidenten van uw bedrijf.

## <a name="next-steps"></a>Volgende stappen
[Beheertaken voor hybride identiteit bepalen](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)

## <a name="see-also"></a>Zie ook
[Overzicht ontwerpoverwegingen](plan-hybrid-identity-design-considerations-overview.md)
