---
title: Geconvergeerde registratie voor Azure AD SSPR en MFA (openbare preview)
description: Azure AD multi-factor Authentication en Self-service voor wachtwoord opnieuw instellen van inschrijving (openbare preview)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 12/10/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry, michmcla
ms.openlocfilehash: 4080fe07f6624c96f6385492268042f6401f3694
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55472674"
---
# <a name="converged-registration-for-self-service-password-reset-and-azure-multi-factor-authentication-public-preview"></a>Geconvergeerde registratie voor self-service voor wachtwoord opnieuw instellen en Azure multi-factor Authentication (openbare preview)

Tot nu toe zijn de gebruikers hoeven te registreren verificatiemethoden voor Azure multi-factor Authentication (MFA) en selfservice voor wachtwoordherstel (SSPR) in twee verschillende portals. Er zijn veel gebruikers verward dat vergelijkbare methoden zijn gebruikt voor zowel de Azure MFA en de SSPR en kunnen niet worden geregistreerd in beide portals. Sommige gebruikers konden vervolgens Azure MFA of SSPR gebruiken als nodig, toonaangevende helpdesk-aanroepen. 

Gebruikers kunnen nu één keer registreren en profiteer van de voordelen van Azure MFA en de self-service voor Wachtwoordherstel. Gebruikers hoeven niet te registreren hun verificatiemethoden voor deze functies twee keer.  

