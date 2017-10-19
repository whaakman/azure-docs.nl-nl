---
title: Azure Active Directory Gebruiksrechtovereenkomst| Microsoft Docs
description: Azure AD Gebruiksrechtovereenkomst biedt u en uw bedrijf de mogelijkheid om gebruikers van Azure AD-services een gebruiksrechtovereenkomst aan te bieden.
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: d55872ef-7e45-4de5-a9a0-3298e3de3565
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/19/2017
ms.author: billmath
ms.openlocfilehash: b6318b419a0ea87fd1fb56656b1161909876f338
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-active-directory-terms-of-use-feature-preview"></a>Azure Active Directory Gebruiksrechtovereenkomst (preview)
Azure AD Gebruiksrechtovereenkomst is een eenvoudige methode waarmee organisaties gegevens kunnen presenteren aan eindgebruikers.  Deze methode zorgt ervoor dat gebruikers relevante disclaimers voor juridische of nalevingsvereisten te zien krijgen.

Voor het weergeven van inhoud maakt Azure AD Gebruiksrechtovereenkomst gebruik van de PDF-indeling.   Deze PDF kan van alles bevatten, zoals bestaande contracten, zodat u tijdens het aanmelden van gebruikers eindgebruikersovereenkomsten kunt verzamelen.  U kunt de gebruiksrechtovereenkomst gebruiken voor toepassingen, groepen gebruikers, en als u voor verschillende doeleinden verschillende gebruiksrechtovereenkomsten hanteert.

In het vervolg van dit document wordt beschreven hoe u aan de slag gaat met Azure AD Gebruiksrechtovereenkomst.  

## <a name="why-use-azure-ad-terms-of-use"></a>Waarom Azure AD Gebruiksrechtovereenkomst gebruiken
Vindt u het lastig om werknemers of gasten zover te krijgen dat ze akkoord gaan met uw gebruiksrechtovereenkomst voordat ze toegang krijgen? Hebt u hulp nodig om uit te zoeken wie de gebruiksvoorwaarden van uw bedrijf nog niet heeft geaccepteerd?  Azure AD Gebruiksrechtovereenkomst is een eenvoudige methode waarmee organisaties gegevens kunnen presenteren aan eindgebruikers.  Deze methode zorgt ervoor dat ze relevante disclaimers voor juridische of nalevingsvereisten te zien krijgen.

