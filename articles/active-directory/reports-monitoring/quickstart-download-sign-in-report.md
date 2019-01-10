---
title: 'Snelstart: Een aanmeldrapport downloaden met behulp van de Azure Portal | Microsoft Docs'
description: Meer informatie over het downloaden van een aanmeldrapport met behulp van de Azure Portal
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 9131f208-1f90-4cc1-9c29-085cacd69317
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 3e20af1c90f0e8a7a582d2d01dc4218a14496c40
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2018
ms.locfileid: "53653285"
---
# <a name="quickstart-download-a-sign-in-report-using-the-azure-portal"></a>Quickstart: een aanmeldrapport downloaden met behulp van Azure Portal

In deze snelstartgids leert u hoe u de aanmeldgegevens download voor uw tenant voor de afgelopen 24 uur.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende zaken nodig:

* Een Azure Active Directory-tenant met een Premium-licentie om het rapport van aanmeldactiviteiten weer te geven. Zie [Aan de slag met Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) om uw versie van Azure Active Directory te upgraden.
* Een gebruiker die de rol **Beveiligingsbeheerder**, **Beveiligingslezer**, **Rapportlezer** of **Globale beheerder** voor de tenant heeft. Bovendien kan elke gebruiker in de tenant toegang krijgen tot hun eigen aanmeldingen.

## <a name="quickstart-download-a-sign-in-report"></a>Quickstart: Een aanmeldrapport downloaden

1. Navigeer naar [Azure Portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** in het linker navigatiedeelvenster en gebruik de knop **Schakelen tussen mappen** om uw actieve map te selecteren.
3. Selecteer in het dashboard, **Azure Active Directory** en selecteer vervolgens **Aanmeldingen**. 
4. Kies **afgelopen 24 uur** in de **Datum** filter vervolgkeuzelijst en selecteer **Toepassen** om de aanmeldingen voor de afgelopen 24 uur weer te geven. 
5. Selecteer de knop **Downloaden** voor het downloaden van een CSV-bestand met de gefilterde records. 

![Rapportage](./media/quickstart-download-sign-in-report/download-sign-ins.png)

## <a name="next-steps"></a>Volgende stappen

* [Aanmeldactiviteitenrapporten in de Azure Active Directory-portal](concept-sign-ins.md)
* [Azure Active Directory-rapportretentie](reference-reports-data-retention.md)
* [Azure Active Directory reporting latencies](reference-reports-latencies.md) (Rapportagevertraging in Azure Active Directory)
