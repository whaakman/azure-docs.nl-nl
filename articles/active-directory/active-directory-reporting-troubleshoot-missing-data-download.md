---

title: 'Probleemoplossing: Ontbrekende gegevens in de gedownloade activiteitenlogboeken van Azure Active Directory | Microsoft Docs'
description: Biedt een oplossing voor ontbrekende gegevens in de gedownloade activiteitenlogboeken van Azure Active Directory.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ffce7eb1-99da-4ea7-9c4d-2322b755c8ce
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2017
ms.author: markvi
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 9109c698e4e8b43eeb7534c338adc99476012a3f
ms.contentlocale: nl-nl
ms.lasthandoff: 05/08/2017


---

# <a name="i-cant-find-any-data-in-the-azure-active-directory-activity-logs-i-have-downloaded"></a>Ik zie geen gegevens in de activiteitenlogboeken van Azure Active Directory die ik heb gedownload


## <a name="symptoms"></a>Symptomen

Ik heb de activiteitenlogboeken (audit of aanmeldingen) gedownload en ik zie niet alle records voor de tijd die ik heb geselecteerd. Hoe komt dat? 

 ![Rapportage](./media/active-directory-reporting-troubleshoot-missing-data-download/01.png)
 

## <a name="cause"></a>Oorzaak

Wanneer u activiteitenlogboeken downloadt in Azure Portal, worden er maximaal 120.000 records opgehaald, gesorteerd op meest recent. 

## <a name="resolution"></a>Oplossing

U kunt gebruikmaken van [API's van Azure AD Reporting](active-directory-reporting-api-getting-started.md) om maximaal een miljoen records op te halen. Onze aanbevolen aanpak is om volgens een vast schema een script uit te voeren waarmee de API's van Reporting worden aangeroepen om gedurende een bepaalde periode (bijvoorbeeld dagelijks of wekelijks) alleen de gewijzigde records op te halen.

## <a name="next-steps"></a>Volgende stappen
Zie [Azure Active Directory reporting FAQ](active-directory-reporting-faq.md) (Veelgestelde vragen over Azure Active Directory-rapportage).