Azure AD Gebruiksrechtovereenkomst kan in de volgende scenario's worden gebruikt:
-   Algemene gebruiksvoorwaarden voor alle gebruikers in uw organisatie.
-   Specifieke gebruiksvoorwaarden gebaseerd op gebruikerskenmerken (bijvoorbeeld artsen versus verpleegkundigen of binnenlandse werknemers versus werknemers in het buitenland, wat gebeurt met [dynamische groepen](https://azure.microsoft.com/updates/azure-active-directory-dynamic-membership-for-groups)).
-   Specifieke gebruiksvoorwaarden op basis van de toegang tot apps die een grote bedrijfsimpact hebben, zoals Salesforce.


## <a name="prerequisites"></a>Vereisten
Gebruik de volgende stappen voor het configureren van Azure AD Gebruiksrechtovereenkomst:

1. Meld u bij Azure AD aan als hoofdbeheerder, beveiligingsbeheerder of beheerder met voorwaardelijke toegang tot de directory waarvoor u Azure AD Gebruiksrechtovereenkomst wilt configureren.
2. Zorg ervoor dat de map een Azure AD Premium P1-, P2-, EMS E3- of EMS E5-abonnement heeft.  Als dat niet het geval is, [schaft u Azure AD Premium aan](active-directory-get-started-premium.md) of [start u een proefversie](https://azure.microsoft.com/trial/get-started-active-directory/).
3. Geef het dashboard Azure AD Gebruiksrechtovereenkomst weer op [https://aka.ms/catou](https://aka.ms/catou).



## <a name="add-company-terms-of-use"></a>Gebruiksvoorwaarden voor een bedrijf toevoegen
Wanneer u uw gebruiksrechtovereenkomst hebt voltooid, gebruikt u de volgende procedure om deze toe te voegen.

### <a name="to-add-terms-of-use"></a>Gebruiksrechtovereenkomst toevoegen
1. Ga naar het dashboard op [https://aka.ms/catou](https://aka.ms/catou)
2. Klik op Add.</br>
![Gebruiksrechtovereenkomst toevoegen](media/active-directory-tou/tou2.png)
3. Voer de **Naam** in voor de gebruiksrechtovereenkomst
4. Voer de **Weergavenaam** in.  Dit is de koptekst die gebruikers te zien krijgen wanneer ze zich aanmelden.
5. **Blader** naar de PDF met uw voltooide gebruiksrechtovereenkomst en selecteer deze.  De aanbevolen tekengrootte is 24.
6. U kunt de geüploade gebruiksrechtovereenkomst **afdwingen** door een sjabloon of beleid voor aangepaste voorwaardelijke toegang te gebruiken.  Met een beleid voor aangepaste voorwaardelijke toegang kunt u gedetailleerde gebruiksvoorwaarden opgeven voor een specifieke cloudtoepassing of groep gebruikers.  Zie [Een voorwaardelijk toegangsbeleid configureren](active-directory-conditional-access-best-practices.md) voor meer informatie
7. Klik op **Create**.
8. Als u een sjabloon voor aangepaste voorwaardelijke toegang hebt geselecteerd, kunt u in een volgend scherm het CA-beleid aanpassen.
7. Nu wordt uw nieuwe gebruiksrechtovereenkomst weergegeven.</br>

![Gebruiksrechtovereenkomst toevoegen](media/active-directory-tou/tou3.png)

## <a name="delete-terms-of-use"></a>Gebruiksrechtovereenkomst verwijderen
Met de volgende procedure kunt u oude gebruiksvoorwaarden verwijderen:

### <a name="to-delete-terms-of-use"></a>Gebruiksrechtovereenkomst verwijderen
1. Ga naar het dashboard op [https://aka.ms/catou](https://aka.ms/catou)
2. Selecteer de gebruiksrechtovereenkomst die u wilt verwijderen.
3. Klik op **Verwijderen**.
4. Nu wordt uw nieuwe gebruiksrechtovereenkomst niet meer weergegeven.


## <a name="audit-terms-of-use"></a>Gebruiksrechtovereenkomst controleren
Met Azure AD Gebruiksrechtovereenkomst kunt u eenvoudig controleren wie uw gebruiksvoorwaarden heeft geaccepteerd.  Gebruik de volgende procedure om de controle te starten:

### <a name="to-audit-terms-of-use"></a>Gebruiksrechtovereenkomst controleren
1. Ga naar het dashboard op [https://aka.ms/catou](https://aka.ms/catou)
2. Klik op Controlegebeurtenis.</br>
![Controlegebeurtenis](media/active-directory-tou/tou8.png)
3.  Op het scherm Azure AD-auditlogboeken kunt u de informatie met behulp van de beschikbare keuzelijsten filteren om specifieke auditlogboekinformatie weer te geven.
![Controlegebeurtenis](media/active-directory-tou/tou9.png)
4.  U kunt de informatie ook downloaden naar een CSV-bestand voor lokaal gebruik.

## <a name="what-users-see"></a>Wat gebruikers te zien krijgen
Gebruikers binnen het bereik krijgen het volgende te zien op het moment dat een gebruiksrechtovereenkomst is gemaakt en afgedwongen.  Tijdens de aanmelding worden deze schermen weergegeven.
-   Het wordt aangeraden om in de PDF een lettertype met tekengrootte 24 te gebruiken.
![Controlegebeurtenis](media/active-directory-tou/tou10.png)
-   Op een mobiele telefoon wordt dit scherm weergegeven</br></br>
![Controlegebeurtenis](media/active-directory-tou/tou11.png)

## <a name="additional-information"></a>Aanvullende informatie
Hier volgt informatie waarvan u zich bewust moet zijn en die nuttig kan zijn bij het gebruik van gebruiksvoorwaarden.


Gebruikers in het bereik moeten zich af- en aanmelden om te voldoen aan een nieuw beleid:
 - als een beleid voor voorwaardelijke toegang is ingeschakeld in een gebruiksrechtovereenkomst
 - of als een tweede gebruiksrechtovereenkomst is gemaakt

Dat komt omdat een beleid voor voorwaardelijke toegang onmiddellijk van kracht wordt. Als dit gebeurt, krijgt de beheerder 'sad clouds' of 'problemen met Azure AD-tokens' te zien. De beheerder moet zich afmelden en zich vervolgens opnieuw aanmelden om te voldoen aan het nieuwe beleid.





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
A: Ja. Dit doet u door voor deze groepen of apps een afzonderlijk beleid voor voorwaardelijke toegang te maken. Als een gebruiker binnen het bereik van meerdere gebruiksrechtovereenkomsten valt, gaan ze akkoord met één gebruiksrechtovereenkomst tegelijk.
 
**V: Wat gebeurt er als een gebruiker de gebruiksrechtovereenkomst afwijst?**</br>
A: De gebruiker krijgt dan geen toegang tot de toepassing. De gebruiker moet zich opnieuw aanmelden en de gebruiksrechtovereenkomst accepteren om toegang te krijgen.