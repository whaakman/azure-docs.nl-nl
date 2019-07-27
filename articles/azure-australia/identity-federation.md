---
title: Identiteits Federatie in azure Australië
description: Richt lijnen voor het configureren van identiteits Federatie binnen de Australische regio's om te voldoen aan de specifieke vereisten van het beleid voor de Australische overheid, voor schriften en wetgeving.
author: galey801
ms.service: azure-australia
ms.topic: quickstart
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: e161e36ee7b403381b65f52a6f416be09025d0a2
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68570857"
---
# <a name="identity-federation-in-azure-australia"></a>Identiteits Federatie in azure Australië

Identiteits beheer en Federatie met open bare Cloud aanbiedingen is een van de meest essentiële eerste stappen voor het gebruik van de Cloud. De Azure Active Directory service van micro soft slaat gebruikers gegevens op om toegang tot Cloud Services mogelijk te maken en is een vereiste om andere Azure-Services te gebruiken.

In dit artikel worden de belangrijkste ontwerp punten besproken voor het implementeren van Azure Active Directory, het synchroniseren van gebruikers van een Active Directory Domain Services domein en het implementeren van beveiligde authenticatie. Specifieke focus vindt plaats op de aanbevelingen in de Information Security Manual (ISM) en Azure-certificerings rapporten van de Australische Cyber Security Center.

