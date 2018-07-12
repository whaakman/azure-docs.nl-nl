---
title: Technische mogelijkheden van Azure-beveiliging | Microsoft Docs
description: Meer informatie over cloud-gebaseerde computing services met een ruime keuze aan rekenprocessen en -services die u omhoog en omlaag automatisch schalen kunnen om te voldoen aan de behoeften van uw toepassing of enterprise.
services: security
documentationcenter: na
author: UnifyCloud
manager: mbaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2017
ms.author: TomSh
ms.openlocfilehash: 6643db7b732cc5b01ce7602eb3d679c130c46720
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972193"
---
# <a name="azure-security-technical-capabilities"></a>Technische mogelijkheden van Azure-beveiliging

Bij de huidige en toekomstige Azure klanten te begrijpen en gebruikmaken van de verschillende beveiligingsgerelateerde mogelijkheden die beschikbaar zijn in en rondom het Azure-Platform, Microsoft heeft ontwikkeld die een reeks White Papers, overzichten van de beveiliging, aanbevolen procedures en Controlelijsten voor. De onderwerpen in termen van diepgaande en brede toepassing variëren en worden regelmatig bijgewerkt. Dit document is onderdeel van de reeks zoals samengevat in de onderstaande sectie Abstract. Meer informatie over deze Azure-beveiliging-serie kan worden gevonden op (URL).

## <a name="azure-platform"></a>Azure-platform

