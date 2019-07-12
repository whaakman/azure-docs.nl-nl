---
title: Selecteer een pagina-contract - Azure Active Directory B2C
description: Meer informatie over het selecteren van een pagina-contract in Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/04/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: c3ee05096b0bfd071ea569105973097ce9727b07
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604531"
---
# <a name="select-a-page-contract-in-azure-active-directory-b2c-using-custom-policies"></a>Selecteer een pagina-contract in Azure Active Directory B2C met behulp van aangepaste beleidsregels

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Of u nu werkt met gebruikersstromen of aangepast beleid, kunt u de JavaScript-code voor client-side inschakelen in uw Azure Active Directory (Azure AD) B2C-beleid. JavaScript inschakelen voor uw toepassingen, moet u een element dat u wilt toevoegen de [aangepast beleid](active-directory-b2c-overview-custom.md), selecteert u een contract pagina en gebruik [b2clogin.com](b2clogin.md) in uw aanvragen.

Een pagina-contract is een samenwerkingsverband van elementen die Azure AD B2C biedt en de inhoud die u opgeeft.

In dit artikel wordt beschreven hoe u een pagina-contract in Azure AD B2C door deze te configureren in een aangepast beleid selecteren.

> [!NOTE]
> Als u wilt voor het inschakelen van JavaScript voor gebruikersstromen, Zie [JavaScript en pagina ondersteuningscontract versies in Azure Active Directory B2C](user-flow-javascript-overview.md).

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
| `urn:com:microsoft:aad:b2c:elements:claimsconsent:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:claimsconsent:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:globalexception:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:globalexception:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.1.0` |
| `urn:com:microsoft:aad:b2c:elements:idpselection:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:multifactor:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:multifactor:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.1.0` |
| `urn:com:microsoft:aad:b2c:elements:selfasserted:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.1.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssd:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssd:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.1.0` |

## <a name="version-change-log"></a>Versie-wijzigingenlogboek

Pagina contract pakketten worden regelmatig bijgewerkt oplossingen en verbeteringen in de paginaelementen bevatten. De volgende wijzigingenlogboek Hiermee geeft u de wijzigingen in elke versie.

### <a name="110"></a>1.1.0

- Uitzonderingspagina (globalexception)
  - Fix voor toegankelijkheid
  - Het standaardbericht verwijderd wanneer er geen contact met het beleid
  - Standaard CSS verwijderd
- MFA-pagina (multifactor)
  - 'Controleer of de Code' knop verwijderd
  - Het invoerveld voor de code nu duurt invoeren maximaal zes (6) tekens
  - De pagina wordt automatisch geprobeerd om te controleren of de code hebt ingevoerd toen een 6-cijferige code is ingevoerd, zonder een knop hoeft te worden geklikt
  - Als de code is onjuist dat het invoerveld worden automatisch gewist
  - Na drie (3) pogingen met een onjuiste code wordt B2C een fout teruggestuurd naar de relying party
  - Toegankelijkheid voor problemen
  - Standaard CSS verwijderd
- Zelf-gecontroleerde pagina (selfasserted)
  - Waarschuwing verwijderd annuleren
  - CSS-klasse voor fout elementen
  - Fout-logica verbeterd weergeven/verbergen
  - Standaard CSS verwijderd
- Geïntegreerde SSP (unifiedssp)
  - Toegevoegde aangemeld blijven in besturingselement (KMSI)

### <a name="100"></a>1.0.0

- Eerste release

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe u de gebruikersinterface van uw toepassingen kunt aanpassen [aanpassen van de gebruikersinterface van uw toepassing met behulp van een aangepast beleid in Azure Active Directory B2C](active-directory-b2c-ui-customization-custom.md).
