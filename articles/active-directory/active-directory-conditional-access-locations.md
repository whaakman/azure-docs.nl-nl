---
title: Voorwaarden van de locatie van voorwaardelijke toegang van Azure Active Directory | Microsoft Docs
description: Informatie over het gebruik van de voorwaarde locatie toegang tot uw cloud-apps op basis van de netwerklocatie van de gebruiker.
services: active-directory
keywords: voorwaardelijke toegang tot apps, voorwaardelijke toegang met Azure AD, beveiligde toegang tot bedrijfsresources, beleidsregels voor voorwaardelijke toegang
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/11/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 47b4d70c991bd618ea4ea6e5d2fd1dea86911798
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="location-conditions-in-azure-active-directory-conditional-access"></a>Voorwaarden van de locatie van voorwaardelijke toegang van Azure Active Directory 

Met [voorwaardelijke toegang van Azure Active Directory (Azure AD)](active-directory-conditional-access-azure-portal.md), kunt u bepalen hoe gemachtigde gebruikers hebben toegang tot uw cloud-apps. De voorwaarde van de locatie van een beleid voor voorwaardelijke toegang kunt u deze instellingen naar de netwerklocaties van uw gebruikers toegang.

In dit artikel biedt u informatie die u nodig hebt om de locatie-voorwaarde te configureren. 

## <a name="locations"></a>Locaties

Azure AD, schakelt u eenmalige aanmelding op op apparaten, apps en services vanaf elke locatie op het openbare internet. U kunt de toegang tot uw cloud-apps op basis van de netwerklocatie van een gebruiker beheren met de voorwaarde locatie. Algemene gebruiksvoorbeelden voor de voorwaarde locatie zijn:

- Meervoudige verificatie vereisen voor gebruikers toegang krijgen tot een service wanneer ze buiten het bedrijfsnetwerk vallen.  

- Blokkeert de toegang voor gebruikers die een service vanuit specifieke landen of regio's. 

Een locatie is een label voor een netwerklocatie van de multi-factor authentication-server of een benoemde locatie vertegenwoordigt IP-adressen goedgekeurde.


## <a name="named-locations"></a>Benoemde locaties 

U kunt logische groeperingen van IP-adresbereiken, landen en regio's te maken met benoemde locaties. 

 De locatie van een naam heeft de volgende onderdelen:

![Locaties](./media/active-directory-conditional-access-locations/42.png)

- **Naam** -de weergavenaam van een benoemde locatie.

- **IP-adresbereiken** -adres van een of meer IP-adresbereiken in CIDR-notatie.

