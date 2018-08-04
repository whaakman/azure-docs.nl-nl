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
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/28/2018
ms.author: ergreenl
ms.openlocfilehash: 6be67a92c95ccf1161ffeeb636ee4f998c65fa05
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39503686"
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
| AADDS500 | *Het beheerde domein voor het laatst is gesynchroniseerd met Azure AD op [datum]. Het is mogelijk dat gebruikers kan zich niet aanmelden bij het beheerde domein of groepslidmaatschappen kunnen niet worden gesynchroniseerd met Azure AD.* | [Synchronisatie nog niet heeft plaatsgevonden sinds langere tijd](#aadds500-synchronization-has-not-completed-in-a-while) |
| AADDS501 | *Het beheerde domein is laatste back-ups op [datum].* | [Een back-up nog niet is gemaakt in een tijdje](#aadds501-a-backup-has-not-been-taken-in-a-while) |
| AADDS502 | *Het certificaat voor secure LDAP voor het beheerde domein verloopt op [datum]].* | [Certificaat voor secure LDAP verloopt](active-directory-ds-troubleshoot-ldaps.md#aadds502-secure-ldap-certificate-expiring) |
| AADDS503 | *Het beheerde domein is onderbroken omdat de Azure-abonnement dat is gekoppeld aan het domein niet actief is.* | [Onderbreking vanwege een uitgeschakeld abonnement](#aadds503-suspension-due-to-disabled-subscription) |
| AADDS504 | *Het beheerde domein is onderbroken vanwege een ongeldige configuratie. De service is te beheren, patch, of bijwerken van de domeincontrollers voor uw beheerde domein gedurende een lange periode.* | [Onderbreking vanwege een ongeldige configuratie](#aadds504-suspension-due-to-an-invalid-configuration) |


## <a name="aadds100-missing-directory"></a>AADDS100: Ontbrekende map
**Waarschuwing:**

*De Azure AD-directory die zijn gekoppeld aan uw beheerde domein is mogelijk verwijderd. Het beheerde domein is niet langer in een ondersteunde configuratie. Microsoft kan niet controleren, beheren, patch en synchroniseren van uw beheerde domein.*

**Oplossing:**

Deze fout wordt meestal veroorzaakt door onjuist verplaatsen van uw Azure-abonnement naar een nieuwe Azure AD-directory en het verwijderen van de oude Azure AD-directory die is nog steeds gekoppeld aan Azure AD Domain Services.

Dit is een fout niet kan worden hersteld. Als u wilt oplossen, moet u [verwijderen van uw bestaande beheerde domein](active-directory-ds-disable-aadds.md) en maak deze opnieuw in uw nieuwe adreslijst. Als u problemen bij het verwijderen ondervindt, neem dan contact op met het productteam van Azure Active Directory Domain Services [voor ondersteuning van](active-directory-ds-contact-us.md).

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: Azure AD B2C wordt uitgevoerd in deze map
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

## <a name="aadds500-synchronization-has-not-completed-in-a-while"></a>AADDS500: Synchronisatie is niet voltooid in een tijdje

**Waarschuwing:**

*Het beheerde domein voor het laatst is gesynchroniseerd met Azure AD op [datum]. Het is mogelijk dat gebruikers kan zich niet aanmelden bij het beheerde domein of groepslidmaatschappen kunnen niet worden gesynchroniseerd met Azure AD.*

**Oplossing:**

[Controleer de status van uw domein](active-directory-ds-check-health.md) voor alle waarschuwingen die op problemen in uw configuratie van uw beheerde domein wijzen kunnen. Problemen met de configuratie kunnen soms van Microsoft-mogelijkheid om te synchroniseren van uw beheerde domein te blokkeren. Als u kunt geen waarschuwingen oplossen, wacht twee uur en controleer back-ups maken om te zien als de synchronisatie is voltooid.

Hier volgen enkele veelvoorkomende redenen waarom synchronisatie op de beheerde domeinen stopt:
- Netwerkverbinding is geblokkeerd op het beheerde domein. Lees hoe voor meer informatie over het controleren van uw netwerk voor problemen aan [problemen met Netwerkbeveiligingsgroepen oplossen](active-directory-ds-troubleshoot-nsg.md) en [vereisten voor Azure AD Domain Services](active-directory-ds-networking.md).
-  Wachtwoordsynchronisatie is niet ingesteld of voltooid. Als u Wachtwoordsynchronisatie instelt, lezen [in dit artikel](active-directory-ds-getting-started-password-sync.md).

## <a name="aadds501-a-backup-has-not-been-taken-in-a-while"></a>AADDS501: Een back-up niet is gemaakt in een tijdje

**Waarschuwing:**

*Het beheerde domein is laatste back-ups op [datum].*

**Oplossing:**

[Controleer de status van uw domein](active-directory-ds-check-health.md) voor alle waarschuwingen die op problemen in uw configuratie van uw beheerde domein wijzen kunnen. Problemen met de configuratie kunnen soms van Microsoft-mogelijkheid om te synchroniseren van uw beheerde domein te blokkeren. Als u kunt geen waarschuwingen oplossen, wacht twee uur en controleer back-ups maken om te zien als de synchronisatie is voltooid.


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
