---
title: Application Insights om op te lossen aangepast beleid in Azure Active Directory B2C | Microsoft Docs
description: hoe u tot het instellen van Application Insights aan het traceren van de uitvoering van aangepaste beleidsregels.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: a6ec4c7d239754fe3211b528dd0ac64ee150ad3c
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58089366"
---
# <a name="azure-active-directory-b2c-collecting-logs"></a>Azure Active Directory B2C: Verzamelen van Logboeken

Dit artikel bevat stappen voor het verzamelen van Logboeken van Azure AD B2C, zodat u met uw aangepaste beleidsregels problemen kunt.

>[!NOTE]
>Op dit moment de gedetailleerde activiteitenlogboeken die hier worden beschreven zijn ontworpen **alleen** om te helpen bij de ontwikkeling van aangepaste beleidsregels. Gebruik geen Ontwikkelingsmodus in productie.  Logboeken verzamelen van alle claims verzonden naar en van de id-providers tijdens de ontwikkeling.  Als in de productieomgeving gebruikt, verantwoordelijkheid de ontwikkelaar van de krijgt voor PII (privé Identifiable Information) die in het logboek voor App Insights waarvan ze eigenaar zijn verzameld.  Deze gedetailleerde logboeken worden alleen verzameld wanneer het beleid wordt geplaatst op **ONTWIKKELINGSMODUS**.


## <a name="use-application-insights"></a>Application Insights gebruiken

Azure AD B2C ondersteunt een functie voor het verzenden van gegevens naar Application Insights.  Application Insights biedt een manier om de diagnose-uitzonderingen en prestatieproblemen toepassing visualiseren.

### <a name="setup-application-insights"></a>Application Insights instellen

1. Ga naar de [Azure Portal](https://portal.azure.com). Zorg ervoor dat u zich in de tenant met uw Azure-abonnement (niet uw Azure AD B2C-tenant).
1. Klik op **+ nieuw** in het navigatiemenu aan.
1. Zoek en selecteer **Application Insights**, klikt u vervolgens op **maken**.
1. Vul het formulier in en klikt u op **maken**. Selecteer **algemene** voor de **toepassingstype**.
1. Zodra de resource is gemaakt, opent u de Application Insights-resource.
1. Zoek **eigenschappen** in de in het menu links en klik erop.
1. Kopieer de **Instrumentatiesleutel** en bewaar het voor de volgende sectie.

### <a name="set-up-the-custom-policy"></a>Het aangepaste beleid instellen

1. Open het RP-bestand (bijvoorbeeld SignUpOrSignin.xml).
1. De volgende kenmerken toevoegen aan de `<TrustFrameworkPolicy>` element:

   ```XML
   DeploymentMode="Development"
   UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
   ```

1. Als deze niet al bestaat, Voeg een onderliggend knooppunt `<UserJourneyBehaviors>` naar de `<RelyingParty>` knooppunt. Deze moeten zich direct na de `<DefaultUserJourney ReferenceId="UserJourney Id from your extensions policy, or equivalent (for example:SignUpOrSigninWithAAD" />`
2. Het volgende knooppunt toevoegen als een onderliggende site van de `<UserJourneyBehaviors>` element. Vervang `{Your Application Insights Key}` met de **Instrumentatiesleutel** die u hebt verkregen vanuit Application Insights in de vorige sectie.

   ```XML
   <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
   ```

   * `DeveloperMode="true"` Geeft aan Application Insights te bekorten de telemetrie via de verwerkings-pipeline goed voor ontwikkeling, maar beperkte bij hoge volumes.
   * `ClientEnabled="true"` verzendt het Application Insights-client-side-script voor het bijhouden van pagina-fouten bekijken en aan de clientzijde (niet nodig).
   * `ServerEnabled="true"` de bestaande UserJourneyRecorder JSON verzonden als een aangepaste gebeurtenis naar Application Insights.
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

### <a name="see-the-logs-in-application-insights"></a>Zie de logboeken in Application Insights

>[!NOTE]
> Er is een korte vertraging (minder dan vijf minuten) voordat u nieuwe logboeken in Application Insights kunt zien.

1. Open de Application Insights-resource die u hebt gemaakt in de [Azure-portal](https://portal.azure.com).
1. In de **overzicht** menu, klikt u op **Analytics**.
1. Open een nieuw tabblad in Application Insights.
1. Hier volgt een lijst met query's die kunt u de logboeken

| Query’s uitvoeren | Description |
|---------------------|--------------------|
traceringen | Zie alle logboeken die worden gegenereerd door Azure AD B2C |
traceringen \| waar tijdstempel > ago(1d) | Zie alle logboeken die worden gegenereerd door Azure AD B2C voor de laatste dag

De vermeldingen mogelijk lang.  Exporteren naar CSV voor nader bekijken.

U kunt meer informatie over het Analytics-hulpprogramma [hier](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).

>[!NOTE]
>De community heeft ontwikkeld, een gebruiker reis viewer zodat ontwikkelaars die identiteit.  Niet wordt ondersteund door Microsoft en beschikbaar gesteld strikt als-is.  Leest van uw Application Insights-instantie en biedt een gestructureerde weergave van de gebruiker reis gebeurtenissen.  U de broncode downloaden en deze in uw eigen oplossing te implementeren.

De versie van de viewer die gebeurtenissen in Application Insights leest bevindt [hier](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies/tree/master/wingtipgamesb2c/src/WingTipUserJourneyPlayerWebApplication)

>[!NOTE]
>Op dit moment de gedetailleerde activiteitenlogboeken die hier worden beschreven zijn ontworpen **alleen** om te helpen bij de ontwikkeling van aangepaste beleidsregels. Gebruik geen Ontwikkelingsmodus in productie.  Logboeken verzamelen van alle claims verzonden naar en van de id-providers tijdens de ontwikkeling.  Als in de productieomgeving gebruikt, verantwoordelijkheid de ontwikkelaar van de krijgt voor PII (privé Identifiable Information) die in het logboek voor App Insights waarvan ze eigenaar zijn verzameld.  Deze gedetailleerde logboeken worden alleen verzameld wanneer het beleid wordt geplaatst op **ONTWIKKELINGSMODUS**.

[GitHub-opslagplaats voor niet-ondersteunde aangepast beleid voor voorbeelden en meer hulpprogramma 's](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies)



## <a name="next-steps"></a>Volgende stappen

Verken de gegevens in Application Insights om te begrijpen hoe optreedt in de Identity-Ervaringsframework onderliggende B2C werkt met het oog op uw eigen identiteit.
