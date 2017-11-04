---
title: Technische mogelijkheden van Azure-beveiliging | Microsoft Docs
description: Meer informatie over cloud-gebaseerde computers onder meer services als een groot aantal compute-exemplaren en services die kunnen worden geschaald omhoog en omlaag automatisch om te voldoen aan de behoeften van uw toepassing of enterprise.
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2017
ms.author: TomSh
ms.openlocfilehash: 7288466cf31e180a16db18f8ddfe02ace3588a8d
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="azure-security-technical-capabilities"></a>Technische mogelijkheden van Azure-beveiliging

Bij de huidige en toekomstige Azure klanten begrijpen en gebruiken van de verschillende betrekking hebben op beveiliging mogelijkheden beschikbaar in en rond de Azure-Platform, Microsoft heeft ontwikkeld die een reeks whitepapers, overzichten van de beveiliging, Best Practices en Controlelijsten. De onderwerpen in termen van breedte en diepte liggen en regelmatig worden bijgewerkt. Dit document is onderdeel van de reeks zoals samengevat in de onderstaande sectie Abstract. Meer informatie over deze reeks Azure-beveiliging kan worden gevonden op (URL).

## <a name="azure-platform"></a>Azure-platform

[Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) een cloudplatform bestaat uit de infrastructuur en toepassingsservices met geïntegreerde gegevensservices en geavanceerde analyses en hulpprogramma's voor ontwikkelaars en -services, gehost in Microsoft van openbare cloud data centers. Klanten kunnen Azure gebruiken voor veel verschillende capaciteiten en scenario's, uit de basic compute, netwerk en opslag, mobiele en web-app-services, voor het volledige cloud-scenario's zoals Internet der dingen worden gebruikt met open-source technologieën en geïmplementeerd als hybride cloud of gehost binnen het datacenter van de klant. Azure biedt cloud technologie als bouwstenen kunnen bedrijven opslaan kosten, snel innoveren en systemen proactief beheren. Wanneer u bouwen op of IT-middelen naar een cloudprovider migreren, worden de mogelijkheden van die organisatie aan uw toepassingen en gegevens beschermen met de services en de besturingselementen die ze bieden voor het beheren van de beveiliging van uw assets cloud-gebaseerde afhankelijk.

Microsoft Azure is de enige cloud computing-provider die biedt een veilige, consistente toepassingsplatform en infrastructuur-as-a-service voor teams werken binnen hun andere cloud vaardigheden en niveaus van project complexiteit, met geïntegreerde dataservices en analyses die onthullen intelligence van gegevens naar waar deze zich voor zowel Microsoft als niet-Microsoft-platforms, open frameworks en hulpprogramma's, biedt opties voor het integreren van cloud met on-premises evenals Azure-cloud-services binnen implementeren een on-premises datacenters. Als onderdeel van de Microsoft Cloud vertrouwd, zijn klanten afhankelijk van Azure voor toonaangevende beveiliging, betrouwbaarheid, naleving, privacy en het netwerk vast van mensen, partners en processen voor de ondersteuning van organisaties in de cloud.

U kunt met Microsoft Azure:

- Versnellen innovatie aan de cloud.

- Power zakelijke beslissingen te nemen en apps met insights.

- Vrij te maken en implementeren van elke locatie.

- Beveiligen van hun bedrijf.

## <a name="scope"></a>Bereik

