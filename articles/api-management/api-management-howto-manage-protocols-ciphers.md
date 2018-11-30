---
title: Protocollen en coderingen in Azure API Management beheren | Microsoft Docs
description: Informatie over het beheren van (TLS, SSL)-protocollen en coderingen (DES) in Azure API Management.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/15/2018
ms.author: apimpm
ms.openlocfilehash: a55d16a35b5eec1af2b24d02e158905493615999
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52441109"
---
# <a name="manage-protocols-and-ciphers-in-azure-api-management"></a>Protocollen en coderingen in Azure API Management beheren

Met Azure API Management biedt ondersteuning voor meerdere versies van TLS-protocol voor zowel client-en back-end, evenals de 3DES-codering.

Deze handleiding leest u over het beheren van protocollen en coderingen configuratie voor een Azure API Management-exemplaar.

![Protocollen en coderingen in APIM beheren](./media/api-management-howto-manage-protocols-ciphers/api-management-protocols-ciphers.png)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen de stappen in dit artikel, moet u het volgende hebben:

* Exemplaar van API Management

## <a name="how-to-manage-tls-protocols-and-3des-cipher"></a>Over het beheren van TLS-protocollen en 3DES-codering

1. Navigeer naar uw **exemplaar van API Management** in Azure portal.
2. Selecteer **SSL** in het menu.  
    ![Beheren van protocollen en coderingen in APIM - menu](./media/api-management-howto-manage-protocols-ciphers/api-management-menu.png)
3. In- of uitschakelen van de gewenste protocollen of versleuteling.
4. Klik op **Opslaan**. Wijzigingen worden toegepast binnen een uur.  
    ![Beheren van protocollen en coderingen in APIM - opslaan](./media/api-management-howto-manage-protocols-ciphers/api-management-protocols-ciphers-save.png)

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [TLS (Transport Layer Security)](https://docs.microsoft.com/dotnet/framework/network-programming/tls).
* Bekijk meer [video's](https://azure.microsoft.com/documentation/videos/index/?services=api-management) over API Management.