---
title: Aangepaste kenmerken definiëren in Azure Active Directory B2C | Microsoft Docs
description: Definieer aangepaste kenmerken voor uw toepassing in Azure Active Directory B2C voor het verzamelen van informatie over uw klanten.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 53d9aec5a689babb637d2eff6b36ea3b8bd8d97f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55173935"
---
# <a name="define-custom-attributes-in-azure-active-directory-b2c"></a>Aangepaste kenmerken definiëren in Azure Active Directory B2C

 Elke toepassing klantgerichte heeft unieke vereisten van de gegevens die moeten worden opgehaald. Uw Azure Active Directory (Azure AD) B2C-tenant wordt geleverd met een ingebouwde groep die zijn opgeslagen in kenmerken, zoals de voornaam, achternaam, plaats en postcode. Met Azure AD B2C, kunt u de set kenmerken die zijn opgeslagen op elke klantaccount uitbreiden. 
 
 Kunt u aangepaste kenmerken in de [Azure-portal](https://portal.azure.com/) en deze gebruiken in uw proefaccount gebruikersstromen, registreren of aanmelden gebruikersstromen of gebruikersstromen bewerken van profielen. U kunt ook lezen en schrijven van deze kenmerken met behulp van de [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md). Aangepaste kenmerken in Azure AD B2C gebruikt [Azure AD Graph API Directory-Schemauitbreidingen](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-directory-schema-extensions).

## <a name="create-a-custom-attribute"></a>Een aangepast kenmerk maken

1. Meld u als globale beheerder van de Azure AD B2C-tenant aan bij [Azure Portal](https://portal.azure.com/).
2. Zorg ervoor dat u de map met uw Azure AD B2C-tenant gebruikt door hiernaar over te schakelen rechtsboven in de Azure Portal. Selecteer de abonnementsgegevens en selecteer vervolgens **Schakelen tussen mappen**. 

    ![Overschakelen naar de Azure AD B2C-tenant](./media/active-directory-b2c-reference-custom-attr/switch-directories.png)

    Kies de map met uw tenant.

    ![Map selecteren](./media/active-directory-b2c-reference-custom-attr/select-directory.png)

3. Kies **Alle services** linksboven in de Azure Portal, zoek **Azure AD B2C** en selecteer deze.
4. Selecteer **gebruikerskenmerken**, en selecteer vervolgens **toevoegen**.
5. Geef een **naam** voor het aangepaste kenmerk (bijvoorbeeld ' ShoeSize")
6. Kies een **gegevenstype**. Alleen **tekenreeks**, **Booleaanse**, en **Int** beschikbaar zijn.
7. Geef eventueel een **beschrijving** ter informatie bedoeld. 
8. Klik op **Create**.

Het aangepaste kenmerk is nu beschikbaar in de lijst met **gebruikerskenmerken** en voor gebruik in uw gebruikersstromen. Een aangepast kenmerk wordt alleen de eerste keer dat deze wordt gebruikt in een beleid gemaakt, en niet wanneer u deze toevoegen aan de lijst met **gebruikerskenmerken**.

## <a name="use-a-custom-attribute-in-your-user-flow"></a>Gebruik een aangepast kenmerk in de gebruikersstroom

1. Selecteer in uw Azure AD B2C-tenant, **gebruikersstromen**.
2. Selecteer het beleid (bijvoorbeeld ' B2C_1_SignupSignin') om dit te openen. 
4. Selecteer **gebruikerskenmerken** en selecteer vervolgens het aangepaste kenmerk (bijvoorbeeld ' ShoeSize'). Klik op **Opslaan**.
5. Selecteer **toepassingsclaims** en selecteer vervolgens het aangepaste kenmerk. 
6. Klik op **Opslaan**.

U kunt de **gebruikersstroom uitvoeren** functie op de gebruikersstroom om te controleren of de ervaring van de klant. U ziet nu **ShoeSize** in de lijst met kenmerken die zijn verzameld tijdens de registratie reis, en het in het token verzonden naar de toepassing te zien.

