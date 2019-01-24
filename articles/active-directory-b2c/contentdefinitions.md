---
title: ContentDefinitions - Azure Active Directory B2C | Microsoft Docs
description: Geef het ContentDefinitions-element van een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 1fa5688b5895ed7418161234f6af06fecace631c
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2019
ms.locfileid: "54849376"
---
# <a name="contentdefinitions"></a>ContentDefinitions

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

U kunt het uiterlijk van een [door technisch profiel zelf de bevestigde](self-asserted-technical-profile.md). Azure Active Directory (Azure AD) B2C wordt code wordt uitgevoerd in de browser van uw klant en maakt gebruik van een moderne manier Cross-Origin Resource Sharing (CORS) genoemd. 

Voor het aanpassen van de gebruikersinterface, u een URL opgeven in de **ContentDefinition** element met aangepaste HTML-inhoud. In het profiel van de zelf-gecontroleerde technische of **OrchestrationStep**, u verwijzen naar die inhoud definitie-id. De definitie van de inhoud kan bevatten een **LocalizedResourcesReferences** element dat hiermee een lijst wordt gelokaliseerde resources om te laden. Azure AD B2C worden samengevoegd gebruikersinterface-elementen met de HTML-inhoud die wordt geladen vanaf uw URL en vervolgens wordt de pagina voor de gebruiker.

De **ContentDefinitions** element met URL's in HTML5-sjablonen die kunnen worden gebruikt in een gebruikersbeleving. De pagina HTML5 URI wordt gebruikt voor een opgegeven gebruikersinterface stap. Bijvoorbeeld: het aanmelden of registreren voor wachtwoord opnieuw instellen of foutpagina's. U kunt het uiterlijk wijzigen door de LoadUri voor het HTML5-bestand te overschrijven. U kunt nieuwe inhoudsdefinities maken op basis van uw behoeften. Dit element mag een verwijzing gelokaliseerde resources aan de lokalisatie-id opgegeven in de [lokalisatie](localization.md) element.

Het volgende voorbeeld ziet u de definitie van de inhoud-id en de definitie van gelokaliseerde bronnen:

```XML
<ContentDefinition Id="api.localaccountsignup">
  <LoadUri>~/tenant/default/selfAsserted.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Local account sign up page</Item>
  </Metadata>
  <LoalizedResourcesReferences MergeBehavior="Prepend">
    <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="api.localaccountsignup.en" />
    <LocalizedResourcesReference Language="es" LocalizedResourcesReferenceId="api.localaccountsignup.es" />
    ...
```

De metagegevens van de **LocalAccountSignUpWithLogonEmail** zelf door de bevestigde technisch profiel bevat de definitie van de inhoud-id **ContentDefinitionReferenceId** ingesteld op `api.localaccountsignup`

```XML
<TechnicalProfile Id="LocalAccountSignUpWithLogonEmail">
  <DisplayName>Email signup</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ContentDefinitionReferenceId">api.localaccountsignup</Item>
    ...
  </Metadata>
  ...
```


## <a name="contentdefinition"></a>ContentDefinition

De **ContentDefinition** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| Id | Ja | Een id voor een inhoudsdefinitie. De waarde is opgegeven in de **definitie-id's van inhoud** verderop in deze pagina. |

De **ContentDefinition** element bevat de volgende elementen:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| LoadUri | 1:1 | Een tekenreeks is die de URL van het HTML5-pagina voor de definitie van de inhoud bevat. |
| RecoveryUri | 0:1 | Een tekenreeks is die de URL van de HTML-pagina voor het weergeven van een fout met betrekking tot de definitie van de inhoud bevat. | 
| DataUri | 1:1 | Een tekenreeks zijn met de relatieve URL van een HTML-bestand met de ervaring van de gebruiker om aan te roepen voor de stap. |  
| Metagegevens | 1:1 | Een verzameling van sleutel/waarde-paren die de metagegevens gebruikt door de definitie van de inhoud bevat. | 
| LocalizedResourcesReferences | 0:1 | Een verzameling van gelokaliseerde bronnen verwijzingen. Dit element gebruiken om aan te passen de lokalisatie van een gebruikerskenmerk voor interface en claims. |

