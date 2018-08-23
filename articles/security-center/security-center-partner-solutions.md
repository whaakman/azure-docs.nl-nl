---
title: Beheren van verbonden partneroplossingen in Azure Security Center | Microsoft Docs
description: In dit document wordt Azure Security Center geïntroduceerd. U kunt in Azure Security Center in één oogopslag zien wat de integriteitsstatus is van de partneroplossingen die zijn geïntegreerd met uw Azure-abonnement.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 70c076ef-3ad4-4000-a0c1-0ac0c9796ff1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/20/2018
ms.author: terrylan
ms.openlocfilehash: 3d4c9c79ff90ee816719f0179fbb5096b0035854
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "42058137"
---
# <a name="managing-connected-partner-solutions-with-azure-security-center"></a>Beheren van verbonden partneroplossingen met Azure Security Center
In dit artikel leert u hoe u beheren en controleren van verbonden beveiligingsoplossingen in Azure Security Center.

## <a name="monitoring-partner-solutions"></a>Partneroplossingen bewaken
De status van verbonden oplossingen bewaken en basisbeheertaken uitvoeren:

1. Onder **Security Center - overzicht**, selecteer **beveiligingsoplossingen**.

  ![Selecteer beveiligingsoplossingen][1]

  De **verbonden oplossingen** sectie bevat beveiligingsoplossingen die zijn verbonden met Security Center en informatie over de status van elke oplossing.

  ![Partneroplossingen][2]

   De status van een partneroplossing kan zijn:

   * In orde (groen) - er is geen probleem met de status.
   * Niet in orde (rood): er is een probleem met de status waarvoor onmiddellijke aandacht is vereist.
   * Statusproblemen (oranje): de oplossing is gestopt melden van de status.
   * Niet gerapporteerd (grijs): de oplossing heeft alles wat niet gerapporteerd nog, de status van een oplossing mogelijk ondersteuningsteams als deze onlangs is verbonden en is nog steeds de implementatie, of geen statusgegevens beschikbaar is.

   > [!NOTE]
   > Als de gegevens van status niet beschikbaar is, ziet u Security Center de datum en tijd van de laatste gebeurtenis ontvangen om aan te geven of de oplossing al dan niet rapporteert. Als er geen statusgegevens beschikbaar is en er zijn geen waarschuwingen worden ontvangen binnen de afgelopen 14 dagen, Security Center geeft aan dat de oplossing beschadigd of niet rapporteren is.
   >
   >

2. Selecteer **weergave** voor meer informatie en opties, inclusief:

  - **Oplossingenconsole**. Hiermee opent u de ervaring voor deze oplossing.
  - **VM koppelen**. Hiermee opent u de blade toepassingen koppelen. Hier kunt u resources verbinden met de partneroplossing.
  - **Oplossing verwijderen**.
  - **Configureer**.

   ![Details van partneroplossingen][3]

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u kunt beheren en controleren van verbonden beveiligingsoplossingen in Security Center. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Overzicht van oplossingen voor beveiliging](security-center-partner-integration.md) : Leer hoe u verbinding maken en beheren van beveiligingsoplossingen.
* [Microsoft Advanced Threat Analytics (ATA) verbinding](security-center-ata-integration.md) : meer informatie over het verbinding maken met waarschuwingen van ATA.
* [Azure Active Directory (AD) Identity Protection koppelen ](security-center-aadip-integration.md) : meer informatie over het verbinding maken met waarschuwingen van Azure AD Identity Protection.
* [Partner- en oplossingenintegratie](security-center-partner-integration.md) -een overzicht van de integratie van andere beveiligingsoplossingen.
* [Beheren en erop reageren op beveiligingswaarschuwingen](security-center-managing-and-responding-alerts.md) : informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.

<!--Image references-->
[1]: ./media/security-center-partner-solutions/partner-solutions-tile.png
[2]: ./media/security-center-partner-solutions/partner-solutions.png
[3]: ./media/security-center-partner-solutions/partner-solutions-detail.png
