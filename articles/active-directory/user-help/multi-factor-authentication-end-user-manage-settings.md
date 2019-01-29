---
title: Instellingen voor verificatie in twee stappen verificatie - Azure Active Directory beheren | Microsoft Docs
description: Beheren hoe u Azure multi-factor Authentication, inclusief uw contactgegevens wijzigen of configureren van uw apparaten gebruiken.
services: active-directory
keywords: Multi-factor authentication-client, verificatieprobleem, correlatie-ID
author: eross-msft
manager: daveba
ms.reviewer: richagi
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: lizross
ms.openlocfilehash: ab3de9b010c0356dea17520b497235e0bf7c818e
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55181313"
---
# <a name="manage-your-settings-for-two-step-verification"></a>Uw instellingen beheren voor verificatie in twee stappen
In dit artikel vindt u antwoorden op vragen over het bijwerken van instellingen voor verificatie in twee stappen verificatie of multi-factor authentication. Als u problemen met aanmelden bij uw account hebt, raadpleegt u [problemen hebt met verificatie in twee stappen](multi-factor-authentication-end-user-troubleshoot.md) voor oplossingen voor problemen.

## <a name="where-to-find-the-settings-page"></a>Waar vind ik de instellingenpagina
Afhankelijk van hoe uw bedrijf van Azure multi-factor Authentication instellen, zijn er een paar plaatsen waar u uw instellingen, zoals uw telefoonnummer wijzigen kunt.

Als het ondersteuningsteam van uw bedrijf verzonden een specifieke URL of naar stappen voor het beheren van verificatie in twee stappen, volgt u deze instructies. Anders moeten de volgende instructies werken voor iedereen anders. Als u als te werk volgt, maar dezelfde opties niet ziet, betekent dit dat uw werk of school hun eigen portal aangepast. Vraag uw beheerder voor de koppeling naar de Azure multi-factor Authentication-portal.

**Om te gaan naar de pagina extra beveiliging met verificatie**

- Ga naar https://aka.ms/MFASetup.

    ![Proofup](./media/multi-factor-authentication-end-user-manage-settings/proofup.png)

Als u op deze koppeling te klikken niet werkt, krijgt u ook aan de **aanvullende beveiligingsverificatie** pagina door de volgende stappen:

1. Aanmelden bij [https://myapps.microsoft.com](https://myapps.microsoft.com)  

2. Selecteer de accountnaam van uw in de rechterbovenhoek en selecteer vervolgens **profiel**.

3. Selecteer **aanvullende beveiligingsverificatie**.  

    ![Myapps](./media/multi-factor-authentication-end-user-manage-settings/myapps1.png)

4. De pagina aanvullende verificatie wordt geladen met de instellingen.

    ![Proofup](./media/multi-factor-authentication-end-user-manage-settings/proofup.png)

## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>Ik wil mijn telefoonnummer wijzigen of een secundaire nummer toevoegen
Het is belangrijk een telefoonnummer voor secundaire verificatie configureren.  Omdat het primaire telefoonnummer en uw mobiele app waarschijnlijk op de dezelfde telefoon zijn, is de tweede telefoonnummer de enige manier kunt u zich weer toegang te krijgen tot je account als uw telefoon is zoekgeraakt of gestolen.

> [!NOTE]
> Als u geen toegang tot uw primaire telefoonnummer hebben en hulp krijgen bij aan uw account in, Zie de [problemen hebt met verificatie in twee stappen](multi-factor-authentication-end-user-troubleshoot.md) artikel voor meer informatie.  

**Het primaire telefoonnummer wijzigen:**  

1. Op de **aanvullende beveiligingsverificatie** pagina, selecteert u in het tekstvak met uw huidige telefoonnummer en bewerken met het nieuwe telefoonnummer.  
2. Selecteer **Opslaan**.  
3. Als dit telefoonnummer het getal dat u voor uw voorkeursoptie voor verificatie-optie gebruikt is, moet u het nieuwe aantal controleren voordat u deze kunt opslaan.  

**Een tweede telefoonnummer toevoegen:**  

1. Op de pagina aanvullende beveiligingsinstellingen verificatie, schakel het selectievakje in naast **alternatieve telefoon voor authenticatie.**  
2. Voer uw secundaire telefoonnummer in het tekstvak in.  
3. Selecteer **opslaan** en uw wijzigingen zijn voltooid.  

## <a name="require-two-step-verification-again-on-a-device-youve-marked-as-trusted"></a>Vereisen van verificatie in twee stappen opnieuw op een apparaat dat u hebt gemarkeerd als vertrouwd

Afhankelijk van de instellingen van uw organisatie, moet u wellicht een selectievakje met de tekst ' niet opnieuw vragen gedurende **X** dagen ' wanneer u verificatie in twee stappen uitvoert in uw browser. Als u dit selectievakje en vervolgens uw apparaat kwijtraakt of denkt dat uw account is aangetast, moet u verificatie in twee stappen weer naar al uw apparaten.

1. Selecteer op de pagina van de verificatie extra beveiliging **verificatie met meerdere factoren herstellen op eerder vertrouwde apparaten**.
2. De volgende keer dat u zich aanmeldt op elk apparaat, wordt u gevraagd om uit te voeren van verificatie in twee stappen.

## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>Hoe kan ik Microsoft Authenticator opschonen vanuit het oude apparaat en verplaatsen naar een nieuwe?
Wanneer u de app van uw apparaat verwijderen of het apparaat opnieuw instelt, wordt de activering op de back-end niet verwijderd. Zie voor meer informatie, [Microsoft Authenticator](microsoft-authenticator-app-how-to.md).

## <a name="next-steps"></a>Volgende stappen
* Problemen oplossen met tips en help op [problemen hebt met verificatie in twee stappen](multi-factor-authentication-end-user-troubleshoot.md)
* Instellen van [app-wachtwoorden](multi-factor-authentication-end-user-app-passwords.md) voor alle apps die verificatie in twee stappen niet ondersteunen.
