---
title: Geconvergeerde registratie voor Azure AD SSPR en MFA (openbare preview)
description: Azure AD multi-factor Authentication-verificatie en selfservice voor wachtwoord opnieuw instellen van inschrijving (openbare preview)
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry, michmcla
ms.openlocfilehash: d6915ce659d96021d4185be3818919fcfb9d4371
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39492889"
---
# <a name="converged-registration-for-self-service-password-reset-and-azure-multi-factor-authentication-public-preview"></a>Geconvergeerde registratie voor self-service voor wachtwoord opnieuw instellen en Azure multi-factor Authentication (openbare preview)

Tot nu toe zijn de gebruikers hoeven te registreren verificatiemethoden voor Azure multi-factor Authentication (MFA) en selfservice voor wachtwoordherstel (SSPR) in twee verschillende portals. Er zijn veel gebruikers verward door het feit dat vergelijkbare methoden zijn gebruikt voor zowel de Azure MFA en de SSPR en kunnen niet worden geregistreerd in beide portals. Deze verschillen geleid tot sommige gebruikers niet kan Azure MFA of SSPR wanneer dat nodig is, leidt tot een helpdesk-aanroep te gebruiken en mogelijk een tevreden gebruikers. Gebruikers kunnen nu één keer registreren en profiteer van de voordelen van Azure MFA en de self-service voor Wachtwoordherstel, zodat u niet hoeft te registreren hun verificatiemethoden voor deze functies twee keer.  

