---
title: Azure Active Directory Gebruiksrechtovereenkomst| Microsoft Docs
description: Azure AD Gebruiksrechtovereenkomst biedt u en uw bedrijf de mogelijkheid gebruikers van Azure AD-services een gebruiksrechtovereenkomst aan te bieden.
services: active-directory
author: rolyon
manager: mtillman
editor: ''
ms.assetid: d55872ef-7e45-4de5-a9a0-3298e3de3565
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: compliance-reports
ms.date: 07/18/2018
ms.author: rolyon
ms.openlocfilehash: b1a11fdb685b8b4e5c513931269c479506d943a0
ms.sourcegitcommit: dc646da9fbefcc06c0e11c6a358724b42abb1438
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39136652"
---
# <a name="azure-active-directory-terms-of-use-feature"></a>Functie Azure Active Directory Gebruiksrechtovereenkomst
Azure AD Gebruiksrechtovereenkomst is een eenvoudige methode waarmee organisaties informatie kunnen presenteren aan eindgebruikers. Deze presentatie zorgt ervoor dat gebruikers relevante disclaimers voor juridische vereisten of nalevingsvereisten te zien krijgen. In dit artikel wordt beschreven hoe u aan de slag kunt gaan met Azure AD Gebruiksrechtovereenkomst.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-can-i-do-with-terms-of-use"></a>Wat kan ik doen met Gebruiksrechtovereenkomst?
Met Azure AD Gebruiksrechtovereenkomst kunt u het volgende doen:
- Werknemers of gasten dwingen akkoord te gaan met uw gebruiksrechtovereenkomst voordat ze toegang krijgen.
- Algemene gebruiksrechtovereenkomst presenteren voor alle gebruikers in uw organisatie.
- Specifieke gebruiksvoorwaarden presenteren op basis van gebruikerskenmerken (bijvoorbeeld artsen versus verpleegkundigen of binnenlandse werknemers versus werknemers in het buitenland) met behulp van [dynamische groepen](users-groups-roles/groups-dynamic-membership.md).
- Specifieke gebruiksrechtovereenkomsten presenteren bij toegang tot toepassingen met grote bedrijfsimpact, zoals Salesforce.
- Aanwezig gebruiksvoorwaarden presenteren in verschillende talen.
- Een lijst maken met personen die wel of niet akkoord zijn gegaan met uw Gebruiksrechtovereenkomst.
- Een controlelogboek weergeven van Gebruiksrechtovereenkomst-activiteiten.

## <a name="prerequisites"></a>Vereisten
U moet over het volgende beschikken om Azure AD Gebruiksrechtovereenkomst te gebruiken en te configureren:

