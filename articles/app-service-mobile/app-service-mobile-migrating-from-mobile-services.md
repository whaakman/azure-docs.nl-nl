---
title: Migreren van Mobile Services naar een mobiele App Service-App
description: Meer informatie over het eenvoudig migreren uw Mobile Services-toepassing in een App Service-mobiele App
services: app-service\mobile
documentationcenter: 
author: conceptdev
manager: crdun
editor: 
ms.assetid: 07507ea2-690f-4f79-8776-3375e2adeb9e
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: crdun
ms.openlocfilehash: f3d89c627f462c9e34b2ff067972be56f5bed32f
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="article-top"></a>Uw bestaande Azure Mobile Service migreren naar Azure App Service
Met de [algemene beschikbaarheid van Azure App Service], Azure Mobile Services-sites kunnen worden eenvoudig gemigreerd ter plekke om te profiteren van alle functies van de Azure App Service.  Dit document wordt uitgelegd wat u kunt verwachten wanneer uw site uit Azure Mobile Services migreren naar Azure App Service.

## <a name="what-does-migration-do"></a>Wat doet migratie naar uw site
Migratie van uw Azure Mobile Service Hiermee schakelt u uw mobiele Service in een [Azure App Service] app zonder de code.  Uw Notification Hubs, SQL gegevensverbinding verificatie-instellingen, geplande taken en domeinnaam blijven ongewijzigd.  Mobiele clients met behulp van uw Azure Mobile Service blijven werken normaal.  Migratie wordt uw service opnieuw gestart zodra deze worden overgedragen naar Azure App Service.

