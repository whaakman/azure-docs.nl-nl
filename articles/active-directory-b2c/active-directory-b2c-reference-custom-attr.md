---
title: Aangepaste kenmerken van Azure Active Directory B2C | Microsoft Docs
description: Het gebruik van aangepaste kenmerken in Azure Active Directory B2C voor het verzamelen van informatie over uw consumenten.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2016
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 893dfbae96d2cfea01b1f281f888e9281bf582f9
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441913"
---
# <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>Azure Active Directory B2C: Aangepaste kenmerken gebruiken voor het verzamelen van informatie over uw consumenten
Uw Azure Active Directory (Azure AD) B2C-directory wordt geleverd met een ingebouwde set met informatie over (kenmerken): voornaam, achternaam, plaats, postcode en andere kenmerken. Elke consumentgerichte toepassing heeft echter unieke vereisten op welke kenmerken voor het verzamelen van consumenten. Met Azure AD B2C, kunt u de set kenmerken die zijn opgeslagen op elke consumentenaccount uitbreiden. U kunt aangepaste kenmerken maken op de [Azure-portal](https://portal.azure.com/) en deze gebruiken in uw registratiebeleid, zoals hieronder wordt weergegeven. U kunt ook lezen en schrijven van deze kenmerken met behulp van de [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

> [!NOTE]
> Aangepaste kenmerken gebruiken [Azure AD Graph API Directory-Schemauitbreidingen](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-directory-schema-extensions).
> 
> 

## <a name="create-a-custom-attribute"></a>Een aangepast kenmerk maken
1. [Volg deze stappen om te navigeren naar de blade B2C-functies in Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klik op **gebruikerskenmerken**.
3. Klik op **+Toevoegen** boven aan de blade.
4. Geef een **naam** voor het aangepaste kenmerk (bijvoorbeeld ' ShoeSize') en eventueel een **beschrijving**. Klik op **Create**.
   
   > [!NOTE]
   > Alleen de 'Tekenreeks', de 'Booleaanse waarde' en 'Integer' **gegevenstypen** op dit moment beschikbaar zijn.
   > 
   > 

Het aangepaste kenmerk is nu beschikbaar in de lijst met **gebruikerskenmerken**, en voor gebruik in uw registratiebeleid.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Een aangepast kenmerk in het registratiebeleid gebruiken
1. [Volg deze stappen om te navigeren naar de blade B2C-functies in Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klik op **Registratiebeleid**.
3. Klik op het registratiebeleid (bijvoorbeeld ' B2C_1_SiUp') om deze te openen. Klik op **bewerken** aan de bovenkant van de blade.
4. Klik op **registratiekenmerken** en selecteert u het aangepaste kenmerk (bijvoorbeeld ' ShoeSize'). Klik op **OK**.
5. Klik op **toepassingsclaims** en selecteert u het aangepaste kenmerk. Klik op **OK**.
6. Klik op **opslaan** aan de bovenkant van de blade.

U kunt de functie 'Nu uitvoeren' van het beleid gebruiken om te controleren of de ervaring van consumenten. U moet nu Zie "ShoeSize" in de lijst met kenmerken die zijn verzameld tijdens de consumer Meld u aan en weergegeven in het token verzonden naar de toepassing.

## <a name="notes"></a>Opmerkingen
* Samen met registratiebeleid, kunnen ook aangepaste kenmerken worden gebruikt in het beleid voor registreren of aanmelden en beleid voor profielbewerking.
* Er is een bekende beperking van aangepaste kenmerken. Het is alleen de eerste keer dat deze wordt gebruikt in een beleid gemaakt, en niet wanneer u deze toevoegen aan de lijst met **gebruikerskenmerken**.

