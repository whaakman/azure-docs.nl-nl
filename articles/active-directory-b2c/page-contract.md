---
title: Selecteer een pagina-contract in Azure Active Directory B2C | Microsoft Docs
description: Meer informatie over het selecteren van een pagina-contract in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 637cdb338496764e64c18a887673808ef4e8415a
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55203450"
---
# <a name="select-a-page-contract-in-azure-active-directory-b2c-using-custom-policies"></a>Selecteer een pagina-contract in Azure Active Directory B2C met behulp van aangepaste beleidsregels

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

U kunt een contract pagina in Azure Active Directory (Azure AD) B2C selecteren door te configureren in een [aangepast beleid](active-directory-b2c-overview-custom.md). Een pagina-contract is een associatie met elementen die Azure AD B2C biedt en de inhoud die u opgeeft. Als u van plan bent te gebruiken [Javascript](javascript-samples.md), moet u de versie van een contract voor al uw inhoudsdefinities in uw aangepast beleid te definiëren.

## <a name="replace-datauri-values"></a>Gegevens-URI die waarden vervangen

In uw aangepaste beleidsregels, hebt u mogelijk [ContentDefinitions](contentdefinitions.md) waarmee de HTML-sjablonen gebruikt in de gebruikersbeleving worden gedefinieerd. De **ContentDefinition** bevat een **gegevens-URI die** die verwijst naar de pagina-elementen die is geleverd door Azure AD B2C. De **LoadUri** is het relatieve pad naar de HTML en CSS-inhoud die u opgeeft.

```XML
<ContentDefinition Id="api.idpselections">
  <LoadUri>~/tenant/default/idpSelector.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Idp selection page</Item>
    <Item Key="language.intro">Sign in</Item>
  </Metadata>
</ContentDefinition>
```

Als u wilt een contract pagina selecteert, wijzigt u de **gegevens-URI die** waarden in uw [ContentDefinitions](contentdefinitions.md) in uw beleid. Door het overschakelen van de oude **gegevens-URI die** waarden naar de nieuwe waarden, bent u een onveranderbaar pakket te selecteren. Het voordeel van het gebruik van dit pakket is dat u weet het wordt niet wijzigen en leiden tot onverwacht gedrag op de pagina. 

Als u een pagina-contract instelt, gebruikt u de volgende tabel om te zoeken **gegevens-URI die** waarden. 

| Oude gegevens-URI die waarde | Nieuwe gegevens-URI die waarde |
| ----------------- | ----------------- |
| urn: com:microsoft:aad:b2c:elements:idpselection:1.0.0 | urn: com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0 |
| urn: com:microsoft:aad:b2c:elements:unifiedssd:1.0.0 | urn: com:microsoft:aad:b2c:elements:contract:unifiedssd:1.0.0 | 
| urn: com:microsoft:aad:b2c:elements:claimsconsent:1.0.0 | urn: com:microsoft:aad:b2c:elements:contract:claimsconsent:1.0.0 |
| urn: com:microsoft:aad:b2c:elements:multifactor:1.0.0 | urn: com:microsoft:aad:b2c:elements:contract:multifactor:1.0.0 |
| urn: com:microsoft:aad:b2c:elements:multifactor:1.1.0 | urn: com:microsoft:aad:b2c:elements:contract:multifactor:1.1.0 |
| urn: com:microsoft:aad:b2c:elements:selfasserted:1.0.0 | urn: com:microsoft:aad:b2c:elements:contract:selfasserted:1.0.0 |
| urn: com:microsoft:aad:b2c:elements:selfasserted:1.1.0 | urn: com:microsoft:aad:b2c:elements:contract:selfasserted:1.1.0 | 
| urn: com:microsoft:aad:b2c:elements:unifiedssp:1.0.0 | urn: com:microsoft:aad:b2c:elements:contract:unifiedssp:1.0.0 |
| urn: com:microsoft:aad:b2c:elements:unifiedssp:1.1.0 | urn: com:microsoft:aad:b2c:elements:contract:unifiedssp:1.1.0 |
| urn: com:microsoft:aad:b2c:elements:globalexception:1.0.0 | urn: com:microsoft:aad:b2c:elements:contract:globalexception:1.0.0 |
| urn: com:microsoft:aad:b2c:elements:globalexception:1.1.0 | urn: com:microsoft:aad:b2c:elements:contract:globalexception:1.1.0 |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe u de gebruikersinterface van uw toepassingen kunt aanpassen [aanpassen van de gebruikersinterface van uw toepassing met behulp van een aangepast beleid in Azure Active Directory B2C](active-directory-b2c-ui-customization-custom.md).



