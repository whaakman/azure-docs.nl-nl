---
title: Logboekregistratie in de spraak-SDK - spraakservices
titleSuffix: Azure Cognitive Services
description: Schakel logboekregistratie in de spraak-SDK.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: amishu
ms.openlocfilehash: e8f470647c218188705cfa65cba7077e62d1b2ac
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148031"
---
# <a name="enable-logging-in-the-speech-sdk"></a>Logboekregistratie inschakelen in de spraak-SDK

Logboekregistratie naar bestand is een optionele functie van de spraak-SDK. Tijdens het ontwikkelen van biedt logboekregistratie aanvullende informatie en diagnostische gegevens van de belangrijkste onderdelen die de spraak-SDK. Kan worden ingeschakeld door de eigenschap `Speech_LogFilename` op een spraak-configuratieobject naar de locatie en naam van het logboekbestand. Logboekregistratie wereldwijd worden geactiveerd wanneer een kenmerk van die configuratie is gemaakt en daarna kan niet worden uitgeschakeld. U kunt de naam van een logbestand niet wijzigen tijdens een sessie logboekregistratie uitvoeren.

> [!NOTE]
> Logboekregistratie is beschikbaar in alle ondersteunde spraak SDK programmeertalen, met uitzondering van JavaScript.

## <a name="sample"></a>Voorbeeld

Naam van het logboekbestand is opgegeven voor een configuratieobject. Waarbij de `SpeechConfig` als een voorbeeld en ervan uitgaande dat u een exemplaar hebt gemaakt met de naam `config`:

```csharp
config.SetProperty(PropertyId.Speech_LogFilename, "LogfilePathAndName");
```

```java
config.setProperty(PropertyId.Speech_LogFilename, "LogfilePathAndName");
```

```C++
config->SetProperty(PropertyId::Speech_LogFilename, "LogfilePathAndName");
```

```Python
config.set_property(speechsdk.PropertyId.Speech_LogFilename, "LogfilePathAndName")
```

```objc
[config setPropertyTo:@"LogfilePathAndName" byId:SPXSpeechLogFilename];
```

U kunt een kenmerk van het configuratie-object maken. Hiermee schakelt u logboekregistratie voor alle kenmerken.

> [!NOTE]
> Als u maakt een `SpeechSynthesizer` van het configuratie-object, wordt deze niet logboekregistratie inschakelen. Als u logboekregistratie al is ingeschakeld, ontvangt u ook diagnostische gegevens van de `SpeechSynthesizer`.

## <a name="create-a-log-file-on-different-platforms"></a>Maken van een logboekbestand op verschillende platforms

Voor Windows of Linux, kan het logboekbestand zijn in elk pad die schrijfmachtiging voor de gebruiker heeft. Schrijfmachtigingen voor de locaties van systeem in andere besturingssystemen kunnen worden beperkt of standaard beperkt.

### <a name="universal-windows-platform-uwp"></a>UWP (Universal Windows Platform)

UWP-toepassingen moeten worden logboekbestanden plaatsen in een van de toepassing gegevenslocaties (lokaal, roaming of tijdelijk). Een logboekbestand kan worden gemaakt in de lokale map:

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
StorageFile logFile = await storageFolder.CreateFileAsync("logfile.txt", CreationCollisionOption.ReplaceExisting);
config.SetProperty(PropertyId.Speech_LogFilename, logFile.Path);
```

Machtiging voor UWP-toepassingen is informatie beschikbaar over toegang tot het bestand [hier](https://docs.microsoft.com/windows/uwp/files/file-access-permissions).

### <a name="android"></a>Android

U kunt een logboekbestand opslaan naar interne opslag, externe opslag of de cachemap. Bestanden die zijn gemaakt in de interne opslag of de map voor schijfcache zijn exclusief zijn voor de toepassing. Is het raadzaam om een logboekbestand gemaakt in de externe opslag.

```java
File dir = context.getExternalFilesDir(null);
File logFile = new File(dir, "logfile.txt");
config.setProperty(PropertyId.Speech_LogFilename, logFile.getAbsolutePath());
```

De bovenstaande code wordt een logboekbestand worden opgeslagen in de externe opslag in de hoofdmap van een specifieke map. Een gebruiker toegang heeft tot het bestand met de manager van het bestand (meestal in `Android/data/ApplicationName/logfile.txt`). Het bestand wordt verwijderd wanneer de toepassing wordt verwijderd.

U moet ook om aan te vragen `WRITE_EXTERNAL_STORAGE` machtiging in het manifestbestand:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" package="...">
  ...
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
  ...
</manifest>
```

Meer over gegevens en opslag voor Android-toepassingen is beschikbaar [hier](https://developer.android.com/guide/topics/data/data-storage.html).

#### <a name="ios"></a>iOS

Alleen de mappen in de sandbox-toepassing zijn toegankelijk. Bestanden kunnen worden gemaakt in de documenten, de bibliotheek en de tijdelijke mappen. Bestanden in de map documenten kunnen beschikbaar worden gesteld aan een gebruiker. Het volgende codefragment toont het maken van een logbestand in de toepassingsmap document:

```objc
NSString *filePath = [
  [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) firstObject]
    stringByAppendingPathComponent:@"logfile.txt"];
[speechConfig setPropertyTo:filePath byId:SPXSpeechLogFilename];
```

Voor toegang tot een bestand moet toevoegen de hieronder eigenschappen aan de `Info.plist` lijst met eigenschappen van de toepassing:

```xml
<key>UIFileSharingEnabled</key>
<true/>
<key>LSSupportsOpeningDocumentsInPlace</key>
<true/>
```

Meer over iOS-bestandssysteem is beschikbaar [hier](https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Onze C#-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

