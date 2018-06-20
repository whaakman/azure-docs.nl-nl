---
title: Activiteitsrapporten voor Azure Active Directory-gebruiker niet vinden in Azure portal | Microsoft Docs
description: Meer informatie over waar de activiteitsrapporten van de Azure Active Directory-gebruiker zich bevindt in de Azure-portal.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: users-groups-roles
ms.date: 12/06/2017
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: f22219a0e2ff342e25a2efdeb319f389250ecfef
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36231318"
---
# <a name="find-activity-reports-in-the-azure-portal"></a>Activiteitsrapporten niet vinden in de Azure-portal

In dit artikel worden beschreven Activiteitsrapporten voor Azure Active Directory-gebruiker zoeken in de Azure portal.

## <a name="activity-and-integrated-app-reports"></a>Activiteit en geïntegreerde app-rapporten

Voor het context-rapporten in de Azure portal, worden bestaande rapporten samenvoegen in één weergave. Één onderliggende API biedt de gegevens naar de weergave.

Voor deze weergave over de **Azure Active Directory** blade onder **activiteit**, selecteer **controlelogboeken**.

![Controlelogboeken](./media/active-directory-reporting-migration/482.png "Controlelogboeken")

De volgende rapporten worden geconsolideerd in deze weergave:

* Controlerapport
* Activiteit voor wachtwoord opnieuw instellen
* Registratie-activiteit wachtwoord opnieuw instellen
* In activiteit
* Wijzigingen in de groep Office365 naam
* Inrichten van de activiteit van een account
* Overschakeling van de status van het wachtwoord
* Fouten bij het inrichten van een account


Het gebruiksrapport van toepassing is uitgebreid en is opgenomen in de **aanmeldingen** weergeven. Voor deze weergave over de **Azure Active Directory** blade onder **activiteit**, selecteer **aanmeldingen**.

![Aanmeldingen weergave](./media/active-directory-reporting-migration/483.png "aanmeldingen weergeven")

De **aanmeldingen** weergave bevat alle gebruikersaanmeldingen. U kunt deze informatie gebruiken om op te halen van informatie over het gebruik van de toepassing. Ook vindt u informatie over het gebruik van toepassing in de **bedrijfstoepassingen** overzicht, in de **beheren** sectie.

![Bedrijfstoepassingen](./media/active-directory-reporting-migration/484.png "bedrijfstoepassingen")

## <a name="access-a-specific-report"></a>Toegang tot een specifiek rapport

Hoewel de Azure-portal één weergave biedt, kunt ook u bekijken bepaalde rapporten.

### <a name="audit-logs"></a>Controlelogboeken

Als reactie op feedback van klanten in de Azure portal kunt u geavanceerde filter voor toegang tot de gewenste gegevens. Een filter kunt u een *activiteitscategorie*, die hier worden de verschillende typen activiteit geregistreerd in Azure AD. Als u wilt beperken resultaten als u zoekt, kunt u een categorie.

Bijvoorbeeld, als u alleen geïnteresseerd in activiteiten met betrekking tot selfservice wachtwoorden opnieuw ingesteld, kunt u de **Self-service wachtwoordbeheer** categorie. De categorieën die u ziet zijn gebaseerd op de bron die u in werkt.  

![Categorie-opties op de pagina Filter controlelogboeken](./media/active-directory-reporting-migration/06.png "categorie-opties op de pagina controlelogboeken Filter")

Categorieën voor activiteiten omvatten:

- Hoofddirectory
- Self-service voor wachtwoordbeheer
- Self-service voor groepsbeheer
- Account inrichten

### <a name="application-usage"></a>Gebruik van de toepassing

Voor meer informatie over het gebruik van de toepassing voor alle apps of voor een enkele app onder **activiteit**, selecteer **aanmeldingen**. U kunt de resultaten beperken, kunt u filteren op gebruikersnaam of toepassing.

![Filter aanmelden gebeurtenissen pagina](./media/active-directory-reporting-migration/07.png "pagina aanmelden gebeurtenissen filteren")

### <a name="security-reports"></a>Beveiligingsrapporten

#### <a name="azure-ad-anomalous-activity-reports"></a>Azure AD afwijkende activiteit rapporten

Azure AD-beveiligingsgroep voor afwijkende activiteit rapporten waarmee u één centrale weergave worden samengevoegd. Deze weergave toont alle risicogebeurtenissen die betrekking hebben op beveiliging dat Azure AD kunt detecteren en van rapporteren.

De volgende tabel afwijkende activiteit een lijst met de Azure AD beveiligingsrapporten en bijbehorende risico gebeurtenistypen in de Azure portal.

| Azure AD afwijkende activiteitenrapport |  Identity protection risico gebeurtenistype|
| :--- | :--- |
| Gebruikers van wie de referenties zijn gelekt | Gelekte referenties |
| Onregelmatige aanmeldingsactiviteiten | Onmogelijke reis naar ongewone locaties |
| Aanmeldingen vanaf mogelijk geïnfecteerde apparaten | Aanmeldingen vanaf geïnfecteerde apparaten|
| Aanmeldingen van onbekende bronnen | Aanmeldingen vanaf anonieme IP-adressen |
| Aanmeldingen van IP-adressen met verdachte activiteit | Aanmeldingen van IP-adressen met verdachte activiteit |
| - | Aanmeldingen vanaf onbekende locaties |

De volgende Azure AD afwijkende activiteitsbeveiliging rapporten zijn niet opgenomen als risicogebeurtenissen in de Azure portal:

* Aanmeldingen na meerdere mislukte pogingen
* Aanmeldingen vanuit meerdere locaties

Zie [Risicogebeurtenissen in Azure Active Directory](active-directory-identity-protection-risk-events.md) voor meer informatie.  


#### <a name="detected-risk-events"></a>Gedetecteerde risico 's

In de Azure-portal, opent u rapporten over gedetecteerde risico's op de **Azure Active Directory** blade onder **beveiliging**. Gedetecteerde risicogebeurtenissen die worden bijgehouden in de volgende rapporten:   

- Gebruikers risico
- Riskante aanmeldingen

![Beveiligingsrapporten](./media/active-directory-reporting-migration/04.png "beveiligingsrapporten")

Zie voor meer informatie over beveiligingsrapporten:

- [Gebruikers op beveiligingsrapport risico's in de Azure Active Directory-portal](active-directory-reporting-security-user-at-risk.md)
- [Riskant aanmeldingen rapport in de Azure Active Directory-portal](active-directory-reporting-security-risky-sign-ins.md)


Om weer te geven de **toepassingsgebruik** rapporteren op de **Azure Active Directory** blade onder **beheren**, selecteer **bedrijfstoepassingen**, en selecteer vervolgens **aanmeldingen**.


![Enterprise toepassingen aanmeldingen rapport](./media/active-directory-reporting-migration/199.png)

## <a name="next-steps"></a>Volgende stappen

Zie [Azure Active Directory-rapportage](active-directory-reporting-azure-portal.md) voor een overzicht van de rapportage.