- Abonnement op Azure AD Premium P1, P2, EMS E3 of EMS E5.
    - Als u niet over een van deze abonnementen beschikt, kunt u [Azure AD Premium downloaden](fundamentals/active-directory-get-started-premium.md) of [Azure AD Premium uitproberen](https://azure.microsoft.com/trial/get-started-active-directory/).
- Een van de volgende beheerdersaccounts voor de directory die u wilt configureren:
    - Globale beheerder
    - Beveiligingsbeheerder
    - Voorwaardelijke-toegangsbeheerder

## <a name="terms-of-use-document"></a>Document met gebruiksrechtovereenkomst

Voor het weergeven van inhoud maakt Azure AD Gebruiksrechtovereenkomst gebruik van de PDF-indeling. Het PDF-bestand mag inhoud, zoals bestaande bestek, zodat u kunt voor het verzamelen van eindgebruikers overeenkomsten tijdens de gebruiker zich aanmeldt. De aanbevolen tekengrootte voor de PDF is 24.

## <a name="add-terms-of-use"></a>Gebruiksrechtovereenkomst toevoegen
Wanneer u uw gebruiksrechtovereenkomstdocument hebt voltooid, gebruikt u de volgende procedure om deze toe te voegen.

1. Meld u aan bij Azure als globale beheerder, beveiligingsbeheerder of voorwaardelijke-toegangsbeheerder.

1. Navigeer naar **Gebruiksrechtovereenkomst** op [https://aka.ms/catou](https://aka.ms/catou).

    ![Gebruiksrechtovereenkomst-blade](media/active-directory-tou/tou-blade.png)

1. Klik op **Nieuwe voorwaarden**.

    ![Gebruiksrechtovereenkomst toevoegen](media/active-directory-tou/new-tou.png)

1. Voer de **Naam** in voor de gebruiksrechtovereenkomst

2. Voer de **Weergavenaam** in.  Dit is de koptekst die gebruikers te zien krijgen wanneer ze zich aanmelden.

3. **Blader** naar de PDF met uw voltooide gebruiksrechtovereenkomst en selecteer deze.

4. **Selecteer** een taal voor de Gebruiksrechtovereenkomst.  Via de taaloptie kunt u meerdere gebruiksvoorwaarden uploaden, elk met een andere taal.  Welke versie van de gebruiksvoorwaarden aan een eindgebruiker wordt weergegeven, is gebaseerd op de browservoorkeuren.

5. Selecteer Aan of Uit bij de optie **Gebruikers verplichten de gebruiksvoorwaarden uit te vouwen**.  Als deze instelling Aan staat, moeten eindgebruikers de gebruiksrechtovereenkomst bekijken alvorens ze deze accepteren.

6. Onder **Voorwaardelijke toegang** kunt u de geüploade gebruiksrechtovereenkomst **afdwingen** door in de vervolgkeuzelijst een sjabloon of beleid voor aangepaste voorwaardelijke toegang te selecteren.  Met een beleid voor aangepaste voorwaardelijke toegang kunt u een gedetailleerde gebruiksrechtovereenkomst opgeven voor een specifieke cloudtoepassing of groep gebruikers.  Zie [Een voorwaardelijk toegangsbeleid configureren](active-directory-conditional-access-best-practices.md) voor meer informatie.

    >[!IMPORTANT]
    >Controlemechanismen voor het beleid voor voorwaardelijk toegang (waaronder Gebruiksrechtovereenkomst) bieden geen ondersteuning voor handhaving op serviceaccounts.  Het is raadzaam om alle serviceaccounts uit te sluiten van het beleid voor voorwaardelijke toegang.

7. Klik op **Create**.

8. Als u een sjabloon voor aangepaste voorwaardelijke toegang hebt geselecteerd, kunt u in een volgend scherm het beleid voor voorwaardelijke toegang aanpassen.

    Nu wordt uw nieuwe gebruiksrechtovereenkomst weergegeven.

    ![Gebruiksrechtovereenkomst toevoegen](media/active-directory-tou/create-tou.png)

## <a name="view-report-of-who-has-accepted-and-declined"></a>Rapport weergeven van wie heeft geaccepteerd en geweigerd
In de Gebruiksrechtovereenkomst-blade ziet u de aantallen gebruikers die al dan niet hebben geaccepteerd. Deze aantallen en wie hebben geaccepteerd/geweigerd worden opgeslagen voor de duur van de gebruiksrechtovereenkomst.

1. Meld u aan bij Azure en navigeer naar **Gebruiksrechtovereenkomst** op [https://aka.ms/catou](https://aka.ms/catou).

    ![Controlegebeurtenis](media/active-directory-tou/view-tou.png)

1. Klik op de getallen onder **Geaccepteerd** of **Geweigerd** om de huidige status van gebruikers te zien.

    ![Controlegebeurtenis](media/active-directory-tou/accepted-tou.png)

## <a name="view-azure-ad-audit-logs"></a>Auditlogboeken weergeven Azure AD
Als u aanvullende activiteiten wilt bekijken, bevat Azure AD Gebruiksrechtovereenkomst auditlogboeken. Elke toestemming van de gebruiker wordt een gebeurtenis in de auditlogboeken die zijn opgeslagen voor 30 dagen geactiveerd. U kunt deze logboeken bekijken in de portal of downloaden als CSV-bestand.

Aan de slag met Azure AD controlelogboeken, gebruikt u de volgende procedure:

1. Meld u aan bij Azure en navigeer naar **Gebruiksrechtovereenkomst** op [https://aka.ms/catou](https://aka.ms/catou).

1. Klik op **Auditlogboeken weergeven**.

    ![Controlegebeurtenis](media/active-directory-tou/audit-tou.png)

1. In het scherm Azure AD-auditlogboeken kunt u de informatie met behulp van de beschikbare keuzelijsten filteren om specifieke auditlogboekinformatie weer te geven.

    ![Controlegebeurtenis](media/active-directory-tou/audit-logs-tou.png)

1. U kunt ook op **Downloaden** klikken om de informatie te downloaden naar een CSV-bestand voor lokaal gebruik.

## <a name="what-terms-of-use-looks-like-for-users"></a>Hoe Gebruiksrechtovereenkomst eruitziet voor gebruikers
Zodra een gebruiksrechtovereenkomst is gemaakt en afgedwongen, zien gebruikers, die binnen het bereik, het volgende scherm tijdens het aanmelden.

![Controlegebeurtenis](media/active-directory-tou/user-tou.png)

Het volgende scherm laat zien hoe Gebruiksrechtovereenkomst eruitziet op mobiele apparaten.

![Controlegebeurtenis](media/active-directory-tou/mobile-tou.png)

### <a name="how-users-can-review-their-terms-of-use"></a>Hoe gebruikers hun gebruiksvoorwaarden kunnen bekijken
Gebruikers kunnen de gebruiksvoorwaarden die ze hebben geaccepteerd, controleren en inzien met behulp van de volgende procedure.

1. Aanmelden bij [ https://myapps.microsoft.com ](https://myapps.microsoft.com).

1. Klik in de rechterbovenhoek op uw naam en selecteer **Profiel** in de vervolgkeuzelijst.

    ![Profiel](media/active-directory-tou/tou14.png)

1. Klik in uw profielpagina op **Gebruiksvoorwaarden controleren**.

    ![Controlegebeurtenis](media/active-directory-tou/tou13a.png)

1. Van hieruit kunt u de gebruiksrechtovereenkomst bekijken die u hebt geaccepteerd. 

## <a name="delete-terms-of-use"></a>Gebruiksrechtovereenkomst verwijderen
Met de volgende procedure kunt u een oude gebruiksrechtovereenkomst verwijderen.

1. Meld u aan bij Azure en navigeer naar **Gebruiksrechtovereenkomst** op [https://aka.ms/catou](https://aka.ms/catou).

1. Selecteer de gebruiksrechtovereenkomst die u wilt verwijderen.

1. Klik op **Gebruiksvoorwaarden verwijderen**.

1. Klik in het bericht waarin u wordt gevraagd of u wilt doorgaan op **Ja**.

    ![Gebruiksrechtovereenkomst toevoegen](media/active-directory-tou/delete-tou.png)

    Nu wordt uw gebruiksrechtovereenkomst niet meer weergegeven.

## <a name="deleted-users-and-active-terms-of-use"></a>Verwijderde gebruikers en actieve gebruiksrechtovereenkomst
Standaard blijft een verwijderde gebruiker gedurende 30 dagen in Azure AD aanwezig met de status Verwijderd. In deze periode kan de gebruiker eventueel door een beheerder worden teruggezet.  Na dertig dagen wordt die gebruiker definitief verwijderd.  Daarnaast kan een globale beheerder via de Azure Active Directory-portal expliciet [een recentelijk verwijderde gebruiker definitief verwijderen](fundamentals/active-directory-users-restore.md) voordat de dertig dagen zijn verstreken.  Als een gebruiker definitief is verwijderd, worden latere gegevens over die gebruiker uit de actieve gebruiksrechtovereenkomst verwijderd.  Controle-informatie over verwijderde gebruikers blijft in het auditlogboek aanwezig.

## <a name="policy-changes"></a>Beleidswijzigingen
Beleid voor voorwaardelijke toegang wordt onmiddellijk van kracht. Als dit gebeurt, gaan de beheerder 'verdrietige clouds' of 'Token problemen met Azure AD'. De beheerder moet zich afmelden en opnieuw aanmelden om te voldoen aan het nieuwe beleid.

>[!IMPORTANT]
> Gebruikers in het bereik moeten zich af- en aanmelden om te voldoen aan een nieuw beleid:
> - als een beleid voor voorwaardelijke toegang wordt ingeschakeld voor een gebruiksrechtovereenkomst
> - of als er een tweede gebruiksrechtovereenkomst wordt gemaakt

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**V: Hoe kan ik zien wanneer/of een gebruiker een gebruiksrechtovereenkomst heeft geaccepteerd?**</br>
A: klik op de voorwaarden van de blade gebruiken, klikt u op het aantal onder **geaccepteerde**. U kunt ook weergeven of zoeken naar de accept-activiteit in de Azure AD auditlogboeken. Zie voor meer informatie, [rapport weergeven van wie heeft geaccepteerd en afgewezen](#view-who-has-accepted-and-declined) en [weergave Azure AD-auditlogboeken](#view-azure-ad-audit-logs).
 
**V: hoe lang worden gegevens opgeslagen?**</br>
A: de gebruiker wordt geteld in de voorwaarden van het rapport gebruiken en die voor de levensduur van de gebruiksrechtovereenkomst geaccepteerd/afgewezen worden opgeslagen. De Azure AD-audit logboeken worden opgeslagen voor 30 dagen.

**V: Waarom zie ik een ander aantal toestemmingen in de voorwaarden van gebruik rapport vergeleken met de Azure AD auditlogboeken?**</br>
A: de gebruiksrechtovereenkomst gebruik rapport wordt opgeslagen voor de levensduur van deze gebruiksrechtovereenkomst, terwijl de Azure AD-audit logboeken worden opgeslagen voor 30 dagen. Ook de voorwaarden van gebruik rapport alleen de huidige toestemming-status van gebruikers weergegeven. Bijvoorbeeld, als een gebruiker geweigerd en vervolgens accepteert de gebruiksrechtovereenkomst gebruik rapport wordt alleen weergegeven van die gebruiker accepteren. Als u nodig hebt om te zien van de geschiedenis, kunt u de Azure AD auditlogboeken.

**V: als ik van de gebruiksrechtovereenkomst wijzigt,, moeten gebruikers opnieuw accepteren?**</br>
A: Ja, kan een beheerder van de gebruiksrechtovereenkomst wijzigen en het vereist dat gebruikers de nieuwe voorwaarden accepteren.

**V: als hyperlinks in PDF-document met de gebruiksrechtovereenkomst, eindgebruikers zich erop klikt?**</br>
A: het PDF-bestand wordt standaard weergegeven als een JPEG, zodat hyperlinks niet geklikt. Gebruikers hebben de optie te selecteren **hebt u problemen met weergeven? Klik hier**, die wordt weergegeven het PDF-bestand systeemeigen waar hyperlinks worden ondersteund.

**V: Wordt een gebruiksrechtovereenkomst in meerdere talen ondersteund?**</br>
A: Ja.  Er zijn momenteel 18 verschillende talen die een beheerder voor één gebruiksrechtovereenkomst kan configureren. 

**V: Wanneer wordt de gebruiksrechtovereenkomst geactiveerd?**</br>
A: De gebruiksrechtovereenkomst wordt geactiveerd tijdens het aanmelden.

**V: Voor welke toepassingen kan ik een gebruiksrechtovereenkomst implementeren?**</br>
A: U kunt u een beleid voor voorwaardelijke toegang maken voor bedrijfstoepassingen die gebruikmaken van moderne verificatie.  Zie [Bedrijfstoepassingen](./manage-apps/view-applications-portal.md) voor meer informatie.

**V: Kan ik meerdere gebruiksrechtovereenkomsten toevoegen voor een bepaalde gebruiker of app?**</br>
A: Ja. Dit doet u door voor deze groepen of toepassingen een afzonderlijk beleid voor voorwaardelijke toegang te maken. Als gebruikers binnen het bereik van meerdere gebruiksrechtovereenkomsten vallen, gaan ze akkoord met één gebruiksrechtovereenkomst tegelijk.
 
**V: Wat gebeurt er als een gebruiker de gebruiksrechtovereenkomst afwijst?**</br>
A: De gebruiker krijgt dan geen toegang tot de toepassing. De gebruiker moet zich opnieuw aanmelden en ga akkoord met de voorwaarden om toegang te krijgen.
 
**Vraag: is het mogelijk om de gebruiksvoorwaarden die eerder zijn geaccepteerd unaccept?**</br>
A: u kunt [revisie geaccepteerd eerder gebruiksvoorwaarden](#how-users-can-review-their-terms-of-use), maar er is momenteel een manier te unaccept.

## <a name="next-steps"></a>Volgende stappen

- [Best practices for conditional access in Azure Active Directory](active-directory-conditional-access-best-practices.md) (Best practices voor voorwaardelijke toegang in Azure Active Directory)