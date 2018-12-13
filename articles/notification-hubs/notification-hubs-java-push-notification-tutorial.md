---
title: Notification Hubs gebruiken met Java
description: Leer hoe u Azure Notification Hubs gebruiken vanuit een back-end in Java.
services: notification-hubs
documentationcenter: ''
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 4c3f966d-0158-4a48-b949-9fa3666cb7e4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: java
ms.devlang: java
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 3251e2ecc9171081c5128dd0782eecdf83064114
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53312252"
---
# <a name="how-to-use-notification-hubs-from-java"></a>Hoe u Notification Hubs gebruiken vanuit Java
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Dit onderwerp beschrijft de belangrijkste functies van de nieuwe volledig ondersteunde officiële Azure Notification Hub Java SDK.
Dit project is een open source-project en vindt u de gehele SDK-code hier [Java SDK].

In het algemeen kunt u alle functies van Notification Hubs openen vanuit een Java/PHP/Python/Ruby back-end met behulp van de Notification Hub REST-interface zoals beschreven in de MSDN-onderwerp [Notification Hubs REST-API's](https://msdn.microsoft.com/library/dn223264.aspx). Deze Java-SDK biedt een thin-wrapper ten opzichte van deze REST-API in Java.

De SDK biedt momenteel ondersteuning:

* CRUD van Notification Hubs
* CRUD op registraties
* Installatie-Management
* Registraties voor importeren/exporteren
* Reguliere verzendt
* Geplande verzendt
* Asynchrone bewerkingen via Java NIO
* Ondersteunde platforms: APNS (iOS), GCM (Android), WNS (Windows Store-apps), MPNS (Windows Phone), ADM (Amazon Kindle Fire), Baidu (Android zonder Google-services)

## <a name="sdk-usage"></a>SDK-gebruik
### <a name="compile-and-build"></a>Compileren en bouwen
Gebruik [Maven]

Bouwen:

    mvn package

## <a name="code"></a>Code
### <a name="notification-hub-cruds"></a>Notification Hub CRUDs
**Maak een NamespaceManager:**

    NamespaceManager namespaceManager = new NamespaceManager("connection string")

**Notification Hub maken:**

    NotificationHubDescription hub = new NotificationHubDescription("hubname");
    hub.setWindowsCredential(new WindowsCredential("sid","key"));
    hub = namespaceManager.createNotificationHub(hub);

 OF

    hub = new NotificationHub("connection string", "hubname");

**Notification Hub ophalen:**

    hub = namespaceManager.getNotificationHub("hubname");

**Notification Hub bijwerken:**

    hub.setMpnsCredential(new MpnsCredential("mpnscert", "mpnskey"));
    hub = namespaceManager.updateNotificationHub(hub);

**Notification Hub verwijderen:**

    namespaceManager.deleteNotificationHub("hubname");

### <a name="registration-cruds"></a>Registratie CRUDs
**Maak een Notification Hub-client:**

    hub = new NotificationHub("connection string", "hubname");

**Windows-registratie maken:**

    WindowsRegistration reg = new WindowsRegistration(new URI(CHANNELURI));
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");
    hub.createRegistration(reg);

**IOS-registratie maken:**

    AppleRegistration reg = new AppleRegistration(DEVICETOKEN);
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");
    hub.createRegistration(reg);

Op deze manier kunt u rapporten maken voor Android (GCM), Windows Phone (MPNS) en Kindle Fire (ADM).

**Sjabloonregistraties maken:**

    WindowsTemplateRegistration reg = new WindowsTemplateRegistration(new URI(CHANNELURI), WNSBODYTEMPLATE);
    reg.getHeaders().put("X-WNS-Type", "wns/toast");
    hub.createRegistration(reg);

**Rapporten maken met behulp van registratie-ID + upsert maken patroon**

Hiermee verwijdert u dubbele vanwege verloren antwoorden als registratie-id's opslaan op het apparaat:

    String id = hub.createRegistrationId();
    WindowsRegistration reg = new WindowsRegistration(id, new URI(CHANNELURI));
    hub.upsertRegistration(reg);

**Registraties bijwerken:**

    hub.updateRegistration(reg);

**Registraties te verwijderen:**

    hub.deleteRegistration(regid);

**Query-registraties:**

* **Afzonderlijke registratie krijgen:**

        hub.getRegistration(regid);

* **Haal alle registraties in hub:**

        hub.getRegistrations();

* **Registraties met tag ophalen:**

        hub.getRegistrationsByTag("myTag");

* **Registraties kanaal ophalen:**

        hub.getRegistrationsByChannel("devicetoken");


Alle query's ondersteunen $top en voortzetting van tokens.

### <a name="installation-api-usage"></a>Installatie van API-gebruik
Installatie API is een alternatief mechanisme voor het registratiebeheer van de. In plaats van het onderhouden van meerdere registraties, die niet essentieel zijn en eenvoudig kunnen worden gedaan ten onrechte of inefficiënt, is het nu mogelijk om met een EENMALIGE installatie-object. Installatie bevat alles wat u nodig hebt: push-kanaal (apparaattoken), labels, sjablonen, secundaire tegels (voor WNS en APNS). U hoeft niet de service meer-ID ophalen - NET GUID of een andere id genereren, op apparaat behouden en verzenden naar uw back-end, samen met push-kanaal (apparaattoken) aanroepen.
Op de back-end, moet u slechts één aanroep doen: CreateOrUpdateInstallation, het is volledig idempotent zijn, dus u kunt indien nodig opnieuw uit te voeren.

Als voorbeeld voor Amazon Kindle Fire:

    Installation installation = new Installation("installation-id", NotificationPlatform.Adm, "adm-push-channel");
    hub.createOrUpdateInstallation(installation);

Als u bijwerken wilt:

    installation.addTag("foo");
    installation.addTemplate("template1", new InstallationTemplate("{\"data\":{\"key1\":\"$(value1)\"}}","tag-for-template1"));
    installation.addTemplate("template2", new InstallationTemplate("{\"data\":{\"key2\":\"$(value2)\"}}","tag-for-template2"));
    hub.createOrUpdateInstallation(installation);

Voor geavanceerde scenario's, gebruikt u de mogelijkheid gedeeltelijke update, waarmee u kunt alleen bepaalde eigenschappen van het object installatie wijzigen. Gedeeltelijke update is een subset van JSON-Patch bewerkingen die kan worden uitgevoerd voor de installatie-object.

    PartialUpdateOperation addChannel = new PartialUpdateOperation(UpdateOperationType.Add, "/pushChannel", "adm-push-channel2");
    PartialUpdateOperation addTag = new PartialUpdateOperation(UpdateOperationType.Add, "/tags", "bar");
    PartialUpdateOperation replaceTemplate = new PartialUpdateOperation(UpdateOperationType.Replace, "/templates/template1", new InstallationTemplate("{\"data\":{\"key3\":\"$(value3)\"}}","tag-for-template1")).toJson());
    hub.patchInstallation("installation-id", addChannel, addTag, replaceTemplate);

