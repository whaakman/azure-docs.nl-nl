---
title: Het zoeken van een specifieke API die nodig zijn voor een toepassing ontwikkelde aangepaste | Microsoft Docs
description: Het configureren van de machtigingen die u nodig hebt voor toegang tot een bepaalde API in uw aangepaste ontwikkeld Azure AD-toepassing
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: e0c07fd030339d025894520500d2cd948d31af45
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Het zoeken van een specifieke API die nodig zijn voor een toepassing ontwikkelde aangepaste

Toegang tot API's moeten worden geconfigureerd met toegangsbereiken en rollen. Als u uw resource toepassing web-API's voor clienttoepassingen weergeven wilt, moet u toegangsscopes en -rollen configureren voor de API. Als u een clienttoepassing toegang krijgt tot een web-API wilt, moet u machtigingen voor toegang tot de API in de registratie van de app te configureren.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Configureren van de toepassing van een resource te kunnen stellen van web-API 's

Bij het blootstellen van uw web-API, de API weergegeven in de **selecteert u een API** lijst bij het toevoegen van machtigingen aan een app te registreren. Volg de stappen in om toe te voegen toegangsbereiken, [toegangsbereiken toe te voegen aan uw toepassing resource](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).

## <a name="configuring-a-client-application-to-access-web-apis"></a>Een clienttoepassing toegang krijgt tot de web-API's configureren

Als u machtigingen aan uw app-registratie toevoegen, kunt u **API toegang toevoegen** naar blootgestelde web-API's. Voor toegang tot web-API's moet u de stappen [referenties of machtigingen voor toegang tot web-API's toevoegen](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).

## <a name="next-steps"></a>Volgende stappen

-   [Toepassingen integreren met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [Inzicht in de Azure Active Directory-toepassingsmanifest](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


