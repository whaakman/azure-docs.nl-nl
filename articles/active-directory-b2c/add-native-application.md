---
title: Toevoegen van een systeemeigen clienttoepassing - Azure Active Directory B2C | Microsoft Docs
description: Informatie over het toevoegen van een systeemeigen clienttoepassing met uw Active Directory B2C-tenant.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.author: davidmu
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: conceptual
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 3f988e61c152be820e7e490e13908fb4ab54e6c5
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56673389"
---
# <a name="add-a-native-client-application-to-your-azure-active-directory-b2c-tenant"></a>Een systeemeigen clienttoepassing met uw Azure Active Directory B2C-tenant toevoegen

Systeemeigen client resources moeten worden geregistreerd in uw tenant voor uw toepassing met Azure Active Directory B2C communiceren kan.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat u de map gebruikt met uw Azure AD B2C-tenant door te klikken op het **Map- en abonnementsfilter** in het bovenste menu en de map te kiezen waarin uw tenant zich bevindt.
3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
1. Selecteer **Toepassingen** en vervolgens **Toevoegen**.
2. Voer een naam in voor de toepassing. Bijvoorbeeld *nativeapp1*.
3. Selecteer voor **Web-app / web-API opnemen** **Nee**.
4. Selecteer **Systeemeigen client opnemen** **Ja**.
5. Voer voor **Omleidings-URI** een geldige omleidings-URI met een aangepast schema in. Er zijn twee belangrijke overwegingen bij het kiezen van een omleidings-URI:

    - **Uniek**: het schema van de omleidings-URI moet voor elke toepassing uniek zijn. In het voorbeeld `com.onmicrosoft.contoso.appname://redirect/path` is `com.onmicrosoft.contoso.appname` het schema. Dit patroon moet worden gevolgd. Als twee toepassingen hetzelfde schema delen, kan de gebruiker een toepassing kiezen. Als de gebruiker een foute keuze maakt, mislukt de aanmelding.
    - **Volledig**: de omleidings-URI moet een schema en een pad hebben. Het pad moet ten minste één slash na het domein bevatten. Bijvoorbeeld: `//contoso/` werkt en `//contoso` is mislukt. Zorg ervoor dat de omleidings-URI geen speciale tekens bevat, zoals onderstrepingstekens.

6. Klik op **Create**.
7. Noteer de toepassings-ID die u gebruikt wanneer u uw systeemeigen clienttoepassing configureren op de eigenschappenpagina.