Installatie verwijderen:

    hub.deleteInstallation(installation.getInstallationId());

CreateOrUpdate, Patch en Delete uiteindelijk consistent zijn met Get. De aangevraagde bewerking wordt alleen gaat naar de wachtrij van het systeem tijdens de oproep en in de achtergrond wordt uitgevoerd. Ophalen is niet ontworpen voor belangrijkste runtime-scenario, maar alleen voor foutopsporing en probleemoplossing, is nauw worden beperkt door de service.

Verzenden stroom voor installaties is dezelfde als die voor registraties. Kennisgeving aan de installatie van bepaalde doel-code alleen gebruiken ' de installatie-id: {desired-id} '. In dit geval is de code:

    Notification n = Notification.createWindowsNotification("WNS body");
    hub.sendNotification(n, "InstallationId:{installation-id}");

Voor een van verschillende sjablonen:

    Map<String, String> prop =  new HashMap<String, String>();
    prop.put("value3", "some value");
    Notification n = Notification.createTemplateNotification(prop);
    hub.sendNotification(n, "InstallationId:{installation-id} && tag-for-template1");

### <a name="schedule-notifications-available-for-standard-tier"></a>Plan meldingen (beschikbaar voor STANDARD-laag)
Hetzelfde als normale verzenden, maar met een extra parameter - scheduledTime tekst wanneer de melding moet worden geleverd. Service accepteert een willekeurig punt van de tijd tussen nu + 5 minuten en nu + 7 dagen.

**Plannen van een systeemeigen Windows-melding:**

    Calendar c = Calendar.getInstance();
    c.add(Calendar.DATE, 1);
    Notification n = Notification.createWindowsNotification("WNS body");
    hub.scheduleNotification(n, c.getTime());

