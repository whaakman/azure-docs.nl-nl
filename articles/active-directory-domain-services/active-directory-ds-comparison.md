---
title: 'Azure AD Domain Services: Vergelijken Azure AD Domain Services naar ZELFOPLOSSING domeincontrollers | Microsoft Docs'
description: Vergelijking van Azure Active Directory Domain Services met ZELFOPLOSSING domeincontrollers
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 165249d5-e0e7-4ed1-aa26-91a05a87bdc9
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: maheshu
ms.openlocfilehash: c384046d280e03de5a808d245dd273fdf7b44549
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-decide-if-azure-ad-domain-services-is-right-for-your-use-case"></a>U moet kiezen als Azure AD Domain Services is geschikt voor uw use case
U kunt uw workloads in Azure Infrastructure Services implementeren zonder te hoeven maken over het onderhoud van infrastructuur voor identiteiten in Azure met Azure AD Domain Services. Deze beheerde service wijkt af van een normale implementatie van Windows Server Active Directory die u implementeert en beheert zelf. De service is eenvoudig te implementeren en biedt geautomatiseerde statuscontrole en het doorvoeren. We zijn voortdurend de service voor het toevoegen van ondersteuning voor algemene implementatiescenario's in ontwikkeling.

We raden het volgende lezen materiaal aan om te bepalen of u Azure AD Domain Services:

