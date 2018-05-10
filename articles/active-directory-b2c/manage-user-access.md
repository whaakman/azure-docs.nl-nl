---
title: Gebruikers beheren in Azure AD B2C | Microsoft Docs
description: Informatie over het identificeren van minderjarigen, datum geboortedatum en land gegevens verzamelen en acceptatie van voorwaarden voor het gebruik in uw toepassing met behulp van Azure AD B2C ophalen.
services: active-directory-b2c
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.workload: identity
ms.topic: article
ms.date: 05/04/2018
ms.author: davidmu
ms.openlocfilehash: c44135a3069966b14d8760e4daa009ab8d39ccca
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="manage-user-access-in-azure-ad-b2c"></a>Gebruikers beheren in Azure AD B2C

In dit artikel bevat informatie over hoe u gebruikerstoegang tot uw toepassingen met behulp van Azure Active Directory (AD) B2C kunt beheren. Toegangsbeheer in uw toepassing bevat:

- Minderjarigen identificeren en beheren van toegang tot uw toepassing gebruiken
- Ouderlijke toestemming voor minderjarigen gebruik van uw toepassingen vereisen
- Verzamelen van gegevens van geboortedatum en land-gegevens van de gebruiker
- Vastleggen van de voorwaarden van overeenkomst en beperken van toegang gebruiken

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

