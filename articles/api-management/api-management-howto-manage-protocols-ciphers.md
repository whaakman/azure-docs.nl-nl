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
ms.openlocfilehash: 454ba5c42694581bfa8fb1ec69ce97ac906b53d4
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/13/2018
ms.locfileid: "35914778"
---
# <a name="manage-protocols-and-ciphers-in-azure-api-management"></a>Protocollen en coderingen in Azure API Management beheren

Met Azure API Management biedt ondersteuning voor meerdere versies van TLS-protocol voor zowel client-en back-end, evenals de 3DES-codering.

Deze handleiding leest u over het beheren van protocollen en coderingen configuratie voor een Azure API Management-exemplaar.

![Protocollen en coderingen in APIM beheren](./media/api-management-howto-manage-protocols-ciphers/api-management-protocols-ciphers.png)

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

* Meer informatie over [TLS (Transport Layer Security)](https://docs.microsoft.com/en-us/dotnet/framework/network-programming/tls).
* Bekijk meer [video's](https://azure.microsoft.com/documentation/videos/index/?services=api-management) over API Management.