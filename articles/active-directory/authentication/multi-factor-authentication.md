---
title: Meer informatie over verificatie in twee stappen in de Azure MFA | Microsoft Docs
description: Wat is Azure multi-factor Authentication, MFA, en de verschillende methoden en versies die beschikbaar zijn waarom te gebruiken.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: d58d81d85dac7e5cd520b8d8a3fb5d91650e0cbe
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2018
ms.locfileid: "39161602"
---
# <a name="what-is-azure-multi-factor-authentication"></a>Wat is Azure Multi-Factor Authentication?

Verificatie in twee stappen is een verificatiemethode die meer dan één verificatiemethode is vereist en wordt een essentiële tweede beveiligingslaag toegevoegd aan gebruikersaanmeldingen en transacties. Dit gebeurt door de vereist twee of meer van de volgende verificatiemethoden:

* Iets dat u weet (doorgaans een wachtwoord)
* Iets dat u hebt (een vertrouwd apparaat dat is niet eenvoudig worden gedupliceerd, zoals een telefoon)
* Iets dat u bent (biometrie)

<center>![Gebruikersnaam en wachtwoord](./media/multi-factor-authentication/pword.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![certificaten](./media/multi-factor-authentication/phone.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Phone slimme](./media/multi-factor-authentication/hware.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Smart Card](./media/multi-factor-authentication/smart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Virtual Smart Card](./media/multi-factor-authentication/vsmart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![gebruikersnaam en wachtwoord](./media/multi-factor-authentication/cert.png)</center>

Azure Multi-Factor Authentication (MFA) is een Microsoft-oplossing voor verificatie in twee stappen. Azure MFA helpt bij het bewaken van de toegang tot uw gegevens en toepassingen en komt tegemoet aan de wensen van gebruikers die een eenvoudige aanmeldprocedure willen. Het biedt krachtige verificatie via een reeks verificatiemethoden waaronder verificatie per telefoon, sms of mobiele app.

## <a name="why-use-azure-multi-factor-authentication"></a>Waarom kiezen voor Azure Multi-Factor Authentication?
Vandaag, meer dan ooit, zijn er steeds meer mensen verbonden. Met Smartphones, tablets, laptops en pc's hebben mensen meerdere opties om toegang tot hun accounts en toepassingen vanaf elke locatie en blijf verbonden op elk gewenst moment.

Azure multi-factor Authentication is een gebruiksvriendelijke, schaalbare en betrouwbare oplossing waarmee u een tweede verificatiemethode om uw gebruikers te beschermen.

| ![Eenvoudig te gebruiken](./media/multi-factor-authentication/simple.png) | ![Schaalbaar](./media/multi-factor-authentication/scalable.png) | ![Altijd beveiligd](./media/multi-factor-authentication/protected.png) | ![Betrouwbaar](./media/multi-factor-authentication/reliable.png) |
|:---:|:---:|:---:|:---:|
| **Eenvoudig te gebruiken** |**Schaalbare** |**Altijd beveiligd** |**Betrouwbare** |

* **Eenvoudig te gebruiken** -Azure multi-factor Authentication is eenvoudig te instellen en gebruiken. De extra beveiliging die wordt geleverd met Azure multi-factor Authentication kan gebruikers hun eigen apparaten beheren. Beste van alle, in veel gevallen deze kan worden ingesteld met een paar eenvoudige klikken.
* **Schaalbare** -Azure multi-factor Authentication gebruikt de kracht van de cloud en kan worden geïntegreerd met uw on-premises AD en aangepaste apps. Deze beveiliging ook uitgebreid naar uw hoog volume, essentiële scenario's.
* **Altijd beveiligd** -Azure multi-factor Authentication biedt sterke verificatie met behulp van de hoogste industrienormen.
* **Betrouwbare** -Microsoft garandeert een beschikbaarheid van 99,9% van de Azure multi-factor Authentication. De service wordt beschouwd als niet beschikbaar wanneer deze niet kan ontvangen of verwerken van aanvragen voor verificatie voor de verificatie in twee stappen.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [de werking van Azure multi-factor Authentication](concept-mfa-howitworks.md)

- Meer informatie over de verschillende [versies en -verbruikmethoden voor Azure multi-factor Authentication](concept-mfa-licensing.md)
