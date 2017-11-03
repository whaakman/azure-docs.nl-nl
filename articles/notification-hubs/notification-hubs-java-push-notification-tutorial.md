---
title: Notification Hubs gebruiken met Java
description: Informatie over het gebruik van Azure Notification Hubs vanuit een Java-back-end.
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 4c3f966d-0158-4a48-b949-9fa3666cb7e4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: java
ms.devlang: java
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 41f978750ddef9f7e878c65b0017e909720154aa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-notification-hubs-from-java"></a>Hoe Notification Hubs gebruiken vanuit Java
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

Dit onderwerp beschrijft de belangrijkste functies van de nieuwe volledig ondersteunde officiële Azure Notification Hub Java SDK. Dit is een open source-project en vindt u de volledige code SDK aan [Java SDK]. 

In het algemeen kunt u alle functies van de Notification Hubs kunt openen vanaf een Java/PHP/Python/Ruby back-end met de Notification Hub REST-interface, zoals beschreven in de MSDN-onderwerp [Notification Hubs REST-API's](http://msdn.microsoft.com/library/dn223264.aspx). Deze SDK voor Java biedt een thin wrapper via deze interfaces REST in Java. 

De SDK biedt momenteel ondersteuning:

* CRUD op Notification Hubs 
* CRUD op registraties
* Installatie-Management
* Registraties voor importeren/exporteren
* Reguliere verzendt
* Geplande verzendt
* Asynchrone bewerkingen via Java NIO
* Ondersteunde platforms: APNS (iOS), GCM (Android), WNS (Windows Store-apps), MPNS (Windows Phone), ADM (Amazon Kindle Fire), Baidu (Android zonder Google-services) 

## <a name="sdk-usage"></a>Gebruik van de SDK
### <a name="compile-and-build"></a>Compileren en bouwen
Gebruik [Maven]

Voor het bouwen:

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

**Haal de Notification Hub:**

    hub = namespaceManager.getNotificationHub("hubname");

**Notification Hub werken:**

    hub.setMpnsCredential(new MpnsCredential("mpnscert", "mpnskey"));
    hub = namespaceManager.updateNotificationHub(hub);

**Verwijderen van Notification Hub:**

    namespaceManager.deleteNotificationHub("hubname");

### <a name="registration-cruds"></a>Registratie CRUDs
**Maak een Notification Hub-client:**

    hub = new NotificationHub("connection string", "hubname");

**Maak de registratie van Windows:**

    WindowsRegistration reg = new WindowsRegistration(new URI(CHANNELURI));
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");    
    hub.createRegistration(reg);

**IOS-registratie maken:**

    AppleRegistration reg = new AppleRegistration(DEVICETOKEN);
    reg.getTags().add("myTag");
    reg.getTags().add("myOtherTag");
    hub.createRegistration(reg);

U kunt ook registraties maken voor Android (GCM), Windows Phone (MPNS) en Kindle Fire (ADM).

**Sjabloon registraties maken:**

    WindowsTemplateRegistration reg = new WindowsTemplateRegistration(new URI(CHANNELURI), WNSBODYTEMPLATE);
    reg.getHeaders().put("X-WNS-Type", "wns/toast");
    hub.createRegistration(reg);

**Registraties maken met behulp van maken registrationid + upsert patroon**

Hiermee verwijdert u duplicaten vanwege geen antwoorden verloren als de registratie-id's worden opgeslagen op het apparaat:

    String id = hub.createRegistrationId();
    WindowsRegistration reg = new WindowsRegistration(id, new URI(CHANNELURI));
    hub.upsertRegistration(reg);

**Registraties bijwerken:**

    hub.updateRegistration(reg);

**Registraties verwijderen:**

    hub.deleteRegistration(regid);

**Query-registraties:**

* **Afzonderlijke registratie ophalen:**
  
    hub.getRegistration(regid);
* **Haal alle registraties in hub:**
  
    hub.getRegistrations();
* **Registraties met code ophalen:**
  
    hub.getRegistrationsByTag("myTag");
* **Haal registraties door kanaal:**
  
    hub.getRegistrationsByChannel("devicetoken");

Alle query's ondersteunen $top en voortzetting tokens.

### <a name="installation-api-usage"></a>Gebruik API-installatie
API-installatie is een alternatief mechanisme voor het beheer van de registratie. In plaats van meerdere registraties die niet is heel eenvoudig en gemakkelijk kan worden gedaan ten onrechte of inefficiënt onderhouden, is het nu mogelijk om met een enkele installatie-object. Installatie bevat alles wat u nodig: push-kanaal (apparaattoken), tags, sjablonen, secundaire tegels (voor WNS en APNS). U hoeft niet de service voor meer-Id ophalen - NET GUID of een andere id genereren, op apparaat behouden en verzenden naar uw back-end samen met push-kanaal (apparaattoken) aanroepen. Op de back-end moet u slechts één aanroep doen: CreateOrUpdateInstallation, is het volledig idempotent, dus probeer zonodig opnieuw.

Als voorbeeld voor Amazon Kindle Fire als volgt uitziet:

    Installation installation = new Installation("installation-id", NotificationPlatform.Adm, "adm-push-channel");
    hub.createOrUpdateInstallation(installation);

Als u bijwerken wilt: 

    installation.addTag("foo");
    installation.addTemplate("template1", new InstallationTemplate("{\"data\":{\"key1\":\"$(value1)\"}}","tag-for-template1"));
    installation.addTemplate("template2", new InstallationTemplate("{\"data\":{\"key2\":\"$(value2)\"}}","tag-for-template2"));
    hub.createOrUpdateInstallation(installation);

Voor geavanceerde scenario's hebben we gedeeltelijke updatefunctie zodat alleen bepaalde eigenschappen van het object installatie wijzigen. Gedeeltelijke update is in feite subset van JSON-Patch-bewerkingen die u met de installatie-object uitvoeren kunt.

    PartialUpdateOperation addChannel = new PartialUpdateOperation(UpdateOperationType.Add, "/pushChannel", "adm-push-channel2");
    PartialUpdateOperation addTag = new PartialUpdateOperation(UpdateOperationType.Add, "/tags", "bar");
    PartialUpdateOperation replaceTemplate = new PartialUpdateOperation(UpdateOperationType.Replace, "/templates/template1", new InstallationTemplate("{\"data\":{\"key3\":\"$(value3)\"}}","tag-for-template1")).toJson());
    hub.patchInstallation("installation-id", addChannel, addTag, replaceTemplate);

