---
title: Een profiel van de technische definiëren in een aangepast beleid in Azure Active Directory B2C | Microsoft Docs
description: Definieer het technische profiel van een Azure Active Directory in een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: f08d5732939e853785e2cce1d1e9ed8eea7f9ad7
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45543136"
---
# <a name="define-a-validation-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Een profiel van de technische definiëren in een aangepast beleid voor Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]
 
Het technische profiel van een validatie is een gewone technisch profiel van een protocol, zoals [Azure Active Directory](active-directory-technical-profile.md) of een [REST-API](restful-technical-profile.md). Het technische validatieprofiel uitvoerclaims resultaat of een foutbericht HTTP 409 (Conflict antwoordstatuscode), met de volgende gegevens:

```JSON
{
    "version": "1.0.0",
    "status": 409,
    "userMessage": "Your error message"
}
```

Claims die zijn retuned vanuit een technisch profiel van de validatie worden opnieuw toegevoegd aan de eigenschappenverzameling claims. U kunt deze claims in de volgende technische validatie-profielen.

Technische profielen validatie worden uitgevoerd in de volgorde waarin ze worden weergegeven in de **ValidationTechnicalProfiles** element. U kunt configureren in een profiel van de technische of de uitvoering van de volgende validatie technische profielen moet nog steeds als de validatie van technisch profiel wordt een fout gegenereerd of voltooid is.  

Een validatie technisch profiel voorwaardelijk kan worden uitgevoerd op basis van voorwaarden die zijn gedefinieerd in de **ValidationTechnicalProfile** element. U kunt bijvoorbeeld controleren of een bepaalde claims aanwezig is, of als een claim gelijk is of niet aan de opgegeven waarde.

Een zelf-gecontroleerde technisch profiel kan een validatie technisch profiel moet worden gebruikt voor het valideren van sommige of alle van de uitvoerclaims definiëren. Alle van de invoer claims van de waarnaar wordt verwezen, technisch profiel moet worden weergegeven in de uitvoerclaims van de verwijzende validatie technisch profiel.


## <a name="validationtechnicalprofiles"></a>ValidationTechnicalProfiles

De **ValidationTechnicalProfiles** element bevat de volgende elementen:

| Element | Gevallen | Beschrijving |
| ------- | ----------- | ----------- |
| ValidationTechnicalProfile | 1: n | Een technisch profiel moet worden gebruikt voor het valideren van sommige of alle van de uitvoerclaims van de verwijzende technisch profiel. |

De **ValidationTechnicalProfile** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| referenceId | Ja | Een id van een technisch profiel al gedefinieerd in het beleid of het bovenliggende beleid. |
|ContinueOnError|Nee| Geeft aan of validatie van de validatie van de volgende technische profielen blijven moet als deze validaiton technisch profiel wordt een fout gegenereerd. Maximale waarden: `true` of `false` (standaard de verwerking van verdere validatie profielen wordt gestopt en is een fout geretourneerd). 
|ContinueOnSuccess | Nee | Geeft aan of validatie van de validatie van de volgende profielen blijven moet als deze technische validatieprofiel is geslaagd. Maximale waarden: `true` of `false`. De standaardwaarde is `true`, wat inhoudt dat de verwerking van verdere validatie profielen blijft. |

De **ValidationTechnicalProfile** element bevat het volgende element:

| Element | Gevallen | Beschrijving |
| ------- | ----------- | ----------- |
| Voorwaarden | 0:1 | Een lijst met voorwaarden waaraan moet worden voldaan voor het technische validatieprofiel uit te voeren. |

De **voorwaarde** element bevat het volgende kenmerk:

| Kenmerk | Vereist | Beschrijving |
| --------- | -------- | ----------- |
| Type | Ja | Het type selectievakje of de query uit te voeren voor de voorwaarde. Een van beide `ClaimsExist` om ervoor te zorgen dat de acties moeten worden uitgevoerd als de opgegeven claims aanwezig is in de huidige een claimset van de gebruiker is opgegeven of `ClaimEquals` is opgegeven dat de acties die moeten worden uitgevoerd als de opgegeven claim bestaat en de waarde gelijk is aan de opgegeven waarde. |
| ExecuteActionsIf | Ja | Geeft aan of de acties in de eerste voorwaarde moeten worden uitgevoerd als de test true of false is. | 

De **voorwaarde** element bevat de volgende elementen:

| Element | Gevallen | Beschrijving |
| ------- | ----------- | ----------- |
| Waarde | 1: n | De gegevens die wordt gebruikt door de controle. Als het type van deze controle is `ClaimsExist`, dit veld geeft aan dat een ClaimTypeReferenceId om op te vragen. Als het type van controle is `ClaimEquals`, dit veld geeft aan dat een ClaimTypeReferenceId om op te vragen. Terwijl een andere waarde-element de waarde bevat moet worden gecontroleerd.|
| Bewerking | 1:1 | De actie die moet worden uitgevoerd als de controle van de voorwaarde in een orchestration-stap ingesteld op true is. De waarde van de **actie** is ingesteld op `SkipThisValidationTechnicalProfile`. Hiermee geeft u de gekoppelde validatieregels technisch profiel moet niet worden uitgevoerd. | 

### <a name="example"></a>Voorbeeld

Volgende voorbeeld wordt met deze technische validatie-profielen: 

1. De eerste validatie van technisch profiel gebruikersreferenties controleert en als er een fout, zoals een ongeldige gebruikersnaam of onjuist wachtwoord optreedt wordt niet voortgezet. 
2. De volgende technische validatieprofiel, niet worden uitgevoerd als de claim userType niet bestaat of als de waarde van het userType `Partner`. Het technische validatieprofiel probeert te lezen van het gebruikersprofiel in de database interne klant bent en doorgaan als een fout, zoals REST API-service niet beschikbaar of een interne fout optreedt.
3. De laatste validatie technisch profiel, wordt niet uitgevoerd als de claim userType niet al bestond, of als de waarde van het userType `Customer`. Het technische validatieprofiel probeert te lezen van het gebruikersprofiel in de interne partnerdatabase en wordt voortgezet als een fout, zoals REST API-service niet beschikbaar of een interne fout optreedt.

```XML
<ValidationTechnicalProfiles>
  <ValidationTechnicalProfile ReferenceId="login-NonInteractive" ContinueOnError="false"  />
    
  <ValidationTechnicalProfile ReferenceId="REST-ReadProfileFromCustomertsDatabase" ContinueOnError="true" >
    <Preconditions>
       <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
          <Value>userType</Value>
          <Action>SkipThisValidationTechnicalProfile</Action>
       </Precondition>
       <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
         <Value>userType</Value>
         <Value>Partner</Value>
         <Action>SkipThisValidationTechnicalProfile</Action>
       </Precondition>
    </Preconditions>          
  </ValidationTechnicalProfile>

  <ValidationTechnicalProfile ReferenceId="REST-ReadProfileFromPartnersDatabase" ContinueOnError="true" >
    <Preconditions>
       <Precondition Type="ClaimsExist" ExecuteActionsIf="false">
          <Value>userType</Value>
          <Action>SkipThisValidationTechnicalProfile</Action>
       </Precondition>
       <Precondition Type="ClaimEquals" ExecuteActionsIf="true">
         <Value>userType</Value>
         <Value>Customer</Value>
         <Action>SkipThisValidationTechnicalProfile</Action>
       </Precondition>
    </Preconditions>          
  </ValidationTechnicalProfile>
</ValidationTechnicalProfiles>
```