### <a name="importexport-available-for-standard-tier"></a>Import/Export (beschikbaar voor STANDARD-laag)
Soms is het vereist bewerking tegen registraties bulksgewijs uit te voeren. Meestal is het voor de integratie met een ander systeem of slechts een zeer grote oplossing te zeggen hadden bijwerken van de tags. Het verdient aanbeveling niet ophalen/bijwerken stroom gebruiken als registraties duizenden betrokken zijn. Import/Export-mogelijkheid is ontworpen om te kunnen krijgen van het scenario. In feite bieden u een toegang tot sommige blob-container in uw storage-account als een bron van binnenkomende gegevens en de locatie voor de uitvoer.

**Exporttaak verzenden:**

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ExportRegistrations);
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);


**Import-taak verzenden:**

    NotificationHubJob job = new NotificationHubJob();
    job.setJobType(NotificationHubJobType.ImportCreateRegistrations);
    job.setImportFileUri("input file uri with SAS signature");
    job.setOutputContainerUri("container uri with SAS signature");
    job = hub.submitNotificationHubJob(job);

**Wacht totdat de taak is voltooid:**

    while(true){
        Thread.sleep(1000);
        job = hub.getNotificationHubJob(job.getJobId());
        if(job.getJobStatus() == NotificationHubJobStatus.Completed)
            break;
    }

**Alle taken ophalen:**

    List<NotificationHubJob> jobs = hub.getAllNotificationHubJobs();

**De URI met SAS-handtekening:** Deze URL is de URL van een blob-bestand of de blob-container plus de set parameters, zoals machtigingen en verlooptijd plus de handtekening van al deze dingen gemaakt met behulp van de account-SAS-sleutel. Azure Storage-SDK voor Java heeft uitgebreide mogelijkheden, inclusief het maken van deze soorten URI's. Als alternatief voor eenvoudige kunt u Kijk eens ImportExportE2E test klasse (van de GitHub-locatie) die beschikt over eenvoudige en compacte implementatie van het algoritme voor ondertekening.

### <a name="send-notifications"></a>Meldingen verzenden
Het object melding is gewoon een hoofdtekst met kopteksten, bepaalde hulpprogrammamethoden voor het te helpen bij het bouwen van de sjabloon en systeemeigen meldingen-objecten.

* **Windows Store en Windows Phone 8.1 (zonder Silverlight)**

        String toast = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">Hello from Java!</text></binding></visual></toast>";
        Notification n = Notification.createWindowsNotification(toast);
        hub.sendNotification(n);
* **iOS**

        String alert = "{\"aps\":{\"alert\":\"Hello from Java!\"}}";
        Notification n = Notification.createAppleNotification(alert);
        hub.sendNotification(n);
* **Android**

        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createGcmNotification(message);
        hub.sendNotification(n);
* **Windows Phone 8.0 en 8.1 Silverlight**

        String toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>Hello from Java!</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
        Notification n = Notification.createMpnsNotification(toast);
        hub.sendNotification(n);
* **Kindle Fire**

        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createAdmNotification(message);
        hub.sendNotification(n);
* **Verzenden naar labels**
  
        Set<String> tags = new HashSet<String>();
        tags.add("boo");
        tags.add("foo");
        hub.sendNotification(n, tags);
* **Verzenden naar tagexpressie**

        hub.sendNotification(n, "foo && ! bar");
* **Sjabloon melding verzenden**

        Map<String, String> prop =  new HashMap<String, String>();
        prop.put("prop1", "v1");
        prop.put("prop2", "v2");
        Notification n = Notification.createTemplateNotification(prop);
        hub.sendNotification(n);

Uitvoering van uw Java-code, moet er nu een melding wordt weergegeven op het doelapparaat produceren.

## <a name="next-steps"></a>Volgende stappen
In dit onderwerp laten zien hoe u een eenvoudige Java-REST-client voor Notification Hubs maakt. Hier kunt u het volgende doen:

* Download de volledige [Java SDK], die de gehele SDK-code bevat.
* Experimenteer met de voorbeelden:
  * [Aan de slag met Notification Hubs]
  * [Belangrijk nieuws te verzenden]
  * [Gelokaliseerde belangrijk nieuws te verzenden]
  * [Meldingen verzenden naar geverifieerde gebruikers]
  * [Platformonafhankelijke meldingen verzenden naar geverifieerde gebruikers]

[Java SDK]: https://github.com/Azure/azure-notificationhubs-java-backend
[Get started tutorial]: notification-hubs-ios-apple-push-notification-apns-get-started.md
[Aan de slag met Notification Hubs]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Belangrijk nieuws te verzenden]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Gelokaliseerde belangrijk nieuws te verzenden]: notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md
[Meldingen verzenden naar geverifieerde gebruikers]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Platformonafhankelijke meldingen verzenden naar geverifieerde gebruikers]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Maven]: http://maven.apache.org/