Voordat u deze nieuwe ervaring voor uw organisatie inschakelt, wordt aangeraden dat u in dit artikel bekijken en de [gebruikersdocumentatie](https://aka.ms/securityinfoguide) om de impact die de ervaring voor uw gebruikers te begrijpen. U kunt de documentatie voor de gebruiker te trainen en voorbereiden van uw gebruikers voor de nieuwe ervaring en zorgen voor een geslaagde implementatie.

|     |
| --- |
| Geconvergeerde registratie voor self-service voor wachtwoord opnieuw instellen en Azure multi-factor Authentication is een openbare preview-functie van Azure Active Directory (Azure AD). Zie [Supplemental Terms of Use for Microsoft Azure Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews) voor meer informatie.|
|     |

Als u wilt dat uw gebruikers kunnen verificatiemethoden registreren voor Azure multi-factor Authentication en selfservice voor wachtwoordherstel in één ervaring, voert u de volgende stappen uit:

1. Aanmelden bij de Azure portal aan als een globale beheerder of Gebruikerbeheerder.
2. Blader naar **Azure Active Directory** > **gebruikersinstellingen** > **beheren van instellingen voor toegang tot deelvenster preview-functies**.
3. Onder **gebruikers kunnen preview-functies gebruiken voor het registreren en beheren van beveiligingsgegevens**, u kunt kiezen om in te schakelen voor een **geselecteerde** groep van gebruikers of voor **alle** gebruikers.

Gebruikers kunnen nu gaat u naar [ https://aka.ms/setupsecurityinfo ](https://aka.ms/setupsecurityinfo) hun verificatiemethoden registreren voor MFA en Self-service voor Wachtwoordherstel. Zie voor meer informatie over wat uw gebruikers in deze nieuwe ervaring zien, de [gebruikersdocumentatie](https://aka.ms/securityinfoguide).  

> [!NOTE]
> Nadat u deze ervaring hebt ingeschakeld, kunnen gebruikers die registreren of bevestigen hun telefoonnummer of mobiele app via de nieuwe ervaring gebruiken ze voor MFA en SSPR, als deze methoden zijn ingeschakeld in het beleid voor MFA en Self-service voor Wachtwoordherstel. Als u deze vervolgens uitschakelt, wordt gebruikers die gaat u naar de vorige pagina van de registratie van SSPR op https:/aka.ms/ssprsetup moet naar de multi-factor authentication uitvoeren voordat ze krijgen de pagina tot toegang.  

## <a name="how-it-works"></a>Hoe werkt het?

Als een gebruiker heeft eerder verificatiemethoden via de afzonderlijke ervaringen van MFA en SSPR-registratie hebt geregistreerd, moeten ze niet dat gegevens opnieuw registreren. Maar als uw instellingen vereisen gebruikers dat zich registreren voor MFA of SSPR, zien ze mogelijk een prompt om te controleren van hun beveiligingsgegevens tijdens het aanmelden.

Als een gebruiker heeft een methode die kan worden gebruikt voor MFA hebt geregistreerd, wordt ze gevraagd om uit te voeren met meervoudige verificatie voordat ze krijgen de nieuwe ervaring tot toegang.

Als u registratie hebt afgedwongen voor MFA of SSPR en een gebruiker heeft nog niet zijn geregistreerd, wordt ze gevraagd om u te registreren wanneer ze zich aanmelden.

Gebruikers wordt gevraagd om u te registreren bij het aanmelden, Zie de volgende ervaring:

![Geconvergeerde registratie. Methoden instellen als een nieuwe gebruiker.](./media/concept-registration-mfa-sspr-converged/concept-registration-add-methods.png)

> [!NOTE]
> Deze ervaring wordt weergegeven alleen wanneer een gebruiker wordt gevraagd om u te registreren bij het aanmelden. Gebruikers die gaat u rechtstreeks naar de ervaring op https://aka.ms/setupsecurityinfo ziet u een andere versie van de ervaring, die verderop in dit artikel wordt beschreven.

De methoden weergegeven verificatie wijzigen op basis van de methoden die zijn ingeschakeld in uw beleid MFA en Self-service voor Wachtwoordherstel. De gebruiker gevraagd om u te registreren van het minimum aantal verificatiemethoden die nodig zijn om te voldoen aan het MFA-beleid en/of de SSPR-beleid. Als er flexibiliteit in welke verificatiemethoden de gebruiker kan registreren, kunnen ze selecteren **Kies beveiligingsgegevens** andere verificatiemethoden kiezen.  

> [!NOTE]
> Als u het gebruik van zowel de mobiele app-meldingen en de mobiele app-code inschakelt, kunnen gebruikers die de Microsoft Authenticator-app via een melding te registreren zowel melding en code gebruiken om hun identiteit te verifiëren.

In tegenstelling tot de ervaring van MFA-registratie worden gebruikers niet gevraagd om u te registreren van een app-wachtwoord wanneer de nieuwe ervaring voor inschrijving te doorlopen. In plaats daarvan moeten ze volgen de stappen in de zelfstudie apps wachtwoorden voor het registreren van app-wachtwoorden in de nieuwe ervaring.  

Nadat een gebruiker is registratie voltooid, wordt automatisch de standaardmethode voor MFA ingesteld. Als de gebruiker een verificator-app geregistreerd, is de standaardmethode voor aan-app ingesteld. Als de gebruiker een verificator-app niet registreren heeft en hun telefoonnummer alleen geregistreerd, is de standaardmethode voor ingesteld op telefonische oproep. Gebruikers kunnen hun standaard wijzigen door te gaan naar https://aka.ms/setupsecurityinfo en selecteren **standaardinstelling wijzigen**.  

Als de registratie wordt niet afgedwongen, kunnen gebruikers beheren hun eigen verificatiemethoden op https://aka.ms/setupsecurityinfo. Als een gebruiker heeft eerder een methode die kan worden gebruikt voor MFA hebt geregistreerd, wordt ze gevraagd om u te multi-factor authentication uitvoeren voordat ze krijgen de pagina tot toegang.  

![Geconvergeerde registratie. U kunt methoden bewerken als een geregistreerde gebruiker.](./media/concept-registration-mfa-sspr-converged/concept-registration-edit-methods.png)

Gebruikers zien op deze pagina eerder geregistreerde verificatiemethoden en ondersteunde verificatiemethoden geregistreerd voor, zoals een telefoon (werk). Gebruikers kunnen ook toevoegen, bewerken of verwijderen van hun verificatiemethoden (met uitzondering van de telefoon (werk)).  

Auditlogboeken voor deze nieuwe ervaring bestaan onder de categorie verificatiemethoden van het auditlogboek.  

## <a name="known-issues"></a>Bekende problemen

**Standaardmethode voor MFA is ingesteld op telefonische oproep wanneer een gebruiker een telefoon registreert met behulp van de SMS-bericht**

   * Sommige gebruikers hebben gemerkt dat de standaardmethode voor MFA is ingesteld op telefonische oproep nadat ze hun telefoonnummer registreren met behulp van de SMS-bericht. Gebruikers kunnen dit probleem oplossen door het veranderen van de standaard-methode door de instructies in het artikel [beheren van je beveiligingsgegevens (preview)](../user-help/security-info-manage-settings.md#change-your-info).

**Gebruiker heeft geen toegang tot de nieuwe ervaring voor inschrijving nadat een beheerder de standaardmethode voor uitgeschakeld**

   * Sommige gebruikers mogelijk geen toegang krijgt tot de nieuwe registratie-ervaring als de beheerder heeft de eerder geregistreerde standaardmethode voor MFA uitgeschakeld. Hier volgt een voorbeeldscenario:
      1. Gebruiker eerder ingeschreven hun telefoonnummer en hun standaardmethode ingesteld op telefonische oproep.
      2. Beheerder schakelt telefoonoproep als een MFA-methode voor de tenant.
      3. Gebruiker wordt gevraagd om u te registreren tijdens het aanmelden, omdat ze nodig hebben voor het registreren van een aanvullende methode om te voldoen aan de tenant SSPR-beleid.
      4. Gebruiker probeert te registreren, maar geen toegang tot de pagina en in een lus is vastgelopen omdat een standaard-methode is niet ingesteld.

## <a name="next-steps"></a>Volgende stappen

[Informatie over het implementeren van Azure AD-selfservice wachtwoord opnieuw instellen](howto-sspr-deployment.md)

[Meer informatie over het meervoudige verificatie vereisen voor aanmelden](howto-mfa-getstarted.md)

[Meer informatie over het configureren van verificatiemethoden voor gebruiker](https://aka.ms/securityinfoguide)
