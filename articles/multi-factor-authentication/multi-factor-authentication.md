---
title: Meer informatie over verificatie in twee stappen in de Azure MFA | Microsoft Docs
description: 'Wat is Azure multi-factor Authentication, MFA, meer informatie over de multi-factor Authentication-client en de verschillende methoden en versies die beschikbaar zijn waarom gebruiken. '
keywords: Inleiding tot MFA mfa overzicht, wat is mfa
services: multi-factor-authentication
documentationcenter: 
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.assetid: c40d7a34-1274-4496-96b0-784850c06e9b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/03/2017
ms.author: joflore
ms.openlocfilehash: 63b717fb08861b0efe902400a8765f9515ed5b1a
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="what-is-azure-multi-factor-authentication"></a>Wat is Azure Multi-Factor Authentication?
Verificatie in twee stappen is een authenticatiemethode die meer dan één verificatiemethode is vereist en een kritieke tweede beveiligingslaag wordt toegevoegd aan de gebruikersaanmeldingen en transacties. Het werkt door te vereisen van twee of meer van de volgende verificatiemethoden:

* Iets u weet (doorgaans een wachtwoord)
* Iets er (een vertrouwd apparaat die niet eenvoudig wordt gedupliceerd, zoals een telefoon)
* Iets dat u (biometrie)

<center>![Gebruikersnaam en wachtwoord](./media/multi-factor-authentication/pword.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![certificaten](./media/multi-factor-authentication/phone.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Smart Phone](./media/multi-factor-authentication/hware.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![smartcard](./media/multi-factor-authentication/smart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![virtuele smartcard](./media/multi-factor-authentication/vsmart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![gebruikersnaam en wachtwoord](./media/multi-factor-authentication/cert.png)</center>

Azure Multi-Factor Authentication (MFA) is een Microsoft-oplossing voor verificatie in twee stappen. Azure MFA helpt bij het bewaken van de toegang tot uw gegevens en toepassingen en komt tegemoet aan de wensen van gebruikers die een eenvoudige aanmeldprocedure willen. Het biedt krachtige verificatie via een reeks verificatiemethoden waaronder verificatie per telefoon, sms of mobiele app.

## <a name="why-use-azure-multi-factor-authentication"></a>Waarom Azure multi-factor Authentication gebruiken?
Vandaag de dag, meer dan ooit, zijn er steeds meer mensen verbonden. Met slim telefoons, tablets, laptops en pc's hebben mensen meerdere opties voor toegang tot hun accounts en toepassingen overal en op elk gewenst moment de verbinding behouden.

Azure multi-factor Authentication is een eenvoudig te gebruiken, schaalbare en betrouwbare oplossing die een tweede methode voor verificatie biedt bij het beveiligen van uw gebruikers.

| ![Eenvoudig te gebruiken](./media/multi-factor-authentication/simple.png) | ![Schaalbaar](./media/multi-factor-authentication/scalable.png) | ![Altijd is beveiligd](./media/multi-factor-authentication/protected.png) | ![Betrouwbaar](./media/multi-factor-authentication/reliable.png) |
|:---:|:---:|:---:|:---:|
| **Eenvoudig te gebruiken** |**Schaalbare** |**Altijd is beveiligd** |**Betrouwbare** |

* **Eenvoudig te gebruiken** -Azure multi-factor Authentication is eenvoudig te installeren en gebruiken. De extra beveiliging die wordt geleverd met Azure multi-factor Authentication kan gebruikers hun eigen apparaten beheren. Beste van alle in veel gevallen deze kan worden ingesteld met een paar eenvoudige klikken.
* **Schaalbare** -Azure multi-factor Authentication gebruikt de kracht van de cloud en kan worden geïntegreerd met uw on-premises AD en aangepaste apps. Deze beveiliging wordt zelfs uitgebreid naar uw hoog volume, essentiële scenario's.
* **Altijd is beveiligd** -Azure multi-factor Authentication biedt sterke verificatie met behulp van de hoogste industrienormen.
* **Betrouwbare** -Microsoft garandeert een beschikbaarheid van 99,9% van de Azure multi-factor Authentication. De service wordt beschouwd als niet beschikbaar wanneer het is niet ontvangen of verificatie aanvragen verwerkt voor de verificatie in twee stappen.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [de werking van Azure multi-factor Authentication](multi-factor-authentication-how-it-works.md)

- Meer informatie over de verschillende [versies en verbruikmethoden voor Azure multi-factor Authentication](multi-factor-authentication-versions-plans.md)
