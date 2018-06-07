---
title: Probleem bij het maken van een toepassing toepassingsproxy | Microsoft Docs
description: Het oplossen van problemen met het maken van toepassingen voor toepassingsproxy in de Azure AD-beheerportal
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: harshja
ms.openlocfilehash: 953d3399012b92442c0dcb4c0db717f2bf273f1b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34591793"
---
# <a name="problem-creating-an-application-proxy-application"></a>Probleem bij het maken van een toepassing toepassingsproxy 

Hieronder ziet u enkele veelvoorkomende problemen face mensen bij het maken van een nieuwe toepassing proxy-toepassing.

## <a name="recommended-documents"></a>Aanbevolen documenten 

Zie voor meer informatie over het maken van een toepassing toepassingsproxy via de beheerportal, [toepassingen publiceren met Azure AD-toepassingsproxy](manage-apps/application-proxy-publish-azure-portal.md).

Als u de stappen in dit document volgt en een fout bij het maken van de toepassing krijgt, Zie de foutdetails voor informatie en suggesties voor het oplossen van de toepassing. De meeste foutberichten bevatten een voorgestelde oplossing. 

## <a name="specific-things-to-check"></a>Specifieke wat u moet controleren

Controleer of algemene fouten te voorkomen:

-   U bent een beheerder met een machtiging voor het maken van een toepassing toepassingsproxy

-   De interne URL is uniek.

-   De externe URL is uniek.

-   De URL's met http of https beginnen en eindigen met een "/"

-   De URL moet een domeinnaam zijn en niet een IP-adres

Het foutbericht moet worden weergegeven in de rechterbovenhoek bij het maken van de toepassing. U kunt ook het meldingspictogram voor de foutberichten selecteren.

   ![Prompt voor melding](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Volgende stappen
[Toepassingsproxy inschakelen in de Azure-portal](manage-apps/application-proxy-enable.md)
