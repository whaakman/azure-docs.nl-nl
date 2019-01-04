---
title: Migreren vanuit Mobile Services naar een mobiele App van App Service
description: Leer hoe u eenvoudig uw Mobile Services-toepassing migreren voor een App Service Mobile App
services: app-service\mobile
documentationcenter: ''
author: conceptdev
manager: crdun
editor: ''
ms.assetid: 07507ea2-690f-4f79-8776-3375e2adeb9e
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: crdun
ms.openlocfilehash: 17fbaa7efcdd9e0de675defb3958a61f29bbc3fe
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53714318"
---
# <a name="article-top"></a>Uw bestaande Azure Mobile Service migreren naar Azure App Service
Met de [algemene beschikbaarheid van Azure App Service], Azure Mobile Services-sites kunnen eenvoudig worden gemigreerd in-place om te profiteren van alle functies van Azure App Service.  Dit document wordt uitgelegd wat u kunt verwachten wanneer uw site van Azure Mobile Services migreren naar Azure App Service.

## <a name="what-does-migration-do"></a>Wat doet migratie naar uw site
Hiermee schakelt u de migratie van uw Azure Mobile Service uw Mobile Service in een [Azure App Service] app zonder de code.  Uw Notification Hubs, SQL-gegevensverbinding verificatie-instellingen, geplande taken en domeinnaam blijven ongewijzigd.  Mobiele clients met behulp van uw Azure Mobile Service blijven normaal functioneren.  Migratie wordt uw service opnieuw opgestart nadat deze zijn overgebracht naar Azure App Service.

[!INCLUDE [app-service-mobile-migrate-vs-upgrade](../../includes/app-service-mobile-migrate-vs-upgrade.md)]

## <a name="why-migrate"></a>Waarom moet u uw site migreren
Microsoft wordt aangeraden dat u Azure Mobile Service om te profiteren van de functies van Azure Appservice, inclusief migreren:

* Nieuwe hostfuncties, waaronder [WebJobs] en [aangepaste domeinnamen].
* Bewaken en problemen oplossen met [Application Insights].
* Ingebouwde DevOps-hulpprogramma's, met inbegrip van [faseringssleuven], terugdraai- en in productie testen.
* [Automatisch schalen], taakverdeling, en [Bewaking van toepassingsprestaties].

Zie voor meer informatie over de voordelen van Azure App Service, de [Mobile Services versus App Service] onderwerp.

## <a name="before-you-begin"></a>Voordat u begint
Voordat u begint met een grote werk op uw site, moet u back-up van uw Mobile Service-scripts en SQL-database.

## <a name="migrating-site"></a>Migreren van uw sites
Het migratieproces worden alle sites binnen één Azure-regio gemigreerd.

Voor het migreren van uw site:

1. Meld u aan bij de [klassieke Azure-Portal].
2. Selecteer een mobiele Service in de regio die u wilt migreren.
3. Klik op de **migreren naar App Service** knop.

   ![De knop migreren][0]
4. Lees de migreren naar het dialoogvenster App Service.
5. Voer de naam van uw Mobile Service in het vak.  Bijvoorbeeld, als uw domeinnaam contoso.azure mobile.net is, voert u *contoso* in het vak.
6. Klik op de knop tikken.

De status van de migratie in de monitor voor de activiteit controleren. Uw site wordt vermeld als *migreren* in de klassieke Azure-Portal.

  ![Migratie van activiteiten controleren][1]

Elke migratie kan duren vanaf 3 en 15 minuten per mobiele service die wordt gemigreerd.  Uw site blijven beschikbaar tijdens de migratie.
Uw site wordt opnieuw opgestart aan het einde van het migratieproces.  De site is niet beschikbaar tijdens het opstarten, die een paar seconden duurt.

## <a name="finalizing-migration"></a>De migratie voltooien
Plan voor het testen van uw site van een mobiele client aan het einde van het migratieproces.  Zorg ervoor dat u alle Clientacties in de algemene zonder wijzigingen in de mobiele client kunt uitvoeren.  

### <a name="update-app-service-tier"></a>Selecteer een geschikte App Service-prijscategorie
Hebt u meer flexibiliteit in na de migratie naar Azure App Service-prijzen.

