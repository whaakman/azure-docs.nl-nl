---
title: Monitor en traceren gebruik van gratis Azure-services | Microsoft Docs
description: Informatie over het gebruik van gratis services controleren. Gebruik van Azure portal en gebruik van csv.
services: ''
documentationcenter: ''
author: amberbhargava
manager: amberb
editor: ''
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/25/2017
ms.author: cwatson
ms.openlocfilehash: df049a87763f3aae8da2db153f876b88ed39b988
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47390235"
---
# <a name="check-usage-of-free-services-included-with-your-azure-free-account"></a>Gebruik van gratis services inbegrepen bij uw gratis Azure-account controleren 

U betaalt geen voor services die zijn gratis inbegrepen met gratis Azure-account, tenzij u de grenzen van deze services overschrijdt. Om te blijven met de limieten, kunt u ofwel de Azure portal of uw gebruiksbestand om te controleren en bijhouden van het gebruik van gratis services gebruiken. 

## <a name="check-usage-on-the-azure-portal"></a>Gebruik in Azure portal controleren

1.  Meld u aan bij [Azure Portal]( http://portal.azure.com).

2.  Selecteer in de navigatiebalk aan de linkerkant gebied **alle services**.

3.  Selecteer **Abonnementen**.

4.  Selecteer het abonnement dat u hebt gemaakt toen u zich hebt geregistreerd voor een gratis account.

    ![Schermafbeelding van alle abonnementen](./media/billing-check-usage-of-free-services/select-free-account-subscription.png)

5.  De overzichtssectie ziet u de essentiële informatie over uw abonnement, zoals de abonnements-ID, bieden een type en de naam van abonnement. U vindt hier ook informatie over wanneer uw tegoed gratis account zou verlopen.

    ![Schermafbeelding van essentiële informatie van abonnement](./media/billing-check-usage-of-free-services/subscription-essential-information.png)

6.  Schuif omlaag naar zoeken naar informatie over uw huidige en geraamde kosten. De kosten omvatten het gebruik van services die niet zijn opgenomen in uw gratis account en het gebruik van meer dan de grenzen van gratis services. 

    ![Schermafbeelding van informatie over de kosten van abonnementen](./media/billing-check-usage-of-free-services/subscription-cost-information.png)

7.  Het laatste gedeelte van de overzichtssectie bevat een tabel op het gebruik van gratis services. 

    ![Schermafbeelding van het gebruik van gratis services](./media/billing-check-usage-of-free-services/subscription-usage-free-services.png)

    De tabel bevat de volgende kolommen:

* **Meternaam:** identificeert de maateenheid voor de meter wordt gebruikt. Zie voor meer informatie over de toewijzing van de meter-service, [inzicht in de gratis service toewijzing van de meter](billing-understand-free-service-meter-mapping.md). 
* **Gebruikslimiet:** het gebruik en de limiet voor het meten van de huidige maand. U kunt ook deze informatie vinden in de statusbalk.
* **Status:** gebruik van de status van de meter. Op basis van uw gebruikspatroon, kunt u hebben een van deze statuten.
  * **Niet in gebruik:** u hebt niet de meter gebruikt of het gebruik van de meter heeft niet het factureringssysteem bereikt.
  * **Overschreden op \<datum >:** u hebt de limiet voor de meter overschreden op \<datum >.
  * **Waarschijnlijk Exceed:** u waarschijnlijk niet langer zijn dan de limiet voor de meter.
  * **Overschrijdt op \<datum >:** u waarschijnlijk langer zijn dan de limiet voor de meter op \<datum >.


## <a name="check-usage-through-the-usage-file"></a>Gebruik controleren via het gebruiksbestand

Uw gebruiksbestand bevat gedetailleerde informatie voor uw Azure-abonnement. U kunt downloaden van uw maandelijkse en dagelijks gebruiksbestand van Azure-Accountcentrum. Zie voor meer informatie over het van-gebruiksbestand downloaden en begrijpen van de toegang is vereist, [ophalen factuur- en gebruiksgegevens](billing-download-azure-invoice-daily-usage-date.md). Zie voor meer informatie over de kolommen in het gebruiksbestand, [meer informatie over uw gebruik](billing-understand-your-usage.md). 

Het van-gebruiksbestand bevat informatie over het gebruik van gratis en betaalde services. Gratis service meters zou hebben **gratis** toegevoegd aan het einde van de meternaam van de. Zoeken naar gratis meters, open het bestand in excel en filter de **Metercategorie kolom** voor cellen met tekst **- vrij** (Filters gebruiken om tekst &rarr; filter Contains) &nbsp;

![Schermafbeelding van het gebruik van gratis services](./media/billing-check-usage-of-free-services/free-services-usage-csv.png)


## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning

Als u hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.
