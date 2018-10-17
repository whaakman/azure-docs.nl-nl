---
title: 'Snelstart: Een auditrapport dowloaden met behulp van de Azure Portal | Microsoft Docs'
description: Meer informatie over het downloaden van een auditrapport met behulp van de Azure Portal
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4de121ea-f4aa-4c8a-aae4-700c2c5e97a2
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 09/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: d6ecf62b46c25ec7cf4ab80219d1bc220e4eb4cc
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46367403"
---
# <a name="quickstart-download-an-audit-report-using-the-azure-portal"></a>Snelstart: Een auditrapport downloaden met behulp van de Azure Portal

In deze snelstartgids leert u hoe u de auditlogboeken downloadt voor uw tenant voor de afgelopen 24 uur.

## <a name="prerequisites"></a>Vereisten

U hebt de volgende zaken nodig:

* Een Azure Active Directory-tenant. 
* Een gebruiker, die in de rol is van Beveiligingsadministrator, Beveiligingslezer of Globale administrator voor de tenant. Bovendien kan elke gebruiker in de tenant toegang krijgen tot hun eigen auditlogboeken.

## <a name="quickstart-download-an-audit-report"></a>Snelstart: Een auditrapport downloaden

1. Navigeer naar [Azure Portal](https://portal.azure.com).
2. Selecteer **Azure Active Directory** in het linker navigatiedeelvenster en gebruik de knop **Schakelen tussen mappen** om uw actieve map te selecteren.
3. Selecteer in het dashboard, **Azure Active Directory** en selecteer vervolgens **Auditlogboeken**. 
4. Kies **de afgelopen 24 uur** in de filter vervolgkeuzelijst **Datumbereik** en selecteer **Toepassen** om de auditlogboeken voor de afgelopen 24 uur weer te geven. 
5. Selecteer de knop **Downloaden** voor het downloaden van een CSV-bestand met de gefilterde records. 

![Rapportage](./media/quickstart-download-audit-report/download-audit-logs.png)

## <a name="next-steps"></a>Volgende stappen

* [Aanmeldactiviteitenrapporten in de Azure Active Directory-portal](concept-sign-ins.md)
* [Azure Active Directory-rapportretentie](reference-reports-data-retention.md)
* [Azure Active Directory reporting latencies](reference-reports-latencies.md) (Rapportagevertraging in Azure Active Directory)
