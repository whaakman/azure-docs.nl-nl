---
title: Geavanceerde Azure Stack-evaluatietaken | Microsoft Docs
description: Dit artikel wordt beschreven geavanceerde taken voor Azure Stack-evaluatie.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 0b65f37310b911184c05fd8fb8554eeb824814b8
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364809"
---
# <a name="advanced-azure-stack-development-kit-evaluation-tasks"></a>Geavanceerde Azure Stack Development Kit-evaluatietaken
Nadat u vertrouwd bent met de algemene Azure Stack Development Kit (ASDK) servicefuncties en mogelijkheden hebben opgedaan, kunt u uw kennis van Azure Stack verder verdiepen door te testen van meer geavanceerde scenario's. Deze evaluatietaken geavanceerdere worden volledig gedocumenteerd in de documentatie van Azure Stack-operators.

> [!NOTE]
> Hoewel veel taken van de operator worden ondersteund voor zowel ASDK en productie, Azure Stack implementaties met meerdere knooppunten, worden niet alle gebruiksscenario's voor ASDK implementaties ondersteund. Zie [ASDK en meerdere knooppunten Azure Stack verschillen](asdk-what-is.md#asdk-and-multi-node-azure-stack-differences) voor meer informatie.

## <a name="delegate-offers-in-azure-stack"></a>Aanbiedingen in Azure Stack delegeren
Als de Azure Stack-operators wilt u meestal om andere personen die verantwoordelijk is voor het maken van aanbiedingen en aanmelden van gebruikers. Bijvoorbeeld, als u een serviceprovider bent, kunt u wederverkopers aan klanten aanmelden en deze beheren namens. Of als u deel uitmaakt van een centrale IT-groep in een onderneming bent, kunt u dochterondernemingen registreren van gebruikers zonder uw tussenkomst.

[Aanbiedingen in Azure Stack delegeren](.\.\azure-stack-delegated-provider.md) helpt u bij deze taken door de mogelijkheid om te bereiken en meer gebruikers dan u rechtstreeks kunt beheren. 

## <a name="make-sql-databases-available-to-your-azure-stack-users"></a>SQL-databases beschikbaar maken voor uw Azure Stack-gebruikers
Als een Azure Stack-Operator, kunt u voorstellen die zo uw gebruikers kunnen maken (tenants) SQL-databases die ze met hun cloud-eigen apps, websites en werkbelastingen gebruiken kunnen maken. Door op te geven deze databases aangepaste, op aanvraag, op basis van cloud naar uw gebruikers, kunt u ze Bespaar tijd en resources. 

Gebruik de SQL Server-resource provider-adapter kan [SQL-databases beschikbaar maken voor uw Azure Stack-gebruikers](.\.\azure-stack-tutorial-sql-server.md) als een service van Azure Stack. Nadat u de resourceprovider kunt u deze verbinding met een of meer exemplaren van SQL Server.

## <a name="make-web-and-api-apps-available-to-your-azure-stack-users"></a>Web-apps en API-apps beschikbaar te maken voor uw Azure Stack-gebruikers
Als een Azure Stack-Operator, kunt u voorstellen die zo uw gebruikers kunnen maken (tenants) maken van Azure Functions- en web- en API-toepassingen. Met toegang tot deze op aanvraag, op basis van cloud-apps aan uw gebruikers, kunt u ze Bespaar tijd en resources.

De resourceprovider App Service te implementeren [web-apps en API-apps beschikbaar te maken voor uw Azure Stack-gebruikers](.\.\azure-stack-tutorial-app-service.md)

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over het aanbod met geïntegreerde Azure Stack-systemen](.\.\azure-stack-offer-services-overview.md)