>[!Note] 
>In dit artikel bevat informatie die kan worden gebruikt ter ondersteuning van uw verplichtingen onder de GDPR. Als u algemene informatie over GDPR zoekt, raadpleegt u de [GDPR sectie van de portal Service vertrouwen](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

## <a name="control-minor-access"></a>Secundaire toegang beheren

Toepassingen en organisaties kunnen besluiten te blokkeren minderjarigen toepassingen en services die niet zijn bedoeld voor deze doelgroep te gebruiken. U kunt ook toepassingen en organisaties besluiten minderjarigen accepteren en vervolgens de ouderlijke toestemming te beheren en toegestane ervaringen voor minderjarigen leveren, zoals bepaald door de bedrijfsregels en toegestaan door de regelgeving. 

Als een gebruiker wordt geïdentificeerd als een secundaire, kan de gebruikersstroom in Azure AD B2C worden ingesteld op een van drie opties:

- **Verzenden van een ondertekende JWT id_token terug naar de toepassing** : de gebruiker is geregistreerd in de map en een token wordt geretourneerd naar de toepassing. Klik in de toepassing wordt voortgezet met behulp van bedrijfsregels. De toepassing kan bijvoorbeeld een proces ouderlijke toestemming voortzetten. U doet dit door u kiest voor het ontvangen van de **ageGroup** en **consentProvidedForMinor** claims van de toepassing.
- **Een niet-ondertekende JSON-token verzendt naar de toepassing** -aan de toepassing in Azure AD B2C meldt de gebruiker is een secundaire en wordt de status van de ouderlijke toestemming van de gebruiker. Klik in de toepassing wordt voortgezet met behulp van bedrijfsregels. Een geslaagde verificatie met de toepassing niet wordt voltooid door een JSON-token. De toepassing is verwerkt door de niet-geverifieerde gebruiker volgens de claims die zijn opgenomen in het JSON-token, waaronder **naam**, **e**, **ageGroup**, en **consentProvidedForMinor**.
- **Voorkomen dat de gebruiker** - als de gebruiker een secundaire is en ouderlijke toestemming is niet opgegeven.  Azure AD B2C kan vervolgens een scherm naar de gebruiker die informeert van geblokkeerd opleveren.  Geen token is uitgegeven, de toegang is geblokkeerd en de gebruikersaccount is niet gemaakt tijdens een reis registratie. Hiervoor moet u een geschikte HTML/CSS inhoudspagina om te informeren over de gebruiker en de juiste opties aanwezig opgeven. Er is geen verdere actie vereist door de toepassing voor nieuwe registraties.

## <a name="get-parental-consent"></a>Ouderlijke toestemming ophalen

Afhankelijk van de toepassing regelgeving mogelijk ouderlijke toestemming vereist om te worden toegekend door een gebruiker is geverifieerd als een volwassene.  Azure AD B2C biedt geen een ervaring om te controleren of de leeftijd van een persoon en laat een geverifieerde volwassene ouderlijke toestemming verlenen aan een secundaire.  Deze ervaring moet geleverd worden door de toepassing of een andere serviceprovider.

Hier volgt een voorbeeld van een gebruikersstroom om ouderlijke toestemming te verzamelen:

1. Een [Azure Active Directory Graph API](https://msdn.microsoft.com/en-us/library/azure/ad/graph/api/api-catalog) bewerking, wordt de gebruiker als een secundaire en retourneert de gebruikersgegevens naar de toepassing in de vorm van een niet-ondertekende JSON-token.
2. De toepassing verwerkt het JSON-token en ziet u een scherm naar de secundaire meegedeeld dat ouderlijke toestemming is vereist en toestemming van een bovenliggende online-aanvragen. 
3. Azure AD B2C toont een aanmeldingspagina-reis waarbij de gebruiker is toegestaan normaal aanmelden en een token uitgeeft aan de toepassing die is ingesteld om op te nemen **legalAgeGroupClassification = 'minorWithParentalConsent'** verzamelt van de toepassing de e-mailadres van de bovenliggende en wordt gecontroleerd of het bovenliggende element een volwassene met behulp van een vertrouwde bron, zoals een nationale ID office-, licentie-verificatie of creditcard bewijs. Als dit lukt, wordt de secundaire aan te melden met behulp van de stroom van de gebruiker Azure AD B2C gevraagd in de toepassing. Als u toestemming is geweigerd (bijvoorbeeld **legalAgeGroupClassification = 'minorWithoutParentalConsent'**, Azure AD B2C retourneert een JSON-token (niet aanmelden) aan de toepassing toestemming het proces opnieuw starten. Het is eventueel mogelijk om de gebruikersstroom waarbij een volwassene of een secundaire kunt weer toegang tot een secundaire account door een registratiecode te sturen naar de secundaire e-mailadres of de volwassene e-mailadres op record te passen.
4. De toepassing biedt een optie voor de secundaire toestemming intrekken.
5. Wanneer de secundaire of volwassene trekt toestemming, de Azure AD Graph API kan worden gebruikt om te wijzigen **consetProvidedForMinor** naar **geweigerd**. De toepassing mogelijk ook voor kiezen om te verwijderen van een secundaire waarvan toestemming is ingetrokken. Het is eventueel mogelijk voor het aanpassen van de gebruikersstroom waarbij de geverifieerde secundaire (of de bovenliggende account gebruikt de secundaire) toestemming kan intrekken. Azure AD B2C-records **consentProvidedForMinor** als **geweigerd**.

Voor meer informatie over de **legalAgeGroupClassification**, **consentProvidedForMinor**, en **ageGroup**, Zie [gebruiker brontype](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/user). Zie voor meer informatie over aangepaste kenmerken [gebruik van aangepaste kenmerken voor het verzamelen van informatie over uw consumenten](active-directory-b2c-reference-custom-attr.md). Bij het opstellen van uitgebreide kenmerken met behulp van Azure AD Graph API de lange versie van het kenmerk moet worden gebruikt zoals 'extension_18b70cf9bb834edd8f38521c2583cd86_dateOfBirth': ' 2011-01-01T00:00:00Z '

## <a name="gather-date-of-birth-and-country-data"></a>Datum van geboortedatum en land gegevens verzamelen

Toepassingen kunnen vertrouwen op Azure AD B2C voor het verzamelen van geboortedatum (geboortedatum) en het land van alle gebruikers tijdens de registratie. Als geboortedatum of landinformatie nog niet bestaat, deze kan worden gevraagd van de gebruiker tijdens de volgende reis voor verificatie (sign-in). Gebruikers niet mogelijk om door te gaan zonder op te geven, geboortedatum en landinformatie. Op basis van de landcode en het opgegeven geboortedatum, Azure AD B2C wordt bepaald of de afzonderlijke wordt beschouwd als een secundaire volgens de normen regelgeving van dat land. 

Een aangepaste gebruikersstroom kunt verzamelen geboortedatum en land- en gebruik Azure AD B2C transformatie om te bepalen claims de **ageGroup** en persistent maken van het resultaat (of de landcode en het geboortedatum informatie rechtstreeks behouden) in de map.

De volgende stappen ziet u de logica die wordt gebruikt voor het berekenen van **ageGroup** van Geboortedatum:

1. Probeer het land op de landcode vinden in de lijst. Als u het land niet wordt gevonden, terugvallen op **standaard**.
2. Als de **MinorConsent** knooppunt in het land-element aanwezig is:  <br>a. De vroegste datum die de gebruiker moest geboren zijn op om te worden beschouwd als een volwassene berekenen. Voorbeeld: Geboortedatum is 3-14-2015 en **MinorConsent** is 18, minimale geboortedatum zou 14-3-2000.
    <br>b. Vergelijk de minimale geboortedatum met de werkelijke geboortedatum. Als de minimale geboortedatum voordat de geboortedatum van de gebruiker is, het resultaat van de berekening **secundaire** als de leeftijd groepsberekening.
3. Als de **MinorNoConsentRequired** knooppunt is aanwezig in de land-element, herhaalt u stap 2a en 2b met behulp van de waarde van **MinorNoConsentRequired**. Retourneert de uitvoer van 2b **MinorNoConsentRequired** als de minimale geboortedatum voordat de geboortedatum van de gebruiker is. 
4. Geen van beide berekeningen geretourneerd waar het resultaat van de berekening **volwassenen**.

Als een toepassing betrouwbaar verzamelde geboortedatum of land gegevens via andere methoden heeft, kan de toepassing de GRAPH API gebruiken voor het bijwerken van de gebruikersrecord met deze informatie. Bijvoorbeeld:

- Als een gebruiker bekend is dat deze een volwassene, werkt het kenmerk directory **ageGroup** met een waarde van **volwassenen**.
- Als een gebruiker bekend is dat deze een secundaire, werkt het kenmerk directory **ageGroup** met een waarde van **secundaire** en stel **consentProvidedForMinor** zo nodig.

Zie voor meer informatie over het verzamelen van gegevens van de geboortedatum [met leeftijd beperken in Azure AD B2C](basic-age-gating.md).

## <a name="capture-terms-of-use-agreement"></a>Vastleggen van de gebruiksrechtovereenkomst

Wanneer u uw toepassing ontwikkelt, moet u doorgaans vastleggen van de gebruiker acceptatie van voorwaarden voor het gebruik binnen hun toepassingen, zonder dat er, of alleen secundaire deelname uit de map van de gebruiker.  Het is mogelijk, maar voor het gebruik van een Azure AD B2C gebruiker stromen naar overeenkomst van gebruiksvoorwaarden verzamelen, tenzij acceptatie is verleend en af te dwingen acceptatie van toekomstige wijzigingen in de gebruiksvoorwaarden op basis van de datum van laatste acceptatie en de datum van de meest recente versio de toegang beperken n van de gebruiksvoorwaarden.

**Gebruiksvoorwaarden** kan ook 'Toestemming om gegevens te delen met derden.' bevatten  Het positieve acceptatie van deze voorwaarden van een gebruiker technisch mogelijk worden verzameld als gecombineerd of de gebruiker kan zich mogelijk één en niet op de andere, afhankelijk van de lokale regelgeving en bedrijfsregels te accepteren.

De volgende stappen beschrijven de mogelijkheden voor het beheren van gebruiksvoorwaarden:

1. Record instemming met de gebruiksvoorwaarden en de datum van instemming met behulp van de Graph API en uitgebreide kenmerken. U kunt dit doen met behulp van beide ingebouwde en aangepaste gebruiker stromen. Het is raadzaam dat u maakt en gebruikt de **extension_termsOfUseConsentDateTime** en **extension_termsOfUseConsentVersion** kenmerken.
2. Maken van een selectievakje vereist uit met de titel 'Gebruiksvoorwaarden accepteren' en het resultaat vastleggen bij het aanmelden. U kunt dit doen met behulp van beide ingebouwde en aangepaste gebruiker stromen.
3. Azure AD B2C slaat de voorwaarden van de gebruiksrechtovereenkomst en toestemming. De Graph API kan worden gebruikt om op te vragen voor de status van een gebruiker door te lezen van het kenmerk toestelnummer gebruikt voor het vastleggen van het antwoord, bijvoorbeeld lezen **termsOfUseTestUpdateDateTime**. U kunt dit doen met behulp van beide ingebouwde en aangepaste gebruiker stromen.
4. Vereisen dat gebruikers van bijgewerkte gebruiksvoorwaarden door te vergelijken met de datum van de acceptatie van de datum van de nieuwste versie van de gebruiksvoorwaarden. Dit kan alleen worden uitgevoerd met behulp van een aangepaste gebruiker-stroom. Gebruik het uitgebreide kenmerk **extension_termsOfUseConsentDateTime** en vergelijk de waarde voor de claim van **termsOfUseTextUpdateDateTime**als acceptatie is oud en dwing vervolgens een nieuwe acceptatie toegang met behulp van de logica van beleid zelf die wordt beweerd scherm, anders worden geblokkeerd.
5. Vereisen dat gebruikers van bijgewerkte gebruiksvoorwaarden door te vergelijken met het versienummer van de acceptatie van de laatste geaccepteerde versienummer. Dit kan alleen worden uitgevoerd met behulp van een aangepaste gebruiker-stroom. Gebruik het uitgebreide kenmerk **extension_termsOfUseConsentDateTime** en vergelijk de waarde voor de claim van **extension_termsOfUseConsentVersion**als acceptatie is oud en dwing vervolgens een nieuwe acceptatie toegang met behulp van de logica van beleid zelf die wordt beweerd scherm, anders worden geblokkeerd.

Vastleggen van de voorwaarden van gebruik toestemming kan worden gepresenteerd aan de gebruiker onder de volgende scenario's:

- Een nieuwe gebruiker zich aanmeldt. De gebruiksvoorwaarden worden weergegeven. het resultaat toestemming is opgeslagen.
- Een gebruiker zich aanmeldt en toestemming voor de meest recente of actieve voorwaarden van de overeenkomst al eerder is geaccepteerd. De gebruiksvoorwaarden worden niet weergegeven.
- Een gebruiker zich aanmeldt en heeft niet de toestemming voor de meest recente of actieve gebruiksvoorwaarden geaccepteerd. De gebruiksvoorwaarden worden weergegeven. het resultaat toestemming is opgeslagen.
- Een gebruiker zich aanmeldt en toestemming voor een oudere gebruiksrechtovereenkomst, die nu wordt bijgewerkt naar een nieuwere versie is al worden geaccepteerd. De gebruiksvoorwaarden worden weergegeven. het resultaat toestemming is opgeslagen.

De volgende afbeelding ziet u de aanbevolen gebruikersstroom:

![acceptatie van de gebruikersstroom](./media/manage-user-access/user-flow.png) 

Hier volgt een voorbeeld van de voorwaarden van een datum-/ op basis van toestemming voor het gebruik in een claim:

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

Hier volgt een voorbeeld van de voorwaarden van een versie die op basis van toestemming voor het gebruik in een claim:

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

- Informatie over het verwijderen en het exporteren van gebruikersgegevens in [gebruikersgegevens beheren](manage-user-data.md)
