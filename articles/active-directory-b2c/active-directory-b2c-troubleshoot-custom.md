---
title: Application Insights om op te lossen aangepast beleid dat is - Azure AD B2C | Microsoft Docs
description: het Application Insights om te traceren, het uitvoeren van aangepaste beleidsregels instellen
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: mtillman
editor: parakhj
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: saeda
ms.openlocfilehash: 65a39479b4d4b86d569501636e4a0678b052d426
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="azure-active-directory-b2c-collecting-logs"></a>Azure Active Directory B2C: Verzamelen van Logboeken

Dit artikel bevat stappen voor het verzamelen van Logboeken van Azure AD B2C, zodat u met uw eigen beleid problemen kunt.

>[!NOTE]
>Op dit moment wordt de gedetailleerde activiteitenlogboeken hier beschreven zijn ontworpen **alleen** helpt bij de ontwikkeling van aangepast beleid. Gebruik geen Ontwikkelingsmodus in productie.  Logboeken verzamelen van alle claims die worden verzonden naar en van de id-providers tijdens de ontwikkeling.  Als dit wordt gebruikt in productie, verantwoordelijkheid de ontwikkelaar van de voor PII (privé identificeerbare informatie) verzameld in het logboek voor de App Insights waarvan ze eigenaar.  Deze gedetailleerde logboeken worden alleen verzameld wanneer het beleid wordt geplaatst op **ONTWIKKELINGSMODUS**.


## <a name="use-application-insights"></a>Application Insights gebruiken

Azure AD B2C ondersteunt een functie voor het verzenden van gegevens naar Application Insights.  Application Insights biedt een manier voor het opsporen van uitzonderingen en prestatieproblemen toepassing visualiseren.

### <a name="setup-application-insights"></a>Application Insights instellen

1. Ga naar de [Azure Portal](https://portal.azure.com). Zorg ervoor dat u zich in de tenant met uw Azure-abonnement (niet uw Azure AD B2C-tenant).
1. Klik op **+ nieuw** in het navigatiemenu links.
1. Zoek en selecteer **Application Insights**, klikt u vervolgens op **maken**.
1. Vul het formulier en klik op **maken**. Selecteer **algemene** voor de **toepassingstype**.
1. Zodra de resource is gemaakt, opent u de Application Insights-resource.
1. Zoeken naar **eigenschappen** in het menu van links en klik erop.
1. Kopieer de **Instrumentatiesleutel** en voor de volgende sectie op te slaan.

### <a name="set-up-the-custom-policy"></a>Het aangepaste beleid instellen

1. Open het bestand RP (bijvoorbeeld SignUpOrSignin.xml).
1. De volgende kenmerken toevoegen aan de `<TrustFrameworkPolicy>` element:

  ```XML
  DeploymentMode="Development"
  UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
  ```

1. Als deze niet al bestaat, een onderliggend knooppunt toevoegen `<UserJourneyBehaviors>` naar de `<RelyingParty>` knooppunt. Het moet zich onmiddellijk na de`<DefaultUserJourney ReferenceId="UserJourney Id from your extensions policy, or equivalent (for example:SignUpOrSigninWithAAD" />`
2. Het volgende knooppunt toevoegen als een onderliggend element van de `<UserJourneyBehaviors>` element. Zorg ervoor dat u `{Your Application Insights Key}` met de **Instrumentatiesleutel** die u hebt verkregen via de Application Insights in de vorige sectie.

  ```XML
  <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
  ```

  * `DeveloperMode="true"`Hiermee geeft u ApplicationInsights voor de telemetrie via de pipeline verwerking goed snellere voor ontwikkeling, maar beperkte op hoge volumes.
  * `ClientEnabled="true"`verzendt het clientscript ApplicationInsights voor het bijhouden van pagina-fouten weergeven en clientzijde (niet nodig).
  * `ServerEnabled="true"`verzendt de bestaande UserJourneyRecorder JSON als een aangepaste gebeurtenis naar Application Insights.
Voorbeeld:

  ```XML
  <TrustFrameworkPolicy
    ...
    TenantId="fabrikamb2c.onmicrosoft.com"
    PolicyId="SignUpOrSignInWithAAD"
    DeploymentMode="Development"
    UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
  >
    ...
    <RelyingParty>
      <DefaultUserJourney ReferenceId="UserJourney ID from your extensions policy, or equivalent (for example: SignUpOrSigninWithAzureAD)" />
      <UserJourneyBehaviors>
        <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
      </UserJourneyBehaviors>
      ...
  </TrustFrameworkPolicy>
  ```

3. Upload het beleid.

### <a name="see-the-logs-in-application-insights"></a>Raadpleeg de logboeken in Application Insights

>[!NOTE]
> Er is een korte vertraging (minder dan vijf minuten) voordat u nieuwe logboeken in Application Insights kunt zien.

1. Open de Application Insights-resource die u hebt gemaakt in de [Azure-portal](https://portal.azure.com).
1. In de **overzicht** menu, klik op **Analytics**.
1. Open een nieuw tabblad in Application Insights.
1. Hier volgt een lijst van query's die kunt u raadpleegt u de logboeken

| Query’s uitvoeren | Beschrijving |
|---------------------|--------------------|
Traceringen | Zie alle logboeken die worden gegenereerd door Azure AD B2C |
traceringen \| waar tijdstempel > ago(1d) | Zie alle logboeken die worden gegenereerd door Azure AD B2C voor de laatste dag

De vermeldingen mogelijk lang.  Exporteren naar CSV voor nader bekijken.

U kunt meer informatie over het hulpprogramma analyse [hier](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).

>[!NOTE]
>De community kan een gebruiker reis viewer zodat ontwikkelaars identiteit heeft ontwikkeld.  Niet wordt ondersteund door Microsoft en beschikbaar gesteld strikt als-is.  Het leest uit uw Application Insights-exemplaar en biedt een weergave ook structuur van de gebruiker reis gebeurtenissen.  U de broncode downloaden en deze implementeren in uw eigen oplossing.

>[!NOTE]
>Op dit moment wordt de gedetailleerde activiteitenlogboeken hier beschreven zijn ontworpen **alleen** helpt bij de ontwikkeling van aangepast beleid. Gebruik geen Ontwikkelingsmodus in productie.  Logboeken verzamelen van alle claims die worden verzonden naar en van de id-providers tijdens de ontwikkeling.  Als dit wordt gebruikt in productie, verantwoordelijkheid de ontwikkelaar van de voor PII (privé identificeerbare informatie) verzameld in het logboek voor de App Insights waarvan ze eigenaar.  Deze gedetailleerde logboeken worden alleen verzameld wanneer het beleid wordt geplaatst op **ONTWIKKELINGSMODUS**.

[Github-opslagplaats voor niet-ondersteunde aangepast beleid voor voorbeelden en bijbehorende hulpprogramma 's](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies)



## <a name="next-steps"></a>Volgende stappen

Verken de gegevens in Application Insights om te begrijpen hoe de identiteit ervaring Framework onderliggende B2C werkt voor het leveren van uw eigen identiteit optreedt.
