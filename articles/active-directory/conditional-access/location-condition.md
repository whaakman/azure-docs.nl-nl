---
title: Wat is de locatievoorwaarden in Azure Active Directory voor voorwaardelijke toegang? | Microsoft Docs
description: Informatie over het gebruik van de locatievoorwaarde voor het beheren van toegang tot uw cloud-apps op basis van de netwerklocatie van de gebruiker.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleid voor voorwaardelijke toegang
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/01/2019
ms.author: markvi
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: e405b592e75ca8b9fd811c7f891baafa19e528da
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57241184"
---
# <a name="what-is-the-location-condition-in-azure-active-directory-conditional-access"></a>Wat is de locatievoorwaarde in Azure Active Directory voor voorwaardelijke toegang? 

Met [voorwaardelijke toegang van Azure Active Directory (Azure AD)](../active-directory-conditional-access-azure-portal.md), u kunt bepalen hoe gemachtigde gebruikers hebben toegang tot uw cloudapps. De locatievoorwaarde van een beleid voor voorwaardelijke toegang kunt u instellingen aan de netwerklocaties van uw gebruikers toegang verbinden.

In dit artikel biedt u de informatie die u nodig hebt om de locatievoorwaarde te configureren. 

## <a name="locations"></a>Locaties

Azure AD, schakelt u eenmalige aanmelding op apparaten, apps en services vanaf elke locatie op het openbare internet. U kunt de toegang tot uw cloud-apps op basis van de netwerklocatie van een gebruiker beheren met de locatievoorwaarde. Algemene scenario's voor de locatievoorwaarde zijn:

- Meervoudige verificatie vereisen voor gebruikers die toegang krijgen tot een service wanneer ze buiten het bedrijfsnetwerk bevinden vallen.

- Blokkeert de toegang voor gebruikers met toegang tot een service van specifieke landen of regio's.

Een locatie is een label voor een netwerklocatie bevindt die een vertegenwoordigt een benoemde locatie of multi-factor authentication IP-adressen vertrouwde.


## <a name="named-locations"></a>Benoemde locaties 

U kunt logische groeperingen van IP-adresbereiken, landen en regio's maken met benoemde locaties. 

U hebt toegang tot uw benoemde locaties in de **beheren** sectie van de pagina voor voorwaardelijke toegang.

![Locaties](./media/location-condition/02.png)

 


Een benoemde locatie heeft de volgende onderdelen:

![Locaties](./media/location-condition/42.png)

- **Naam** -de naam van een benoemde locatie.

- **IP-adresbereiken** -een of meer IPv4-adresbereiken in CIDR-indeling. Een IPv6-adresbereik op te geven wordt niet ondersteund.