Verwijder de installatie:

    hub.deleteInstallation(installation.getInstallationId());

CreateOrUpdate, Patch en Delete zijn uiteindelijk consistent is met Get. De aangevraagde bewerking wordt alleen overschakelt naar de wachtrij van het systeem tijdens de oproep en wordt uitgevoerd op de achtergrond. Houd er rekening mee dat Get is niet ontworpen voor belangrijkste runtime-scenario, maar alleen voor foutopsporing en het oplossen van problemen, het is nauw beperkt door de service.

Verzenden stroom voor installaties is hetzelfde als voor registraties. Hebben zojuist wij een optie voor het doel van de melding aan de installatie van bepaalde: tag gebruiken ' de installatie-id: {desired-id} '. Voor de bovenstaande eruit deze als volgt:

    Notification n = Notification.createWindowsNotification("WNS body");
    hub.sendNotification(n, "InstallationId:{installation-id}");

Voor een van de verschillende sjablonen:

    Map<String, String> prop =  new HashMap<String, String>();
    prop.put("value3", "some value");
    Notification n = Notification.createTemplateNotification(prop);
    hub.sendNotification(n, "InstallationId:{installation-id} && tag-for-template1");

### <a name="schedule-notifications-available-for-standard-tier"></a>Meldingen (beschikbaar voor STANDARD-laag) plannen
Hetzelfde als reguliere verzenden, maar met één extra parameter - scheduledTime die aangeeft wanneer de melding moet worden geleverd. Service accepteert elk punt in tijd tussen nu + 5 minuten en nu + 7 dagen.

**Plannen van een systeemeigen Windows-melding:**

    Calendar c = Calendar.getInstance();
    c.add(Calendar.DATE, 1);    
    Notification n = Notification.createWindowsNotification("WNS body");
    hub.scheduleNotification(n, c.getTime());

