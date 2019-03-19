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
ms.author: banders
ms.openlocfilehash: 4e940a12cd57ef136cfd9ead298f9afcd2d6ad1f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57849853"
---
# <a name="check-usage-of-free-services-included-with-your-azure-free-account"></a>Gebruik van gratis services inbegrepen bij uw gratis Azure-account controleren 

U bent niet in rekening gebracht voor services die zijn gratis inbegrepen met gratis Azure-account, tenzij u de grenzen van deze services overschrijdt. Om te blijven met de limieten, kunt u ofwel de Azure portal of uw gebruiksbestand om te controleren en bijhouden van het gebruik van gratis services gebruiken. 

## <a name="check-usage-on-the-azure-portal"></a>Gebruik in Azure portal controleren

1.  Meld u aan bij [Azure Portal](https://portal.azure.com).

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

* **Meternaam:** Identificeert de maateenheid voor de meter wordt gebruikt. Zie voor meer informatie over de toewijzing van de meter-service, [inzicht in de gratis service toewijzing van de meter](billing-understand-free-service-meter-mapping.md).
* **Gebruikslimiet:** Huidige maand gebruik en de limiet voor de meter. U kunt ook deze informatie vinden in de statusbalk.
* **Status:** Gebruik de status van de meter. Op basis van uw gebruikspatroon, kunt u hebben een van deze statuten.
  * **Niet in gebruik:** U hebt de meter nog niet hebt gebruikt of het gebruik van de meter nog niet het factureringssysteem bereikt.
  * **Overschreden op \<datum >:** U hebt de limiet voor de meter overschreden op \<datum >.
  * **Waarschijnlijk geen overschrijding:** U kunt waarschijnlijk geen overschrijding van de limiet voor de meter.
  * **Overschrijdt op \<datum >:** U bent waarschijnlijk zal overschrijden de limiet voor de meter op \<datum >.

## <a name="check-usage-through-the-usage-file"></a>Gebruik controleren via het gebruiksbestand

Uw gebruiksbestand bevat gedetailleerde informatie voor uw Azure-abonnement. U kunt downloaden van uw maandelijkse en dagelijks gebruiksbestand van Azure-Accountcentrum. Zie voor meer informatie over het van-gebruiksbestand downloaden en begrijpen van de toegang is vereist, [ophalen factuur- en gebruiksgegevens](billing-download-azure-invoice-daily-usage-date.md). Zie voor meer informatie over de kolommen in het gebruiksbestand, [meer informatie over uw gebruik](billing-understand-your-usage.md).

Het gebruiksbestand bevat informatie over het gebruik van gratis en betaalde services. Gratis service meters zou hebben **gratis** toegevoegd aan het einde van de meternaam van de. Zoeken naar gratis meters, open het bestand in excel en filter de **Metercategorie kolom** voor cellen die de tekst **- vrij** (Filters gebruiken om tekst &rarr; filter Contains) &nbsp;

![Schermafbeelding van het gebruik van gratis services](./media/billing-check-usage-of-free-services/free-services-usage-csv.png)

## <a name="need-help-contact-us"></a>Hulp nodig? Neem contact met ons op.

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://go.microsoft.com/fwlink/?linkid=2083458).