---
title: 'Azure Active Directory Domain Services: Waarschuwingen oplossen | Microsoft Docs'
description: Waarschuwingen voor Azure AD Domain Services oplossen
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2018
ms.author: ergreenl
ms.openlocfilehash: c71528ed8453bcde05e29eb609ca2cde64bad8de
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57309414"
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Azure AD Domain Services - waarschuwingen oplossen
Dit artikel bevat de handleidingen voor probleemoplossing voor alle waarschuwingen die op uw beheerde domein optreden kunnen.


Kies de stappen voor probleemoplossing die overeenkomen met de ID of het bericht in de waarschuwing.

| **Waarschuwings-ID** | **Waarschuwingsbericht** | **Resolutie** |
| --- | --- | :--- |
| AADDS001 | *Secure LDAP via internet is ingeschakeld voor het beheerde domein. Toegang tot poort 636 is echter niet vergrendeld met behulp van een netwerkbeveiligingsgroep. Dit kan een beveiligingsrisico voor gebruikersaccounts in het beheerde domein voor wachtwoord brute-force-aanvallen.* | [Onjuiste beveiligde LDAP-configuratie](active-directory-ds-troubleshoot-ldaps.md) |
| AADDS100 | *De Azure AD-directory die zijn gekoppeld aan uw beheerde domein is mogelijk verwijderd. Het beheerde domein is niet langer in een ondersteunde configuratie. Microsoft kan niet controleren, beheren, patch en synchroniseren van uw beheerde domein.* | [Ontbrekende map](#aadds100-missing-directory) |
| AADDS101 | *Azure AD Domain Services kan niet worden ingeschakeld in een Azure AD B2C-Directory.* | [Azure AD B2C wordt uitgevoerd in deze map](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Een Service-Principal die is vereist voor Azure AD Domain Services te laten functioneren is verwijderd uit uw Azure AD-directory. Deze configuratie heeft gevolgen voor de mogelijkheid van Microsoft om te controleren, beheren, toepassen van patches en synchroniseren van uw beheerde domein.* | [Service-Principal ontbreekt](active-directory-ds-troubleshoot-service-principals.md) |
| AADDS103 | *Het IP-adresbereik voor het virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld, wordt in een openbare IP-adresbereik. Azure AD Domain Services moeten zijn ingeschakeld in een virtueel netwerk met een privé IP-adresbereik. Deze configuratie heeft gevolgen voor de mogelijkheid van Microsoft om te controleren, beheren, patch en synchroniseren van uw beheerde domein.* | [Adres bevindt zich in een openbare IP-adresbereik](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *Microsoft kan niet tot de domeincontrollers voor dit beheerde domein. Dit kan gebeuren als een netwerkbeveiligingsgroep (NSG) geconfigureerd op uw virtuele netwerk blokkeert de toegang tot het beheerde domein. Een andere mogelijke reden is als er een door de gebruiker gedefinieerde route blokkeert binnenkomend verkeer van internet is.* | [Netwerkfout](active-directory-ds-troubleshoot-nsg.md) |
| AADDS105 | *De service-principal met de toepassings-ID "d87dcbc6-a371-462e-88e3-28ad15ec4e64" is verwijderd en vervolgens opnieuw gemaakt. De recreatie blijven achter inconsistent machtigingen op Azure AD Domain Services-resources die nodig zijn om uw beheerde domein te behouden. Synchronisatie van wachtwoorden in uw beheerde domein kan worden beïnvloed.* | [De toepassing van de synchronisatie van wachtwoord is verlopen](active-directory-ds-troubleshoot-service-principals.md#alert-aadds105-password-synchronization-application-is-out-of-date) |
| AADDS106 | *Uw Azure-abonnement dat is gekoppeld aan uw beheerde domein is verwijderd.  Azure AD Domain Services moet een actief abonnement om door te gaan naar behoren werkt.* | [Azure-abonnement is niet gevonden](#aadds106-your-azure-subscription-is-not-found) |
| AADDS107 | *Uw Azure-abonnement dat is gekoppeld aan uw beheerde domein is niet actief.  Azure AD Domain Services moet een actief abonnement om door te gaan naar behoren werkt.* | [Azure-abonnement is uitgeschakeld](#aadds107-your-azure-subscription-is-disabled) |
| AADDS108 | *Het abonnement dat is gebruikt door Azure AD Domain Services is verplaatst naar een andere map. Azure AD Domain Services moet een actief abonnement hebt in dezelfde map te laten functioneren.* | [Abonnement verplaatst mappen](#aadds108-subscription-moved-directories) |
| AADDS109 | *Een resource die wordt gebruikt voor uw beheerde domein is verwijderd. Deze resource is nodig voor Azure AD Domain Services te laten functioneren.* | [Een resource is verwijderd](#aadds109-resources-for-your-managed-domain-cannot-be-found) |
| AADDS110 | *Het subnet dat is geselecteerd voor de implementatie van Azure AD Domain Services is vol en heeft geen ruimte voor de extra domeincontroller die moet worden gemaakt.* | [Subnet is vol.](#aadds110-the-subnet-associated-with-your-managed-domain-is-full) |
| AADDS111 | *Een service-principal die gebruikmaakt van Azure AD Domain Services voor uw domein is niet gemachtigd om resources op het Azure-abonnement te beheren. De service-principal moet machtigingen voor het afhandelen van uw beheerde domein te krijgen.* | [Service-principal niet toegestaan](#aadds111-service-principal-unauthorized) |
| AADDS112 | *We hebben geïdentificeerd dat het subnet van het virtuele netwerk in dit domein mogelijk niet voldoende IP-adressen. Azure AD Domain Services moet ten minste twee beschikbare IP-adressen binnen het subnet is ingeschakeld in. We raden u aan ten minste 3 tot 5 extra IP-adressen binnen het subnet. Dit wordt mogelijk veroorzaakt als andere virtuele machines binnen het subnet, dus het toewijzen van het aantal beschikbare IP-adressen of als er een beperking voor het aantal beschikbare IP-adressen in het subnet is worden geïmplementeerd.* | [Er is onvoldoende IP-adressen](#aadds112-not-enough-ip-address-in-the-managed-domain) |
| AADDS113 | *De resources die worden gebruikt door Azure AD Domain Services zijn gedetecteerd in een onverwachte status en kunnen niet worden hersteld.* | [Resources zijn niet kan worden hersteld](#aadds113-resources-are-unrecoverable) |
| AADDS114 | *Het subnet dat is geselecteerd voor de implementatie van Azure AD Domain Services is ongeldig en kan niet worden gebruikt.* | [Ongeldig subnet](#aadds114-subnet-invalid) |
| AADDS115 | *Een of meer van de netwerkresources die worden gebruikt door het beheerde domein kan niet worden uitgevoerd op als het doelbereik is vergrendeld.* | [Resources zijn vergrendeld](#aadds115-resources-are-locked) |
| AADDS116 | *Een of meer van de netwerkresources die worden gebruikt door het beheerde domein kan niet worden uitgevoerd op vanwege beleid restriction(s).* | [Resources zijn onbruikbaar.](#aadds116-resources-are-unusable) |
| AADDS500 | *Het beheerde domein voor het laatst is gesynchroniseerd met Azure AD op [datum]. Het is mogelijk dat gebruikers kan zich niet aanmelden bij het beheerde domein of groepslidmaatschappen kunnen niet worden gesynchroniseerd met Azure AD.* | [Synchronisatie nog niet heeft plaatsgevonden sinds langere tijd](#aadds500-synchronization-has-not-completed-in-a-while) |
| AADDS501 | *Het beheerde domein is laatste back-ups op [datum].* | [Een back-up nog niet is gemaakt in een tijdje](#aadds501-a-backup-has-not-been-taken-in-a-while) |
| AADDS502 | *Het certificaat voor secure LDAP voor het beheerde domein verloopt op [datum].* | [Certificaat voor secure LDAP verloopt](active-directory-ds-troubleshoot-ldaps.md#aadds502-secure-ldap-certificate-expiring) |
| AADDS503 | *Het beheerde domein is onderbroken omdat de Azure-abonnement dat is gekoppeld aan het domein niet actief is.* | [Onderbreking vanwege een uitgeschakeld abonnement](#aadds503-suspension-due-to-disabled-subscription) |
| AADDS504 | *Het beheerde domein is onderbroken vanwege een ongeldige configuratie. De service is te beheren, patch, of bijwerken van de domeincontrollers voor uw beheerde domein gedurende een lange periode.* | [Onderbreking vanwege een ongeldige configuratie](#aadds504-suspension-due-to-an-invalid-configuration) |



## <a name="aadds100-missing-directory"></a>AADDS100: Ontbrekende map
**Waarschuwing:**

*De Azure AD-directory die zijn gekoppeld aan uw beheerde domein is mogelijk verwijderd. Het beheerde domein is niet langer in een ondersteunde configuratie. Microsoft kan niet controleren, beheren, patch en synchroniseren van uw beheerde domein.*

**Oplossing:**

Deze fout wordt meestal veroorzaakt door onjuist verplaatsen van uw Azure-abonnement naar een nieuwe Azure AD-directory en het verwijderen van de oude Azure AD-directory die is nog steeds gekoppeld aan Azure AD Domain Services.

Dit is een fout niet kan worden hersteld. Als u wilt oplossen, moet u [verwijderen van uw bestaande beheerde domein](active-directory-ds-disable-aadds.md) en maak deze opnieuw in uw nieuwe adreslijst. Als u problemen bij het verwijderen ondervindt, neem dan contact op met het productteam van Azure Active Directory Domain Services [voor ondersteuning van](active-directory-ds-contact-us.md).

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: Azure AD B2C wordt uitgevoerd in deze directory
**Waarschuwing:**

*Azure AD Domain Services kan niet worden ingeschakeld in een Azure AD B2C-Directory.*

**Oplossing:**

>[!NOTE]
>Om te blijven gebruiken van Azure AD Domain Services, moet u uw Azure AD Domain Services-exemplaar in een niet - Azure AD B2C-directory opnieuw maken.

Volg deze stappen voor het herstellen van uw service:

1. [Verwijderen van uw beheerde domein](active-directory-ds-disable-aadds.md) uit uw bestaande Azure AD-map.
2. Maak een nieuwe map die is niet een Azure AD B2C-directory.
3. Ga als volgt de [aan de slag](active-directory-ds-getting-started.md) handleiding voor het opnieuw maken van een beheerd domein.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: Adres bevindt zich in een openbare IP-adresbereik

**Waarschuwing:**

*Het IP-adresbereik voor het virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld, wordt in een openbare IP-adresbereik. Azure AD Domain Services moeten zijn ingeschakeld in een virtueel netwerk met een privé IP-adresbereik. Deze configuratie heeft gevolgen voor de mogelijkheid van Microsoft om te controleren, beheren, patch en synchroniseren van uw beheerde domein.*

**Oplossing:**

> [!NOTE]
> Om dit probleem op te lossen, moet u uw bestaande beheerde domein verwijderen en opnieuw maken in een virtueel netwerk met een privé IP-adresbereik. Dit proces te verstorend is.

Voordat u begint, lees de **privé IP-v4-adresruimte** in sectie [in dit artikel](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces).

In het virtuele netwerk, kunnen machines aanvragen versturen naar Azure-resources die zich in het hetzelfde IP-adresbereik als die zijn geconfigureerd voor het subnet. Echter omdat het virtuele netwerk is geconfigureerd voor dit bereik, deze aanvragen worden gerouteerd binnen het virtuele netwerk en de beoogde webresources niet bereiken. Deze configuratie kan leiden tot onverwachte fouten met Azure AD Domain Services.

**Als u het IP-adresbereik op internet die in uw virtuele netwerk is geconfigureerd, kunt u deze waarschuwing negeren. Echter, Azure AD Domain Services kunnen niet doorvoeren voor de [SLA](https://azure.microsoft.com/support/legal/sla/active-directory-ds/v1_0/)] met deze configuratie omdat dit tot onverwachte fouten leiden kan.**


1. [Verwijderen van uw beheerde domein](active-directory-ds-disable-aadds.md) uit uw directory.
2. Het IP-adresbereik voor het subnet oplossen
  1. Navigeer naar de [pagina met virtuele netwerken in Azure portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks).
  2. Selecteer het virtuele netwerk dat u wilt gebruiken voor Azure AD Domain Services.
  3. Klik op **adresruimte** onder instellingen
  4. Het adresbereik bijwerken door te klikken op de bestaande adresbereik en te bewerken of een aanvullend adresbereik toe te voegen. Zorg ervoor dat het nieuwe adresbereik in een privé-IP-adresbereik. Sla uw wijzigingen op.
  5. Klik op **subnetten** in de navigatiebalk links.
  6. Klik op het subnet dat u wilt bewerken in de tabel.
  7. Het adresbereik bijwerken en sla de wijzigingen.
3. Ga als volgt [het ophalen van aan de slag met behulp van Azure AD Domain Services-handleiding](active-directory-ds-getting-started.md) om uw beheerde domein opnieuw te maken. Zorg ervoor dat u een virtueel netwerk met een privé IP-adresbereik selecteren.
4. Naar domein voert u uw virtuele machines naar het nieuwe domein [in deze handleiding](active-directory-ds-admin-guide-join-windows-vm-portal.md).
8. Om te controleren of de waarschuwing is opgelost, Controleer de status van uw domein in twee uur.

## <a name="aadds106-your-azure-subscription-is-not-found"></a>AADDS106: Kan uw Azure-abonnement niet vinden

**Waarschuwing:**

*Uw Azure-abonnement dat is gekoppeld aan uw beheerde domein is verwijderd.  Azure AD Domain Services moet een actief abonnement om door te gaan naar behoren werkt.*

**Oplossing:**

Azure AD Domain Services vereist een abonnement op de functie en kan niet worden verplaatst naar een ander abonnement. Omdat het Azure-abonnement waaraan uw beheerde domein is gekoppeld, is verwijderd, moet u opnieuw maken van een Azure-abonnement en Azure AD Domain Services.

1. Maak een Azure-abonnement
2. [Verwijderen van uw beheerde domein](active-directory-ds-disable-aadds.md) uit uw bestaande Azure AD-map.
3. Ga als volgt de [aan de slag](active-directory-ds-getting-started.md) handleiding voor het opnieuw maken van een beheerd domein.

## <a name="aadds107-your-azure-subscription-is-disabled"></a>AADDS107: Het Azure-abonnement is uitgeschakeld

**Waarschuwing:**

*Uw Azure-abonnement dat is gekoppeld aan uw beheerde domein is niet actief.  Azure AD Domain Services moet een actief abonnement om door te gaan naar behoren werkt.*

**Oplossing:**


1. [Uw Azure-abonnement verlengen](https://docs.microsoft.com/azure/billing/billing-subscription-become-disable).
2. Nadat het abonnement wordt verlengd, wordt Azure AD Domain Services een melding ontvangen van Azure naar uw beheerde domein opnieuw in te schakelen.

## <a name="aadds108-subscription-moved-directories"></a>AADDS108: Abonnement verplaatst mappen

**Waarschuwing:**

*Het abonnement dat is gebruikt door Azure AD Domain Services is verplaatst naar een andere map. Azure AD Domain Services moet een actief abonnement hebt in dezelfde map te laten functioneren.*

**Oplossing:**

Kunt u het abonnement dat is gekoppeld aan Azure AD Domain Services terug naar de vorige map verplaatsen, of u moet [verwijderen van uw beheerde domein](active-directory-ds-disable-aadds.md) uit de bestaande map en maak deze opnieuw in de gekozen map (met een nieuw abonnement of wijzig de map wordt uw Azure AD Domain Services-exemplaar is in).

## <a name="aadds109-resources-for-your-managed-domain-cannot-be-found"></a>AADDS109: Kan de resources voor het beheerde domein niet vinden

**Waarschuwing:**

*Een resource die wordt gebruikt voor uw beheerde domein is verwijderd. Deze resource is nodig voor Azure AD Domain Services te laten functioneren.*

**Oplossing:**

Azure AD Domain Services maakt specifieke resources tijdens de implementatie om te laten functioneren, met inbegrip van openbare IP-adressen, NIC's en een load balancer. Als een van de benoemde worden verwijderd, wordt dit zorgt ervoor dat uw beheerde domein zich in een niet-ondersteunde status en voorkomt u dat uw domein wordt beheerd. Deze waarschuwing wordt gevonden wanneer iemand die dit kan de Azure AD Domain Services-resources bewerken een benodigde bron verwijdert. De volgende stappen beschrijven het herstellen van uw beheerde domein.

1.  Navigeer naar de health-pagina van Azure AD Domain Services
  1.    Reis naar de [pagina Azure AD Domain Services](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) in Azure portal.
  2.    Klik in de navigatiebalk links op **Health**
2.  Controleer of de waarschuwing is minder dan 4 uur
  1.    Klik op de waarschuwing met de ID van de health-pagina **AADDS109**
  2.    De waarschuwing heeft een tijdstempel voor wanneer het eerst is gevonden. Als dat tijdstempel minder dan 4 uur geleden is, is er een kans dat Azure AD Domain Services opnieuw van de verwijderde resourcegroep maken kunt.
3.  Als de waarschuwing meer dan 4 uur is, wordt het beheerde domein heeft een onherstelbare status. U moet verwijderen en opnieuw maken van Azure AD Domain Services.


## <a name="aadds110-the-subnet-associated-with-your-managed-domain-is-full"></a>AADDS110: Het subnet dat is gekoppeld aan uw beheerde domein is vol.

**Waarschuwing:**

*Het subnet dat is geselecteerd voor de implementatie van Azure AD Domain Services is vol en heeft geen ruimte voor de extra domeincontroller die moet worden gemaakt.*

**Oplossing:**

Dit is een fout niet kan worden hersteld. Als u wilt oplossen, moet u [verwijderen van uw bestaande beheerde domein](active-directory-ds-disable-aadds.md) en [opnieuw maken van uw beheerde domein](active-directory-ds-getting-started.md)

## <a name="aadds111-service-principal-unauthorized"></a>AADDS111: Service-principal niet toegestaan

**Waarschuwing:**

*Een service-principal die gebruikmaakt van Azure AD Domain Services voor uw domein is niet gemachtigd om resources op het Azure-abonnement te beheren. De service-principal moet machtigingen voor het afhandelen van uw beheerde domein te krijgen.*

**Oplossing:**

Onze service-principals moeten toegang hebben om te kunnen beheren en maken van resources in uw beheerde domein. Iemand heeft de service-principal toegang geweigerd en nu is het om resources te beheren. Volg de stappen voor het verlenen van toegang tot uw service-principal.

1. Meer informatie over [RBAC controle en het verlenen van toegang tot toepassingen in Azure portal](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)
2. De toegang beoordelen die de service-principal met de ID ```abba844e-bc0e-44b0-947a-dc74e5d09022``` en de toegang is geweigerd op een eerder tijdstip.


## <a name="aadds112-not-enough-ip-address-in-the-managed-domain"></a>AADDS112: Onvoldoende IP-adressen in het beheerde domein

**Waarschuwing:**

*We hebben geïdentificeerd dat het subnet van het virtuele netwerk in dit domein mogelijk niet voldoende IP-adressen. Azure AD Domain Services moet ten minste twee beschikbare IP-adressen binnen het subnet is ingeschakeld in. We raden u aan ten minste 3 tot 5 extra IP-adressen binnen het subnet. Dit wordt mogelijk veroorzaakt als andere virtuele machines binnen het subnet, dus het toewijzen van het aantal beschikbare IP-adressen of als er een beperking voor het aantal beschikbare IP-adressen in het subnet is worden geïmplementeerd.*

**Oplossing:**

1. Uw beheerde domein verwijderen uit uw tenant.
2. Het IP-adresbereik voor het subnet oplossen
  1. Navigeer naar de [pagina met virtuele netwerken in Azure portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks).
  2. Selecteer het virtuele netwerk dat u wilt gebruiken voor Azure AD Domain Services.
  3. Klik op **adresruimte** onder instellingen
  4. Het adresbereik bijwerken door te klikken op de bestaande adresbereik en te bewerken of een aanvullend adresbereik toe te voegen. Sla uw wijzigingen op.
  5. Klik op **subnetten** in de navigatiebalk links.
  6. Klik op het subnet dat u wilt bewerken in de tabel.
  7. Het adresbereik bijwerken en sla de wijzigingen.
3. Ga als volgt [het ophalen van aan de slag met behulp van Azure AD Domain Services-handleiding](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started) om uw beheerde domein opnieuw te maken. Zorg ervoor dat u een virtueel netwerk met een privé IP-adresbereik selecteren.
4. Naar domein voert u uw virtuele machines naar het nieuwe domein [in deze handleiding](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-join-windows-vm-portal).
5. Controleer de status van uw domein in twee uur om ervoor te zorgen dat u de stappen correct hebt voltooid.

## <a name="aadds113-resources-are-unrecoverable"></a>AADDS113: Resources zijn niet kan worden hersteld

**Waarschuwing:**

*De resources die worden gebruikt door Azure AD Domain Services zijn gedetecteerd in een onverwachte status en kunnen niet worden hersteld.*

**Oplossing:**

Dit is een fout niet kan worden hersteld. Als u wilt oplossen, moet u [verwijderen van uw bestaande beheerde domein](active-directory-ds-disable-aadds.md) en [opnieuw maken van uw beheerde domein](active-directory-ds-getting-started.md).

## <a name="aadds114-subnet-invalid"></a>AADDS114: Ongeldig subnet

**Waarschuwing:**

*Het subnet dat is geselecteerd voor de implementatie van Azure AD Domain Services is ongeldig en kan niet worden gebruikt.*

**Oplossing:**

Dit is een fout niet kan worden hersteld. Als u wilt oplossen, moet u [verwijderen van uw bestaande beheerde domein](active-directory-ds-disable-aadds.md) en [opnieuw maken van uw beheerde domein](active-directory-ds-getting-started.md).

## <a name="aadds115-resources-are-locked"></a>AADDS115: Resources zijn vergrendeld

**Waarschuwing:**

*Een of meer van de netwerkresources die worden gebruikt door het beheerde domein kan niet worden uitgevoerd op als het doelbereik is vergrendeld.*

**Oplossing:**

1.  Beoordeling Resource Manager-bewerking logboeken op de netwerkbronnen (deze informatie geeft op welke vergrendeling houdt gewijzigd).
2.  Verwijder de vergrendeling op de resources zodat de service-principal voor Azure AD Domain Services kan worden uitgevoerd op deze.

## <a name="aadds116-resources-are-unusable"></a>AADDS116: Resources zijn onbruikbaar.

**Waarschuwing:**

*Een of meer van de netwerkresources die worden gebruikt door het beheerde domein kan niet worden uitgevoerd op vanwege beleid restriction(s).*

**Oplossing:**

1.  Beoordeling Resource Manager, bewerkingslogboeken op de netwerkbronnen voor uw beheerde domein.
2.  De beperkingen van het beleid op de resources verzwakken zodat de AAD-DS-service-principal op deze kan werken.



## <a name="aadds500-synchronization-has-not-completed-in-a-while"></a>AADDS500: Synchronisatie is niet voltooid in een tijdje

**Waarschuwing:**

*Het beheerde domein voor het laatst is gesynchroniseerd met Azure AD op [datum]. Het is mogelijk dat gebruikers kan zich niet aanmelden bij het beheerde domein of groepslidmaatschappen kunnen niet worden gesynchroniseerd met Azure AD.*

**Oplossing:**

[Controleer de status van uw domein](active-directory-ds-check-health.md) voor alle waarschuwingen die op problemen in uw configuratie van uw beheerde domein wijzen kunnen. Problemen met de configuratie kunnen soms van Microsoft-mogelijkheid om te synchroniseren van uw beheerde domein te blokkeren. Als u kunt geen waarschuwingen oplossen, wacht twee uur en controleer back-ups maken om te zien als de synchronisatie is voltooid.

Hier volgen enkele veelvoorkomende redenen waarom synchronisatie op de beheerde domeinen stopt:
- Netwerkverbinding is geblokkeerd op het beheerde domein. Lees hoe voor meer informatie over het controleren van uw netwerk voor problemen aan [problemen met Netwerkbeveiligingsgroepen oplossen](active-directory-ds-troubleshoot-nsg.md) en [vereisten voor Azure AD Domain Services](active-directory-ds-networking.md).
-  Wachtwoordsynchronisatie is niet ingesteld of voltooid. Als u Wachtwoordsynchronisatie instelt, lezen [in dit artikel](active-directory-ds-getting-started-password-sync.md).

## <a name="aadds501-a-backup-has-not-been-taken-in-a-while"></a>AADDS501: In een tijdje is niet een back-up gemaakt

**Waarschuwing:**

*Het beheerde domein is laatste back-ups op [datum].*

**Oplossing:**

[Controleer de status van uw domein](active-directory-ds-check-health.md) voor alle waarschuwingen die op problemen in uw configuratie van uw beheerde domein wijzen kunnen. Problemen met de configuratie kunnen soms van Microsoft back-up van uw beheerde domein kunnen blokkeren. Als u kunt geen waarschuwingen oplossen, wacht twee uur en controleer back-ups maken om te zien als de back-up is voltooid.


## <a name="aadds503-suspension-due-to-disabled-subscription"></a>AADDS503: Onderbreking vanwege een uitgeschakeld abonnement

**Waarschuwing:**

*Het beheerde domein is onderbroken omdat de Azure-abonnement dat is gekoppeld aan het domein niet actief is.*

**Oplossing:**

> [!WARNING]
> Als uw beheerde domein is onderbroken voor een lange periode, is het gevaar wordt verwijderd. Het is raadzaam om de onderbreking zo snel mogelijk. Meer informatie gaat u naar [in dit artikel](active-directory-ds-suspension.md).

Om terug te zetten van uw service, [uw Azure-abonnement verlengen](https://docs.microsoft.com/azure/billing/billing-subscription-become-disable) die zijn gekoppeld aan uw beheerde domein.

## <a name="aadds504-suspension-due-to-an-invalid-configuration"></a>AADDS504: Onderbreking vanwege een ongeldige configuratie

**Waarschuwing:**

*Het beheerde domein is onderbroken vanwege een ongeldige configuratie. De service is te beheren, patch, of bijwerken van de domeincontrollers voor uw beheerde domein gedurende een lange periode.*

**Oplossing:**

> [!WARNING]
> Als uw beheerde domein is onderbroken voor een lange periode, is het gevaar wordt verwijderd. Het is raadzaam om de onderbreking zo snel mogelijk. Meer informatie gaat u naar [in dit artikel](active-directory-ds-suspension.md).

[Controleer de status van uw domein](active-directory-ds-check-health.md) voor alle waarschuwingen die op problemen in uw configuratie van uw beheerde domein wijzen kunnen. Als u een van deze waarschuwingen oplossen kunt, doen. Nadat de contact op met ondersteuning voor uw abonnement opnieuw activeren.


## <a name="contact-us"></a>Contact opnemen
Neem contact op met het productteam van Azure Active Directory Domain Services naar [feedback geven of voor ondersteuning van](active-directory-ds-contact-us.md).
