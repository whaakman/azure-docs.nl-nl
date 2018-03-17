---
title: Geavanceerde Azure Stack-evaluatietaken | Microsoft Docs
description: Geavanceerde Azure Stack-evaluatietaken beschreven.
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: c4bf76aa07ec5025d9e53b5518929199ace27e18
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/17/2018
---
# <a name="advanced-azure-stack-development-kit-evaluation-tasks"></a>Geavanceerde Azure Stack Development Kit evaluatietaken
Nadat u hebt bekend bent met de basisfuncties voor Azure Stack Development Kit (ASDK)-service en de mogelijkheden heeft gekregen, kunt u uw kennis van de Azure-Stack verder verdiepen door het testen van meer geavanceerde scenario's. Deze evaluatietaken meer geavanceerde worden volledig gedocumenteerd in de documentatie van Azure Stack Operator.

> [!NOTE]
> Veel operator taken worden wel ondersteund voor zowel ASDK en productie, implementaties met meerdere knooppunten Azure Stack, worden niet alle gebruiksscenario's voor ASDK-implementaties ondersteund. Zie [ASDK en met meerdere knooppunten Azure Stack verschillen](asdk-what-is.md#asdk-and-multi-node-azure-stack-differences) voor meer informatie.

## <a name="delegate-offers-in-azure-stack"></a>Aanbiedingen in Azure Stack delegeren
Als de Azure-Stack-Operator wilt vaak u andere personen verantwoordelijk aanbiedingen maken en gebruikers aanmelden. Bijvoorbeeld, als u een serviceprovider bent, kunt u resellers registreren klanten en deze beheren namens jou. Of als u deel uitmaakt van een centrale IT-groep in een onderneming, kunt u dochterondernemingen aan te melden gebruikers zonder uw tussenkomst.

[Delegeren van aanbiedingen in Azure-Stack](.\.\azure-stack-delegated-provider.md) helpt u met deze taken door de mogelijkheid om te bereiken en meer gebruikers dan u rechtstreeks kunt beheren. 

## <a name="make-sql-databases-available-to-your-azure-stack-users"></a>SQL-databases beschikbaar maken voor uw Azure-Stack-gebruikers
Als een Azure-Stack-Operator, kunt u aanbiedingen die uw gebruikers laten maken (tenants) maken van SQL-databases die ze met hun cloud-systeemeigen apps, websites en werkbelastingen gebruiken kunnen. Dankzij deze aangepaste, op verzoek, cloud-gebaseerde databases aan uw gebruikers, kunt u ze bespaart tijd en bronnen. 

Gebruik de SQL Server-adapter resource provider voor [SQL-databases beschikbaar maken voor de gebruikers van uw Azure-Stack](.\.\azure-stack-tutorial-sql-server.md) als een service van Azure-Stack. Nadat u de resourceprovider kunt u deze verbinding met een of meer exemplaren van SQL Server.

## <a name="make-web-and-api-apps-available-to-your-azure-stack-users"></a>Web- en API-apps beschikbaar te maken voor uw Azure-Stack-gebruikers
Als een Azure-Stack-Operator, kunt u aanbiedingen die uw gebruikers laten maken (tenants) Azure Functions- en web- en API-toepassingen maken. Dankzij de toegang tot deze op aanvraag, cloud-gebaseerde apps aan uw gebruikers, kunt u ze bespaart tijd en bronnen.

De App Service-bronprovider om te implementeren [web- en API-apps beschikbaar te maken voor uw Azure-Stack-gebruikers](.\.\azure-stack-tutorial-app-service.md)

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het aanbieden van services met Azure-Stack geïntegreerd systemen](.\.\azure-stack-offer-services-overview.md)