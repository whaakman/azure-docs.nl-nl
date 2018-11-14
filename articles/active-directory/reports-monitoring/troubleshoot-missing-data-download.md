---
title: 'Probleemoplossing: Ontbrekende gegevens in de gedownloade activiteitenlogboeken van Azure Active Directory | Microsoft Docs'
description: Biedt een oplossing voor ontbrekende gegevens in de gedownloade activiteitenlogboeken van Azure Active Directory.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 2607c5dacf6f261f27e7805e02df189a2753404c
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51625642"
---
# <a name="i-cant-find-all-the-data-in-the-azure-active-directory-activity-logs-i-downloaded"></a>Ik kan niet alle gegevens vinden in de activiteitenlogboeken van Azure Active Directory die ik gedownload

## <a name="symptoms"></a>Symptomen

Ik heb de activiteitenlogboeken (audit of aanmeldingen) gedownload en ik zie niet alle records voor de tijd die ik heb geselecteerd. Hoe komt dat? 

 ![Rapportage](./media/troubleshoot-missing-data-download/01.png)
 
## <a name="cause"></a>Oorzaak

Wanneer u activiteitenlogboeken in Azure portal downloadt, worden er maximaal 5000 records, gesorteerd op meest recente eerst. 

## <a name="resolution"></a>Oplossing

U kunt gebruikmaken van [API's van Azure AD Reporting](concept-reporting-api.md) om maximaal een miljoen records op te halen. Onze aanbevolen aanpak is het [een script uitvoeren op een gepland schema](tutorial-signin-logs-download-script.md) waarmee de rapportage-API's om records te halen in alleen de gewijzigde gedurende een bepaalde periode (bijvoorbeeld dagelijks of wekelijks) aangeroepen. 

## <a name="next-steps"></a>Volgende stappen

* [Veelgestelde vragen over Azure Active Directory-rapporten](reports-faq.md)