- **Markeren als vertrouwde locatie** -een vlag die u kunt instellen voor een benoemde locatie om aan te geven van een vertrouwde locatie. Vertrouwde locaties zijn meestal netwerkgebieden die worden beheerd door uw IT-afdeling. Naast de voorwaardelijke toegang vertrouwde benoemde locaties worden ook gebruikt door Azure Identity Protection en Azure AD beveiligingsrapporten te verminderen [valse positieven](active-directory-reporting-risk-events.md#impossible-travel-to-atypical-locations-1).

- **Land / regio's** -deze optie kunt u een of meer land of regio voor het definiëren van een benoemde locatie selecteren. 

- **Onbekende gebieden omvatten** -Some IP-adressen zijn niet toegewezen aan een bepaald land. Deze optie kunt u kiezen of deze IP-adressen moeten worden opgenomen in de benoemde locatie. Wanneer het beleid met behulp van de benoemde locatie met onbekende locaties overeenkomen moet, kan deze controle worden.

Het aantal benoemde locaties die u kunt configureren, wordt beperkt door de grootte van het gerelateerde object in Azure AD. U kunt configureren:

- De naam van een locatie met maximaal 1200 IP-adresbereiken.

- Maximaal 90 benoemde locaties met één IP-bereik dat is toegewezen aan elk van deze.




## <a name="trusted-ips"></a>Goedgekeurde IP-adressen

U kunt ook configureren met IP-adresbereiken van uw organisatie lokaal intranet in voor de [instellingen voor multi-factor authentication-service](https://account.activedirectory.windowsazure.com/usermanagement/mfasettings.aspx). Deze functie kunt u maximaal 50 IP-adresbereiken configureren. Er zijn de IP-adresbereiken in CIDR-notatie. Zie voor meer informatie [goedgekeurde IP-adressen](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).  

Als u IP-adressen geconfigureerd vertrouwde een, ze worden weergegeven als **MFA goedgekeurde IP-Adressen** in de lijst met locaties voor de locatie-voorwaarde.   

### <a name="skipping-multi-factor-authentication"></a>Meervoudige verificatie overslaan

U kunt op de instellingenpagina van multi-factor authentication-service bedrijfsintranet gebruikers identificeren door te selecteren **multi-factor authentication overslaan voor aanvragen van federatieve gebruikers op mijn intranet**. Deze instelling geeft aan dat de zakelijke binnen claim, dat is uitgegeven door AD FS, netwerk moeten worden vertrouwd en gebruikt om de gebruiker als in het bedrijfsnetwerk te identificeren. Zie voor meer informatie [de goedgekeurde IP-adressen inschakelen met behulp van voorwaardelijke toegang](../multi-factor-authentication/multi-factor-authentication-whats-next.md#enable-the-trusted-ips-feature-by-using-conditional-access).

Nadat u deze optie inschakelt, met inbegrip van de benoemde locatie **MFA goedgekeurde IP-Adressen** voor elk beleid wordt toegepast met dit geselecteerd.

Voorwaardelijke toegang is voor mobiele en bureaublad-toepassingen die hebben lange levensduur van de sessie, regelmatig opnieuw geëvalueerd. De standaardwaarde is één keer per uur. Wanneer de binnen bedrijfsnetwerk claim is en alleen op het moment van de eerste verificatie uitgegeven, Azure AD kan geen een lijst met vertrouwde IP-adresbereiken. In dit geval is het moeilijker om te bepalen of de gebruiker nog steeds op het bedrijfsnetwerk bevinden:

1. Controleer of IP-adres van de gebruiker zich in een van de vertrouwde IP-adresbereiken.

2. Controleer of de eerste drie octetten van IP-adres van de gebruiker overeenkomen met de eerste 3 octetten van het IP-adres van de eerste verificatie. Het IP-adres wordt vergeleken met de eerste verificatie, omdat dit wanneer is de binnen bedrijfsnetwerk claim oorspronkelijk uitgegeven en de locatie van de gebruiker is gevalideerd.

Als beide stappen mislukken, een gebruiker wordt beschouwd als niet meer op een goedgekeurde IP-adres.



## <a name="location-condition-configuration"></a>Configuratie van voorwaarde

Wanneer u de locatie-voorwaarde te configureren, hebt u de optie onderscheid maken tussen:

- Elke locatie 
- Alle vertrouwde locaties
- Geselecteerde locaties

![Locaties](./media/active-directory-conditional-access-locations/01.png)

### <a name="any-location"></a>Elke locatie

Standaard selecteren **elke locatie** zorgt ervoor dat een beleid wordt toegepast op alle IP-adressen, wat betekent elk adres op het Internet dat. Deze instelling is niet beperkt tot IP-adressen die u hebt geconfigureerd als benoemde locatie. Wanneer u selecteert **elke locatie**, kunt u nog steeds specifieke locaties uitsluiten van een beleid. U kunt bijvoorbeeld een beleid toepassen op alle locaties excepts vertrouwde locaties voor het instellen van het bereik voor alle locaties, met uitzondering van het bedrijfsnetwerk.

### <a name="all-trusted-locations"></a>Alle vertrouwde locaties

Deze optie is van toepassing op:

- Alle locaties die zijn gemarkeerd als vertrouwde locatie
- **MFA goedgekeurde IP-Adressen** (indien geconfigureerd)


### <a name="selected-locations"></a>Geselecteerde locaties

Met deze optie kunt u een of meer benoemde locaties selecteren. Voor een beleid met deze instelling toe te passen, moet een gebruiker verbinding maken via een van de geselecteerde locaties. U klikt op Whe **selecteren** het benoemde netwerk Selectiebesturingselement waarin de lijst met benoemde netwerken wordt geopend. De lijst worden ook weergegeven als de netwerklocatie is gemarkeerd als vertrouwd. De benoemde locatie aangeroepen **MFA goedgekeurde IP-adressen** wordt gebruikt om op te nemen van de IP-instellingen die kunnen worden geconfigureerd in de pagina multi-factor authentication-service-instelling.

## <a name="what-you-should-know"></a>Wat u moet weten

### <a name="when-is-a-location-evaluated"></a>Wanneer een locatie geëvalueerd?

Beleid voor voorwaardelijke toegang worden geëvalueerd wanneer: 

- Een gebruiker zich in eerste instantie aanmeldt 

- Azure AD geeft een token voor de cloud-app die op het beleid voor voorwaardelijke toegang is ingesteld. 

Azure AD geeft standaard een token op uurbasis. Na het verplaatsen van buiten het bedrijfsnetwerk, binnen een uur wordt het beleid afgedwongen voor toepassingen die moderne authenticatie gebruiken.


### <a name="user-ip-address"></a>IP-gebruikersadres

Het IP-adres dat wordt gebruikt in de evaluatie van het beleid is het openbare IP-adres van de gebruiker. Voor apparaten in een particulier netwerk, dit is niet de client-IP van het apparaat van de gebruiker op het intranet, is het adres dat wordt gebruikt door het netwerk verbinding maken met het openbare internet. 

### <a name="bulk-uploading-and-downloading-of-named-locations"></a>Bulksgewijs uploaden en downloaden van benoemde locaties

Wanneer u maken of bijwerken kunnen met de naam locaties voor bulksgewijze updates uploaden of downloaden van een CSV-bestand met de IP-adresbereiken. Een upload vervangen door de IP-adresbereiken in de lijst die uit het bestand. Elke rij van het bestand bevat één IP-adresbereik in CIDR-notatie. 


### <a name="cloud-proxies-and-vpns"></a>Cloud-proxy's en VPN-verbindingen 

Wanneer u een in de cloud gehoste proxy of VPN-oplossing gebruikt, wordt het IP-adres Azure AD gebruikt tijdens het evalueren van een beleid is het IP-adres van de proxy. De header X-Forwarded-For (XFF) met de gebruikers die openbare IP-adres niet gebruikt wordt omdat er geen validatie dat ze afkomstig zijn van een vertrouwde bron, dus opleveren een methode voor een IP-adres faking. 

Wanneer een cloud-proxy is, een beleid dat wordt gebruikt voor het vereisen van een apparaat aan een domein kan worden gebruikt of de binnen corpnet claim vanaf AD FS.



### <a name="api-support-and-powershell"></a>API-ondersteuning en PowerShell 

API en PowerShell is nog niet ondersteund voor benoemde locaties of voor beleid voor voorwaardelijke toegang.





## <a name="next-steps"></a>Volgende stappen

- Als u weten hoe beleid voor voorwaardelijke toegang configureren wilt, Zie [aan de slag met voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-azure-portal-get-started.md).

- Als u klaar om te configureren van beleidsregels voor voorwaardelijke toegang voor uw omgeving bent, Zie de [best practices voor voorwaardelijke toegang in Azure Active Directory](active-directory-conditional-access-best-practices.md). 