De classificatie van informatie die is opgeslagen in Azure Active Directory moet besluiten over de manier waarop deze is ontworpen. Het volgende fragment wordt gegeven uit het [ACSC-certificerings rapport, Microsoft Azure](https://aka.ms/au-irap):

>**ACSC-certificerings rapport – Microsoft Azure** Azure Active Directory (Azure AD) moet worden geconfigureerd met Active Directory Federation-Services wanneer de Gemenebests entiteiten het gebruik en de gegevens inhoud van hun Active Directory op beveiligd classificeren. Hoewel Active Directory gegevens bij de classificatie van de niet-geclassificeerde UDLM-indeling (verspreidings beperking) geen Federatie vereist, kunnen Gemenebests entiteiten de Federatie nog steeds implementeren om Risico's te beperken die zijn gekoppeld aan de service die wordt aangeboden van buiten van Australië.

Daarom zijn de gegevens die worden geauthenticeerd en het mechanisme waarmee gebruikers worden geverifieerd, de twee belang rijke zaken die hier worden besproken.

## <a name="key-design-considerations"></a>Belangrijkste overwegingen bij het ontwerpen

### <a name="user-synchronisation"></a>Gebruikers synchronisatie

Bij het implementeren van Azure AD Connect zijn er verschillende beslissingen die moeten worden gemaakt over de gegevens die worden gedistribueerd. Azure AD Connect is gebaseerd op Microsoft Identity Manager en biedt een robuuste functie-set voor het [Transformeren](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-best-practices-changing-default-configuration) van gegevens tussen directory's.

Micro soft Consulting Services kan worden ingeschakeld voor een ADRAP-evaluatie van uw bestaande Windows Server-Active Directory. De ADRAP helpt bij het bepalen van eventuele problemen die moeten worden opgelost voordat u kunt synchroniseren met Azure Active Directory. Deze service wordt in het algemeen in micro soft Premier Support-overeenkomsten meegenomen.

Het [IDFix-hulp programma](https://docs.microsoft.com/office365/enterprise/install-and-run-idfix) scant uw on-premises Active Directory domein op problemen voordat u de synchronisatie met Azure AD uitvoert. IDFix is een sleutel eerste stap voordat u Azure AD Connect implementeert. Hoewel een IDFix-scan een groot aantal problemen kan identificeren, kunnen veel van deze problemen snel worden opgelost met scripts of worden gewerkt met behulp van gegevens transformaties in Azure AD Connect.

Voor Azure AD moeten gebruikers een extern routeerbaar domein op het hoogste niveau hebben om verificatie mogelijk te maken. Als uw domein een UPN-achtervoegsel heeft dat niet extern routeerbaar is, moet u de [alternatieve aanmeldings-id](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-userprincipalname) instellen in AD Connect met het kenmerk mail van de gebruiker. Gebruikers melden zich vervolgens aan bij Azure-Services met hun e-mail adres in plaats van de aanmelding van het domein.

Het UPN-achtervoegsel voor gebruikers accounts kan ook worden gewijzigd met hulpprogram ma's zoals Power shell. Dit kan onverwachte gevolgen hebben voor andere verbonden systemen en wordt niet meer beschouwd als best practice.

Bij het bepalen van de kenmerken die u wilt synchroniseren met Azure Active Directory, is het veilig om ervan uit te gaan dat alle kenmerken vereist zijn. Het is zelden mogelijk dat een map daad werkelijke beveiligde gegevens bevat, maar het uitvoeren van een audit wordt aanbevolen. Als er in de Directory beveiligde gegevens worden gevonden, kunt u de gevolgen van het weglaten of transformeren van het kenmerk beoordelen. Als handige hand leiding vindt u een lijst met kenmerken die Microsoft Cloud Services [nodig hebben](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-sync-attributes-synchronized).

### <a name="authentication"></a>Authentication

Het is belang rijk om inzicht te krijgen in de beschik bare opties en hoe ze kunnen worden gebruikt om eind gebruikers te beschermen.
Micro soft biedt [drie systeem eigen oplossingen](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-user-signin) om gebruikers te verifiëren tegen Azure Active Directory:

* Synchronisatie van wacht woord-hash-de gehashte wacht woorden van Active Directory Domain Services worden gesynchroniseerd door Azure AD Connect in Azure Active Directory.
* [Pass-](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta) through-verificatie: wacht woorden blijven binnen Active Directory Domain Services. Gebruikers worden geverifieerd op basis van Active Directory Domain Services via een agent. Er worden geen wacht woorden opgeslagen in azure AD.
* [Federatieve SSO](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-fed-whatis) -Azure Active Directory is Active Directory Federation Services, tijdens het aanmelden, worden gebruikers door Azure doorgestuurd naar Active Directory Federation Services om te verifiëren. Er worden geen wacht woorden opgeslagen in azure AD.

Synchronisatie van wacht woord-hashen kan worden gebruikt in scenario's waarin officiële gegevens worden opgeslagen in de map. Scenario's waarin beveiligde gegevens worden opgeslagen, vereisen een van de twee resterende opties.

Deze drie opties bieden ondersteuning voor het [terugschrijven van wacht woorden](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-writeback), die door de [ACSC-gebruikers handleiding](https://aka.ms/au-irap) worden aangeraden. Indien organisaties moeten het risico van het terugschrijven van wacht woorden evalueren tegen de productiviteits toename en de ondersteunings inspanningen van het gebruik van selfservice voor het opnieuw instellen van wacht woorden te verlagen.

#### <a name="pass-through-authentication-pta"></a>Pass-Through-verificatie (PTA)

Pass Through-verificatie is vrijgegeven nadat de IRAP-analyse is voltooid en daarom; moet afzonderlijk worden geëvalueerd om te bepalen hoe de oplossing past bij het risico profiel van uw organisatie. Pass Through-verificatie geniet de voor keur boven Federatie door micro soft als gevolg van de verbeterde beveiligings postuur.

![Pass-Through-verificatie](media/pta1.png)

Pass-Through-verificatie geeft aan dat er verschillende ontwerp factoren in acht moeten worden genomen:

* De Pass-Through-verificatie agent moet in staat zijn om uitgaande verbindingen met Microsoft Cloud Services tot stand te brengen.
* Er wordt meer dan één agent geïnstalleerd om ervoor te zorgen dat de service Maxi maal beschikbaar is. Het is best practice om ten minste drie agents en Maxi maal 12 agents te implementeren.
* De aanbevolen procedure is om te voor komen dat u de agent rechtstreeks op een Active Directory-domein controller installeert. Bij het implementeren van Azure AD Connect met Pass-Through-verificatie, wordt standaard de agent geïnstalleerd op de AD Connect-server.
* Pass-Through-verificatie is een lager onderhouds optie dan Active Directory Federation Services omdat hiervoor geen dedicated server-infra structuur, certificaat beheer of binnenkomende firewall regels zijn vereist.

#### <a name="active-directory-federation-services-adfs"></a>Active Directory Federation Services (ADFS)

Active Directory Federation Services is opgenomen in de IRAP-evaluatie en is goedgekeurd voor gebruik in beveiligde omgevingen.

![Federation](media/federated-identity.png)

Active Directory Federation Services zijn verschillende ontwerp factoren die in overweging moeten worden genomen:

* Voor Federatie Services is netwerk ingangen vereist voor HTTPS-verkeer via internet of ten minste de service-eind punten van micro soft.
* Federation Services maakt gebruik van PKI en certificaten, die voortdurend beheer en verlenging vereisen.
* Federation Services moeten worden geïmplementeerd op speciale servers en de relevante netwerk infrastructuur vereist om deze extern toegankelijk te maken.

### <a name="multi-factor-authentication-mfa"></a>Multi-Factor Authentication (MFA)

In het gedeelte ISM in multi-factor Authentication wordt aanbevolen de implementatie uit te voeren in de volgende scenario's op basis van uw risico profiel:

* Standaard gebruikers verifiëren
* Bevoegde accounts verifiëren
* Gebruikers verifiëren externe toegang
* Gebruikers die bevoegde acties uitvoeren

Azure Active Directory biedt multi-factor Authentication die kan worden ingeschakeld voor alle of een subset van gebruikers (bijvoorbeeld alleen geprivilegieerde accounts). Micro soft biedt ook een oplossing met de naam voorwaardelijke toegang, waarmee u meer gedetailleerde controle over de manier waarop multi-factor Authentication wordt toegepast (bijvoorbeeld alleen wanneer gebruikers zich aanmelden vanaf externe IP-adresbereiken).

Azure multi-factor Authentication ondersteunt de volgende ISM aanvaard bare vormen van validatie:

* Telefoonoproep
* SMS-bericht
* Microsoft Authenticator toepassing
* Ondersteunde hardware-tokens

Privileged Identity Management, een onderdeel van Azure Active Directory, kan worden gebruikt om het gebruik van multi-factor Authentication af te dwingen wanneer gebruikers hun machtigingen verhogen om aan de vierde aanbeveling te voldoen.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg het artikel over op [rollen gebaseerde toegangs beheer en privileged Identity Management](role-privileged.md).
