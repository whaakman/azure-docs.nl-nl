---
title: Wat is er beveiligde score identiteit? - Azure Active Directory
description: Hoe u de identiteit beveiligde score kunt gebruiken voor het verbeteren van de beveiligingsstatus van uw directory
services: active-directory
ms.service: active-directory
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: tilarso
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6cdff2305914ca6e4144f7784d1a60026a1d27c0
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65988695"
---
# <a name="what-is-the-identity-secure-score-in-azure-active-directory"></a>Wat is de id-beveiligingsscore in Azure Active Directory?

Hoe veilig is uw Azure AD-tenant? Als u niet hoe u deze vragen te beantwoorden weet, wordt er in dit artikel wordt uitgelegd hoe u de identiteit beveiligde score kunt bewaken en verbeteren van de beveiligingsstatus van uw identiteit.

## <a name="what-is-an-identity-secure-score"></a>Wat is een id-beveiligingsscore?

De identiteit beveiligde score is getal tussen 1 tot 223 op die fungeert als een indicator van hoe uitgelijnde u bent met de aanbevelingen van Microsoft voor beveiliging. Elke actie verbetering in beveiligde score identiteit wordt aangepast aan uw specifieke configuratie.  

![Veiligheidsscore](./media/identity-secure-score/identity-secure-score-overview.png)

De score helpt bij het volgende:

- Objectief meten van het beveiligingspostuur van uw identiteit
- Plannen van verbeteringen aan de identiteitsbeveiliging
- Evalueren van het succes van uw verbeteringen

De score en verwante informatie zijn toegankelijk via het dashboard Identity Secure Score. In dit dashboard vindt u deze informatie:

- Uw identiteit beveiligde score
- Een vergelijking van de grafiek wordt weergegeven hoe de score voor het beveiligen van uw identiteit vergelijkt met andere tenants in de dezelfde bedrijfstak en vergelijkbare grootte
- Een grafiek trend is hoe uw beveiligde score identiteit is gewijzigd na verloop van tijd weergeven
- Een lijst van mogelijke verbeteringen

Door de verbeteringsacties te volgen, kunt u het volgende doen:

- Verbeter uw beveiligingspostuur en uw score
- Maak gebruik van de functies die beschikbaar zijn voor uw organisatie als onderdeel van uw investeringen in identiteit

## <a name="how-do-i-get-my-secure-score"></a>Hoe kom ik aan mijn beveiligde score?

De identiteit beveiligde score is beschikbaar in alle edities van Azure AD. U kunt de score bekijken door naar het [dashboard Overzicht van Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/IdentitySecureScore) te gaan.

## <a name="how-does-it-work"></a>Hoe werkt dit?

Elke 48 uur wordt uw beveiligingsconfiguratie geanalyseerd door Azure en worden uw instellingen vergeleken met de aanbevolen procedures. Op basis van het resultaat van deze evaluatie, wordt een nieuwe score berekend voor uw directory. Het is mogelijk dat de beveiligingsconfiguratie is niet volledig afgestemd op de richtlijnen voor best practices en de acties voor verbetering slechts gedeeltelijk wordt voldaan. In deze scenario's worden u alleen toegekend in een deel van de maximale score die beschikbaar zijn voor het besturingselement.

Elke aanbeveling wordt gemeten op basis van uw Azure AD-configuratie. Als u van producten van derden gebruikmaakt om in te schakelen van een best practice-aanbeveling, kunt u deze configuratie in de instellingen van een actie voor verbetering aangeven. U hebt ook de mogelijkheid om in te stellen van de aanbevelingen voor moeten worden genegeerd als ze niet van toepassing op uw omgeving. Een genegeerde aanbeveling draagt niet bij aan de berekening van de score.

![Negeren of actie markeren als van derden vallen](./media/identity-secure-score/identity-secure-score-ignore-or-third-party-reccomendations.png)

## <a name="how-does-it-help-me"></a>Wat heb ik eraan?

De id-beveiligingsscore helpt bij het volgende:

- Objectief meten van het beveiligingspostuur van uw identiteit
- Plannen van verbeteringen aan de identiteitsbeveiliging
- Evalueren van het succes van uw verbeteringen

## <a name="what-you-should-know"></a>Wat u moet weten

### <a name="who-can-use-the-identity-secure-score"></a>Wie kan de id-beveiligingsscore gebruiken?

De id-beveiligingsscore kan worden gebruikt door de volgende rollen:

- Globale beheerder
- Beveiligingsbeheerder
- Beveiligingslezers

### <a name="how-are-controls-scored"></a>Hoe kan ik besturingselementen beoordeeld?