* Overzicht van de [functies die worden aangeboden door Azure AD Domain Services](active-directory-ds-features.md).
* Bekijk algemene [implementatiescenario's voor Azure AD Domain Services](active-directory-ds-scenarios.md).
* Ten slotte [vergelijken van Azure AD Domain Services naar een doe AD-optie](active-directory-ds-comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure).

## <a name="compare-azure-ad-domain-services-to-diy-ad-domain-in-azure"></a>Azure AD Domain Services met ZELFOPLOSSING AD-domein in Azure vergelijken
De volgende tabel kunt u kiezen tussen Azure AD Domain Services gebruiken en beheren van uw eigen AD-infrastructuur in Azure.

| **Functie** | **Azure AD Domain Services** | **'Doe' AD in virtuele machines in Azure** |
| --- |:---:|:---:|
| [**Beheerde service**](active-directory-ds-comparison.md#managed-service) |**&#x2713;** |**& #x 2715;** |
| [**Beveiligde implementaties**](active-directory-ds-comparison.md#secure-deployments) |**&#x2713;** |Er moet een beheerder voor het beveiligen van de implementatie. |
| [**DNS-server**](active-directory-ds-comparison.md#dns-server) |**& #x 2713;**  (service beheerd) |**&#x2713;** |
| [**Domein- of Enterprise administrator-bevoegdheden**](active-directory-ds-comparison.md#domain-or-enterprise-administrator-privileges) |**& #x 2715;** |**&#x2713;** |
| [**Aan domein toevoegen**](active-directory-ds-comparison.md#domain-join) |**&#x2713;** |**&#x2713;** |
| [**Met behulp van NTLM en Kerberos-verificatie van domeinen**](active-directory-ds-comparison.md#domain-authentication-using-ntlm-and-kerberos) |**&#x2713;** |**&#x2713;** |
| [**Kerberos-beperkte overdracht**](active-directory-ds-comparison.md#kerberos-constrained-delegation)|op basis van bronnen|op basis van een resource & op basis van een account|
| [**Aangepaste OE-structuur**](active-directory-ds-comparison.md#custom-ou-structure) |**&#x2713;** |**&#x2713;** |
| [**Schema-uitbreidingen**](active-directory-ds-comparison.md#schema-extensions) |**& #x 2715;** |**&#x2713;** |
| [**AD-domein/forest vertrouwt**](active-directory-ds-comparison.md#ad-domain-or-forest-trusts) |**& #x 2715;** |**&#x2713;** |
| [**LDAP lezen**](active-directory-ds-comparison.md#ldap-read) |**&#x2713;** |**&#x2713;** |
| [**Beveiligde LDAP (LDAPS)**](active-directory-ds-comparison.md#secure-ldap) |**&#x2713;** |**&#x2713;** |
| [**LDAP-schrijven**](active-directory-ds-comparison.md#ldap-write) |**& #x 2715;** |**&#x2713;** |
| [**Groepsbeleid**](active-directory-ds-comparison.md#group-policy) |**&#x2713;** |**&#x2713;** |
| [**Geografisch verspreide implementaties**](active-directory-ds-comparison.md#geo-dispersed-deployments) |**& #x 2715;** |**&#x2713;** |

#### <a name="managed-service"></a>Beheerde service
Azure AD Domain Services-domeinen worden beheerd door Microsoft. U hebt geen zorgen te hoeven maken over patchen, updates, bewaking, back-ups en beschikbaarheid van uw domein. Deze beheertaken worden aangeboden als een service door Microsoft Azure voor uw beheerde domeinen.

#### <a name="secure-deployments"></a>Beveiligde implementaties
Het beheerde domein is veilig volgens de aanbevelingen voor de beveiliging van Microsoft voor AD-implementaties vergrendeld. Deze aanbevelingen voortvloeien uit het AD-productteam tientallen jaren ervaring engineering en de ondersteunende AD-implementaties. Voor implementaties van Doe moet u specifieke implementatiestappen ondernemen om te vergrendelen omlaag/secure uw implementatie.

#### <a name="dns-server"></a>DNS-server
Een beheerd domein van Azure AD Domain Services omvat beheerde DNS-services. Leden van de groep 'AAD DC Administrators' kunnen DNS op het beheerde domein beheren. Leden van deze groep worden volledige DNS-beheer bevoegdheden gegeven voor het beheerde domein. DNS-beheer kan worden uitgevoerd met behulp van de 'DNS-beheerconsole' opgenomen in het pakket Remote Server Administration Tools (RSAT).
[Meer informatie](active-directory-ds-admin-guide-administer-dns.md)

#### <a name="domain-or-enterprise-administrator-privileges"></a>Domein- of Ondernemingsadministrator-bevoegdheden
Deze verhoogde bevoegdheden zijn niet beschikbaar op een beheerde AAD-DS-domein. Toepassingen waarvoor deze verhoogde bevoegdheden kunnen niet worden geïmplementeerd bij AAD DS beheerde domeinen. Een kleinere subset van beheerdersbevoegdheden is beschikbaar voor leden van de groep gedelegeerd beheer met de naam van 'Administrators AAD DC'. Deze bevoegdheden opnemen bevoegdheden DNS configureren, Groepsbeleid zo configureren, krijgen de administrator-bevoegdheden op domein machines enzovoort.

#### <a name="domain-join"></a>Aan domein toevoegen
U kunt virtuele machines toevoegen aan het beheerde domein vergelijkbaar met hoe u computers aan een AD-domein toevoegen.

#### <a name="domain-authentication-using-ntlm-and-kerberos"></a>Met behulp van NTLM en Kerberos-verificatie van domeinen
Met Azure AD Domain Services, kunt u uw zakelijke referenties te verifiëren met het beheerde domein. Referenties zijn gesynchroniseerd met uw Azure AD-tenant. Gesynchroniseerde tenants Azure AD Connect zorgt ervoor dat wijzigingen met referenties die zijn aangebracht on-premises worden gesynchroniseerd naar Azure AD. Een doe domeininstelling mogelijk moet u voor het instellen van een domein AD-vertrouwensrelatie met uw on-premises AD voor gebruikers om te verifiëren met hun bedrijfsreferenties. Ook moet u wellicht AD-replicatie instellen om ervoor te zorgen dat wachtwoorden worden gesynchroniseerd met uw Azure-domeincontroller virtuele machines.

#### <a name="kerberos-constrained-delegation"></a>Kerberos-beperkte overdracht
U hebt geen beheerder van domein bevoegdheden op een beheerd domein van Active Directory Domain Services. Daarom configureren u Kerberos-beperkte overdracht (Traditioneel) op basis van het account niet. U kunt echter de beter beveiligde bronnen gebaseerde beperkte delegering configureren.
[Meer informatie](active-directory-ds-enable-kcd.md)

#### <a name="custom-ou-structure"></a>Aangepaste OE-structuur
Leden van de groep 'AAD DC-beheerders' kunnen maken van aangepaste OE's binnen het beheerde domein. Gebruikers die aangepaste OE's maken zijn volledige beheerdersrechten worden verleend via de organisatie-eenheid.
[Meer informatie](active-directory-ds-admin-guide-create-ou.md)

#### <a name="schema-extensions"></a>Schema-uitbreidingen
U kunt het basisschema van een beheerd domein van Azure AD Domain Services niet uitbreiden. Toepassingen die afhankelijk van uitbreidingen voor AD-schema (bijvoorbeeld nieuwe kenmerken onder het gebruikersobject zijn) kunnen niet worden opgeheven en daarom verplaatst naar de AAD-DS-domeinen.

#### <a name="ad-domain-or-forest-trusts"></a>AD-domein of Forest-vertrouwensrelaties
Beheerde domeinen kunnen niet worden geconfigureerd voor het instellen van vertrouwensrelaties (binnenkomend en uitgaand) met andere domeinen. Daarom resource forest implementatiescenario's Azure AD Domain Services niet gebruiken. Implementaties waarbij u liever geen wachtwoorden naar Azure AD synchroniseren niet op dezelfde manier Azure AD Domain Services gebruiken.

#### <a name="ldap-read"></a>LDAP lezen
Het beheerde domein ondersteunt LDAP werkbelastingen lezen. Daarom kunt u toepassingen waarmee leesbewerkingen op basis van het beheerde domein LDAP implementeren.

#### <a name="secure-ldap"></a>Secure LDAP
U kunt Azure AD Domain Services beveiligde LDAP toegang bieden tot uw beheerde domein, met inbegrip van via internet configureren.
[Meer informatie](active-directory-ds-admin-guide-configure-secure-ldap.md)

#### <a name="ldap-write"></a>LDAP-schrijven
Het beheerde domein is alleen-lezen voor gebruikersobjecten. Toepassingen die LDAP schrijfbewerkingen op basis van kenmerken van het gebruikersobject uitvoeren werkt daarom niet in een beheerd domein. Bovendien kunnen niet wachtwoorden van gebruikers worden gewijzigd in het beheerde domein. Een ander voorbeeld is de wijziging van groepslidmaatschappen of kenmerken binnen het beheerde domein, is niet toegestaan. Alle wijzigingen naar gebruikerskenmerken of wachtwoorden die zijn aangebracht in Azure AD (via PowerShell/Azure-portal) of on-premises AD worden gesynchroniseerd met het beheerde AAD-DS-domein.

#### <a name="group-policy"></a>Groepsbeleid
Er is een ingebouwde GPO elke voor de containers 'AADDC Computers' en 'AADDC gebruikers'. U kunt deze ingebouwde groepsbeleidsobjecten om Groepsbeleid te configureren. Leden van de groep 'AAD DC-beheerders' kunnen ook aangepaste GPO's maken en deze koppelen aan een bestaande organisatie-eenheden (met inbegrip van aangepaste OE's).
[Meer informatie](active-directory-ds-admin-guide-administer-group-policy.md)

#### <a name="geo-dispersed-deployments"></a>Geografisch verspreide implementaties
Azure AD Domain Services beheerde domeinen zijn beschikbaar in één virtueel netwerk in Azure. Voor scenario's waarvoor domeincontrollers beschikbaar zijn in meerdere Azure-regio's over de hele wereld, instellen van domeincontrollers in Azure IaaS VM's is mogelijk beter alternatief.


## <a name="do-it-yourself-diy-ad-deployment-options"></a>'Doe' (DIY) AD-implementatieopties
Mogelijk hebt u gebruiksvoorbeelden implementatie hier moet u enkele mogelijkheden die worden aangeboden door een Windows Server AD-installatie. In dergelijke gevallen kunt u overwegen een van de volgende opties voor doe (DIY):

* **Zelfstandig cloud domein:** kunt u een zelfstandige cloud domein' gebruik van Azure virtuele machines die zijn geconfigureerd als domeincontrollers instellen. Deze infrastructuur is niet geïntegreerd met uw on-premises AD-omgeving. Deze optie vereist een andere set van cloud-referenties voor het aanmelden/beheer van virtuele machines in de cloud.
* **Implementatie van de bron-forest:** kunt instellen van een domein in de topologie van de bron-forest met behulp van Azure virtuele machines die zijn geconfigureerd als domeincontrollers. Vervolgens kunt u een AD-vertrouwensrelatie configureren met uw on-premises AD-omgeving. U kunt computers lid van domein (Azure VM's) voor deze bron-forest in de cloud. Gebruikersverificatie gebeurt via ofwel een VPN-en ExpressRoute-verbinding met uw on-premises directory.
* **Het lokale domein uitbreiden naar Azure:** u kunt een virtuele Azure-netwerk verbinden met uw on-premises netwerk via een VPN-en ExpressRoute-verbinding. Met deze instelling kunt u virtuele Azure-machines worden toegevoegd aan uw on-premises AD. Er is een ander alternatief ter bevordering van de replica van domeincontrollers van uw lokale domein in Azure als een virtuele machine. U kunt vervolgens instellen deze om te repliceren via een VPN-en ExpressRoute-verbinding met uw on-premises directory. Deze implementatiemodus breidt uw lokale domein effectief naar Azure.

> [!NOTE]
> U wordt bepaald door een optie voor ZELFOPLOSSING is beter geschikt voor use cases in uw implementatie. Houd rekening met [feedback delen](active-directory-ds-contact-us.md) om u te helpen ons inzicht in hoe functies kunt u Azure AD Domain Services in de toekomst hebt gekozen. Deze feedback kunnen we de service beter aanpassen aan uw implementatievereisten en use cases ontwikkelen.
>
>

We hebben gepubliceerd [richtlijnen voor het implementeren van Windows Server Active Directory op Azure Virtual Machines](https://msdn.microsoft.com/library/azure/jj156090.aspx) waarmee ZELFOPLOSSING installaties beter.

## <a name="related-content"></a>Gerelateerde inhoud
* [Onderdelen - Azure AD Domain Services](active-directory-ds-features.md)
* [Implementatiescenario - Azure AD Domain Services](active-directory-ds-scenarios.md)
* [Richtlijnen voor het implementeren van Windows Server Active Directory op virtuele Machines in Azure](https://msdn.microsoft.com/library/azure/jj156090.aspx)
