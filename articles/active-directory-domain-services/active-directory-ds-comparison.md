---
title: 'Azure AD Domain Services: Azure AD Domain Services en ZELFGEMAAKT domeincontrollers vergelijken | Microsoft Docs'
description: Vergelijking van Azure Active Directory Domain Services met zelf domeincontrollers
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 165249d5-e0e7-4ed1-aa26-91a05a87bdc9
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/07/2017
ms.author: ergreenl
ms.openlocfilehash: fbd876ad46342dec7e2e73b92d3aafbd37cc205b
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58892293"
---
# <a name="how-to-decide-if-azure-ad-domain-services-is-right-for-your-use-case"></a>U moet kiezen als Azure AD Domain Services is geschikt voor uw use-casescenario
U kunt uw workloads in Azure Infrastructure Services, zonder te hoeven maken over het beheren van infrastructuur voor identiteiten in Azure implementeren met Azure AD Domain Services. Deze beheerde service wijkt af van een typische implementatie van Windows Server Active Directory die u implementeren en beheren op uw eigen. De service is eenvoudig te implementeren en biedt een geautomatiseerde statuscontrole en herstel. We zijn voortdurend de service voor het toevoegen van ondersteuning voor algemene scenario's voor implementatie in ontwikkeling.

Als u wilt beslissen of u het gebruik van Azure AD Domain Services wordt aangeraden het volgende materiaal voor lezen:

