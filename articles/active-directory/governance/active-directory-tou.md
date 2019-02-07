---
title: Azure Active Directory Gebruiksrechtovereenkomst| Microsoft Docs
description: Beschrijft hoe u aan de slag met Azure AD gebruiksrechtovereenkomst gebruiken om gegevens te presenteren aan werknemers of gasten voordat ze toegang krijgen.
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
ms.subservice: compliance
ms.date: 01/10/2019
ms.author: rolyon
ms.openlocfilehash: 2b8836525e085b41cc8d011087c4fc71afe453cb
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55822507"
---
# <a name="azure-active-directory-terms-of-use-feature"></a>Functie Azure Active Directory Gebruiksrechtovereenkomst
Azure AD Gebruiksrechtovereenkomst is een eenvoudige methode waarmee organisaties informatie kunnen presenteren aan eindgebruikers. Deze presentatie zorgt ervoor dat gebruikers relevante disclaimers voor juridische vereisten of nalevingsvereisten te zien krijgen. In dit artikel wordt beschreven hoe u aan de slag met de gebruiksvoorwaarden.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview-videos"></a>Overzicht van video 's

De volgende video biedt een kort overzicht van de gebruiksvoorwaarden.

>[!VIDEO https://www.youtube.com/embed/tj-LK0abNao]

Zie voor aanvullende video's:
- [Gebruiksvoorwaarden in Azure Active Directory implementeren](https://www.youtube.com/embed/N4vgqHO2tgY)
- [Hoe implementeer gebruiksvoorwaarden in Azure Active Directory](https://www.youtube.com/embed/t_hA4y9luCY)

## <a name="what-can-i-do-with-terms-of-use"></a>Wat kan ik doen met Gebruiksrechtovereenkomst?
Azure AD gebruiksrechtovereenkomst heeft de volgende mogelijkheden:
- Vereisen dat werknemers of gasten accepteert de gebruiksrechtovereenkomst voordat ze toegang krijgen.
- Vereisen dat werknemers of gasten accepteer de gebruiksvoorwaarden op elk apparaat voordat ze toegang krijgen.
- Werknemers of gasten accepteert de gebruiksrechtovereenkomst volgens een terugkerend schema vereist.
- Algemene gebruiksrechtovereenkomst presenteren voor alle gebruikers in uw organisatie.
- Specifieke gebruiksvoorwaarden presenteren op basis van gebruikerskenmerken (bijvoorbeeld artsen versus verpleegkundigen of binnenlandse werknemers versus werknemers in het buitenland) met behulp van [dynamische groepen](../users-groups-roles/groups-dynamic-membership.md).
- Specifieke gebruiksrechtovereenkomsten presenteren bij toegang tot toepassingen met grote bedrijfsimpact, zoals Salesforce.
- Aanwezig gebruiksvoorwaarden presenteren in verschillende talen.
- De lijst die is of met uw gebruiksrechtovereenkomst nog niet is geaccepteerd.
- Helpen bij het voldoen aan de regels.
- Een logboek van de voorwaarden van de activiteit gebruiken voor naleving en audits weergegeven.
- Maken en beheren van de voorwaarden van het gebruik van gebruik [Microsoft Graph-API's](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/agreement) (momenteel in Preview-versie).

## <a name="prerequisites"></a>Vereisten
U moet over het volgende beschikken om Azure AD Gebruiksrechtovereenkomst te gebruiken en te configureren:

- Abonnement op Azure AD Premium P1, P2, EMS E3 of EMS E5.
    - Als u niet over een van deze abonnementen beschikt, kunt u [Azure AD Premium downloaden](../fundamentals/active-directory-get-started-premium.md) of [Azure AD Premium uitproberen](https://azure.microsoft.com/trial/get-started-active-directory/).
- Een van de volgende beheerdersaccounts voor de directory die u wilt configureren:
    - Globale beheerder
    - Beveiligingsbeheerder
    - Voorwaardelijke toegang beheerder

## <a name="terms-of-use-document"></a>Document met gebruiksrechtovereenkomst

Voor het weergeven van inhoud maakt Azure AD Gebruiksrechtovereenkomst gebruik van de PDF-indeling. Het PDF-bestand kan van alles bevatten, zoals bestaande contracten, zodat u tijdens het aanmelden van gebruikers eindgebruikersovereenkomsten kunt verzamelen. Ter ondersteuning van gebruikers op mobiele apparaten, is de aanbevolen tekengrootte in het PDF-bestand 24 punt.

## <a name="add-terms-of-use"></a>Gebruiksrechtovereenkomst toevoegen
Wanneer u uw gebruiksrechtovereenkomstdocument hebt voltooid, gebruikt u de volgende procedure om deze toe te voegen.

1. Aanmelden bij Azure als een globale beheerder, beveiligingsbeheerder of beheerder van voorwaardelijke toegang.

1. Navigeer naar **Gebruiksrechtovereenkomst** op [https://aka.ms/catou](https://aka.ms/catou).

    ![Gebruiksrechtovereenkomst-blade](./media/active-directory-tou/tou-blade.png)

1. Klik op **Nieuwe voorwaarden**.

    ![Gebruiksrechtovereenkomst toevoegen](./media/active-directory-tou/new-tou.png)

1. In de **naam** vak, voer een naam voor de gebruiksvoorwaarden die wordt gebruikt in Azure portal.

1. In de **weergavenaam** vak, voer een titel die gebruikers wanneer ze zich zien aanmelden.

1. Voor **document met gebruiksrechtovereenkomst**, blader naar uw voltooide gebruiksrechtovereenkomst PDF-bestand en selecteert u deze.

1. Selecteer de taal voor uw document met gebruiksrechtovereenkomst. Via de taaloptie kunt u meerdere gebruiksvoorwaarden uploaden, elk met een andere taal. Welke versie van de gebruiksvoorwaarden aan een eindgebruiker wordt weergegeven, is gebaseerd op de browservoorkeuren.

1. Instellen om te vereisen dat eindgebruikers om weer te geven van de gebruiksvoorwaarden alvorens deze te accepteren, **vereisen dat gebruikers om uit te breiden de gebruiksvoorwaarden** naar **op**.

1. Instellen om te vereisen dat eindgebruikers accepteer de gebruiksvoorwaarden op elk apparaat dat ze vanuit openen, **vereisen dat gebruikers accepteren op elk apparaat** naar **op**. Zie voor meer informatie, [Per apparaat gebruiksvoorwaarden](#per-device-terms-of-use).

1. Als u wilt laten verlopen gebruiksvoorwaarden instemt volgens een schema, stelt u de **verlopen toestemmingen** naar **op**. Wanneer een ingesteld op aan, worden twee aanvullende schema-instellingen weergegeven.

    ![Toestemmingen laten verlopen](./media/active-directory-tou/expire-consents.png)

1. Gebruik de **verloopt vanaf** en **frequentie** instellingen om op te geven van de planning voor de voorwaarden van accountwachtwoorden gebruiken. De volgende tabel toont het resultaat voor een aantal voorbeeldinstellingen voor:

    | Verloopt vanaf | Frequentie | Resultaat |
    | --- | --- | --- |
    | De datum van vandaag  | Maandelijks | Vanaf vandaag, moeten gebruikers accepteer de gebruiksvoorwaarden en vervolgens accepteren elke maand. |
    | Datum in de toekomst  | Maandelijks | Vanaf vandaag, moeten gebruikers de gebruiksvoorwaarden accepteren. Wanneer de datum in de toekomst plaatsvindt, toestemmingen verloopt en vervolgens gebruikers elke maand moeten accepteren.  |

    Bijvoorbeeld, als u de verlopen starten op de datum die moet worden ingesteld **1 januari** en frequentie **maandelijkse**, dit is hoe accountwachtwoorden voor twee gebruikers zich kunnen voordoen:

    | Gebruiker | Datum eerste accepteren | Eerst verlopen datum | Ten tweede verlopen datum | Het derde verlopen datum |
    | --- | --- | --- | --- | --- |
    | Els | 1 januari | 1 februari | 1 maart | 1 april |
    | Bob | 15 januari | 1 februari | 1 maart | 1 april |

1. Gebruik de **duur voordat re-acceptatie vereist (dagen)** instelling om op te geven van het aantal dagen voordat de gebruiker moet de gebruiksvoorwaarden accepteren. Hiermee kunnen gebruikers hun eigen planning volgen. Bijvoorbeeld, als u de periode instellen op **30** dagen, dit is hoe accountwachtwoorden voor twee gebruikers zich kunnen voordoen:

    | Gebruiker | Datum eerste accepteren | Eerst verlopen datum | Ten tweede verlopen datum | Het derde verlopen datum |
    | --- | --- | --- | --- | --- |
    | Els | 1 januari | En met 31 januari | Maart 2 | 1 april |
    | Bob | 15 januari | 14 februari | 16 maart | 15 april |

    Het is mogelijk gebruik van de **toestemmingen verlopen** en **duur voordat re-acceptatie vereist (dagen)** instellingen bij elkaar, maar doorgaans een of andere gebruiken.

1. Onder **voorwaardelijke toegang**, gebruikt u de **afdwingen met voorwaardelijke toegang beleidssjabloon** lijst selecteert u de sjabloon om af te dwingen de gebruiksvoorwaarden.

    ![Sjablonen voor voorwaardelijke toegang](./media/active-directory-tou/conditional-access-templates.png)

    | Template | Description |
    | --- | --- |
    | **Toegang tot cloud-apps voor alle gasten** | Beleid voor voorwaardelijke toegang wordt gemaakt voor alle gasten en alle cloud-apps. Dit beleid heeft gevolgen voor de Azure-portal. Nadat deze is gemaakt, kunt u mogelijk vereist voor het afmelden en aanmelden. |
    | **Toegang tot cloud-apps voor alle gebruikers** | Beleid voor voorwaardelijke toegang wordt gemaakt voor alle gebruikers en alle cloud-apps. Dit beleid heeft gevolgen voor de Azure-portal. Nadat deze is gemaakt, zich kunt u afmelden en opnieuw aanmelden vereist. |
    | **Aangepast beleid** | Selecteer de gebruikers, groepen en apps waarvoor deze gebruiksvoorwaarden gelden. |
    | **Het voorwaardelijke toegangsbeleid later maken** | Deze gebruiksrechtovereenkomst wordt weergegeven in de lijst voor beheer verlenen bij het maken van een beleid voor voorwaardelijke toegang. |

    >[!IMPORTANT]
    >Controlemechanismen voor het beleid voor voorwaardelijk toegang (waaronder Gebruiksrechtovereenkomst) bieden geen ondersteuning voor handhaving op serviceaccounts. Het is raadzaam om alle serviceaccounts uit te sluiten van het beleid voor voorwaardelijke toegang.

     Met een beleid voor aangepaste voorwaardelijke toegang kunt u een gedetailleerde gebruiksrechtovereenkomst opgeven voor een specifieke cloudtoepassing of groep gebruikers. Zie voor meer informatie [Snelstart: Gebruiksrechtovereenkomst moet zijn geaccepteerd voordat u toegang tot cloud-apps vereisen](../conditional-access/require-tou.md).

1. Klik op **Create**.

    Als u een sjabloon voor aangepaste voorwaardelijke toegang hebt geselecteerd, klikt u vervolgens weergegeven een nieuw scherm waarin kunt u het beleid voor aangepaste voorwaardelijke toegang maken.

    ![Aangepast beleid](./media/active-directory-tou/custom-policy.png)

    Nu wordt uw nieuwe gebruiksrechtovereenkomst weergegeven.

    ![Gebruiksrechtovereenkomst toevoegen](./media/active-directory-tou/create-tou.png)

## <a name="view-report-of-who-has-accepted-and-declined"></a>Rapport weergeven van wie heeft geaccepteerd en geweigerd
In de Gebruiksrechtovereenkomst-blade ziet u de aantallen gebruikers die al dan niet hebben geaccepteerd. Deze aantallen en wie hebben geaccepteerd/geweigerd worden opgeslagen voor de duur van de gebruiksrechtovereenkomst.

1. Meld u aan bij Azure en navigeer naar **Gebruiksrechtovereenkomst** op [https://aka.ms/catou](https://aka.ms/catou).

    ![Gebruiksrechtovereenkomst-blade](./media/active-directory-tou/view-tou.png)

1. Voor een gebruiksrechtovereenkomst, klikt u op de getallen onder **geaccepteerde** of **geweigerd** om de huidige status voor gebruikers weer te geven.

    ![Instemming voor gebruiksrechtovereenkomst](./media/active-directory-tou/accepted-tou.png)

1. Als u wilt de geschiedenis van een afzonderlijke gebruiker weergeven, klikt u op het weglatingsteken (**...** ) en vervolgens **geschiedenis weergeven**.

    ![Menu Geschiedenis weergeven](./media/active-directory-tou/view-history-menu.png)

    In het deelvenster van de geschiedenis van weergave ziet u een overzicht van alle de dalingen en accountwachtwoorden accepteert.

    ![Geschiedenis-deelvenster weergeven](./media/active-directory-tou/view-history-pane.png)

## <a name="view-azure-ad-audit-logs"></a>Auditlogboeken weergeven Azure AD
Als u aanvullende activiteiten wilt bekijken, bevat Azure AD Gebruiksrechtovereenkomst auditlogboeken. Elke toestemming van de gebruiker activeert een gebeurtenis in de auditlogboeken die zijn opgeslagen voor **30 dagen**. U kunt deze logboeken bekijken in de portal of downloaden als CSV-bestand.

Aan de slag met Azure AD controlelogboeken, gebruikt u de volgende procedure:

1. Meld u aan bij Azure en navigeer naar **Gebruiksrechtovereenkomst** op [https://aka.ms/catou](https://aka.ms/catou).

1. Selecteer een gebruiksrechtovereenkomst.

1. Klik op **Auditlogboeken weergeven**.

    ![Gebruiksrechtovereenkomst-blade](./media/active-directory-tou/audit-tou.png)

1. Op de Azure AD controleren logboeken scherm, kunt u de gegevens met behulp van de opgegeven lijsten om gegevens van specifieke auditlogboeken doel te filteren.

    U kunt ook op **Downloaden** klikken om de informatie te downloaden naar een CSV-bestand voor lokaal gebruik.

    ![Controlelogboeken](./media/active-directory-tou/audit-logs-tou.png)

    Als u een logboek klikt, wordt een deelvenster met details van de extra activiteiten wordt weergegeven.

    ![details van computeractiviteit](./media/active-directory-tou/audit-log-activity-details.png)

## <a name="what-terms-of-use-looks-like-for-users"></a>Hoe Gebruiksrechtovereenkomst eruitziet voor gebruikers
Zodra een gebruiksrechtovereenkomst is gemaakt en wordt afgedwongen, zien gebruikers die binnen het bereik vallen het volgende scherm tijdens het inloggen.

![Web aanmelden van gebruikers](./media/active-directory-tou/user-tou.png)

Het volgende scherm laat zien hoe Gebruiksrechtovereenkomst eruitziet op mobiele apparaten.

![Mobiele aanmelden van gebruikers](./media/active-directory-tou/mobile-tou.png)

Gebruikers zijn alleen vereist om de gebruiksvoorwaarden accepteren één keer te zien ze de gebruiksvoorwaarden opnieuw op de volgende aanmeldingen.

### <a name="how-users-can-review-their-terms-of-use"></a>Hoe gebruikers hun gebruiksvoorwaarden kunnen bekijken
Gebruikers kunnen de gebruiksvoorwaarden die ze hebben geaccepteerd, controleren en inzien met behulp van de volgende procedure.

1. Meld u aan bij [https://myapps.microsoft.com](https://myapps.microsoft.com).

1. In de rechterbovenhoek, klikt u op uw naam en selecteer **profiel**.

    ![Profiel](./media/active-directory-tou/tou14.png)

1. Klik in uw profielpagina op **Gebruiksvoorwaarden controleren**.

    ![Profiel - gebruiksvoorwaarden controleren](./media/active-directory-tou/tou13a.png)

1. Van hieruit kunt u de gebruiksrechtovereenkomst bekijken die u hebt geaccepteerd.

## <a name="edit-terms-of-use-details"></a>Gegevens van gebruiksvoorwaarden bewerken
U kunt sommige gegevens van gebruiksvoorwaarden bewerken, maar u kunt een bestaand document niet wijzigen. De volgende procedure wordt beschreven hoe u de details bewerken.

1. Meld u aan bij Azure en navigeer naar **Gebruiksrechtovereenkomst** op [https://aka.ms/catou](https://aka.ms/catou).

1. Selecteer de gebruiksvoorwaarden die u wilt bewerken.

1. Klik op **gebruiksvoorwaarden bewerken**.

1. In de voorwaarden bewerken van deelvenster gebruikt, wijzigt u de naam, weergavenaam of vereisen dat gebruikers om uit te breiden waarden.

    Als er andere instellingen die u wijzigen wilt, zoals PDF-document, vereisen dat gebruikers accepteren op elk apparaat, verlopen toestemmingen, duur voordat reacceptance of beleid voor voorwaardelijke toegang, moet u een nieuwe gebruiksvoorwaarden maken.

    ![Gebruiksvoorwaarden bewerken](./media/active-directory-tou/edit-tou.png)

1. Klik op **opslaan** uw wijzigingen op te slaan.

    Nadat u uw wijzigingen opslaat, worden gebruikers niet hebben deze wijzigingen onderhevig.

## <a name="add-a-terms-of-use-language"></a>Een gebruiksrechtovereenkomst gebruik taal toevoegen
De volgende procedure wordt beschreven hoe u een gebruiksrechtovereenkomst gebruik taal toevoegen.

1. Meld u aan bij Azure en navigeer naar **Gebruiksrechtovereenkomst** op [https://aka.ms/catou](https://aka.ms/catou).

1. Selecteer de gebruiksvoorwaarden die u wilt bewerken.

1. Klik in het detailvenster op de **talen** tabblad.

    ![Gebruiksrechtovereenkomst toevoegen](./media/active-directory-tou/languages-tou.png)

1. Klik op **taal toevoegen**.

1. In de voorwaarden toevoegen van gebruik taal deelvenster, uw gelokaliseerde PDF-bestand uploaden en selecteer de taal.

    ![Gebruiksrechtovereenkomst toevoegen](./media/active-directory-tou/language-add-tou.png)

1. Klik op **toevoegen** om toe te voegen van de taal.

## <a name="per-device-terms-of-use"></a>Gebruiksvoorwaarden per apparaat

De **vereisen dat gebruikers accepteren op elk apparaat** instelling kunt u vereisen dat eindgebruikers accepteer de gebruiksvoorwaarden op elk apparaat dat ze vanuit openen. De eindgebruiker moet naar het toevoegen van hun apparaat in Azure AD. Wanneer het apparaat is toegevoegd, wordt de apparaat-ID wordt gebruikt om af te dwingen de gebruiksvoorwaarden op elk apparaat.

Hier volgt een lijst van de ondersteunde platformen en software.

> [!div class="mx-tableFixed"]
> |  | iOS | Android | Windows 10 | Overige |
> | --- | --- | --- | --- | --- |
> | **Systeemeigen app** | Ja | Ja | Ja |  |
> | **Microsoft Edge** | Ja | Ja | Ja |  |
> | **Internet Explorer** | Ja | Ja | Ja |  |
> | **Chrome (met de extensie)** | Ja | Ja | Ja |  |

Gebruiksvoorwaarden per apparaat heeft de volgende beperkingen:

- Een apparaat kan alleen worden samengevoegd in één tenant.
- Een gebruiker moet machtigingen voor het toevoegen van hun apparaat hebben.
- De app Intune-inschrijving wordt niet ondersteund.

Als het apparaat van de gebruiker niet is toegevoegd, ontvangt deze een bericht die ze nodig hebben om hun apparaat. Hun ervaring zijn afhankelijk van het platform en de software.

### <a name="join-a-windows-10-device"></a>Een Windows 10-apparaat toevoegen

Als een gebruiker wordt met behulp van Windows 10 en Microsoft Edge, ontvangt deze een bericht dat lijkt op de volgende [toevoegen van hun apparaat](../user-help/user-help-join-device-on-network.md#to-join-an-already-configured-windows-10-device).

![Windows 10 en Microsoft Edge - apparaat een prompt Join](./media/active-directory-tou/per-device-win10-edge.png)

Als ze met chrome werkt, wordt ze gevraagd voor het installeren van de [Windows 10-Accounts extensie](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).

### <a name="browsers"></a>Browsers

Als een gebruiker via de browser die niet wordt ondersteund, wordt ze gevraagd een andere browser te gebruiken.

![Niet-ondersteunde browser](./media/active-directory-tou/per-device-browser-unsupported.png)

## <a name="delete-terms-of-use"></a>Gebruiksrechtovereenkomst verwijderen
Met de volgende procedure kunt u een oude gebruiksrechtovereenkomst verwijderen.

1. Meld u aan bij Azure en navigeer naar **Gebruiksrechtovereenkomst** op [https://aka.ms/catou](https://aka.ms/catou).

1. Selecteer de gebruiksrechtovereenkomst die u wilt verwijderen.

1. Klik op **Gebruiksvoorwaarden verwijderen**.

1. Klik in het bericht waarin u wordt gevraagd of u wilt doorgaan op **Ja**.

    ![Gebruiksrechtovereenkomst verwijderen](./media/active-directory-tou/delete-tou.png)

    Nu wordt uw gebruiksrechtovereenkomst niet meer weergegeven.

## <a name="deleted-users-and-active-terms-of-use"></a>Verwijderde gebruikers en actieve gebruiksrechtovereenkomst
Standaard blijft een verwijderde gebruiker gedurende 30 dagen in Azure AD aanwezig met de status Verwijderd. In deze periode kan de gebruiker eventueel door een beheerder worden teruggezet. Na dertig dagen wordt die gebruiker definitief verwijderd. Daarnaast kan een hoofdbeheerder via de Azure Active Directory-portal expliciet [een recentelijk verwijderde gebruiker definitief verwijderen](../fundamentals/active-directory-users-restore.md) voordat de dertig dagen zijn verstreken. Als een gebruiker definitief is verwijderd, worden latere gegevens over die gebruiker uit de actieve gebruiksrechtovereenkomst verwijderd. Controle-informatie over verwijderde gebruikers blijft in het auditlogboek aanwezig.

## <a name="policy-changes"></a>Beleidswijzigingen
Beleid voor voorwaardelijke toegang wordt onmiddellijk van kracht. Als dit gebeurt, gaan de beheerder 'verdrietige clouds' of 'Token problemen met Azure AD'. De beheerder moet zich afmelden en opnieuw aanmelden om te voldoen aan het nieuwe beleid.

>[!IMPORTANT]
> Gebruikers in het bereik moeten zich af- en aanmelden om te voldoen aan een nieuw beleid:
> - als een beleid voor voorwaardelijke toegang wordt ingeschakeld voor een gebruiksrechtovereenkomst
> - of als er een tweede gebruiksrechtovereenkomst wordt gemaakt

## <a name="b2b-guests-preview"></a>B2B-gasten (Preview)

De meeste organisaties beschikken over een proces voor hun werknemers akkoord gaan met de voorwaarden van gebruik en privacyverklaringen van hun organisatie. Maar hoe kunt u afdwingen de dezelfde toestemmingen voor Azure AD business-to-business (B2B) gasten wanneer ze worden toegevoegd via SharePoint of Teams? Met behulp van voorwaardelijke toegang en gebruiksvoorwaarden, kunt u een beleid rechtstreeks voor B2B-gastgebruikers afdwingen. Tijdens de uitnodiging inwisselen stroom, wordt de gebruiker weergegeven met de gebruiksvoorwaarden. Deze ondersteuning is momenteel in preview.

Gebruiksvoorwaarden wordt alleen weergegeven wanneer de gebruiker een Gast-account in Azure AD heeft. SharePoint Online op dit moment heeft een [ad-hoc extern delen geadresseerde ervaring](/sharepoint/what-s-new-in-sharing-in-targeted-release) voor het delen van een document of een map waarvoor de gebruiker een gastaccount hebben. In dit geval wordt een gebruiksrechtovereenkomst niet weergegeven.

![Alle gastgebruikers](./media/active-directory-tou/b2b-guests.png)

## <a name="support-for-cloud-apps-preview"></a>Ondersteuning voor cloud-apps (Preview)

Gebruiksvoorwaarden kunnen worden gebruikt voor verschillende cloud-apps, zoals Azure Information Protection en Microsoft Intune. Deze ondersteuning is momenteel in preview.

### <a name="azure-information-protection"></a>Azure Information Protection

U kunt een beleid voor voorwaardelijke toegang voor de Azure Information Protection-app configureren en een gebruiksrechtovereenkomst vereisen wanneer een gebruiker toegang heeft tot een beveiligd document. Hiermee wordt een gebruiksrechtovereenkomst voordat een gebruiker toegang tot een beveiligd document voor de eerste keer geactiveerd.

![Azure Information Protection-cloud-app](./media/active-directory-tou/cloud-app-info-protection.png)

### <a name="microsoft-intune-enrollment"></a>Microsoft Intune-inschrijving

U kunt een beleid voor voorwaardelijke toegang voor de app Microsoft Intune-inschrijving configureren en vereisen een gebruiksrechtovereenkomst voordat u de inschrijving van een apparaat bij Intune. Raadpleeg voor meer informatie het [oplossing voor uw organisatie blog-bericht het recht kiezen voorwaarden](https://go.microsoft.com/fwlink/?linkid=2010506&clcid=0x409).

![Cloud-app voor Microsoft Intune](./media/active-directory-tou/cloud-app-intune.png)

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**V: Hoe kan ik zien wanneer / of een gebruiker een gebruiksrechtovereenkomst heeft geaccepteerd?**<br />
A: Klik op de voorwaarden van de blade gebruik, het aantal onder **geaccepteerde**. U kunt ook weergeven of zoeken naar de accept-activiteit in de Azure AD auditlogboeken. Zie voor meer informatie weergeven rapport wie heeft geaccepteerd en geweigerd en [weergave Azure AD-auditlogboeken](#view-azure-ad-audit-logs).

**V: Hoe lang wordt informatie opgeslagen?**<br />
A: De gebruiker wordt geteld in de voorwaarden van het rapport gebruiken en die voor de levensduur van de gebruiksrechtovereenkomst geaccepteerd/afgewezen worden opgeslagen. De Azure AD-audit logboeken worden opgeslagen voor 30 dagen.

**V: Waarom zie ik een ander aantal toestemmingen in de voorwaarden van gebruik rapport vergeleken met de Azure AD auditlogboeken?**<br />
A: De voorwaarden van gebruik rapport wordt opgeslagen voor de levensduur van deze gebruiksrechtovereenkomst, terwijl de Azure AD-audit logboeken worden opgeslagen voor 30 dagen. Ook de voorwaarden van gebruik rapport alleen de huidige toestemming-status van gebruikers weergegeven. Bijvoorbeeld, als een gebruiker geweigerd en vervolgens accepteert de gebruiksrechtovereenkomst gebruik rapport wordt alleen weergegeven van die gebruiker accepteren. Als u nodig hebt om te zien van de geschiedenis, kunt u de Azure AD auditlogboeken.

**V: Als het bewerken van de details voor een gebruiksrechtovereenkomst, moeten gebruikers opnieuw accepteren?**<br />
A: Nee, als een beheerder de details van de bewerkingen voor een gebruiksrechtovereenkomst (naam, weergavenaam, vereisen dat gebruikers om uit te breiden of een taal toevoegen), hoeven er geen gebruikers aan de nieuwe voorwaarden accepteren.

**V: Kan ik een document met bestaande gebruiksrechtovereenkomst bijwerken?**<br />
A: U kunt een document met bestaande gebruiksrechtovereenkomst op dit moment niet bijwerken. Als u wilt wijzigen van een document met gebruiksrechtovereenkomst, wordt u moet maken van een nieuwe gebruiksrechtovereenkomst gebruik exemplaar.

**V: Als hyperlinks in PDF-document met de gebruiksrechtovereenkomst, eindgebruikers zich erop klikt?**<br />
A: Het PDF-bestand wordt standaard weergegeven als een JPEG, zodat hyperlinks niet geklikt. Gebruikers hebben de optie te selecteren **hebt u problemen met weergeven? Klik hier**, die wordt weergegeven het PDF-bestand systeemeigen waar hyperlinks worden ondersteund.

**V: Kunnen een gebruiksrechtovereenkomst ondersteund in meerdere talen?**<br />
A: Ja. Er zijn momenteel 108 verschillende talen een beheerder voor één configureren kan gebruiksrechtovereenkomst. Een beheerder kan meerdere PDF-documenten te uploaden en code die documenten met een bijbehorende taal (maximaal 108). Wanneer eindgebruikers zich aanmelden, we kijken naar de Taalvoorkeur van hun browser en het overeenkomende document wordt weergegeven. Als er geen overeenkomst, geven we weer het standaarddocument, is het eerste document dat is geüpload.

**V: Wanneer is de gebruiksrechtovereenkomst geactiveerd?**<br />
A: De gebruiksrechtovereenkomst wordt geactiveerd tijdens de aanmeldingsprocedure.

**V: Welke toepassingen kan ik een gebruiksrechtovereenkomst te richten?**<br />
A: U kunt een beleid voor voorwaardelijke toegang maken voor bedrijfstoepassingen die moderne authenticatie gebruiken. Zie [Bedrijfstoepassingen](./../manage-apps/view-applications-portal.md) voor meer informatie.

**V: Kan ik meerdere gebruiksrechtovereenkomsten toevoegen aan een bepaalde gebruiker of de app?**<br />
A: Ja, met het maken van meerdere beleidsregels voor voorwaardelijke toegang die gericht is op deze groepen of toepassingen. Als een gebruiker binnen het bereik van meerdere gebruiksrechtovereenkomsten valt, accepteren ze één gebruiksrechtovereenkomst tegelijk.

**V: Wat gebeurt er als een gebruiker de gebruiksrechtovereenkomst afwijst?**<br />
A: De gebruiker is krijgt dan geen toegang tot de toepassing geblokkeerd. De gebruiker moet zich opnieuw aanmelden en de voorwaarden accepteren om toegang te krijgen.

**V: Is het mogelijk om de gebruiksvoorwaarden die eerder zijn geaccepteerd unaccept?**<br />
A: U kunt [revisie eerder gebruiksrechtovereenkomst geaccepteerd](#how-users-can-review-their-terms-of-use), maar er is momenteel een manier te unaccept.

**V: Wat gebeurt er als ik ook Intune-voorwaarden en bepalingen?**<br />
A: Als u beide Azure AD gebruiksrechtovereenkomst hebt geconfigureerd en [Intune bepalingen en voorwaarden](/intune/terms-and-conditions-create), de gebruiker moet beide accepteren. Zie voor meer informatie de [oplossing voor uw organisatie blog-bericht het recht kiezen voorwaarden](https://go.microsoft.com/fwlink/?linkid=2010506&clcid=0x409).

## <a name="next-steps"></a>Volgende stappen

- [Snelstart: Gebruiksrechtovereenkomst moet zijn geaccepteerd voordat u toegang tot cloud-apps vereisen](../conditional-access/require-tou.md)
- [Best practices for conditional access in Azure Active Directory](../conditional-access/best-practices.md) (Best practices voor voorwaardelijke toegang in Azure Active Directory)
