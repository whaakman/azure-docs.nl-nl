---
title: Versiegeschiedenis van Azure AD Connect Health
description: Dit document beschrijft de versies voor Azure AD Connect Health en wat is opgenomen in deze versies.
services: active-directory
documentationcenter: 
author: karavar
manager: mtillman
editor: curtand
ms.assetid: 8dd4e998-747b-4c52-b8d3-3900fe77d88f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: b43eb5e78b70f38226e3e8cb53d1530d348c7c20
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="azure-ad-connect-health-version-release-history"></a>Versiegeschiedenis van status Azure AD Connect
Azure AD Connect Health het team van Azure Active Directory regelmatig bijgewerkt met nieuwe functies en functionaliteit. Dit artikel worden de versies en onderdelen die zijn uitgebracht.

## <a name="december-2017"></a>December 2017
**Agentupdate:**

*   Azure AD Connect Health-agent voor AD DS (versie 3.0.145.0)
  1. Verbeteringen van de agent-beschikbaarheid 
  2. Toegevoegde nieuwe agent voor probleemoplossing opdrachten
  3. Algemene verbeteringen en oplossingen voor problemen
*   Azure AD Connect Health-agent voor AD FS (versie 3.0.145.0)
  1. Toegevoegde nieuwe agent voor probleemoplossing opdrachten
  2. Verbeteringen van de agent-beschikbaarheid 
  3. Algemene verbeteringen en oplossingen voor problemen
  
## <a name="october-2017"></a>Oktober 2017
**Agentupdate:**

 * Azure AD Connect Health-agent voor synchronisatie (versie 3.0.129.0) uitgebracht met Azure AD Connect versie 1.1.649.0
<br></br> Een compatibiliteitsprobleem versie tussen Azure AD Connect en Azure AD Connect Health-Agent voor synchronisatie, vast. Dit probleem is van invloed op klanten die Azure AD Connect in-place upgrade naar versie 1.1.647.0 uitvoert, maar heeft momenteel Health Agent versie 3.0.127.0. Na de upgrade kan de Health-Agent niet meer statusgegevens over Azure AD Connect-synchronisatieservice verzenden naar Azure AD Health-Service. Met deze oplossing wordt Health Agent versie 3.0.129.0 geïnstalleerd tijdens een upgrade ter plekke van Azure AD Connect. Health-Agent versie 3.0.129.0 heeft geen compatibiliteitsprobleem met Azure AD Connect versie 1.1.649.0.

## <a name="july-2017"></a>Juli 2017
**Agentupdate:**

*   Azure AD Connect Health-agent voor AD DS (versie 3.0.68.0)
  1. Algemene verbeteringen en oplossingen voor problemen
  2. Soevereine cloud-ondersteuning
*   Azure AD Connect Health-agent voor AD FS (versie 3.0.68.0)
  1. Algemene verbeteringen en oplossingen voor problemen
  2. Soevereine cloud-ondersteuning
* Azure AD Connect Health-agent voor synchronisatie (versie 3.0.68.0) uitgebracht met Azure AD Connect versie 1.1.614.0
1. Ondersteuning voor Microsoft Azure Government Cloud en Microsoft Cloud Duitsland

## <a name="april-2017"></a>April 2017      
**Agentupdate:**

*   Azure AD Connect Health-agent voor AD FS (versie 3.0.12.0)
  1. Algemene verbeteringen en oplossingen voor problemen
*   Azure AD Connect Health-agent voor AD DS (versie 3.0.12.0)
  1. Prestatiemeteritems uploaden verbeteringen
  2. Algemene verbeteringen en oplossingen voor problemen

## <a name="october-2016"></a>Oktober 2016
**Agentupdate:**

* Azure AD Connect Health-agent voor AD FS \(versie 2.6.408.0\)
1. Verbeteringen in het client-IP-adressen in verificatieaanvragen detecteren
2. Oplossingen voor problemen die zijn gerelateerd aan waarschuwingen
* Azure AD Connect Health-agent voor AD DS (versie 2.6.408.0)
1. Oplossingen voor problemen gerelateerd aan waarschuwingen.
* Azure AD Connect Health-agent voor synchronisatie (versie 2.6.353.0) uitgebracht met Azure AD Connect versie 1.1.281.0
1. Geef de vereiste gegevens voor de foutenrapporten voor synchronisatie
2. Oplossingen voor problemen betrekking hebben op waarschuwingen

**Nieuwe preview-functies:**

* Synchronisatie van foutrapporten voor Azure AD Connect

**Nieuwe functies:**

* Azure AD Connect Health voor AD FS - veld IP-adres is beschikbaar in het rapport over top 50 van gebruikers met een onjuiste gebruikersnaam en wachtwoord.

## <a name="july-2016"></a>Juli 2016
**Nieuwe preview-functies:**

* [Azure AD Connect Health voor AD DS](active-directory-aadconnect-health-adds.md).

## <a name="january-2016"></a>Januari 2016
**Agentupdate:**

* Azure AD Connect Health-agent voor AD FS (versie 2.6.91.1512)

**Nieuwe functies:**

* [Hulpprogramma voor test-connectiviteit voor Azure AD Connect Health-Agents](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service)

## <a name="november-2015"></a>November 2015
**Nieuwe functies:**

* Ondersteuning voor [toegangsbeheer op basis van rollen](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control)

**Nieuwe preview-functies:**

* [Azure AD Connect Health voor synchroniseren](active-directory-aadconnect-health-sync.md).

**Opgeloste problemen:**

* Oplossingen voor fouten weergegeven tijdens agent registraties.

## <a name="september-2015"></a>September 2015
**Nieuwe functies:**

* Onjuiste gebruikersnaam wachtwoord rapport voor AD FS
* Ondersteuning voor het configureren van niet-geverifieerde HTTP-Proxy
* Ondersteuning voor het configureren van de agent op Server core
* Verbeteringen aan waarschuwingen voor AD FS
* Verbeteringen in Azure AD Connect Health-Agent voor AD FS voor de verbinding en gegevens uploaden.

**Opgeloste problemen:**

* Oplossingen voor problemen in inzicht in het gebruik voor AD FS-fouttypen.

## <a name="june-2015"></a>Juni 2015
**Initiële versie van Azure AD Connect Health voor AD FS en AD FS-Proxy.**

**Nieuwe functies:**

* Waarschuwingen voor het controleren van AD FS en AD FS-Proxy-servers met e-mailmeldingen.
* Eenvoudige toegang tot AD FS-topologie en patronen in AD FS-prestatiemeteritems.
* Trend van geslaagde aanvragen voor beveiligingstokens op AD FS-servers die zijn gegroepeerd op toepassingen, verificatiemethoden, aanvragen netwerk locatie enzovoort.
* Ontwikkeling van op AD FS-servers die zijn gegroepeerd op toepassingen, fout typen enzovoort van mislukte aanvragen.
* Eenvoudigere implementatie van Agent met behulp van globale beheerder van Azure AD-referenties.  

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [uw lokale identiteit infrastructuur en de synchronisatie-services bewaken in de cloud](active-directory-aadconnect-health.md).