[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="why-migrate"></a>Waarom moet u uw site migreren
Microsoft is aanbevelen migreren van uw Azure Mobile Service om te profiteren van de functies van Azure App Service, waaronder:

* Nieuwe host-functies, waaronder [WebJobs] en [aangepaste domeinnamen].
* Bewaking en probleemoplossing met New Relic of [Application Insights].
* Ingebouwde DevOps tooling, met inbegrip van [staging-sleuven], terugdraaien en in productie testen.
* [Automatische schaling], taakverdeling, en [prestatiebewaking].

Zie voor meer informatie over de voordelen van Azure App Service, de [tegenover Mobile Services. App Service] onderwerp.

## <a name="before-you-begin"></a>Voordat u begint
Voordat u begint met belangrijke werk op uw site, moet u back-up van uw mobiele Service scripts en SQL-database.

## <a name="migrating-site"></a>Migreren van uw sites
Het migratieproces worden gemigreerd van alle sites binnen één Azure-regio.

Voor het migreren van uw site:

1. Meld u aan bij de [klassieke Azure-Portal].
2. Selecteer een mobiele Service in de regio die u wilt migreren.
3. Klik op de **migreren naar App Service** knop.

   ![De knop migreren][0]
4. Lees de migreren naar het dialoogvenster App Service.
5. Voer de naam van uw mobiele Service in het daarvoor bestemde vak.  Bijvoorbeeld, als uw domeinnaam contoso.azure mobile.net is, voer dan *contoso* in het daarvoor bestemde vak.
6. Klik op de knop maatstreepjes.

De status van de migratie in de activiteitscontrole bewaken. Uw site wordt vermeld als *migreren* in de klassieke Azure-Portal.

  ![Migratie-activiteitsbewaking][1]

Elke migratie kan duren vanaf 3 en 15 minuten per mobiele service die wordt gemigreerd.  Uw site blijft beschikbaar tijdens de migratie.
Uw site wordt opnieuw opgestart aan het einde van het migratieproces.  De site is niet beschikbaar tijdens het opstarten, die een paar seconden duurt.

## <a name="finalizing-migration"></a>De migratie voltooien
Plan voor het testen van uw site van een mobiele client aan het einde van het migratieproces.  Zorg ervoor dat u alle Clientacties in de algemene zonder wijzigingen in de mobiele clients kunt uitvoeren.  

### <a name="update-app-service-tier"></a>Selecteer een geschikte App Service-prijscategorie
Hebt u meer flexibiliteit in na de migratie naar Azure App Service-prijzen.

1. Meld u aan bij [Azure-portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van de gemigreerde mobiele Service.
3. Standaard wordt de blade instellingen geopend.
4. Klik op **App Service-Plan** in het menu instellingen.
5. Klik op de **prijscategorie** tegel.
6. Klik op de tegel die geschikt is voor uw vereisten en klik vervolgens op **Selecteer**.  Mogelijk moet u klikken op **weergeven van alle** om te zien van de beschikbare Prijscategorieën.

Als uitgangspunt raden we aan de volgende lagen:

| Prijscategorie mobiele Service | App Service-prijscategorie |
|:--- |:--- |
| Gratis |F1 Free |
| Basic |B1 Basic |
| Standard |S1 Standard |

Er is aanzienlijk flexibiliteit bij het kiezen van het recht prijscategorie voor uw toepassing.  Raadpleeg [prijzen van App Service] voor volledige informatie over de prijzen van uw nieuwe App Service.

> [!TIP]
> De App Service-standaardcategorie bevat toegang tot veel functies die u gebruiken wilt mogelijk, met inbegrip van [staging-sleuven], automatische back-ups en automatisch schalen.  Bekijk de nieuwe mogelijkheden terwijl u er bent!
>
>

### <a name="review-migration-scheduler-jobs"></a>Bekijk de gemigreerde Scheduler-taken
Scheduler-taken wordt niet meer weergegeven tot ongeveer 30 minuten na de migratie.  Geplande taken blijven op de achtergrond uitgevoerd.
Uw geplande taken weergeven nadat ze opnieuw zichtbaar zijn:

1. Meld u aan bij [Azure-portal].
2. Selecteer **Bladeren >**, voer **planning** in de *Filter* vak en selecteer vervolgens **Scheduler verzamelingen**.

Er zijn een beperkt aantal free-scheduler-taken beschikbaar na de migratie.  Bekijk uw gebruik en de [Azure Scheduler plannen].

### <a name="configure-cors"></a>CORS configureren, indien nodig
Cross-origin-resources delen is een techniek waarmee een website voor toegang tot een Web-API in een ander domein.  Als u van Azure Mobile Services met een koppeling naar een website gebruikmaakt, moet u CORS configureren als onderdeel van de migratie.  Als u Azure Mobile Services uitsluitend op mobiele apparaten gebruiken, klikt u vervolgens hoeft CORS niet te worden geconfigureerd, behalve in uitzonderlijke gevallen.

De gemigreerde CORS-instellingen zijn beschikbaar als de **MS_CrossDomainWhitelist** App-instelling.  Uw site naar de App Service CORS-faciliteit migreren:

1. Meld u aan bij [Azure-portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van de gemigreerde mobiele Service.
3. Standaard wordt de blade instellingen geopend.
4. Klik op **CORS** in het menu API.
5. Voer geen toegestane oorsprongen in het daarvoor bestemde vak, drukt u op Enter na elke opdracht.
6. Nadat uw lijst met toegestane oorsprongen correct is, klikt u op de knop opslaan.

> [!TIP]
> Een van de voordelen van het gebruik van een Azure App Service is dat u uw website en de mobiele service op dezelfde site uitvoeren kunt.  Zie voor meer informatie de [Vervolgstappen](#next-steps) sectie.
>
>

### <a name="download-publish-profile"></a>Een nieuwe publicatieprofiel downloaden
Het publicatieprofiel van uw site wordt gewijzigd wanneer u migreert naar Azure App Service.  Als u publiceren van uw site vanuit Visual Studio wilt, moet u een nieuwe publicatieprofiel.  Het nieuwe publicatieprofiel downloaden:

1. Meld u aan bij [Azure-portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van de gemigreerde mobiele Service.
3. Klik op **Get publicatieprofiel**.

Het bestand met publicatie-instellingen wordt op uw computer gedownload.  Normaal gesproken wordt aangeroepen *sitename*. Met publicatie-instellingen.  De publicatie-instellingen importeren in een bestaand project:

1. Open Visual Studio en het Azure Mobile Service-project.
2. Met de rechtermuisknop op het project in de **Solution Explorer** en selecteer **publiceren...**
3. Klik op **importeren**
4. Klik op **Bladeren** en selecteert u het gedownloade bestand publicatie-instellingen.  Klik op **OK**
5. Klik op **Validate Connection** om te controleren of het werk van de instellingen voor publiceren.
6. Klik op **publiceren** voor het publiceren van uw site.

## <a name="working-with-your-site"></a>Werken met uw na de sitemigratie
Beginnen met werken met uw nieuwe App Service in de [Azure-portal] na de migratie.  Hieronder vindt u informatie op specifieke bewerkingen die u hebt gebruikt om uit te voeren de [klassieke Azure-Portal], samen met hun App Service-equivalent.

### <a name="publishing-your-site"></a>Downloaden en publiceren van een gemigreerde site
Uw site is beschikbaar via de git- of FTP- en met verschillende verschillende mechanismen, met inbegrip van web Deploy, TFS, volgt, GitHub en FTP-opnieuw kan worden gepubliceerd.  De referenties voor de implementatie worden gemigreerd met de rest van uw site.  Als u uw implementatiereferenties niet ingesteld of als u deze niet meer weet, kunt u ze opnieuw instellen:

1. Meld u aan bij [Azure-portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van de gemigreerde mobiele Service.
3. Standaard wordt de blade instellingen geopend.
4. Klik op **implementatiereferenties** bij het publiceren menu.
5. Voer de referenties voor de nieuwe implementatie in de desbetreffende vakken en klik vervolgens op de knop opslaan.

U kunt deze referenties klonen van de site met git of geautomatiseerde implementaties van GitHub, TFS of volgt instellen.  Zie voor meer informatie de [Azure App Service-Implementatiedocumentatie].

### <a name="appsettings"></a>Toepassingsinstellingen
De meeste instellingen voor een gemigreerde mobiele service zijn beschikbaar via de App-instellingen.  U krijgt een lijst met de appinstellingen van de [Azure-portal].
Weergeven of wijzigen van uw appinstellingen:

1. Meld u aan bij [Azure-portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van de gemigreerde mobiele Service.
3. Standaard wordt de blade instellingen geopend.
4. Klik op **toepassingsinstellingen** in het menu Algemeen.
5. Blader naar het gedeelte instellingen van de App en uw app-instelling te vinden.
6. Klik op de waarde van de app-instelling om de waarde te bewerken.  Klik op **opslaan** om op te slaan van de waarde.

U kunt meerdere app-instellingen op hetzelfde moment bijwerken.

> [!TIP]
> Er zijn twee toepassingsinstellingen met dezelfde waarde.  Bijvoorbeeld, wordt er *ApplicationKey* en *MS\_ApplicationKey*.  Beide toepassingsinstellingen op hetzelfde moment bijwerken.
>
>

### <a name="authentication"></a>Verificatie
Alle verificatieinstellingen zijn beschikbaar als de App-instellingen in uw gemigreerde site.  Voor het bijwerken van uw verificatie-instellingen, moet u de juiste app-instellingen wijzigen.  De volgende tabel ziet u de juiste app-instellingen voor uw provider voor verificatie:

| Provider | Client-id | Clientgeheim | Andere instellingen |
|:--- |:--- |:--- |:--- |
| Microsoft-account |**MS\_MicrosoftClientID** |**MS\_MicrosoftClientSecret** |**MS\_MicrosoftPackageSID** |
| Facebook |**MS\_FacebookAppID** |**MS\_FacebookAppSecret** | |
| Twitter |**MS\_TwitterConsumerKey** |**MS\_TwitterConsumerSecret** | |
| Google |**MS\_GoogleClientID** |**MS\_GoogleClientSecret** | |
| Azure AD |**MS\_AadClientID** | |**MS\_AadTenants** |

Opmerking: **MS\_AadTenants** wordt opgeslagen als een door komma's gescheiden lijst met tenant-domeinen (de 'Tenants toegestaan' velden in de Mobile Services-portal).

> [!WARNING]
> **Gebruik niet de verificatiemechanismen in het menu instellingen**
>
> Azure App Service biedt een afzonderlijke 'zonder code' verificatie en autorisatie onder de *verificatie / autorisatie* menu instellingen en de (afgeschaft) *mobiele verificatie* optie Klik in het menu instellingen.  Deze opties zijn niet compatibel met een gemigreerde Azure Mobile Service.  U kunt [upgrade van uw site](app-service-mobile-net-upgrading-from-mobile-services.md) om te profiteren van de Azure App Service-verificatie.
>
>

### <a name="easytables"></a>Gegevens
De *gegevens* tabblad in Mobile Services is vervangen door *gemakkelijk tabellen* binnen de Azure-portal.  Toegang krijgen tot gemakkelijk tabellen:

1. Meld u aan bij [Azure-portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van de gemigreerde mobiele Service.
3. Standaard wordt de blade instellingen geopend.
4. Klik op **gemakkelijk tabellen** in het menu mobiele.

U kunt een tabel toevoegen door te klikken op de **toevoegen** knop of de bestaande tabellen openen door te klikken op de naam van een tabel.  Er zijn verschillende bewerkingen u vanaf deze blade doen kunt, met inbegrip van:

* Tabelmachtigingen wijzigen
* De operationele scripts bewerken
* Het tabelschema beheren
* De tabel verwijderen
* De inhoud van de tabel wissen
* Specifieke rijen van de tabel verwijderen

### <a name="easyapis"></a>API
De *API* tabblad in Mobile Services is vervangen door *eenvoudige API's* binnen de Azure-portal.  Toegang krijgen tot een eenvoudige API's:

1. Meld u aan bij [Azure-portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van de gemigreerde mobiele Service.
3. Standaard wordt de blade instellingen geopend.
4. Klik op **eenvoudige API's** in het menu mobiele.

Uw gemigreerde API's staan al in de blade.  U kunt ook een API toevoegen van deze blade.  Klik op de API voor het beheren van een specifieke API.
De nieuwe blade kunt u de machtigingen aanpassen en het bewerken van de scripts voor de API.

### <a name="on-demand-jobs"></a>Scheduler-taken
Alle scheduler-taken zijn beschikbaar via de Scheduler-Taakverzamelingen-sectie.  Toegang krijgen tot uw scheduler-taken:

1. Meld u aan bij [Azure-portal].
2. Selecteer **Bladeren >**, voer **planning** in de *Filter* vak en selecteer vervolgens **Scheduler verzamelingen**.
3. Selecteer de verzameling van de taak voor uw site.  Dit sjabloon heet *sitename*-taken.
4. Klik op **instellingen**.
5. Klik op **Scheduler-taken** onder beheren.

Geplande taken worden weergegeven met de frequentie die u hebt opgegeven voor de migratie.  Op aanvraag taken zijn uitgeschakeld.  Een taak op aanvraag uitvoeren:

1. Selecteer de taak die u wilt uitvoeren.
2. Klik indien nodig op **inschakelen** zodat de taak.
3. Klik op **instellingen**, klikt u vervolgens **planning**.
4. Selecteer een herhaling van **eenmaal**, klikt u vervolgens op **opslaan**

Uw taken op aanvraag bevinden zich in `App_Data/config/scripts/scheduler post-migration`.  Het is raadzaam dat u alle taken voor op aanvraag naar [WebJobs] converteren of [functies].  Schrijven van nieuwe scheduler-taken als [WebJobs] of [functies].

### <a name="notification-hubs"></a>Notification Hubs
Mobile Services maakt gebruik van Notification Hubs voor pushmeldingen.  De volgende App-instellingen worden gebruikt voor het koppelen van de Notification Hub met uw mobiele Service na de migratie:

| Toepassingsinstelling | Beschrijving |
|:--- |:--- |
| **MS\_PushEntityNamespace** |De Notification Hub-Namespace |
| **MS\_NotificationHubName** |De naam van de Notification Hub |
| **MS\_NotificationHubConnectionString** |De verbindingsreeks van de Notification Hub |
| **MS\_NamespaceName** |Een alias voor MS_PushEntityNamespace |

Uw Notification Hub wordt beheerd via de [Azure-portal].  Houd rekening met de naam van de Notification Hub (u kunt dit vinden met behulp van de App-instellingen):

1. Meld u aan bij [Azure-portal].
2. Selecteer **Bladeren**>, selecteer daarna **Notification Hubs**
3. Klik op de naam van de Notification Hub die zijn gekoppeld aan de mobiele service.

> [!NOTE]
> Als uw Notification HUb is een type 'Mixed', is het niet zichtbaar.  "Gemengde" Typ notification hubs gebruikmaken van zowel Notification Hubs en verouderde Service Bus-functies.  [Converteren van de gemengde naamruimten] voordat u doorgaat.  Zodra de conversie voltooid is, uw notification hub wordt weergegeven in de [Azure-portal].
>
>

Raadpleeg voor meer informatie de [Notification Hubs] documentatie.

> [!TIP]
> Notification Hubs-beheerfuncties de [Azure-portal] zijn nog steeds in preview.  De [klassieke Azure-Portal] blijft beschikbaar voor het beheren van uw Notification Hubs.
>
>

### <a name="legacy-push"></a>Verouderde Push-instellingen
Als u Push geconfigureerd op uw mobiele service vóór de introductie van Notification Hubs, gebruikt u *verouderde push*.  Als u met behulp van Push en een Notification Hub die worden vermeld in de configuratie niet wordt weergegeven, dan is het waarschijnlijk u *verouderde push*.  Deze functie wordt gemigreerd met de functies.  We raden echter aan om te upgraden naar Notification Hubs kort nadat de migratie voltooid is.

De verouderde push-instellingen (met opmerkelijke uitzondering van het APNS-certificaat) zijn in de tussentijd beschikbaar in App-instellingen.  Het APNS-certificaat bijwerken door het vervangen van het juiste bestand op het bestandssysteem.

### <a name="app-settings"></a>Andere Appinstellingen
De volgende aanvullende appinstellingen zijn gemigreerd van uw mobiele Service en onder *instellingen* > *Appinstellingen*:

| Toepassingsinstelling | Beschrijving |
|:--- |:--- |
| **MS\_MobileServiceName** |De naam van uw app |
| **MS\_MobileServiceDomainSuffix** |Het voorvoegsel van het domein. Internet Explorer Azure-mobile.net |
| **MS\_ApplicationKey** |De sleutel van uw toepassing |
| **MS\_hoofdsleutel** |De hoofdsleutel van uw app |

De Toepassingssleutel en de hoofdsleutel zijn identiek aan de sleutels van de toepassing van uw oorspronkelijke mobiele Service.  In het bijzonder wordt de sleutel van de toepassing verzonden door mobiele clients voor het valideren van het gebruik van de mobiele API.

### <a name="cliequivalents"></a>Opdrachtregelopdrachten
U kunt meer de *azure mobiele* opdracht voor het beheren van uw Azure Mobile Services-site.  In plaats daarvan veel functies zijn vervangen door de *azure site* opdracht.  Gebruik de volgende tabel equivalenten vinden voor gewone opdrachten:

| *Azure Mobile* opdracht | Gelijkwaardige *Azure Site* opdracht |
|:--- |:--- |
| mobiele locaties |locatie van sitelijst |
| lijst met mobiele |lijst met de site |
| mobiele weergeven *naam* |site weergeven *naam* |
| mobiele herstart *naam* |opnieuw opstarten site *naam* |
| mobiele opnieuw distribueren *naam* |site-implementatie opnieuw distribueren *commitId* *naam* |
| mobiele sleutelset *naam* *type* *waarde* |site appsetting verwijderen *sleutel* *naam* <br/> toevoegen van de site appsetting *sleutel*=*waarde* *naam* |
| lijst met mobiele config *naam* |sitelijst voor appsetting *naam* |
| mobiele config ophalen *naam* *sleutel* |site van appsetting weergeven *sleutel* *naam* |
| mobiele configuratieset *naam* *sleutel* |site appsetting verwijderen *sleutel* *naam* <br/> toevoegen van de site appsetting *sleutel*=*waarde* *naam* |
| lijst met mobiele domein *naam* |sitelijst voor domein *naam* |
| mobiele domein toevoegen *naam* *domein* |site-domein toevoegen *domein* *naam* |
| mobiele domein verwijderen *naam* |site domein verwijderen *domein* *naam* |
| mobiele scale weergeven *naam* |site weergeven *naam* |
| mobiele schaal wijzigen *naam* |schaal sitemodus *modus* *naam* <br /> site-exemplaren van de schaal *exemplaren* *naam* |
| lijst met mobiele appsetting *naam* |sitelijst voor appsetting *naam* |
| mobiele appsetting toevoegen *naam* *sleutel* *waarde* |toevoegen van de site appsetting *sleutel*=*waarde* *naam* |
| mobiele appsetting verwijderen *naam* *sleutel* |site appsetting verwijderen *sleutel* *naam* |
| mobiele appsetting weergeven *naam* *sleutel* |site appsetting verwijderen *sleutel* *naam* |

Verificatie of push notification-instellingen bijwerken door de juiste instelling voor de toepassing bij te werken.
Bestanden bewerken en de site via de FTP- of git publiceert.

### <a name="diagnostics"></a>Diagnostische gegevens en logboekregistratie
Diagnostische logboekregistratie is gewoonlijk in een Azure App Service uitgeschakeld.  Diagnostische logboekregistratie inschakelen:

1. Meld u aan bij [Azure-portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van de gemigreerde mobiele Service.
3. Standaard wordt de blade instellingen geopend.
4. Selecteer **diagnostische logboeken** in het menu functies.
5. Klik op **ON** voor de volgende Logboeken: **toepassingslogboeken (bestandssysteem)**, **gedetailleerde foutberichten**, en **tracering van mislukte aanvragen**
6. Klik op **bestandssysteem** voor logboekregistratie van webserver
7. Klik op **opslaan**

De logboeken weergeven:

1. Meld u aan bij [Azure-portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van de gemigreerde mobiele Service.
3. Klik op de **extra** knop
4. Selecteer **Logboekstream** in het menu OBSERVE.

Logboeken worden weergegeven in het venster nadat ze zijn gegenereerd.  U kunt ook de logboeken voor latere analyse met behulp van de implementatiereferenties van uw downloaden. Zie voor meer informatie de [logboekregistratie] documentatie.

## <a name="known-issues"></a>Bekende problemen
### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>Verwijderen van een kloon mobiele App gemigreerd zorgt ervoor dat een onderbreking van deze site
Als u uw gemigreerde mobiele service met Azure PowerShell klonen en vervolgens de kloon verwijderen, wordt de DNS-vermelding voor uw productieservice verwijderd.  Uw site is niet langer toegankelijk zijn vanaf Internet.  

Oplossing: Als u klonen van uw site wilt, doen via de portal.

### <a name="changing-webconfig-does-not-work"></a>Het wijzigen van Web.config werkt niet
Als u een ASP.NET-website hebt, wijzigingen in de `Web.config` kan bestand niet toegepast.  De Azure App Service bouwt geschikte `Web.config` bestand tijdens het opstarten voor de ondersteuning van de Mobile Services-runtime.  U kunt bepaalde instellingen (zoals aangepaste headers) met behulp van een XML-transformatiebestand overschrijven.  Maak een bestand in aangeroepen `applicationHost.xdt` -dit bestand moet terechtkomen op de `D:\home\site` map op de Azure-Service.  Upload de `applicationHost.xdt` bestand via een aangepaste implementatie scripts of rechtstreeks met behulp van Kudu.  Hier volgt een voorbeelddocument:

```
<?xml version="1.0" encoding="utf-8"?>
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform">
  <system.webServer>
    <httpProtocol>
      <customHeaders>
        <add name="X-Frame-Options" value="DENY" xdt:Transform="Replace" />
        <remove name="X-Powered-By" xdt:Transform="Insert" />
      </customHeaders>
    </httpProtocol>
    <security>
      <requestFiltering removeServerHeader="true" xdt:Transform="SetAttributes(removeServerHeader)" />
    </security>
  </system.webServer>
</configuration>
```

Zie voor meer informatie de [XDT transformeren voorbeelden] documentatie op GitHub.

### <a name="migrated-mobile-services-cannot-be-added-to-traffic-manager"></a>Gemigreerde Mobile Services kan niet worden toegevoegd aan Traffic Manager
Wanneer u een Traffic Manager-profiel maakt, kunt u niet rechtstreeks een gemigreerde mobiele service aan het profiel kiezen.  Gebruik een 'Extern eindpunt'.  Het externe eindpunt kan alleen worden toegevoegd via PowerShell.  Zie voor meer informatie de [Traffic Manager-zelfstudie](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/).

## <a name="next-steps"></a>De volgende stappen
Nu dat uw toepassing is gemigreerd naar App Service, zijn er nog meer functies die u kunt gebruiken:

* Implementatie [staging-sleuven] kunt u de fase van de wijzigingen in uw site en voert u A / B-tests.
* [WebJobs] bieden een vervanging voor On-demand geplande taken.
* U kunt [continu implementeren] uw site door te GitHub, TFS of volgt koppelen aan uw site.
* U kunt [Application Insights] voor het bewaken van uw site.
* Een website en een mobiele-API van de dezelfde code fungeren.

### <a name="upgrading-your-site"></a>Uw Mobile Services-site upgraden naar Azure Mobile Apps SDK
* Voor de server op basis van een Node.js-projecten, de nieuwe [Mobile Apps Node.js SDK] biedt verschillende nieuwe functies. Bijvoorbeeld: u kunt nu doen lokale ontwikkeling en foutopsporing, een Node.js-versie hoger 0.10 en aanpassen met een middleware Express.js.
* Voor. NET gebaseerde serverprojecten, de nieuwe [Mobile Apps SDK NuGet-pakketten](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) meer flexibiliteit in NuGet afhankelijkheden hebben.  Deze pakketten ondersteuning voor de nieuwe App Service-verificatie en samenstellen met een ASP.NET-project. Zie voor meer informatie over het upgraden, [uw bestaande Mobile Service voor .NET bijwerken naar App Service](app-service-mobile-net-upgrading-from-mobile-services.md).

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[App Service-prijzen]: https://azure.microsoft.com/en-us/pricing/details/app-service/
[Application Insights]: ../application-insights/app-insights-overview.md
[Automatische schaling]: ../app-service/web-sites-scale.md
[Azure App Service]: ../app-service/app-service-web-overview.md
[klassieke Azure-Portal]: https://manage.windowsazure.com
[Azure-portal]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/en-us/regions/
[Azure Scheduler plannen]: ../scheduler/scheduler-plans-billing.md
[continu implementeren]: ../app-service/app-service-continuous-deployment.md
[Converteren van de gemengde naamruimten]: https://azure.microsoft.com/en-us/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: http://curl.haxx.se/
[aangepaste domeinnamen]: ../app-service/app-service-web-tutorial-custom-domain.md
[Fiddler]: http://www.telerik.com/fiddler
[algemene beschikbaarheid van Azure App Service]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[Hybrid Connections]: ../app-service/app-service-hybrid-connections.md
[logboekregistratie]: ../app-service/web-sites-enable-diagnostic-log.md
[Mobile Apps Node.js SDK]: https://github.com/azure/azure-mobile-apps-node
[tegenover Mobile Services. App Service]: app-service-mobile-value-prop-migration-from-mobile-services.md
[Notification Hubs]: ../notification-hubs/notification-hubs-push-notification-overview.md
[prestatiebewaking]: ../app-service/web-sites-monitor.md
[Postman]: http://www.getpostman.com/
[staging-sleuven]: ../app-service/web-sites-staged-publishing.md
[VNet]: ../app-service/web-sites-integrate-with-vnet.md
[XDT transformeren voorbeelden]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[functies]: ../azure-functions/functions-overview.md
