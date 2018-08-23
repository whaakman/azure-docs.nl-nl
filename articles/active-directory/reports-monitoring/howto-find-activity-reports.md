---
title: Activiteitenrapporten van Azure Active Directory-gebruiker niet vinden in Azure portal | Microsoft Docs
description: Meer informatie over waar de activiteitenrapporten van de Azure Active Directory-gebruiker zich bevinden in de Azure-portal.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.component: report-monitor
ms.date: 12/06/2017
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 182537d6f07b624f2395f591681ed4596579bde0
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "42058353"
---
# <a name="find-activity-reports-in-the-azure-portal"></a>Activiteitenrapporten vinden in Azure portal

In dit artikel wordt beschreven hoe u Azure Active Directory-gebruiker activiteitenrapporten vinden in Azure portal.

## <a name="activity-and-integrated-app-reports"></a>Activiteit en de rapporten voor geïntegreerde app

Voor de context op basis van rapportage in Azure portal, worden bestaande rapporten worden samengevoegd in één weergave. Één onderliggende API kunt u de gegevens naar de weergave.

Om dit te openen, op de **Azure Active Directory** blade onder **activiteit**, selecteer **auditlogboeken**.

![Controlelogboeken](./media/howto-find-activity-reports/482.png "Controlelogboeken")

De volgende rapporten worden geconsolideerd in deze weergave:

* Controlerapport
* Activiteit voor wachtwoord opnieuw instellen
* Registratie-activiteit wachtwoord opnieuw instellen
* Activiteit voor selfservicegroepen
* Naamswijzigingen voor Office 365-groep
* Inrichten van de activiteit van een account
* Status van het wachtwoord rollover
* Fouten bij het inrichten van een account


Het gebruiksrapport van toepassing is verbeterd en is opgenomen in de **aanmeldingen** weergeven. Om dit te openen, op de **Azure Active Directory** blade onder **activiteit**, selecteer **aanmeldingen**.

![Aanmeldingsweergave](./media/howto-find-activity-reports/483.png "aanmeldingen weergeven")

De **aanmeldingen** weergave bevat alle gebruikersaanmeldingen. U kunt deze informatie gebruiken om informatie over het gebruik van de toepassing. Ook vindt u informatie over het gebruik van toepassing in de **bedrijfstoepassingen** overzicht in de **beheren** sectie.

![Bedrijfstoepassingen](./media/howto-find-activity-reports/484.png "bedrijfstoepassingen")

## <a name="access-a-specific-report"></a>Toegang tot een specifiek rapport

Hoewel de Azure-portal één weergave biedt, kunt u ook zoeken op gedetailleerde rapporten.

### <a name="audit-logs"></a>Controlelogboeken

In reactie op feedback van klanten in de Azure-portal, kunt u Geavanceerd filteren gebruiken voor toegang tot de gegevens die u wilt. Een filter kunt u een *activiteitscategorie*, waarin de verschillende soorten activiteit geregistreerd in Azure AD. Als u wilt beperken resultaten aan wat u zoekt, kunt u een categorie selecteren.

Bijvoorbeeld, als u alleen geïnteresseerd in activiteiten met betrekking tot de self-service voor wachtwoord opnieuw instellen van wachtwoorden, kunt u de **Self-service wachtwoordbeheer** categorie. De categorieën die u ziet zijn gebaseerd op de resource die u in de werkt.  

![Opties voor de categorie op de pagina Filter auditlogboeken](./media/howto-find-activity-reports/06.png "opties voor de categorie op de pagina auditlogboeken filteren")

Activiteitscategorieën zijn onder andere:

- Hoofddirectory
- Self-service voor wachtwoordbeheer
- Self-service voor groepsbeheer
- Account inrichten

### <a name="application-usage"></a>Toepassingsgebruik

Voor meer informatie over het gebruik van de toepassing voor alle apps of voor één toepassing, onder **activiteit**, selecteer **aanmeldingen**. U kunt de resultaten beperken, kunt u filteren op gebruikersnaam of toepassing.

![Filter aanmeldingsgebeurtenissen pagina](./media/howto-find-activity-reports/07.png "pagina aanmelden gebeurtenissen filteren")

### <a name="security-reports"></a>Beveiligingsrapporten

#### <a name="azure-ad-anomalous-activity-reports"></a>Afwijkende activiteit van Azure AD-rapporten

Azure AD afwijkende activiteit beveiligingsrapporten worden geconsolideerd zodat u één centrale weergave. Deze weergave toont alle beveiligingsgerelateerde risicogebeurtenissen die Azure AD kunt detecteren en van rapporteren.

De volgende tabel afwijkende activiteit een lijst met de Azure AD beveiligingsrapporten en bijbehorende typen risicogebeurtenissen in Azure portal.

| Azure AD afwijkende activiteit-rapport |  Type risicogebeurtenis Identity protection|
| :--- | :--- |
| Gebruikers van wie de referenties zijn gelekt | Gelekte referenties |
| Onregelmatige aanmeldingsactiviteiten | Onmogelijke reis naar ongewone locaties |
| Aanmeldingen vanaf mogelijk geïnfecteerde apparaten | Aanmeldingen vanaf geïnfecteerde apparaten|
| Aanmeldingen van onbekende bronnen | Aanmeldingen vanaf anonieme IP-adressen |
| Aanmeldingen van IP-adressen met verdachte activiteit | Aanmeldingen van IP-adressen met verdachte activiteit |
| - | Aanmeldingen vanaf onbekende locaties |

De volgende afwijkende activiteitsbeveiliging voor Azure AD-rapporten worden niet opgenomen als risicogebeurtenissen in Azure portal:

* Aanmeldingen na meerdere mislukte pogingen
* Aanmeldingen vanuit meerdere locaties

Zie [Risicogebeurtenissen in Azure Active Directory](concept-risk-events.md) voor meer informatie.  


#### <a name="detected-risk-events"></a>Gedetecteerde risico

In de Azure-portal, kunt u toegang tot rapporten over gedetecteerde risico op het **Azure Active Directory** blade onder **SECURITY**. Gedetecteerde risico worden bijgehouden in de volgende rapporten:   

- Gebruikers die risico lopen
- Riskante aanmeldingen

![Beveiligingsrapporten](./media/howto-find-activity-reports/04.png "beveiligingsrapporten")

Zie voor meer informatie over beveiligingsrapporten:

- [Gebruikers op risico's security rapport in de Azure Active Directory-portal](concept-user-at-risk.md)
- [Rapport voor riskante aanmeldingen in de Azure Active Directory-portal](concept-risky-sign-ins.md)


Om weer te geven de **toepassingsgebruik** rapporteren, op de **Azure Active Directory** blade onder **beheren**, selecteer **bedrijfstoepassingen**, en selecteer vervolgens **aanmeldingen**.


![Rapport Enterprise Applications aanmeldingen](./media/howto-find-activity-reports/199.png)

## <a name="next-steps"></a>Volgende stappen

Zie [Azure Active Directory-rapportage](overview-reports.md) voor een overzicht van de rapportage.