- **Als vertrouwde locatie markeren** -een vlag die u kunt instellen voor een benoemde locatie om aan te geven van een vertrouwde locatie bevindt. Vertrouwde locaties zijn meestal netwerkgebieden die worden beheerd door uw IT-afdeling. Naast de voorwaardelijke toegang benoemde locaties vertrouwde worden ook gebruikt door Azure Identity Protection en Azure AD-beveiligingsrapporten te verminderen [fout-positieven](../reports-monitoring/concept-risk-events.md#impossible-travel-to-atypical-locations-1).

- **Landen/regio's** -deze optie kunt u een of meer land of regio voor het definiëren van een benoemde locatie selecteren. 

- **Onbekende gebieden opnemen** -enkele IP-adressen zijn niet toegewezen aan een bepaald land. Deze optie kunt u kiezen of deze IP-adressen op de locatie met de naam moeten worden opgenomen. Gebruik deze instelling wanneer het beleid met behulp van de locatie met de naam van toepassing op onbekende locaties.

Het aantal benoemde locaties die u kunt configureren wordt beperkt door de grootte van het gerelateerde object in Azure AD. U kunt een van de volgende configureren:

- Een locatie met maximaal 1200 IP-bereiken met de naam.

- Maximaal 90 benoemde locaties met één IP-adresbereik dat is toegewezen aan elk van deze.




## <a name="trusted-ips"></a>Goedgekeurde IP-adressen

U kunt ook configureren met IP-adresbereiken van uw organisatie lokaal intranet in voor de [instellingen voor multi-factor authentication-service](https://account.activedirectory.windowsazure.com/usermanagement/mfasettings.aspx). Deze functie kunt u maximaal 50 IP-adresbereiken configureren. Er zijn de IP-adresbereiken in CIDR-indeling. Zie voor meer informatie, [vertrouwde IP-adressen](../authentication/howto-mfa-mfasettings.md#trusted-ips).  

Als u hebt vertrouwde IP-adressen geconfigureerd, worden ze weergegeven als **MFA goedgekeurde IP-Adressen** in de lijst met locaties voor de locatievoorwaarde.   

### <a name="skipping-multi-factor-authentication"></a>Meervoudige verificatie overslaan

Op de pagina van de instellingen in de multi-factor authentication-service, kunt u het bedrijfsintranet gebruikers identificeren door te selecteren **multi-factor authentication overslaan voor aanvragen van federatieve gebruikers op mijn intranet**. Deze instelling geeft aan dat de zakelijke binnen claim, dat is uitgegeven door AD FS, het netwerk moeten worden vertrouwd en gebruikt voor het identificeren van de gebruiker wordt op het bedrijfsnetwerk bevinden. Zie voor meer informatie, [de goedgekeurde IP-adressen-functie inschakelen door middel van voorwaardelijke toegang](../authentication/howto-mfa-mfasettings.md#enable-the-trusted-ips-feature-by-using-conditional-access).

Nadat u deze optie inschakelt, met inbegrip van de benoemde locatie **MFA goedgekeurde IP-Adressen** geldt voor elk beleid met deze geselecteerd.

Voorwaardelijke toegang is voor mobiele en desktoptoepassingen, die hebben lange levensduur van de sessie, regelmatig opnieuw geëvalueerd. De standaardwaarde is één keer per uur. Wanneer de binnen bedrijfsnetwerk alleen op het moment van de initiële verificatie is uitgegeven, wordt Azure AD mogelijk niet een lijst met goedgekeurde IP-bereiken. In dit geval is het moeilijker om te bepalen of de gebruiker nog steeds op het bedrijfsnetwerk bevinden:

1. Controleer of het IP-adres van de gebruiker zich in een van de goedgekeurde IP-bereiken.

2. Controleer of de eerste drie octetten van het IP-adres van de gebruiker overeenkomen met de eerste 3 octetten van het IP-adres van de eerste verificatie. Het IP-adres wordt vergeleken met de initiële verificatie, omdat dit wanneer de binnen bedrijfsnetwerk is oorspronkelijk heeft uitgegeven en de locatie van de gebruiker is gevalideerd.

Als beide stappen mislukken, een gebruiker wordt beschouwd als niet meer op een vertrouwde IP-adres.



## <a name="location-condition-configuration"></a>Configuratie van voorwaarde

Wanneer u de locatievoorwaarde te configureren, hebt u de optie voor het onderscheid maken tussen:

- Elke locatie 
- Alle vertrouwde locaties
- Geselecteerde locaties

![Locaties](./media/location-condition/01.png)

### <a name="any-location"></a>Elke locatie

Standaard selecteren **elke locatie** zorgt ervoor dat een beleid moet worden toegepast op alle IP-adressen, wat betekent elk adres op het Internet dat. Deze instelling is niet beperkt tot IP-adressen die u hebt geconfigureerd als benoemde locatie. Wanneer u selecteert **elke locatie**, u kunt nog steeds specifieke locaties uitsluiten van een beleid. U kunt bijvoorbeeld een beleid toepassen op alle locaties met uitzondering van vertrouwde locaties om in te stellen van het bereik voor alle locaties, met uitzondering van het bedrijfsnetwerk bevinden.

### <a name="all-trusted-locations"></a>Alle vertrouwde locaties

Deze optie is van toepassing op:

- Alle locaties die zijn gemarkeerd als vertrouwde locatie bevindt
- **MFA goedgekeurde IP-Adressen** (indien geconfigureerd)


### <a name="selected-locations"></a>Geselecteerde locaties

Met deze optie kunt u een of meer benoemde locaties selecteren. Voor een beleid met deze instelling om toe te passen, moet een gebruiker verbinding maken vanaf een van de geselecteerde locatie. Wanneer u klikt op **Selecteer** de benoemde netwerk-upbesturingselement voor selectie waarin de lijst met benoemde netwerken wordt geopend. De lijst bevat ook als de netwerklocatie is gemarkeerd als vertrouwd. De benoemde locatie met de naam **MFA goedgekeurde IP-adressen** wordt gebruikt om op te nemen van de IP-instellingen die kunnen worden geconfigureerd op de pagina met multi-factor authentication-service-instelling.

## <a name="what-you-should-know"></a>Wat u moet weten

### <a name="when-is-a-location-evaluated"></a>Wanneer is een locatie geëvalueerd?

Beleid voor voorwaardelijke toegang worden geëvalueerd wanneer: 

- Een gebruiker zich in eerste instantie aanmeldt bij een web-app, mobiel of bureaublad-App. 

- Een mobiele of bureaubladtoepassingen toepassing die gebruikmaakt van moderne verificatie maakt gebruik van een vernieuwingstoken een nieuw toegangstoken verkrijgen. Standaard is dit één keer per uur. 

Dit betekent dat voor mobiele en bureaubladtoepassingen gebruiken moderne verificatie, een wijziging in de locatie gedetecteerd binnen een uur van het wijzigen van de netwerklocatie. Voor mobiele en desktoptoepassingen die niet van moderne verificatie gebruikmaken, wordt het beleid toegepast op elke token aan te vragen. De frequentie van de aanvraag kan variëren op basis van de toepassing. Op dezelfde manier voor webtoepassingen, het beleid wordt toegepast op de eerste aanmelding en goed is voor de levensduur van de sessie in de web-App. De tijd tussen de evaluatie van het beleid wordt ook variëren als gevolg van verschillen in de levensduur van de sessie voor toepassingen. Telkens wanneer de toepassing vraagt een nieuw token aanmelden om wordt het beleid toegepast.


Azure AD geeft standaard een token op uurbasis. Nadat u hebt verplaatst uit het bedrijfsnetwerk bevinden, binnen een uur nadat wordt het beleid afgedwongen voor toepassingen die gebruikmaken van moderne verificatie.


### <a name="user-ip-address"></a>IP-gebruikersadres

Het IP-adres dat wordt gebruikt in de evaluatie van het beleid is het openbare IP-adres van de gebruiker. Voor apparaten in een particulier netwerk, is dit niet het client-IP-adres van het apparaat van de gebruiker op het intranet, dit is het adres dat wordt gebruikt door het netwerk verbinding maken met het openbare internet. Als uw apparaat alleen een IPv6-adres heeft, wordt het configureren van de locatievoorwaarde niet ondersteund.

### <a name="bulk-uploading-and-downloading-of-named-locations"></a>Bulksgewijs uploaden en downloaden van benoemde locaties

Bij het maken of bijwerken kunt met de naam locaties voor bulksgewijze updates, u uploaden of downloaden van een CSV-bestand met de IP-adresbereiken. Een upload vervangt de IP-adresbereiken in de lijst met die van het bestand. Elke rij van het bestand bevat één IP-adresbereik in CIDR-indeling. 


### <a name="cloud-proxies-and-vpns"></a>Cloud-proxy's en VPN-verbindingen 

Wanneer u een in de cloud gehoste proxy- of VPN-oplossing, wordt het IP-adres Azure AD gebruikt tijdens het evalueren van een beleid is het IP-adres van de proxy. De header X-Forwarded-For (XFF) met het openbare IP-adres van de gebruiker wordt niet gebruikt omdat er geen validatie dat het afkomstig van een vertrouwde bron is is, zodat een methode voor een IP-adres faking opleveren. 

Wanneer een cloudproxy is, een beleid dat wordt gebruikt om te vereisen dat een apparaat dat lid is domein kan worden gebruikt of binnen het bedrijfsnetwerk van AD FS.



### <a name="api-support-and-powershell"></a>API-ondersteuning en PowerShell 

API en PowerShell is nog niet ondersteund voor benoemde locaties, of voor beleidsregels voor voorwaardelijke toegang.





## <a name="next-steps"></a>Volgende stappen

- Als u weten hoe u een beleid voor voorwaardelijke toegang configureren wilt, Zie [MFA vereisen voor specifieke apps met voorwaardelijke toegang van Azure Active Directory](app-based-mfa.md).

- Zie [Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](best-practices.md) als u klaar bent om beleid voor voorwaardelijke toegang in Azure Active Directory te configureren. 
