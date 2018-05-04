---
title: 'Azure Active Directory Domain Services: Waarschuwingen oplossen | Microsoft Docs'
description: Waarschuwingen voor Azure AD Domain Services oplossen
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: ergreenl
ms.openlocfilehash: 0c8fc2551f529fbff647d3400144fa2a9600bbd9
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Azure AD Domain Services - waarschuwingen oplossen
Dit artikel bevat richtlijnen voor probleemoplossing voor de waarschuwingen die op uw beheerde domein optreden kunnen.


Kies de stappen voor probleemoplossing die overeenkomen met de ID of het bericht in de waarschuwing.

| **Waarschuwing-ID** | **Waarschuwingsbericht** | **Naamomzetting** |
| --- | --- | :--- |
| AADDS001 | *Beveiligde LDAP via internet is ingeschakeld voor het beheerde domein. Echter, toegang tot poort 636 is niet vergrendeld met behulp van een netwerkbeveiligingsgroep. Dit kan een beveiligingsrisico voor gebruikersaccounts op het beheerde domein aanvallen met brute kracht wachtwoord.* | [Onjuiste beveiligde LDAP-configuratie](active-directory-ds-troubleshoot-ldaps.md) |
| AADDS100 | *De Azure AD-directory die is gekoppeld aan uw beheerde domein is mogelijk verwijderd. Het beheerde domein is niet langer in een ondersteunde configuratie. Microsoft kan niet controleren, beheren, patch en synchroniseren van uw beheerde domein.* | [Ontbrekende map](#aadds100-missing-directory) |
| AADDS101 | *Azure AD Domain Services kan niet worden ingeschakeld in een Azure AD B2C-Directory.* | [Azure AD B2C wordt uitgevoerd in deze map](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Een Service-Principal die is vereist voor Azure AD Domain Services naar behoren is verwijderd van uw Azure AD-directory. Deze configuratie heeft impact op de mogelijkheid van Microsoft als u wilt bewaken, beheren, patch en synchroniseren van uw beheerde domein.* | [Service-Principal ontbreekt](active-directory-ds-troubleshoot-service-principals.md) |
| AADDS103 | *Het IP-adresbereik voor het virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld is in een openbare IP-adresbereik. Azure AD Domain Services moeten zijn ingeschakeld in een virtueel netwerk met een persoonlijke IP-adresbereik. Deze configuratie heeft impact op de mogelijkheid van Microsoft als u wilt bewaken, beheren, patch en synchroniseren van uw beheerde domein.* | [Adres bevindt zich in een openbare IP-adresbereik](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *Microsoft kan niet tot de domeincontrollers voor dit beheerde domein. Dit kan gebeuren als een netwerkbeveiligingsgroep (NSG) geconfigureerd op uw virtuele netwerk blokkeert de toegang tot het beheerde domein. Een andere mogelijke oorzaak is als er een gebruiker gedefinieerde route blokken binnenkomend verkeer van het internet.* | [Netwerkfout](active-directory-ds-troubleshoot-nsg.md) |
| AADDS105 | *De service-principal met de toepassings-ID 'd87dcbc6-a371-462e-88e3-28ad15ec4e64' is verwijderd en vervolgens opnieuw gemaakt. De opnieuw laat achter inconsistente machtigingen op Azure AD Domain Services-bronnen die nodig zijn voor het onderhouden van uw beheerde domein. Synchronisatie van wachtwoorden in uw beheerde domein kan worden beïnvloed.* | [De toepassing van de synchronisatie wachtwoord is verlopen](active-directory-ds-troubleshoot-service-principals.md#alert-aadds105-password-synchronization-application-is-out-of-date) |
| AADDS500 | *Het beheerde domein voor het laatst is gesynchroniseerd met Azure AD op [datum]. Gebruikers mogelijk niet aanmelden bij het beheerde domein of groepslidmaatschappen mogelijk niet gesynchroniseerd met Azure AD.* | [Synchronisatie nog niet is opgetreden in een tijdje](#aadds500-synchronization-has-not-completed-in-a-while) |
| AADDS501 | *Het beheerde domein laatste back-up op [datum].* | [Een back-up nog niet is gemaakt in een tijdje](#aadds501-a-backup-has-not-been-taken-in-a-while) |
| AADDS502 | *Het beveiligde LDAP-certificaat voor het beheerde domein verloopt op XX.* | [Beveiligde LDAP-certificaat verloopt](active-directory-ds-troubleshoot-ldaps.md#aadds502-secure-ldap-certificate-expiring) |
| AADDS503 | *Het beheerde domein is onderbroken omdat de Azure-abonnement gekoppeld aan het domein niet actief is.* | [Opschorten vanwege een uitgeschakeld abonnement](#aadds503-suspension-due-to-disabled-subscription) |
| AADDS504 | *Het beheerde domein is onderbroken vanwege een ongeldige configuratie. De service is kan beheren, patch, of het bijwerken van de domeincontrollers voor uw beheerde domein gedurende een lange periode.* | [Opschorten vanwege een ongeldige configuratie](#aadds504-suspension-due-to-an-invalid-configuration) |


## <a name="aadds100-missing-directory"></a>AADDS100: Directory ontbreekt
**Waarschuwing:**

*De Azure AD-directory die is gekoppeld aan uw beheerde domein is mogelijk verwijderd. Het beheerde domein is niet langer in een ondersteunde configuratie. Microsoft kan niet controleren, beheren, patch en synchroniseren van uw beheerde domein.*

**Oplossing:**

Deze fout wordt meestal veroorzaakt door onjuist uw Azure-abonnement te verplaatsen naar een nieuwe Azure AD-directory en het verwijderen van de oude Azure AD-directory is nog steeds gekoppeld aan Azure AD Domain Services.

Deze fout is onherstelbaar. Om op te lossen, moet u [uw bestaande beheerde domein verwijderen](active-directory-ds-disable-aadds.md) en maak deze opnieuw in de nieuwe map. Als u problemen bij het verwijderen ondervindt, neem dan contact op met het Azure Active Directory Domain Services-productteam [voor ondersteuning](active-directory-ds-contact-us.md).

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: Azure AD B2C wordt uitgevoerd in deze map
**Waarschuwing:**

*Azure AD Domain Services kan niet worden ingeschakeld in een Azure AD B2C-Directory.*

**Oplossing:**

>[!NOTE]
>Om te blijven gebruiken van Azure AD Domain Services, moet u uw Azure AD Domain Services-exemplaar in een niet - Azure AD B2C-directory opnieuw maken.

Volg deze stappen voor het herstellen van uw service:

1. [Verwijderen van uw beheerde domein](active-directory-ds-disable-aadds.md) van uw bestaande Azure AD-directory.
2. Maak een nieuwe map die niet een Azure AD B2C-directory.
3. Ga als volgt de [aan de slag](active-directory-ds-getting-started.md) handleiding voor het opnieuw maken van een beheerd domein.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: Adres bevindt zich in een openbare IP-adresbereik

**Waarschuwing:**

*Het IP-adresbereik voor het virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld is in een openbare IP-adresbereik. Azure AD Domain Services moeten zijn ingeschakeld in een virtueel netwerk met een persoonlijke IP-adresbereik. Deze configuratie heeft impact op de mogelijkheid van Microsoft als u wilt bewaken, beheren, patch en synchroniseren van uw beheerde domein.*

**Oplossing:**

> [!NOTE]
> Om dit probleem op te lossen, moet u uw bestaande beheerde domein verwijderen en opnieuw maken in een virtueel netwerk met een persoonlijke IP-adresbereik. Dit proces is verstoren.

Lees voordat u begint, de **persoonlijke IP v4-adresruimte** in sectie [in dit artikel](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces).

In het virtuele netwerk mag machines aanvragen maken voor Azure-resources die zich in hetzelfde IP-adresbereik als die zijn geconfigureerd voor het subnet. Echter aangezien het virtuele netwerk is geconfigureerd voor dit bereik, deze aanvragen wordt gerouteerd binnen het virtuele netwerk en niet de beoogde webbronnen wordt bereikt. Deze configuratie kan leiden tot onverwachte fouten met Azure AD Domain Services.

**Als u het IP-adresbereik op internet die in het virtuele netwerk is geconfigureerd, kunt u deze waarschuwing negeren. Echter, Azure AD Domain Services kunnen niet opslaan in de [SLA](https://azure.microsoft.com/support/legal/sla/active-directory-ds/v1_0/)] met deze configuratie omdat dit tot onverwachte fouten leiden kan.**


1. [Verwijderen van uw beheerde domein](active-directory-ds-disable-aadds.md) van uw directory.
2. Corrigeer het IP-adresbereik voor het subnet
  1. Navigeer naar de [pagina van de virtuele netwerken op de Azure-portal](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks).
  2. Selecteer het virtuele netwerk dat u wilt gebruiken voor Azure AD Domain Services.
  3. Klik op **adresruimte** onder instellingen
  4. Werk het adresbereik dat door te klikken op het bestaande adresbereik en te bewerken of een aanvullend adresbereik toe te voegen. Zorg ervoor dat het nieuwe adresbereik in een particulier IP-adresbereik. Sla uw wijzigingen op.
  5. Klik op **subnetten** in de linkernavigatiebalk.
  6. Klik op het subnet dat u wilt bewerken in de tabel.
  7. Werk het adresbereik en sla de wijzigingen.
3. Ga als volgt [de handleiding voor ophalen is gestart met behulp van Azure AD Domain Services](active-directory-ds-getting-started.md) opnieuw maken van uw beheerde domein. Zorg ervoor dat u een virtueel netwerk met een particulier IP-adresbereik kiest.
4. Lid van domein uw virtuele machines naar het nieuwe domein, gaat u als volgt [in deze handleiding](active-directory-ds-admin-guide-join-windows-vm-portal.md).
8. Controleer uw domein health in twee uur zodat de waarschuwing is opgelost.

## <a name="aadds500-synchronization-has-not-completed-in-a-while"></a>AADDS500: Synchronisatie is niet voltooid in een tijdje

**Waarschuwing:**

*Het beheerde domein voor het laatst is gesynchroniseerd met Azure AD op [datum]. Gebruikers mogelijk niet aanmelden bij het beheerde domein of groepslidmaatschappen mogelijk niet gesynchroniseerd met Azure AD.*

**Oplossing:**

[Controleer de status van uw domein](active-directory-ds-check-health.md) voor alle waarschuwingen die kunnen wijzen op problemen in uw configuratie van uw beheerde domein. Problemen met de configuratie kunnen soms van Microsoft mogelijkheid om te synchroniseren van uw beheerde domein blokkeren. Als u zich kunt los alle waarschuwingen, wacht u back twee uren en controleer-om te zien als de synchronisatie is voltooid.


## <a name="aadds501-a-backup-has-not-been-taken-in-a-while"></a>AADDS501: Een back-up niet is gemaakt in een tijdje

**Waarschuwing:**

*Het beheerde domein laatste back-up op [datum].*

**Oplossing:**

[Controleer de status van uw domein](active-directory-ds-check-health.md) voor alle waarschuwingen die kunnen wijzen op problemen in uw configuratie van uw beheerde domein. Problemen met de configuratie kunnen soms van Microsoft mogelijkheid om te synchroniseren van uw beheerde domein blokkeren. Als u zich kunt los alle waarschuwingen, wacht u back twee uren en controleer-om te zien als de synchronisatie is voltooid.


## <a name="aadds503-suspension-due-to-disabled-subscription"></a>AADDS503: Opschorten vanwege een uitgeschakeld abonnement

**Waarschuwing:**

*Het beheerde domein is onderbroken omdat de Azure-abonnement gekoppeld aan het domein niet actief is.*

**Oplossing:**

Om uw service te herstellen [vernieuwen van uw Azure-abonnement](https://docs.microsoft.com/azure/billing/billing-subscription-become-disable) die zijn gekoppeld aan uw beheerde domein.

## <a name="aadds504-suspension-due-to-an-invalid-configuration"></a>AADDS504: Opschorten vanwege een ongeldige configuratie

**Waarschuwing:**

*Het beheerde domein is onderbroken vanwege een ongeldige configuratie. De service is kan beheren, patch, of het bijwerken van de domeincontrollers voor uw beheerde domein gedurende een lange periode.*

**Oplossing:**

[Controleer de status van uw domein](active-directory-ds-check-health.md) voor alle waarschuwingen die kunnen wijzen op problemen in uw configuratie van uw beheerde domein. Als u een van deze waarschuwingen oplossen kunt, doen. Nadat de contact op met ondersteuning om uw abonnement opnieuw inschakelen.

## <a name="contact-us"></a>Contact opnemen
Neem contact op met de Azure Active Directory Domain Services-productteam voor [feedback delen of voor ondersteuning](active-directory-ds-contact-us.md).
