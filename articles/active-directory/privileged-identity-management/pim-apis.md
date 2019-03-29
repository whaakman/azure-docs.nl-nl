---
title: Microsoft Graph-API's voor PIM (Preview) - Azure Active Directory | Microsoft Docs
description: Bevat informatie over het gebruik van de Microsoft Graph-API's voor Azure AD Privileged Identity Management (PIM) (Preview).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: pim
ms.topic: overview
ms.date: 11/13/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e54ec4049b2b0cd67c148d881a64a40efff438a2
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58578965"
---
# <a name="microsoft-graph-apis-for-pim-preview"></a>Microsoft Graph-API's voor PIM (preview)

Voor het merendeel van de taken die u in Azure Active Directory (Azure AD) Privileged Identity Management (PIM) met behulp van de Azure portal kunt uitvoeren, u kunt ook uitvoeren met behulp van de [Microsoft Graph-API's](https://developer.microsoft.com/graph/docs/concepts/overview). In dit artikel worden enkele belangrijke concepten beschreven bij het gebruik van de Microsoft Graph-API's voor PIM. Zie de [naslag bij Azure AD Privileged Identity Management-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root) voor meer informatie over de Microsoft Graph-API's.

> [!IMPORTANT]
> API's onder de /bèta-versie in Microsoft Graph zijn beschikbaar als preview en kunnen worden gewijzigd. Het gebruik van deze API's in productie-apps wordt niet ondersteund.

## <a name="required-permissions"></a>Vereiste machtigingen

Voor het aanroepen van de Microsoft Graph-API's voor PIM, hebt u **een of meer** van de volgende machtigingen nodig:

- `Directory.AccessAsUser.All`
- `Directory.Read.All`
- `Directory.ReadWrite.All`
- `PrivilegedAccess.ReadWrite.AzureAD`

### <a name="set-permissions"></a>Machtigingen instellen

Toepassingen die de Microsoft Graph-API's voor PIM willen aanroepen, moeten beschikken over de vereiste machtigingen. De eenvoudigste manier om de vereiste machtigingen op te geven, is door het [toestemmingsframework van Azure AD-](../develop/consent-framework.md) te gebruiken.

### <a name="set-permissions-in-graph-explorer"></a>Machtigingen instellen in Graph Explorer

Als u Graph Explorer gebruikt om uw aanroepen te testen, kunt u de machtigingen opgeven in het hulpprogramma.

1. Meld u als globale beheerder aan bij [Graph Explorer](https://developer.microsoft.com/graph/graph-explorer).

1. Klik op **Machtigingen wijzigen**.

    ![Graph Explorer - machtigingen wijzigen](./media/pim-apis/graph-explorer.png)

1. Vink de machtigingen aan die u wilt opnemen. `PrivilegedAccess.ReadWrite.AzureAD` is nog niet beschikbaar in Graph Explorer.

    ![Graph Explorer - machtigingen wijzigen](./media/pim-apis/graph-explorer-modify-permissions.png)

1. Klik op **Machtigingen wijzigen** om de wijzigingen in de machtigingen toe te passen.

## <a name="next-steps"></a>Volgende stappen

- [Naslag bij Azure AD Privileged Identity Management-API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/privilegedidentitymanagement_root)
