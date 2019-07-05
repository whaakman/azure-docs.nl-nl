---
title: Gebruiksvoorwaarden - Azure Active Directory | Microsoft Docs
description: Aan de slag met Azure Active Directory gebruiksrechtovereenkomst gebruiken om gegevens te presenteren aan werknemers of gasten voordat ze toegang krijgen.
services: active-directory
ms.service: active-directory
ms.subservice: compliance
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jocastel
ms.collection: M365-identity-device-management
ms.openlocfilehash: f891642761a2f692158efbd9111ff96444c4269d
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476192"
---
# <a name="azure-active-directory-terms-of-use"></a>Azure Active Directory-gebruiksvoorwaarden

Azure AD gebruiksrechtovereenkomst biedt een eenvoudige methode die organisaties kunnen gebruiken om gegevens te presenteren aan eindgebruikers. Deze presentatie zorgt ervoor dat gebruikers relevante disclaimers voor juridische vereisten of nalevingsvereisten te zien krijgen. In dit artikel wordt beschreven hoe u aan de slag met de gebruiksvoorwaarden.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="overview-videos"></a>Overzicht van video 's

De volgende video biedt een kort overzicht van de gebruiksvoorwaarden.

>[!VIDEO https://www.youtube.com/embed/tj-LK0abNao]

Zie voor aanvullende video's:
- [Gebruiksvoorwaarden in Azure Active Directory implementeren](https://www.youtube.com/embed/N4vgqHO2tgY)
- [Hoe implementeer gebruiksvoorwaarden in Azure Active Directory](https://www.youtube.com/embed/t_hA4y9luCY)

## <a name="what-can-i-do-with-terms-of-use"></a>Wat kan ik doen met de gebruiksvoorwaarden

Azure AD gebruiksrechtovereenkomst heeft de volgende mogelijkheden:

- Vereisen dat werknemers of gasten accepteert de gebruiksrechtovereenkomst voordat ze toegang krijgen.
- Vereisen dat werknemers of gasten accepteer de gebruiksvoorwaarden op elk apparaat voordat ze toegang krijgen.
- Werknemers of gasten accepteert de gebruiksrechtovereenkomst volgens een terugkerend schema vereist.
- Werknemers of gasten accepteert de gebruiksrechtovereenkomst voorafgaand aan de registratie van beveiligingsgegevens in Azure multi-factor Authentication (MFA) vereist.
- Vereisen dat werknemers accepteer de gebruiksvoorwaarden voorafgaand aan de registratie van beveiligingsgegevens in Azure AD selfservice voor wachtwoordherstel (SSPR).
- Algemene gebruiksrechtovereenkomst voor alle gebruikers die aanwezig zijn in uw organisatie.
- Specifieke gebruiksrechtovereenkomst op basis van gebruikerskenmerken (ex presenteren.) artsen versus verpleegkundigen of binnenlandse werknemers versus werknemers in het buitenland) met behulp van [dynamische groepen](../users-groups-roles/groups-dynamic-membership.md).
- Specifieke gebruiksvoorwaarden presenteren bij het openen van hoge zakelijke impact toepassingen, zoals Salesforce.
- Gebruiksvoorwaarden die aanwezig zijn in verschillende talen.
- De lijst die is of met uw gebruiksrechtovereenkomst nog niet is geaccepteerd.
- Helpen bij het voldoen aan de regels.
- Een logboek van de voorwaarden van de activiteit gebruiken voor naleving en audits weergegeven.
- Maken en beheren van de voorwaarden van het gebruik van gebruik [Microsoft Graph-API's](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/agreement) (momenteel in Preview-versie).

## <a name="prerequisites"></a>Vereisten

Als u wilt gebruiken en configureren van Azure AD gebruiksrechtovereenkomst gebruiken, moet u het volgende hebben:

- Abonnement op Azure AD Premium P1, P2, EMS E3 of EMS E5.
   - Als u niet over een van deze abonnementen beschikt, kunt u [Azure AD Premium downloaden](../fundamentals/active-directory-get-started-premium.md) of [Azure AD Premium uitproberen](https://azure.microsoft.com/trial/get-started-active-directory/).
- Een van de volgende beheerdersaccounts voor de directory die u wilt configureren:
   - Globale beheerder
   - Beveiligingsbeheerder
   - Voorwaardelijke toegang beheerder

## <a name="terms-of-use-document"></a>Document met gebruiksrechtovereenkomst

Azure AD gebruiksrechtovereenkomst maakt gebruik van de PDF-indeling weergeven van inhoud. Het PDF-bestand kan van alles bevatten, zoals bestaande contracten, zodat u tijdens het aanmelden van gebruikers eindgebruikersovereenkomsten kunt verzamelen. Ter ondersteuning van gebruikers op mobiele apparaten, is de aanbevolen tekengrootte in het PDF-bestand 24 punt.

## <a name="add-terms-of-use"></a>Gebruiksrechtovereenkomst toevoegen

Nadat u uw document met gebruiksrechtovereenkomst hebt voltooid, gebruikt u de volgende procedure toe te voegen.

1. Aanmelden bij Azure als een globale beheerder, beveiligingsbeheerder of beheerder van voorwaardelijke toegang.
1. Navigeer naar **Gebruiksrechtovereenkomst** op [https://aka.ms/catou](https://aka.ms/catou).

   ![Voorwaardelijke toegang - voorwaarden van de blade gebruiken](./media/terms-of-use/tou-blade.png)

1. Klik op **Nieuwe voorwaarden**.

   ![Nieuwe term van deelvenster gebruiken om op te geven van de voorwaarden van de instellingen gebruiken](./media/terms-of-use/new-tou.png)

1. In de **naam** vak, voer een naam voor de gebruiksvoorwaarden die wordt gebruikt in Azure portal.
1. In de **weergavenaam** vak, voer een titel die gebruikers wanneer ze zich zien aanmelden.
1. Voor **document met gebruiksrechtovereenkomst**, blader naar uw voltooide gebruiksrechtovereenkomst PDF-bestand en selecteert u deze.
1. Selecteer de taal voor uw document met gebruiksrechtovereenkomst. Via de taaloptie kunt u meerdere gebruiksvoorwaarden uploaden, elk met een andere taal. Welke versie van de gebruiksvoorwaarden aan een eindgebruiker wordt weergegeven, is gebaseerd op de browservoorkeuren.
1. Instellen om te vereisen dat eindgebruikers om weer te geven van de gebruiksvoorwaarden alvorens deze te accepteren, **vereisen dat gebruikers om uit te breiden de gebruiksvoorwaarden** naar **op**.
1. Instellen om te vereisen dat eindgebruikers accepteer de gebruiksvoorwaarden op elk apparaat dat ze vanuit openen, **vereisen dat gebruikers accepteren op elk apparaat** naar **op**. Zie voor meer informatie, [Per apparaat gebruiksvoorwaarden](#per-device-terms-of-use).
1. Als u wilt laten verlopen van voorwaarden instemming voor gebruiksrechtovereenkomst volgens een schema, stelt u **verlopen toestemmingen** naar **op**. Wanneer een ingesteld op aan, worden twee aanvullende schema-instellingen weergegeven.

   ![Verlopen toestemmingen instellingen om in te stellen, begindatum, frequentie en duur](./media/terms-of-use/expire-consents.png)

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

   ![Voorwaardelijke toegang vervolgkeuzelijst te selecteren van een beleidssjabloon](./media/terms-of-use/conditional-access-templates.png)

   | Template | Description |
   | --- | --- |
   | **Toegang tot cloud-apps voor alle gasten** | Beleid voor voorwaardelijke toegang wordt gemaakt voor alle gasten en alle cloud-apps. Dit beleid heeft gevolgen voor de Azure-portal. Nadat deze is gemaakt, kunt u mogelijk vereist voor het afmelden en aanmelden. |
   | **Toegang tot cloud-apps voor alle gebruikers** | Beleid voor voorwaardelijke toegang wordt gemaakt voor alle gebruikers en alle cloud-apps. Dit beleid heeft gevolgen voor de Azure-portal. Nadat deze is gemaakt, zich kunt u afmelden en opnieuw aanmelden vereist. |
   | **Aangepast beleid** | Selecteer de gebruikers, groepen en apps die deze gebruiksvoorwaarden worden toegepast op. |
   | **Het voorwaardelijke toegangsbeleid later maken** | Deze gebruiksrechtovereenkomst wordt weergegeven in de lijst voor beheer verlenen bij het maken van een beleid voor voorwaardelijke toegang. |

   >[!IMPORTANT]
   >Voorwaardelijke toegang beleid besturingselementen (met inbegrip van de gebruiksvoorwaarden) bieden geen ondersteuning voor handhaving op serviceaccounts. Het is raadzaam om alle serviceaccounts uitsluiten van het beleid voor voorwaardelijke toegang.

    Aangepaste beleidsregels voor voorwaardelijke toegang inschakelen gedetailleerde gebruiksrechtovereenkomst, naar een specifieke cloudtoepassing of groep gebruikers. Zie voor meer informatie [Snelstart: Gebruiksrechtovereenkomst moet zijn geaccepteerd voordat u toegang tot cloud-apps vereisen](require-tou.md).

1. Klik op **Create**.

   Als u een aangepaste sjabloon voor voorwaardelijke toegang hebt geselecteerd, klikt u vervolgens weergegeven een nieuw scherm waarin kunt u het aangepaste beleid voor voorwaardelijke toegang maken.

   ![Nieuw deelvenster voor voorwaardelijke toegang als u de sjabloon voor aangepaste voorwaardelijke toegang](./media/terms-of-use/custom-policy.png)

   U ziet nu uw nieuwe gebruiksrechtovereenkomst.

   ![Nieuwe gebruiksvoorwaarden die worden vermeld in de voorwaarden van de blade gebruiken](./media/terms-of-use/create-tou.png)

## <a name="view-report-of-who-has-accepted-and-declined"></a>Rapport weergeven van wie heeft geaccepteerd en geweigerd

In de Gebruiksrechtovereenkomst-blade ziet u de aantallen gebruikers die al dan niet hebben geaccepteerd. Deze telt het aantal en die geaccepteerd/afgewezen worden opgeslagen voor de levensduur van de gebruiksvoorwaarden.

1. Meld u aan bij Azure en navigeer naar **Gebruiksrechtovereenkomst** op [https://aka.ms/catou](https://aka.ms/catou).

   ![Gebruiksrechtovereenkomst gebruik blade met het nummer van de gebruiker weergeven hebben geaccepteerd en niet](./media/terms-of-use/view-tou.png)

1. Voor een gebruiksrechtovereenkomst, klikt u op de getallen onder **geaccepteerde** of **geweigerd** om de huidige status voor gebruikers weer te geven.

   ![De gebruiksrechtovereenkomst toestemmingen deelvenster Overzicht van de gebruikers die hebben geaccepteerd](./media/terms-of-use/accepted-tou.png)

1. Als u wilt de geschiedenis van een afzonderlijke gebruiker weergeven, klikt u op het weglatingsteken ( **...** ) en vervolgens **geschiedenis weergeven**.

   ![Geschiedenis-contextmenu voor een gebruiker weergeven](./media/terms-of-use/view-history-menu.png)

   In het deelvenster van de geschiedenis van weergave ziet u een overzicht van alle de dalingen en accountwachtwoorden accepteert.

   ![Geschiedenis-deelvenster lijsten de geschiedenis accepteert, dalingen en accountwachtwoorden voor een gebruiker weergeven](./media/terms-of-use/view-history-pane.png)

## <a name="view-azure-ad-audit-logs"></a>Auditlogboeken weergeven Azure AD

Als u wilt om extra activiteiten te bekijken, bevat Azure AD gebruiksrechtovereenkomst auditlogboeken. Elke toestemming van de gebruiker activeert een gebeurtenis in de auditlogboeken die zijn opgeslagen voor **30 dagen**. U kunt deze logboeken bekijken in de portal of downloaden als CSV-bestand.

Aan de slag met Azure AD controlelogboeken, gebruikt u de volgende procedure:

1. Meld u aan bij Azure en navigeer naar **Gebruiksrechtovereenkomst** op [https://aka.ms/catou](https://aka.ms/catou).
1. Selecteer een gebruiksrechtovereenkomst.
1. Klik op **Auditlogboeken weergeven**.

   ![Voorwaarden van de blade voor gebruik met de weergave audit logs-optie is gemarkeerd](./media/terms-of-use/audit-tou.png)

1. Op de Azure AD controleren logboeken scherm, kunt u de gegevens met behulp van de opgegeven lijsten om gegevens van specifieke auditlogboeken doel te filteren.

   U kunt ook op **Downloaden** klikken om de informatie te downloaden naar een CSV-bestand voor lokaal gebruik.

   ![Azure AD-auditlogboeken scherm aanbieding datum, doel-beleid, gestart door, en activiteit](./media/terms-of-use/audit-logs-tou.png)

   Als u een logboek klikt, wordt een deelvenster met details van de extra activiteiten wordt weergegeven.

   ![Details van de activiteit voor een logboek weer met activiteit, de status van de activiteit, gestart door, beleid doel](./media/terms-of-use/audit-log-activity-details.png)

## <a name="what-terms-of-use-looks-like-for-users"></a>Welke gebruiksvoorwaarden lijkt voor gebruikers

Zodra een gebruiksrechtovereenkomst is gemaakt en afgedwongen, zien gebruikers, die binnen het bereik, het volgende scherm tijdens het aanmelden.

![Voorbeeld van de gebruiksrechtovereenkomst die wordt weergegeven wanneer een gebruiker zich aanmeldt](./media/terms-of-use/user-tou.png)

Gebruikers kunnen de gebruiksvoorwaarden bekijken en indien nodig, knoppen gebruiken in-en uitzoomen.

![Weergave van gebruiksvoorwaarden met knoppen voor inzoomen](./media/terms-of-use/zoom-buttons.png)

Het volgende scherm ziet u hoe gebruiksvoorwaarden eruitziet op mobiele apparaten.

![Voorbeeld van de gebruiksrechtovereenkomst die wordt weergegeven wanneer een gebruiker zich aanmeldt op een mobiel apparaat](./media/terms-of-use/mobile-tou.png)

Gebruikers zijn alleen vereist voor de gebruiksvoorwaarden accepteren één keer en zien ze de gebruiksvoorwaarden opnieuw op de volgende aanmeldingen.

### <a name="how-users-can-review-their-terms-of-use"></a>Hoe gebruikers hun gebruiksvoorwaarden kunnen controleren

Gebruikers controleren en bekijken van de gebruiksvoorwaarden die ze hebben geaccepteerd met behulp van de volgende procedure.

1. Meld u aan bij [https://myapps.microsoft.com](https://myapps.microsoft.com).
1. In de rechterbovenhoek, klikt u op uw naam en selecteer **profiel**.

   ![MyApps-website met van de gebruiker-deelvenster geopend](./media/terms-of-use/tou14.png)

1. Klik in uw profielpagina op **Gebruiksvoorwaarden controleren**.

   ![Profielpagina voor een gebruiker met de voorwaarden van de beoordeling van de verbinding gebruiken](./media/terms-of-use/tou13a.png)

1. Vanaf hier kunt u de gebruiksrechtovereenkomst bekijken die u hebt geaccepteerd.

## <a name="edit-terms-of-use-details"></a>Gegevens van gebruiksvoorwaarden bewerken

U kunt sommige gegevens van gebruiksvoorwaarden bewerken, maar u kunt een bestaand document niet wijzigen. De volgende procedure wordt beschreven hoe u de details bewerken.

1. Meld u aan bij Azure en navigeer naar **Gebruiksrechtovereenkomst** op [https://aka.ms/catou](https://aka.ms/catou).
1. Selecteer de gebruiksvoorwaarden die u wilt bewerken.
1. Klik op **gebruiksvoorwaarden bewerken**.
1. In de voorwaarden bewerken van deelvenster gebruikt, wijzigt u de naam, weergavenaam of vereisen dat gebruikers om uit te breiden waarden.

   Als er andere instellingen die u wijzigen wilt, zoals PDF-document, vereisen dat gebruikers accepteren op elk apparaat, verlopen toestemmingen, duur voordat reacceptance of beleid voor voorwaardelijke toegang, moet u een nieuwe gebruiksvoorwaarden maken.

   ![Gebruiksvoorwaarden van gebruik deelvenster waarin de naam bewerken en vouw opties](./media/terms-of-use/edit-tou.png)

1. Klik op **opslaan** uw wijzigingen op te slaan.

   Nadat u uw wijzigingen opslaat, worden gebruikers niet hebben deze wijzigingen onderhevig.

## <a name="add-a-terms-of-use-language"></a>Een gebruiksrechtovereenkomst gebruik taal toevoegen

De volgende procedure wordt beschreven hoe u een gebruiksrechtovereenkomst gebruik taal toevoegen.

1. Meld u aan bij Azure en navigeer naar **Gebruiksrechtovereenkomst** op [https://aka.ms/catou](https://aka.ms/catou).
1. Selecteer de gebruiksvoorwaarden die u wilt bewerken.
1. Klik in het detailvenster op de **talen** tabblad.

   ![Gebruiksvoorwaarden geselecteerd en het tabblad talen worden weergegeven in het deelvenster met details](./media/terms-of-use/languages-tou.png)

1. Klik op **taal toevoegen**.
1. In de voorwaarden toevoegen van gebruik taal deelvenster, uw gelokaliseerde PDF-bestand uploaden en selecteer de taal.

   ![Gebruiksrechtovereenkomst gebruik taal deelvenster met opties voor het uploaden van gelokaliseerde PDF-bestanden toevoegen](./media/terms-of-use/language-add-tou.png)

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
- Azure AD B2B-gebruikers worden niet ondersteund.

Als het apparaat van de gebruiker niet is toegevoegd, ontvangt deze een bericht die ze nodig hebben om hun apparaat. Hun ervaring zijn afhankelijk van het platform en de software.

### <a name="join-a-windows-10-device"></a>Een Windows 10-apparaat toevoegen

Als een gebruiker wordt met behulp van Windows 10 en Microsoft Edge, ontvangt deze een bericht dat lijkt op de volgende [toevoegen van hun apparaat](../user-help/user-help-join-device-on-network.md#to-join-an-already-configured-windows-10-device).

![Windows 10 en Microsoft Edge - dat aangeeft dat het apparaat moet worden geregistreerd](./media/terms-of-use/per-device-win10-edge.png)

Als ze met chrome werkt, wordt ze gevraagd voor het installeren van de [Windows 10-Accounts extensie](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji).

### <a name="browsers"></a>Browsers

Als een gebruiker via de browser die niet wordt ondersteund, wordt ze gevraagd een andere browser te gebruiken.

![Dat aangeeft dat het apparaat moet worden geregistreerd, maar de browser wordt niet ondersteund](./media/terms-of-use/per-device-browser-unsupported.png)

## <a name="delete-terms-of-use"></a>Gebruiksvoorwaarden verwijderen

Oude gebruiksrechtovereenkomst met de volgende procedure kunt u verwijderen.

1. Meld u aan bij Azure en navigeer naar **Gebruiksrechtovereenkomst** op [https://aka.ms/catou](https://aka.ms/catou).
1. Selecteer de gebruiksrechtovereenkomst die u wilt verwijderen.
1. Klik op **Gebruiksvoorwaarden verwijderen**.
1. Klik in het bericht waarin u wordt gevraagd of u wilt doorgaan op **Ja**.

   ![Bericht waarin wordt gevraagd om bevestiging gebruiksvoorwaarden verwijderen](./media/terms-of-use/delete-tou.png)

   U ziet niet langer de gebruiksrechtovereenkomst.

## <a name="deleted-users-and-active-terms-of-use"></a>Verwijderde gebruikers en actieve gebruiksvoorwaarden

Standaard blijft een verwijderde gebruiker gedurende 30 dagen in Azure AD aanwezig met de status Verwijderd. In deze periode kan de gebruiker eventueel door een beheerder worden teruggezet. Na dertig dagen wordt die gebruiker definitief verwijderd. Daarnaast kan een hoofdbeheerder via de Azure Active Directory-portal expliciet [een recentelijk verwijderde gebruiker definitief verwijderen](../fundamentals/active-directory-users-restore.md) voordat de dertig dagen zijn verstreken. Een gebruiker permanent is verwijderd, verdere gegevens over die gebruiker wordt verwijderd uit de actieve gebruiksvoorwaarden. Controle-informatie over verwijderde gebruikers blijft in het auditlogboek aanwezig.

## <a name="policy-changes"></a>Beleidswijzigingen

Beleid voor voorwaardelijke toegang zijn onmiddellijk van kracht. Als dit gebeurt, gaan de beheerder 'verdrietige clouds' of 'Token problemen met Azure AD'. De beheerder moet zich afmelden en opnieuw aanmelden om te voldoen aan het nieuwe beleid.

> [!IMPORTANT]
> Gebruikers in het bereik moeten zich af- en aanmelden om te voldoen aan een nieuw beleid:
>
> - beleid voor voorwaardelijke toegang is ingeschakeld op een gebruiksrechtovereenkomst
> - of als er een tweede gebruiksrechtovereenkomst is gemaakt

## <a name="b2b-guests-preview"></a>B2B-gasten (Preview)

De meeste organisaties beschikken over een proces voor hun werknemers akkoord gaan met de voorwaarden van gebruik en privacyverklaringen van hun organisatie. Maar hoe kunt u afdwingen de dezelfde toestemmingen voor Azure AD business-to-business (B2B) gasten wanneer ze worden toegevoegd via SharePoint of Teams? Met behulp van voorwaardelijke toegang en gebruiksvoorwaarden, kunt u een beleid rechtstreeks voor B2B-gastgebruikers afdwingen. Tijdens de uitnodiging inwisselen stroom, wordt de gebruiker weergegeven met de gebruiksvoorwaarden. Deze ondersteuning is momenteel in preview.

Gebruiksvoorwaarden wordt alleen weergegeven wanneer de gebruiker een Gast-account in Azure AD heeft. SharePoint Online op dit moment heeft een [ad-hoc extern delen geadresseerde ervaring](/sharepoint/what-s-new-in-sharing-in-targeted-release) voor het delen van een document of een map waarvoor de gebruiker een gastaccount hebben. In dit geval wordt een gebruiksrechtovereenkomst niet weergegeven.

![Gebruikers en groepen deelvenster - tabblad met de optie voor alle Gast gebruikers dit selectievakje is ingeschakeld](./media/terms-of-use/b2b-guests.png)

## <a name="support-for-cloud-apps-preview"></a>Ondersteuning voor cloud-apps (Preview)

Gebruiksvoorwaarden kunnen worden gebruikt voor verschillende cloud-apps, zoals Azure Information Protection en Microsoft Intune. Deze ondersteuning is momenteel in preview.

### <a name="azure-information-protection"></a>Azure Information Protection

U kunt een beleid voor voorwaardelijke toegang voor de Azure Information Protection-app configureren en een gebruiksrechtovereenkomst vereisen wanneer een gebruiker toegang heeft tot een beveiligd document. Hiermee wordt een gebruiksrechtovereenkomst voordat een gebruiker toegang tot een beveiligd document voor de eerste keer geactiveerd.

![Deelvenster voor cloud-apps met Microsoft Azure Information Protection-app geselecteerd](./media/terms-of-use/cloud-app-info-protection.png)

### <a name="microsoft-intune-enrollment"></a>Microsoft Intune-inschrijving

U kunt een beleid voor voorwaardelijke toegang voor de app Microsoft Intune-inschrijving configureren en vereisen een gebruiksrechtovereenkomst voordat u de inschrijving van een apparaat bij Intune. Raadpleeg voor meer informatie het [oplossing voor uw organisatie blog-bericht het recht kiezen voorwaarden](https://go.microsoft.com/fwlink/?linkid=2010506&clcid=0x409).

![Deelvenster voor cloud-apps met Microsoft Intune-app geselecteerd](./media/terms-of-use/cloud-app-intune.png)

> [!NOTE]
> De app Intune-inschrijving wordt niet ondersteund voor [Per apparaat gebruiksvoorwaarden](#per-device-terms-of-use).

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

**V: Is het mogelijk om een gebruiksrechtovereenkomst die eerder is geaccepteerd unaccept?**<br />
A: U kunt [revisie eerder gebruiksrechtovereenkomst geaccepteerd](#how-users-can-review-their-terms-of-use), maar er is momenteel een manier te unaccept.

**V: Wat gebeurt er als ik ook Intune-voorwaarden en bepalingen?**<br />
A: Als u beide Azure AD gebruiksrechtovereenkomst hebt geconfigureerd en [Intune bepalingen en voorwaarden](/intune/terms-and-conditions-create), de gebruiker moet beide accepteren. Zie voor meer informatie de [oplossing voor uw organisatie blog-bericht het recht kiezen voorwaarden](https://go.microsoft.com/fwlink/?linkid=2010506&clcid=0x409).

**V: Welke eindpunten gebruikt de voorwaarden van de service gebruiken voor verificatie?**<br />
A: Gebruiksvoorwaarden maakt gebruik van de volgende eindpunten voor verificatie: https://tokenprovider.termsofuse.identitygovernance.azure.com en https://account.activedirectory.windowsazure.com. Als uw organisatie een acceptatielijst van URL's voor inschrijving heeft, moet u om toe te voegen deze eindpunten aan de acceptatielijst, samen met de Azure AD-eindpunten voor aanmelden.

## <a name="next-steps"></a>Volgende stappen

- [Snelstart: Gebruiksrechtovereenkomst moet zijn geaccepteerd voordat u toegang tot cloud-apps vereisen](require-tou.md)
- [Aanbevolen procedures voor voorwaardelijke toegang in Azure Active Directory](best-practices.md)
