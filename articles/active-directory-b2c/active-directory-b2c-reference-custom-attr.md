---
title: Aangepaste kenmerken definiëren in Azure Active Directory B2C | Microsoft Docs
description: Definieer aangepaste kenmerken voor uw toepassing in Azure Active Directory B2C voor het verzamelen van informatie over uw klanten.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 16907cb06d7e291a3da5b9d30b4d88cf4d50abf3
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66509587"
---
# <a name="define-custom-attributes-in-azure-active-directory-b2c"></a>Aangepaste kenmerken definiëren in Azure Active Directory B2C

 Elke toepassing klantgerichte heeft unieke vereisten van de gegevens die moeten worden opgehaald. Uw Azure Active Directory (Azure AD) B2C-tenant wordt geleverd met een ingebouwde groep die zijn opgeslagen in kenmerken, zoals de voornaam, achternaam, plaats en postcode. Met Azure AD B2C, kunt u de set kenmerken die zijn opgeslagen op elke klantaccount uitbreiden. 
 
 Kunt u aangepaste kenmerken in de [Azure-portal](https://portal.azure.com/) en deze gebruiken in uw proefaccount gebruikersstromen, registreren of aanmelden gebruikersstromen of gebruikersstromen bewerken van profielen. U kunt ook lezen en schrijven van deze kenmerken met behulp van de [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md). Aangepaste kenmerken in Azure AD B2C gebruikt [Azure AD Graph API Directory-Schemauitbreidingen](/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-directory-schema-extensions).  

> [!NOTE]
> Ondersteuning voor nieuwere [Microsoft Graph API](https://docs.microsoft.com/graph/overview?view=graph-rest-1.0) voor query's in Azure AD B2C-tenant zich nog in ontwikkeling.
>

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

Als u een nieuwe gebruiker met behulp van een beleid dat gebruikmaakt van het zojuist gemaakte aangepaste kenmerk hebt gemaakt, het object kan worden doorzocht [Azure AD Graph Explorer](https://docs.microsoft.com/azure/active-directory/develop/active-directory-graph-api-quickstart). U kunt ook kunt u de [ **gebruikersstroom uitvoeren** ](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-user-flows) functie op de gebruikersstroom om te controleren of de ervaring van de klant. U ziet nu **ShoeSize** in de lijst met kenmerken die zijn verzameld tijdens de registratie reis, en het in het token verzonden naar de toepassing te zien. 