1. Meld u aan bij [Azure Portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van uw gemigreerde mobiele Service.
3. De blade instellingen wordt standaard geopend.
4. Klik op **App Service-Plan** in het menu instellingen.
5. Klik op de **prijscategorie** tegel.
6. Klik op de tegel die geschikt is voor uw vereisten en klik vervolgens op **Selecteer**.  U moet mogelijk klikken **weergeven van alle** om te zien van de beschikbare Prijscategorieën.

Als uitgangspunt raden we aan de volgende lagen:

| Mobile Service-prijscategorie | App Service-prijscategorie |
|:--- |:--- |
| Gratis |F1 Free |
| Basic |Basic B1 |
| Standard |S1 Standard |

Er is veel flexibiliteit bij het kiezen van het recht voor prijscategorie voor uw toepassing.  Raadpleeg [prijzen van App Service] voor volledige informatie over de prijzen van uw nieuwe App Service.

> [!TIP]
> De standaardlaag van App Service bevat toegang tot veel functies die u gebruiken wilt mogelijk, met inbegrip van [faseringssleuven], automatische back-ups en automatisch schalen.  Bekijk de nieuwe mogelijkheden terwijl u er bent!
>
>

### <a name="review-migration-scheduler-jobs"></a>Bekijk de gemigreerde Scheduler-taken
Scheduler-taken worden niet meer zichtbaar tot ongeveer 30 minuten na de migratie.  Geplande taken toch op de achtergrond worden uitgevoerd.
Uw geplande taken weergeven nadat ze opnieuw zichtbaar zijn:

1. Meld u aan bij [Azure Portal].
2. Selecteer **Bladeren >**, voer **planning** in de *Filter* vak en selecteer vervolgens **Scheduler verzamelingen**.

Er zijn een beperkt aantal free-scheduler-taken beschikbaar na de migratie.  Bekijk uw gebruik en de [Abonnementen voor Azure Scheduler].

### <a name="configure-cors"></a>CORS configureren, indien nodig
Cross-origin-resource delen is een techniek waarmee een website voor toegang tot een Web-API op een ander domein.  Als u van Azure Mobile Services met een koppeling naar een website gebruikmaakt, moet u CORS configureren als onderdeel van de migratie.  Als u Azure Mobile Services toegang hebben tot uitsluitend van mobiele apparaten, klikt u vervolgens hoeft CORS niet te worden geconfigureerd, behalve in uitzonderlijke gevallen.

De gemigreerde CORS-instellingen zijn beschikbaar als de **MS_CrossDomainWhitelist** App-instelling.  Voor het migreren van uw site aan de CORS voor App Service:

1. Meld u aan bij [Azure Portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van uw gemigreerde mobiele Service.
3. De blade instellingen wordt standaard geopend.
4. Klik op **CORS** in het menu van de API.
5. Voer alle oorsprongen toegestaan in het vak dat is opgegeven, drukt u op Enter na elke opdracht.
6. Nadat uw lijst met toegestane oorsprongen juist is, klikt u op de knop opslaan.

> [!TIP]
> Een van de voordelen van het gebruik van een Azure App Service is dat u uw website en de mobiele service op dezelfde site uitvoeren kunt.  Zie voor meer informatie de [Vervolgstappen](#next-steps) sectie.
>
>

### <a name="download-publish-profile"></a>Een nieuwe publicatieprofiel downloaden
Het publicatieprofiel van uw site wordt gewijzigd wanneer u migreert naar Azure App Service.  Als u van plan bent voor het publiceren van uw site bij vanuit Visual Studio, moet u een nieuw profiel voor publiceren.  Het nieuwe publicatieprofiel downloaden:

1. Meld u aan bij [Azure Portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van uw gemigreerde mobiele Service.
3. Klik op **publicatieprofiel ophalen**.

Het bestand met publicatie-instellingen is gedownload naar uw computer.  Normaal gesproken wordt aangeroepen *sitename*. Met publicatie-instellingen.  De publicatie-instellingen importeren in een bestaand project:

1. Open Visual Studio en uw Azure Mobile Service-project.
2. Met de rechtermuisknop op uw project in de **Solution Explorer** en selecteer **publiceren...**
3. Klik op **Importeren**.
4. Klik op **Bladeren** en selecteer uw gedownloade publish settings-bestand.  Klik op **OK**
5. Klik op **verbinding valideren** om te controleren of het werk van de instellingen voor publiceren.
6. Klik op **publiceren** voor het publiceren van uw site.

## <a name="working-with-your-site"></a>Werken met uw na de sitemigratie
Beginnen met werken met uw nieuwe App Service in de [Azure Portal] na de migratie.  Hieronder vindt u enkele opmerkingen bij de specifieke bewerkingen die u hebt gebruikt om uit te voeren de [Klassieke Azure-portal], samen met hun App Service-equivalent.

### <a name="publishing-your-site"></a>Downloaden en publiceren van uw gemigreerde site
Uw site is beschikbaar via git of FTP- en kan opnieuw worden gepubliceerd met diverse verschillende mechanismen, met inbegrip van WebDeploy, TFS, GitHub, Mercurial en FTP.  Referenties voor de implementatie worden gemigreerd met de rest van uw site.  Als u de referenties voor implementatie niet ingesteld of u ze niet meer weet, kunt u ze herstellen:

1. Meld u aan bij [Azure Portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van uw gemigreerde mobiele Service.
3. De blade instellingen wordt standaard geopend.
4. Klik op **implementatiereferenties** in de publicatie menu.
5. Geef de nieuwe referenties voor implementatie in de desbetreffende vakken en klik vervolgens op de knop opslaan.

U kunt deze referenties gebruiken om te klonen van de site met git of instellen van geautomatiseerde implementaties vanuit GitHub, TFS of Mercurial.  Zie de [Azure App Service-Implementatiedocumentatie] voor meer informatie.

### <a name="appsettings"></a>Toepassingsinstellingen
De meeste instellingen voor een gemigreerde mobiele service zijn beschikbaar via de App-instellingen.  Krijgt u een lijst van de appinstellingen van de [Azure Portal].
Weergeven of wijzigen van uw app-instellingen:

1. Meld u aan bij [Azure Portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van uw gemigreerde mobiele Service.
3. De blade instellingen wordt standaard geopend.
4. Klik op **toepassingsinstellingen** in het menu Algemeen.
5. Blader naar het gedeelte instellingen van de App en uw app-instelling te vinden.
6. Klik op de waarde van de app-instelling om de waarde te bewerken.  Klik op **opslaan** om op te slaan de waarde.

U kunt meerdere app-instellingen bijwerken op hetzelfde moment.

> [!TIP]
> Er zijn twee toepassingsinstellingen in met dezelfde waarde.  Bijvoorbeeld, ziet u mogelijk *ApplicationKey* en *MS\_ApplicationKey*.  Beide toepassingsinstellingen op hetzelfde moment bijwerken.
>
>

### <a name="authentication"></a>Verificatie
Alle verificatie-instellingen zijn beschikbaar als App-instellingen in uw gemigreerde site.  Als u wilt uw verificatie-instellingen bijwerken, moet u de juiste app-instellingen wijzigen.  De volgende tabel ziet u de juiste app-instellingen voor uw verificatieprovider:

| Provider | Client-id | Clientgeheim | Overige instellingen |
|:--- |:--- |:--- |:--- |
| Microsoft-account |**MS\_MicrosoftClientID** |**MS\_MicrosoftClientSecret** |**MS\_MicrosoftPackageSID** |
| Facebook |**MS\_FacebookAppID** |**MS\_FacebookAppSecret** | |
| Twitter |**MS\_TwitterConsumerKey** |**MS\_TwitterConsumerSecret** | |
| Google |**MS\_GoogleClientID** |**MS\_GoogleClientSecret** | |
| Azure AD |**MS\_AadClientID** | |**MS\_AadTenants** |

Opmerking: **MS\_AadTenants** wordt opgeslagen als een door komma's gescheiden lijst domeinen van de tenant (de 'Tenants toegestaan' velden in de portal voor Mobile Services).

> [!WARNING]
> **Gebruik niet de verificatiemechanismen in het menu instellingen**
>
> Azure App Service biedt een afzonderlijk 'zonder code' verificatie en autorisatie systeem onder de *verificatie / autorisatie* menu instellingen en de (afgeschaft) *mobiele verificatie* optie Klik in het menu instellingen.  Deze opties zijn niet compatibel met een gemigreerde Azure mobiele Service.  U kunt [upgrade van uw site](app-service-mobile-net-upgrading-from-mobile-services.md) om te profiteren van de Azure App Service-verificatie.
>
>

### <a name="easytables"></a>Data
De *gegevens* tabblad in Mobile Services is vervangen door *eenvoudige tabellen* in Azure portal.  Voor toegang tot eenvoudige tabellen:

1. Meld u aan bij [Azure Portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van uw gemigreerde mobiele Service.
3. De blade instellingen wordt standaard geopend.
4. Klik op **eenvoudige tabellen** in het menu van het mobiele.

U kunt een tabel toevoegen door te klikken op de **toevoegen** knop of toegang krijgen tot uw bestaande tabellen door te klikken op een tabelnaam wordt opgegeven.  Er zijn verschillende bewerkingen u vanaf deze blade doen kunt, met inbegrip van:

* Wijzigen van de tabelmachtigingen
* De operationele scripts bewerken
* Het tabelschema beheren
* De tabel verwijderen
* De inhoud van de tabel wissen
* Specifieke rijen van de tabel verwijderen

### <a name="easyapis"></a>API
De *API* tabblad in Mobile Services is vervangen door *eenvoudige API's* in Azure portal.  Voor toegang tot eenvoudige API's:

1. Meld u aan bij [Azure Portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van uw gemigreerde mobiele Service.
3. De blade instellingen wordt standaard geopend.
4. Klik op **eenvoudige API's** in het menu van het mobiele.

Uw gemigreerde API's staan al in de blade.  U kunt ook een API toevoegen vanaf deze blade.  Klik op de API voor het beheren van een bepaalde API.
In de nieuwe blade kunt u de machtigingen aanpassen en bewerken van de scripts voor de API.

### <a name="on-demand-jobs"></a>Scheduler-taken
Alle scheduler-taken zijn beschikbaar via de Scheduler-Taakverzamelingen-sectie.  Voor toegang tot de scheduler-taken:

1. Meld u aan bij [Azure Portal].
2. Selecteer **Bladeren >**, voer **planning** in de *Filter* vak en selecteer vervolgens **Scheduler verzamelingen**.
3. Selecteer de Taakverzameling voor uw site.  De naam ervan is *sitename*-taken.
4. Klik op **Instellingen**.
5. Klik op **Scheduler-taken** onder beheren.

Geplande taken worden weergegeven met de frequentie die u hebt opgegeven voor de migratie.  On-demand-taken zijn uitgeschakeld.  Een taak op aanvraag uitvoeren:

1. Selecteer de taak die u wilt uitvoeren.
2. Klik indien nodig, op **inschakelen** om in te schakelen van de taak.
3. Klik op **instellingen**, klikt u vervolgens **planning**.
4. Selecteer een terugkeerpatroon van de **eenmaal**, klikt u vervolgens op **opslaan**

Uw taken op aanvraag bevinden zich in `App_Data/config/scripts/scheduler post-migration`.  Het is raadzaam dat u alle on-demand taken [webjobs] converteren of [functies].  Schrijven van nieuwe scheduler-taken als [WebJobs] of [functies].

### <a name="notification-hubs"></a>Notification Hubs
Mobile Services maakt gebruik van Notification Hubs voor pushmeldingen.  De volgende App-instellingen worden gebruikt voor de Notification Hub koppelen aan uw mobiele Service na de migratie:

| Toepassingsinstelling | Description |
|:--- |:--- |
| **MS\_PushEntityNamespace** |De Notification Hub-Namespace |
| **MS\_NotificationHubName** |De naam van de Notification Hub |
| **MS\_NotificationHubConnectionString** |De Notification Hub-verbindingsreeks |
| **MS\_NamespaceName** |Een alias voor MS_PushEntityNamespace |

Uw Notification Hub wordt beheerd via de [Azure Portal].  Houd rekening met de naam van de Notification Hub (u kunt dit vinden met behulp van de App-instellingen):

1. Meld u aan bij [Azure Portal].
2. Selecteer **Bladeren**> en selecteer vervolgens **Notification Hubs**
3. Klik op de naam van de Notification Hub die is gekoppeld aan de mobiele service.

> [!NOTE]
> Als uw Notification HUb is een type "Gemengde", is het niet zichtbaar.  "Gemengde" Typ notification hubs gebruikmaken van zowel Notification Hubs en verouderde Service Bus-functies.  [Uw gemengde naamruimten converteren] voordat u doorgaat.  Zodra de conversie voltooid is, de notification hub wordt weergegeven in de [Azure Portal].
>
>

Raadpleeg voor meer informatie de [Notification Hubs] documentatie.

> [!TIP]
> Notification Hubs-beheerfuncties de [Azure Portal] zijn nog steeds in preview.  De [Klassieke Azure-portal] blijft beschikbaar voor het beheren van uw Notification Hubs.
>
>

### <a name="legacy-push"></a>Verouderde Pushinstellingen
Als u Pushmeldingen op uw mobiele service vóór de introductie van Notification Hubs hebt geconfigureerd, gebruikt u *verouderde push*.  Als u met behulp van Push en een Notification Hub die worden vermeld in de configuratie niet wordt weergegeven, dan is het waarschijnlijk u *verouderde push*.  Deze functie is gemigreerd met de functies.  We raden echter aan dat u een upgrade naar Notification Hubs uitvoert zodra de migratie voltooid is.

In de tussentijd zijn alle verouderde pushinstellingen (met de uitzondering die aandacht vereist van het APNS-certificaat) beschikbaar in Appinstellingen.  Het APNS-certificaat bijwerken door het vervangen van het juiste bestand op het bestandssysteem.

### <a name="app-settings"></a>Andere App-instellingen
De volgende extra app-instellingen zijn gemigreerd vanuit uw mobiele Service en beschikbaar zijn onder *instellingen* > *App-instellingen*:

| Toepassingsinstelling | Description |
|:--- |:--- |
| **MS\_MobileServiceName** |De naam van uw app |
| **MS\_MobileServiceDomainSuffix** |Het domeinvoorvoegsel. Internet Explorer azure-mobile.net |
| **MS\_ApplicationKey** |De sleutel van uw toepassing |
| **MS\_MasterKey** |De hoofdsleutel van uw app |

De Toepassingssleutel en de hoofdsleutel zijn identiek aan de sleutels van de toepassing van uw oorspronkelijke mobiele Service.  In het bijzonder wordt de Toepassingssleutel verzonden door mobiele clients voor het valideren van het gebruik van de mobiele-API.

### <a name="cliequivalents"></a>Opdrachtregelopdrachten
U kunt meer de *azure mobiele* opdracht voor het beheren van uw Azure Mobile Services-site.  In plaats daarvan veel functies zijn vervangen door de *azure site* opdracht.  Gebruik de volgende tabel om te zoeken equivalenten voor veelgebruikte opdrachten:

| *Azure Mobile* opdracht | Gelijkwaardige *Azure Site* opdracht |
|:--- |:--- |
| mobiele locaties |locatie van sitelijst |
| lijst met mobiele |lijst met de site |
| mobiele show *naam* |site-show *naam* |
| mobiele opnieuw *naam* |opnieuw opstarten van site *naam* |
| mobiele opnieuw implementeren *naam* |implementatie opnieuw implementeren van site *commitId* *naam* |
| mobiele sleutelset *naam* *type* *waarde* |site-instelling verwijderen *sleutel* *naam* <br/> site-instelling toevoegen *sleutel*=*waarde* *naam* |
| lijst met mobiele config *naam* |sitelijst voor appsetting *naam* |
| ophalen van de mobiele configuratie *naam* *sleutel* |site-instelling show *sleutel* *naam* |
| mobiele configuratieset *naam* *sleutel* |site-instelling verwijderen *sleutel* *naam* <br/> site-instelling toevoegen *sleutel*=*waarde* *naam* |
| lijst met mobiele domein *naam* |lijst met het domein van de site *naam* |
| mobiele-domein toevoegen *naam* *domein* |het domein toevoegen *domein* *naam* |
| mobiele domein verwijderen *naam* |site-domein verwijderen *domein* *naam* |
| mobiele schaal show *naam* |site-show *naam* |
| mobiele schaal wijzigen *naam* |site-schaalmodus *modus* *naam* <br /> site-schalingsinstanties *exemplaren* *naam* |
| lijst met mobiele appsetting *naam* |sitelijst voor appsetting *naam* |
| mobiele appsetting toevoegen *naam* *sleutel* *waarde* |site-instelling toevoegen *sleutel*=*waarde* *naam* |
| verwijderen van mobiele appsetting *naam* *sleutel* |site-instelling verwijderen *sleutel* *naam* |
| mobiele appsetting show *naam* *sleutel* |site-instelling verwijderen *sleutel* *naam* |

Verificatie of push-meldingsinstellingen bijwerken door de juiste instelling van de toepassing bij te werken.
Bestanden bewerken en publiceer uw website via ftp of git.

### <a name="diagnostics"></a>Diagnostische gegevens en logboekregistratie
Registratie in diagnoselogboek is gewoonlijk in een Azure App Service uitgeschakeld.  Diagnostische logboekregistratie inschakelen:

1. Meld u aan bij [Azure Portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van uw gemigreerde mobiele Service.
3. De blade instellingen wordt standaard geopend.
4. Selecteer **diagnostische logboeken** onder het menu functies.
5. Klik op **ON** voor de volgende Logboeken: **Toepassingslogboeken (bestandssysteem)**, **gedetailleerde foutberichten**, en **tracering van mislukte aanvragen**
6. Klik op **bestandssysteem** voor Web server-logboekregistratie
7. Klik op **Opslaan**.

De logboeken weergeven:

1. Meld u aan bij [Azure Portal].
2. Selecteer **alle resources** of **App Services** klik vervolgens op de naam van uw gemigreerde mobiele Service.
3. Klik op de **extra** knop
4. Selecteer **Log Stream** onder het menu OBSERVE.

Logboeken worden weergegeven in het venster als ze worden gegenereerd.  U kunt ook de logboeken voor latere analyse met behulp van de referenties voor implementatie downloaden. Zie voor meer informatie de [logboekregistratie] documentatie.

## <a name="known-issues"></a>Bekende problemen
### <a name="deleting-a-migrated-mobile-app-clone-causes-a-site-outage"></a>Verwijderen van een kloon Mobile App gemigreerd zorgt ervoor dat de uitval
Als u uw gemigreerde mobiele service met behulp van Azure PowerShell klonen en vervolgens de kloon verwijderen, wordt de DNS-vermelding voor uw productieservice verwijderd.  Uw site is niet meer toegankelijk zijn vanaf Internet.  

Oplossing: Als u klonen van uw site wilt, doen via de portal.

### <a name="changing-webconfig-does-not-work"></a>Wijzigen van Web.config werkt niet
Als u een ASP.NET-website hebt, wijzigingen in de `Web.config` bestand kan niet worden toegepast.  De Azure App Service bouwt een geschikte `Web.config` bestand tijdens het opstarten voor de ondersteuning van de Mobile Services-runtime.  U kunt bepaalde instellingen (zoals aangepaste kopteksten) met behulp van een XML-transformatiebestand overschrijven.  Maak een bestand in met de naam `applicationHost.xdt` -dit bestand moet terechtkomen de `D:\home\site` map op de Azure-Service.  Upload de `applicationHost.xdt` bestand via een aangepaste implementatiescript of rechtstreeks met Kudu.  Hieronder ziet u een voorbeelddocument:

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

Zie voor meer informatie de [Voorbeelden van XDT transformeren] documentatie op GitHub.

### <a name="migrated-mobile-services-cannot-be-added-to-traffic-manager"></a>Gemigreerde Mobile Services kan niet worden toegevoegd aan Traffic Manager
Wanneer u een Traffic Manager-profiel maakt, kunt u niet rechtstreeks een gemigreerde mobiele service aan het profiel kiezen.  Gebruik een 'Extern eindpunt'.  Het externe eindpunt kan alleen worden toegevoegd via PowerShell.  Zie voor meer informatie de [Traffic Manager-zelfstudie](https://azure.microsoft.com/blog/azure-traffic-manager-external-endpoints-and-weighted-round-robin-via-powershell/).

## <a name="next-steps"></a>Volgende stappen
Nu dat uw toepassing is gemigreerd naar App Service, zijn er nog meer functies die u kunt gebruiken:

* Implementatie [faseringssleuven] kunt u fase wijzigingen naar uw site en uitvoeren van A / B-tests.
* [WebJobs] bieden een vervanging voor On-demand geplande taken.
* U kunt [continu implementeren] uw site koppelt u uw site aan GitHub, TFS of Mercurial.
* U kunt [Application Insights] voor het bewaken van uw site.
* Een website en een mobiele-API van dezelfde code fungeren.

### <a name="upgrading-your-site"></a>Uw Mobile Services-site upgraden naar Azure Mobile Apps SDK
* Projecten met Node.js gebaseerde server, de nieuwe [Node.js-SDK voor mobiele Apps] biedt verschillende nieuwe functies. Bijvoorbeeld, kunt u nu lokale ontwikkeling en foutopsporing, gebruik van een Node.js-versie hierboven 0,10 en aanpassen met een Express.js-middleware.
* Voor. NET op basis van server-projecten, wordt de nieuwe [Mobile Apps SDK NuGet-pakketten](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) beschikt over meer flexibiliteit op NuGet-afhankelijkheden.  Deze pakketten ondersteuning voor de nieuwe App Service-verificatie en samenstellen met een ASP.NET-project. Zie voor meer informatie over het upgraden van [uw bestaande .NET Mobile Service upgraden naar App Service](app-service-mobile-net-upgrading-from-mobile-services.md).

<!-- Images -->
[0]: ./media/app-service-mobile-migrating-from-mobile-services/migrate-to-app-service-button.PNG
[1]: ./media/app-service-mobile-migrating-from-mobile-services/migration-activity-monitor.png
[2]: ./media/app-service-mobile-migrating-from-mobile-services/triggering-job-with-postman.png

<!-- Links -->
[prijzen van App Service]: https://azure.microsoft.com/pricing/details/app-service/
[Application Insights]: ../application-insights/app-insights-overview.md
[Automatisch schalen]: ../app-service/web-sites-scale.md
[Azure App Service]: ../app-service/overview.md
[Klassieke Azure-portal]: https://manage.windowsazure.com
[Azure Portal]: https://portal.azure.com
[Azure Region]: https://azure.microsoft.com/regions/
[Abonnementen voor Azure Scheduler]: ../scheduler/scheduler-plans-billing.md
[continu implementeren]: ../app-service/deploy-continuous-deployment.md
[Uw gemengde naamruimten converteren]: https://azure.microsoft.com/blog/updates-from-notification-hubs-independent-nuget-installation-model-pmt-and-more/
[curl]: https://curl.haxx.se/
[Aangepaste domeinnamen]: ../app-service/app-service-web-tutorial-custom-domain.md
[Fiddler]: https://www.telerik.com/fiddler
[algemene beschikbaarheid van Azure App Service]: https://azure.microsoft.com/blog/announcing-general-availability-of-app-service-mobile-apps/
[Hybrid Connections]: ../app-service/app-service-hybrid-connections.md
[Logboekregistratie]: ../app-service/troubleshoot-diagnostic-logs.md
[Node.js-SDK voor mobiele Apps]: https://github.com/azure/azure-mobile-apps-node
[Mobile Services versus App Service]: app-service-mobile-value-prop-migration-from-mobile-services.md
[Notification Hubs]: ../notification-hubs/notification-hubs-push-notification-overview.md
[Bewaking van toepassingsprestaties]: ../app-service/web-sites-monitor.md
[Postman]: https://www.getpostman.com/
[faseringssleuven]: ../app-service/deploy-staging-slots.md
[VNet]: ../app-service/web-sites-integrate-with-vnet.md
[Voorbeelden van XDT transformeren]: https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples
[Functies]: ../azure-functions/functions-overview.md