Voordat u deze nieuwe ervaring voor uw organisatie inschakelt, wordt aangeraden dat u in dit artikel bekijken, evenals onze [eindgebruikersdocumentatie](https://aka.ms/securityinfoguide) de ervaring, om te begrijpen van de impact heeft op uw gebruikers. U kunt de [eindgebruikersdocumentatie](https://aka.ms/securityinfoguide) te trainen en uw gebruikers bij de voorbereiding voor de nieuwe ervaring en zorg ervoor een geslaagde implementatie dat.

|     |
| --- |
| Geconvergeerde registratie voor self-service voor wachtwoord opnieuw instellen en Azure multi-factor Authentication is een openbare preview-functie van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Als u wilt dat uw gebruikers kunnen verificatiemethoden registreren voor Azure multi-factor Authentication en selfservice voor wachtwoordherstel in één ervaring, voert u de volgende stappen uit:

1. Aanmelden bij de Azure portal aan als een globale beheerder of Gebruikerbeheerder.
2. Blader naar **Azure Active Directory**, **gebruikersinstellingen**, **beheren van instellingen voor toegang tot deelvenster preview-functies**.
3. Onder **gebruikers kunnen preview-functies gebruiken voor het registreren en beheren van beveiligingsgegevens**, u kunt kiezen om in te schakelen voor een **geselecteerde** groep van gebruikers of voor **alle** gebruikers.

Gebruikers kunnen nu gaat u naar [ https://aka.ms/setupsecurityinfo ](https://aka.ms/setupsecurityinfo) hun verificatiemethoden registreren voor MFA en Self-service voor Wachtwoordherstel. Zie voor meer informatie over wat uw gebruikers in deze nieuwe ervaring zien, onze [eindgebruikersdocumentatie](https://aka.ms/securityinfoguide).  

> [!NOTE]
> Nadat u deze ervaring, gebruikers die registreren of bevestigen hun telefoonnummer in- of mobiele app via de nieuwe ervaring de mogelijkheid om ze te gebruiken voor MFA en SSPR, hebben als deze methoden zijn ingeschakeld in het beleid voor MFA en Self-service voor Wachtwoordherstel. Als u deze vervolgens uitschakelt, moeten de gebruikers die gaat u naar de vorige pagina van de registratie van SSPR op aka.ms/ssprsetup om uit te voeren MFA voordat ze krijgen de pagina tot toegang.  

## <a name="how-it-works"></a>Hoe alles werkt

Als een gebruiker heeft eerder verificatiemethoden via de afzonderlijke registratie ervaringen van MFA en SSPR geregistreerd, hoeft ze niet te registreren die gegevens opnieuw. Echter, als de instellingen vereisen gebruikers dat zich registreren voor MFA of SSPR, zien zij mogelijk een prompt om hun beveiligingsgegevens te controleren tijdens het aanmelden.

Als een gebruiker heeft een methode die kan worden gebruikt voor MFA hebt geregistreerd, wordt ze gevraagd om uit te voeren MFA voordat ze krijgen de nieuwe ervaring tot toegang.

Als u registratie hebt afgedwongen voor MFA of SSPR en een gebruiker heeft nog niet zijn geregistreerd, wordt ze gevraagd om u te registreren wanneer ze zich aanmelden.

Gebruikers wordt gevraagd om u te registreren bij het aanmelden, ziet de volgende ervaring:

![Geconvergeerde registratie. Methoden instellen als een nieuwe gebruiker](./media/concept-registration-mfa-sspr-converged/concept-registration-add-methods.png)

> [!NOTE]
> Deze ervaring wordt alleen weergegeven wanneer een gebruiker wordt gevraagd om u te registreren bij het aanmelden. Gebruikers die gaat u rechtstreeks naar de nieuwe ervaring op aka.ms/setupsecurityinfo ziet een andere versie van de ervaring, die verderop in dit artikel wordt beschreven.

De authenticatiemethoden die worden weergegeven wordt op basis van de methoden die zijn ingeschakeld in uw MFA of SSPR-beleid gewijzigd. De gebruiker wordt gevraagd om u te registreren van het minimum aantal verificatiemethoden die nodig zijn om te voldoen aan het MFA-beleid en/of de SSPR-beleid. Als er flexibiliteit in welke verificatiemethoden de gebruiker kan registreren, kunnen ze selecteren **Kies beveiligingsgegevens** andere verificatiemethoden kiezen.  

In tegenstelling tot de ervaring van MFA-registratie worden gebruikers niet gevraagd om u te registreren van een app-wachtwoord wanneer de nieuwe ervaring voor inschrijving te doorlopen. In plaats daarvan moeten ze volgen de stappen in de zelfstudie apps wachtwoorden voor het registreren van app-wachtwoorden in de nieuwe ervaring.  

Nadat een gebruiker de registratie, worden automatisch de standaardmethode voor MFA ingesteld. Als de gebruiker een verificator-app geregistreerd, wordt de standaardmethode voor worden ingesteld op de app. Als de gebruiker een verificator-app niet registreren heeft en hun telefoonnummer alleen geregistreerd, wordt de standaardmethode voor worden ingesteld op telefonische oproep. Gebruikers kunnen hun standaard wijzigen door te gaan naar [ https://aka.ms/setupsecurityinfo ](https://aka.ms/setupsecurityinfo) en selecteren **standaardinstelling wijzigen**.  

Als de registratie wordt niet afgedwongen, kunnen gebruikers beheren hun eigen verificatiemethoden op [ https://aka.ms/setupsecurityinfo ](https://aka.ms/setupsecurityinfo). Als een gebruiker heeft eerder een methode die kan worden gebruikt om uit te voeren MFA hebt geregistreerd, wordt ze gevraagd om uit te voeren MFA voordat ze krijgen de pagina tot toegang.  

![Geconvergeerde registratie. Methoden als een geregistreerde gebruiker bewerken](./media/concept-registration-mfa-sspr-converged/concept-registration-edit-methods.png)

Op deze pagina ziet gebruikers eerder geregistreerde verificatiemethoden en ondersteunde verificatiemethoden geregistreerd voor deze zoals telefoon (werk). Gebruikers kunnen ook toevoegen, bewerken of verwijderen van hun verificatiemethoden (met uitzondering van de telefoon (werk)).  

Auditlogboeken voor deze nieuwe ervaring bestaan onder de categorie verificatiemethoden van het auditlogboek.  

## <a name="known-issues"></a>Bekende problemen

**Standaardmethode voor MFA is ingesteld op telefonische oproep wanneer gebruiker registreert telefoon met SMS-bericht**

   * Sommige gebruikers is het mogelijk dat de standaardmethode voor MFA is ingesteld op telefonische oproep nadat ze hun telefoonnummer met behulp van de SMS-bericht hebt geregistreerd. Gebruikers kunnen dit probleem oplossen door het veranderen van de standaard-methode door de instructies in het artikel te volgen [beheren van je beveiligingsgegevens (preview)](../user-help/security-info-manage-settings.md#change-your-info).

**Geen toegang tot de nieuwe ervaring voor inschrijving nadat de beheerder schakelt de standaardmethode voor gebruiker**

   * Sommige gebruikers mogelijk geen toegang kunnen krijgen tot de nieuwe ervaring voor inschrijving als de eerder geregistreerde standaard MFA-methode is uitgeschakeld door de beheerder. Hier volgt een voorbeeldscenario:
      1. Gebruiker eerder ingeschreven hun telefoonnummer en hun standaardmethode ingesteld op telefonische oproep.
      2. Beheerder schakelt telefoonoproep als een MFA-methode voor de tenant.
      3. Gebruiker wordt gevraagd om u te registreren tijdens het aanmelden, omdat ze nodig hebben voor het registreren van een aanvullende methode om te voldoen aan de tenant SSPR-beleid.
      4. Gebruiker probeert te registreren, maar geen toegang tot de pagina vanwege een niet met een standaardmethode en in een lus is vastgelopen.

## <a name="next-steps"></a>Volgende stappen

[Informatie over het implementeren van Azure AD-selfservice wachtwoord opnieuw instellen](howto-sspr-deployment.md)

[Meer informatie over het meervoudige verificatie vereisen bij het aanmelden](howto-mfa-getstarted.md)

[Documentatie voor eindgebruikers verificatie methode configuration](https://aka.ms/securityinfoguide)