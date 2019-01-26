---
title: Over het vinden van een bepaalde API nodig is voor een aangepaste toepassing | Microsoft Docs
description: Het configureren van de machtigingen die u nodig hebt voor toegang tot een bepaalde API in uw aangepaste ontwikkeld Azure AD-toepassing
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.openlocfilehash: b8c0f6a72d3b48c8e93a21fe3ad68e5799b39a5a
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55078403"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Over het vinden van een bepaalde API nodig is voor een aangepaste toepassing

Toegang tot API's moeten worden geconfigureerd met toegangsbereiken en rollen. Als u weergeven van uw resource toepassing web-API's voor clienttoepassingen wilt, moet u toegangsbereiken en -rollen configureren voor de API. Als u wilt dat een clienttoepassing toegang krijgt tot een web-API, moet u machtigingen voor toegang tot de API in de app-registratie configureren.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Een resourcetoepassing configureren voor het beschikbaar maken van web-API's

Wanneer u uw web-API, de API worden weergegeven in de **Select an API** lijst bij het toevoegen van machtigingen aan een app-registratie. Volg de stappen in om toe te voegen toegangsbereiken, [toegangsbereiken toe te voegen aan uw resourcetoepassing](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).

## <a name="configuring-a-client-application-to-access-web-apis"></a>Configureren van een clienttoepassing voor toegang tot web-API 's

Als u machtigingen aan uw app-registratie toevoegen, kunt u **API-toegang toevoegen** naar blootgestelde web-API's. Voor toegang tot web-API's, volgt u de stappen die worden beschreven in [referenties of machtigingen voor toegang tot web-API's toevoegen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).

## <a name="next-steps"></a>Volgende stappen

-   [Toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [Inzicht krijgen in de Azure Active Directory-toepassingsmanifest](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


