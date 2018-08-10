---
title: Gebruikers beheren in Azure Active Directory B2C | Microsoft Docs
description: Informatie over het identificeren van minderjarigen, datum van geboortedatum en land-gegevens verzamelen en acceptatie van gebruiksvoorwaarden ophalen in uw toepassing met behulp van Azure AD B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/24/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 15bc320bd82ea21ff608fcc834ba51b9bc7b6dea
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39716141"
---
# <a name="manage-user-access-in-azure-active-directory-b2c"></a>Gebruikers beheren in Azure Active Directory B2C

In dit artikel wordt beschreven hoe u toegang tot uw toepassingen beheren met behulp van Azure Active Directory (Azure AD) B2C. Toegangsbeheer in uw toepassing bevat:

- Minderjarigen identificeren en beheren van gebruikerstoegang tot uw toepassing.
- Vereist dat er is ouderlijke toestemming voor minderjarigen uw toepassingen te gebruiken.
- Gegevensverzameling geboortedatum en land van gebruikers.
- Vastleggen van een overeenkomst gebruiksvoorwaarden van en toegang te beperken.

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

## <a name="control-minor-access"></a>Secundaire toegang beheren

Toepassingen en organisaties kunnen besluiten te blokkeren minderjarigen met behulp van toepassingen en services die niet zijn bedoeld voor deze doelgroep. U kunt ook toepassingen en organisaties besluiten minderjarigen accepteren en vervolgens de ouderlijke toestemming beheren en toegestane ervaringen voor minderjarigen bieden, zoals bepaald door de bedrijfsregels en toegestaan door verordening. 

Als een gebruiker wordt geïdentificeerd als een minderjarige, kunt u de gebruikersstroom instellen in Azure AD B2C op een van drie opties:

- **Een ondertekende JWT-id_token verzenden naar de toepassing**: de gebruiker is geregistreerd in de map en een token wordt geretourneerd naar de toepassing. De toepassing wordt vervolgens voortgezet door toe te passen van bedrijfsregels. De toepassing kan bijvoorbeeld een proces ouderlijke toestemming voortzetten. Kies voor het gebruik van deze methode voor het ontvangen van de **ageGroup** en **consentProvidedForMinor** claims van de toepassing.

- **Verzenden van een niet-ondertekende JSON-token naar de toepassing**: aan de toepassing in Azure AD B2C meldt dat de gebruiker een minderjarige en bevat informatie over de status van de ouderlijke toestemming van de gebruiker. De toepassing wordt vervolgens voortgezet door toe te passen van bedrijfsregels. Een geslaagde verificatie met de toepassing niet wordt voltooid door een JSON-token. De toepassing moet verwerken van de niet-geverifieerde gebruiker op basis van de claims die zijn opgenomen in het JSON-token, waaronder **naam**, **e**, **ageGroup**, en **consentProvidedForMinor**.

- **Blokkeren dat de gebruiker**: als een gebruiker een minderjarige is en ouderlijke toestemming is niet opgegeven, Azure AD B2C kunt aan de gebruiker weten dat hij of zij is geblokkeerd. Geen token is uitgegeven, de toegang is geblokkeerd en het gebruikersaccount dat is niet gemaakt tijdens een reis registratie. Voor het implementeren van deze melding, bieden u een geschikte inhoud HTML/CSS-pagina om te informeren over de gebruiker en de juiste opties voor aanwezig. Er is geen verdere actie vereist door de toepassing voor nieuwe registraties.

## <a name="get-parental-consent"></a>Er is ouderlijke toestemming ophalen

Afhankelijk van de toepassing verordening mogelijk ouderlijke toestemming worden verleend door een gebruiker is geverifieerd als een volwassene. Azure AD B2C biedt geen een ervaring om te controleren of de leeftijd van een persoon en vervolgens een geverifieerde volwassene ouderlijke toestemming verlenen aan een secundaire. Deze ervaring moet worden opgegeven door de toepassing of een andere serviceprovider.

Hier volgt een voorbeeld van een beleid voor het verzamelen van ouderlijke toestemming:

