---
title: Aan de slag met Azure AD in Visual Studio WebApi projecten | Microsoft Docs
description: Hoe u aan de slag met Azure Active Directory in WebApi projecten nadat verbinding maken met of maken van een Azure AD met Visual Studio services verbonden
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: bf1eb32d-25cd-4abf-8679-2ead299fedaa
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/19/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: a756316054dd3bb63f3b0a9f59621bb1345bc693
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-active-directory-and-visual-studio-connected-services-webapi-projects"></a>Aan de slag met Azure Active Directory en Visual Studio verbonden services (WebApi-projecten)
> [!div class="op_single_selector"]
> * [Aan de slag](vs-active-directory-webapi-getting-started.md)
> * [Wat is er gebeurd](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="requiring-authentication-to-access-controllers"></a>Verificatie te vereisen voor toegang tot domeincontrollers
Alle domeincontrollers in uw project zijn adorned met de **autoriseren** kenmerk. Dit kenmerk moet de gebruiker moet worden geverifieerd voordat toegang tot de API's die door deze domeincontrollers gedefinieerd. Verwijder dit kenmerk van de controller zodat de controller voor anonieme toegang. Als u de machtigingen instelt op een meer gedetailleerd niveau wilt, toepassen van het kenmerk voor elke methode waarvoor de autorisatie in plaats van aan de controllerklasse toepast.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

