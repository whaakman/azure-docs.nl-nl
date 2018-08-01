---
title: Probleem bij het maken van een toepassing toepassingsproxy | Microsoft Docs
description: Het oplossen van problemen met het maken van de toepassingsproxy-toepassingen in de Azure AD-beheerportal
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
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 3f0c05673ec970e5763f27fc0045b9a529b2ffee
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365471"
---
# <a name="problem-creating-an-application-proxy-application"></a>Probleem bij het maken van een toepassing Application Proxy 

Hieronder vindt u enkele van de algemene problemen gezichten van mensen bij het maken van een nieuwe application proxy-toepassing.

## <a name="recommended-documents"></a>Aanbevolen documenten 

Zie voor meer informatie over het maken van een Application Proxy-toepassing via de beheerportal, [toepassingen publiceren die gebruikmaken van Azure AD-toepassingsproxy](manage-apps/application-proxy-publish-azure-portal.md).

Als u de stappen in dit document volgen en zijn er een fout optreedt bij het maken van de toepassing, Zie de foutdetails voor meer informatie en suggesties voor het oplossen van de toepassing. De meeste foutberichten bevatten een voorgestelde oplossing. 

## <a name="specific-things-to-check"></a>Bepaalde dingen om te controleren

Algemene om fouten te voorkomen, controleert u of:

-   U bent een beheerder met de machtiging voor het maken van een toepassing Application Proxy

-   De interne URL is uniek

-   De externe URL is uniek

-   De URL's met http of https beginnen en eindigen met een '/'

-   De URL moet de naam van een domein en niet een IP-adres

Het foutbericht moet worden weergegeven in de rechterbovenhoek bij het maken van de toepassing. U kunt ook het meldingspictogram om te zien van de foutberichten selecteren.

   ![Melding prompt](./media/application-proxy-config-problem/error-message.png)

## <a name="next-steps"></a>Volgende stappen
[Toepassingsproxy inschakelen in Azure portal](manage-apps/application-proxy-enable.md)
