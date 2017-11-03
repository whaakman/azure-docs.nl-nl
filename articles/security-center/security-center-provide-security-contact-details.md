---
title: Neem contact op met informatie over de beveiliging in Azure Security Center bieden | Microsoft Docs
description: Dit document wordt beschreven hoe u contact op met informatie over de beveiliging in Azure Security Center bieden.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 26b5dcb4-ce3f-4f22-8d56-d2bf743cfc90
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/15/2017
ms.author: terrylan
ms.openlocfilehash: 726b59c45e2eb18eebe28a180db23336ae141408
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="provide-security-contact-details-in-azure-security-center"></a>Neem contact op met informatie over de beveiliging in Azure Security Center bieden
Azure Security Center wordt het beste contact op met informatie over de beveiliging voor uw Azure-abonnement te leveren als u dat nog niet gedaan hebt. Deze informatie wordt gebruikt door Microsoft om contact met u op te nemen als door Microsoft Security Response Center (MSRC) wordt gedetecteerd dat uw klantgegevens zijn geopend door een illegale of niet-geautoriseerde derde. MSRC voert selecteert u beveiligingsbewaking van de Azure-netwerk en de infrastructuur en threat intelligence en misbruik klachten ontvangt van een derde partij.

Een e-mailmelding wordt verzonden op het eerste dagelijkse exemplaar van een waarschuwing en alleen voor waarschuwingen met hoog dreigingsniveau. E-mailvoorkeuren kunnen alleen worden geconfigureerd voor abonnementsbeleid. Deze instellingen worden overgenomen door resourcegroepen binnen een abonnement.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
1. Onder **aanbevelingen**, selecteer **contact op met informatie over de beveiliging bieden**.
   ![Neem contact op met beveiliging bieden][1]
2. Selecteer het Azure-abonnement voor contactgegevens op.
3. Hiermee opent u **beveiligingsbeleid - e-mailmeldingen**.

   ![Contactgegevens voor beveiliging verstrekken][2]

   * Voer de security contact op met e-mailadres of de adressen van elkaar gescheiden door komma's. Er is een limiet aan het aantal e-mailadressen die u kunt invoeren.
   * Geef één beveiliging contact op met internationaal telefoonnummer.
   * Schakel de optie voor het ontvangen van e-mailberichten over waarschuwingen met hoog dreigingsniveau **Stuur mij e-mails over waarschuwingen**.
   * U moet in de toekomst, de optie voor het e-mailmeldingen verzenden naar abonnementseigenaren. Deze optie is momenteel grijs weergegeven.
   * Selecteer **opslaan** de contactgegevens van de beveiliging toepassen op uw abonnement.

## <a name="see-also"></a>Zie ook
Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) --Lees het laatste nieuws van de Azure-beveiliging en de informatie.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