1. Een [Azure Active Directory Graph API](https://msdn.microsoft.com/en-us/library/azure/ad/graph/api/api-catalog) bewerking, wordt de gebruiker als een minderjarige en de gebruikersgegevens voor de toepassing in de vorm van een niet-ondertekende JSON-token retourneert.

2. De toepassing verwerkt het JSON-token en ziet u een scherm naar de secundaire, hoogte hem of haar dat ouderlijke toestemming vereist is en de toestemming van een bovenliggende online aanvraagt. 

3. Azure AD B2C toont een reis aanmelding dat de gebruiker bij normaal aanmelden kan en problemen met een token bij de toepassing die is ingesteld om op te nemen **legalAgeGroupClassification = "minorWithParentalConsent"**. De toepassing worden verzameld van het e-mailadres van de bovenliggende en controleert of het bovenliggende object een volwassene. Om dit te doen, maakt gebruik van een vertrouwde bron, zoals een nationale ID office-, licentie-verificatie of creditcard bewijs. Als controle geslaagd is, wordt de toepassing gevraagd om de secundaire aan te melden met behulp van de stroom van de gebruiker Azure AD B2C. Als de toestemming is geweigerd (bijvoorbeeld, als **legalAgeGroupClassification = "minorWithoutParentalConsent"**), Azure AD B2C retourneert een JSON-token (niet een aanmelding) voor de toepassing de toestemming van het proces opnieuw starten. Het is eventueel mogelijk voor het aanpassen van de gebruikersstroom dat een volwassene of een secundaire weer toegang te van een secundaire account krijgen kunt door een wordt registratiecode verzonden naar e-mailadres van de secundaire of e-mailadres van de volwassene op record.

4. De toepassing biedt een optie voor de minderjarige toestemming intrekken.

5. Als de secundaire of de volwassene trekt u toestemming, de Azure AD Graph-API kan worden gebruikt om te wijzigen **consentProvidedForMinor** naar **geweigerd**. U kunt ook besluiten de toepassing te verwijderen van een secundaire waarvan toestemming is ingetrokken. Het is eventueel mogelijk voor het aanpassen van de gebruikersstroom, zodat de geverifieerde secundaire (of bovenliggende die van de secundaire-account wordt gebruikt) kan toestemming intrekken. Azure AD B2C-records **consentProvidedForMinor** als **geweigerd**.

Voor meer informatie over **legalAgeGroupClassification**, **consentProvidedForMinor**, en **ageGroup**, Zie [resourcetype van gebruiker](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/user). Zie voor meer informatie over aangepaste kenmerken [aangepaste kenmerken gebruiken voor het verzamelen van informatie over uw consumenten](active-directory-b2c-reference-custom-attr.md). Als u uitgebreide kenmerken kunt oplossen met behulp van de Azure AD Graph-API, moet u de lange versie van het kenmerk, zoals *extension_18b70cf9bb834edd8f38521c2583cd86_dateOfBirth*: *2011-01-01T00:00:00Z*.

## <a name="gather-date-of-birth-and-country-data"></a>Datum van geboortedatum en land-gegevens verzamelen

Toepassingen mogelijk afhankelijk van de Azure AD B2C voor het verzamelen van de datum van de geboortedatum (geboortedatum) en het land van alle gebruikers tijdens de registratie. Als deze informatie nog niet bestaat, de toepassing kan aanvragen het van de gebruiker tijdens de volgende reis van verificatie (aanmelden). Gebruikers kunnen niet worden voortgezet zonder op te geven hun geboortedatum en land. Azure AD B2C gebruikt de informatie om te bepalen of de afzonderlijke wordt beschouwd als een minderjarige op basis van het wettelijk verplichte standaarden van dat land. 

Een aangepaste gebruikersstroom kunt verzamelen geboortedatum en landinformatie en gebruik Azure AD B2C claims transformatie om te bepalen de **ageGroup** en het resultaat blijven (of blijven de geboortedatum en landinformatie rechtstreeks) in de map.

De volgende stappen laten zien dat de logica die wordt gebruikt voor het berekenen van **ageGroup** uit de geboortedatum van de gebruiker:

1. Proberen te vinden van het land van de landcode in de lijst. Als het land niet wordt gevonden, terugvallen op **standaard**.

2. Als de **MinorConsent** knooppunt in het land/regio-element aanwezig is:

    a. De datum waarop de gebruiker moet zijn geboren op om te worden beschouwd als een volwassene berekenen. Bijvoorbeeld, als de huidige datum is 14 maart 2015 en **MinorConsent** is 18, de geboortedatum moet niet later zijn dan 14 maart 2000.

    b. Vergelijk de minimale geboortedatum met de werkelijke geboortedatum. Als de minimale geboortedatum voordat de geboortedatum van de gebruiker is, het resultaat van de berekening **kleine** als de berekening leeftijdsgroep.

3. Als de **MinorNoConsentRequired** knooppunt is aanwezig in de land-element, herhaalt u stap 2a en 2b met behulp van de waarde van **MinorNoConsentRequired**. De uitvoer van 2b retourneert **MinorNoConsentRequired** als de minimale geboortedatum vóór de geboortedatum van de gebruiker. 

4. De berekening wordt geretourneerd als geen van beide berekening true retourneert, **volwassenen**.

Als een toepassing op betrouwbare wijze verzamelde geboortedatum of land/regio gegevens via andere methoden heeft, kan de toepassing de Graph API gebruiken om bij te werken van de record van de gebruiker met deze informatie. Bijvoorbeeld:

- Als een gebruiker bekend is dat een volwassene, werkt u de directory-kenmerk **ageGroup** met een waarde van **volwassenen**.
- Als een gebruiker bekend is dat een minderjarige, werkt u het directory-kenmerk **ageGroup** met een waarde van **kleine** en stel **consentProvidedForMinor**, indien van toepassing.

Zie voor meer informatie over het verzamelen van gegevens van de geboortedatum [leeftijdsbeperking in Azure AD B2C gebruikt](basic-age-gating.md).

## <a name="capture-terms-of-use-agreement"></a>Gebruiksvoorwaarden vastleggen

Wanneer u uw toepassing ontwikkelt, moet u normaal gesproken vastleggen gebruikers acceptatie van gebruiksvoorwaarden in hun toepassingen zonder dat er of alleen secundaire, deelname van de map. Het is wel mogelijk echter gebruik van een Azure AD B2C-gebruikersstroom te verzamelen van een gebruiker acceptatie van gebruiksvoorwaarden, beperken van toegang als acceptatie is niet verleend en afdwingen van instemming met de toekomstige wijzigingen in de gebruiksrechtovereenkomst, op basis van de datum van de meest recente transformatieregels voor acceptatie en de datum van de  meest recente versie van de gebruiksvoorwaarden.

**Gebruiksvoorwaarden** kan ook 'Toestemming geven om gegevens te delen met derden.' bevatten Afhankelijk van lokale voorschriften en bedrijfsregels, kunt u de acceptatie van een gebruiker van beide voorwaarden gecombineerd verzamelen of kunt u de gebruiker één voorwaarde en de andere niet te accepteren.

De volgende stappen wordt beschreven hoe u gebruiksvoorwaarden kunt beheren:

1. De transformatieregels voor acceptatie van de gebruiksvoorwaarden en de datum van acceptatie vastleggen met behulp van de Graph API en uitgebreide kenmerken. U kunt dit doen met behulp van zowel ingebouwde en aangepaste gebruikersstromen. We raden aan dat u maakt en gebruikt de **extension_termsOfUseConsentDateTime** en **extension_termsOfUseConsentVersion** kenmerken.

2. Maken van een vereiste selectievakje met het label 'Gebruiksvoorwaarden accepteren' en het resultaat opnemen tijdens de registratie. U kunt dit doen met behulp van zowel ingebouwde en aangepaste gebruikersstromen.

3. Azure AD B2C worden de voorwaarden van de gebruiksrechtovereenkomst en acceptatie van de gebruiker opgeslagen. U kunt de Graph-API aan query gebruiken voor de status van een gebruiker door te lezen van het kenmerk toestelnummer die wordt gebruikt om vast te leggen van de reactie (bijvoorbeeld lezen **termsOfUseTestUpdateDateTime**). U kunt dit doen met behulp van zowel ingebouwde en aangepaste gebruikersstromen.

4. Vereist acceptatie van bijgewerkte gebruiksvoorwaarden door het vergelijken van de datum van instemming met de datum van de meest recente versie van de gebruiksvoorwaarden. U kunt de datums vergelijken alleen met behulp van een aangepast beleid. Gebruik de uitgebreide kenmerk **extension_termsOfUseConsentDateTime**, en vergelijk de waarde op de claim van **termsOfUseTextUpdateDateTime**. Als de transformatieregels voor acceptatie verouderd is, dwingt u een nieuwe acceptatie door een zelf-gecontroleerde scherm weer te geven. Anders wordt de toegang blokkeren met behulp van de logica van beleid.

5. Instemming met de bijgewerkte gebruiksvoorwaarden door het vergelijken van het versienummer van de transformatieregels voor acceptatie op de meest recente geaccepteerde versienummer dat is vereist. U kunt de versienummers vergelijken alleen met behulp van een aangepast beleid. Gebruik de uitgebreide kenmerk **extension_termsOfUseConsentDateTime**, en vergelijk de waarde op de claim van **extension_termsOfUseConsentVersion**. Als de transformatieregels voor acceptatie verouderd is, dwingt u een nieuwe acceptatie door een zelf-gecontroleerde scherm weer te geven. Anders wordt de toegang blokkeren met behulp van de logica van beleid.

U kunt voorwaarden van de acceptatie van de gebruik onder de volgende scenario's kunt vastleggen:

- Een nieuwe gebruiker zich aanmeldt. De gebruiksvoorwaarden worden weergegeven en de acceptatie-resultaat wordt opgeslagen.
- Een gebruiker zich aanmeldt die eerder de meest recente of actieve gebruiksrechtovereenkomst heeft geaccepteerd. De gebruiksvoorwaarden worden niet weergegeven.
- Een gebruiker zich aanmeldt die al niet de meest recente of actieve gebruiksrechtovereenkomst heeft geaccepteerd. De gebruiksvoorwaarden worden weergegeven en de acceptatie-resultaat wordt opgeslagen.
- Een gebruiker zich aanmeldt die al een oudere versie van de gebruiksrechtovereenkomst, die nu zijn bijgewerkt naar de nieuwste versie heeft geaccepteerd. De gebruiksvoorwaarden worden weergegeven en de acceptatie-resultaat wordt opgeslagen.

De volgende afbeelding ziet u de aanbevolen gebruikersstroom:

![Acceptatie van de gebruikersstroom](./media/manage-user-access/user-flow.png) 

Hier volgt een voorbeeld van een datum/tijd op basis van voorwaarden van toestemming voor het gebruik in een claim:

```
<ClaimsTransformations>
  <ClaimsTransformation Id="GetNewUserAgreeToTermsOfUseConsentDateTime" TransformationMethod="GetCurrentDateTime">
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="extension_termsOfUseConsentDateTime" TransformationClaimType="currentDateTime" />
    </OutputClaims>
  </ClaimsTransformation>
  <ClaimsTransformation Id="IsTermsOfUseConsentRequired" TransformationMethod="IsTermsOfUseConsentRequired">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="extension_termsOfUseConsentDateTime" TransformationClaimType="termsOfUseConsentDateTime" />
    </InputClaims>
    <InputParameters>
      <InputParameter Id="termsOfUseTextUpdateDateTime" DataType="dateTime" Value="2098-01-30T23:03:45" />
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="termsOfUseConsentRequired" TransformationClaimType="result" />
    </OutputClaims>
  </ClaimsTransformation>
</ClaimsTransformations>
```

Hier volgt een voorbeeld van een versie die op basis van voorwaarden van toestemming voor het gebruik in een claim:

```
<ClaimsTransformations>
  <ClaimsTransformation Id="GetEmptyTermsOfUseConsentVersionForNewUser" TransformationMethod="CreateStringClaim">
    <InputParameters>
      <InputParameter Id="value" DataType="string" Value=""/>
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="extension_termsOfUseConsentVersion" TransformationClaimType="createdClaim" />
    </OutputClaims>
  </ClaimsTransformation>
  <ClaimsTransformation Id="GetNewUserAgreeToTermsOfUseConsentVersion" TransformationMethod="CreateStringClaim">
    <InputParameters>
      <InputParameter Id="value" DataType="string" Value="V1"/>
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="extension_termsOfUseConsentVersion" TransformationClaimType="createdClaim" />
    </OutputClaims>
  </ClaimsTransformation>
  <ClaimsTransformation Id="IsTermsOfUseConsentRequiredForVersion" TransformationMethod="CompareClaimToValue">
    <InputClaims>
      <InputClaim ClaimTypeReferenceId="extension_termsOfUseConsentVersion" TransformationClaimType="inputClaim1" />
    </InputClaims>
    <InputParameters>
      <InputParameter Id="compareTo" DataType="string" Value="V1" />
      <InputParameter Id="operator" DataType="string" Value="not equal" />
      <InputParameter Id="ignoreCase" DataType="string" Value="true" />
    </InputParameters>
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="termsOfUseConsentRequired" TransformationClaimType="outputClaim" />
    </OutputClaims>
  </ClaimsTransformation>
</ClaimsTransformations> 
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het verwijderen en exporteren van gebruikersgegevens, [gebruikersgegevens beheren](manage-user-data.md).
