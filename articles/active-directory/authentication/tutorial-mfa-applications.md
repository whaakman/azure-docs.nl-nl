---
title: Een Azure Multi-Factor Authentication-pilot inschakelen
description: In deze zelfstudie schakelt u Azure AD Multi-Factor Authentication in voor een groep pilotgebruikers
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: a229eef3d49964e71becf066d67f60b18da6fc18
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67113247"
---
# <a name="tutorial-complete-an-azure-multi-factor-authentication-pilot-roll-out"></a>Zelfstudie: Een Azure Multi-Factor Authentication-pilot implementeren

In deze zelfstudie helpt u u bij het configureren van beleid voor voorwaardelijke toegang inschakelen van Azure multi-factor Authentication (MFA Azure) wanneer u zich aanmeldt bij Azure portal. Het beleid is geïmplementeerd en getest op een specifieke groep pilotgebruikers. Implementatie van Azure MFA met behulp van voorwaardelijke toegang biedt aanzienlijke flexibiliteit voor organisaties en beheerders in vergelijking met de traditionele methode afgedwongen.

> [!div class="checklist"]
> * Azure Multi-Factor Authentication inschakelen
> * Azure Multi-Factor Authentication testen

## <a name="prerequisites"></a>Vereisten

* Een werkende Azure AD-tenant waarop minimaal een proeflicentie is ingeschakeld.
* Een account met de bevoegdheden van een globale beheerder.
* Zie het artikel [Quickstart: Nieuwe gebruikers toevoegen aan Azure Active Directory](../add-users-azure-active-directory.md) als een testgebruiker zonder beheerdersbevoegdheden een wachtwoord heeft dat u kent, en u een gebruiker moet maken.
* Zie het artikel [Een groep maken en leden toevoegen in Azure Active Directory](../active-directory-groups-create-azure-portal.md) als u een pilotgroep hebt om mee te testen waarvan de gebruiker zonder beheerdersbevoegdheden lid is, en u een groep wilt maken.

## <a name="enable-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication inschakelen

1. Meld u aan bij [Azure Portal](https://portal.azure.com) met een globale-beheerdersaccount.
1. Blader naar **Azure Active Directory**, **voorwaardelijke toegang**
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
1. Klik op **Maken**

## <a name="test-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication testen

Om te bewijzen dat uw beleid voor voorwaardelijke toegang werkt, test u aanmelden bij een resource die geen MFA vereist en klikt u vervolgens naar de Azure-portal die MFA vereist.

1. Open een nieuw browservenster in de InPrivate- of incognitomodus en blader naar [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com).
   * Meld u aan als de testgebruiker die u hebt gemaakt als onderdeel van de sectie over vereisten in dit artikel, en zoals u ziet wordt u niet gevraagd om een MFA uit te voeren.
   * Sluit het browservenster.
2. Open een nieuw browservenster in de InPrivate- of incognitomodus en blader naar [https://portal.azure.com](https://portal.azure.com).
   * Meld u aan als de testgebruiker die u hebt gemaakt als onderdeel van de sectie over vereisten in dit artikel, en zoals u ziet bent u nu verplicht om u te registreren voor Azure Multi-Factor Authentication en er gebruik van te maken.
   * Sluit  het browservenster.

## <a name="clean-up-resources"></a>Resources opschonen

Als u besluit dat u niet langer gebruik wilt maken van de functionaliteit die u als onderdeel van deze zelfstudie hebt geconfigureerd, moet u de volgende wijziging aanbrengen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Blader naar **Azure Active Directory**, **voorwaardelijke toegang**.
1. Selecteer het beleid voor voorwaardelijke toegang die u hebt gemaakt.
1. Klik op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u Azure Multi-Factor Authentication ingeschakeld. Ga verder met de volgende zelfstudie om te zien hoe Azure Identity Protection kan worden geïntegreerd in de selfservice voor het opnieuw instellen van wachtwoorden en Multi-Factor Authentication-ervaringen.

> [!div class="nextstepaction"]
> [Risico's beoordelen bij het aanmelden](tutorial-risk-based-sspr-mfa.md)
