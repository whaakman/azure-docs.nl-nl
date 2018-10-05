---
title: Wat is id-beveiligingsscore in Azure AD? - preview | Microsoft Docs
description: Lees hoe u de id-beveiligingsscore kunt gebruiken om het beveiligingspostuur van uw Azure AD-tenant te verbeteren.
services: active-directory
keywords: id-beveiligingsscore, Azure AD, beveiligde toegang tot bedrijfsbronnen
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: fundamentals
ms.topic: overview
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: a9bb22cae2e9fcd607a81d442f4fb51f12c43454
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224134"
---
# <a name="what-is-the-identity-secure-score-in-azure-ad---preview"></a>Wat is de id-beveiligingsscore in Azure AD? - preview

Hoe veilig is uw Azure AD-tenant? Als u het antwoord op deze vraag schuldig moet blijven, leest u dit artikel om erachter te komen hoe de id-beveiligingsscore kan helpen om het beveiligingspostuur van uw identiteit te controleren en te verbeteren. 

## <a name="what-is-an-identity-secure-score"></a>Wat is een id-beveiligingsscore?

De id-beveiligingsscore is een getal tussen 1 en 248 dat aangeeft in welke mate u voldoet aan de aanbevolen procedures van Microsoft inzake beveiliging.


![Beveiligingsscore](./media/identity-secure-score/01.png)



De score helpt bij het volgende:

- Objectief meten van het beveiligingspostuur van uw identiteit

- Plannen van verbeteringen aan de identiteitsbeveiliging

- Evalueren van het succes van uw verbeteringen 


De score en verwante informatie zijn toegankelijk via het dashboard Identity Secure Score. In dit dashboard vindt u deze informatie:

- Uw id-beveiligingsscore

    ![Beveiligingsscore](./media/identity-secure-score/02.png)

- Een vergelijkingsgrafiek

    ![Beveiligingsscore](./media/identity-secure-score/03.png)

- Een trendgrafiek

    ![Beveiligingsscore](./media/identity-secure-score/04.png)

- Een lijst met verbeteringsacties. 

    ![Beveiligingsscore](./media/identity-secure-score/05.png)


Door de verbeteringsacties te volgen, kunt u het volgende doen:

- Uw beveiligingspostuur en uw score verbeteren.
 
- Gebruikmaken van de functies van Microsoft Identity. 



## <a name="how-do-i-get-my-secure-score"></a>Hoe kom ik aan mijn beveiligde score?

De id-beveiligingsscore is beschikbaar in alle edities van Azure AD.

U kunt de score bekijken door naar het [dashboard Overzicht van Azure AD](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/IdentitySecureScore) te gaan.



## <a name="how-does-it-work"></a>Hoe werkt het?

Elke 48 uur wordt uw beveiligingsconfiguratie geanalyseerd door Azure en worden uw instellingen vergeleken met de aanbevolen procedures. Op basis van het resultaat van deze evaluatie wordt er een nieuwe score berekend voor uw tenant. Dit betekent dat het tot 48 uur kan duren duurt voordat een wijziging in uw configuratie is verwerkt in uw score. 

Elke aanbeveling wordt gemeten op basis van uw Azure AD-configuratie. Als u producten van derden gebruikt om een aanbevolen procedure toe te passen, kunt u dit aangeven in de instellingen van een verbeteringsactie.

![Beveiligingsscore](./media/identity-secure-score/07.png)


Bovendien hebt u de mogelijkheid om aanbevelingen te negeren als deze niet van toepassing zijn op uw omgeving. Een genegeerde aanbeveling draagt niet bij aan de berekening van de score. 
 
![Beveiligingsscore](./media/identity-secure-score/06.png)



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

### <a name="what-does-not-scored-mean"></a>Wat betekent [Geen score]?

Acties met het label [Geen score] zijn acties die u kunt uitvoeren in uw organisatie, maar die geen score krijgen omdat ze nog niet zijn gekoppeld aan de tool. U kunt dus wel uw beveiliging verbeteren, maar u krijgt hier op dit moment geen punten voor.

### <a name="how-often-is-my-score-updated"></a>Hoe vaak wordt mijn score bijgewerkt?

De score wordt één keer per dag berekend (om ongeveer 01:00 PST). Als u een wijziging aanbrengt in een gemeten actie, wordt de score automatisch de volgende dag bijgewerkt. Het duurt maximaal 48 uur voordat een wijziging is verwerkt in de score.


### <a name="my-score-changed-how-do-i-figure-out-why"></a>Mijn score is gewijzigd. Hoe kom ik erachter waarom?

Klik op de pagina van Score Analyzer in de [portal van Secure Score](https://securescore.microsoft.com/#!/score) op een gegevenspunt voor een bepaalde dag en scrol omlaag om de voltooide en niet-voltooide acties voor die dag te zien en erachter te komen wat er is veranderd.

### <a name="does-the-secure-score-measure-my-risk-of-getting-breached"></a>Geeft de id-beveiligingsscore een indicatie van de kans dat ik te maken krijg met een beveiligingsprobleem?

Nee, dat niet. De beveiligingsscore is geen absolute indicatie van hoe groot de kans is dat u te maken gaat krijgen met een beveiligingsprobleem. De score geeft aan in welke mate u functies hebt geïmplementeerd die het risico van een beveiligingsprobleem beperken. Er bestaan geen services die volledige bescherming kunnen bieden tegen een beveiligingsschending. De beveiligingsscore mag dan ook op geen enkele manier worden geïnterpreteerd als een garantie hiertegen.

### <a name="how-should-i-interpret-my-score"></a>Hoe moet ik mijn score interpreteren?

U krijgt punten voor het configureren van aanbevolen beveiligingsfuncties of het uitvoeren van beveiligingstaken (zoals het lezen van rapporten). Sommige acties krijgen een score voor gedeeltelijke uitvoering, zoals het inschakelen van meervoudige verificatie (MFA) voor uw gebruikers. Uw beveiligingsscore is een directe afspiegeling van de Microsoft-beveiligingsservices die u gebruikt. Vergeet niet dat beveiliging altijd in balans moet zijn met bruikbaarheid. Alle beveiligingsmechanismen hebben in meer of mindere mate impact op de gebruiker. Mechanismen met een kleine impact op de gebruiker hebben geen tot nauwelijks gevolgen voor de dagelijkse taken van uw gebruikers.

Als u de geschiedenis van uw beveiligingsscore wilt zien, gaat u naar de pagina van Score Analyzer in de [portal van Secure Score](https://securescore.microsoft.com/#!/score). Kies een specifieke datum om te zien welke mechanismen waren ingeschakeld voor die dag en wat dat voor punten heeft opgeleverd.


### <a name="how-does-the-identity-secure-score-relate-to-the-office-365-secure-score"></a>Hoe is id-beveiligingsscore gerelateerd aan de beveiligingsscore van Office 365? 

De [beveiligingsscore van Office 365](https://docs.microsoft.com/office365/securitycompliance/office-365-secure-score) wordt binnenkort gemigreerd naar een aggregatie van vijf verschillende scores:

- Identiteit

- Gegevens

- Apparaten

- Infrastructuur

- Apps

De id-beveiligingsscore vertegenwoordigt het identiteitsdeel van de beveiligingsscore van Office 365. Dit betekent dat uw aanbevelingen voor de id-beveiligingsscore en de identiteitsscore in Office 365 hetzelfde zijn. 


## <a name="next-steps"></a>Volgende stappen

Als u een video wilt zien over de beveiligingsscore van Office 365 wilt, klikt u [hier](https://www.youtube.com/watch?v=jzfpDJ9Kg-A).
 
