---
title: Azure Active Directory Gebruiksrechtovereenkomst| Microsoft Docs
description: Azure AD Gebruiksrechtovereenkomst biedt u en uw bedrijf de mogelijkheid om gebruikers van Azure AD-services een gebruiksrechtovereenkomst aan te bieden.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: d55872ef-7e45-4de5-a9a0-3298e3de3565
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/14/2017
ms.author: billmath
ms.openlocfilehash: c01ea7c6a05c77244ab29fe6eec00ed1e49b6f7e
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="azure-active-directory-terms-of-use-feature-preview"></a>Azure Active Directory Gebruiksrechtovereenkomst (preview)
Azure AD Gebruiksrechtovereenkomst is een eenvoudige methode waarmee organisaties gegevens kunnen presenteren aan eindgebruikers.  Deze methode zorgt ervoor dat gebruikers relevante disclaimers voor juridische vereisten of nalevingsvereisten te zien krijgen.

Voor het weergeven van inhoud maakt Azure AD Gebruiksrechtovereenkomst gebruik van de PDF-indeling.   Deze PDF kan van alles bevatten, zoals bestaande contracten, zodat u tijdens het aanmelden van gebruikers eindgebruikersovereenkomsten kunt verzamelen.  U kunt de gebruiksrechtovereenkomst gebruiken voor toepassingen, groepen gebruikers en als u voor verschillende doeleinden verschillende gebruiksrechtovereenkomsten hanteert.

In het vervolg van dit document wordt beschreven hoe u aan de slag gaat met Azure AD Gebruiksrechtovereenkomst.  

## <a name="why-use-azure-ad-terms-of-use"></a>Waarom Azure AD Gebruiksrechtovereenkomst gebruiken
Vindt u het lastig om werknemers of gasten zover te krijgen dat ze akkoord gaan met uw gebruiksrechtovereenkomst voordat ze toegang krijgen? Hebt u hulp nodig om uit te zoeken wie de gebruiksrechtovereenkomst van uw bedrijf nog niet heeft geaccepteerd?  Azure AD Gebruiksrechtovereenkomst is een eenvoudige methode waarmee organisaties gegevens kunnen presenteren aan eindgebruikers.  Deze methode zorgt ervoor dat ze relevante disclaimers voor juridische of nalevingsvereisten te zien krijgen.

