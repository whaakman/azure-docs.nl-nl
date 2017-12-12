---
title: Activiteitsrapporten voor Azure Active Directory-gebruiker niet vinden in Azure portal | Microsoft Docs
description: Meer informatie over waar de activiteitsrapporten van de Azure Active Directory-gebruiker zich bevindt in de Azure-portal.
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: d93521f8-dc21-4feb-aaff-4bb300f04812
ms.service: active-directory
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: identity
ms.date: 12/06/2017
ms.author: curtand
ms.reviewer: dhanyahk
ms.openlocfilehash: 23c186e268e9a43982ec6c34d350900793fad8de
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="find-activity-reports-in-the-azure-portal"></a>Activiteitsrapporten niet vinden in de Azure-portal

In dit artikel worden beschreven Activiteitsrapporten voor Azure Active Directory-gebruiker zoeken in de Azure portal.

## <a name="whats-new"></a>Nieuwe functies

Rapporten in de klassieke Azure portal zijn ingedeeld in categorieën:
* Beveiligingsrapporten
* Activiteitsrapporten
* Geïntegreerde app-rapporten

### <a name="activity-and-integrated-app-reports"></a>Activiteit en geïntegreerde app-rapporten

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

Azure AD-beveiligingsgroep voor afwijkende activiteit rapporten vanuit de klassieke Azure portal om u te bieden met één centrale weergave zijn geconsolideerd. Deze weergave toont alle risicogebeurtenissen die betrekking hebben op beveiliging dat Azure AD kunt detecteren en van rapporteren.

De volgende tabel afwijkende activiteit een lijst met de Azure AD beveiligingsrapporten en bijbehorende risico gebeurtenistypen in de Azure portal.

| Azure AD afwijkende activiteitenrapport |  Identity protection risico gebeurtenistype|
| :--- | :--- |
| Gebruikers van wie de referenties zijn gelekt | Gelekte aanmeldingsreferenties |
| Onregelmatige aanmeldingsactiviteiten | Onmogelijke reis naar ongewone locaties |
| Aanmeldingen vanaf mogelijk geïnfecteerde apparaten | Aanmeldingen vanaf geïnfecteerde apparaten|
| Aanmeldingen van onbekende bronnen | Aanmeldingen vanaf anonieme IP-adressen |
| Aanmeldingen van IP-adressen met verdachte activiteit | Aanmeldingen van IP-adressen met verdachte activiteit |
| - | Aanmeldingen vanaf onbekende locaties |

De volgende Azure AD afwijkende activiteitsbeveiliging rapporten zijn niet opgenomen als risicogebeurtenissen in de Azure portal:

* Aanmeldingen na meerdere mislukte pogingen
* Aanmeldingen vanuit meerdere locaties

Deze rapporten zijn nog steeds beschikbaar in de klassieke Azure portal, maar ze op een bepaald moment in de toekomst wordt afgeschaft.

Zie [Risicogebeurtenissen in Azure Active Directory](active-directory-identity-protection-risk-events.md) voor meer informatie.  


#### <a name="detected-risk-events"></a>Gedetecteerde risico 's

In de Azure-portal, opent u rapporten over gedetecteerde risico's op de **Azure Active Directory** blade onder **beveiliging**. Gedetecteerde risicogebeurtenissen die worden bijgehouden in de volgende rapporten:   

- Gebruikers risico
- Riskante aanmeldingen

![Beveiligingsrapporten](./media/active-directory-reporting-migration/04.png "beveiligingsrapporten")

Zie voor meer informatie over beveiligingsrapporten:

- [Gebruikers op beveiligingsrapport risico's in de Azure Active Directory-portal](active-directory-reporting-security-user-at-risk.md)
- [Riskant aanmeldingen rapport in de Azure Active Directory-portal](active-directory-reporting-security-risky-sign-ins.md)


## <a name="activity-reports-in-the-azure-classic-portal-vs-the-azure-portal"></a>Activiteitsrapporten in de klassieke Azure portal vergeleken met de Azure-portal

De tabel in deze sectie worden de bestaande rapporten in de klassieke Azure portal. Ook wordt beschreven hoe u de dezelfde informatie kunt krijgen in de Azure-portal.

Alle controle om gegevens te bekijken, op de **Azure Active Directory** blade onder **activiteit**, gaat u naar **controlelogboeken**.

![Controlelogboeken](./media/active-directory-reporting-migration/61.png "Controlelogboeken")

| Klassieke Azure-portal                 | Om te zoeken in de Azure portal                                                         |
| ---                                  | ---                                                                        |
| Controlelogboeken                           | Voor **activiteitscategorie**, selecteer **Core Directory**.                       |
| Activiteit voor wachtwoord opnieuw instellen              | Voor **activiteitscategorie**, selecteer **Self-service wachtwoordbeheer**. |
| Registratie-activiteit wachtwoord opnieuw instellen | Voor **activiteitscategorie**, selecteer **Self-service wachtwoordbeheer**.     |
| In activiteit         | Voor **activiteitscategorie**, selecteer **groepsbeheer met Self-service**.        |
| Inrichten van de activiteit van een account        | Voor **activiteitscategorie**, selecteer **Account gebruikersaanvragen**.         |
| Overschakeling van de status van het wachtwoord             | Voor **activiteitscategorie**, selecteer **automatische overschakeling van de App-wachtwoord**.      |
| Fouten bij het inrichten van een account          | Voor **activiteitscategorie**, selecteer **Account gebruikersaanvragen**.        |
| Wijzigingen in de groep Office365 naam         | Voor **activiteitscategorie**, selecteer **Self-service wachtwoordbeheer**. Voor **activiteit brontype**, selecteer **groep**. Voor **Activiteitbron**, selecteer **O365 groepen**.|

Om weer te geven de **toepassingsgebruik** rapporteren op de **Azure Active Directory** blade onder **beheren**, selecteer **bedrijfstoepassingen**, en selecteer vervolgens **aanmeldingen**.


![Enterprise toepassingen aanmeldingen rapport](./media/active-directory-reporting-migration/199.png "Enterprise toepassingen aanmeldingen rapport")

## <a name="next-steps"></a>Volgende stappen

Zie [Azure Active Directory-rapportage](active-directory-reporting-azure-portal.md) voor een overzicht van de rapportage.
