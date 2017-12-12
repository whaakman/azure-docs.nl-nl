---
title: Azure Identity begrijpen | Microsoft Docs
description: Haal een basiskennis van Microsoft Azure identiteit oplossing voorwaarden, concepten en aanbevelingen voor u het beste identiteit governance beslissing neemt voor uw organisatie.
keywords: 
author: jeffgilb
manager: mtillman
ms.reviewer: jsnow
ms.author: jeffgilb
ms.date: 7/17/2017
ms.topic: article
ms.prod: 
ms.service: azure
ms.technology: 
ms.assetid: 
ms.custom: it-pro
ms.openlocfilehash: 4438917db93c37ddbba3e7ee692b2e3c065d2beb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="understand-azure-identity-solutions"></a>Azure-identiteitsoplossingen begrijpen
Microsoft Azure Active Directory (Azure AD) is een identiteits- en toegangsbeheer cloud beheeroplossing die directoryservices en identiteit governance Toepassingsbeheer toegang biedt. Azure AD snel [kunt van eenmalige aanmelding (SSO)](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-sso) naar 1. 000 de met vooraf geïntegreerde commerciële en aangepaste apps in de [Azure AD-toepassingsgalerie](https://azure.microsoft.com/marketplace/active-directory/all/). Veel van deze apps die u waarschijnlijk al, zoals Office 365, Salesforce.com vak, ServiceNow en Workday gebruikt.

Eén Azure AD-directory is automatisch gekoppeld aan een Azure-abonnement wanneer deze wordt gemaakt. Als de identity-service in Azure biedt Azure AD vervolgens alle identiteits- en controlefuncties voor toegang voor cloudresources. Deze resources kunnen gebruikers, apps en groepen voor een afzonderlijke tenant (organisatie) bevatten zoals weergegeven in het volgende diagram:

![Azure Active Directory](./media/understand-azure-identity-solutions/azure-ad.png)

Microsoft Azure biedt verschillende manieren gebruikmaken van de identiteit als een service (IDaaS) met verschillende bevoegdheidsniveaus complexiteit om te voldoen aan de behoeften van uw afzonderlijke organisatie. De rest van dit artikel helpt u begrijpen basic Azure identity-terminologie en -concepten, evenals aanbevelingen voor het maken van de beste keuze uit de beschikbare opties.

## <a name="terms-to-know"></a>Voorwaarden weten

Voordat u op een Azure identity-oplossing voor uw organisatie kunt, moet u een basiskennis van de voorwaarden die meestal wordt gebruikt bij het bespreken van de identiteit van de Azure-services.

|Term weten| Beschrijving|
|-----|-----|
|Azure-abonnement |Abonnementen worden gebruikt om te betalen voor Azure-cloudservices en zijn meestal gekoppeld aan een creditcard. U kunt meerdere abonnementen hebt, maar het kan lastig zijn om resources tussen abonnementen te delen.|
|Azure-tenant | Een Azure AD-tenant is representatief is voor één organisatie. Er is een toegewijde, vertrouwde instantie van Azure AD dat automatisch wordt gemaakt wanneer een organisatie zich registreert voor een Microsoft cloud service-abonnement zoals Azure, Intune of Office 365. Tenants kunnen toegang krijgen tot services in een specifieke omgeving (één tenant) of in een gedeelde omgeving met andere organisaties (multitenant).|
|Azure AD-map | Elke Azure-tenant is een toegewijde, vertrouwde Azure AD-adreslijst met gebruikers, groepen en toepassingen van de tenant. Deze wordt gebruikt voor het uitvoeren van de identiteit en toegang tot beheerfuncties voor tenantbronnen. Omdat een unieke Azure AD-directory wordt automatisch ingericht voor uw organisatie te vertegenwoordigen wanneer u zich aanmeldt voor een Microsoft-cloudservice zoals Azure, Microsoft Intune of Office 365, soms ziet u de voorwaarden *tenant*, *Azure AD*, en *Azure AD-directory* door elkaar gebruikt. |
|Aangepast domein | Wanneer u eerst zich aanmeldt voor een Microsoft cloud service-abonnement, uw tenant (organisatie) gebruikt een *. onmicrosoft.com* domeinnaam. Echter, de meeste organisaties hebben een of meer domeinnamen die worden gebruikt voor het bedrijf en die eindgebruikers gebruiken voor toegang tot bedrijfsbronnen. U kunt uw aangepaste domeinnaam toevoegen aan Azure AD, zodat de domeinnaam bekend aan uw gebruikers, zoals is  *alice@contoso.com*  in plaats van  *alice@contoso.onmicrosoft.com* . |
|Azure AD-account | Dit zijn de identiteiten die zijn gemaakt met behulp van Azure AD of een andere Microsoft-cloudservice, zoals Office 365. Ze zijn opgeslagen in Azure AD en toegankelijk is voor een van de organisatie cloud service-abonnementen. |
|Azure-abonnementbeheerder| De accountbeheerder is de persoon die zich heeft aangemeld of het Azure-abonnement hebt aangeschaft. Ze kunnen gebruiken de [Accountcentrum](https://account.azure.com/Subscriptions) voor het uitvoeren van verschillende beheertaken, zoals het maken van abonnementen abonnementen annuleren, de facturering voor een abonnement te wijzigen of wijzigen van de servicebeheerder. |
|Globale beheerder van Azure AD | Azure AD met globale beheerders hebben volledige toegang tot alle beheerfuncties van Azure AD. De persoon die zich voor een Microsoft cloud service-abonnement automatisch aanmeldt wordt een globale beheerder standaard. U kunt meer dan één globale beheerder hebben, maar alleen globale beheerders kunnen toewijzen van [andere beheerdersrollen](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) voor gebruikers. |
|Microsoft-account | Microsoft-accounts (gemaakt door u voor persoonlijk gebruik) bieden toegang tot Microsoft-producten voor consumenten gerichte en in de cloud-services, zoals Outlook (Hotmail), OneDrive, Xbox LIVE of Office 365. Deze identiteiten worden gemaakt en opgeslagen in het Microsoft-account klantidentiteitssysteem uitgevoerd door Microsoft.|
|Werk-of schoolaccounts | Werk- of schoolaccount accounts (uitgegeven door een beheerder voor gebruik van zakelijke/academic) bieden toegang tot enterprise zakelijke Microsoft cloudservices, zoals Azure, Intune of Office 365.|


## <a name="concepts-to-understand"></a>Concepten om te begrijpen

Nu dat u de voorwaarden basic Azure identity weet, moet u meer informatie over deze Azure identity-concepten die u helpen een gefundeerde Azure identity-service-beslissing nemen.

|Concept om te begrijpen |Beschrijving|
|-----|-----|
|[Hoe Azure-abonnementen worden gekoppeld aan Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-how-subscriptions-associated-directory) |Elke Azure-abonnement heeft een vertrouwensrelatie met een Azure AD-directory voor het verifiëren van gebruikers, services en apparaten. *Meerdere abonnementen kunnen dezelfde Azure AD-directory vertrouwen, maar een abonnement vertrouwt slechts één Azure AD-directory*. Er is een vertrouwensrelatie met dit in tegenstelling tot de relatie die een abonnement heeft met andere Azure-resources (websites, databases, enzovoort), die meer op onderliggende resources van een abonnement lijken. Als u een abonnement is verlopen, klikt u vervolgens toegang tot resources die zijn gekoppeld aan het abonnement, met uitzondering van Azure AD ook geblokkeerd. De Azure AD-directory blijft echter in Azure, zodat u kunt een ander abonnement aan die directory koppelen en doorgaan om tenant resources te beheren.|
|[Hoe Azure AD werkt licentieverlening](https://docs.microsoft.com/azure/active-directory/active-directory-licensing-get-started-azure-portal) | Wanneer u kopen of Enterprise Mobility Suite, Azure AD Premium of Azure AD Basic activeren, wordt uw directory bijgewerkt met het abonnement, met inbegrip van de geldigheidsperiode en vooruitbetaalde licenties. Zodra het abonnement actief is, kunt u de service worden beheerd door globale beheerders van Azure AD en gebruikt door gebruikers met een licentie. Uw abonnementsgegevens, waaronder het aantal licenties toegewezen of beschikbaar is, is beschikbaar in de Azure-portal van de **Azure Active Directory** > **licenties** blade. Dit is ook de beste plaats voor het beheren van uw licentietoewijzingen.|
|[Toegangsbeheer op basis van rollen in de Azure-portal](https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is)|Azure op rollen gebaseerde toegangsbeheer (RBAC) biedt Geavanceerd toegangsbeheer voor Azure-resources. Te veel machtigingen kunnen blootstellen account en een kwaadwillende personen. Te weinig machtigingen betekent dat werknemers hun werk efficiënt kunnen niet ophalen. Met RBAC kunt u kunt bieden werknemers de exacte machtigingen ze nodig hebben op basis van drie elementaire functies die van toepassing op alle brongroepen: eigenaar, bijdrager, reader. U kunt ook maken met maximaal 2.000 van uw eigen [aangepaste RBAC-rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles) om te voldoen aan uw specifieke behoeften. |
|[Hybride identiteit](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect)|Hybride identiteit wordt bereikt door uw lokale Windows Server Active Directory (AD DS) te integreren met Azure AD via [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect). Hiermee kunt u een algemene identiteit bieden voor uw gebruikers voor Office 365, Azure, en lokale apps of SaaS-toepassingen die zijn geïntegreerd met Azure AD. Met hybride identiteit, moet u effectief uw on-premises omgeving naar de cloud voor identiteits- en toegangsbeheer uitbreiden.|

### <a name="the-difference-between-windows-server-ad-ds-and-azure-ad"></a>Het verschil tussen Windows Server AD DS en Azure AD
Zowel de Azure Active Directory (Azure AD) en de lokale Active Directory (Active Directory Domain Services of AD DS) zijn systemen die Active directory-gegevens opslaan en beheren van communicatie tussen gebruikers en bronnen, met inbegrip van gebruikersprocessen aanmelden, verificatie en zoekacties in Active directory.

Als u al bekend bent met lokale Windows Server Active Directory Domain Services (AD DS), het eerst geïntroduceerd in Windows 2000 Server en vervolgens u waarschijnlijk de basisprincipes van een identity-service kent. Het is echter ook belangrijk te weten dat Azure AD niet een domeincontroller in de cloud is. Het is een geheel nieuwe manier van het bieden van identiteit als een service (IDaaS) in Azure die u moet een geheel nieuwe manier om gegevensclassificatie volledig Profiteer van cloud-gebaseerde mogelijkheden en uw organisatie te beschermen tegen moderne bedreigingen. 

AD DS is een serverfunctie in Windows Server, wat betekent dat deze kan worden geïmplementeerd op fysieke of virtuele machines. Er is een hiërarchische structuur op basis van X.500. DNS wordt gebruikt voor het zoeken naar objecten, kunnen worden gecommuniceerd met het gebruik van LDAP en voornamelijk gebruikt Kerberos voor authenticatie. Active Directory organisatie-eenheden (OE's) en groepsbeleidsobjecten (GPO's) kunnen naast machines toevoegen aan het domein en vertrouwensrelaties tussen domeinen worden gemaakt.

IT hun perimeter beveiliging voor het jaar met AD DS, maar moderne, perimeter minder ondernemingen ondersteunende identiteiten voor werknemers, klanten en partners, een nieuw besturingselement vlak moet heeft beveiligd. Azure AD is dit vlak identity-besturingselement. Beveiliging buiten de firewall van het bedrijf is verplaatst naar de cloud waar Azure AD bedrijfsbronnen en -toegang beveiligt door een algemene identiteit bieden voor gebruikers (on-premises of in de cloud). Hierdoor hebben uw gebruikers de flexibiliteit om veilige toegang tot de apps die ze nodig hebben voor hun werk vanaf vrijwel elk apparaat. Naadloze risico gebaseerde beveiliging Gegevensbesturingselementen, ondersteund door de mogelijkheden van machine learning en gedetailleerde rapporten, zijn ook op voorwaarde dat IT-behoeften te houden van bedrijfsgegevens beveiligen.

Azure AD is een klant meerdere openbare adreslijstservice, wat betekent dat een tenant voor uw cloud-servers en toepassingen zoals Office 365 in Azure AD kunt maken. Gebruikers en groepen worden gemaakt in een platte structuur zonder OE's of GPO's. Verificatie wordt uitgevoerd via protocollen, zoals SAML, WS-Federation en OAuth. Het is mogelijk query uitvoeren op Azure AD, maar in plaats van LDAP moet u een REST-API aangeroepen AD Graph API. Deze werkt met alle via HTTP en HTTPS.

### <a name="extend-office-365-management-and-security-capabilities"></a>Mogelijkheden voor het beheer en beveiliging van Office 365 uitbreiden
Al gebruikmaakt van Office 365? U kunt uw digitale transformatie versnellen door ingebouwde Office 365-mogelijkheden met Azure AD voor het beveiligen van uw resources worden uitgebreid beveiligde productiviteit inschakelen voor uw hele werknemers. Wanneer u Azure AD, naast de mogelijkheden voor Office 365, kunt u de volledige toepassing portfolio met één identiteit waarmee eenmalige aanmelding voor alle apps kunt beveiligen. U kunt de mogelijkheden van de voorwaardelijke toegang op basis niet alleen op de locatie van de status, maar de gebruiker, apparaat, toepassing en risico ook uitbreiden. Multi-factor authentication (MFA) mogelijkheden bieden nog meer beveiliging wanneer u deze nodig. U moet extra toezicht van de gebruiker heeft bevoegdheden krijgen en geef beheerderstoegang op aanvraag, just in time. Uw gebruikers productiever en maken minder helpdesk-tickets, dankzij de selfservice mogelijkheden die Azure AD levert zoals opnieuw instellen van vergeten wachtwoorden, toegangsaanvragen van toepassingen, en groepen maken en beheren.

> [!TIP]
> Wilt u meer informatie over het gebruik van Azure AD-identiteitsbeheer met Office 365? [Ophalen van de e-book](https://info.microsoft.com/Extend-Office-365-security-with-EMS.html).

## <a name="microsoft-azure-identity-solutions"></a>Microsoft Azure-identiteitsoplossingen

Microsoft Azure biedt verschillende manieren voor het beheren van uw gebruikers-id's of ze volledig worden beheerd op locatie, alleen in de cloud of zelfs ergens in tussen. Deze opties zijn onder andere: doe (DIY) AD DS in Azure, Azure Active Directory (Azure AD), hybride identiteit en Azure AD Domain Services.

### <a name="do-it-yourself-diy-ad-ds"></a>Doe (ZELFOPLOSSING) AD DS
Voor bedrijven die slechts een kleine footprint in de cloud moeten **Doe (DIY) AD DS** kan worden gebruikt in Azure. Deze optie ondersteunt vele Windows Server AD DS-scenario's die geschikt zijn voor de implementatie van virtuele machines (VM's) in Azure zijn. U kunt bijvoorbeeld een virtuele machine in Azure maken als een domeincontroller in een veel datacenter die is verbonden met het externe netwerk. Van daaruit zou de virtuele machine kunnen verificatieaanvragen van externe gebruikers ondersteunen en prestaties van het verificatieproces verbeteren. Deze optie is ook geschikt als vervanging voor anders kostbare noodherstelsites relatief lage kosten die als host fungeert voor een klein aantal domeincontrollers en één virtueel netwerk in Azure. Tot slot moet u mogelijk een toepassing in Azure, zoals SharePoint, die Windows Server AD DS vereist, maar heeft geen afhankelijkheid van de on-premises netwerk of de zakelijke Windows Server Active Directory implementeren. In dit geval kan u een geïsoleerde forest in Azure te voldoen aan vereisten voor de SharePoint-serverfarm implementeren. Dit wordt ook ondersteund voor het implementeren van netwerktoepassingen die verbinding met de on-premises netwerk en de lokale Active Directory vereisen.

### <a name="azure-active-directory-azure-ad"></a>Azure Active Directory (Azure AD)
**Azure AD-zelfstandige** is een volledig cloud-gebaseerde identiteit en toegang te beheren als een Service (IDaaS). Azure AD biedt u een set krachtige mogelijkheden voor het beheer van gebruikers en groepen. U kunt hiermee de toegang tot on-premises en cloudtoepassingen beveiligen, waaronder webservices van Microsoft, zoals Office 365 en een groot aantal SaaS-toepassingen (Software as a Service) van andere leveranciers dan Microsoft. Azure AD wordt geleverd in drie versies: gratis, basis- en Premium. Azure AD verhoogt de efficiëntie en breidt beveiliging buiten de firewall naar een nieuw besturingselement vlak die zijn beveiligd door Azure machine learning en andere geavanceerde beveiligingsfuncties.

### <a name="hybrid-identity"></a>Hybride identiteit
In plaats daarvan dan kiezen tussen on-premises of cloud-gebaseerde identiteitsoplossingen, veel forward denken CIO's en bedrijven die anticiperen op lange termijn richting van hun bedrijf begonnen, uitbreidt hun on-premises mappen naar de cloud via **hybride identiteit** oplossingen. Met hybride identiteit, krijgt u een documentatieklanten werken wereldwijd, identiteit en toegang beheeroplossing die veilig en productief toegang verleent aan de gebruikers toepassingen nodig om hun werk te doen.

> [!TIP]
> Voor meer informatie over hoe CIO's hebben gemaakt Azure Active Directory een centrale onderdeel van hun IT-strategieën, downloaden de [van CIO-handleiding voor Azure Active Directory](https://aka.ms/AzureADCIOGuide).

### <a name="azure-ad-domain-services"></a>Azure AD Domain Services
**Azure AD Domain Services** biedt een optie cloud-gebaseerde AD DS voor lichte Configuratiebeheer voor virtuele machine van Azure en een manier om te voldoen aan vereisten van de lokale identiteit voor toepassingsontwikkeling voor netwerk- en testdoeleinden gebruiken. Azure AD Domain Services is niet bedoeld om lift- en verplaatsen van uw on-premises AD DS-infrastructuur voor het virtuele Azure-machines worden beheerd door Azure AD Domain Services. In plaats daarvan moet de Azure VM's in de beheerde domeinen worden gebruikt ter ondersteuning van de ontwikkeling, testen en verkeer van on-premises toepassingen die AD DS-verificatiemethoden naar de cloud vereisen.

## <a name="common-scenarios-and-recommendations"></a>Algemene scenario's en aanbevelingen

Hier volgen enkele algemene identiteit en toegang scenario's met betrekking tot welke Azure identiteitsoptie mogelijk meest geschikt is voor elk aanbevelingen.

|Identiteitsscenario| Aanbeveling|
|-----|-----|
|Mijn organisatie grote investeringen in de lokale Windows Server Active Directory heeft gemaakt, maar we willen uitbreiden van de identiteit van de cloud.| De meest gebruikte Azure identity-oplossing is [hybride identiteit](https://docs.microsoft.com/azure/active-directory/active-directory-hybrid-identity-design-considerations-overview). Als u al investeringen in de lokale AD DS genomen hebt, kunt u eenvoudig de identiteit van de cloud met Azure AD Connect uitbreiden.|
|Mijn bedrijf in de cloud is geboren en we hebben geen investeringen in oplossingen voor on-premises-identiteit.| [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) is de beste keuze voor bedrijven cloudconfiguratie zonder lokale investeringen.|
|Moet ik lightweight Azure VM-configuratie en controle om te voldoen aan on-premises identiteitsvereisten voor de app ontwikkelen en testen.|[Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) is een goede keuze als u AD DS gebruiken voor lightweight Azure VM-configuratie-besturingselement moet of wilt ontwikkelen of migreren van oudere, directory geschikt on-premises toepassingen naar de cloud.|  
|Ik moet ondersteuning bieden voor enkele virtuele machines in Azure, maar mijn bedrijf is nog steeds sterk geïnvesteerd in de lokale Active Directory (AD DS).|Gebruik [ZELFOPLOSSING AD DS](https://msdn.microsoft.com/library/azure/jj156090.aspx) Azure VM's gebruiken wanneer u een paar virtuele machines ondersteunen en hebben grote investeringen met AD DS moet on-premises. |

## <a name="where-can-i-learn-more"></a>Waar vind ik meer informatie?
We hebben een berg geweldige bronnen online voor meer informatie over Azure AD. Hier volgt een lijst met artikelen die u geweldige kunt u op weg:

* [De map voor het beheer van hybride met Azure AD Connect inschakelen](active-directory-aadconnect.md)
* [Extra beveiliging voor een ooit verbonden wereld](../multi-factor-authentication/multi-factor-authentication.md)
* [Gebruiker inrichting en het opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren](active-directory-saas-app-provisioning.md)
* [Aan de slag met Azure AD-rapportage](active-directory-reporting-getting-started.md)
* [De wachtwoorden beheren vanaf elke locatie](active-directory-passwords-update-your-own-password.md)
* [Wat is de toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory?](active-directory-appssoaccess-whatis.md)
* [Gebruiker inrichting en het opheffen van inrichting voor SaaS-toepassingen met Azure Active Directory automatiseren](active-directory-saas-app-provisioning.md)
* [Het verstrekken van veilige externe toegang tot on-premises toepassingen](active-directory-application-proxy-get-started.md)
* [Managing access to resources with Azure Active Directory groups](active-directory-manage-groups.md) (Toegang tot resources beheren met Azure Active Directory-groepen)
* [Wat is Microsoft Azure Active Directory-licentieverlening?](active-directory-licensing-whatis-azure-portal.md)
* [Hoe kan ik cloudapps die worden gebruikt in mijn organisatie detecteren](active-directory-cloudappdiscovery-whatis.md)

## <a name="next-steps"></a>Volgende stappen

Nu dat u Azure identity-concepten en de opties die beschikbaar zijn voor u begrijpt, kunt u de volgende bronnen om te beginnen met het implementeren van de optie die u hebt gekozen:

[Meer informatie over hybride Azure-identiteitsoplossingen](https://docs.microsoft.com/azure/active-directory/choose-hybrid-identity-solution)

[Meer informatie in een omgeving met Azure Proof-of-Concept](https://aka.ms/aad-poc)

[Azure AD in de productieomgeving implementeren](https://aka.ms/aad-onboard)
