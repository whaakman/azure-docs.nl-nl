---
title: 'Azure Active Directory B2C: Aangepaste kenmerken | Microsoft Docs'
description: Het gebruik van aangepaste kenmerken in Azure Active Directory B2C informatie verzamelen over uw consumenten
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mtillman
editor: bryanla
ms.assetid: 055ffb0a-197b-4716-8dad-1fd8a01e174f
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 3e2c8b3ab223cf269129d7723c37d98c24f0653c
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>Azure Active Directory B2C: Gebruik aangepaste kenmerken voor het verzamelen van informatie over uw consumenten
Uw Azure Active Directory (Azure AD) B2C-directory wordt geleverd met een ingebouwde verzameling gegevens (kenmerken): voornaam, achternaam, plaats, postcode en andere kenmerken. Elke consumentgerichte toepassing heeft echter unieke vereisten op welke kenmerken voor het verzamelen van consumenten. Met Azure AD B2C, kunt u de set kenmerken die zijn opgeslagen op elke consumentenaccount uitbreiden. U kunt aangepaste kenmerken maken op de [Azure-portal](https://portal.azure.com/) en deze gebruiken in uw registratie-beleid, zoals hieronder wordt weergegeven. U kunt ook lezen en schrijven van deze kenmerken met behulp van de [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).

> [!NOTE]
> Gebruik van aangepaste kenmerken [Azure AD Graph API Directory-Schemauitbreidingen](https://msdn.microsoft.com/library/azure/dn720459.aspx).
> 
> 

## <a name="create-a-custom-attribute"></a>Een aangepast kenmerk maken
1. [Volg deze stappen om te navigeren naar de blade B2C-functies in de Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klik op **gebruikerskenmerken**.
3. Klik op **+Toevoegen** boven aan de blade.
4. Geef een **naam** voor het aangepaste kenmerk (bijvoorbeeld ' ShoeSize') en eventueel een **beschrijving**. Klik op **Create**.
   
   > [!NOTE]
   > Alleen de 'Tekenreeks', de 'Boolean' en 'Integer' **gegevenstypen** zijn momenteel beschikbaar.
   > 
   > 

Het aangepaste kenmerk is nu beschikbaar in de lijst met **gebruikerskenmerken**, en voor gebruik in uw registratie-beleid.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Gebruik een aangepast kenmerk in het registratiebeleid
1. [Volg deze stappen om te navigeren naar de blade B2C-functies in de Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).
2. Klik op **Registratiebeleid**.
3. Klik op het registratiebeleid (bijvoorbeeld ' B2C_1_SiUp') om deze te openen. Klik op **bewerken** boven aan de blade.
4. Klik op **registratiekenmerken** en selecteert u het aangepaste kenmerk (bijvoorbeeld ' ShoeSize'). Klik op **OK**.
5. Klik op **toepassingsclaims** en selecteert u het aangepaste kenmerk. Klik op **OK**.
6. Klik op **opslaan** boven aan de blade.

U kunt de functie 'Nu uitvoeren' op het beleid gebruiken om te controleren of de consumer-ervaring. U moet nu Zie 'ShoeSize' in de lijst met kenmerken die zijn verzameld tijdens consumenten registreren en deze weergegeven in het token terug naar uw toepassing verzonden.

## <a name="notes"></a>Opmerkingen
* Samen met aanmelding beleid kunnen ook aangepaste kenmerken worden gebruikt in beleid voor het registreren of aanmelden en profiel bewerken van beleid.
* Er is een bekende beperking van aangepaste kenmerken. Het is alleen de eerste keer wordt gebruikt in een beleid gemaakt en niet wanneer u deze toevoegen aan de lijst met **gebruikerskenmerken**.