* Overzicht van de [functies die worden aangeboden door Azure AD Domain Services](active-directory-ds-features.md).
* Beoordeling gemeenschappelijke [implementatiescenario's voor Azure AD Domain Services](active-directory-ds-scenarios.md).
* Ten slotte [vergelijken van Azure AD Domain Services naar een doe AD-optie](active-directory-ds-comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure).

## <a name="compare-azure-ad-domain-services-to-diy-ad-domain-in-azure"></a>Azure AD Domain Services en ZELFGEMAAKT AD-domein in Azure vergelijken
De volgende tabel kunt u kiezen tussen het gebruik van Azure AD Domain Services en het beheer van uw eigen AD-infrastructuur in Azure.

| **Functie** | **Azure AD Domain Services** | **"Doe" AD in Azure VM 's** |
| --- |:---:|:---:|
| [**Beheerde service**](active-directory-ds-comparison.md#managed-service) |**&#x2713;** |**&#x2715;** |
| [**Beveiligde implementaties**](active-directory-ds-comparison.md#secure-deployments) |**&#x2713;** |Er moet een beheerder voor het beveiligen van de implementatie. |
| [**DNS-server**](active-directory-ds-comparison.md#dns-server) |**&#x2713;**(beheerde service) |**&#x2713;** |
| [**Domein- of administrator-bevoegdheden**](active-directory-ds-comparison.md#domain-or-enterprise-administrator-privileges) |**&#x2715;** |**&#x2713;** |
| [**Lid maken van domein**](active-directory-ds-comparison.md#domain-join) |**&#x2713;** |**&#x2713;** |
| [**Domeinverificatie met behulp van NTLM en Kerberos**](active-directory-ds-comparison.md#domain-authentication-using-ntlm-and-kerberos) |**&#x2713;** |**&#x2713;** |
| [**Kerberos-beperkte overdracht**](active-directory-ds-comparison.md#kerberos-constrained-delegation)|op basis van resource|op basis van een resource & op basis van een account|
| [**Aangepaste OE-structuur**](active-directory-ds-comparison.md#custom-ou-structure) |**&#x2713;** |**&#x2713;** |
| [**Schema-uitbreidingen**](active-directory-ds-comparison.md#schema-extensions) |**&#x2715;** |**&#x2713;** |
| [**AD-domein/forest vertrouwt**](active-directory-ds-comparison.md#ad-domain-or-forest-trusts) |**&#x2715;** |**&#x2713;** |
| [**LDAP lezen**](active-directory-ds-comparison.md#ldap-read) |**&#x2713;** |**&#x2713;** |
| [**Secure LDAP (LDAPS)**](active-directory-ds-comparison.md#secure-ldap) |**&#x2713;** |**&#x2713;** |
| [**LDAP-schrijven**](active-directory-ds-comparison.md#ldap-write) |**&#x2715;** |**&#x2713;** |
| [**Groepsbeleid**](active-directory-ds-comparison.md#group-policy) |**&#x2713;** |**&#x2713;** |
| [**Geografisch gedistribueerde implementaties**](active-directory-ds-comparison.md#geo-dispersed-deployments) |**&#x2715;** |**&#x2713;** |

#### <a name="managed-service"></a>Beheerde service
Azure AD Domain Services-domeinen worden beheerd door Microsoft. U geen zorgen over patches, updates, bewaking, back-ups, en ervoor te zorgen dat de beschikbaarheid van uw domein te maken. Deze beheertaken worden aangeboden als een service in Microsoft Azure voor uw beheerde domeinen.

#### <a name="secure-deployments"></a>Beveiligde implementaties
Het beheerde domein is veilig aan de hand van aanbevelingen voor de beveiliging van Microsoft voor AD-implementaties vergrendeld. Deze aanbevelingen afkomstig zijn van het AD-productteam tientallen jaren ervaring engineering- en AD-implementaties ondersteunen. Voor implementaties van Doe moet u specifieke implementatiestappen ondernemen om te vergrendelen omlaag/secure uw implementatie.

#### <a name="dns-server"></a>DNS-server
Een beheerd domein van Azure AD Domain Services bevat beheerde DNS-services. Leden van de groep 'AAD DC Administrators' kunnen DNS in het beheerde domein beheren. Leden van deze groep krijgt volledige DNS-beheer-bevoegdheden voor het beheerde domein. DNS-beheer kan worden uitgevoerd met behulp van de 'DNS-beheerconsole' opgenomen in het pakket Remote Server Administration Tools (RSAT).
[Meer informatie](active-directory-ds-admin-guide-administer-dns.md)

#### <a name="domain-or-enterprise-administrator-privileges"></a>Domein- of Enterprise-beheerder-bevoegdheden
Deze verhoogde bevoegdheden zijn niet beschikbaar op een beheerde AAD-DS-domein. Toepassingen waarvoor deze verhoogde bevoegdheden kunnen niet worden geïmplementeerd op basis van AAD-DS beheerde domeinen. Een kleinere subset van beheerdersbevoegdheden is beschikbaar voor leden van de gedelegeerd beheergroep 'AAD DC Administrators' genoemd. Deze bevoegdheden zijn rechten om te configureren van DNS, Groepsbeleid te configureren, administrator-bevoegdheden op domein machines enzovoort krijgen.

#### <a name="domain-join"></a>Lid maken van domein
U kunt virtuele machines toevoegen aan het beheerde domein vergelijkbaar met hoe u computers aan een AD-domein toevoegen.

#### <a name="domain-authentication-using-ntlm-and-kerberos"></a>Domeinverificatie met behulp van NTLM en Kerberos
Met Azure AD Domain Services, kunt u uw bedrijfsreferenties gebruiken om te verifiëren met het beheerde domein. Referenties worden bewaard gesynchroniseerd met uw Azure AD-tenant. Voor gesynchroniseerde tenants Azure AD Connect zorgt ervoor dat wijzigingen met referenties die on-premises naar Azure AD worden gesynchroniseerd. Met een doe domeininstelling u mogelijk nodig hebt voor het instellen van een AD-domein-vertrouwensrelatie met uw on-premises AD voor gebruikers om te verifiëren met hun zakelijke referenties. Mogelijk moet u ook AD-replicatie instellen om ervoor te zorgen dat wachtwoorden van gebruikers met uw Azure-domein netwerkcontroller-VM's synchroniseren.

#### <a name="kerberos-constrained-delegation"></a>Kerberos-beperkte overdracht
U hebt geen domein Administrator-bevoegdheden op een Active Directory Domain Services beheerde domein. U kunt op basis van een account (Traditioneel) Kerberos-beperkte overdracht daarom niet configureren. U kunt echter de beter beveiligde bronnen gebaseerde beperkte delegering configureren.
[Meer informatie](active-directory-ds-enable-kcd.md)

#### <a name="custom-ou-structure"></a>Aangepaste OE-structuur
Leden van de groep 'AAD DC Administrators' kunnen aangepaste OE's binnen het beheerde domein te maken. Gebruikers voor het maken van aangepaste OE's krijgen volledige beheerdersrechten over de organisatie-eenheid.
[Meer informatie](active-directory-ds-admin-guide-create-ou.md)

#### <a name="schema-extensions"></a>Schema-uitbreidingen
U kunt het basisschema van een Azure AD Domain Services beheerde domein niet uitbreiden. Toepassingen die afhankelijk van uitbreidingen van AD-schema (bijvoorbeeld nieuwe kenmerken onder het gebruikersobject zijn) kunnen daarom niet worden opgeheven en verplaatst naar de AAD-DS-domeinen.

#### <a name="ad-domain-or-forest-trusts"></a>AD-domein of Forest-vertrouwensrelaties
Beheerde domeinen kunnen niet worden geconfigureerd voor het instellen van vertrouwensrelaties (Inkomend/uitgaand) met andere domeinen. Daarom resource forest implementatiescenario's Azure AD Domain Services niet gebruiken. Implementaties waarbij u liever niet te synchroniseren van wachtwoorden naar Azure AD kunnen niet op dezelfde manier gebruikt u Azure AD Domain Services.

#### <a name="ldap-read"></a>LDAP lezen
LDAP lezen workloads biedt ondersteuning voor het beheerde domein. Daarom kunt u toepassingen die uitvoeren van LDAP-leesbewerkingen op basis van het beheerde domein implementeren.

#### <a name="secure-ldap"></a>Secure LDAP
U kunt Azure AD Domain Services voor toegang van secure LDAP tot uw beheerde domein, met inbegrip van via internet configureren.
[Meer informatie](active-directory-ds-admin-guide-configure-secure-ldap.md)

#### <a name="ldap-write"></a>LDAP-schrijven
Het beheerde domein is alleen-lezen voor gebruikersobjecten. Toepassingen die uitvoeren van LDAP-schrijfbewerkingen op basis van kenmerken van het gebruikersobject werkt daarom niet in een beheerd domein. Bovendien kunnen niet wachtwoorden van gebruikers worden gewijzigd in het beheerde domein. Een ander voorbeeld is de wijziging van lidmaatschap van groepen of groepskenmerken binnen het beheerde domein, dit is niet toegestaan. Echter, de wijzigingen aan gebruikerskenmerken of wachtwoorden die u in Azure AD (via PowerShell/Azure portal) of on-premises AD worden gesynchroniseerd met het beheerde AAD-DS-domein.

#### <a name="group-policy"></a>Groepsbeleid
Er is een ingebouwde groepsbeleidsobject elke voor de containers 'AADDC Computers' en 'AADDC gebruikers'. U kunt deze ingebouwde groepsbeleidsobjecten om Groepsbeleid te configureren. Leden van de groep 'AAD DC Administrators' kunnen ook aangepaste GPO's maken en deze koppelen aan een bestaande organisatie-eenheden (met inbegrip van aangepaste OE's).
[Meer informatie](active-directory-ds-admin-guide-administer-group-policy.md)

#### <a name="geo-dispersed-deployments"></a>Geografisch verspreide implementaties
Azure AD Domain Services beheerde domeinen zijn beschikbaar in één virtueel netwerk in Azure. Voor scenario's waarin de domeincontrollers beschikbaar zijn in meerdere Azure-regio's over de hele wereld, instellen van domeincontrollers in Azure IaaS VM's mogelijk beter alternatief.


## <a name="do-it-yourself-diy-ad-deployment-options"></a>'Do-it-yourself' (DIY) AD deployment options
Mogelijk hebt u implementatie use cases waar u enkele van de mogelijkheden die door de installatie van een Windows Server AD nodig hebt. In dergelijke gevallen, houd rekening met een van de volgende opties voor doe (DIY):

* **Zelfstandige cloud domein:** U kunt een zelfstandige cloud domein"met behulp van Azure virtuele machines die zijn geconfigureerd als domeincontrollers instellen. Deze infrastructuur is niet geïntegreerd met uw on-premises AD-omgeving. Deze optie vereist een andere set 'cloudreferenties' aanmelden/beheren van virtuele machines in de cloud.
* **Implementatie van de bron-forest:** U kunt een domein in de topologie van de bron-forest instellen met behulp van Azure virtuele machines die zijn geconfigureerd als domeincontrollers. Vervolgens kunt u een AD-vertrouwensrelatie configureren met uw on-premises AD-omgeving. Lid van domein computers (Azure VM's) kunt u naar dit forest resource in de cloud. Gebruikersverificatie gebeurt via beide een VPN/ExpressRoute-verbinding met uw on-premises directory.
* **Breid uw on-premises domein naar Azure:** U kunt een Azure-netwerk verbinden met uw on-premises netwerk via een VPN/ExpressRoute-verbinding. Met deze instelling kunt u virtuele Azure-machines worden toegevoegd aan uw on-premises AD. Een ander alternatief is om te promoten replicadomeincontrollers van uw on-premises domein in Azure als een virtuele machine. U kunt vervolgens instellen deze om te repliceren via een VPN/ExpressRoute-verbinding met uw on-premises directory. Uw on-premises domein uitbreidt deze implementatiemodus effectief naar Azure.

> [!NOTE]
> U kan bepalen dat een optie voor het zelf beter voor de use cases in uw implementatie geschikt is. Houd rekening met [feedback delen](active-directory-ds-contact-us.md) aan ons beter te begrijpen wat functies wilt helpen u Azure AD Domain Services in de toekomst hebt gekozen. Deze feedback helpt ons de service beter aan de behoeften van uw implementatiebehoeften en use cases ontwikkelen.
>
>

We hebben gepubliceerd [richtlijnen voor het implementeren van Windows Server Active Directory op Azure Virtual Machines](/windows-server/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) te vereenvoudigen en ZELFGEMAAKT installaties.

## <a name="related-content"></a>Gerelateerde inhoud
* [Functies - Azure AD Domain Services](active-directory-ds-features.md)
* [Implementatiescenario's - Azure AD Domain Services](active-directory-ds-scenarios.md)
* [Richtlijnen voor het implementeren van Windows Server Active Directory op Azure Virtual Machines](/windows-server/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100)