### <a name="importexport-available-for-standard-tier"></a>(Beschikbaar voor de prijscategorie STANDARD) voor importeren/exporteren
Soms is het vereist bulksgewijze bewerking tegen registraties uit te voeren. Meestal is voor de integratie met een ander systeem of alleen een enorme fix om in te spreken bijwerken de labels. Het verdient sterk niet ophalen/bijwerken stroom gebruiken als we duizenden registraties bedoelen. Mogelijkheid voor importeren/exporteren is ontworpen voor het scenario. In feite bieden u een toegang tot een aantal blob-container onder uw storage-account als een bron van binnenkomende gegevens en de locatie voor de uitvoer.

**Taak voor het exporteren verzenden:**

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

**Wacht totdat de taak wordt uitgevoerd:**

    while(true){
        Thread.sleep(1000);
        job = hub.getNotificationHubJob(job.getJobId());
        if(job.getJobStatus() == NotificationHubJobStatus.Completed)
            break;
    }       

**Alle taken ophalen:**

    List<NotificationHubJob> jobs = hub.getAllNotificationHubJobs();

**URI met SAS-handtekening:** dit is de URL van een aantal blob-bestand of de blob-container plus set parameters zoals machtigingen en verlooptijd plus handtekening van deze dingen die zijn gemaakt met behulp van account-SAS-sleutel. Azure Storage Java SDK heeft geavanceerde mogelijkheden, waaronder het maken van dergelijke soort URI's. U kunt als alternatief voor eenvoudige kijken ImportExportE2E test klasse (van de github-locatie) met zeer basic en compact uitvoering van handtekeningalgoritme nemen.

### <a name="send-notifications"></a>Meldingen verzenden
Het object melding is gewoon een instantie met kopteksten, bepaalde hulpprogrammamethoden voor het helpen bij het bouwen van de sjabloon en systeemeigen meldingen-objecten.

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
* **Fire kindle**
  
        String message = "{\"data\":{\"msg\":\"Hello from Java!\"}}";
        Notification n = Notification.createAdmNotification(message);
        hub.sendNotification(n);
* **Verzenden naar labels**
  
        Set<String> tags = new HashSet<String>();
        tags.add("boo");
        tags.add("foo");
        hub.sendNotification(n, tags);
* **Verzenden naar het code-expressie**       
  
        hub.sendNotification(n, "foo && ! bar");
* **Sjabloon melding verzenden**
  
        Map<String, String> prop =  new HashMap<String, String>();
        prop.put("prop1", "v1");
        prop.put("prop2", "v2");
        Notification n = Notification.createTemplateNotification(prop);
        hub.sendNotification(n);

Uw Java-code wordt uitgevoerd, moet u een melding weergegeven op het doelapparaat nu produceren.

## <a name="next-steps"></a>Volgende stappen
In dit onderwerp we hebt u geleerd hoe u een eenvoudige Java REST-client voor Notification Hubs maakt. Hier kunt u het volgende doen:

* Downloaden van de volledige [Java SDK], die de hele SDK-code bevat. 
* Met de voorbeelden spelen:
  * [Aan de slag met Notification Hubs]
  * [Belangrijk nieuws te verzenden]
  * [Gelokaliseerde belangrijk nieuws te verzenden]
  * [Meldingen verzenden aan geverifieerde gebruikers]
  * [Cross-platform meldingen verzenden aan geverifieerde gebruikers]

[Java SDK]: https://github.com/Azure/azure-notificationhubs-java-backend
[Get started tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Aan de slag met Notification Hubs]: http://www.windowsazure.com/manage/services/notification-hubs/getting-started-windows-dotnet/
[Belangrijk nieuws te verzenden]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-dotnet/
[Gelokaliseerde belangrijk nieuws te verzenden]: http://www.windowsazure.com/manage/services/notification-hubs/breaking-news-localized-dotnet/
[Meldingen verzenden aan geverifieerde gebruikers]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users/
[Cross-platform meldingen verzenden aan geverifieerde gebruikers]: http://www.windowsazure.com/manage/services/notification-hubs/notify-users-xplat-mobile-services/
[Maven]: http://maven.apache.org/