### <a name="datauri"></a>DataUri

De **gegevens-URI die** element wordt gebruikt om op te geven van de pagina-id. Azure AD B2C maakt gebruik van de id van de pagina te laden en UI-elementen en clientzijde JavaScript initiëren. De indeling van de waarde is `urn:com:microsoft:aad:b2c:elements:page-name:version`.  De volgende tabel bevat van de pagina-id die u kunt gebruiken.

| Waarde |   Description |
| ----- | ----------- |
| `urn:com:microsoft:aad:b2c:elements:globalexception:1.1.0` | Een foutpagina weergegeven als er een uitzondering of een fout wordt aangetroffen. |
| `urn:com:microsoft:aad:b2c:elements:idpselection:1.0.0` | Een lijst met de id-providers die gebruikers uit tijdens het aanmelden kiezen kunnen. | 
| `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0` | Een formulier voor het aanmelden met een lokaal account dat gebaseerd op een e-mailadres of de naam van een gebruiker wordt weergegeven. Deze waarde biedt ook de 'Houd mij op de functionaliteit voor aanmelden' en 'Wachtwoord vergeten?' koppeling. | 
| `urn:com:microsoft:aad:b2c:elements:unifiedssd:1.0.0` | Een formulier voor het aanmelden met een lokaal account dat gebaseerd op een e-mailadres of de naam van een gebruiker wordt weergegeven. |
| `urn:com:microsoft:aad:b2c:elements:multifactor:1.1.0` | Controleert of telefoonnummers via SMS of spraak tijdens het registreren of aanmelden. |
| `urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0` | Een formulier waarmee gebruikers maken of bijwerken van hun profiel wordt weergegeven. | 


### <a name="localizedresourcesreferences"></a>LocalizedResourcesReferences

De **LocalizedResourcesReferences** element bevat de volgende elementen:

| Element | Gevallen | Description |
| ------- | ----------- | ----------- |
| LocalizedResourcesReference | 1: n | Een lijst met gelokaliseerde resource-verwijzingen voor de definitie van de inhoud. | 

De **LocalizedResourcesReferences** element bevat de volgende kenmerken:

| Kenmerk | Vereist | Description |
| --------- | -------- | ----------- |
| Taal | Ja | Een tekenreeks met een ondersteunde taal voor het beleid per RFC 5646 - Tags voor talen te identificeren. |
| LocalizedResourcesReferenceId | Ja | De id van de **LocalizedResources** element. |

Het volgende voorbeeld ziet u een inhoudsdefinitie registreren of aanmelden:

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>~/tenant/default/unified.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Signin and Signup</Item>
  </Metadata>
</ContentDefinition>
```

Het volgende voorbeeld ziet u een inhoudsdefinitie met registreren of aanmelden met een verwijzing naar lokalisatie voor Engels, Frans en Spaans:

```XML
<ContentDefinition Id="api.signuporsignin">
  <LoadUri>~/tenant/default/unified.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Signin and Signup</Item>
  </Metadata>
  <LocalizedResourcesReferences MergeBehavior="Prepend">
    <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="api.signuporsignin.en" />
    <LocalizedResourcesReference Language="fr" LocalizedResourcesReferenceId="api.signuporsignin.rf" />
    <LocalizedResourcesReference Language="es" LocalizedResourcesReferenceId="api.signuporsignin.es" />
