---
title: Het oplossen van fouten van Docker-client op Windows met behulp van Visual Studio | Microsoft Docs
description: Problemen met het oplossen van problemen die kunnen optreden bij gebruik van Visual Studio maken en implementeren van web-apps op Docker in Windows met behulp van Visual Studio.
services: azure-container-service
documentationcenter: na
author: mlearned
manager: douge
editor: 
ms.assetid: 346f70b9-7b52-4688-a8e8-8f53869618d3
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 06/08/2016
ms.author: mlearned
ms.openlocfilehash: 89fa04a1107b6abb49aefd68066443717ac9b731
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-visual-studio-docker-development"></a>Problemen met Visual Studio Docker-ontwikkeling

Wanneer u met Visual Studio Tools voor Docker Preview werkt, kunnen enkele problemen optreden vanwege de aard van de preview.
Hier volgen enkele veelvoorkomende problemen en oplossingen.  

## <a name="visual-studio-2017-rc"></a>Visual Studio 2017 RC

### <a name="linux-containers"></a>**Linux-containers**

####  <a name="build-errors-occur-when-debugging-a-net-core-web-or-console-application"></a>Bouwen fouten optreden bij foutopsporing in een web- of console toepassing voor .NET Core  

Dit kan zijn gerelateerd aan het station waar het project bevindt zich niet delen met Docker voor Windows.  Er wordt een fout als volgt:

```
The "PrepareForLaunch" task failed unexpectedly.
Microsoft.DotNet.Docker.CommandLineClientException: Creating network "webapplication13628050196_default" with the default driver
Building webapplication1
Creating webapplication13628050196_webapplication1_1
ERROR: for webapplication1  Cannot create container for service webapplication1: C: drive is not shared. Please share it in Docker for Windows Settings
```
Dit probleem oplossen:

1. Met de rechtermuisknop op **Docker voor Windows** in het systeemvak en selecteer vervolgens **instellingen**.  
2. Selecteer **gedeelde stations** en delen van het station waarop het project zich bevindt.

### <a name="windows-containers"></a>**Windows-containers**

De volgende problemen zijn specifiek voor foutopsporing van .NET Framework-console en web toepassingen in Windows-containers.

#### <a name="prerequisites"></a>Vereisten

