---
title: Bedreigingen voor resources en -gegevens in Azure Active Directory B2C beheren
description: Meer informatie over detectie en risicobeperking technieken voor denial-of-service-aanvallen en aanvallen wachtwoord in Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 7232917df6018c9c8afc7e7edd3730a277b193f4
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798227"
---
# <a name="manage-threats-to-resources-and-data-in-azure-active-directory-b2c"></a>Bedreigingen voor resources en -gegevens in Azure Active Directory B2C beheren

Azure Active Directory (Azure AD) B2C bevat ingebouwde functies die u kunnen helpen beschermen tegen bedreigingen voor uw resources en de gegevens. Deze bedreigingen zijn denial-of-service-aanvallen en wachtwoord aanvallen. Denial-of-service-aanvallen kunnen resources niet beschikbaar maken voor beoogde gebruikers. Wachtwoord aanvallen leiden tot niet-geautoriseerde toegang tot bronnen.

## <a name="denial-of-service-attacks"></a>Denial-of-service-aanvallen

Azure AD B2C beschermt tegen SYN overstroming aanvallen met behulp van een cookie SYN. Azure AD B2C wordt ook beveiligt tegen denial-of-service-aanvallen door limieten voor tarieven en verbindingen.

## <a name="password-attacks"></a>Wachtwoord aanvallen

Wachtwoorden die zijn ingesteld door gebruikers moeten redelijk complex zijn. Azure AD B2C heeft risicobeperking technieken voor het wachtwoord aanvallen. Risicobeperking omvat detectie van wachtwoord brute-force-aanvallen en woordenboekaanvallen wachtwoord. Azure AD B2C analyseert met behulp van verschillende signalen, de integriteit van aanvragen. Azure AD B2C is ontworpen om u te onderscheiden op intelligente wijze beoogde gebruikers tegen hackers en botnets.

Azure AD B2C maakt gebruik van een geavanceerde strategie om account te vergrendelen. De accounts zijn vergrendeld op basis van het IP-adres van de aanvraag en de ingevoerde wachtwoorden. De duur van de vergrendeling van het verhoogt ook op basis van de kans dat het een aanval. Nadat een wachtwoord is 10 keer zonder succes geprobeerd (de poging standaarddrempelwaarde), een vergrendeling van één minuut optreedt. De volgende keer dat een aanmelding mislukt nadat het account is is ontgrendeld (dat wil zeggen, nadat het account is automatisch door de service is ontgrendeld, zodra de vergrendeling is verlopen), een andere één minuut accountvergrendeling plaatsvindt en wordt voortgezet voor elke mislukte aanmelding. Herhaaldelijk hetzelfde wachtwoord invoert, is het niet meegeteld als meerdere mislukte aanmeldingen.

De eerste 10 accountvergrendeling perioden zijn een minuut lang. De volgende 10 accountvergrendeling perioden zijn iets langer en duur na elke 10 accountvergrendeling perioden worden verhoogd. De teller voor accountvergrendeling wordt opnieuw ingesteld op nul na een geslaagde aanmelding, wanneer het account niet is vergrendeld. Perioden van de vergrendeling van het kunnen tot vijf uur duren.

## <a name="manage-password-protection-settings"></a>Instellingen voor beveiliging beheren

Voor het beheren van instellingen voor beveiliging, met inbegrip van de drempelwaarde voor vergrendeling van:

1. Navigeer naar [Azure Portal](https://portal.azure.com).
1. Selecteer de **map + abonnement** filteren in het menu rechtsboven in de portal en selecteer vervolgens uw Azure AD B2C-tenant.
1. Selecteer **Azure Active Directory** in het menu links (of selecteer **alle services** in de linkerbovenhoek van de portal en zoek vervolgens naar en selecteer *Azure Active Directory*).
1. Onder **Security**, selecteer **verificatiemethoden**en selecteer vervolgens **wachtwoordbeveiliging**.
1. Voer uw gewenste wachtwoord protection-instellingen en selecteer vervolgens **opslaan**.

    ![Azure portal wachtwoord protection-pagina in Azure AD-instellingen](media/active-directory-b2c-reference-threat-management/portal-02-password-protection.png)
    <br />*De drempel voor accountvergrendelingen instelt op 5 in **wachtwoordbeveiliging** instellingen*.

## <a name="view-locked-out-accounts"></a>Weergave vergrendelde accounts

Als u informatie over vergrendelde accounts, kunt u de Active Directory controleren [rapport van aanmeldingsactiviteiten](../active-directory/reports-monitoring/reference-sign-ins-error-codes.md). Onder **Status**, selecteer **fout**. Mislukte pogingen aanmelden met een **aanmelden foutcode** van `50053` duiden op een vergrendeld account:

![Sectie van de Azure AD-in rapport weergeven met de vergrendelde account](media/active-directory-b2c-reference-threat-management/portal-01-locked-account.png)

Zie voor meer informatie over het weergeven van het rapport van aanmeldingsactiviteiten in Azure Active Directory, [aanmelden foutcodes voor aanmeldactiviteitenrapporten](../active-directory/reports-monitoring/reference-sign-ins-error-codes.md).