[Microsoft Azure](https://azure.microsoft.com/overview/what-is-azure/) een cloudplatform bestaat uit de infrastructuur en toepassingsservices, met geïntegreerde gegevensservices en geavanceerde analyses en ontwikkelhulpprogramma's en services, die worden gehost binnen Microsoft de openbare cloud data centers. Klanten kunnen gebruiken Azure voor veel verschillende capaciteiten en scenario's, vanaf de basis van de berekenings-, netwerk- en opslag, mobiele en web-app-services naar volledige cloud-scenario's, zoals Internet of Things, en worden gebruikt met open source-technologieën en geïmplementeerd als hybride cloud of gehoste binnen het datacenter van een klant. Azure biedt cloudtechnologie zoals bouwstenen Bespaar op kosten, bedrijven te helpen om snel innoveren en systemen proactief beheren. Wanneer u baseren of IT-activa naar een cloudprovider migreren, zijn u vertrouwen op de beveiligingmogelijkheden die aan uw toepassingen en gegevens beschermen met de services en de besturingselementen die ze bieden voor het beheren van de beveiliging van uw cloud-gebaseerde activa.

Microsoft Azure is de enige cloud computing-provider die biedt een platform voor beveiligde, consistente toepassingen en infrastructuur-as-a-service voor teams binnen hun verschillende cloud-vaardigheden en complexiteitsniveaus project, met geïntegreerde dataservices werken en analyses die intelligence van gegevens onthullen waar deze, via zowel Microsoft als niet-Microsoft-platforms bestaat, frameworks en hulpprogramma's, biedt opties voor het integreren van cloud met on-premises ook implementeren van Azure-cloud-services binnen openen on-premises datacenters. Als onderdeel van de vertrouwde Microsoft-Cloud vertrouwen klanten op Azure voor toonaangevende beveiliging, betrouwbaarheid, naleving, privacy en het enorme netwerk van mensen, partners en processen voor de ondersteuning van organisaties in de cloud.

Met Microsoft Azure, kunt u het volgende doen:

- Versnel innovatie met de cloud.

- Zakelijke beslissingen en apps met inzichten.

- Bouw naar eigen inzicht en overal implementeren.

- Bescherm hun bedrijf.

## <a name="scope"></a>Bereik

Het centrale punt van dit technisch document heeft betrekking op beveiligingsfuncties en -functionaliteit met ondersteuning van de kernonderdelen van Microsoft Azure, namelijk [Microsoft Azure Storage](https://docs.microsoft.com/azure/storage/storage-introduction), [Microsoft Azure SQL-Databases](https://docs.microsoft.com/azure/sql-database/), [ Van Microsoft Azure virtuele machine model](https://docs.microsoft.com/azure/virtual-machines/), en de hulpprogramma's en de infrastructuur die alles. In dit technisch document wordt de focus op Microsoft Azure technische mogelijkheden beschikbaar is als klanten te voldoen aan hun rol bij het beschermen van de beveiliging en privacy van gegevens.

Het belang van inzicht in deze gedeelde verantwoordelijkheid-model is het essentieel is voor klanten die worden verplaatst naar de cloud. Cloudproviders bieden aanzienlijke voordelen voor beveiliging en naleving inspanningen, maar deze voordelen niet absolve doen voor de klant van het beveiligen van hun gebruikers, toepassingen en service-aanbiedingen.

Voor IaaS-oplossingen, de klant is verantwoordelijk of een gedeelde verantwoordelijkheid voor het beveiligen en beheren van het besturingssysteem, netwerkconfiguratie, toepassingen, identiteit, clients en gegevens.  PaaS-oplossingen bouwen op IaaS-implementaties, de klant is nog steeds verantwoordelijk of een gedeelde verantwoordelijkheid voor het beveiligen en beheren van toepassingen, identiteit, clients en gegevens. Voor SaaS-oplossingen, Nonetheless, blijft de klant verantwoording. Zij moeten ervoor zorgen dat gegevens correct is ingedeeld en ze een verantwoordelijkheid delen voor het beheren van hun gebruikers- en eindpunt-apparaten.

Dit document biedt geen gedetailleerde dekking van een van de bijbehorende onderdelen van Microsoft Azure-platform, zoals Azure Web Sites, Azure Active Directory, HDInsight, Media Services en andere services die op de belangrijkste onderdelen zijn gelaagd. Hoewel een minimumniveau van algemene informatie is opgegeven, lezers wordt aangenomen dat bekend bent met Azure basic-concepten, zoals beschreven in de verwijzingen naar andere geleverd door Microsoft en opgenomen in de koppelingen in dit technische document.

## <a name="available-security-technical-capabilities-to-fulfil-user-customer-responsibility---big-picture"></a>Technische mogelijkheden van de beschikbare beveiligingsupdates te voldoen aan de verantwoordelijkheid van de gebruiker (s) - totaalbeeld

Microsoft Azure biedt services waarmee klanten kunnen voldoen aan de behoeften voor beveiliging, privacy en naleving. In de volgende afbeelding kunt u verschillende Azure-services beschikbaar zijn voor gebruikers om te bouwen een infrastructuur met veilige en compatibele toepassingen op basis van industrienormen uitgelegd.

![Beschikbare beveiligingsupdates technische mogelijkheden grote afbeelding](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig1.png)

## <a name="manage-and-control-identity-and-user-access-protect"></a>Beheren en besturen identiteit en gebruikerstoegang toegang (beveiligen)

Azure helpt u zakelijke en persoonlijke gegevens beveiligen doordat u gebruikersidentiteiten en referenties te beheren en toegang beheren.

### <a name="azure-active-directory"></a>Azure Active Directory

Identiteits- en toegangsbeheer management solutions Help bij Microsoft IT toegang beveiligen tot toepassingen en bronnen in het datacenter van de zakelijke en naar de cloud, extra niveaus van validatie, zoals meervoudige verificatie en voorwaardelijke toegang inschakelen beleidsregels. Bewaking verdachte activiteiten via geavanceerde beveiliging reporting, controle en waarschuwingen helpt bij het beperken potentiële beveiligingsproblemen met zich mee. [Azure Active Directory Premium](https://docs.microsoft.com/azure/active-directory/active-directory-editions) biedt eenmalige aanmelding voor duizenden cloud-apps (SaaS) en toegang tot web-apps die u on-premises uitvoert.

De voordelen van de beveiliging van Azure Active Directory (Azure AD) zijn de mogelijkheid om:

- Maken en beheren van één identiteit voor elke gebruiker binnen uw onderneming hybride, synchroon houden van gebruikers, groepen en apparaten.

- Eenmalige aanmelding toegang bieden tot uw toepassingen met inbegrip van duizenden vooraf geïntegreerde SaaS-apps.

- Inschakelen van beveiliging van de toegang tot toepassingen door het afdwingen van multi-factor Authentication op basis van regels voor zowel on-premises en cloudtoepassingen.

- Veilige externe toegang tot on-premises webtoepassingen via Azure AD Application Proxy richt.

De [Azure Active Directory-portal](http://aad.portal.azure.com/) vindt u een deel van de Azure-portal. U kunt vanuit dit dashboard een overzicht van de status van uw organisatie en dieper in te gaan met het beheren van de directory, gebruikers of toegang tot toepassingen.

![Azure Active Directory](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig2.png)

Hier volgen de belangrijkste Azure Identity management mogelijkheden:

- Eenmalige aanmelding

- Multi-Factor Authentication

- Beveiligingsbewaking, meldingen en machine learning gebaseerde rapporten

- Identiteits- en toegangsbeheer van consumenten

- Apparaatregistratie

- Privileged identity management

- Identiteitsbeveiliging

#### <a name="single-sign-on"></a>Eenmalige aanmelding

[Eenmalige aanmelding (SSO)](https://azure.microsoft.com/documentation/videos/overview-of-single-sign-on/) betekent toegang te hebben tot alle toepassingen en bronnen die u nodig hebt om zaken te doen, als u zich aanmeldt bij het gebruik van slechts één keer één gebruikersaccount. Nadat u bent aangemeld, u toegang hebt tot alle toepassingen, zonder dat nodig is om te verifiëren (bijvoorbeeld, typ een wachtwoord) een tweede keer.

Veel organisaties zijn afhankelijk van de software als een service (SaaS)-toepassingen zoals Office 365, Box en Salesforce voor de productiviteit van eindgebruikers. In het verleden, IT-personeel die nodig zijn voor het afzonderlijk maken en bijwerken van gebruikersaccounts in elke SaaS-toepassing en moesten gebruikers een wachtwoord voor elke SaaS-toepassing.

[Azure AD uitbreiding van on-premises Active Directory naar de cloud](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis), zodat gebruikers hun primaire organisatie-account niet alleen te gebruiken zich aanmelden bij hun domein apparaten en resources van het bedrijf, maar ook alle de web- en SaaS-toepassingen die nodig zijn voor de taak.

Niet alleen gebruikers hoeven niet te beheren van meerdere sets met gebruikersnamen en wachtwoorden, toegang tot de toepassing kan worden automatisch ingericht of is verlopen op basis van groepen in de organisatie en hun status als een werknemer. [Introduceert Azure AD security- en toegangsbeheer van governance](https://docs.microsoft.com/azure/active-directory/active-directory-sso-integrate-saas-apps) waarmee u kunt centraal beheren van toegang van gebruikers voor SaaS-toepassingen.

#### <a name="multi-factor-authentication"></a>Multi-Factor Authentication

[Azure multi-factor Authentication (MFA)](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) is een verificatiemethode waarbij het gebruik van meer dan één verificatiemethode is vereist en wordt een essentiële tweede beveiligingslaag toegevoegd aan gebruikersaanmeldingen en transacties. [MFA helpt beschermen](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-how-it-works) toegang tot gegevens en toepassingen aan de wensen van gebruikers voor een eenvoudige aanmeldprocedure. Het biedt een robuuste verificatie met een scala aan opties voor verificatie: telefoonoproep, tekstbericht of mobiele app notification of verificatie van code en van derden OAuth-tokens.

#### <a name="security-monitoring-alerts-and-machine-learning-based-reports"></a>Beveiligingsbewaking, meldingen en machine learning gebaseerde rapporten

Beveiligingsbewaking, meldingen en machine learning gebaseerde rapporten die inconsistente toegangspatronen identificeren kunt u uw bedrijf te beveiligen. U kunt de toegang van Azure Active Directory, en gebruiksrapporten meer inzicht verkrijgen in de integriteit en beveiliging van de adreslijst van uw organisatie. Met deze informatie bepalen een directory-beheerder beter waar mogelijke beveiligingsrisico's schuilen zodat ze voldoende plannen maken kunnen die risico's te beperken.

In de Azure-portal of via de [Azure Active Directory-portal](http://aad.portal.azure.com/), [rapporten](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) zijn onderverdeeld in de volgende manieren:

- Anomaliedetectie-rapporten: aanmelden gebeurtenissen waarop we gevonden worden afwijkende bevatten. Ons doel is u op de hoogte van deze activiteit maken en kunt u bepalen of een gebeurtenis verdacht is.

- Geïntegreerde toepassing rapporten – bieden inzicht in hoe cloud-toepassingen in uw organisatie worden gebruikt. Azure Active Directory biedt integratie met duizenden cloudtoepassingen.

- Foutenrapporten – duiden op fouten die optreden bij het inrichten van accounts voor externe toepassingen.

- Rapporten voor specifieke gebruikers – weergeven apparaat/teken in de activiteitsgegevens voor een specifieke gebruiker.

- Activiteitenlogboeken – bevatten een record van alle gecontroleerde gebeurtenissen binnen de afgelopen 24 uur, afgelopen 7 dagen of afgelopen 30 dagen en groep is gewijzigd en het wachtwoord opnieuw instellen en registratie-activiteit.

#### <a name="consumer-identity-and-access-management"></a>Identiteits- en toegangsbeheer van consumenten

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) is een maximaal beschikbare, wereldwijde, identiteitsbeheerservice voor consumentgerichte toepassingen die kan worden opgeschaald naar honderden miljoenen identiteiten. De service kan worden geïntegreerd in zowel mobiele als webplatforms. Uw klanten kunnen zich bij al uw toepassingen aanmelden via verschillende aanpasbare methoden met hun bestaande sociale accounts of door nieuwe aanmeldingsgegevens te maken.

In de afgelopen, ontwikkelaars van toepassingen die wilden [registreren en aanmelden bij de consumenten](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview) in hun toepassingen schreven hun eigen code. Bovendien gebruikten ze on-premises databases of systemen om gebruikersnamen en wachtwoorden op te slaan. Azure Active Directory B2C biedt uw organisatie een betere manier om identiteitsbeheer van consumenten integreren in toepassingen met behulp van een beveiligde, op standaarden gebaseerd platform en een grote set uitbreidbare beleidsregels.

Wanneer u Azure Active Directory B2C, kunnen uw consumenten zich registreren voor uw toepassingen met behulp van hun bestaande sociale accounts (Facebook, Google, Amazon, LinkedIn) of door nieuwe referenties (e-mailadres en wachtwoord of gebruikersnaam en wachtwoord) te maken.

#### <a name="device-registration"></a>Apparaatregistratie

[Azure AD-apparaatregistratie](https://docs.microsoft.com/azure/active-directory/device-management-introduction) vormt de basis voor apparaatgebaseerde [voorwaardelijke toegang](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup) scenario's. Wanneer een apparaat is geregistreerd, biedt Azure AD-apparaatregistratie het apparaat met een identiteit die wordt gebruikt voor verificatie van het apparaat, wanneer de gebruiker zich aanmeldt. Het geverifieerde apparaat en de kenmerken van het apparaat kunnen vervolgens worden gebruikt voor het afdwingen van voorwaardelijk toegangsbeleid voor toepassingen die in de cloud en on-premises worden gehost.

In combinatie met een [beheer van mobiele apparaten (MDM)](https://www.microsoft.com/itshowcase/Article/Content/588/Mobile-device-management-at-Microsoft) -oplossing, zoals Intune, de apparaatkenmerken in Azure Active Directory zijn bijgewerkt met extra informatie over het apparaat. Hiermee kunt u extra regels voor voorwaardelijke toegang maken die toegang afdwingen van apparaten, zodat ze voldoen aan uw standaarden voor beveiliging en compliance

#### <a name="privileged-identity-management"></a>Privileged identity management

[Azure Active Directory (AD) Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) kunt u beheren, bepalen, en bevoegde identiteiten en toegang tot resources in Azure AD, evenals andere Microsoft online services zoals Office 365 en Microsoft Intune.

Soms moeten gebruikers bevoorrechte bewerkingen in Azure of Office 365-resources en andere SaaS-apps uitvoeren. Dit betekent vaak dat organisaties hebben zodat ze permanente bevoorrechte toegang in Azure AD. Dit is een groeiende beveiligingsrisico voor de cloud gehoste bronnen omdat organisaties voldoende kunnen niet controleren wat gebruikers doen met hun beheerdersbevoegdheden. Ook als een gebruikersaccount met bevoegde toegang is geknoeid, die een inbreuk kan van invloed zijn op de algehele cloudbeveiliging. Azure AD Privileged Identity Management helpt bij het oplossen van dit risico.

Azure AD Privileged Identity Management kunt u:

- Zien welke gebruikers zijn beheerders van Azure AD

- Op aanvraag, 'just-in-time' beheerderstoegang tot Microsoft Online Services zoals Office 365 en Intune inschakelen

- Rapporten over beheerder beheerderstoeganggeschiedenis en wijzigingen in toewijzingen van beheerder ophalen

- Ontvang waarschuwingen over toegang tot een bevoorrechte rol

#### <a name="identity-protection"></a>Identiteitsbeveiliging

[Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) is een beveiligingsservice die een geconsolideerde weergave risicogebeurtenissen en mogelijke beveiligingsproblemen die betrekking hebben op de identiteiten van uw organisatie biedt. Identity Protection maakt gebruik van bestaande van Azure Active Directory anomaly detectiemogelijkheden (beschikbaar via Azure AD-rapporten voor afwijkende activiteiten) en introduceert nieuwe typen risicogebeurtenissen die in realtime afwijkingen kunnen.

## <a name="secured-resource-access-in-azure"></a>Toegang tot beveiligde resources in Azure

Toegangsbeheer in Azure wordt gestart vanuit het oogpunt van de facturering. De eigenaar van een Azure-account toegankelijk is voor uw bezoek de [Azure Account Center](https://account.windowsazure.com/subscriptions), is het Account Administrator (AA). Abonnementen vormen een container voor facturering, maar ook fungeren ze als een beveiligingsgrens: elk abonnement heeft een Service systeembeheerder (SA) die u kunt toevoegen, verwijderen en aanpassen van Azure-resources in het desbetreffende abonnement via de Azure-portal. De SA van een nieuw abonnement in de standaard is de AA, maar de AA kunt wijzigen in de SA in het Azure-Accountcentrum.

![Toegang tot beveiligde resources in Azure](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig3.png)

Abonnementen hebben ook een koppeling met een directory. De map definieert een groep gebruikers. Dit kunnen gebruikers van werk of school die de map gemaakt zijn kunnen, of ze externe gebruikers (dat wil zeggen, een Microsoft-Accounts). Abonnementen zijn ook toegankelijk via een subset van de directorygebruikers die zijn toegewezen als Service-beheerder (SA) of CO-beheerder (CA); de enige uitzondering hierop is dat, vanwege de verouderde Microsoft-Accounts (voorheen Windows Live ID) kan worden toegewezen als SA of CA zonder aanwezig zijn in de map.

Beveiliging-georiënteerde bedrijven moeten zich richten op uw werknemers de exacte machtigingen die ze nodig hebben. Te veel machtigingen kunnen een account voor aanvallen worden blootgesteld. Te weinig machtigingen betekenen dat werknemers hun werk efficiënter kunnen niet ophalen. [Azure Role-Based Access Control (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) helpt dit probleem oplossen door het aanbieden van verfijnd toegangsbeheer voor Azure.

![Toegang tot beveiligde bronnen ](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig4.png)

Met RBAC kunt u taken scheiden binnen uw team en alleen de mate van toegang verlenen aan gebruikers die nodig is om de taken uit te voeren. In plaats van zodat iedereen onbeperkte machtigingen in uw Azure-abonnement of resources, kunt u alleen bepaalde acties toestaan. Bijvoorbeeld, RBAC gebruiken om één werknemer virtuele machines in een abonnement beheren terwijl een andere SQL-databases binnen hetzelfde abonnement kunt beheren.

![Toegang tot beveiligde resources in Azure (RBAC)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig5.png)

## <a name="azure-data-security-and-encryption-protect"></a>Azure-gegevensbeveiliging en -versleuteling (beveiligen)

Een van de sleutels voor de bescherming van gegevens in de cloud is accounting voor de mogelijke statussen in die uw gegevens zich kunnen voordoen en welke besturingselementen beschikbaar zijn voor die status zijn. De aanbevelingen zijn Azure-gegevens en encryption aanbevolen procedures voor de statussen van de volgende gegevens.

- Inactieve: Dit bevat alle informatie opslagobjecten, containers en typen die statisch aanwezig zijn op de fysieke media, worden deze magnetische of optische schijven.

- In-Transit: Wanneer gegevens worden overgebracht tussen onderdelen, locaties of programma's, zoals als meer dan het netwerk via een service bus (van on-premises naar de cloud en vice versa, met inbegrip van hybride verbindingen, zoals ExpressRoute), of tijdens een i/o-proces , dit wordt beschouwd als zijnde in beweging.

### <a name="encryption-at-rest"></a>Versleuteling 'at rest'

Als u wilt bereiken versleuteling-at-rest, doet u elk van de volgende:

Ondersteuning voor ten minste één van de aanbevolen versleuteling modellen in de volgende tabel worden beschreven om gegevens te versleutelen.

| Versleuteling modellen |  |  |  |
| ----------------  | ----------------- | ----------------- | --------------- |
| Server-versleuteling | Server-versleuteling | Server-versleuteling | Client-versleuteling
| Server-Side-versleuteling door de Service beheerde sleutels | Server-side-versleuteling met Customer-Managed sleutels in Azure Key Vault | Server-side-codering met on-premises klanten beheerde sleutels |
| • Azure Resourceproviders de versleuteling en ontsleuteling bewerkingen uitvoeren <br> • Microsoft beheert de sleutels <br>• Volledige cloudfunctionaliteit | • Azure Resourceproviders de versleuteling en ontsleuteling bewerkingen uitvoeren<br>• Klant Hiermee bepaalt u sleutels via Azure Key Vault<br>• Volledige cloudfunctionaliteit | • Azure Resourceproviders de versleuteling en ontsleuteling bewerkingen uitvoeren <br>• Klant Hiermee bepaalt u sleutels On-premises <br> • Volledige cloudfunctionaliteit| • Azure-services geen ontsleutelde gegevens zien. <br>• Klanten sleutels on-premises houden (of in andere beveiligde winkels). Sleutels zijn niet beschikbaar voor Azure-services <br>• Verminderd cloudfunctionaliteit|

### <a name="enabling-encryption-at-rest"></a>Inschakelen van versleuteling at-rest

**Uw gegevens opgeslagen van alle locaties te identificeren**

Het doel van versleuteling-at-Rest is om alle gegevens te versleutelen. In dat geval wordt voorkomen dat de mogelijkheid om belangrijke gegevens of alle persistente locaties ontbreekt. Het inventariseren van alle gegevens die zijn opgeslagen door uw toepassing. 

> [!Note] 
> Niet alleen 'toepassingsgegevens' of ' persoonlijke ', maar geen gegevens met betrekking tot de toepassing met inbegrip van metagegevens (abonnement toewijzingen van contract info, PII) account.

Houd rekening met welke winkels die u gebruikt voor het opslaan van gegevens. Bijvoorbeeld:

- Externe opslag (bijvoorbeeld SQL Azure, Document DB, HDInsights, Data Lake, enz.)

- Tijdelijke opslag (een lokale cache, waaronder gegevens van de tenant)

- In-memory-cache (in kan worden geplaatst het wisselbestand.)

### <a name="leverage-the-existing-encryption-at-rest-support-in-azure"></a>Maak gebruik van de bestaande versleuteling-at-rest-ondersteuning in Azure

Maak gebruik van de bestaande versleuteling-at-Rest-ondersteuning voor elke winkel die u gebruikt.

- Azure Storage: Zie [Azure Storage-Serviceversleuteling voor Data-at-Rest](https://docs.microsoft.com/azure/storage/storage-service-encryption),

- SQL Azure: Zie [Transparent Data Encryption (TDE), SQL Always Encrypted](https://msdn.microsoft.com/library/mt163865.aspx)

- VM- & lokale disk-opslag ([Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption))

Gebruik Azure Disk Encryption voor virtuele machine en lokale schijfopslag waar dit wordt ondersteund:

#### <a name="iaas"></a>IaaS

Services met IaaS-VM's (Windows of Linux) moeten gebruiken [Azure Disk Encryption](https://microsoft.sharepoint.com/teams/AzureSecurityCompliance/Security/SitePages/Azure%20Disk%20Encryption.aspx) voor het versleutelen van volumes met gegevens van de klant.

#### <a name="paas-v2"></a>PaaS-v2

Services die worden uitgevoerd op PaaS-v2 met behulp van Service Fabric kunt gebruiken Azure disk encryption voor virtuele-Machineschaalset [VMSS] voor het versleutelen van hun PaaS v2 VM's.

#### <a name="paas-v1"></a>PaaS v1

Azure Disk Encryption wordt momenteel niet ondersteund voor PaaS v1. Daarom moet u versleuteling van toepassing op persistente gegevens in rust te versleutelen.  Dit omvat, maar is niet beperkt tot toepassingsgegevens, tijdelijke bestanden, logboeken en crashdumps.

De meeste services moeten proberen gebruikmaken van de versleuteling van een storage resourceprovider. Sommige services moeten doen expliciete versleuteling, bijvoorbeeld een sleutelmateriaal persistent (certificaten, root / master sleutels) moeten worden opgeslagen in Key Vault.

Als u versleuteling op de service met de klant beheerde sleutels ondersteunt moeten er een manier voor de klant om op te halen van de sleutel aan ons. De ondersteunde en aanbevolen manier om dat te doen door te integreren met Azure Key Vault (AKV). In dit geval kunnen klanten toevoegen en beheren van hun sleutels in Azure Key Vault. Een klant kan informatie over het gebruik van Azure Sleutelkluis via [aan de slag met Key Vault](http://go.microsoft.com/fwlink/?linkid=521402).

Als u wilt integreren met Azure Key Vault, voegt u code voor het aanvragen van een sleutel uit AKV wanneer dat nodig is voor ontsleuteling.

- Zie [Azure Key Vault-stap voor stap](https://blogs.technet.microsoft.com/kv/2015/06/02/azure-key-vault-step-by-step/) voor informatie over het integreren met Azure Sleutelkluis.

Als u ondersteuning voor de klant beheerde sleutels, moet u voor een UX voor de klant om op te geven die Key Vault (of de URI van Key Vault) te gebruiken.

Als versleuteling-at-Rest de versleuteling van de host, infrastructuur en tenant-gegevens, het verlies van gegevens van de sleutels vanwege een systeemfout is opgetreden omvat of schadelijke activiteiten betekenen de versleutelde gegevens dat kan gaat verloren. Het is daarom essentieel dat de versleuteling-at-Rest-oplossing een uitgebreide disaster recovery verhaal tegen systeemfouten en schadelijke activiteiten heeft.

Services die versleuteling-at-Rest implementeren zijn doorgaans nog steeds gevoelig zijn voor de versleutelingssleutels of gegevens blijft niet-versleuteld op de host-station (bijvoorbeeld in het wisselbestand van het hostbesturingssysteem.) Services moeten daarom dat het hostvolume voor hun services worden versleuteld. Om deze Compute team heeft ingeschakeld voor de implementatie van de Host-versleuteling, die wordt gebruikt [Bitlocker](https://technet.microsoft.com/library/dn306081.aspx) NKP en -extensies op de DCM-service en de agent voor het versleutelen van het hostvolume.

De meeste services zijn geïmplementeerd op standaard Azure-VM's. Dergelijke services moeten krijgen [Host versleuteling](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) automatisch wanneer Compute ingeschakeld. Voor services die worden uitgevoerd in Compute beheerde clusters host versleuteling automatisch ingeschakeld als Windows Server 2016 wordt uitgerold.

### <a name="encryption-in-transit"></a>Versleuteling in-transit

Beveiligen van gegevens die onderweg zijn, moet essentieel onderdeel van uw strategie voor gegevensbescherming. Omdat gegevens heen en van veel locaties verplaatsen is, is de algemene aanbeveling dat u altijd SSL/TLS-protocollen voor het uitwisselen van gegevens op verschillende locaties. In sommige gevallen wilt u mogelijk het hele communicatiekanaal tussen uw on-premises en cloud isoleren infrastructuur met behulp van een virtueel particulier netwerk (VPN).

Voor gegevens verplaatsen tussen uw on-premises infrastructuur en Azure, moet u rekening houden met passende beveiligingsmaatregelen zoals HTTPS- of VPN.

Voor organisaties die nodig zijn voor het beveiligen van toegang vanaf meerdere werkstations die zich on-premises naar Azure, gebruikt u [Azure site-naar-site VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-site-to-site-create).

Voor organisaties die nodig zijn voor het beveiligen van toegang van een werkstation zich on-premises naar Azure, gebruikt u [punt-naar-Site VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-point-to-site-create).

Grotere gegevenssets kunnen worden verplaatst via een speciale snelle WAN-verbinding, zoals [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Als u gebruiken van ExpressRoute wilt, kunt u ook de gegevens op het niveau van de toepassing via versleutelen [SSL/TLS](https://support.microsoft.com/kb/257591) of andere protocollen voor extra beveiliging.

Als u met Azure Storage via de Azure Portal communiceert, worden alle transacties plaatsvinden via HTTPS. [REST API voor Storage](https://msdn.microsoft.com/library/azure/dd179355.aspx) via HTTPS kan ook worden gebruikt om te communiceren met [Azure Storage](https://azure.microsoft.com/services/storage/) en [Azure SQL Database](https://azure.microsoft.com/services/sql-database/).

Organisaties die niet voldoen aan het beveiligen van gegevens die onderweg zijn, zijn vatbaar voor [man-in-the-middle-aanvallen](https://technet.microsoft.com/library/gg195821.aspx), [niet kan worden afgeluisterd](https://technet.microsoft.com/library/gg195641.aspx), en sessiehijacking. Deze aanvallen mag de eerste stap bij het toegang krijgen tot vertrouwelijke gegevens.

U kunt meer informatie over Azure VPN-optie lezen van het artikel [Planning en ontwerp voor VPN-Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design).

### <a name="enforce-file-level-data-encryption"></a>Afdwingen op het niveau bestandsversleuteling

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) gebruikt beleidsregels voor versleuteling, identiteit en verificatie om uw bestanden en e-mailbericht te beveiligen. Azure RMS werkt op meerdere apparaten, telefoons, tablets en pc's door te beschermen, zowel binnen als buiten uw organisatie. Deze mogelijkheid is mogelijk omdat Azure RMS voegt een niveau van beveiliging die met de gegevens gekoppeld blijft zelfs wanneer deze grenzen van uw organisatie.

Wanneer u Azure RMS gebruiken om uw bestanden te beveiligen, gebruikt u industriestandaard cryptografie met volledige ondersteuning van [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). Wanneer u Azure RMS voor de beveiliging van gegevens gebruiken, hebt u de zekerheid dat de beveiliging blijft van toepassing op het bestand, zelfs als deze wordt gekopieerd naar de opslag die niet onder het beheer van IT, zoals een cloudopslagservice. Hetzelfde doet zich voor bestanden die worden gedeeld via e-mail, het bestand is beveiligd als bijlage aan een e-mailbericht met instructies voor hoe u de beveiligde bijlage te openen.
Bij het plannen van Azure RMS acceptatie raden we het volgende:

- Installeer de [RMS sharing-app](https://technet.microsoft.com/library/dn339006.aspx). Deze app kan worden geïntegreerd in Office-toepassingen door installatie van een Office-invoegtoepassing, zodat gebruikers kunnen bestanden eenvoudig rechtstreeks beveiligen.

- Configureren van toepassingen en services voor Azure RMS ondersteunen

- Maak [aangepaste sjablonen](https://technet.microsoft.com/library/dn642472.aspx) die overeenkomen met uw zakelijke vereisten. Bijvoorbeeld: een sjabloon voor bovenste geheime gegevens die moet worden toegepast op alle bovenste geheim gerelateerde e-mailberichten.

Organisaties die zwakke op [gegevensclassificatie](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) en bestandsbeveiliging mogelijk gevoeliger voor lekken van gegevens. Zonder de juiste Bestandsbeveiliging, organisaties niet mogelijk om te verkrijgen van zakelijke inzichten worden verkregen, misbruik kunt controleren en te voorkomen dat kwaadwillende toegang tot bestanden.

> [!Note]
> U kunt meer informatie over Azure RMS lezen van het artikel [aan de slag met Azure Rights Management](https://technet.microsoft.com/library/jj585016.aspx).

## <a name="secure-your-application-protect"></a>Beveiligen van uw toepassing (beveiligen)
Hoewel Azure verantwoordelijk is voor het beveiligen van de infrastructuur en het platform dat uw toepassing wordt uitgevoerd, is het uw verantwoordelijkheid voor het beveiligen van uw toepassing zelf. Met andere woorden, wilt u ontwikkelen, implementeren en beheren van uw toepassingscode en inhoud op een veilige manier. Zonder deze kunt uw toepassingscode of inhoud blijven mogelijk kwetsbaar voor aanvallen.

### <a name="web-application-firewall-waf"></a>Web Application Firewall (WAF)
[De Web application firewall (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) is een functie van [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) die gecentraliseerde beveiliging van uw webtoepassingen tegen algemene aanvallen en beveiligingsproblemen biedt.

Web Application Firewall is gebaseerd op regels uit de [Core Rule Set 3.0 of 2.2.9 van OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project). Webtoepassingen zijn in toenemende mate het doel van aanvallen die gebruikmaken van veelvoorkomende bekende beveiligingsproblemen. Veelvoorkomende aanvallen zijn hierbij onder andere aanvallen met SQL-injecties en aanvallen via scripting op meerdere sites. Het kan een hele uitdaging zijn om dergelijke aanvallen in toepassingscode te voorkomen en dit kan tevens veel onderhoud, patching en controle vereisen op meerdere lagen van de toepassingstopologie. Een gecentraliseerde firewall voor webtoepassingen maakt het beveiligingsbeheer veel eenvoudiger en biedt toepassingsbeheerders meer veiligheid tegen bedreigingen of aanvallen. Een WAF-oplossing kan ook sneller reageren op een beveiligingsrisico door een patch voor een bekend beveiligingsprobleem toe te passen op een centrale locatie in plaats van elke afzonderlijke webtoepassing te beveiligen. Bestaande toepassingsgateways kunnen eenvoudig worden geconverteerd naar een toepassingsgateway met Web Application Firewall.

Hieronder ziet u enkele voorbeelden van veelvoorkomende beveiligingsproblemen waartegen Web Application Firewall bescherming biedt:

- Beveiliging tegen SQL-injecties

- Beveiliging tegen scripting op meerdere sites

- Beveiliging tegen veelvoorkomende aanvallen via internet, zoals opdrachtinjectie, het smokkelen van HTTP-aanvragen, het uitsplitsen van HTTP-antwoorden en aanvallen waarbij een extern bestand wordt ingesloten

- Beveiliging tegen schendingen van het HTTP-protocol

- Beveiliging tegen afwijkingen van het HTTP-protocol, zoals een gebruikersagent voor de host en Accept-headers die ontbreken

- Beveiliging tegen bots, crawlers en scanners

- Detectie van veelvoorkomende onjuiste configuraties van toepassingen (dat wil zeggen, Apache, IIS, enz.)

> [!Note]
> Voor een gedetailleerde lijst van regels en waartegen ze beveiliging bieden, Zie de volgende [Core rule set](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview#core-rule-sets):

Azure biedt ook diverse functies voor eenvoudig te gebruiken als u wilt beveiligen, zowel binnenkomend als uitgaand verkeer voor uw app. Azure helpt klanten hun toepassingscode beveiligen doordat extern vindt u functionaliteit om het scannen van uw webtoepassing voor beveiligingsproblemen.

- [Azure Active Directory-verificatie voor uw app instellen](https://azure.microsoft.com/blog/azure-websites-authentication-authorization/)

- [Verkeer naar uw app beveiligen met het Transport Layer Security (TLS/SSL) - HTTPS inschakelen](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl)

  - [Al het binnenkomende verkeer via HTTPS-verbinding afdwingen](http://microsoftazurewebsitescheatsheet.info/)

  - [Strikte Transport Security (HSTS) inschakelen](http://microsoftazurewebsitescheatsheet.info/#enable-http-strict-transport-security-hsts)

- [Toegang tot uw app beperken door IP-adres van client](http://microsoftazurewebsitescheatsheet.info/#filtering-traffic-by-ip)

- [Toegang tot uw app beperken door het gedrag van client - aanvraag frequentie en gelijktijdigheid](http://microsoftazurewebsitescheatsheet.info/#dynamic-ip-restrictions)

- [Scan de code van uw web-app voor beveiligingsproblemen met behulp van Tinfoil Security Scanning](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/)

- [Wederzijdse TLS-verificatie om te vereisen dat clientcertificaten verbinding maken met uw web-app configureren](https://docs.microsoft.com/azure/app-service/app-service-web-configure-tls-mutual-auth)

- [Configureren van een certificaat voor gebruik van uw app veilig verbinding maken met externe bronnen](https://azure.microsoft.com/blog/using-certificates-in-azure-websites-applications/)

- [Headers van de standaard-server om te voorkomen dat de hulpprogramma's van uw app fingerprinting verwijderen](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/)

- [Uw app veilig verbinding te maken met resources in een particulier netwerk met behulp van punt-naar-Site-VPN](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet)

- [Uw app veilig verbinding te maken met resources in een particulier netwerk maken via hybride verbindingen](https://docs.microsoft.com/azure/app-service/app-service-hybrid-connections)

Azure App Service gebruikt dezelfde antimalwareoplossing uit die worden gebruikt door Azure Cloud Services en Virtual Machines. Voor meer informatie over deze verwijzen naar onze [anti-malware-documentatie](https://docs.microsoft.com/azure/security/azure-security-antimalware).

## <a name="secure-your-network-protect"></a>Beveilig uw netwerk (beveiligen)
Microsoft Azure omvat een robuuste netwerkinfrastructuur ter ondersteuning van uw toepassing en de vereisten voor service-connectiviteit. Verbinding met het netwerk mogelijk is tussen de resources die zich bevinden in Azure, tussen on-premises en wordt gehost op Azure-resources, en naar en van Internet en Azure.

De [Azure netwerkinfrastructuur](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-networking-guidelines) kunt u veilig verbinding maken met Azure-resources met elkaar met [virtuele netwerken (VNets)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview). Een VNet is een weergave van uw eigen netwerk in de cloud. Een VNet is een logische isolatie van het Azure-cloud-netwerk toegewezen aan uw abonnement. U kunt VNets verbinden met uw on-premises netwerken.

![Beveilig uw netwerk (beveiligen)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig6.png)

Als u eenvoudige niveau netwerktoegangsbeheer (op basis van IP-adres en de protocollen TCP of UDP), dan kunt u [Netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg). Een Netwerkbeveiligingsgroep (NSG) is een eenvoudige filterregels voor pakketten firewall en zorgt ervoor dat u voor het beheren van toegang op basis van een [5-tuple](https://www.techopedia.com/definition/28190/5-tuple).

Azure-netwerken ondersteunt de mogelijkheid om aan te passen van de routering-gedrag voor netwerkverkeer op uw virtuele netwerken van Azure. U kunt dit doen door het configureren van [door de gebruiker gedefinieerde Routes](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) in Azure.

[Geforceerde tunneling](https://www.petri.com/azure-forced-tunneling) is een mechanisme dat u gebruiken kunt om ervoor te zorgen dat uw services zijn niet toegestaan om een verbinding aan apparaten op Internet te zetten.

Azure ondersteunt toegewezen WAN-verbinding verbinding met uw on-premises netwerk en een Azure-netwerk met [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction). De koppeling tussen Azure en uw site maakt gebruik van een speciale verbinding die niet via het openbare Internet loopt. Als uw Azure-toepassing wordt uitgevoerd in meerdere datacenters, kunt u [Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) te routeren van aanvragen van gebruikers op intelligente wijze over exemplaren van de toepassing. Ook kunt u verkeer routeren naar services niet wordt uitgevoerd in Azure als ze via Internet toegankelijk zijn.

## <a name="virtual-machine-security-protect"></a>Beveiliging van virtuele machines (beveiligen)

[Virtuele Machines van Azure](https://docs.microsoft.com/azure/virtual-machines/) kunt u een brede reeks computeroplossingen op flexibele wijze inzetten. Met ondersteuning voor Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP en Azure BizTalk Services kunt u elke workload en elke taal implementeren op vrijwel elk besturingssysteem.

Met Azure, kunt u [antimalwaresoftware](https://docs.microsoft.com/azure/security/azure-security-antimalware) van leveranciers van beveiligingsoplossingen, zoals Microsoft, Symantec, Trend Micro en Kaspersky aan uw virtuele machines beschermen tegen schadelijke bestanden, adware en andere dreigingen.

Microsoft Antimalware voor Azure Cloud Services en virtuele Machines is een realtime-beveiliging-functie waarmee u kunt herkennen en verwijderen van virussen, spyware en andere schadelijke software. Microsoft Antimalware biedt configureerbare meldingen wanneer bekende schadelijke of ongewenste software probeert zichzelf te installeren of uitvoeren op uw Azure-systemen.

[Azure Backup](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) is een schaalbare oplossing die uw toepassingsgegevens met zonder enige kapitaalinvestering en minimale beschermt bedrijfskosten. Toepassingsfouten kunnen uw gegevens beschadigen, en menselijke fouten kunnen fouten introduceren in uw toepassingen. Met Azure Backup, worden uw virtuele machines met Windows en Linux beveiligd.

[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) kunt indelen replicatie, failover en herstel van workloads en apps, zodat ze beschikbaar vanaf een secundaire locatie zijn als uw primaire locatie uitvalt.

## <a name="ensure-compliance-cloud-services-due-diligence-checklist-protect"></a>Naleving: Cloud services vanwege de zorgvuldigheid Controlelijst (beveiligen)

Microsoft ontwikkelde [de controlelijst voor Cloud Services de zorgvuldigheid vervaldatum](https://aka.ms/cloudchecklist.download) waarmee organisaties kunnen uitoefenen vanwege de zorgvuldigheid zoals overgang naar de cloud overweegt. Het biedt een structuur voor een organisatie met een grootte en hetzelfde type, particuliere bedrijven en organisaties in de publieke sector, inclusief overheid op alle niveaus en non-profitorganisaties — om hun eigen prestaties, service, gegevensbeheer en governance doelstellingen te identificeren en vereisten. Hierdoor kunnen ze de aanbiedingen van andere cloudserviceproviders, die uiteindelijk vormen de basis voor een cloud-serviceovereenkomst vergelijken.

De controlelijst biedt een framework dat component door component met een nieuwe internationale norm voor cloud-serviceovereenkomsten, ISO/IEC 19086 uitgelijnd. Deze standaard biedt een uniforme reeks overwegingen voor organisaties beslissingen over de acceptatie van cloudcomputing, en maakt een gemeenschappelijke volledig is ontwikkeld voor het vergelijken van cloud service-aanbiedingen.

De controlelijst bevordert een grondig gerenommeerde verplaatsen naar de cloud, biedt gestructureerde richtlijnen en een consistente en herhaalbare aanpak voor het kiezen van een cloudserviceprovider.

Acceptatie van cloudcomputing is niet langer gewoon een beslissing technologie. Omdat controlelijst vereisten touch op elk aspect van een organisatie, ze te organiseren van alle belangrijke interne-besluitvormers fungeren, de CIO en CISO, evenals informatie over wetgeving, risico management, inkoop en naleving-professionals. Dit verhoogt de efficiëntie van de besluitvorming en besluiten die volledig is ontwikkeld in geluid redenering, waardoor de kans op onvoorziene obstakels aan de transformatie.

Daarnaast kunnen de Controlelijst:

- Beschrijft sleutel discussieonderwerpen voor besluitvormers aan het begin van het overstapproces voor de cloud.

- Ondersteunt uitgebreide zakelijke discussies over regelgeving en de doelstellingen van organisatie voor privacy, persoonlijk identificeerbare informatie (PII) en beveiliging van gegevens.

- Helpt organisaties bij het identificeren van mogelijke problemen die invloed kunnen zijn op een cloudproject.

- Biedt een consistente set vragen, met de dezelfde voorwaarden, definities, metrische gegevens en producten voor elke provider, voor het vereenvoudigen van het proces van het vergelijken van aanbiedingen van andere cloudserviceproviders.

## <a name="azure-infrastructure-and-application-security-validation-detect"></a>Azure-infrastructuur en toepassing beveiligingsvalidatie (detecteren)

[Azure Operational Security](https://docs.microsoft.com/azure/security/azure-operational-security) verwijst naar de services, besturingselementen en functies die beschikbaar zijn voor gebruikers voor het beveiligen van hun gegevens, toepassingen en andere items in Microsoft Azure.

![Beveiligingsvalidatie (detecteren)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig7.png)

Operationele beveiliging in Azure is gebouwd op een framework waarin de kennis opgedaan uit een diverse mogelijkheden die uniek voor Microsoft zijn, met inbegrip van de Microsoft Security Development Lifecycle (SDL), het programma Microsoft Security Response Center , en een diep besef van het landschap van cyberveiligheidsbedreigingen.

### <a name="microsoft-operations-management-suiteoms"></a>Microsoft operations management Suite (OMS)

[Microsoft Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) is de IT-beheeroplossing voor de hybride cloud. Alleen wordt gebruikt of om uit te breiden van uw bestaande System Center-implementatie, OMS, zodat u de maximale flexibiliteit en controle voor cloud-gebaseerd beheer van uw infrastructuur.

![Microsoft operations management Suite (OMS)](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig8.png)

Met OMS, kunt u een instantie in elke cloud, met inbegrip van on-premises, Azure, AWS, Windows Server, Linux, VMware en OpenStack, tegen lagere kosten dan concurrerende, door oplossingen te beheren. OMS is ontwikkeld voor de cloudgeoriënteerde wereld en biedt een nieuwe benadering voor het beheren van uw onderneming dat wil zeggen de snelste en meest rendabele manier om te voldoen aan de nieuwe zakelijke uitdagingen en geschikt voor de nieuwe werkbelastingen, toepassingen en cloudomgevingen.

### <a name="log-analytics"></a>Log Analytics

[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics) biedt bewakingsservices voor OMS door in een centrale opslagplaats gegevens te verzamelen van beheerde resources. Deze gegevens kunnen gebeurtenissen, prestatiegegevens en aangepaste gegevens omvatten die via de API worden geleverd. Na verzameling zijn de gegevens beschikbaar voor waarschuwingen, analyse en export.

![Log Analytics](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig9.png)

Met deze methode kunt u gegevens uit een groot aantal bronnen consolideren, zodat u kunt combineren gegevens uit uw Azure-services met uw bestaande on-premises omgeving. De methode maakt ook een duidelijk onderscheid tussen het verzamelen van gegevens en het bewerken hiervan. Zo zijn alle bewerkingen beschikbaar voor alle soorten gegevens.

### <a name="azure-security-center"></a>Azure Security Center

[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) helpt u bij het detecteren, voorkomen van en reageren op bedreigingen dankzij een verhoogde zichtbaarheid van en controle over de beveiliging van uw Azure-resources. Het biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor uw Azure-abonnementen, helpt bedreigingen te detecteren die anders onopgemerkt zouden blijven, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen.

Security Center analyseert de beveiligingsstatus van uw Azure-resources om mogelijke beveiligingsproblemen op te sporen. Een lijst met aanbevelingen begeleidt u bij het configureren van benodigde besturingselementen.

Voorbeelden zijn:

- Inrichting van antimalware om schadelijke software te identificeren en te verwijderen

- Configuratie van netwerkbeveiligingsgroepen en regels voor het verkeer naar virtuele machines beheren

- Inrichten van Web Application Firewalls om te beschermen tegen aanvallen die zijn gericht op uw webtoepassingen

- Implementatie van ontbrekende systeemupdates

- Aanpassing van besturingssysteemconfiguraties die niet overeenkomen met de aanbevolen basislijnen

Security Center verzamelt, analyseert en integreert automatisch logboekgegevens van uw Azure-resources, het netwerk en partneroplossingen zoals antimalwareprogramma's en firewalls. Wanneer er dreigingen worden gedetecteerd, wordt een beveiligingswaarschuwing gemaakt. Voorbeelden zijn detectie van:

- Geïnfecteerde VM's die communiceren met bekende schadelijke IP-adressen

- Geavanceerde malware die is gedetecteerd met Windows Foutrapportage

- Beveiligingsaanvallen op virtuele machines

- Beveiligingswaarschuwingen van geïntegreerde antimalwareprogramma's en firewalls

### <a name="azure-monitor"></a>Met Azure monitor

[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) bevat verwijzingen naar informatie over specifieke typen resources. Het biedt visualisatie, query, routering, waarschuwingen, automatisch schalen en automatisering op gegevens zowel in de Azure-infrastructuur (activiteitenlogboek) en elke afzonderlijke Azure-resource (diagnostische logboeken).

Cloud-Apps zijn complexe met veel bewegende onderdelen bevatten. Monitoring biedt gegevens om ervoor te zorgen dat uw toepassing actief en wordt uitgevoerd in een foutloze toestand bevindt. Ook kunt u potentiële problemen voorkomen of oplossen van het verleden zijn.

![Azure Monitor](media/azure-security-technical-capabilities/azure-security-technical-capabilities-fig10.png) Bovendien kunt u bewakingsgegevens diep om inzicht te krijgen over uw toepassing. Deze kennis kan u helpen te verbeteren van de prestaties van de toepassing of onderhoud, of Automatiseer acties die anders handmatig worden opgelost moeten zouden.

Controle van de netwerkbeveiliging van uw is het essentieel is voor het netwerk beveiligingsproblemen detecteren en ervoor te zorgen dat voldoet aan uw IT-beveiliging- en regelgeving governance-model. Met de weergave van de beveiligingsgroep, kunt u de geconfigureerde Netwerkbeveiligingsgroep en beveiligingsregels voor verbindingen, evenals de effectieve beveiligingsregels ophalen. Met de lijst met regels die worden toegepast, kunt u bepalen de poorten die geopend zijn en ss beveiligingslek met betrekking tot het netwerk.

### <a name="network-watcher"></a>Netwerk-watcher

[Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher) is een regionale service waarmee u kunt bewaken en diagnosticeren op het niveau van een netwerk in, naar en van Azure. Diagnose en visualisatie hulpprogramma's die beschikbaar zijn met Network Watcher kunnen u begrijpen, diagnosticeren en inzicht verkrijgen in uw netwerk in Azure. Deze service omvat pakket vastleggen, volgende hop, IP-stroom controleren, weergave van de beveiligingsgroep, NSG-stroomlogboeken. Scenario niveau bewaking biedt een end-to-weergave van netwerkbronnen in tegenstelling tot afzonderlijke resource netwerkbewaking.

### <a name="storage-analytics"></a>Opslaganalyses

[Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) kunt metrische gegevens opslaan die samengevoegde transactiestatistieken en capaciteitsgegevens gegevens over aanvragen voor een opslagservice. Transacties worden gerapporteerd op zowel de API bewerking-niveau, evenals op het niveau van de service opslag en capaciteit wordt gerapporteerd op het niveau van de storage-service. Metrische gegevens kan worden gebruikt storage service-gebruik analyseren, problemen diagnosticeren met aanvragen voor de storage-service en voor het verbeteren van de prestaties van toepassingen die gebruikmaken van een service.

### <a name="application-insights"></a>Application Insights

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) is een uitbreidbare service voor Application Performance Management (APM) voor webontwikkelaars op meerdere platforms. Hiermee kunt u uw livewebtoepassing controleren. Afwijkende prestaties worden automatisch gedetecteerd. Het bevat krachtige analysehulpmiddelen om te helpen u problemen identificeren en te begrijpen wat gebruikers doen met uw app. Het is bedoeld om u te helpen de prestaties en bruikbaarheid continu te verbeteren. Het werkt voor apps op uiteenlopende platforms, waaronder .NET, Node.js en J2EE, on-premises of in de cloud gehost. Het kan worden geïntegreerd met uw devOps-proces en bevat verbindingspunten naar een verschillende hulpprogramma's voor ontwikkeling.

Met deze service kunt u het volgende controleren:

- **Aantal aanvragen, reactietijden en foutpercentages** - ga na welke pagina's het populairst zijn op welke tijdstippen van de dag en waar uw gebruikers zich bevinden. Ontdek welke pagina's het beste presteren. Als uw reactietijden en foutpercentages omhoog gaan wanneer er meer aanvragen binnenkomen, hebt u mogelijk te weinig resources.

- **Aantal afhankelijkheidsrelaties, reactietijden en foutpercentages** - controleer of externe services zorgen voor vertraging.

- **Uitzonderingen** - Analyseer de cumulatieve statistische gegevens, of Kies specifieke gegevens en Zoom in op de stack-trace en verwante aanvragen. Zowel server- als browseruitzonderingen worden gerapporteerd.

- **Paginaweergaven en de prestaties bij het laden van pagina’s** - deze gegevens worden gerapporteerd door de browsers van uw gebruikers.

- **AJAX-aanroepen van webpagina's** -tarieven, reactietijden en foutpercentages.

- **Gebruikers- en sessieaantallen.**

- **Prestatiemeteritems** van uw Windows- of Linux-servers, zoals die voor CPU-, geheugen- en netwerkgebruik.

- **Diagnostische gegevens van hosts** van Docker of Azure.

- **Diagnostische traceerlogboeken** van uw app - met behulp hiervan kunt u de samenhang vaststellen tussen traceergebeurtenissen en aanvragen.

- **Aangepaste gebeurtenissen en metrische gegevens** u zelf schrijft in de code-client of server u zakelijke gebeurtenissen bijhouden zoals verkochte artikelen of gewonnen spellen.

De infrastructuur voor uw toepassing bestaat meestal uit veel onderdelen, zoals een virtuele machine, een opslagaccount en een virtueel netwerk, of een webtoepassing, database, databaseserver en services van derden. Deze onderdelen moet u niet zien als afzonderlijke entiteiten, maar als onderdelen die één entiteit vormen en aan elkaar zijn gerelateerd en afhankelijk zijn van elkaar. U implementeert, beheert en bewaakt deze onderdelen als groep. [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) kunt u werken met de resources in uw oplossing als een groep.

U kunt alle resources voor uw oplossing implementeren, bijwerken of verwijderen in een enkele, gecoördineerde bewerking. Voor implementatie gebruikt u een sjabloon. Deze sjabloon kan voor verschillende omgevingen worden gebruikt, zoals testen, faseren en productie. Resource Manager biedt beveiliging, controle en tagfuncties die u na de implementatie helpen bij het beheren van uw resources.

**De voordelen van het gebruik van Resource Manager**

Resource Manager biedt diverse voordelen:

- U kunt alle resources voor uw oplossing implementeren, beheren en bewaken als groep, in plaats van deze resources afzonderlijk te verwerken.

- U kunt gedurende de ontwikkelingscyclus uw oplossing herhaaldelijk implementeren en erop vertrouwen dat uw resources consistent worden geïmplementeerd.

- U kunt uw infrastructuur beheren via declaratieve sjablonen in plaats van scripts.

- U kunt de afhankelijkheden tussen resources, zodat ze zijn geïmplementeerd in de juiste volgorde definiëren.

- U kunt toegangsbeheer toepassen op alle services in de resourcegroep omdat op rollen gebaseerd toegangsbeheer (RBAC) is geïntegreerd in het beheerplatform.

- U kunt tags toepassen op de resources om alle resources in uw abonnement op een logische manier te organiseren.

- U kunt de facturering van uw organisatie transparanter maken door te kijken naar de kosten voor een groep resources met dezelfde tag.

> [!Note]
> Resource Manager biedt een nieuwe manier om uw oplossingen te implementeren en te beheren. Als u de eerdere implementatiemodel en wilt gebruikt voor meer informatie over de wijzigingen, Zie [Understanding Resource Manager-implementatie en klassieke implementatie](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over beveiliging vindt u enkele van de onderwerpen over onze uitgebreide beveiliging:

- [Controle en logboekregistratie](https://www.microsoft.com/en-us/trustcenter/security/auditingandlogging)

- [Cybercrime](https://www.microsoft.com/en-us/trustcenter/security/cybercrime)

- [Ontwerp en operationele beveiliging](https://www.microsoft.com/en-us/trustcenter/security/designopsecurity)

- [Versleuteling](https://www.microsoft.com/en-us/trustcenter/security/encryption)

- [Identiteits-en toegang](https://www.microsoft.com/en-us/trustcenter/security/identity)

- [Netwerkbeveiliging](https://www.microsoft.com/en-us/trustcenter/security/networksecurity)

- [Beveiligingsbeheer](https://www.microsoft.com/en-us/trustcenter/security/threatmanagement)