</LocalizedResourcesReferences>
</ContentDefinition>
```

Zie voor meer informatie over lokalisatieondersteuning voor toevoegen aan uw inhoudsdefinities, [lokalisatie](localization.md).

## <a name="content-definition-ids"></a>Inhoudsdefinitie id 's

Het kenmerk ID van de **ContentDefinition** element Hiermee geeft u het type van de pagina die is gekoppeld aan de definitie van de inhoud. Het element definieert de context die u een aangepaste HTML5/CSS-sjabloon wilt toepassen. De volgende tabel beschrijft de set inhoudsdefinitie id's die wordt herkend door de Identiteitservaring-Framework en de typen van de pagina's die betrekking hebben op deze. U kunt uw eigen inhoudsdefinities maken met een willekeurige ID.

| Id | Standaardsjabloon | Description | 
| -- | ---------------- | ----------- |
| **api.error** | [exception.cshtml](https://login.microsoftonline.com/static/tenant/default/exception.cshtml) | **Foutpagina** : geeft een fout weergeeft wanneer een uitzondering of een fout is opgetreden. |
| **api.idpselections** | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **Pagina voor het id-provider selecteren** -een lijst met id-providers die gebruikers uit tijdens het aanmelden kiezen kunnen. De opties zijn meestal enterprise id-providers, sociale id-providers, zoals Facebook en Google + of lokale accounts. |
| **api.idpselections.signup** | [idpSelector.cshtml](https://login.microsoftonline.com/static/tenant/default/idpSelector.cshtml) | **Selectie van de id-provider voor aanmelding bij** -een lijst met id-providers die gebruikers uit tijdens de registratie kiezen kunnen. De opties zijn meestal enterprise id-providers, sociale id-providers, zoals Facebook en Google + of lokale accounts. |
| **api.localaccountpasswordreset** | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Pagina voor vergeten wachtwoorden** -een formulier die gebruikers moeten worden voltooid voor het starten van een wachtwoord opnieuw instellen wordt weergegeven. |
| **api.localaccountsignin** | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Lokaal account aanmelden pagina** -een formulier voor het aanmelden met een lokaal account dat gebaseerd op een e-mailadres of de naam van een gebruiker wordt weergegeven. Een tekstvak voor invoer en wachtwoord vermelding in bevatten het formulier. |
| **api.localaccountsignup** | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Pagina voor het registreren van lokale account** -een formulier om zich te registreren voor een lokaal account dat gebaseerd op een e-mailadres of de naam van een gebruiker wordt weergegeven. Het formulier kan bevatten verschillende besturingselementen voor tekstinvoer, zoals: invoer een text box, een wachtwoordinvoervak, een keuzerondje, vervolgkeuzelijsten enkelvoudige selectie, en selectievakjes voor meervoudige selectie. |
| **api.phonefactor** | [multifactor-1.0.0.cshtml](https://login.microsoftonline.com/static/tenant/default/multifactor-1.0.0.cshtml) | **Multi-factor authentication-pagina** -telefoonnummers, verifieert met behulp van tekst of spraak, tijdens het registreren of aanmelden. |
| **api.selfasserted** | [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) | **Aanmeldpagina voor sociaal account** -een formulier die gebruikers moeten worden voltooid wanneer ze zich aanmelden met behulp van een bestaand account van een sociale id-provider wordt weergegeven. Deze pagina is vergelijkbaar met de voorgaande sociaal account aanmelden pagina, met uitzondering van de velden van de vermelding wachtwoord. |
| **api.selfasserted.profileupdate** | [updateprofile.html](https://login.microsoftonline.com/static/tenant/default/updateProfile.cshtml) | **Update-profielpagina** -een formulier die voor gebruikers toegankelijk voor het bijwerken van hun profiel wordt weergegeven. Deze pagina is vergelijkbaar met de pagina, met uitzondering van de vermelding wachtwoordvelden sociaal account aanmelden. |
| **api.signuporsignin** | [unified.html](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) | **Uniforme pagina voor registreren of aanmelden** -de gebruiker zich kunnen registreren en aanmelden proces worden verwerkt. Gebruikers kunnen enterprise id-providers, sociale id-providers zoals Facebook of Google + of lokale accounts gebruiken. |
 
