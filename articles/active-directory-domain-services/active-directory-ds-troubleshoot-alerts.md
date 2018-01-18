---
title: 'Azure Active Directory Domain Services: Waarschuwingen oplossen | Microsoft Docs'
description: Waarschuwingen voor Azure AD Domain Services oplossen
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: ergreenl
ms.openlocfilehash: b2e0edf3588f3b1db5f4b6641019be1ded9cb50e
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Azure AD Domain Services - waarschuwingen oplossen
Dit artikel bevat richtlijnen voor probleemoplossing voor de waarschuwingen die op uw beheerde domein optreden kunnen.


Kies de stappen voor probleemoplossing die met overeenkomen of waarschuwing-ID of het bericht dat u ondervindt.

| **Waarschuwing-ID** | **Waarschuwingsbericht** | **Naamomzetting** |
| --- | --- | :--- |
| AADDS001 | *Beveiligde LDAP via internet is ingeschakeld voor het beheerde domein. Echter, toegang tot poort 636 is niet vergrendeld met behulp van een netwerkbeveiligingsgroep. Dit kan een beveiligingsrisico voor gebruikersaccounts op het beheerde domein aanvallen met brute kracht wachtwoord.* | [Onjuiste beveiligde LDAP-configuratie](active-directory-ds-troubleshoot-ldaps.md) |
| AADDS100 | *De Azure AD-directory die is gekoppeld aan uw beheerde domein is mogelijk verwijderd. Het beheerde domein is niet langer in een ondersteunde configuratie. Microsoft kan niet controleren, beheren, patch en synchroniseren van uw beheerde domein.* | [Ontbrekende map](#aadds100-missing-directory) |
| AADDS101 | *Azure AD Domain Services kan niet worden ingeschakeld in een Azure AD B2C-Directory.* | [Azure AD B2C wordt uitgevoerd in deze map](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Een Service-Principal die is vereist voor Azure AD Domain Services naar behoren is verwijderd van uw Azure AD-directory. Deze configuratie heeft impact op de mogelijkheid van Microsoft als u wilt bewaken, beheren, patch en synchroniseren van uw beheerde domein.* | [Service-Principal ontbreekt](active-directory-ds-troubleshoot-service-principals.md) |
| AADDS103 | *Het IP-adresbereik voor het virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld is in een openbare IP-adresbereik. Azure AD Domain Services moeten zijn ingeschakeld in een virtueel netwerk met een persoonlijke IP-adresbereik. Deze configuratie heeft impact op de mogelijkheid van Microsoft als u wilt bewaken, beheren, patch en synchroniseren van uw beheerde domein.* | [Adres bevindt zich in een openbare IP-adresbereik](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *Microsoft kan niet tot de domeincontrollers voor dit beheerde domein. Dit kan gebeuren als een netwerkbeveiligingsgroep (NSG) geconfigureerd op uw virtuele netwerk blokkeert de toegang tot het beheerde domein. Een andere mogelijke oorzaak is als er een gebruiker gedefinieerde route blokken binnenkomend verkeer van het internet.* | [Network Error](active-directory-ds-troubleshoot-nsg.md) |

## <a name="aadds100-missing-directory"></a>AADDS100: Directory ontbreekt
**Waarschuwing:**

*De Azure AD-directory die is gekoppeld aan uw beheerde domein is mogelijk verwijderd. Het beheerde domein is niet langer in een ondersteunde configuratie. Microsoft kan niet controleren, beheren, patch en synchroniseren van uw beheerde domein.*

**Herstel:**

Deze fout wordt meestal veroorzaakt door onjuist uw Azure-abonnement te verplaatsen naar een nieuwe Azure AD-directory en het verwijderen van de oude Azure AD-directory is nog steeds gekoppeld aan Azure AD Domain Services.

Deze fout is onherstelbaar. Om op te lossen, moet u [uw bestaande beheerde domein verwijderen](active-directory-ds-disable-aadds.md) en maak deze opnieuw in de nieuwe map. Als u problemen bij het verwijderen ondervindt, neem dan contact op met het Azure Active Directory Domain Services-productteam [voor ondersteuning](active-directory-ds-contact-us.md).

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: Azure AD B2C wordt uitgevoerd in deze map
**Waarschuwing:**

*Azure AD Domain Services kan niet worden ingeschakeld in een Azure AD B2C-Directory.*

**Herstel:**

>[!NOTE]
>Om te blijven gebruiken van Azure AD Domain Services, moet u uw Azure AD Domain Services-exemplaar in een niet - Azure AD B2C-directory opnieuw maken.

Volg deze stappen voor het herstellen van uw service:

1. [Verwijderen van uw beheerde domein](active-directory-ds-disable-aadds.md) van uw bestaande Azure AD-directory.
2. Maak een nieuwe map die niet een Azure AD B2C-directory.
3. Ga als volgt de [aan de slag](active-directory-ds-getting-started.md) handleiding voor het opnieuw maken van een beheerd domein.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: Adres bevindt zich in een openbare IP-adresbereik

**Waarschuwing:**

*Het IP-adresbereik voor het virtuele netwerk waarin u Azure AD Domain Services hebt ingeschakeld is in een openbare IP-adresbereik. Azure AD Domain Services moeten zijn ingeschakeld in een virtueel netwerk met een persoonlijke IP-adresbereik. Deze configuratie heeft impact op de mogelijkheid van Microsoft als u wilt bewaken, beheren, patch en synchroniseren van uw beheerde domein.*

**Herstel:**

> [!NOTE]
> Om dit probleem op te lossen, moet u uw bestaande beheerde domein verwijderen en opnieuw maken in een virtueel netwerk met een persoonlijke IP-adresbereik. Dit proces is verstoren.

Lees voordat u begint, de **persoonlijke IP v4-adresruimte** in sectie [in dit artikel](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces).

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
8. Controleer de status van uw domein in twee uur om ervoor te zorgen dat u de stappen correct hebt voltooid.


## <a name="contact-us"></a>Contact opnemen
Neem contact op met de Azure Active Directory Domain Services-productteam voor [feedback delen of voor ondersteuning](active-directory-ds-contact-us.md).