Het centrale punt van dit technisch document betreft beveiligingsfuncties en -functionaliteit ondersteunen de basisonderdelen van Microsoft Azure, namelijk [Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-introduction), [Microsoft Azure SQL-Databases](https://docs.microsoft.com/azure/sql-database/), [ Microsoft Azure virtuele machine model](https://docs.microsoft.com/azure/virtual-machines/    ), en de hulpprogramma's en de infrastructuur die alles beheren. Dit document de focus op Microsoft Azure technische mogelijkheden beschikbaar zijn voor u als klanten te voldoen aan de rol bij het beschermen van de beveiliging en privacy van gegevens.

Het belang van inzicht in dit model gedeelde verantwoordelijkheid is essentieel voor klanten die zijn verplaatst naar de cloud. Cloudproviders bieden aanzienlijke voordelen voor beveiliging en naleving inspanningen, maar deze voordelen niet absolve doen voor de klant hun gebruikers, toepassingen en serviceaanbiedingen beveiligen.

De klant is verantwoordelijk of een gedeelde verantwoordelijk is voor het beveiligen en beheren van het besturingssysteem, netwerkconfiguratie, toepassingen, identiteit, clients en gegevens voor IaaS-oplossingen.  PaaS-oplossingen bouwen op IaaS-implementaties, de klant is nog steeds zelf verantwoordelijk of een gedeelde verantwoordelijkheid voor het beveiligen en beheren van toepassingen, identiteit, clients en gegevens. Voor SaaS-oplossingen, Nonetheless, blijft de klant verantwoordelijk zijn. Ze moeten ervoor zorgen dat gegevens correct ingedeeld en ze een verantwoordelijkheid delen voor het beheren van hun gebruikers- en eindpunt-apparaten.

Dit document biedt geen gedetailleerde dekking van elk van de bijbehorende onderdelen van Microsoft Azure-platform zoals Azure websites, Azure Active Directory, HDInsight, Media Services en andere services die op de belangrijkste onderdelen zijn gelaagd. Hoewel een minimaal niveau van algemene informatie is opgegeven, zijn lezers verondersteld bekend bent met Azure basisconcepten, zoals beschreven in andere verwijzingen naar door Microsoft geleverd en opgenomen in de koppelingen in dit document.


## <a name="available-security-technical-capabilities-to-fulfil-user-customer-responsibility---big-picture"></a>Beschikbare beveiligingsupdates technische mogelijkheden om te voldoen aan de verantwoordelijkheid van de gebruiker (klant) - grote afbeelding

Microsoft Azure biedt services waarmee klanten kunnen de beveiliging, privacy en naleving behoeften. De volgende afbeelding kunt u verschillende Azure-services beschikbaar zijn voor gebruikers voor het bouwen van een beveiligd en compatibel infrastructuur op basis van industrienormen uitgelegd.

![Beschikbare beveiligingsupdates technische mogelijkheden Big-afbeelding](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig1.png)

## <a name="manage-and-control-identity-and-user-access-protect"></a>Beheren en beheren van identiteiten en -toegang (beveiligen)

Azure helpt u zakelijke en persoonlijke gegevens beveiligen doordat u uw gebruikers-id's en referenties beheren en toegang beheren.

### <a name="azure-active-directory"></a>Azure active directory

Microsoft identiteits- en toegangsbeheer management oplossingen zorgen ervoor dat IT beveiligen toegang tot toepassingen en bronnen in het zakelijke datacenter en in de cloud, extra niveaus van validatie van de multi-factor authentication en voorwaardelijke toegang inschakelen beleid. Bewaking verdachte activiteiten via geavanceerde beveiliging rapportage, controle en waarschuwingen helpt bij het verminderen mogelijke beveiligingsproblemen. [Azure Active Directory Premium](https://docs.microsoft.com/azure/active-directory/active-directory-editions) biedt eenmalige aanmelding tot duizenden cloud-apps (SaaS) en toegang tot web-apps die u on-premises uitgevoerd.

Beveiligingsvoordelen van Azure Active Directory (AD) omvatten de mogelijkheid om te:

- Maken en beheren van één identiteit voor elke gebruiker in uw onderneming hybride, gebruikers, groepen en apparaten synchroon te houden.

- Eenmalige aanmelding toegang bieden tot uw toepassingen met inbegrip van duizenden vooraf geïntegreerde SaaS-apps.

- Toepassingsbeveiliging toegang inschakelen door het afdwingen van multi-factor Authentication op basis van regels voor zowel on-premises en cloudtoepassingen.

- Veilige externe toegang tot on-premises webtoepassingen via Azure AD-toepassingsproxy inrichten.

[Azure active directory-portal](http://aad.portal.azure.com/) vindt u een deel van de azure-portal. Uit dit dashboard kunt u een overzicht van de status van uw organisatie en eenvoudig Duik in de directory, gebruikers of toegang tot toepassingen beheren.

![Azure active directory](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig2.png)

Volgende zijn belangrijkste Azure Identity management mogelijkheden:

- Eenmalige aanmelding

- Multi-Factor Authentication

- Beveiligingsbewaking, waarschuwingen en machine learning gebaseerde rapporten

- Identiteits- en toegangsbeheer van consumenten

- Apparaatregistratie

- Beschermde identiteitsbeheer

- Identiteitsbeveiliging

#### <a name="single-sign-on"></a>Eenmalige aanmelding

[Eenmalige aanmelding (SSO)](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) betekent wordt toegang tot alle toepassingen en bronnen die u nodig hebt om zaken te doen, wanneer u zich aanmeldt bij het gebruik van slechts één keer één gebruikersaccount. Wanneer u bent aangemeld, kunt u alle de toepassingen die u nodig zonder hebt vereist om te verifiëren openen (bijvoorbeeld: Typ een wachtwoord) een tweede keer.

Veel organisaties zijn afhankelijk van de software als een dienst (SaaS)-toepassingen zoals Office 365, het selectievakje en Salesforce voor de productiviteit van eindgebruikers. In het verleden hebben IT-personeel nodig afzonderlijk maken en bijwerken van gebruikersaccounts in elke SaaS-toepassing en moesten gebruikers een wachtwoord voor elke SaaS-toepassing.

[Azure AD loopt door op de lokale Active Directory in de cloud](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis), zodat gebruikers hun primaire organisatie-account niet alleen te gebruiken zich aanmelden bij hun apparaten domein en de resources van het bedrijf, maar ook alle de web- en SaaS-toepassingen die nodig zijn voor de taak.

Niet alleen gebruikers hoeven niet te beheren van meerdere sets met gebruikersnamen en wachtwoorden, toegang tot de toepassing kan worden automatisch ingerichte of ongedaan ingerichte op basis van organisatie-groepen en hun status als een werknemer. [Azure AD de beveiliging en toegang governance besturingselementen introduceert](https://docs.microsoft.com/azure/active-directory/active-directory-sso-integrate-saas-apps) waarmee u kunt het centraal beheren van toegang van gebruikers over SaaS-toepassingen.

#### <a name="multi-factor-authentication"></a>Multi-Factor Authentication

[Azure multi-factor authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) is een methode voor verificatie dat vereist het gebruik van meer dan één verificatiemethode en een kritieke tweede beveiligingslaag wordt toegevoegd aan de gebruikersaanmeldingen en transacties. [MFA helpt safeguard](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-how-it-works) toegang tot gegevens en toepassingen en te voldoen aan de behoeften van de gebruiker voor een eenvoudig proces aanmelden. Levert sterke verificatie via een aantal opties voor verificatie: telefoonoproep, tekstbericht of mobiele app melding of verificatie van code en derden OAuth-tokens.

#### <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Beveiligingsbewaking, waarschuwingen en machine learning gebaseerde rapporten

Beveiligingsbewaking en waarschuwingen en machine learning gebaseerde rapporten die inconsistente toegangspatronen aangeven kunt u uw bedrijf te beveiligen. U kunt toegang tot Azure Active Directory- en gebruiksrapporten meer inzicht verkrijgen in de integriteit en beveiliging van de directory van uw organisatie. Met deze informatie kunt kan directory-beheerder beter bepalen waar mogelijk beveiligingsrisico's mogelijk liggen zodat ze voldoende plannen kunnen die risico's te beperken.

In de Azure portal of via [Azure Active directory-portal](http://aad.portal.azure.com/), [rapporten](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) zijn onderverdeeld in de volgende manieren:

- Rapporten voor afwijkingsdetectie – bevatten aanmelden gebeurtenissen die we afwijkende worden gevonden. Ons doel is u rekening houden met deze activiteit en kunt u om te bepalen of een gebeurtenis verdacht is.

- Geïntegreerde toepassing rapporten – bieden inzicht in hoe cloud-toepassingen in uw organisatie worden gebruikt. Azure Active Directory biedt integratie met duizenden cloud-toepassingen.

- Foutenrapporten – duiden op fouten die zich kunnen voordoen bij het inrichten van accounts kunnen externe toepassingen.

- Apparaat/teken weergeven gebruikersspecifieke rapporten – in de activiteitsgegevens voor een specifieke gebruiker.

- Een record van alle controlegebeurtenissen bevatten activiteitenlogboeken – in de afgelopen 24 uur, afgelopen 7 dagen of afgelopen 30 dagen en wijzigingen in groep activiteiten en activiteit voor wachtwoord opnieuw instellen en registratie.

#### <a name="consumer-identity-and-access-management"></a>Identiteits- en toegangsbeheer van consumenten

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) is een maximaal beschikbare, wereldwijde, identity management-service voor consumententoepassingen voor honderden miljoenen identiteiten. De service kan worden geïntegreerd in zowel mobiele als webplatforms. Uw klanten kunnen zich bij al uw toepassingen aanmelden via verschillende aanpasbare methoden met hun bestaande sociale accounts of door nieuwe aanmeldingsgegevens te maken.

In de afgelopen, toepassingsontwikkelaars die wilden [aanmelden en meld u aan consumenten](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview) in hun toepassingen schreven hun eigen code. Bovendien gebruikten ze on-premises databases of systemen om gebruikersnamen en wachtwoorden op te slaan. Azure Active Directory B2C biedt uw organisatie een betere manier identiteitsbeheer van consumenten integreren in toepassingen met behulp van een veilige, op standaarden gebaseerd platform en een grote set uitbreidbare beleidsregels.

Wanneer u Azure Active Directory B2C gebruikt, uw consumenten zich registreren voor uw toepassingen met behulp van hun bestaande sociale accounts (Facebook, Google, Amazon, LinkedIn) of door het maken van nieuwe referenties (e-mailadres en wachtwoord of gebruikersnaam en wachtwoord).

Apparaatregistratie

[Azure AD-apparaatregistratie](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-device-registration-overview) vormt de basis voor apparaatgebaseerde [voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-device-registration-overview) scenario's. Wanneer een apparaat is geregistreerd, biedt Azure Active Directory-apparaatregistratie het apparaat met een identiteit die wordt gebruikt voor verificatie van het apparaat wanneer de gebruiker zich aanmeldt. Het geverifieerde apparaat en de kenmerken van het apparaat kunnen vervolgens worden gebruikt voor het afdwingen van voorwaardelijk toegangsbeleid voor toepassingen die in de cloud en on-premises worden gehost.

In combinatie met een [beheer van mobiele apparaten (MDM)](https://www.microsoft.com/itshowcase/Article/Content/588/Mobile-device-management-at-Microsoft) oplossing zoals Intune, de apparaatkenmerken in Azure Active Directory worden bijgewerkt met aanvullende informatie over het apparaat. Hiermee kunt u extra regels voor voorwaardelijke toegang maken die toegang afdwingen van apparaten, zodat ze voldoen aan uw standaarden voor beveiliging en compliance

#### <a name="privileged-identity-management"></a>Beschermde identiteitsbeheer

[Azure Active Directory (AD) Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) kunt u beheren, beheren, en controleren van bevoegde identiteiten en toegang tot bronnen in Azure AD en ook voor andere Microsoft online services, zoals Office 365 of Microsoft Intune.

Gebruikers moeten soms bij het uitvoeren van bevoorrechte bewerkingen in de resources in Azure of Office 365 of andere SaaS-apps. Dit betekent vaak dat organisaties hebben om hem permanente bevoegde toegang in Azure AD. Dit is een groeiende beveiligingsrisico voor cloud-gebaseerde bronnen omdat organisaties voldoende kunnen niet controleren wat gebruikers doen met hun beheerdersbevoegdheden. Als een gebruikersaccount met uitgebreide toegang is aangetast, kan die een inbreuk bovendien invloed op de algehele beveiliging van de cloud. Azure AD Privileged Identity Management helpt bij het oplossen van dit risico.

Azure AD Privileged Identity Management kunt u:

- Zien welke gebruikers zijn Azure AD-beheerders

- On-demand 'just in time' beheerderstoegang toewijzen voor Microsoft Online Services, zoals Office 365 en Intune inschakelen

- Rapporten over beheerder toegang tot geschiedenis en wijzigingen in beheerderstoewijzingen ophalen

- Ontvang waarschuwingen over de toegang tot een bevoorrechte rol

#### <a name="identity-protection"></a>Identiteitsbeveiliging

[Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) is een beveiligingsservice biedt een geconsolideerde weergave risicogebeurtenissen en mogelijke beveiligingsproblemen die invloed hebben op de identiteiten van uw organisatie. Identity Protection gebruikt bestaande Azure Active Directory van afwijkingsdetectie-detectiemogelijkheden (beschikbaar via Azure AD-rapporten voor afwijkende activiteiten) en introduceert nieuwe risico gebeurtenistypen die afwijkingen in realtime kunnen detecteren.

## <a name="secured-resource-access-in-azure"></a>Toegang tot beveiligde bronnen in Azure

Toegangsbeheer in Azure wordt gestart vanuit het oogpunt van facturering van. De eigenaar van een Azure-account toegankelijk in via de [Azure-Accountcentrum](https://account.windowsazure.com/subscriptions), is het Account Administrator (AA). Abonnementen zijn een container voor facturering, maar ze ook fungeren als een beveiligingsgrens: elk abonnement heeft een Service Administrator (SA) die u kunt toevoegen, verwijderen en wijzigen van de Azure-resources in het desbetreffende abonnement met behulp van de Azure-portal. De SA standaard van een nieuw abonnement is de AA, maar de AA de SA in het midden van Azure-Accounts kunt wijzigen.

![Toegang tot beveiligde bronnen in Azure](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig3.png)

Abonnementen hebben ook een koppeling naar een map. De map definieert een set van gebruikers. Deze kunnen gebruikers van werk of school dat de map gemaakt, of ze kunnen externe gebruikers (dat wil zeggen, een Microsoft-Accounts). Abonnementen zijn toegankelijk voor een subset van de directorygebruikers die zijn toegewezen als Service-beheerder (SA) of Medebeheerder (CA); de enige uitzondering hierop is dat, omwille van de oudere Microsoft-Accounts (voorheen Windows Live ID) kan worden toegewezen als SA of CA zonder aanwezig in de map.

Beveiliging gerichte bedrijven moeten zich richten op uw werknemers de exacte machtigingen die ze nodig hebben. Te veel machtigingen kunnen een account dat kwaadwillende personen worden blootgesteld. Te weinig machtigingen betekenen dat werknemers hun werk efficiënt kunnen niet ophalen. [Azure op rollen gebaseerde toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is) helpt dit probleem oplossen door het aanbieden van Geavanceerd toegangsbeheer voor Azure.

![Toegang tot beveiligde bronnen ](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig4.png)

Met RBAC kunt u taken scheiden binnen uw team en de hoeveelheid toegang verlenen aan gebruikers die ze nodig hebben voor het uitvoeren van hun taken. In plaats van iedereen geven onbeperkte machtigingen in uw Azure-abonnement of de bronnen, kunt u alleen bepaalde acties. Bijvoorbeeld, gebruikmaken van RBAC te laten een werknemer virtuele machines in een abonnement beheren terwijl een andere SQL-databases binnen hetzelfde abonnement kunt beheren.

![Toegang tot beveiligde bronnen in Azure(RBAC)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig5.png)

## <a name="azure-data-security-and-encryption-protect"></a>Azure gegevensbeveiliging en -versleuteling (beveiligen)

Een van de sleutels voor bescherming van gegevens in de cloud is accounting voor de mogelijke statussen in die uw gegevens zich kunnen voordoen, en welke besturingselementen beschikbaar zijn voor die status. Voor Azure data encryption best practices voor beveiliging en aanbevelingen worden rond de statussen van de volgende gegevens.

- In rust: Dit omvat alle informatie opslagobjecten, containers en typen die statisch bestaan op de fysieke media, worden deze magnetische of optische schijf.

- In Transit: Wanneer gegevens worden overgebracht tussen onderdelen, locaties of programma's, zoals via het netwerk via een servicebus (van on-premises naar cloud en vice versa, inclusief hybride verbindingen zoals ExpressRoute) of tijdens een i/o, dit wordt beschouwd als worden in beweging.

### <a name="encryption--rest"></a>Versleuteling @ rest

Als u versleuteling in de Rest van de volgende opties:

Ondersteuning voor ten minste één van de aanbevolen versleuteling modellen in de volgende tabel wordt beschreven om gegevens te versleutelen.

| Versleuteling modellen |  |  |  |
| ----------------  | ----------------- | ----------------- | --------------- |
| Server-versleuteling | Server-versleuteling | Server-versleuteling | Client-versleuteling
| Serverzijde codering met sleutels voor Service beheerd | Serverzijde versleuteling met Customer-Managed sleutels in Azure Sleutelkluis | Server-side '-codering met on-premises beheer van de klant sleutels |
| • Azure Resourceproviders de versleuteling en ontsleuteling bewerkingen uitvoeren <br> • Microsoft beheert de sleutels <br>• Volledige cloud-functionaliteit | • Azure Resourceproviders de versleuteling en ontsleuteling bewerkingen uitvoeren<br>• Klant bepaalt sleutels via Azure Sleutelkluis<br>• Volledige cloud-functionaliteit | • Azure Resourceproviders de versleuteling en ontsleuteling bewerkingen uitvoeren <br>• Klant bepaalt sleutels On-premises <br> • Volledige cloud-functionaliteit| • Azure-services te ontsleutelen gegevens niet zien <br>• Klanten houden van sleutels lokale (of in andere secure winkels). Sleutels zijn niet beschikbaar voor Azure-services <br>• Verminderd cloud-functionaliteit|

### <a name="enabling-encryption-at-rest"></a>Codering in rust inschakelen

**Uw gegevens opslaat voor alle locaties te identificeren**

Het doel van versleuteling in rust is om alle gegevens te versleutelen. Hierdoor wordt voorkomen dat de mogelijkheid van belangrijke gegevens of alle persistente locaties ontbreekt. Opsomming van alle gegevens die zijn opgeslagen door uw toepassing. 

> [!Note] 
> Niet alleen 'toepassingsgegevens' of ' PII', maar geen gegevens met betrekking tot de toepassing zoals account metagegevens (abonnement toewijzingen van contract info, PII).

Overweeg welke winkels die u gebruikt voor het opslaan van gegevens. Bijvoorbeeld:

- Externe opslag (bijvoorbeeld SQL Azure, Document-DB, HDInsights, Data Lake, enz.)

- Tijdelijke opslag (een lokale cache met gegevens van de tenant)

- Cache in het geheugen (in kan worden geplaatst het wisselbestand.)

### <a name="leverage-the-existing-encryption-at-rest-support-in-azure"></a>Gebruik de bestaande versleuteling op rest-ondersteuning in Azure

Voor elke store die u gebruikt, gebruikmaken van de bestaande versleuteling op Rest-ondersteuning.

- Azure Storage: Zie [Azure Storage-Service: versleuteling voor gegevens in rust](https://docs.microsoft.com/azure/storage/storage-service-encryption),

- SQL Azure: Zie [transparante gegevensversleuteling (TDE), SQL altijd versleuteld.](https://msdn.microsoft.com/library/mt163865.aspx)

- Virtuele machine en de lokale schijf opslag ([Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption))

Gebruik voor de virtuele machine en lokale schijfopslag Azure Disk Encryption indien ondersteund:

IaaS

Services met IaaS VM's (Windows of Linux) moeten gebruiken [Azure Disk Encryption](https://microsoft.sharepoint.com/teams/AzureSecurityCompliance/Security/SitePages/Azure%20Disk%20Encryption.aspx) voor het versleutelen van volumes met gegevens van de klant.

PaaS-v2

Services die worden uitgevoerd op PaaS-v2 met Service Fabric via Azure disk encryption voor virtuele-Machineschaalset [VMSS] coderen van hun PaaS v2 VM's.

PaaS v1

Azure Disk Encryption is momenteel niet ondersteund voor PaaS v1. Daarom moet u versleuteling van toepassing op persistente gegevens in rust te versleutelen.  Dit omvat, maar is niet beperkt tot toepassingsgegevens, tijdelijke bestanden, logboeken en crashdumps.

De meeste services moeten proberen gebruikmaken van de versleuteling van een opslagprovider van de resource. Sommige services moeten expliciet versleuteling doen, bijvoorbeeld een sleutelmateriaal persistent (certificaten, root / de hoofdsleutel van de sleutels) moeten worden opgeslagen in de Sleutelkluis.

Als u ondersteuning bieden voor versleuteling aan servicezijde met sleutels door de klant beheerd moeten er een manier om de klant ophalen van de sleutel. De ondersteunde en aanbevolen manier om dat te doen door de integratie van Azure van met Azure Key Vault (Sleutelkluis). In dit geval kunnen klanten toevoegen en beheren van de sleutels in Azure Sleutelkluis. Een klant kan informatie over het gebruik van Azure Sleutelkluis via [aan de slag met Sleutelkluis](http://go.microsoft.com/fwlink/?linkid=521402).

Als u wilt integreren met Azure Sleutelkluis, voegt u code voor het aanvragen van een sleutel van Azure Sleutelkluis wanneer deze nodig is voor ontsleuteling.

- Zie [Azure Key Vault – stapsgewijze](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step/) voor informatie over het integreren met Azure Sleutelkluis.

Als u beheer van de klant sleutels ondersteunt, moet u bieden een UX voor de klant om op te geven welke Sleutelkluis (of Key Vault URI) te gebruiken.

Als versleuteling in rust de versleuteling van de host, infrastructuur en tenant-gegevens, het verlies van de sleutels vanwege een systeemfout is opgetreden omvat of schadelijke activiteiten zou bijvoorbeeld kunnen de versleutelde gegevens gaat verloren. Het is daarom essentieel dat de versleuteling bij rust oplossing tegen systeemfouten en schadelijke activiteiten uitgebreide disaster recovery verhaal heeft.

Services die versleuteling in rust implementeren zijn doorgaans nog steeds gevoelig zijn voor de versleutelingssleutels of gegevens blijft niet-versleuteld op de host-station (bijvoorbeeld in het wisselbestand van het hostbesturingssysteem.) Daarom moet services dat het hostvolume voor hun services is versleuteld. Om deze Compute team de implementatie van de Host-versleuteling wordt gebruikt is ingeschakeld [Bitlocker](https://technet.microsoft.com/library/dn306081.aspx) NKP en uitbreidingen voor de DCM-service en de agent voor het versleutelen van het hostvolume.

De meeste services worden geïmplementeerd op standaard Azure Virtual machines. Dergelijke services krijgt [Host versleuteling](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) automatisch wanneer Compute ingeschakeld. Voor services die worden uitgevoerd in Compute beheerde clusters host versleuteling automatisch ingeschakeld als Windows Server 2016 is geïmplementeerd.

### <a name="encryption-in-transit"></a>Codering in transit

Beveiligen van gegevens die onderweg moet essentieel onderdeel van uw strategie voor gegevensbescherming. Omdat gegevens heen en weer vanaf verschillende locaties verplaatst, is de algemene aanbeveling dat u altijd SSL/TLS-protocollen gebruiken voor het uitwisselen van gegevens op verschillende locaties. In sommige gevallen wilt u mogelijk het hele communicatiekanaal tussen uw on-premises en cloud isoleren infrastructuur met behulp van een virtueel particulier netwerk (VPN).

Gegevens verplaatsen tussen uw on-premises infrastructuur en Azure, moet u passende beveiligingsmaatregelen zoals HTTPS- of VPN.

Voor organisaties die nodig zijn om de toegang van meerdere werkstations die zich on-premises naar Azure te beveiligen, gebruikt u [Azure site-naar-site VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-site-to-site-create).

Voor organisaties die nodig zijn om de toegang van een werkstation zich on-premises naar Azure te beveiligen, gebruikt u [punt-naar-Site VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-point-to-site-create).

Grotere gegevenssets kan worden verplaatst via een speciale snelle WAN-verbinding, zoals [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Als u kiest voor ExpressRoute, kunt u ook de gegevens op het niveau van de toepassing met behulp van coderen [SSL/TLS](https://support.microsoft.com/kb/257591) of andere protocollen voor extra beveiliging.

Als u met Azure Storage via de Azure-Portal communiceert, worden alle transacties plaatsvinden via HTTPS. [REST API voor Storage](https://msdn.microsoft.com/library/azure/dd179355.aspx) via HTTPS kan ook worden gebruikt om te communiceren met [Azure Storage](https://azure.microsoft.com/services/storage/) en [Azure SQL Database](https://azure.microsoft.com/services/sql-database/).

Organisaties die niet voldoen aan het beveiligen van gegevens die onderweg zijn vatbaar voor [man-in-the-middle-aanvallen](https://technet.microsoft.com/library/gg195821.aspx), [afgeluisterd](https://technet.microsoft.com/library/gg195641.aspx), en sessiehijacking. Deze aanvallen kunnen worden de eerste stap bij het toegang krijgen tot vertrouwelijke gegevens.

U kunt meer informatie over Azure VPN-optie lezen van het artikel [Planning en ontwerp voor VPN-Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design).

### <a name="enforce-file-level-data-encryption"></a>Bestand niveau gegevensversleuteling afdwingen

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) maakt gebruik van beleidsregels voor versleuteling, identiteit en verificatie om te helpen uw bestanden en e-mail te beveiligen. Azure RMS werkt op meerdere apparaten, telefoons, tablets en pc's voor beveiliging binnen uw organisatie en buiten uw organisatie. Deze mogelijkheid is mogelijk omdat Azure RMS wordt toegevoegd een beveiligingsniveau dat aan de gegevens gekoppeld blijft, zelfs wanneer deze de fysieke grenzen van uw organisatie.

Wanneer u Azure RMS gebruikt om uw bestanden te beveiligen, gebruikt u industriestandaard cryptografie met volledige ondersteuning van [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). Wanneer u Azure RMS-gegevensbeveiliging gebruikmaken, hebt u de zekerheid dat de beveiliging blijft van toepassing op het bestand, zelfs als deze wordt gekopieerd naar opslag die niet onder het beheer van IT, zoals een cloudopslagservice. Dezelfde vindt plaats voor bestanden die worden gedeeld via e-mail, het bestand is beveiligd als bijlage aan een e-mailbericht met instructies voor het openen van de beveiligde bijlage.
Bij het plannen van de overstap op Azure RMS raden we het volgende:

- Installeer de [app RMS sharing](https://technet.microsoft.com/library/dn339006.aspx). Deze app kan worden geïntegreerd met Office-toepassingen door installatie van een Office-invoegtoepassing zodat gebruikers kunnen bestanden eenvoudig rechtstreeks beveiligen.

- Configureer toepassingen en services voor ondersteuning van Azure RMS

- Maak [aangepaste sjablonen](https://technet.microsoft.com/library/dn642472.aspx) die overeenstemming met uw zakelijke vereisten. Bijvoorbeeld: een sjabloon voor bovenste geheime gegevens die moet worden toegepast op alle bovenste geheim gerelateerde e-mailberichten.

Organisaties die zich op zwakke [gegevensclassificatie](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) en bestandsbeveiliging vatbaar voor lekken van gegevens. Zonder de juiste bestandsbeveiliging niet organisaties kunnen zakelijke inzichten te verkrijgen, op misbruik kunt controleren en te voorkomen dat schadelijke toegang tot bestanden.

> [!Note]
> U meer informatie over Azure RMS door te lezen van het artikel [aan de slag met Azure Rights Management](https://technet.microsoft.com/library/jj585016.aspx).

## <a name="secure-your-application-protect"></a>Beveiligen van uw toepassing (beveiligen)
Hoewel Azure verantwoordelijk is voor het beveiligen van de infrastructuur en het platform dat op uw toepassing wordt uitgevoerd, is uw verantwoordelijkheid voor het beveiligen van uw toepassing zelf. Met andere woorden, wilt u ontwikkelen, implementeren en beheren van uw toepassingscode en inhoud op een veilige wijze. Zonder deze kunt uw toepassingscode of inhoud nog steeds kwetsbaar voor aanvallen.

### <a name="web-application-firewall-waf"></a>Web Application Firewall (WAF)
[Web application firewall (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) is een functie van [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) die biedt gecentraliseerd beveiliging van uw webtoepassingen van algemene aanvallen en beveiligingsproblemen.

Web Application Firewall is gebaseerd op regels uit de [Core Rule Set 3.0 of 2.2.9 van OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project). Webtoepassingen zijn in toenemende mate het doel van aanvallen die gebruikmaken van veelvoorkomende bekende beveiligingsproblemen. Veelvoorkomende aanvallen zijn hierbij onder andere aanvallen met SQL-injecties en aanvallen via scripting op meerdere sites. Het kan een hele uitdaging zijn om dergelijke aanvallen in toepassingscode te voorkomen en dit kan tevens veel onderhoud, patching en controle vereisen op meerdere lagen van de toepassingstopologie. Een gecentraliseerde firewall voor webtoepassingen maakt het beveiligingsbeheer veel eenvoudiger en biedt toepassingsbeheerders meer veiligheid tegen bedreigingen of aanvallen. Een WAF-oplossing kan ook sneller reageren op een beveiligingsrisico door een patch voor een bekend beveiligingsprobleem toe te passen op een centrale locatie in plaats van elke afzonderlijke webtoepassing te beveiligen. Bestaande toepassingsgateways kunnen eenvoudig worden geconverteerd naar een toepassingsgateway met Web Application Firewall.

Hieronder ziet u enkele voorbeelden van veelvoorkomende beveiligingsproblemen waartegen Web Application Firewall bescherming biedt:

- Beveiliging tegen SQL-injecties

- Beveiliging tegen scripting op meerdere sites

- Beveiliging tegen veelvoorkomende aanvallen via internet, zoals opdrachtinjectie, het smokkelen van HTTP-aanvragen, het uitsplitsen van HTTP-antwoorden en aanvallen waarbij een extern bestand wordt ingesloten

- Beveiliging tegen schendingen van het HTTP-protocol

- Beveiliging tegen afwijkingen van het HTTP-protocol, zoals een gebruikersagent voor de host en Accept-headers die ontbreken

- Beveiliging tegen bots, crawlers en scanners

- Detectie van veelvoorkomende onvolkomenheden in toepassing (dat wil zeggen, Apache, IIS, enz.)

> [!Note]
> Voor een gedetailleerde lijst met regels en hun beschermingsmaatregelen raadpleegt u de volgende [regelsets Core](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview#core-rule-sets):

Azure biedt ook diverse functies voor eenvoudig te gebruiken om u te helpen beveiligen, zowel binnenkomend als uitgaand verkeer voor uw app. Azure ook helpt klanten hun toepassingscode beveiligen doordat extern opgegeven functionaliteit om het scannen van uw webtoepassing op beveiligingsproblemen.

- [Azure Active Directory-verificatie voor uw app instellen](https://azure.microsoft.com/blog/azure-websites-authentication-authorization/)


- [Beveiliging van verkeer naar uw app door in te schakelen Transport Layer Security (TLS/SSL) - HTTPS](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl)

    - [Alle binnenkomend verkeer via HTTPS-verbinding afdwingen](http://microsoftazurewebsitescheatsheet.info/)

  - [Strikte transportbeveiliging (HSTS) inschakelen](http://microsoftazurewebsitescheatsheet.info/#enable-http-strict-transport-security-hsts)


- [Toegang tot uw app beperken door IP-adres van client](http://microsoftazurewebsitescheatsheet.info/#filtering-traffic-by-ip)

- [Toegang tot uw app beperken door van client probleem - frequentie van de aanvraag en gelijktijdigheid van taken](http://microsoftazurewebsitescheatsheet.info/#dynamic-ip-restrictions)

- [Scannen van uw web-app-code voor beveiligingsproblemen met behulp van Tinfoil Security Scanning](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/)

- [TLS wederzijdse verificatie om te vereisen clientcertificaten verbinding maken met uw web-app configureren](https://docs.microsoft.com/azure/app-service/app-service-web-configure-tls-mutual-auth)

- [Configureren van een clientcertificaat voor gebruik van uw app veilig verbinding kunnen maken naar externe bronnen](https://azure.microsoft.com/blog/using-certificates-in-azure-websites-applications/)

- [Headers Standard van SQL server om te voorkomen dat de hulpprogramma's van uw app fingerprinting verwijderen](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/)

- [Uw app veilig verbinding kunnen maken met resources in een particulier netwerk met punt-naar-Site VPN](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet)

- [Uw app veilig verbinding kunnen maken met resources in een particulier netwerk met behulp van hybride verbindingen](https://docs.microsoft.com/azure/app-service/app-service-hybrid-connections)

Azure App Service gebruikt dezelfde antimalwareoplossing uit die worden gebruikt door Azure Cloud Services en virtuele Machines. Voor meer informatie over deze verwijzen naar onze [Antimalware documentatie](https://docs.microsoft.com/azure/security/azure-security-antimalware).

## <a name="secure-your-network-protect"></a>Beveiliging van uw netwerk (beveiligen)
Microsoft Azure omvat een robuuste netwerkinfrastructuur ter ondersteuning van uw toepassing en de vereisten voor service-connectiviteit. Verbinding met het netwerk mogelijk is tussen bronnen in Azure, tussen on-premises en Azure gehoste bronnen, en naar en van het Internet en Azure.

De [Azure netwerkinfrastructuur](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-networking-guidelines) kunt u Azure-resources veilig verbinding kunnen maken met elkaar met [virtuele netwerken (vnet's)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview). Een VNet is een weergave van uw eigen netwerk in de cloud. Een VNet is een logische isolatie van het Azure-cloud-netwerk toegewezen aan uw abonnement. U kunt de VNets verbinden met uw on-premises netwerken.

![Beveiliging van uw netwerk (beveiligen)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig6.png)

Als u Basisnetwerk toegangsbeheer op gebruikersniveau moet (op basis van IP-adres en de TCP- of UDP-protocollen), dan kunt u [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg). Een Netwerkbeveiligingsgroep (NSG) is een eenvoudige filterregels voor pakketten firewall en Hiermee kunt u toegangsbeheer op basis van een [5-tuple](https://www.techopedia.com/definition/28190/5-tuple).

Azure-netwerken ondersteunt de mogelijkheid om aan te passen van het routeringsgedrag voor netwerkverkeer op uw virtuele netwerken van Azure. U kunt dit doen door het configureren van [zelfgedefinieerde Routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) in Azure.

[Geforceerde tunneling](https://www.petri.com/azure-forced-tunneling) is een mechanisme die u gebruiken kunt om ervoor te zorgen dat uw services niet zijn toegestaan voor het initiëren van een verbinding met apparaten op Internet.

Azure ondersteunt toegewezen WAN-koppeling connectiviteit voor uw on-premises netwerk en een Azure-netwerk met [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction). De koppeling tussen Azure en uw site maakt gebruik van een speciale verbinding die u niet via het openbare Internet gaat. Als uw Azure-toepassing wordt uitgevoerd in meerdere datacenters, kunt u [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) op route-aanvragen van gebruikers op intelligente wijze over exemplaren van de toepassing. Ook kunt u verkeer routeren voor services in Azure niet worden uitgevoerd als ze toegankelijk vanaf Internet zijn.

## <a name="virtual-machine-security-protect"></a>Beveiliging van de virtuele machine (beveiligen)

[Virtuele Machines in Azure](https://docs.microsoft.com/azure/virtual-machines/) kunt u een breed scala aan oplossingen computergebruik in een flexibele manier te implementeren. Met ondersteuning voor Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP en Azure BizTalk Services kunt u elke workload en elke taal implementeren op vrijwel elk besturingssysteem.

Met Azure, kunt u [antimalwaresoftware](https://docs.microsoft.com/azure/security/azure-security-antimalware) van beveiliging leveranciers zoals Microsoft, Symantec, Trend Micro en Kaspersky uw virtuele machines beschermen tegen schadelijke bestanden, adware en andere dreigingen.

Microsoft Antimalware voor Azure Cloud Services en virtuele Machines is een functie real-timebeveiliging die helpt bepalen en virussen, spyware en andere schadelijke software verwijderen. Microsoft Antimalware biedt configureerbare waarschuwingen wanneer bekende schadelijke of ongewenste software probeert zichzelf te installeren of uitvoeren op uw Azure-systemen.

[Azure Backup](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) is een schaalbare oplossing die uw toepassingsgegevens met nul investeringen en minimale beschermt bedrijfskosten. Toepassingsfouten kunnen uw gegevens beschadigen, en menselijke fouten kunnen fouten introduceren in uw toepassingen. Uw virtuele machines met Windows en Linux zijn beveiligd met Azure Backup.

[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) helpt bij het indelen replicatie, failovers en herstel van workloads en apps, zodat ze beschikbaar vanaf een secundaire locatie zijn als uw primaire locatie uitvalt.

## <a name="ensure-compliance-cloud-services-due-diligence-checklist-protect"></a>Naleving: Cloudservices vanwege toewijding inzetten Controlelijst (beveiligen)

Microsoft heeft ontwikkeld [de controlelijst voor Cloud Services toewijding inzetten vanwege](https://aka.ms/cloudchecklist.download) waarmee organisaties dat oefening innen als ze een verplaatsing naar de cloud overwegen. Dit biedt een structuur voor een organisatie met een grootte en het type: persoonlijke bedrijven en organisaties van openbare-sector, met inbegrip van de overheid op alle niveaus en non-profitorganisaties: voor het identificeren van hun eigen prestaties, service, beheer en toezicht doelstellingen en vereisten. Hierdoor kunnen ze de aanbiedingen van andere cloudserviceproviders, uiteindelijk vormen de basis voor een cloud-serviceovereenkomst vergelijken.

De controlelijst biedt een framework dat component door component met een nieuwe internationale standaard voor cloud serviceovereenkomsten, ISO/IEC 19086 wordt uitgelijnd. Deze standaard biedt een consistente verzameling overwegingen voor organisaties om te nemen van beslissingen over cloud acceptatie en een compleet gemeenschappelijke voor het vergelijken van serviceaanbiedingen cloud maken.

De controlelijst bijdraagt aan een grondig gerenommeerde te verplaatsen naar de cloud, bieden gestructureerde richtlijnen en een consistente, herhaalbare aanpak voor het kiezen van een cloud-serviceprovider.

Acceptatie van de cloud is niet langer gewoon een beslissing technologie. Controlelijst vereisten touch op elk aspect van een organisatie, daarom gebruikt voor het organiseren van alle belangrijke interne-besluitvormers: de CIO en CISO evenals de juridische, bestaat de kans professionals beheer- en inkoop en naleving. Dit verhoogt de efficiëntie van de besluitvormingsproces en compleet beslissingen ten aanzien van geluid redenering, waardoor de kans op onvoorziene problemen stimuleren.

Daarnaast biedt de Controlelijst:

- Beschrijft de bespreking van de belangrijkste onderwerpen voor besluitvormers aan het begin van het overstapproces voor de cloud.

- Biedt ondersteuning voor uitgebreide zakelijke discussies over voorschriften en de doelstellingen van organisatie voor privacy, persoonsgegevens (PII) en beveiliging van gegevens.

- Helpt organisaties bij potentiële problemen die invloed kunnen zijn op een cloudproject op te sporen.

- Biedt een consistente set vragen, met dezelfde voorwaarden, definities, metrische gegevens en producten voor elke provider, de vergelijking van aanbiedingen van andere cloudserviceproviders vereenvoudigen.

## <a name="azure-infrastructure-and-application-security-validation-detect"></a>Azure-infrastructuur en toepassing beveiligingsvalidatie (detecteren)

[Azure bedrijfsbeveiliging](https://docs.microsoft.com/azure/security/azure-operational-security) verwijst naar de services, besturingselementen en functies die beschikbaar zijn voor gebruikers voor het beveiligen van hun gegevens, toepassingen en andere elementen in Microsoft Azure.

![mislukte beveiligingsvalidaties (detecteren)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig7.png)

Azure operationele beveiliging is gebaseerd op een framework dat de kennis die is opgedaan met een verschillende mogelijkheden die uniek voor Microsoft zijn, met inbegrip van de Microsoft Security Development Lifecycle (SDL), het programma Microsoft Security Response Centre opgenomen , en grondige kennis van de threat cybersecurity Liggend.

### <a name="microsoft-operations-management-suiteoms"></a>Microsoft operations management suite(OMS)

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) kan de IT-beheeroplossing voor de hybride cloud. Alleen wordt gebruikt of als u wilt uitbreiden van uw bestaande System Center-implementatie, OMS biedt u de maximale flexibiliteit en controle voor cloud-gebaseerde beheer van uw infrastructuur.

![Microsoft operations management suite(OMS)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig8.png)

Met OMS, kunt u een instantie in een cloud, met inbegrip van lokale, Azure, AWS, Windows Server, Linux, VMware en OpenStack, tegen lagere kosten dan concurrerende oplossingen beheren. OMS is gebouwd voor de cloud eerste, biedt een nieuwe benadering voor het beheren van uw onderneming is de snelste, meest rendabele manier om te voldoen aan de nieuwe zakelijke uitdagingen en geschikt voor de nieuwe werkbelastingen, toepassingen en cloudomgevingen.

### <a name="log-analytics"></a>Log Analytics

[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) biedt bewakingsservices voor OMS door in een centrale opslagplaats gegevens te verzamelen van beheerde resources. Deze gegevens kunnen gebeurtenissen, prestatiegegevens en aangepaste gegevens omvatten die via de API worden geleverd. Na verzameling zijn de gegevens beschikbaar voor waarschuwingen, analyse en export.

![Log Analytics](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig9.png)

Deze methode kunt u gegevens uit diverse bronnen consolideren, zodat u kunt combineren gegevens van uw Azure-services met uw bestaande lokale omgeving. De methode maakt ook een duidelijk onderscheid tussen het verzamelen van gegevens en het bewerken hiervan. Zo zijn alle bewerkingen beschikbaar voor alle soorten gegevens.

### <a name="azure-security-center"></a>Azure security center

[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) helpt u bij het detecteren, voorkomen van en reageren op bedreigingen dankzij een verhoogde zichtbaarheid van en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

Security Center analyseert de beveiligingsstatus van uw Azure-resources om mogelijke beveiligingsproblemen op te sporen. Een lijst met aanbevelingen begeleidt u bij het configureren van benodigde besturingselementen.

Voorbeelden zijn:

- Inrichting van antimalware om schadelijke software te identificeren en te verwijderen

- Netwerkbeveiligingsgroepen en regels voor het verkeer voor beheer voor virtuele machines configureren

- Inrichten van Web Application Firewalls om te beschermen tegen aanvallen die zijn gericht op uw webtoepassingen

- Implementatie van ontbrekende systeemupdates

- Aanpassing van besturingssysteemconfiguraties die niet overeenkomen met de aanbevolen basislijnen

Security Center verzamelt, analyseert en integreert automatisch logboekgegevens van uw Azure-resources, het netwerk en partneroplossingen zoals antimalwareprogramma's en firewalls. Wanneer er dreigingen worden gedetecteerd, wordt een beveiligingswaarschuwing gemaakt. Voorbeelden zijn detectie van:

- Verdachte VM's die communiceren met bekende schadelijke IP-adressen

- Geavanceerde malware die is gedetecteerd met Windows Foutrapportage

- Beveiligingsaanvallen op virtuele machines

- Beveiligingswaarschuwingen van geïntegreerde antimalwareprogramma's en firewalls

### <a name="azure-monitor"></a>Monitor voor Azure

[Monitor voor Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) bevat koppelingen naar informatie over specifieke typen resources. Biedt visualisatie, query routering, waarschuwingen, automatisch schalen en automatisering op gegevens zowel in de Azure-infrastructuur (activiteitenlogboek) en elke afzonderlijke Azure-resource (diagnostische logboeken).

Cloud-toepassingen zijn complexe met veel bewegende onderdelen. Monitoring biedt gegevens om ervoor te zorgen dat uw toepassing up blijft en wordt uitgevoerd in een foutloze toestand bevindt. Ook kunt u potentiële problemen voorkomen of oplossen uit het verleden zijn.

![Monitor voor Azure](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig10.png) Bovendien kunt u bewakingsgegevens grondige om inzicht te krijgen over uw toepassing. Deze kennis kan u helpen bij het verbeteren van de prestaties van toepassingen of onderhoud of acties die anders worden handmatige interventie moeten automatiseren.

Controle van de netwerkbeveiliging van uw is het essentieel zijn voor netwerk beveiligingsproblemen detecteren en de naleving van uw IT-beveiliging en regelgeving governance model. Overzicht van de beveiligingsgroep, kunt u de geconfigureerde Netwerkbeveiligingsgroep en beveiligingsregels voor verbindingen, evenals de effectieve beveiligingsregels voor verbindingen ophalen. Met de lijst met regels die zijn toegepast, kunt u bepalen de poorten die geopend zijn en ss beveiligingsproblemen van het netwerk.

### <a name="network-watcher"></a>Netwerk-watcher

[Netwerk-Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) is een regionale service waarmee u kunt bewaken en onderzoeken van de voorwaarden op het netwerkniveau van een in, naar en van Azure. Netwerkcontrole en visualisatie hulpprogramma's beschikbaar met de netwerk-Watcher kunnen u begrijpen, diagnoses stellen en Verkrijg inzicht in uw netwerk in Azure. Deze service omvat pakketopname, volgende hop, IP-stroom controleren, groep beveiligingsweergave, NSG stroom Logboeken. Scenario niveau bewaking biedt een complete weergave van netwerkbronnen in tegenstelling tot afzonderlijke resource netwerkbewaking.

### <a name="storage-analytics"></a>Opslaganalyses

[Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) metrische gegevens die samengevoegde statistieken en capaciteit transactiegegevens over aanvragen met storage-service zijn kan opslaan. Transacties worden gerapporteerd op beide API bewerking niveau en op het niveau van de service storage en capaciteit op het niveau van de service storage wordt gerapporteerd. Metrische gegevens kan worden gebruikt om storage-service analyseren, onderzoeken van problemen met aanvragen ten opzichte van de storage-service en verbeteren de prestaties van toepassingen die gebruikmaken van een service.

### <a name="application-insights"></a>Application insights

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) is een uitbreidbaar Management APM (Application Performance)-service voor webontwikkelaars op meerdere platforms. Hiermee kunt u uw livewebtoepassing controleren. Afwijkende prestaties worden automatisch gedetecteerd. Dit omvat analytics krachtige hulpprogramma's voor hulp bij het vaststellen van problemen en om te begrijpen wat gebruikers doen met uw app. Het is bedoeld om u te helpen de prestaties en bruikbaarheid continu te verbeteren. Het werkt voor apps op uiteenlopende platforms, waaronder .NET, Node.js en J2EE, on-premises of in de cloud gehost. Het kan worden geïntegreerd met uw devOps-proces en verwijst naar een verschillende ontwikkelingsprogramma's heeft.

Met deze service kunt u het volgende controleren:

- **Aantal aanvragen, reactietijden en foutpercentages** - ga na welke pagina's het populairst zijn op welke tijdstippen van de dag en waar uw gebruikers zich bevinden. Ontdek welke pagina's het beste presteren. Als uw reactietijden en foutpercentages omhoog gaan wanneer er meer aanvragen binnenkomen, hebt u mogelijk te weinig resources.

- **Aantal afhankelijkheidsrelaties, reactietijden en foutpercentages** - controleer of externe services zorgen voor vertraging.

- **Uitzonderingen** - analyseren van de samengevoegde statistieken of Kies specifieke exemplaren en inzoomen op de stack-trace en de verwante aanvragen. Zowel server- als browseruitzonderingen worden gerapporteerd.

- **Paginaweergaven en de prestaties bij het laden van pagina’s** - deze gegevens worden gerapporteerd door de browsers van uw gebruikers.

- **AJAX-aanroepen van webpagina's** -tarieven responstijden en uitvalpercentage.

- **Gebruikers- en sessiegegevens telt.**

- **Prestatiemeteritems** van uw Windows- of Linux-servers, zoals die voor CPU-, geheugen- en netwerkgebruik.

- **Diagnostische gegevens van hosts** van Docker of Azure.

- **Diagnostische traceerlogboeken** van uw app - met behulp hiervan kunt u de samenhang vaststellen tussen traceergebeurtenissen en aanvragen.

- **Aangepaste gebeurtenissen en metrische gegevens** zelf in de code client of server zakelijke gebeurtenissen bijhouden zoals verkochte artikelen, of games gewonnen te schrijven.
De infrastructuur voor uw toepassing bestaat meestal uit veel onderdelen, zoals een virtuele machine, een opslagaccount en een virtueel netwerk, of een webtoepassing, database, databaseserver en services van derden. Deze onderdelen moet u niet zien als afzonderlijke entiteiten, maar als onderdelen die één entiteit vormen en aan elkaar zijn gerelateerd en afhankelijk zijn van elkaar. U implementeert, beheert en bewaakt deze onderdelen als groep. [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) kunt u werken met de resources in uw oplossing als een groep.

U kunt alle resources voor uw oplossing implementeren, bijwerken of verwijderen in een enkele, gecoördineerde bewerking. Voor implementatie gebruikt u een sjabloon. Deze sjabloon kan voor verschillende omgevingen worden gebruikt, zoals testen, faseren en productie. Resource Manager biedt beveiliging, controle en tagfuncties die u na de implementatie helpen bij het beheren van uw resources.

**De voordelen van het gebruik van Resource Manager**

Resource Manager biedt diverse voordelen:

- U kunt alle resources voor uw oplossing implementeren, beheren en bewaken als groep, in plaats van deze resources afzonderlijk te verwerken.

- U kunt gedurende de ontwikkelingscyclus uw oplossing herhaaldelijk implementeren en erop vertrouwen dat uw resources consistent worden geïmplementeerd.

- U kunt uw infrastructuur beheren via declaratieve sjablonen in plaats van scripts.

- U kunt de afhankelijkheden tussen resources, zodat deze zijn geïmplementeerd in de juiste volgorde definiëren.

- U kunt toegangsbeheer toepassen op alle services in de resourcegroep omdat op rollen gebaseerd toegangsbeheer (RBAC) is geïntegreerd in het beheerplatform.

- U kunt tags toepassen op de resources om alle resources in uw abonnement op een logische manier te organiseren.

- U kunt de facturering van uw organisatie transparanter maken door te kijken naar de kosten voor een groep resources met dezelfde tag.

> [!Note]
> Resource Manager biedt een nieuwe manier om uw oplossingen te implementeren en te beheren. Als u het eerdere implementatiemodel en gewenste gebruikt voor meer informatie over de wijzigingen, Zie [Understanding Resource Manager-implementatie en klassieke implementatie](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over beveiliging door het lezen van enkele van de onderwerpen over onze uitgebreide beveiliging:

- [Controle en logboekregistratie](https://www.microsoft.com/en-us/trustcenter/security/auditingandlogging)

- [Cybercriminaliteit](https://www.microsoft.com/en-us/trustcenter/security/cybercrime)

- [Ontwerp- en operationele beveiliging](https://www.microsoft.com/en-us/trustcenter/security/designopsecurity)

- [Versleuteling](https://www.microsoft.com/en-us/trustcenter/security/encryption)

- [Identiteits-en toegang](https://www.microsoft.com/en-us/trustcenter/security/identity)

- [Netwerkbeveiliging](https://www.microsoft.com/en-us/trustcenter/security/networksecurity)

- [Beveiligingsbeheer](https://www.microsoft.com/en-us/trustcenter/security/threatmanagement)
