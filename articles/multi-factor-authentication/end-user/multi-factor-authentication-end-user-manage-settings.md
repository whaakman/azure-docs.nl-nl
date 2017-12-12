---
title: De verificatie in twee stappen instellingen beheren | Microsoft Docs
description: Beheren hoe u Azure multi-factor Authentication, inclusief het aanpassen van uw contactgegevens of configureren van uw apparaten gebruiken.
services: multi-factor-authentication
keywords: multifactor-verificatie-client, verificatieprobleem, correlatie-ID
documentationcenter: 
author: barlanmsft
manager: mtillman
ms.reviewer: richagi
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: barlan
ms.custom: end-user
ms.openlocfilehash: 8d84574283aa0c94ce303b0a7e3bde335c0eb2b8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="manage-your-settings-for-two-step-verification"></a>De instellingen voor verificatie in twee stappen beheren
In dit artikel antwoorden op vragen over het bijwerken van de instellingen voor de verificatie of meervoudige verificatie in twee stappen. Als u problemen die zijn aangemeld bij uw account hebt, raadpleegt u [problemen hebt met verificatie in twee stappen](multi-factor-authentication-end-user-troubleshoot.md) voor hulp bij probleemoplossing.

## <a name="where-to-find-the-settings-page"></a>Waar vind ik de instellingenpagina
Afhankelijk van hoe uw bedrijf van Azure multi-factor Authentication instellen, zijn er enkele locaties waar u uw instellingen zoals uw telefoonnummer wijzigen kunt.

Als uw bedrijf ondersteunen verzonden een specifieke URL of naar stappen voor het beheren van verificatie in twee stappen, volgt u deze instructies. Anders moeten de volgende instructies werken voor alle andere. Als u deze stappen maar dezelfde opties niet ziet, betekent dit dat uw werk of school hun eigen portal wordt aangepast. Vraag uw beheerder voor de koppeling naar de Azure multi-factor Authentication-portal.

1. Aanmelden bij [https://myapps.microsoft.com](https://myapps.microsoft.com)  
2. Selecteer de accountnaam van uw in de rechterbovenhoek en selecteer vervolgens **profiel**.  
3. Selecteer **aanvullende beveiligingsverificatie**.  

    ![Myapps](./media/multi-factor-authentication-end-user-manage/myapps1.png)
4. De pagina extra beveiliging verificatie wordt geladen met uw instellingen.

    ![Proofup](./media/multi-factor-authentication-end-user-manage/proofup.png)

## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>Ik wil mijn telefoonnummer wijzigen of toevoegen van een secundaire getal
Het is belangrijk voor het configureren van een telefoonnummer van de secundaire verificatie.  Omdat je primaire telefoonnummer en uw mobiele app waarschijnlijk op de dezelfde telefoon zijn, is de tweede telefoonnummer de enige manier waarop u zich weer toegang te krijgen tot je account als uw telefoon is kwijtgeraakt of gestolen.

> [!NOTE]
> Als u geen toegang tot uw primaire telefoonnummer hebben en aan uw account in hulp nodig hebt, raadpleegt u onze help-onderwerpen in [problemen hebt met verificatie in twee stappen](multi-factor-authentication-end-user-troubleshoot.md).  

**Het primaire telefoonnummer wijzigen:**  

1. Klik op de pagina aanvullende beveiligingsinstellingen verificatie selecteren in het tekstvak met uw huidige telefoonnummer en met uw nieuwe telefoonnummer te bewerken.  
2. Selecteer **Opslaan**.  
3. Als dit het aantal die u voor uw voorkeursoptie voor verificatie-optie gebruikt is, hebt u het nieuwe nummer controleren voordat u deze kunt opslaan.  

**Een tweede telefoonnummer toevoegen:**  

1. Op de pagina aanvullende beveiligingsinstellingen verificatie, schakel het selectievakje in naast **telefoon voor alternatieve authenticatie.**  
2. Voer uw secundaire telefoonnummer in het tekstvak.  
3. Selecteer **opslaan** en uw wijzigingen zijn voltooid.  

## <a name="require-two-step-verification-again-on-a-device-youve-marked-as-trusted"></a>Verificatie in twee stappen opnieuw op een apparaat dat u hebt gemarkeerd als vertrouwd vereisen

Afhankelijk van de instellingen van uw organisatie, moet u wellicht een selectievakje met de tekst ' niet opnieuw vragen voor **X** dagen ' wanneer u verificatie in twee stappen uitvoeren op uw browser. Als u dit selectievakje inschakelt en uw apparaat kwijtraakt of denkt dat uw account is geknoeid, moet u verificatie in twee stappen weer naar al uw apparaten.

1. Selecteer op de pagina controle van extra beveiliging **multi factor authentication herstellen op eerder vertrouwde apparaten**.
2. De volgende keer dat u zich op elk apparaat aanmelden, wordt u gevraagd een verificatie in twee stappen uitvoeren.

## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>Hoe ik Microsoft Authenticator opschonen van mijn oude apparaat en verplaatsen naar een nieuwe?
Wanneer u de app van uw apparaat verwijderen of het apparaat opnieuw instelt, wordt de activering op de back-end niet verwijderd. Zie voor meer informatie [Microsoft Authenticator](microsoft-authenticator-app-how-to.md).

## <a name="next-steps"></a>Volgende stappen
* Problemen oplossen met tips en help op [problemen hebt met verificatie in twee stappen](multi-factor-authentication-end-user-troubleshoot.md)
* Instellen van [app-wachtwoorden](multi-factor-authentication-end-user-app-passwords.md) voor alle apps die verificatie in twee stappen niet ondersteunen.
