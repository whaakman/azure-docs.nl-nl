---
title: 'Snelstart: Een aanmeldrapport downloaden met behulp van de Azure Portal | Microsoft Docs'
description: Meer informatie over het downloaden van een aanmeldrapport met behulp van de Azure Portal
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 9131f208-1f90-4cc1-9c29-085cacd69317
ms.service: active-directory
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 74602a64820d277de2222e29e5d9b59354d709f0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197378"
---
# <a name="quickstart-download-a-sign-in-report-using-the-azure-portal"></a>Quickstart: een aanmeldrapport downloaden met behulp van Azure Portal

In deze snelstartgids leert u hoe u de aanmeldgegevens download voor uw tenant voor de afgelopen 24 uur. U kunt maximaal 5000 records downloaden vanuit de Azure-portal. De records worden gesorteerd op meest recent zodat u standaard de meest recente 5000 records krijgt. 

## <a name="prerequisites"></a>Vereisten

U hebt de volgende zaken nodig:

* Een Azure Active Directory-tenant met een Premium-licentie om het rapport van aanmeldactiviteiten weer te geven. Zie [Aan de slag met Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) om uw versie van Azure Active Directory te upgraden. Als er vóór de upgrade nog geen activiteitgegevens waren, duurt het na het upgraden naar een premium-licentie enkele dagen voordat er gegevens worden weergegeven in de rapporten.
* Een gebruiker die de rol **Beveiligingsbeheerder**, **Beveiligingslezer**, **Rapportlezer** of **Globale beheerder** voor de tenant heeft. Bovendien kan elke gebruiker in de tenant toegang krijgen tot hun eigen aanmeldingen.

## <a name="quickstart-download-a-sign-in-report"></a>Snelstart: Een aanmeldrapport downloaden

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
