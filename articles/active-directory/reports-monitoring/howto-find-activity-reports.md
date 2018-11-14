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
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: fab94088d1d54012a955b0663b078d03b13d6299
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624909"
---
# <a name="find-activity-reports-in-the-azure-portal"></a>Activiteitenrapporten vinden in Azure portal

In dit artikel leert u hoe u Azure Active Directory (Azure AD) gebruiker-activiteitenrapporten vinden in Azure portal.

## <a name="audit-logs-report"></a>Rapport voor audittrails

Het Audittrailrapport zijn gerapporteerd combineert meerdere rapporten met betrekking tot de activiteiten van de toepassing in één weergave voor het melden van op basis van een context. Voor toegang tot het Audittrailrapport zijn gerapporteerd:

1. Navigeer naar [Azure Portal](https://portal.azure.com).
2. Selecteer uw directory in de rechterbovenhoek en selecteer vervolgens de **Azure Active Directory** blade van het navigatiedeelvenster links.
3. Selecteer **auditlogboeken** uit de **activiteit** sectie van de Azure Active Directory-blade. 

    ![Controlelogboeken](./media/howto-find-activity-reports/482.png "Controlelogboeken")

Het Audittrailrapport zijn gerapporteerd consolideert de volgende rapporten:

* Auditrapport
* Activiteit voor wachtwoord opnieuw instellen
* Registratie-activiteit wachtwoord opnieuw instellen
* Activiteit voor selfservicegroepen
* Naamswijzigingen voor Office 365-groep
* Inrichten van de activiteit van een account
* Status van het wachtwoord rollover
* Fouten bij het inrichten van een account

### <a name="filtering-on-audit-logs"></a>Filteren op auditlogboeken

U kunt geavanceerde filters in het controlerapport gebruiken voor toegang tot een specifieke categorie audit-gegevens door op te geven in de **activiteitscategorie** filter. Bijvoorbeeld, als u wilt weergeven van alle activiteiten met betrekking tot de self-service voor wachtwoord opnieuw instellen, selecteert u de **selfservice wachtwoordbeheer** categorie. 

    ![Category options on the Filter Audit Logs page](./media/howto-find-activity-reports/06.png "Category options on the Filter Audit Logs page")

Activiteitscategorieën zijn onder andere:

- Hoofddirectory
- Self-service voor wachtwoordbeheer
- Self-service voor groepsbeheer
- Account inrichten


## <a name="sign-ins-report"></a>Rapport aanmeldingen 

De **aanmeldingen** weergave bevat alle gebruikersaanmeldingen, evenals de **toepassingsgebruik** rapport. Ook vindt u informatie over het gebruik van toepassing in de **beheren** sectie van de **bedrijfstoepassingen** overzicht.

    ![Enterprise applications](./media/howto-find-activity-reports/484.png "Enterprise applications")

Voor toegang tot het rapport aanmeldingen:

1. Navigeer naar [Azure Portal](https://portal.azure.com).
2. Selecteer uw directory in de rechterbovenhoek en selecteer vervolgens de **Azure Active Directory** blade van het navigatiedeelvenster links.
3. Selecteer **Signins** uit de **activiteit** sectie van de Azure Active Directory-blade. 

    ![Aanmeldingsweergave](./media/howto-find-activity-reports/483.png "aanmeldingen weergeven")


### <a name="filtering-on-application-name"></a>Filteren op toepassingsnaam

U kunt het rapport aanmeldingen gebruiken voor meer informatie over het toepassingsgebruik van, door te filteren op gebruikersnaam of toepassing.

![Filter aanmeldingsgebeurtenissen pagina](./media/howto-find-activity-reports/07.png "pagina aanmelden gebeurtenissen filteren")

## <a name="security-reports"></a>Beveiligingsrapporten

### <a name="anomalous-activity-reports"></a>Rapporten van afwijkende activiteiten

Afwijkende activiteitenrapporten bevatten informatie over beveiliging gerelateerde risicogebeurtenissen die Azure AD kunt detecteren en rapporteren van.

De volgende tabel afwijkende activiteit een lijst met de Azure AD beveiligingsrapporten en bijbehorende typen risicogebeurtenissen in Azure portal. Zie [Risicogebeurtenissen in Azure Active Directory](concept-risk-events.md) voor meer informatie.  


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


### <a name="detected-risk-events"></a>Gedetecteerde risico

U hebt toegang tot rapporten over gedetecteerde risico in de **Security** sectie van de **Azure Active Directory** -blade in de [Azure-portal](https://portal.azure.com). Gedetecteerde risico worden bijgehouden in de volgende rapporten:   

- [Gebruikers die risico lopen](concept-user-at-risk.md)
- [Riskante aanmeldingen](concept-risky-sign-ins.md)

    ![Beveiligingsrapporten](./media/howto-find-activity-reports/04.png "beveiligingsrapporten")

## <a name="next-steps"></a>Volgende stappen

* [Audit-Logboeken, overzicht](concept-audit-logs.md)
* [Overzicht van aanmeldingen](concept-sign-ins.md)
* [Overzicht van riskante gebeurtenissen](concept-risk-events.md)