1. Visual Studio 2017 RC (of hoger) met de .NET Core en Docker Preview werkbelasting moet worden geïnstalleerd.
2. Patches van Windows 10 Verjaardag Update die aan de meest recente Windows Update. Specifiek [KB3194798](https://support.microsoft.com/en-us/help/3194798/cumulative-update-for-windows-10-version-1607-and-windows-server-2016-october-11,-2016) moet worden geïnstalleerd. 
3. [Docker voor Windows](https://docs.docker.com/docker-for-windows/) (1.13.0 bouwen of hoger) moet worden geïnstalleerd.
4. **Overschakelen naar de Windows-containers** moet worden geselecteerd. Klik in het systeemvak op **Docker voor Windows**, en selecteer vervolgens **overschakelen naar de Windows-containers**. Nadat de computer opnieuw is opgestart, moet u ervoor zorgen dat deze instelling wordt bewaard.

#### <a name="console-output-does-not-appear-in-visual-studios-output-window-while-debugging-a-console-application"></a>Console-uitvoer wordt niet weergegeven in het venster Visual Studio tijdens het opsporen van een consoletoepassing

Dit is een bekend probleem met de foutopsporingsfunctie (msvsmon.exe), die momenteel niet is ontworpen voor dit scenario. Ondersteuning voor dit scenario kan worden opgenomen in een toekomstige release. Als de uitvoer van de consoletoepassing in Visual Studio wilt weergeven, gebruikt **Docker: Start-Project**, hetgeen gelijk is aan **starten zonder foutopsporing**.

#### <a name="debugging-web-applications-with-the-release-configuration-fails-with-403-forbidden-error"></a>Webtoepassingen met de release-configuratie mislukt met fout voor (403) verboden foutopsporing

Om dit probleem omzeilen web.release.config openen in de oplossing en commentaar of verwijderen van de volgende regels:

```
<compilation xdt:Transform="RemoveAttributes(debug)" />
```

## <a name="visual-studio-2015"></a>Visual Studio 2015

### <a name="linux-containers"></a>**Linux-containers**

#### <a name="unable-to-validate-volume-mapping"></a>Kan toewijzing valideren
Toewijzing is vereist voor het delen van de broncode en binaire bestanden van uw toepassing met de app-map in de container.  Toewijzingen voor specifieke volume bevinden zich in een docker-compose.dev.debug.yml en docker-compose.dev.release.yml. Als er bestanden zijn gewijzigd op de hostmachine, weerspiegelen de containers deze wijzigingen in een vergelijkbare mapstructuur.

Toewijzing inschakelen:

1. Klik op **Moby** in het systeemvak en selecteer **instellingen**.
2. Selecteer **gedeelde schijven**.
3. Selecteer het station dat als host fungeert voor uw project en het station waar % USERPROFILE % zich bevindt.
4. Klik op **Toepassen**.

U kunt controleren of de toewijzing werkt, opnieuw en selecteer F5 uit vanuit Visual Studio nadat u een of meer stations hebt gedeeld, of voert u de volgende code vanaf een opdrachtprompt.

> [!NOTE]
> In dit voorbeeld wordt ervan uitgegaan dat de map van uw gebruikers zich op station C bevindt en dat deze is gedeeld.
> Wijzig zo nodig als u een ander station hebt gedeeld.

```
docker run -it -v /c/Users/Public:/wormhole busybox
```

Voer de volgende code in de Linux-container.

```
/ # ls
```

U ziet een mappenlijst weergegeven in de map gebruikers/openbaar. Als er worden geen bestanden worden weergegeven en de map /c/Users/Public niet leeg is, wordt toewijzing is niet juist geconfigureerd.

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

Ga naar de map wormhole om te zien van de inhoud van de `/c/Users/Public` directory:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

> [!NOTE]
> Wanneer u met virtuele Linux-machines werkt, is de container-bestandssysteem is hoofdlettergevoelig.

## <a name="build-prepareforbuild-task-failed-unexpectedly"></a>: 'PrepareForBuild' opbouwtaak is onverwacht mislukt

Microsoft.DotNet.Docker.CommandLine.ClientException: Een fout opgetreden tijdens het verbinding maken.

Controleren of de standaard Docker-host wordt uitgevoerd. Open een opdrachtprompt en voer:

```
docker info
```

Als dit een fout retourneert, probeert te starten de **Docker voor Windows** bureaublad-app. Als de bureaublad-app wordt uitgevoerd, klikt u vervolgens **Moby** moeten worden weergegeven in het systeemvak. Met de rechtermuisknop op **Moby** en open **instellingen**. Klik op **opnieuw**, en start vervolgens opnieuw Docker.

## <a name="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Een dialoogvenster voor standaardfouten van deze gebeurtenis treedt op bij een poging tot Docker-ondersteuning toevoegen of een toepassing ASP.NET Core in een container voor foutopsporing (F5)

Na het verwijderen en extensies installeert, kan de cache beheerd uitbreidbaarheid Framework (MEF) in Visual Studio beschadigd raken. Wanneer dit het geval is, kunnen er diverse foutberichten worden weergegeven wanneer u bent Docker-ondersteuning toe te voegen en/of probeert uit te voeren of opsporen (F5) in uw toepassing ASP.NET Core. Gebruik de volgende stappen uit om te verwijderen en opnieuw genereren van de cache MEF als tijdelijke oplossing.

1. Sluit alle exemplaren van Visual Studio.
1. Open % USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\.
1. Verwijder de volgende mappen:
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Open Visual Studio.
1. Het scenario opnieuw proberen.
