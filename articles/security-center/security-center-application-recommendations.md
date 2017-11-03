---
title: Beveiligen van uw toepassingen in Azure Security Center | Microsoft Docs
description: Dit document gaat in Azure Security Center aanbevelingen die u helpen bij uw Azure-toepassingen beveiligen en blijven in overeenstemming met het beveiligingsbeleid.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: b5fc7a9e-24b1-415f-b3b5-62a53f5dd424
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2016
ms.author: terrylan
ms.openlocfilehash: b5bc02082fa8c2681aa67910729870921fec329d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="protecting-your-applications-in-azure-security-center"></a>Beveiligen van uw toepassingen in Azure Security Center
Azure Security Center analyseert de beveiligingsstatus van uw Azure-resources. Wanneer het Beveiligingscentrum identificeert mogelijke beveiligingsproblemen, maakt deze aanbevelingen die u bij het proces begeleiden van het configureren van benodigde besturingselementen.  Aanbevelingen hebben betrekking op Azure brontypen: virtuele machines (VM's), netwerken, SQL en toepassingen.

In dit artikel biedt aanbevelingen voor toepassingen.  Toepassing aanbevelingen center om de implementatie van web application firewall.  Gebruik de onderstaande tabel als referentie om te begrijpen van de beschikbare toepassingsaanvragen aanbevelingen en wat elke doet als u deze toe te passen.

## <a name="available-application-recommendations"></a>Beschikbare toepassingsaanvragen aanbevelingen
| Aanbeveling | Beschrijving |
| --- | --- |
| [Een firewall voor webtoepassingen toevoegen](security-center-add-web-application-firewall.md) |Raadt aan dat u een web application firewall (WAF) voor web-eindpunten implementeert. Een WAF aanbeveling wordt voor een openbare internetgerichte IP (exemplaar niveau IP- of Load Balanced IP) met een gekoppelde netwerkbeveiligingsgroep met poorten voor inkomend web openen (80,443) weergegeven.</br></br>Security Center raadt aan dat u een WAF om te helpen beschermen tegen aanvallen die gericht is op uw webtoepassingen op virtuele machines en op App Service-omgeving inrichten. Een as-omgeving (App Service omgeving) is een [Premium](https://azure.microsoft.com/pricing/details/app-service/) service-plan optie van Azure App Service die een volledig geïsoleerde en toegewezen omgeving biedt voor het veilig uitvoeren van Azure App Service-apps. Zie voor meer informatie over het as-omgeving, de [documentatie van App Service-omgeving](../app-service/environment/intro.md).</br></br>U kunt meerdere webtoepassingen in Security Center beveiligen door deze toepassingen toevoegen aan uw bestaande WAF-implementaties. |
| [Toepassingsbeveiliging voltooien](security-center-add-web-application-firewall.md#finalize-application-protection) |Voor het voltooien van de configuratie van een WAF moet verkeer worden gerouteerd naar het toestel WAF. Deze aanbeveling te volgen, voltooit de benodigde installatiebestanden wijzigingen. |

## <a name="see-also"></a>Zie ook
Zie de volgende onderwerpen voor meer informatie over de aanbevelingen die betrekking hebben op andere Azure-resource-typen:

* [Beveiligen van uw virtuele machines in Azure Security Center](security-center-virtual-machine-recommendations.md)
* [Beveiligen van uw netwerk in Azure Security Center](security-center-network-recommendations.md)
* [Beveiligen van uw Azure SQL-service in Azure Security Center](security-center-sql-service-recommendations.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
