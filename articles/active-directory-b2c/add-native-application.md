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
ms.component: B2C
ms.openlocfilehash: ee3323ec23b37318dbd80c85d6dd7515ce1ce06b
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55757655"
---
# <a name="add-a-native-client-application-to-your-azure-active-directory-b2c-tenant"></a>Een systeemeigen clienttoepassing met uw Azure Active Directory B2C-tenant toevoegen

Systeemeigen client resources moeten worden geregistreerd in uw tenant voor uw toepassing met Azure Active Directory B2C communiceren kan.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant door te klikken op de **map- en abonnementsfilter** in het bovenste menu en de map waarin uw tenant te kiezen.
3. Kies **alle services** in de linkerbovenhoek van de Azure portal en vervolgens zoeken naar en selecteer **Azure AD B2C**.
1. Selecteer **toepassingen**, en selecteer vervolgens **toevoegen**.
2. Voer een naam voor de toepassing. Bijvoorbeeld, *nativeapp1*.
3. Voor **inclusief web-app / web-API**, selecteer **Nee**.
4. Voor **systeemeigen client opnemen**, selecteer **Ja**.
5. Voor **omleidings-URI**, voer een geldige omleidings-URI met een aangepast schema. Er zijn twee belangrijke overwegingen bij het kiezen van een omleidings-URI:

    - **De unieke** -het schema van de omleidings-URI moet uniek zijn voor elke toepassing. In het voorbeeld `com.onmicrosoft.contoso.appname://redirect/path`, `com.onmicrosoft.contoso.appname` het schema. Dit patroon moet worden gevolgd. Als twee toepassingen hetzelfde schema delen, krijgt de gebruiker een keuze voor het kiezen van een toepassing. Als de gebruiker een foute keuze maakt, het aanmelden is mislukt.
    - **Volledige** -de omleidings-URI moet een schema en een pad. Het pad moet ten minste één forward slash bevatten na het domein. Bijvoorbeeld, `//contoso/` werkt en `//contoso` is mislukt. Zorg ervoor dat de omleidings-URI bevat geen speciale tekens zoals onderstrepingstekens bevatten.

6. Klik op **Create**.
7. Noteer de toepassings-ID die u gebruikt wanneer u uw systeemeigen clienttoepassing configureren op de eigenschappenpagina.