Besturingselementen kunnen op twee manieren worden beoordeeld. Sommige binaire wijze worden beoordeeld: u 100% van de score ophalen als de functie of instelling geconfigureerd op basis van onze aanbeveling. Andere scores worden berekend als een percentage van de configuratie van de totale. Bijvoorbeeld, als u de aanbeveling voor verbetering statussen krijgt u 30 punten als u al uw gebruikers met MFA beveiligt en u alleen 5 van 100 totaal aantal gebruikers die zijn beveiligd hebt, u wilt toekennen aan een gedeeltelijke score ongeveer 2 punten (5-beveiligd / totaal 100 * 30 max punten = 2 punten gedeeltelijke score) .

### <a name="what-does-not-scored-mean"></a>Wat betekent [Geen score]?

Acties met het label [Geen score] zijn acties die u kunt uitvoeren in uw organisatie, maar die geen score krijgen omdat ze nog niet zijn gekoppeld aan de tool. U kunt dus wel uw beveiliging verbeteren, maar u krijgt hier op dit moment geen punten voor.

### <a name="how-often-is-my-score-updated"></a>Hoe vaak wordt mijn score bijgewerkt?

De score wordt één keer per dag berekend (om ongeveer 01:00 PST). Als u een wijziging aanbrengt in een gemeten actie, wordt de score automatisch de volgende dag bijgewerkt. Het duurt maximaal 48 uur voordat een wijziging is verwerkt in de score.

### <a name="my-score-changed-how-do-i-figure-out-why"></a>Mijn score is gewijzigd. Hoe kom ik erachter waarom?

Ga naar de [Microsoft 365 security center](https://security.microsoft.com/), waar u vindt de volledige beveiligde score van Microsoft. U kunt gemakkelijk alle wijzigingen aan uw beveiligde score zien aan de hand van de uitgebreide wijzigingen op het tabblad Geschiedenis.

### <a name="does-the-secure-score-measure-my-risk-of-getting-breached"></a>De beveiligde score meten Mijn risico van het ophalen van geschonden?

Nee, dat niet. De beveiligde score heeft een absolute meting niet express van hoe waarschijnlijk u staat op te halen geschonden. De score geeft aan in welke mate u functies hebt geïmplementeerd die het risico van een beveiligingsprobleem beperken. Er is geen service kan niet garanderen dat u niet wordt geschonden, en de beveiligde score moet niet worden geïnterpreteerd als garantie op geen enkele manier.

### <a name="how-should-i-interpret-my-score"></a>Hoe moet ik mijn score interpreteren?

U krijgt punten voor het configureren van aanbevolen beveiligingsfuncties of het uitvoeren van beveiligingstaken (zoals het lezen van rapporten). Sommige acties krijgen een score voor gedeeltelijke uitvoering, zoals het inschakelen van meervoudige verificatie (MFA) voor uw gebruikers. Uw beveiligde score is rechtstreeks van de Microsoft security-services die u gebruikt. Houd er rekening mee dat de beveiliging moet worden verdeeld met bruikbaarheid. Alle beveiligingsmechanismen hebben in meer of mindere mate impact op de gebruiker. Mechanismen met een kleine impact op de gebruiker hebben geen tot nauwelijks gevolgen voor de dagelijkse taken van uw gebruikers.

Als u wilt zien van de geschiedenis van een score, Ga naar de [Microsoft 365-Beveiligingscentrum](https://security.microsoft.com/) en controleren van uw algehele beveiligde score van Microsoft. U kunt wijzigingen bekijken op uw algemene beveiligde score klikt u op de geschiedenis weergeven. Kies een specifieke datum om te zien welke mechanismen waren ingeschakeld voor die dag en wat dat voor punten heeft opgeleverd.

### <a name="how-does-the-identity-secure-score-relate-to-the-office-365-secure-score"></a>Hoe is id-beveiligingsscore gerelateerd aan de beveiligingsscore van Office 365?

De [Microsoft secure score](https://docs.microsoft.com/office365/securitycompliance/microsoft-secure-score) bevat vijf verschillende beheer- en score categorieën:

- Identiteit
- Gegevens
- Apparaten
- Infrastructuur
- Apps

De identiteit beveiligde score geeft het deel van de identiteit van de beveiligde score van Microsoft. Deze overlapping betekent dat uw aanbevelingen voor de identiteit van de beveiligde score en de score identiteit in Microsoft hetzelfde zijn.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over beveiligde Microsoft-score](https://docs.microsoft.com/office365/securitycompliance/microsoft-secure-score)
