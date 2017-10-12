---
title: Beveiligingswaarschuwingen verwerken in Azure Security Center | Microsoft Docs
description: Dit document bevat informatie over het gebruik van de mogelijkheden van Azure Security Center om beveiligingsincidenten af te handelen.
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: e8feb669-8f30-49eb-ba38-046edf3f9656
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.openlocfilehash: a302f8cb2555eef469a24da2523fdd9b97cc5730
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="handling-security-incidents-in-azure-security-center"></a>Beveiligingsincidenten afhandelen in Azure Security Center
Sorteren en onderzoeken van beveiligingswaarschuwingen kan zelfs voor de meest ervaren beveiligingsanalisten tijdrovend zijn, en velen weten zelfs niet waar te beginnen. Met behulp van [analytics](security-center-detection-capabilities.md) om de informatie tussen afzonderlijke [beveiligingswaarschuwingen](security-center-managing-and-responding-alerts.md) te verbinden kan het Beveiligingscentrum u één overzicht geven van de aanval en alle gerelateerde waarschuwingen - zo begrijpt u snel wat de aanvaller deed en welke resources betrokken zijn.

Dit document bespreekt hoe u beveiligingswaarschuwingen in het Beveiligingscentrum gebruikt als hulpmiddel bij het afhandelen van beveiligingsincidenten.

## <a name="what-is-a-security-incident"></a>Wat is een beveiligingsincident?
In het Beveiligingscentrum is een beveiligingsincident een samenloop van alle waarschuwingen voor een resource die overeenstemt met [kill chain](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/)-patronen. Incidenten verschijnen in de tegel en blade voor [Beveiligingswaarschuwingen](security-center-managing-and-responding-alerts.md). Een incident zal de lijst van gerelateerde waarschuwingen tonen, zodat u meer info krijgt over elke gebeurtenis.

## <a name="managing-security-incidents"></a>Beveiligingsincidenten beheren
U kunt uw huidige beveiligingsincidenten controleren met de tegel Beveiligingswaarschuwingen. Open Azure Portal en voer de volgende stappen uit om meer informatie over elk incident weer te geven:

1. Op het Security Center-dashboard ziet u de tegel **Beveiligingswaarschuwingen**.

    ![De tegel Beveiligingswaarschuwingen in Security Center](./media/security-center-incident/security-center-incident-fig1.png)

2. Klik op deze tegel om hem uit te breiden. Als er een beveiligingsincident wordt gedetecteerd, verschijnt hij onder de grafiek beveiligingswaarschuwingen zoals hieronder afgebeeld:

    ![Beveiligingsincident](./media/security-center-incident/security-center-incident-fig2.png)

3. Merk op dat de beschrijving van een beveiligingsincident een ander pictogram heeft vergeleken met andere waarschuwingen. Klik erop om meer informatie over dit incident te bekijken.

    ![Beveiligingsincident](./media/security-center-incident/security-center-incident-fig3.png)

4. Op de blade **incident** ziet u meer details over dit beveiligingsincident, waaronder een volledige omschrijving, ernst (in dit geval hoog), huidige status (in dit geval *actief*, wat impliceert dat de gebruiker nog niets heeft gedaan - dit kan door met de rechtermuisknop te klikken op het incident op de blade **Beveiligingswaarschuwingen**), de aangevallen resource (in dit geval *VM1*) en de stappen om het incident te herstellen. In het onderste venster ziet u de waarschuwingen die in dit incident zijn opgenomen. Als u meer informatie wilt over elke waarschuwing, klikt u er gewoon op. Er wordt nu een nieuwe blade geopend, zoals hieronder aangegeven:

    ![Beveiligingsincident](./media/security-center-incident/security-center-incident-fig4.png)

De informatie op deze blade verschilt, afhankelijk van de waarschuwing. Lees [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md) voor meer info over het beheren van deze waarschuwingen. Enkele belangrijke overwegingen met betrekking tot deze mogelijkheid:

* Een nieuw filter kunt u de weergave alleen Incident, alleen waarschuwingen of zowel kunt aanpassen.
* Dezelfde waarschuwing kan als onderdeel van een incident (indien van toepassing) bestaan en worden weergegeven als een afzonderlijke waarschuwing.

## <a name="see-also"></a>Zie ook
In dit document hebt u kunnen lezen hoe u de mogelijkheid beveiligingsincidenten in Azure Security Center gebruikt. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Detectiemogelijkheden van Azure Security Center](security-center-detection-capabilities.md)
* [Plannings- en bedieningsgids voor Azure Security Center](security-center-planning-and-operations-guide.md)
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md)
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.