Azure AD Gebruiksrechtovereenkomst kan in de volgende scenario's worden gebruikt:
-   Algemene gebruiksrechtovereenkomst voor alle gebruikers in uw organisatie.
-   Specifieke gebruiksvoorwaarden gebaseerd op gebruikerskenmerken (bijvoorbeeld artsen versus verpleegkundigen of binnenlandse werknemers versus werknemers in het buitenland, wat gebeurt met [dynamische groepen](https://azure.microsoft.com/updates/azure-active-directory-dynamic-membership-for-groups)).
-   Specifieke gebruiksrechtovereenkomst op basis van de toegang tot apps die een grote bedrijfsimpact hebben, zoals Salesforce.


## <a name="prerequisites"></a>Vereisten
Gebruik de volgende stappen voor het configureren van Azure AD Gebruiksrechtovereenkomst:

1. Meld u bij Azure AD aan als hoofdbeheerder, beveiligingsbeheerder of beheerder met voorwaardelijke toegang tot de directory waarvoor u Azure AD Gebruiksrechtovereenkomst wilt configureren.
2. Zorg ervoor dat de directory een Azure AD Premium P1-, P2-, EMS E3- of EMS E5-abonnement heeft.  Als dat niet het geval is, [schaft u Azure AD Premium aan](active-directory-get-started-premium.md) of [start u een proefversie](https://azure.microsoft.com/trial/get-started-active-directory/).
3. Geef het dashboard voor Azure AD Gebruiksrechtovereenkomst weer op [https://aka.ms/catou](https://aka.ms/catou).

>[!IMPORTANT]
>Controlemechanismen voor het beleid voor voorwaardelijk toegang (waaronder de gebruiksvoorwaarden) bieden geen ondersteuning voor handhaving op serviceaccounts.  Het is raadzaam om alle serviceaccounts uit te sluiten van het beleid voor voorwaardelijke toegang.

## <a name="add-company-terms-of-use"></a>Gebruiksrechtovereenkomst voor een bedrijf toevoegen
Wanneer u uw gebruiksrechtovereenkomst hebt voltooid, gebruikt u de volgende procedure om deze toe te voegen.

### <a name="to-add-terms-of-use"></a>Gebruiksrechtovereenkomst toevoegen
1. Ga naar het dashboard op [https://aka.ms/catou](https://aka.ms/catou)
2. Klik op Toevoegen.</br>
![Gebruiksrechtovereenkomst toevoegen](media/active-directory-tou/tou12.png)
3. Voer de **Naam** in voor de gebruiksrechtovereenkomst
4. Voer de **Weergavenaam** in.  Dit is de koptekst die gebruikers te zien krijgen wanneer ze zich aanmelden.
5. **Blader** naar de PDF met uw voltooide gebruiksrechtovereenkomst en selecteer deze.  De aanbevolen tekengrootte is 24.
6. **Selecteer** een taal voor de gebruiksvoorwaarden.  Via de taaloptie kunt u meerdere gebruiksvoorwaarden uploaden, elk met een andere taal.  Welke versie van de gebruiksvoorwaarden aan een eindgebruiker wordt weergegeven, is gebaseerd op de browservoorkeuren.
7. Schakel de optie **Gebruikers verplichten de gebruiksvoorwaarden uit te vouwen** in of uit.  Als deze optie is ingeschakeld, moeten eindgebruikers de gebruiksvoorwaarden bekijken alvorens deze te accepteren.
8. In de sectie **Voorwaardelijke toegang** kunt u de geüploade gebruiksrechtovereenkomst **afdwingen** door een sjabloon of beleid voor aangepaste voorwaardelijke toegang te gebruiken.  Met een beleid voor aangepaste voorwaardelijke toegang kunt u een gedetailleerde gebruiksrechtovereenkomst opgeven voor een specifieke cloudtoepassing of groep gebruikers.  Zie [Een voorwaardelijk toegangsbeleid configureren](active-directory-conditional-access-best-practices.md) voor meer informatie
9. Klik op **Maken**.
10. Als u een sjabloon voor aangepaste voorwaardelijke toegang hebt geselecteerd, kunt u in een volgend scherm het CA-beleid aanpassen.
11. Nu wordt uw nieuwe gebruiksrechtovereenkomst weergegeven.</br>

![Gebruiksrechtovereenkomst toevoegen](media/active-directory-tou/tou3.png)

## <a name="delete-terms-of-use"></a>Gebruiksrechtovereenkomst verwijderen
Met de volgende procedure kunt u een oude gebruiksrechtovereenkomst verwijderen:

### <a name="to-delete-terms-of-use"></a>Gebruiksrechtovereenkomst verwijderen
1. Ga naar het dashboard op [https://aka.ms/catou](https://aka.ms/catou)
2. Selecteer de gebruiksrechtovereenkomst die u wilt verwijderen.
3. Klik op **Verwijderen**.
4. Nu wordt uw nieuwe gebruiksrechtovereenkomst niet meer weergegeven.


## <a name="audit-terms-of-use"></a>Gebruiksrechtovereenkomst controleren
Met Azure AD Gebruiksrechtovereenkomst kunt u eenvoudig controleren wie uw gebruiksrechtovereenkomst heeft geaccepteerd.  Gebruik de volgende procedure om de controle te starten:

### <a name="to-audit-terms-of-use"></a>Gebruiksrechtovereenkomst controleren
1. Ga naar het dashboard op [https://aka.ms/catou](https://aka.ms/catou)
2. Klik op Controlegebeurtenis.</br>
![Controlegebeurtenis](media/active-directory-tou/tou8.png)
3.  In het scherm Azure AD-auditlogboeken kunt u de informatie met behulp van de beschikbare keuzelijsten filteren om specifieke auditlogboekinformatie weer te geven.
![Controlegebeurtenis](media/active-directory-tou/tou9.png)
4.  U kunt de informatie ook downloaden naar een CSV-bestand voor lokaal gebruik.

## <a name="what-users-see"></a>Wat gebruikers te zien krijgen
Gebruikers binnen het bereik krijgen het volgende te zien op het moment dat een gebruiksrechtovereenkomst is gemaakt en afgedwongen.  Tijdens de aanmelding worden deze schermen weergegeven.
-   Het wordt aangeraden om in de PDF een lettertype met tekengrootte 24 te gebruiken.
![Controlegebeurtenis](media/active-directory-tou/tou10.png)
-   Op een mobiele telefoon wordt dit scherm weergegeven</br></br>
![Controlegebeurtenis](media/active-directory-tou/tou11.png)

### <a name="review-terms-of-use"></a>Gebruiksvoorwaarden controleren
Gebruikers kunnen de gebruiksvoorwaarden die ze hebben geaccepteerd, controleren en inzien.  Dit kan via de volgende procedure:

1. Navigeer naar [https://myapps.microsoft.com](https://myapps.microsoft.com) en meld u aan.
2. Klik in de rechterbovenhoek op uw naam en selecteer **Profiel** in de vervolgkeuzelijst.
![Profiel](media/active-directory-tou/tou14.png)

3. Klik in uw profiel op **Gebruiksvoorwaarden controleren**.
![Controlegebeurtenis](media/active-directory-tou/tou13a.png)

4.  Vanaf hier kunt u de gebruiksvoorwaarden bekijken die u hebt geaccepteerd. 


## <a name="additional-information"></a>Aanvullende informatie
Hier volgt informatie waarvan u zich bewust moet zijn en die nuttig kan zijn bij het gebruik van een gebruiksrechtovereenkomst.

Gebruikers in het bereik moeten zich af- en aanmelden om te voldoen aan een nieuw beleid:
 - als een beleid voor voorwaardelijke toegang is ingeschakeld in een gebruiksrechtovereenkomst
 - of als er een tweede gebruiksrechtovereenkomst is gemaakt

Dat komt omdat een beleid voor voorwaardelijke toegang onmiddellijk van kracht wordt. Als dit gebeurt, krijgt de beheerder 'verdrietige clouds' of 'problemen met Azure AD-tokens' te zien. De beheerder moet zich afmelden en zich vervolgens opnieuw aanmelden om te voldoen aan het nieuwe beleid.





## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**V: Hoe kan ik zien wanneer/of een gebruiker een gebruiksrechtovereenkomst heeft geaccepteerd?**</br>
A: Wanneer een gebruiker de gebruiksrechtovereenkomst heeft geaccepteerd, wordt dit in het auditlogboek opgenomen. U kunt het auditlogboek van Azure AD doorzoeken om de resultaten te bekijken.  

**V: Als de gebruiksrechtovereenkomst wordt gewijzigd, moeten gebruikers deze dan opnieuw accepteren?**</br>
A: Ja, de beheerder kan de voorwaarden van de gebruiksrechtovereenkomst wijzigen. In dat geval moet de nieuwe gebruiksrechtovereenkomst opnieuw worden geaccepteerd.

**V: Wordt een gebruiksrechtovereenkomst ondersteund in meerdere talen?**</br>
A: Nee, het is momenteel niet mogelijk om meerdere talen in één gebruiksrechtovereenkomst op te nemen.  U kunt echter een bereik toekennen aan een groep (zo kunt u voor Frankrijk een andere gebruiksrechtovereenkomst hanteren dan voor het Verenigd Koninkrijk). 

**V: Wanneer wordt de gebruiksrechtovereenkomst geactiveerd?**</br>
A: De gebruiksrechtovereenkomst wordt geactiveerd tijdens het aanmelden.

**V: Voor welke toepassingen kan ik een gebruiksrechtovereenkomst implementeren?**</br>
A: U kunt u een beleid voor voorwaardelijke toegang maken voor bedrijfstoepassingen die gebruikmaken van moderne verificatie.  Zie [Bedrijfstoepassingen](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-view-azure-portal) voor meer informatie.

**V: Kan ik meerdere gebruiksrechtovereenkomsten toevoegen aan een bepaalde gebruiker of app?**</br>
A: Ja. Dit doet u door voor deze groepen of apps een afzonderlijk beleid voor voorwaardelijke toegang te maken. Als gebruikers binnen het bereik van meerdere gebruiksrechtovereenkomsten vallen, gaan ze akkoord met één gebruiksrechtovereenkomst tegelijk.
 
**V: Wat gebeurt er als een gebruiker de gebruiksrechtovereenkomst afwijst?**</br>
A: De gebruiker krijgt dan geen toegang tot de toepassing. De gebruiker moet zich opnieuw aanmelden en de gebruiksrechtovereenkomst accepteren om toegang te krijgen.