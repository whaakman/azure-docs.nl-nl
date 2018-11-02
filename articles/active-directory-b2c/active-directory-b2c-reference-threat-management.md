---
title: Beheren van bedreigingen voor resources en -gegevens in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over detectie en risicobeperking technieken voor denial-of-service-aanvallen en aanvallen wachtwoord in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ee99e7346d438e81a0cd25f8c522838524732568
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50912846"
---
# <a name="manage-threats-to-resources-and-data-in-azure-active-directory-b2c"></a>Bedreigingen voor resources en -gegevens in Azure Active Directory B2C beheren

Azure Active Directory (Azure AD) B2C bevat ingebouwde functies die u kunnen helpen beschermen tegen bedreigingen voor uw resources en de gegevens. Deze bedreigingen zijn denial-of-service-aanvallen en wachtwoord aanvallen. Denial-of-service-aanvallen kunnen resources niet beschikbaar maken voor beoogde gebruikers. Wachtwoord aanvallen leiden tot niet-geautoriseerde toegang tot bronnen. 

## <a name="denial-of-service-attacks"></a>Denial-of-service-aanvallen

Azure AD B2C beschermt tegen SYN overstroming aanvallen met behulp van een cookie SYN. Azure AD B2C wordt ook beveiligt tegen denial-of-service-aanvallen door limieten voor tarieven en verbindingen.

## <a name="password-attacks"></a>Wachtwoord aanvallen

Wachtwoorden die zijn ingesteld door gebruikers moeten redelijk complex zijn. Azure AD B2C heeft risicobeperking technieken voor het wachtwoord aanvallen. Risicobeperking omvat wachtwoord brute-force-aanvallen en woordenboekaanvallen wachtwoord. Azure AD B2C analyseert met behulp van verschillende signalen, de integriteit van aanvragen. Azure AD B2C is ontworpen om u te onderscheiden op intelligente wijze beoogde gebruikers tegen hackers en botnets. 

Azure AD B2C maakt gebruik van een geavanceerde strategie om account te vergrendelen. De accounts zijn vergrendeld op basis van het IP-adres van de aanvraag en de ingevoerde wachtwoorden. De duur van de vergrendeling van het verhoogt ook op basis van de kans dat het een aanval. Nadat een wachtwoord 10 keer zonder succes probeert is, wordt een vergrendeling van één minuut gemaakt. De volgende keer dat een aanmelding mislukt is nadat het account ontgrendeld is, een andere één minuut accountvergrendeling vindt plaats en voor elke mislukte aanmelding blijft. Herhaaldelijk hetzelfde wachtwoord invoert, is het niet meegeteld als meerdere mislukte aanmeldingen. 

De eerste 10 accountvergrendeling perioden zijn een minuut lang. De volgende 10 accountvergrendeling perioden zijn iets langer en duur na elke 10 accountvergrendeling perioden worden verhoogd. De teller voor accountvergrendeling wordt opnieuw ingesteld op nul na een geslaagde aanmelding, wanneer het account niet is vergrendeld. Perioden van de vergrendeling van het kunnen tot vijf uur duren. 

Op dit moment kunt u niet:

- Activeren van een vergrendeling met minder dan 10 mislukte aanmeldingen
- Een lijst met vergrendeld accounts ophalen
- De vergrendeling die het beleid configureren

Voor meer informatie gaat u naar de [Microsoft Trust Center](https://www.microsoft.com/trustcenter/default.aspx).
