---
title: 'Controle en rapportage van een gebruiker met B2B-samenwerking: Azure Active Directory | Microsoft Docs'
description: Eigenschappen van de Gast-gebruikers kunnen worden geconfigureerd in Azure Active Directory B2B-samenwerking
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 506721a23a5a18eaea76efaca61df49f20dc9228
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65812472"
---
# <a name="auditing-and-reporting-a-b2b-collaboration-user"></a>Controle en rapportage van een gebruiker B2B-samenwerking
Met gastgebruikers hebt u de mogelijkheden die vergelijkbaar is met controle met gebruikers. 

## <a name="access-reviews"></a>Toegangsbeoordelingen
Met toegangsbeoordelingen kunt u periodiek controleren of gastgebruikers ook kunnen nog steeds toegang tot uw resources nodig. De **Toegangsbeoordelingen** functie is beschikbaar in **Azure Active Directory** onder **beheren** > **organisatie relaties**. (U kunt ook zoeken naar "toegangsbeoordelingen" van **alle services** in Azure portal.) Zie voor meer informatie over het gebruik van toegangsbeoordelingen, [gasttoegang beheren met Azure AD-toegang beoordeelt](../governance/manage-guest-access-with-access-reviews.md).

## <a name="audit-logs"></a>Auditlogboeken

De Azure AD-audit-logboeken bieden records van systeem- en -activiteiten, met inbegrip van activiteiten die zijn gestart door gastgebruikers. Voor toegang tot de logboeken voor controle, in **Azure Active Directory**onder **bewaking**, selecteer **auditlogboeken**. Hier volgt een voorbeeld van de uitnodiging en een inwisselen historie van genodigde Sam Oogle:

![Schermopname die laat zien en voorbeeld van een audit het foutenlogboek van uitvoer](./media/auditing-and-reporting/audit-log.png)

U kunt Duik in elk van deze gebeurtenissen te halen. Bijvoorbeeld, gaan we kijken naar de acceptatie van de details.

![Schermopname die laat zien en voorbeeld van uitvoer van activiteit-details](./media/auditing-and-reporting/activity-details.png)

U kunt ook deze logboeken exporteren uit Azure AD en het gebruik van de rapportagetool in te voeren van uw keuze om aangepaste rapporten.

### <a name="next-steps"></a>Volgende stappen

- [Eigenschappen van gebruikers van B2B-samenwerking](user-properties.md)

