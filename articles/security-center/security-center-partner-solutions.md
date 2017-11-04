---
title: Het beheren van verbonden partneroplossingen in Azure Security Center | Microsoft Docs
description: "In dit document wordt Azure Security Center geïntroduceerd. U kunt in Azure Security Center in één oogopslag zien wat de integriteitsstatus is van de partneroplossingen die zijn geïntegreerd met uw Azure-abonnement."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/16/2017
ms.author: terrylan
ms.openlocfilehash: 181e1e00716987732ee809df6171c2f71087f3e1
ms.sourcegitcommit: bd0d3ae20773fc87b19dd7f9542f3960211495f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2017
---
# <a name="managing-connected-partner-solutions-with-azure-security-center"></a>Verbonden partneroplossingen met Azure Security Center beheren
Dit artikel begeleidt u bij het beheren en controleren van verbonden beveiligingsoplossingen in Azure Security Center.

## <a name="monitoring-partner-solutions"></a>Partneroplossingen bewaken
De status van verbonden beveiligingsoplossingen bewaken en basic beheer uitvoeren:

1. Onder **Security Center - overzicht**, selecteer **beveiligingsoplossingen**.

  ![Selecteer beveiligingsoplossingen][1]

  De **oplossingen verbonden** sectie bevat beveiligingsoplossingen die zijn verbonden met Security Center en informatie over de status van elke oplossing.

  ![Partneroplossingen][2]

   De status van een partneroplossing kan zijn:

   * Goed (groen) - er is geen probleem met de status.
   * Niet in orde (rood): er is een probleem met de status waarvoor onmiddellijke aandacht is vereist.
   * Statusproblemen (oranje): de oplossing is gestopt melden van de status.
   * Niet gerapporteerd (grijs): de oplossing is niet gemeld iets nog een oplossing status is mogelijk omdat als er onlangs is verbonden en nog steeds implementeert of health gegevens niet beschikbaar.

   > [!NOTE]
   > Als u gegevens van status niet beschikbaar is, ziet u Security Center de datum en tijd van de laatste gebeurtenis ontvangen om aan te geven of de oplossing is rapportage of niet. Als er geen statusgegevens beschikbaar is en er geen waarschuwingen worden ontvangen binnen de afgelopen veertien dagen, Security Center geeft aan dat de oplossing beschadigd of niet reporting is.
   >
   >

2. Selecteer **weergave** voor aanvullende informatie en -opties, inclusief:

  - **Oplossingenconsole**. Hiermee opent u de beheerervaring voor deze oplossing.
  - **VM koppelen**. Hiermee opent u de blade toepassingen koppelen. Hier kunt u resources verbinden met de partneroplossing.
  - **Verwijderen van oplossing**.
  - **Configureer**.

   ![Details van partneroplossingen][3]

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe beheren en controleren van verbonden beveiligingsoplossingen in Security Center. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Oplossingen beveiligingsoverzicht](security-center-partner-integration.md) : informatie over het verbinding maken en beheren van beveiligingsoplossingen.
* [Verbinding maken met Microsoft Advanced Threat Analytics (ATA)](security-center-ata-integration.md) : informatie over het aansluiten van waarschuwingen van ATA.
* [Verbinding maken met Azure Active Directory (AD) Identity Protection ](security-center-aadip-integration.md) : informatie over het aansluiten van waarschuwingen van Azure AD Identity Protection.
* [Integratie met partners en -oplossingen](security-center-partner-integration.md) -krijgt u een overzicht van de integratie van andere beveiligingsoplossingen.
* [Beheren en erop reageren op beveiligingswaarschuwingen](security-center-managing-and-responding-alerts.md) : informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/partner-solutions.png
[3]: ./media/security-center-partner-solutions/partner-solutions-detail.png
