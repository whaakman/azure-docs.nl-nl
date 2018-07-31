---
title: Een Azure Multi-Factor Authentication-pilot inschakelen
description: In deze zelfstudie schakelt u Azure AD Multi-Factor Authentication in voor een groep pilotgebruikers
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: 098973e2ece3477ec87b154c0304c4ca7e0246d1
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39163288"
---
# <a name="tutorial-complete-an-azure-multi-factor-authentication-pilot-roll-out"></a>Zelfstudie: Een Azure Multi-Factor Authentication-pilot implementeren

In deze zelfstudie vindt u stapsgewijze instructies voor het configureren van beleid voor voorwaardelijke toegang waardoor Multi-Factor Authentication (Azure MFA) wordt ingeschakeld als u zich bij Azure Portal aanmeldt. Het beleid is geïmplementeerd en getest op een specifieke groep pilotgebruikers. De implementatie van Azure MFA waarbij gebruikt wordt gemaakt van voorwaardelijke toegang, biedt aanzienlijke flexibiliteit voor organisaties en beheerders in vergelijking met de traditionele methode die werd afgedwongen.

> [!div class="checklist"]
> * Azure Multi-Factor Authentication inschakelen
> * Azure Multi-Factor Authentication testen

## <a name="prerequisites"></a>Vereisten

* Een werkende Azure AD-tenant waarop minimaal een proeflicentie is ingeschakeld.
* Een account met de bevoegdheden van een globale beheerder.
* Zie het artikel [Snelstart: Nieuwe gebruikers toevoegen aan Azure Active Directory](../add-users-azure-active-directory.md) als een testgebruiker zonder beheerdersbevoegdheden een bij u bekend wachtwoord heeft voor testdoeleinden, en u een gebruiker moet maken.
* Zie het artikel [Een groep maken en leden toevoegen in Azure Active Directory](../active-directory-groups-create-azure-portal.md) als u een pilotgroep hebt om mee te testen waarvan de gebruiker zonder beheerdersbevoegdheden lid is, en u een groep wilt maken.

## <a name="enable-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication inschakelen

1. Meld u aan bij [Azure Portal](https://portal.azure.com) met een globale-beheerdersaccount.
1. Blader naar **Azure Active Directory**, **Voorwaardelijke toegang**.
1. Selecteer **Nieuw beleid**.
1. Geef uw beleid de naam **MFA-pilot**
1. Onder **Gebruikers en groepen** selecteert u het keuzerondje **Gebruikers en groepen selecteren**.
    * Selecteer de pilotgroep die u hebt gemaakt als onderdeel van de sectie over vereisten in dit artikel.
    * Klik op **Gereed**.
1. Onder **Cloud-apps** selecteert u het keuzerondje **Apps selecteren**.
    * De cloud-app voor Azure Portal is **Microsoft Azure Management**.
    * Klik op **Selecteren**.
    * Klik op **Gereed**.
1. Sla de sectie **Voorwaarden** over.
1. Zorg ervoor dat bij **Verlenen** het keuzerondje **Toegang verlenen** is geselecteerd.
    * Schakel het selectievakje **Multi-Factor Authentication vereisen** in.
    * Klik op **Selecteren**.
1. Sla de sectie **Sessie** over.
1. Stel de wisselknop **Beleid inschakelen** in op **Aan**.
1. Klik op **Maken**.

## <a name="test-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication testen

Om te bewijzen dat uw beleid voor voorwaardelijke toegang werkt, gaat u zich als test aanmelden bij een resource waarvoor geen MFA is vereist, en vervolgens meldt u zich aan bij Azure Portal waarvoor wel MFA is vereist.

1. Open een nieuw browservenster in de InPrivate- of incognitomodus en blader naar [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com).
   * Meld u aan als de testgebruiker die u hebt gemaakt als onderdeel van de sectie over vereisten in dit artikel, en zoals u ziet wordt u niet gevraagd om een MFA uit te voeren.
   * Sluit het browservenster.
2. Open een nieuw browservenster in de InPrivate- of incognitomodus en blader naar [https://portal.azure.com](https://portal.azure.com).
   * Meld u aan als de testgebruiker die u hebt gemaakt als onderdeel van de sectie over vereisten in dit artikel, en zoals u ziet bent u nu verplicht om u te registreren voor Azure Multi-Factor Authentication en er gebruik van te maken.
   * Sluit het browservenster.

## <a name="clean-up-resources"></a>Resources opschonen

Als u besluit dat u niet langer gebruik wilt maken van de functionaliteit die u als onderdeel van deze zelfstudie hebt geconfigureerd, moet u de volgende wijziging aanbrengen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Blader naar **Azure Active Directory**, **Voorwaardelijke toegang**.
1. Selecteer het beleid voor voorwaardelijke toegang dat u hebt gemaakt.
1. Klik op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u Azure Multi-Factor Authentication ingeschakeld. Ga verder met de volgende zelfstudie om te zien hoe Azure Identity Protection kan worden geïntegreerd in de selfservice voor het opnieuw instellen van wachtwoorden en Multi-Factor Authentication-ervaringen.

> [!div class="nextstepaction"]
> [Risico's beoordelen bij het aanmelden](tutorial-risk-based-sspr-mfa.md)