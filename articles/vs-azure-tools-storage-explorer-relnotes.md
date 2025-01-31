---
title: Opmerkingen bij de release van Microsoft Azure Storage Explorer
description: Opmerkingen bij de release voor Microsoft Azure Storage Explorer
services: storage
documentationcenter: na
author: cawaMS
manager: paulyuk
editor: ''
ms.assetid: ''
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2018
ms.author: cawa
ms.openlocfilehash: 1a051f3a0c55e207e6a53955d1cb4b9ea7e54a4d
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67544148"
---
# <a name="microsoft-azure-storage-explorer-release-notes"></a>Opmerkingen bij de release van Microsoft Azure Storage Explorer

In dit artikel bevat de releaseopmerkingen voor Azure Storage Explorer 1.9.0 release, evenals de opmerkingen bij de release voor eerdere versies.

[Microsoft Azure Storage Explorer](./vs-azure-tools-storage-manage-with-storage-explorer.md) is een zelfstandige app waarmee u eenvoudig met Azure Storage-gegevens kunt werken via Windows, macOS en Linux.

## <a name="version-190"></a>Versie 1.9.0
7/1/2019

### <a name="download-azure-storage-explorer-190"></a>Azure Storage Explorer 1.9.0 downloaden
- [Azure Storage Explorer 1.9.0 voor Windows](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Azure Storage Explorer 1.9.0 voor Mac](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Azure Storage Explorer 1.9.0 voor Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>Nieuw

* Nu kunt u de Blob-containers via Azure AD (ACL of RBAC-machtigingen) koppelen. Deze functie is bedoeld om u te helpen bij gebruikers die toegang tot containers, maar niet de Storage-Accounts die de containers hebben in. Zie onze Getting Started Guide voor meer informatie over deze functie.
* Verkrijgen te verbreken van de lease nu werken met RBAC. [#1354](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1354)
* Toegangsbeleid beheren en de instelling openbaar toegangsniveau nu werkt met RBAC. [#1355](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1355)
* Verwijderen blob mappen werken nu met RBAC. [#1450](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1450)
* Het wijzigen van blob-toegangslaag nu werken met RBAC. [#1446](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1446)
* U kunt nu snel opnieuw instellen van Snelweergavetoegang via 'Help' → "Herstellen". [#1327](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1327)

### <a name="preview-features"></a>Preview-functies

* Apparaat code flow aanmelden is nu beschikbaar om te bekijken. Als u wilt inschakelen, gaat u naar 'Preview' → 'Gebruik apparaat Code Flow aanmelden'. U wordt aangeraden alle gebruikers die hebben gehad problemen met lege aanmelding bij windows om te proberen deze functie, zoals het blijken te zijn van een meer betrouwbare vorm van aanmelding.
* Storage Explorer is geïntegreerd met AzCopy is momenteel beschikbaar voor preview. Als u wilt inschakelen, gaat u naar 'Preview' → 'Met AzCopy voor verbeterde Blob uploaden en downloaden'. BLOB-overdracht is voltooid met AzCopy moeten worden snellere en beter.

### <a name="fixes"></a>Oplossingen

* Vaste wordt kan niet worden geladen van meer dan 50 abonnementen voor één account. [#1416](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1416)
* Vaste de knop 'Aanmelden' werkt niet op de informatiebalk die wordt weergegeven wanneer een directe koppeling is mislukt. [#1358](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1358)
* Vaste niet wordt .app-bestanden in macOS te uploaden. [#1119](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1119)
* Vaste "opnieuw alle' werkt niet voor de naam van een mislukte blob wijzigen. [#992](https://www.github.com/Microsoft/AzureStorageExplorer/issues/992)
* Vaste "Annuleren" werkt niet tijdens het openen van een blob. [#1464](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1464)
* Vaste meerdere problemen met de spelling en knopinfo in het hele product. Veel dank alle die deze problemen gerapporteerd. [#1303](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1303), [#1328](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1328), [#1329](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1329), [#1331](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1331), [#1336](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1336), [#1352](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1352), [#1368](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1368), [#1395](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1395)

### <a name="known-issues"></a>Bekende problemen

* Bij het uitvoeren van een niet - AzCopy Blob downloaden, wordt de MD5 voor grote bestanden niet wordt gecontroleerd. Dit is vanwege een fout in de Storage-SDK. [#1212](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1212)
* Als u met RBAC kunt moet Storage Explorer enkele management layer-machtigingen voor toegang tot uw opslagresources. Zie de [problemen oplossen met](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) voor meer informatie.
* Probeert te krijgen tot ADLS Gen2 Blobs wanneer u zich achter een proxy mislukken.
* Loskoppelen van een resource koppelen via SAS-URI, zoals een blob-container kan een fout veroorzaken waarmee wordt voorkomen dat andere bijlagen correct weergegeven. U kunt dit probleem omzeilen, vernieuwt u het knooppunt voor de. Zie #537 voor meer informatie.
* Als u Visual Studio voor Mac en ooit een aangepaste AAD-configuratie hebt gemaakt, is het wellicht niet mogelijk om aan te melden. Verwijder de inhoud van het probleem te omzeilen, ~ /. IdentityService/AadConfigurations. Als u in dat geval heeft niet de blokkering opheffen u, als opmerking bij dit probleem.
* Azurite is nog niet volledig geïmplementeerd alle Storage-API's. Als gevolg hiervan kunnen er onverwachte fouten of problemen bij het gebruik van Azurite voor opslag.
* In zeldzame gevallen kan de focus structuur ophalen vastgelopen op Snelweergavetoegang. Registreer de focus, kunt u Alles vernieuwen.
* Uploaden vanaf uw OneDrive-map werkt niet vanwege een fout in NodeJS. De fout is opgelost, maar nog niet is geïntegreerd in Electron. Als tijdelijke oplossing voor dit probleem tijdens het uploaden naar of downloaden van een blob-container, kunt u de experimentele functie voor AzCopy.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter omzeilen hier beschreven.
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Azure Stack biedt geen ondersteuning voor de volgende functies. Poging tot het gebruiken van deze functies tijdens het werken met Azure Stack kunnen resources leiden tot onverwachte fouten.
   * Bestandsshares
   * Toegangslagen
   * Voorlopig verwijderen
   * ADLS Gen2
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Storage Explorer op Linux uitgevoerd, moet bepaalde afhankelijkheden moeten eerst worden geïnstalleerd. Controleer de Storage Explorer [problemen oplossen met](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#linux-dependencies) voor meer informatie.

## <a name="previous-releases"></a>Vorige versies

* [Versie 1.8.1 upgraden](#version-181)
* [Versie 1.8.0](#version-180)
* [Versie 1.7.0](#version-170)
* [Versie 1.6.2](#version-162)
* [Versie 1.6.1](#version-161)
* [Versie 1.6.0](#version-160)
* [Versie 1.5.0](#version-150)
* [Versie 1.4.4](#version-144)
* [Versie 1.4.3](#version-143)
* [Versie 1.4.2](#version-142)
* [Versie 1.4.1](#version-141)
* [Versie 1.3.0](#version-130)
* [Versie 1.2.0](#version-120)
* [Versie 1.1.0](#version-110)
* [Versie 1.0.0](#version-100)
* [Versie 0.9.6](#version-096)
* [Versie 0.9.5](#version-095)
* [Versie 0.9.4 en 0.9.3](#version-094-and-093)
* [Versie 0.9.2](#version-092)
* [Versie 0.9.1 tot en en 0.9.0](#version-091-and-090)
* [Versie 0.8.16](#version-0816)
* [Versie 0.8.14](#version-0814)
* [Versie 0.8.13](#version-0813)
* [Versie 0.8.12 en 0.8.11 en 0.8.10](#version-0812-and-0811-and-0810)
* [Versie 0.8.9 en 0.8.8](#version-089-and-088)
* [Versie 0.8.7](#version-087)
* [Versie 0.8.6](#version-086)
* [Versie 0.8.5](#version-085)
* [Versie 0.8.4](#version-084)
* [Versie 0.8.3](#version-083)
* [Versie 0.8.2](#version-082)
* [Versie 0.8.0](#version-080)
* [Versie 0.7.20160509.0](#version-07201605090)
* [Versie 0.7.20160325.0](#version-07201603250)
* [Versie 0.7.20160129.1](#version-07201601291)
* [Versie 0.7.20160105.0](#version-07201601050)
* [Versie 0.7.20151116.0](#version-07201511160)

## <a name="version-181"></a>Versie 1.8.1 upgraden
5/13/2019

### <a name="hotfixes"></a>Hotfixes
* In sommige gevallen retourneert te klikken op 'meer ' laden op het resourceniveau van de niet de volgende pagina van resources. Dit probleem is opgelost. [#1359](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1359)
* Op Windows mislukken AzCopy downloaden als een enkel bestand of map is gedownload en de naam van het bestand of map heeft een teken dat ongeldig voor een Windows-pad is. Dit probleem is opgelost. [#1350](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1350)
* In zeer uitzonderlijke gevallen, er tijdens het uitvoeren van een nieuwe naam is van een bestandsshare of een naam in een bestandsshare als de exemplaren voor de naam wijzigen is mislukt of als de opslag verkennen om te bevestigen van het succes van de exemplaren met Azure, kan de mogelijkheid van Storage Explorer de o verwijderen oorspronkelijke bestanden voordat de kopie is voltooid. Dit probleem is opgelost.

### <a name="new"></a>Nieuw

* De geïntegreerde AzCopy-versie is bijgewerkt naar versie 10.1.0.
* CTRL / Cmd + R kan nu worden gebruikt voor het vernieuwen van de momenteel gerichte editor. [#1097](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1097)
* De API van Azure Stack-Storage-versie is gewijzigd in 17-04-2017.
* Het dialoogvenster toegang beheren voor ADLS Gen2 wordt nu het masker synchroon houden op een manier die vergelijkbaar is met andere hulpprogramma's voor POSIX-machtigingen. De gebruikersinterface, waarschuwt u als een wijziging wordt doorgevoerd waarmee zorgt ervoor dat de machtigingen van een gebruiker of groep overschrijdt de grenzen van het masker. [#1253](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1253)
* De vlag voor het berekenen en instellen van de MD5-hash is nu ingeschakeld voor de AzCopy-uploads. [#1223](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1223)


### <a name="preview-features"></a>Preview-functies

* Apparaat code flow aanmelden is nu beschikbaar om te bekijken. Als u wilt inschakelen, gaat u naar 'Preview' → 'Gebruik apparaat Code Flow aanmelden'. U wordt aangeraden alle gebruikers die hebben gehad problemen met lege aanmelding bij windows om te proberen deze functie, zoals het blijken te zijn van een meer betrouwbare vorm van aanmelding.
* Storage Explorer is geïntegreerd met AzCopy is momenteel beschikbaar voor preview. Als u wilt inschakelen, gaat u naar 'Preview' → 'Met AzCopy voor verbeterde Blob uploaden en downloaden'. BLOB-overdracht is voltooid met AzCopy moeten worden snellere en beter.

### <a name="fixes"></a>Oplossingen

* Het dialoogvenster toegangsbeleid stelt u een vervaldatum niet meer op toegang-beleid voor opslag die nog geen een vervaldatum. [#764](https://www.github.com/Microsoft/AzureStorageExplorer/issues/764)
* Sommige wijzigingen zijn aangebracht aan het genereren van SAS-dialoogvenster om te controleren of dat opgeslagen toegangsbeleid correct worden gebruikt bij het genereren van een SAS. [#1269](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1269)
* Wanneer u probeert te uploaden van een niet - 512 byte uitgelijnd bestand naar een pagina-Blob, wordt nu een meer relevante fout weergegeven in Storage Explorer. [#1050](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1050)
* Kopiëren van een Blob-container die u een weergavenaam gebruikt, mislukken. Nu wordt de werkelijke naam van de Blob-container gebruikt. [#1166](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1166)
* Er wordt geprobeerd bepaalde acties uitvoeren op een ADLS-Gen2 map waarin het unicode-tekens in de naam, mislukken. Alle bewerkingen moeten nu worden uitgevoerd. [#980](https://www.github.com/Microsoft/AzureStorageExplorer/issues/980)

### <a name="known-issues"></a>Bekende problemen

* Bij het uitvoeren van een niet - AzCopy Blob downloaden, wordt de MD5 voor grote bestanden niet wordt gecontroleerd. Dit is vanwege een fout in de Storage-SDK. [#1212](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1212)
* Als u met RBAC kunt moet Storage Explorer enkele management layer-machtigingen voor toegang tot uw opslagresources. Zie de [problemen oplossen met](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) voor meer informatie.
* Probeert te krijgen tot ADLS Gen2 Blobs wanneer u zich achter een proxy mislukken.
* Loskoppelen van een resource koppelen via SAS-URI, zoals een blob-container kan een fout veroorzaken waarmee wordt voorkomen dat andere bijlagen correct weergegeven. U kunt dit probleem omzeilen, vernieuwt u het knooppunt voor de. Zie #537 voor meer informatie.
* Als u Visual Studio voor Mac en ooit een aangepaste AAD-configuratie hebt gemaakt, is het wellicht niet mogelijk om aan te melden. Verwijder de inhoud van het probleem te omzeilen, ~ /. IdentityService/AadConfigurations. Als u in dat geval heeft niet de blokkering opheffen u, als opmerking bij dit probleem.
* Azurite is nog niet volledig geïmplementeerd alle Storage-API's. Als gevolg hiervan kunnen er onverwachte fouten of problemen bij het gebruik van Azurite voor opslag.
* In zeldzame gevallen kan de focus structuur ophalen vastgelopen op Snelweergavetoegang. Registreer de focus, kunt u Alles vernieuwen.
* Uploaden vanaf uw OneDrive-map werkt niet vanwege een fout in NodeJS. De fout is opgelost, maar nog niet is geïntegreerd in Electron. Als tijdelijke oplossing voor dit probleem tijdens het uploaden naar of downloaden van een blob-container, kunt u de experimentele functie voor AzCopy.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter omzeilen hier beschreven.
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Azure Stack biedt geen ondersteuning voor de volgende functies. Poging tot het gebruiken van deze functies tijdens het werken met Azure Stack kunnen resources leiden tot onverwachte fouten.
   * Bestandsshares
   * Toegangslagen
   * Voorlopig verwijderen
   * ADLS Gen2
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Storage Explorer op Linux uitgevoerd, moet bepaalde afhankelijkheden moeten eerst worden geïnstalleerd. Controleer de Storage Explorer [problemen oplossen met](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#linux-dependencies) voor meer informatie.

## <a name="version-180"></a>Versie 1.8.0
5/1/2019

### <a name="new"></a>Nieuw

* De geïntegreerde AzCopy-versie is bijgewerkt naar versie 10.1.0.
* CTRL / Cmd + R kan nu worden gebruikt voor het vernieuwen van de momenteel gerichte editor. [#1097](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1097)
* De API van Azure Stack-Storage-versie is gewijzigd in 17-04-2017.
* Het dialoogvenster toegang beheren voor ADLS Gen2 wordt nu het masker synchroon houden op een manier die vergelijkbaar is met andere hulpprogramma's voor POSIX-machtigingen. De gebruikersinterface, waarschuwt u als een wijziging wordt doorgevoerd waarmee zorgt ervoor dat de machtigingen van een gebruiker of groep overschrijdt de grenzen van het masker. [#1253](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1253)
* De vlag voor het berekenen en instellen van de MD5-hash is nu ingeschakeld voor de AzCopy-uploads. [#1223](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1223)


### <a name="preview-features"></a>Preview-functies

* Apparaat code flow aanmelden is nu beschikbaar om te bekijken. Als u wilt inschakelen, gaat u naar 'Preview' → 'Gebruik apparaat Code Flow aanmelden'. U wordt aangeraden alle gebruikers die hebben gehad problemen met lege aanmelding bij windows om te proberen deze functie, zoals het blijken te zijn van een meer betrouwbare vorm van aanmelding.
* Storage Explorer is geïntegreerd met AzCopy is momenteel beschikbaar voor preview. Als u wilt inschakelen, gaat u naar 'Preview' → 'Met AzCopy voor verbeterde Blob uploaden en downloaden'. BLOB-overdracht is voltooid met AzCopy moeten worden snellere en beter.

### <a name="fixes"></a>Oplossingen

* Het dialoogvenster toegangsbeleid stelt u een vervaldatum niet meer op toegang-beleid voor opslag die nog geen een vervaldatum. [#764](https://www.github.com/Microsoft/AzureStorageExplorer/issues/764)
* Sommige wijzigingen zijn aangebracht aan het genereren van SAS-dialoogvenster om te controleren of dat opgeslagen toegangsbeleid correct worden gebruikt bij het genereren van een SAS. [#1269](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1269)
* Wanneer u probeert te uploaden van een niet - 512 byte uitgelijnd bestand naar een pagina-Blob, wordt nu een meer relevante fout weergegeven in Storage Explorer. [#1050](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1050)
* Kopiëren van een Blob-container die u een weergavenaam gebruikt, mislukken. Nu wordt de werkelijke naam van de Blob-container gebruikt. [#1166](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1166)
* Er wordt geprobeerd bepaalde acties uitvoeren op een ADLS-Gen2 map waarin het unicode-tekens in de naam, mislukken. Alle bewerkingen moeten nu worden uitgevoerd. [#980](https://www.github.com/Microsoft/AzureStorageExplorer/issues/980)

### <a name="known-issues"></a>Bekende problemen

* Bij het uitvoeren van een niet - AzCopy Blob downloaden, wordt de MD5 voor grote bestanden niet wordt gecontroleerd. Dit is vanwege een fout in de Storage-SDK. [#1212](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1212)
* Als u met RBAC kunt moet Storage Explorer enkele management layer-machtigingen voor toegang tot uw opslagresources. Zie de [problemen oplossen met](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) voor meer informatie.
* Probeert te krijgen tot ADLS Gen2 Blobs wanneer u zich achter een proxy mislukken.
* Loskoppelen van een resource koppelen via SAS-URI, zoals een blob-container kan een fout veroorzaken waarmee wordt voorkomen dat andere bijlagen correct weergegeven. U kunt dit probleem omzeilen, vernieuwt u het knooppunt voor de. Zie #537 voor meer informatie.
* Als u Visual Studio voor Mac en ooit een aangepaste AAD-configuratie hebt gemaakt, is het wellicht niet mogelijk om aan te melden. Verwijder de inhoud van het probleem te omzeilen, ~ /. IdentityService/AadConfigurations. Als u in dat geval heeft niet de blokkering opheffen u, als opmerking bij dit probleem.
* Azurite is nog niet volledig geïmplementeerd alle Storage-API's. Als gevolg hiervan kunnen er onverwachte fouten of problemen bij het gebruik van Azurite voor opslag.
* In zeldzame gevallen kan de focus structuur ophalen vastgelopen op Snelweergavetoegang. Registreer de focus, kunt u Alles vernieuwen.
* Uploaden vanaf uw OneDrive-map werkt niet vanwege een fout in NodeJS. De fout is opgelost, maar nog niet is geïntegreerd in Electron. Als tijdelijke oplossing voor dit probleem tijdens het uploaden naar of downloaden van een blob-container, kunt u de experimentele functie voor AzCopy.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter omzeilen hier beschreven.
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Azure Stack biedt geen ondersteuning voor de volgende functies. Poging tot het gebruiken van deze functies tijdens het werken met Azure Stack kunnen resources leiden tot onverwachte fouten.
   * Bestandsshares
   * Toegangslagen
   * Voorlopig verwijderen
   * ADLS Gen2
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Storage Explorer op Linux uitgevoerd, moet bepaalde afhankelijkheden moeten eerst worden geïnstalleerd. Controleer de Storage Explorer [problemen oplossen met](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting?tabs=1804#linux-dependencies) voor meer informatie.

## <a name="version-170"></a>Versie 1.7.0
3/5/2019

### <a name="download-azure-storage-explorer-170"></a>Download Azure Storage Explorer 1.7.0
- [Azure Storage Explorer 1.7.0 voor Windows](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Azure Storage Explorer 1.7.0 voor Mac](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Azure Storage Explorer 1.7.0 voor Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>Nieuw

* U kunt nu de eigenaar en de groep die eigenaar is bij het beheren van toegang voor een ADLS-Gen2 container, bestand of map wijzigen.
* Op Windows is Opslagverkenner van updates binnen het product nu een incrementele installeren. Dit moet resulteren in een snellere update-ervaring. Als u liever een schone installatie, dan kunt u downloaden de [installatieprogramma](https://azure.microsoft.com/features/storage-explorer/) zelf en installeer vervolgens handmatig. #1089

### <a name="preview-features"></a>Preview-functies

* Apparaat code flow aanmelden is nu beschikbaar om te bekijken. Als u wilt inschakelen, gaat u naar 'Preview' → 'Gebruik apparaat Code Flow aanmelden'. U wordt aangeraden alle gebruikers die hebben gehad problemen met lege aanmelding bij windows om te proberen deze functie, zoals het blijken te zijn van een meer betrouwbare vorm van aanmelding. #938
* Storage Explorer is geïntegreerd met AzCopy is momenteel beschikbaar voor preview. Als u wilt inschakelen, gaat u naar 'Preview' → 'Met AzCopy voor verbeterde Blob uploaden en downloaden'. BLOB-overdracht is voltooid met AzCopy moeten worden snellere en beter.

### <a name="fixes"></a>Oplossingen

* U kunt nu de blob-type dat die u uploaden wilt als wanneer AzCopy is ingeschakeld. #1111
* Eerder, als u had ingeschakeld statische websites voor een ADLS Gen2 Storage-account en vervolgens gekoppeld met de naam en sleutel, Storage Explorer niet hebben gedetecteerd dat die hiërarchische naamruimte is ingeschakeld. Dit probleem is opgelost. #1081
* In de blob-editor is op status of retentiedagen resterende sorteren verbroken. Dit probleem is opgelost. #1106
* Na 1.5.0, Storage Explorer niet meer wachtte op server side kopieën te voltooien voordat u voltooid melden tijdens een hernoemen of kopiëren en plakken. Dit probleem is opgelost. #976
* Wanneer u de experimentele AzCopy-functie, is de opdracht gekopieerd nadat u hebt geklikt 'Opdracht naar Klembord kopiëren' niet altijd uitvoerbaar op zichzelf. Nu worden alle opdrachten die nodig zijn voor het handmatig uitvoeren van de overdracht gekopieerd. #1079
* Eerder waren ADLS Gen2 blobs niet toegankelijk als u zich achter een proxy bevindt. Dit is vanwege een fout in een nieuwe VPN-bibliotheek die worden gebruikt door de Storage-SDK. Er is een poging om dit probleem te verhelpen gedaan in 1.7.0, maar sommige mensen mogelijk blijven om te zien van problemen. Een volledige oplossing wordt uitgebracht in een toekomstige update. #1090
* In 1.7.0, het opslaan in het bestandsdialoogvenster nu juist onthoudt de laatste locatie die u hebt een bestand opgeslagen. #16
* In het deelvenster Eigenschappen is de SKU-laag van een Storage-account wordt weergegeven als de aard van het account. Dit probleem is opgelost. #654
* Het is soms onmogelijk om de lease van een blob te verbreken, zelfs als u de naam van de blob juist hebt ingevoerd. Dit probleem is opgelost. #1070

### <a name="known-issues"></a>Bekende problemen

* Als u met RBAC kunt moet Storage Explorer enkele management layer-machtigingen voor toegang tot uw opslagresources. Zie de [problemen oplossen met](https://docs.microsoft.com/azure/storage/common/storage-explorer-troubleshooting) voor meer informatie.
* Probeert te krijgen tot ADLS Gen2 Blobs wanneer u zich achter een proxy mislukken.
* Loskoppelen van een resource koppelen via SAS-URI, zoals een blob-container kan een fout veroorzaken waarmee wordt voorkomen dat andere bijlagen correct weergegeven. U kunt dit probleem omzeilen, vernieuwt u het knooppunt voor de. Zie #537 voor meer informatie.
* Loskoppelen van een resource koppelen via SAS-URI, zoals een blob-container kan een fout veroorzaken waarmee wordt voorkomen dat andere bijlagen correct weergegeven. U kunt dit probleem omzeilen, vernieuwt u het knooppunt voor de. Zie voor meer informatie, #537.
* Als u Visual Studio voor Mac en ooit een aangepaste AAD-configuratie hebt gemaakt, is het wellicht niet mogelijk om aan te melden. Verwijder de inhoud van het probleem te omzeilen, ~ /. IdentityService/AadConfigurations. Als u in dat geval heeft niet de blokkering opheffen u, als opmerking bij dit probleem.
* Azurite is nog niet volledig geïmplementeerd alle Storage-API's. Als gevolg hiervan kunnen er onverwachte fouten of problemen bij het gebruik van Azurite voor opslag.
* In zeldzame gevallen kan de focus structuur ophalen vastgelopen op Snelweergavetoegang. Registreer de focus, kunt u Alles vernieuwen.
* Uploaden vanaf uw OneDrive-map werkt niet vanwege een fout in NodeJS. De fout is opgelost, maar nog niet is geïntegreerd in Electron. Als tijdelijke oplossing voor dit probleem tijdens het uploaden naar of downloaden van een blob-container, kunt u de experimentele functie voor AzCopy.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter omzeilen hier beschreven.
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Azure Stack biedt geen ondersteuning voor de volgende functies. Poging tot het gebruiken van deze functies tijdens het werken met Azure Stack kunnen resources leiden tot onverwachte fouten.
   * Bestandsshares
   * Toegangslagen
   * Voorlopig verwijderen
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Voor Linux-gebruikers, moet u voor het installeren van [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-162"></a>Versie 1.6.2
1/9/2019

### <a name="hotfixes"></a>Hotfixes
* In 1.6.1, zijn altijd toegevoegd aan ADLS Gen2 ACL's via de object-id entiteiten die gebruikers niet zijn toegevoegd als groepen. Alleen groepen worden nu als groepen en entiteiten zoals bedrijfstoepassingen andService die beveiligings-Principals zijn toegevoegd als gebruikers toegevoegd. [#1049](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1049)
* Als een ADLS Gen2 Storage-account geen containers heeft en is gekoppeld met de naam en sleutel, zou klikt u vervolgens Storage Explorer niet detecteren of het Opslagaccount ADLS Gen2 is. Dit probleem is opgelost. [#1048](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1048)
* In 1.6.0, zou de conflicten tijdens het kopiëren en plakken niet gevraagd om een oplossing. In plaats daarvan toch de conflicterende kopie al geen Connection. Nu op het eerste conflict wordt u gevraagd hoe u wilt dat deze wordt worden opgelost. [#1014](https://www.github.com/Microsoft/AzureStorageExplorer/issues/1014)
* Vanwege de API-beperkingen, alle validatietests van objectid's in het dialoogvenster Manage Access zijn uitgeschakeld. Validatie wordt nu alleen uitgevoerd voor de gebruiker UPN's. [#954](https://www.github.com/Microsoft/AzureStorageExplorer/issues/954)
* De machtigingen voor een groep kunnen niet worden gewijzigd in het dialoogvenster toegang tot het beheren van ADLS-Gen2. Dit probleem is opgelost. [#958](https://www.github.com/Microsoft/AzureStorageExplorer/issues/958)
* Toegevoegde slepen en neerzetten uploaden ondersteuning in de editor ADLS Gen2. [#953](https://www.github.com/Microsoft/AzureStorageExplorer/issues/953)
* De URL-eigenschap in het dialoogvenster Eigenschappen voor ADLS Gen2 bestanden en mappen is soms ontbreekt een slash (/). Dit probleem is opgelost. [#960](https://www.github.com/Microsoft/AzureStorageExplorer/issues/960)
* Als de huidige machtigingen voor een container ADLS Gen2, bestand of map is mislukt, en vervolgens de fout is nu propertly weergegeven in het activiteitenlogboek. [#965](https://www.github.com/Microsoft/AzureStorageExplorer/issues/965)
* De tijdelijke pad dat is gemaakt voor het openen van bestanden is ingekort om te verminderen de kans dat het maken van een pad dat langer is dan de waarde van MAX_PATH op Windows. [#93](https://www.github.com/Microsoft/AzureStorageExplorer/issues/93)
* Het dialoogvenster verbinding maken met nu correct wordt weergegeven wanneer er geen aangemelde gebruikers zijn en er zijn geen resources zijn gekoppeld. [#944](https://www.github.com/Microsoft/AzureStorageExplorer/issues/944)
* In 1.6.0, wilt opslaan van eigenschappen voor HNS Blobs en bestanden coderen van de waarde van elke eigenschap. Dit leidde tot onnodige codering van waarden die het bevat alleen ASCII-tekens. Waarden wordt nu alleen worden gecodeerd als ze niet-ASCII-tekens bevatten. [#986](https://www.github.com/Microsoft/AzureStorageExplorer/issues/986)
* Het uploaden van een map naar een niet - HNS Blob-container, mislukken als er een SAS gebruikt is en de SAS is geen machtigingen voor lezen. Dit probleem is opgelost. [#970](https://www.github.com/Microsoft/AzureStorageExplorer/issues/970)
* Annuleren van een AzCopy-overdracht werkt niet. Dit probleem is opgelost. [#943](https://www.github.com/Microsoft/AzureStorageExplorer/issues/943)
* AzCopy mislukt bij het downloaden van een map van een ADLS Gen2 Blob-container als de map spaties in de naam ervan. Dit probleem is opgelost. [#990](https://www.github.com/Microsoft/AzureStorageExplorer/issues/990)
* De CosmosDB-editor is onderverdeeld in 1.6.0. Het is nu opgelost. [#950](https://www.github.com/Microsoft/AzureStorageExplorer/issues/950)
        
### <a name="new"></a>Nieuw

* U kunt Storage Explorer nu gebruiken voor toegang tot uw Blob-gegevens via [RBAC](https://go.microsoft.com/fwlink/?linkid=2045904&clcid=0x409). Als u bent aangemeld en Storage Explorer kan niet worden opgehaald van de sleutels voor uw Storage-account is, wordt een OAuth-token worden gebruikt om te verifiëren bij interactie met uw gegevens.
* Storage Explorer biedt nu ondersteuning voor opslagaccounts van ADLS Gen2. Wanneer u Storage Explorer detecteert die hiërarchische naamruimte is ingeschakeld voor een opslagaccount, wordt u Zie '(ADLS Gen2 Preview)' naast de naam van uw opslagaccount. Storage Explorer kan detecteren of er hiërarchische naamruimte is ingeschakeld wanneer u bent aangemeld, of als u uw Storage-Account met de naam en sleutel hebt gekoppeld. U kunt Storage Explorer om te gebruiken voor ADLS Gen2 Storage-accounts:
  * Maken en verwijderen van containers
  * Eigenschappen van de container en machtigingen (linkerkant) beheren
  * Weergeven en gegevens binnen containers gaan
  * Nieuwe mappen maken
  * Uploaden, downloaden, wijzigen en verwijderen van bestanden en mappen
  * Eigenschappen van bestanden en mappen en machtigingen (rechterkant) beheren.
    
    Andere standaard Blob-functies, zoals voorlopig verwijderen en momentopnamen, zijn momenteel niet beschikbaar. Beheer van machtigingen is ook alleen beschikbaar wanneer is aangemeld. Bovendien, als u werkt in een ADLS Gen2 Storage-account, wordt Storage Explorer AzCopy gebruiken voor alle uploads, downloads en maken standaard gebruik van de naam en sleutel referenties voor alle bewerkingen, indien beschikbaar.
* Na sterke gebruikersfeedback, kan de lease break opnieuw leases in één keer op meerdere blobs verbreken worden gebruikt.

### <a name="known-issues"></a>Bekende problemen

* Bij het downloaden van een ADLS Gen2 Storage-account, als een van de bestanden die worden overgebracht al bestaat, wordt soms AzCopy vastlopen. Dit wordt opgelost in een toekomstige hotfix.
* Loskoppelen van een resource koppelen via SAS-URI, zoals een blob-container kan een fout veroorzaken waarmee wordt voorkomen dat andere bijlagen correct weergegeven. U kunt dit probleem omzeilen, vernieuwt u het knooppunt voor de. Zie voor meer informatie, #537.
* Als u Visual Studio voor Mac en ooit een aangepaste AAD-configuratie hebt gemaakt, is het wellicht niet mogelijk om aan te melden. Verwijder de inhoud van het probleem te omzeilen, ~ /. IdentityService/AadConfigurations. Als u in dat geval heeft niet de blokkering opheffen u, als opmerking bij dit probleem.
* Azurite is nog niet volledig geïmplementeerd alle Storage-API's. Als gevolg hiervan kunnen er onverwachte fouten of problemen bij het gebruik van Azurite voor opslag.
* In zeldzame gevallen kan de focus structuur ophalen vastgelopen op Snelweergavetoegang. Registreer de focus, kunt u Alles vernieuwen.
* Uploaden vanaf uw OneDrive-map werkt niet vanwege een fout in NodeJS. De fout is opgelost, maar nog niet is geïntegreerd in Electron. Als tijdelijke oplossing voor dit probleem tijdens het uploaden naar of downloaden van een blob-container, kunt u de experimentele functie voor AzCopy.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter omzeilen hier beschreven.
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Azure Stack biedt geen ondersteuning voor de volgende functies. Poging tot het gebruiken van deze functies tijdens het werken met Azure Stack kunnen resources leiden tot onverwachte fouten.
   * Bestandsshares
   * Toegangslagen
   * Voorlopig verwijderen
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Voor Linux-gebruikers, moet u voor het installeren van [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-161"></a>Versie 1.6.1
12/18/2018

### <a name="hotfixes"></a>Hotfixes
* Vanwege de API-beperkingen, alle validatietests van objectid's in het dialoogvenster Manage Access zijn uitgeschakeld. Validatie wordt nu alleen uitgevoerd voor de gebruiker UPN's. [#954](https://www.github.com/Microsoft/AzureStorageExplorer/issues/954)
* De machtigingen voor een groep kunnen niet worden gewijzigd in het dialoogvenster toegang tot het beheren van ADLS-Gen2. Dit probleem is opgelost. [#958](https://www.github.com/Microsoft/AzureStorageExplorer/issues/958)
* Toegevoegde slepen en neerzetten uploaden ondersteuning in de editor ADLS Gen2. [#953](https://www.github.com/Microsoft/AzureStorageExplorer/issues/953)
* De URL-eigenschap in het dialoogvenster Eigenschappen voor ADLS Gen2 bestanden en mappen is soms ontbreekt een slash (/). Dit probleem is opgelost. [#960](https://www.github.com/Microsoft/AzureStorageExplorer/issues/960)
* Als de huidige machtigingen voor een container ADLS Gen2, bestand of map is mislukt, en vervolgens de fout is nu propertly weergegeven in het activiteitenlogboek. [#965](https://www.github.com/Microsoft/AzureStorageExplorer/issues/965)
* De tijdelijke pad dat is gemaakt voor het openen van bestanden is ingekort om te verminderen de kans dat het maken van een pad dat langer is dan de waarde van MAX_PATH op Windows. [#93](https://www.github.com/Microsoft/AzureStorageExplorer/issues/93)
* Het dialoogvenster verbinding maken met nu correct wordt weergegeven wanneer er geen aangemelde gebruikers zijn en er zijn geen resources zijn gekoppeld. [#944](https://www.github.com/Microsoft/AzureStorageExplorer/issues/944)
* In 1.6.0, wilt opslaan van eigenschappen voor HNS Blobs en bestanden coderen van de waarde van elke eigenschap. Dit leidde tot onnodige codering van waarden die het bevat alleen ASCII-tekens. Waarden wordt nu alleen worden gecodeerd als ze niet-ASCII-tekens bevatten. [#986](https://www.github.com/Microsoft/AzureStorageExplorer/issues/986)
* Het uploaden van een map naar een niet - HNS Blob-container, mislukken als er een SAS gebruikt is en de SAS is geen machtigingen voor lezen. Dit probleem is opgelost. [#970](https://www.github.com/Microsoft/AzureStorageExplorer/issues/970)
* Annuleren van een AzCopy-overdracht werkt niet. Dit probleem is opgelost. [#943](https://www.github.com/Microsoft/AzureStorageExplorer/issues/943)
* AzCopy mislukt bij het downloaden van een map van een ADLS Gen2 Blob-container als de map spaties in de naam ervan. Dit probleem is opgelost. [#990](https://www.github.com/Microsoft/AzureStorageExplorer/issues/990)
* De CosmosDB-editor is onderverdeeld in 1.6.0. Het is nu opgelost. [#950](https://www.github.com/Microsoft/AzureStorageExplorer/issues/950)
        
### <a name="new"></a>Nieuw

* U kunt Storage Explorer nu gebruiken voor toegang tot uw Blob-gegevens via [RBAC](https://go.microsoft.com/fwlink/?linkid=2045904&clcid=0x409). Als u bent aangemeld en Storage Explorer kan niet worden opgehaald van de sleutels voor uw Storage-account is, wordt een OAuth-token worden gebruikt om te verifiëren bij interactie met uw gegevens.
* Storage Explorer biedt nu ondersteuning voor opslagaccounts van ADLS Gen2. Wanneer u Storage Explorer detecteert die hiërarchische naamruimte is ingeschakeld voor een opslagaccount, wordt u Zie '(ADLS Gen2 Preview)' naast de naam van uw opslagaccount. Storage Explorer kan detecteren of er hiërarchische naamruimte is ingeschakeld wanneer u bent aangemeld, of als u uw Storage-Account met de naam en sleutel hebt gekoppeld. U kunt Storage Explorer om te gebruiken voor ADLS Gen2 Storage-accounts:
  * Maken en verwijderen van containers
  * Eigenschappen van de container en machtigingen (linkerkant) beheren
  * Weergeven en gegevens binnen containers gaan
  * Nieuwe mappen maken
  * Uploaden, downloaden, wijzigen en verwijderen van bestanden en mappen
  * Eigenschappen van bestanden en mappen en machtigingen (rechterkant) beheren.
    
    Andere standaard Blob-functies, zoals voorlopig verwijderen en momentopnamen, zijn momenteel niet beschikbaar. Beheer van machtigingen is ook alleen beschikbaar wanneer is aangemeld. Bovendien, als u werkt in een ADLS Gen2 Storage-account, wordt Storage Explorer AzCopy gebruiken voor alle uploads, downloads en maken standaard gebruik van de naam en sleutel referenties voor alle bewerkingen, indien beschikbaar.
* Na sterke gebruikersfeedback, kan de lease break opnieuw leases in één keer op meerdere blobs verbreken worden gebruikt.

### <a name="known-issues"></a>Bekende problemen

* Bij het downloaden van een ADLS Gen2 Storage-account, als een van de bestanden die worden overgebracht al bestaat, wordt soms AzCopy vastlopen. Dit wordt opgelost in een toekomstige hotfix.
* Loskoppelen van een resource koppelen via SAS-URI, zoals een blob-container kan een fout veroorzaken waarmee wordt voorkomen dat andere bijlagen correct weergegeven. U kunt dit probleem omzeilen, vernieuwt u het knooppunt voor de. Zie voor meer informatie, #537.
* Als u Visual Studio voor Mac en ooit een aangepaste AAD-configuratie hebt gemaakt, is het wellicht niet mogelijk om aan te melden. Verwijder de inhoud van het probleem te omzeilen, ~ /. IdentityService/AadConfigurations. Als u in dat geval heeft niet de blokkering opheffen u, als opmerking bij dit probleem.
* Azurite is nog niet volledig geïmplementeerd alle Storage-API's. Als gevolg hiervan kunnen er onverwachte fouten of problemen bij het gebruik van Azurite voor opslag.
* In zeldzame gevallen kan de focus structuur ophalen vastgelopen op Snelweergavetoegang. Registreer de focus, kunt u Alles vernieuwen.
* Uploaden vanaf uw OneDrive-map werkt niet vanwege een fout in NodeJS. De fout is opgelost, maar nog niet is geïntegreerd in Electron. Als tijdelijke oplossing voor dit probleem tijdens het uploaden naar of downloaden van een blob-container, kunt u de experimentele functie voor AzCopy.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter omzeilen hier beschreven.
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Azure Stack biedt geen ondersteuning voor de volgende functies. Poging tot het gebruiken van deze functies tijdens het werken met Azure Stack kunnen resources leiden tot onverwachte fouten.
   * Bestandsshares
   * Toegangslagen
   * Voorlopig verwijderen
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Voor Linux-gebruikers, moet u voor het installeren van [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-160"></a>Versie 1.6.0
12/5/2018

### <a name="new"></a>Nieuw

* U kunt Storage Explorer nu gebruiken voor toegang tot uw Blob-gegevens via [RBAC](https://go.microsoft.com/fwlink/?linkid=2045904&clcid=0x409). Als u bent aangemeld en Storage Explorer kan niet worden opgehaald van de sleutels voor uw Storage-account is, wordt een OAuth-token worden gebruikt om te verifiëren bij interactie met uw gegevens.
* Storage Explorer biedt nu ondersteuning voor opslagaccounts van ADLS Gen2. Wanneer u Storage Explorer detecteert die hiërarchische naamruimte is ingeschakeld voor een opslagaccount, wordt u Zie '(ADLS Gen2 Preview)' naast de naam van uw opslagaccount. Storage Explorer kan detecteren of er hiërarchische naamruimte is ingeschakeld wanneer u bent aangemeld, of als u uw Storage-Account met de naam en sleutel hebt gekoppeld. U kunt Storage Explorer om te gebruiken voor ADLS Gen2 Storage-accounts:
  * Maken en verwijderen van containers
  * Eigenschappen van de container en machtigingen (linkerkant) beheren
  * Weergeven en gegevens binnen containers gaan
  * Nieuwe mappen maken
  * Uploaden, downloaden, wijzigen en verwijderen van bestanden en mappen
  * Eigenschappen van bestanden en mappen en machtigingen (rechterkant) beheren.
    
    Andere standaard Blob-functies, zoals voorlopig verwijderen en momentopnamen, zijn momenteel niet beschikbaar. Beheer van machtigingen is ook alleen beschikbaar wanneer is aangemeld. Bovendien, als u werkt in een ADLS Gen2 Storage-account, wordt Storage Explorer AzCopy gebruiken voor alle uploads, downloads en maken standaard gebruik van de naam en sleutel referenties voor alle bewerkingen, indien beschikbaar.
* Na sterke gebruikersfeedback, kan de lease break opnieuw leases in één keer op meerdere blobs verbreken worden gebruikt.

### <a name="known-issues"></a>Bekende problemen

* Bij het downloaden van een ADLS Gen2 Storage-account, als een van de bestanden die worden overgebracht al bestaat, wordt soms AzCopy vastlopen. Dit wordt opgelost in een toekomstige hotfix.
* Loskoppelen van een resource koppelen via SAS-URI, zoals een blob-container kan een fout veroorzaken waarmee wordt voorkomen dat andere bijlagen correct weergegeven. U kunt dit probleem omzeilen, vernieuwt u het knooppunt voor de. Zie voor meer informatie, #537.
* Als u Visual Studio voor Mac en ooit een aangepaste AAD-configuratie hebt gemaakt, is het wellicht niet mogelijk om aan te melden. Verwijder de inhoud van het probleem te omzeilen, ~ /. IdentityService/AadConfigurations. Als u in dat geval heeft niet de blokkering opheffen u, als opmerking bij dit probleem.
* Azurite is nog niet volledig geïmplementeerd alle Storage-API's. Als gevolg hiervan kunnen er onverwachte fouten of problemen bij het gebruik van Azurite voor opslag.
* In zeldzame gevallen kan de focus structuur ophalen vastgelopen op Snelweergavetoegang. Registreer de focus, kunt u Alles vernieuwen.
* Uploaden vanaf uw OneDrive-map werkt niet vanwege een fout in NodeJS. De fout is opgelost, maar nog niet is geïntegreerd in Electron. Als tijdelijke oplossing voor dit probleem tijdens het uploaden naar of downloaden van een blob-container, kunt u de experimentele functie voor AzCopy.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter omzeilen hier beschreven.
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Azure Stack biedt geen ondersteuning voor de volgende functies. Poging tot het gebruiken van deze functies tijdens het werken met Azure Stack kunnen resources leiden tot onverwachte fouten.
   * Bestandsshares
   * Toegangslagen
   * Voorlopig verwijderen
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Voor Linux-gebruikers, moet u voor het installeren van [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-150"></a>Versie 1.5.0
29-10-2018

### <a name="new"></a>Nieuw

* U kunt nu [AzCopy v10 (Preview)](https://github.com/Azure/azure-storage-azcopy) voor het uploaden en downloaden van Blobs. Deze functie inschakelt gaat u naar het menu 'Experimentele' en klik vervolgens op 'Met AzCopy voor verbeterde Blob uploaden en downloaden'. Wanneer dit is ingeschakeld, wordt u AzCopy gebruikt in de volgende scenario's:
   * Het uploaden van mappen en bestanden naar blob-containers, hetzij via de werkbalk of slepen en neerzetten.
   * Het downloaden van mappen en bestanden, hetzij via het menu van de werkbalk of context.

* Bovendien, als u met behulp van AzCopy:
   * U kunt de AzCopy-opdracht gebruikt voor het uitvoeren van de overdracht naar het Klembord te kopiëren. Klik op 'AzCopy-opdracht naar Klembord kopiëren' in het activiteitenlogboek.
   * U moet de blob-editor handmatig vernieuwen na het uploaden.
   * Uploaden van bestanden voor toevoeg-blobs wordt niet ondersteund, en vhd-bestanden worden geüpload als pagina-blobs en alle andere bestanden worden geüpload als blok-blobs.
   * Fouten en conflicten die tijdens het uploaden of downloaden optreden niet worden weergegeven nadat een uploaden of downloaden is voltooid.

Ten slotte wordt ondersteuning voor het gebruik van AzCopy voor bestandsshares in de toekomst worden binnenkort.
* Electron versie 2.0.11 maakt nu gebruik van Storage Explorer.
* Belangrijke leases kan nu alleen worden uitgevoerd op één blob op een tijdstip. U moet bovendien de naam van de blob waarvan u zijn belangrijke lease invoeren. Deze wijziging is aangebracht om te verminderen de kans op belangrijke per ongeluk een lease, met name voor virtuele machines. #394
* Als u ooit aanmelden problemen ondervindt, kunt u nu proberen opnieuw instellen van verificatie. Ga naar het menu 'Help' en klik op "Herstellen" voor toegang tot deze mogelijkheid. #419

### <a name="fix"></a>Oplossen

* Na de sterke gebruikersfeedback is het knooppunt van de emulator standaard opnieuw worden ingeschakeld. U kunt nog steeds extra emulator verbindingen via het dialoogvenster verbinding maken met toevoegen, maar als de emulator is geconfigureerd voor het gebruik van de standaard-poorten kunt u ook het knooppunt 'Emulator * standaardpoorten' onder ' Lokaal en gekoppeld/Storage-Accounts' gebruiken. #669
* Storage Explorer kunt niet meer u de waarden van de blob-metagegevens waarvoor voorloopspatie of afsluitende spatie instellen. #760
* De knop 'Aanmelden' is altijd ingeschakeld op dezelfde pagina's van het dialoogvenster verbinding maken. Het is nu uitgeschakeld wanneer dat nodig. #761
* Snelle toegang wordt niet langer een fout gegenereerd in de console als er geen items voor Snelweergavetoegang hebt toegevoegd.

### <a name="known-issues"></a>Bekende problemen

* Loskoppelen van een resource koppelen via SAS-URI, zoals een blob-container kan een fout veroorzaken waarmee wordt voorkomen dat andere bijlagen correct weergegeven. U kunt dit probleem omzeilen, vernieuwt u het knooppunt voor de. Zie voor meer informatie, #537.
* Als u Visual Studio voor Mac en ooit een aangepaste AAD-configuratie hebt gemaakt, is het wellicht niet mogelijk om aan te melden. Verwijder de inhoud van het probleem te omzeilen, ~ /. IdentityService/AadConfigurations. Als u in dat geval heeft niet de blokkering opheffen u, als opmerking bij dit probleem.
* Azurite is nog niet volledig geïmplementeerd alle Storage-API's. Als gevolg hiervan kunnen er onverwachte fouten of problemen bij het gebruik van Azurite voor opslag.
* In zeldzame gevallen kan de focus structuur ophalen vastgelopen op Snelweergavetoegang. Registreer de focus, kunt u Alles vernieuwen.
* Uploaden vanaf uw OneDrive-map werkt niet vanwege een fout in NodeJS. De fout is opgelost, maar nog niet is geïntegreerd in Electron. Als tijdelijke oplossing voor dit probleem tijdens het uploaden naar of downloaden van een blob-container, kunt u de experimentele functie voor AzCopy.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter omzeilen hier beschreven.
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Azure Stack biedt geen ondersteuning voor de volgende functies. Poging tot het gebruiken van deze functies tijdens het werken met Azure Stack kunnen resources leiden tot onverwachte fouten.
   * Bestandsshares
   * Toegangslagen
   * Voorlopig verwijderen
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Voor Linux-gebruikers, moet u voor het installeren van [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```


## <a name="version-144"></a>Versie 1.4.4
10/15/2018

### <a name="hotfixes"></a>Hotfixes
* De Azure Resource Management API-versie is teruggedraaid als u wilt deblokkeren Azure US Government-gebruikers. [#696](https://github.com/Microsoft/AzureStorageExplorer/issues/696)
* Het laden van kringvelden CSS-animaties te verminderen de hoeveelheid GPU die worden gebruikt door de Storage Explorer nu gebruiken. [#653](https://github.com/Microsoft/AzureStorageExplorer/issues/653)

### <a name="new"></a>Nieuw
* Externe resource-bijlagen, bijvoorbeeld voor SAS-verbindingen en emulatoren, is aanzienlijk verbeterd. U kunt nu het volgende doen:
   * Pas de weergavenaam van de resource die u wilt toevoegen. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Koppelen aan meerdere lokale emulators met behulp van verschillende poorten. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Gekoppelde resources toevoegen aan Snelweergavetoegang. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Storage Explorer biedt nu ondersteuning voor voorlopig verwijderen. U kunt:
   * Configureer een beleid voor voorlopig verwijderen met de rechtermuisknop op het knooppunt van de Blob-Containers voor uw opslagaccount.
   * Voorlopig verwijderde weergeven blobs in de Blob-Editor door te selecteren ' Active en blobs verwijderd ' in de vervolgkeuzelijst naast de navigatiebalk.
   * Voorlopig verwijderde blobs ongedaan.

### <a name="fixes"></a>Oplossingen
* De actie "CORS-instellingen configureren" is niet meer beschikbaar is op Premium Storage-accounts omdat Premium Storage-accounts bieden geen ondersteuning voor CORS. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Er is nu een Shared Access Signature-eigenschap voor SAS-Services die zijn gekoppeld. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* De actie 'Standaard Toegangslaag ingesteld' is nu beschikbaar voor Blob- en GPV2-Storage-accounts die zijn vastgemaakt aan Snelweergavetoegang. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Soms mislukt Storage Explorer om weer te geven van klassieke opslagaccounts. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Bekende problemen
* Bij het gebruik van emulators, zoals Azure Storage-Emulator of Azurite, moet u deze luistert voor verbindingen op hun standaardpoorten. Anders pas Opslagverkenner weer verbinding te maken met.
* Als u Visual Studio voor Mac en ooit een aangepaste AAD-configuratie hebt gemaakt, is het wellicht niet mogelijk om aan te melden. Verwijder de inhoud van het probleem te omzeilen, ~ /. IdentityService/AadConfigurations. Als u in dat geval heeft niet de blokkering opheffen u, stuur een reactie op [dit probleem](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite is nog niet volledig geïmplementeerd alle Storage-API's. Als gevolg hiervan kunnen er onverwachte fouten of problemen bij het gebruik van Azurite voor opslag.
* In zeldzame gevallen kan de focus structuur ophalen vastgelopen op Snelweergavetoegang. Registreer de focus, kunt u Alles vernieuwen.
* Uploaden vanaf uw OneDrive-map werkt niet vanwege een fout in NodeJS. De fout is opgelost, maar nog niet is geïntegreerd in Electron.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter tijdelijke oplossing beschreven [hier](https://github.com/Azure/azure-storage-node/issues/317).
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde storage-account van Azure Stack.
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Voor Linux-gebruikers, moet u voor het installeren van [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-143"></a>Versie 1.4.3
10/11/2018

### <a name="hotfixes"></a>Hotfixes
* De Azure Resource Management API-versie is teruggedraaid als u wilt deblokkeren Azure US Government-gebruikers. [#696](https://github.com/Microsoft/AzureStorageExplorer/issues/696)
* Het laden van kringvelden CSS-animaties te verminderen de hoeveelheid GPU die worden gebruikt door de Storage Explorer nu gebruiken. [#653](https://github.com/Microsoft/AzureStorageExplorer/issues/653)

### <a name="new"></a>Nieuw
* Externe resource-bijlagen, bijvoorbeeld voor SAS-verbindingen en emulatoren, is aanzienlijk verbeterd. U kunt nu het volgende doen:
   * Pas de weergavenaam van de resource die u wilt toevoegen. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Koppelen aan meerdere lokale emulators met behulp van verschillende poorten. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Gekoppelde resources toevoegen aan Snelweergavetoegang. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Storage Explorer biedt nu ondersteuning voor voorlopig verwijderen. U kunt:
   * Configureer een beleid voor voorlopig verwijderen met de rechtermuisknop op het knooppunt van de Blob-Containers voor uw opslagaccount.
   * Voorlopig verwijderde weergeven blobs in de Blob-Editor door te selecteren ' Active en blobs verwijderd ' in de vervolgkeuzelijst naast de navigatiebalk.
   * Voorlopig verwijderde blobs ongedaan.

### <a name="fixes"></a>Oplossingen
* De actie "CORS-instellingen configureren" is niet meer beschikbaar is op Premium Storage-accounts omdat Premium Storage-accounts bieden geen ondersteuning voor CORS. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Er is nu een Shared Access Signature-eigenschap voor SAS-Services die zijn gekoppeld. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* De actie 'Standaard Toegangslaag ingesteld' is nu beschikbaar voor Blob- en GPV2-Storage-accounts die zijn vastgemaakt aan Snelweergavetoegang. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Soms mislukt Storage Explorer om weer te geven van klassieke opslagaccounts. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Bekende problemen
* Bij het gebruik van emulators, zoals Azure Storage-Emulator of Azurite, moet u deze luistert voor verbindingen op hun standaardpoorten. Anders pas Opslagverkenner weer verbinding te maken met.
* Als u Visual Studio voor Mac en ooit een aangepaste AAD-configuratie hebt gemaakt, is het wellicht niet mogelijk om aan te melden. Verwijder de inhoud van het probleem te omzeilen, ~ /. IdentityService/AadConfigurations. Als u in dat geval heeft niet de blokkering opheffen u, stuur een reactie op [dit probleem](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite is nog niet volledig geïmplementeerd alle Storage-API's. Als gevolg hiervan kunnen er onverwachte fouten of problemen bij het gebruik van Azurite voor opslag.
* In zeldzame gevallen kan de focus structuur ophalen vastgelopen op Snelweergavetoegang. Registreer de focus, kunt u Alles vernieuwen.
* Uploaden vanaf uw OneDrive-map werkt niet vanwege een fout in NodeJS. De fout is opgelost, maar nog niet is geïntegreerd in Electron.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter tijdelijke oplossing beschreven [hier](https://github.com/Azure/azure-storage-node/issues/317).
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde storage-account van Azure Stack.
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Voor Linux-gebruikers, moet u voor het installeren van [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-142"></a>Versie 1.4.2
24-09-2018

### <a name="hotfixes"></a>Hotfixes
* Azure Resource Management API-versie bijwerken naar 2018-07-01, het toevoegen van ondersteuning voor nieuwe Azure Storage-Account. [#652](https://github.com/Microsoft/AzureStorageExplorer/issues/652)

### <a name="new"></a>Nieuw
* Externe resource-bijlagen, bijvoorbeeld voor SAS-verbindingen en emulatoren, is aanzienlijk verbeterd. U kunt nu het volgende doen:
   * Pas de weergavenaam van de resource die u wilt toevoegen. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Koppelen aan meerdere lokale emulators met behulp van verschillende poorten. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Gekoppelde resources toevoegen aan Snelweergavetoegang. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Storage Explorer biedt nu ondersteuning voor voorlopig verwijderen. U kunt:
   * Configureer een beleid voor voorlopig verwijderen met de rechtermuisknop op het knooppunt van de Blob-Containers voor uw opslagaccount.
   * Voorlopig verwijderde weergeven blobs in de Blob-Editor door te selecteren ' Active en blobs verwijderd ' in de vervolgkeuzelijst naast de navigatiebalk.
   * Voorlopig verwijderde blobs ongedaan.

### <a name="fixes"></a>Oplossingen
* De actie "CORS-instellingen configureren" is niet meer beschikbaar is op Premium Storage-accounts omdat Premium Storage-accounts bieden geen ondersteuning voor CORS. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Er is nu een Shared Access Signature-eigenschap voor SAS-Services die zijn gekoppeld. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* De actie 'Standaard Toegangslaag ingesteld' is nu beschikbaar voor Blob- en GPV2-Storage-accounts die zijn vastgemaakt aan Snelweergavetoegang. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Soms mislukt Storage Explorer om weer te geven van klassieke opslagaccounts. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Bekende problemen
* Bij het gebruik van emulators, zoals Azure Storage-Emulator of Azurite, moet u deze luistert voor verbindingen op hun standaardpoorten. Anders pas Opslagverkenner weer verbinding te maken met.
* Als u Visual Studio voor Mac en ooit een aangepaste AAD-configuratie hebt gemaakt, is het wellicht niet mogelijk om aan te melden. Verwijder de inhoud van het probleem te omzeilen, ~ /. IdentityService/AadConfigurations. Als u in dat geval heeft niet de blokkering opheffen u, stuur een reactie op [dit probleem](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite is nog niet volledig geïmplementeerd alle Storage-API's. Als gevolg hiervan kunnen er onverwachte fouten of problemen bij het gebruik van Azurite voor opslag.
* In zeldzame gevallen kan de focus structuur ophalen vastgelopen op Snelweergavetoegang. Registreer de focus, kunt u Alles vernieuwen.
* Uploaden vanaf uw OneDrive-map werkt niet vanwege een fout in NodeJS. De fout is opgelost, maar nog niet is geïntegreerd in Electron.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter tijdelijke oplossing beschreven [hier](https://github.com/Azure/azure-storage-node/issues/317).
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde storage-account van Azure Stack.
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Voor Linux-gebruikers, moet u voor het installeren van [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-141"></a>Versie 1.4.1
08/28/2018

### <a name="hotfixes"></a>Hotfixes
* Storage Explorer is op de eerste keer opstarten wordt voor het genereren van de sleutel die wordt gebruikt voor het versleutelen van gevoelige gegevens. Dit kan problemen veroorzaken bij het gebruik van Snelweergavetoegang en het koppelen van resources. [#535](https://github.com/Microsoft/AzureStorageExplorer/issues/535)
* Als uw account geen MFA voor de starttenant vereist heeft, maar voor sommige andere tenants heeft, zou Opslagverkenner kan geen lijst met abonnementen zijn. Nu, na het aanmelden met een dergelijk account, Storage Explorer u wordt gevraagd uw referenties opnieuw invoeren en uitvoeren van MFA. [#74](https://github.com/Microsoft/AzureStorageExplorer/issues/74)
* Storage Explorer kon geen resources van Azure Duitsland en Azure US Government koppelen. [#572](https://github.com/Microsoft/AzureStorageExplorer/issues/572)
* Als u zich aangemeld bij twee accounts die waren van hetzelfde e-mailadres, mislukken Opslagverkenner soms om weer te geven van uw resources in de structuurweergave wordt weergegeven. [#580](https://github.com/Microsoft/AzureStorageExplorer/issues/580)
* Op Windows-machines, langzamer, zou het beginscherm soms duren voordat een aanzienlijke hoeveelheid tijd om te worden weergegeven. [#586](https://github.com/Microsoft/AzureStorageExplorer/issues/586)
* Het dialoogvenster verbinding maken wordt weergegeven, zelfs als er gekoppelde accounts of -services. [#588](https://github.com/Microsoft/AzureStorageExplorer/issues/588)

### <a name="new"></a>Nieuw
* Externe resource-bijlagen, bijvoorbeeld voor SAS-verbindingen en emulatoren, is aanzienlijk verbeterd. U kunt nu het volgende doen:
   * Pas de weergavenaam van de resource die u wilt toevoegen. [#31](https://github.com/Microsoft/AzureStorageExplorer/issues/31)
   * Koppelen aan meerdere lokale emulators met behulp van verschillende poorten. [#193](https://github.com/Microsoft/AzureStorageExplorer/issues/193)
   * Gekoppelde resources toevoegen aan Snelweergavetoegang. [#392](https://github.com/Microsoft/AzureStorageExplorer/issues/392)
* Storage Explorer biedt nu ondersteuning voor voorlopig verwijderen. U kunt:
   * Configureer een beleid voor voorlopig verwijderen met de rechtermuisknop op het knooppunt van de Blob-Containers voor uw opslagaccount.
   * Voorlopig verwijderde weergeven blobs in de Blob-Editor door te selecteren ' Active en blobs verwijderd ' in de vervolgkeuzelijst naast de navigatiebalk.
   * Voorlopig verwijderde blobs ongedaan.

### <a name="fixes"></a>Oplossingen
* De actie "CORS-instellingen configureren" is niet meer beschikbaar is op Premium Storage-accounts omdat Premium Storage-accounts bieden geen ondersteuning voor CORS. [#142](https://github.com/Microsoft/AzureStorageExplorer/issues/142)
* Er is nu een Shared Access Signature-eigenschap voor SAS-Services die zijn gekoppeld. [#184](https://github.com/Microsoft/AzureStorageExplorer/issues/184)
* De actie 'Standaard Toegangslaag ingesteld' is nu beschikbaar voor Blob- en GPV2-Storage-accounts die zijn vastgemaakt aan Snelweergavetoegang. [#229](https://github.com/Microsoft/AzureStorageExplorer/issues/229)
* Soms mislukt Storage Explorer om weer te geven van klassieke opslagaccounts. [#323](https://github.com/Microsoft/AzureStorageExplorer/issues/323)

### <a name="known-issues"></a>Bekende problemen
* Bij het gebruik van emulators, zoals Azure Storage-Emulator of Azurite, moet u deze luistert voor verbindingen op hun standaardpoorten. Anders pas Opslagverkenner weer verbinding te maken met.
* Als u Visual Studio voor Mac en ooit een aangepaste AAD-configuratie hebt gemaakt, is het wellicht niet mogelijk om aan te melden. Verwijder de inhoud van het probleem te omzeilen, ~ /. IdentityService/AadConfigurations. Als u in dat geval heeft niet de blokkering opheffen u, stuur een reactie op [dit probleem](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite is nog niet volledig geïmplementeerd alle Storage-API's. Als gevolg hiervan kunnen er onverwachte fouten of problemen bij het gebruik van Azurite voor opslag.
* In zeldzame gevallen kan de focus structuur ophalen vastgelopen op Snelweergavetoegang. Registreer de focus, kunt u Alles vernieuwen.
* Uploaden vanaf uw OneDrive-map werkt niet vanwege een fout in NodeJS. De fout is opgelost, maar nog niet is geïntegreerd in Electron.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter tijdelijke oplossing beschreven [hier](https://github.com/Azure/azure-storage-node/issues/317).
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde storage-account van Azure Stack.
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Voor Linux-gebruikers, moet u voor het installeren van [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-130"></a>Versie 1.3.0
07/09/2018

### <a name="new"></a>Nieuw
* Toegang tot de $web containers die worden gebruikt door statische Websites wordt nu ondersteund. Hiermee kunt u eenvoudig uploaden en beheren van bestanden en mappen die worden gebruikt door uw website. [#223](https://github.com/Microsoft/AzureStorageExplorer/issues/223)
* De app-balk op macOS is opnieuw zijn gerangschikt. Wijzigingen omvatten een bestandsmenu, enkele belangrijke wijzigingen van de snelkoppeling en diverse nieuwe opdrachten onder het menu van de app. [#99](https://github.com/Microsoft/AzureStorageExplorer/issues/99)
* Het eindpunt van de instantie voor het aanmelden bij Azure US Government is gewijzigd in https://login.microsoftonline.us/
* Toegankelijkheid: Wanneer een schermlezer actief is, werkt nu toetsenbordnavigatie met tabellen die worden gebruikt voor het weergeven van items aan de rechterkant. U kunt de pijltoetsen om te navigeren rijen en kolommen, Enter om aan te roepen standaardacties, de sleutel van de context menu te openen om het contextmenu voor een item en Shift of besturingselement multiselect gebruiken. [#103](https://github.com/Microsoft/AzureStorageExplorer/issues/103)

### <a name="fixes"></a>Oplossingen
*  Op sommige computers zijn onderliggende processen lang duurt om te starten. Wanneer dit gebeurt, is een fout 'onderliggend proces kan niet tijdig starten' wordt weergegeven. De tijd die is toegewezen voor een onderliggend proces te starten is nu verhoogd van 20 tot 90 seconden. Als u nog steeds door dit probleem ondervindt, stuur een reactie op de gekoppelde GitHub-probleem. [#281](https://github.com/Microsoft/AzureStorageExplorer/issues/281)
* Wanneer u een SA's dat heeft geen machtigingen hebt gelezen, was het niet mogelijk een grote blob uploadt. De logica voor het uploaden is gewijzigd om te werken in dit scenario. [#305](https://github.com/Microsoft/AzureStorageExplorer/issues/305)
* Het niveau van de openbare toegang instellen voor een container alle beleidsregels voor toegang verwijdert, en vice versa. Beleidsregels voor openbare toegang en toegang blijven nu behouden bij het instellen van een van de twee. [#197](https://github.com/Microsoft/AzureStorageExplorer/issues/197)
* "AccessTierChangeTime" zijn afgekapt in het dialoogvenster Eigenschappen. Dit probleem is opgelost. [#145](https://github.com/Microsoft/AzureStorageExplorer/issues/145)
* ' Microsoft Azure Storage Explorer-' voorvoegsel ontbreekt in het dialoogvenster Nieuwe map maken. Dit probleem is opgelost. [#299](https://github.com/Microsoft/AzureStorageExplorer/issues/299)
* Toegankelijkheid: Het dialoogvenster entiteit toevoegen is moeilijk om te navigeren wanneer u VoiceOver. Verbeteringen zijn aangebracht. [#206](https://github.com/Microsoft/AzureStorageExplorer/issues/206)
* Toegankelijkheid: De achtergrondkleur van de knop samenvouwen/uitvouwen voor het deelvenster met eigenschappen en acties is inconsistent met vergelijkbare UI-besturingselementen in thema met hoog Contrast zwart. De kleur is gewijzigd. [#123](https://github.com/Microsoft/AzureStorageExplorer/issues/123)
* Toegankelijkheid: In het thema met hoog Contrast zwart is de focus opmaak voor de knop 'X' in het dialoogvenster Eigenschappen niet zichtbaar. Dit probleem is opgelost. [#243](https://github.com/Microsoft/AzureStorageExplorer/issues/243)
* Toegankelijkheid: De tabbladen met eigenschappen en acties zijn verschillende aria-waarden die het heeft geresulteerd in een ervaring van de lezer subpar scherm ontbreekt. De ontbrekende aria-waarden zijn nu toegevoegd. [#316](https://github.com/Microsoft/AzureStorageExplorer/issues/316)
* Toegankelijkheid: Samengevouwen structuurknooppunten aan de linkerzijde zijn niet aria-uitgevouwen waarde false wordt opgegeven. Dit probleem is opgelost. [#352](https://github.com/Microsoft/AzureStorageExplorer/issues/352)

### <a name="known-issues"></a>Bekende problemen
* Loskoppelen van een resource koppelen via SAS-URI, zoals een blob-container kan een fout veroorzaken waarmee wordt voorkomen dat andere bijlagen correct weergegeven. U kunt dit probleem omzeilen, vernieuwt u het knooppunt voor de. Zie [dit probleem](https://github.com/Microsoft/AzureStorageExplorer/issues/537) voor meer informatie.
* Als u Visual Studio voor Mac en ooit een aangepaste AAD-configuratie hebt gemaakt, is het wellicht niet mogelijk om aan te melden. Verwijder de inhoud van het probleem te omzeilen, ~ /. IdentityService/AadConfigurations. Als u in dat geval heeft niet de blokkering opheffen u, stuur een reactie op [dit probleem](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite is nog niet volledig geïmplementeerd alle Storage-API's. Als gevolg hiervan kunnen er onverwachte fouten of problemen bij het gebruik van Azurite voor opslag.
* In zeldzame gevallen kan de focus structuur ophalen vastgelopen op Snelweergavetoegang. Registreer de focus, kunt u Alles vernieuwen.
* Uploaden vanaf uw OneDrive-map werkt niet vanwege een fout in NodeJS. De fout is opgelost, maar nog niet is geïntegreerd in Electron.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter tijdelijke oplossing beschreven [hier](https://github.com/Azure/azure-storage-node/issues/317).
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* De volgende functies biedt geen ondersteuning voor Azure Stack en er wordt geprobeerd om ze te gebruiken tijdens het werken met Azure Stack kan leiden tot onverwachte fouten:
   * Bestandsshares
   * Toegangslagen
   * Voorlopig verwijderen
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Voor Linux-gebruikers, moet u voor het installeren van [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-120"></a>Versie 1.2.0
06/12/2018

### <a name="new"></a>Nieuw
* Als Storage Explorer niet laden van abonnementen in slechts een subset van uw tenants, worden geladen abonnementen weergegeven samen met een foutbericht dat specifiek voor de tenants die niet zijn geslaagd. [#159](https://github.com/Microsoft/AzureStorageExplorer/issues/159)
* Op Windows, wanneer een update beschikbaar is, kunt u nu kiezen aan "Bijwerken op sluiten". Wanneer deze optie is verzameld, wordt het installatieprogramma voor de update wordt uitgevoerd na het sluiten van Storage Explorer. [#21](https://github.com/Microsoft/AzureStorageExplorer/issues/21)
* Herstel momentopname is toegevoegd aan het contextmenu van de editor voor het delen van bestand bij het weergeven van een momentopname van de bestandsshare. [#131](https://github.com/Microsoft/AzureStorageExplorer/issues/131)
* De knop wissen wachtrij is nu altijd ingeschakeld. [#135](https://github.com/Microsoft/AzureStorageExplorer/issues/135)
* Ondersteuning voor aanmelding bij AD FS Azure Stack is opnieuw worden ingeschakeld. Azure Stack versie 1804 of hoger is vereist. [#150](https://github.com/Microsoft/AzureStorageExplorer/issues/150)

### <a name="fixes"></a>Oplossingen
* Als u momentopnamen voor een bestandsshare waarvan de naam een voorvoegsel van een andere bestandsshare in hetzelfde opslagaccount is hebt bekeken, wordt klikt u vervolgens de momentopnamen voor de bestandsshare ook weergegeven. Dit probleem is opgelost. [#255](https://github.com/Microsoft/AzureStorageExplorer/issues/255)
* Bij het koppelen via SAS wordt zou een bestand herstellen vanuit een momentopname van bestandsshare leiden tot een fout. Dit probleem is opgelost. [#211](https://github.com/Microsoft/AzureStorageExplorer/issues/211)
* Bij het weergeven van momentopnamen voor een blob, wordt de momentopname promoten actie is ingeschakeld als de basis-blob en een momentopname van een enkele zijn geselecteerd. De actie is nu alleen ingeschakeld als een momentopname van een enkele is geselecteerd. [#230](https://github.com/Microsoft/AzureStorageExplorer/issues/230)
* Als één taak (zoals het downloaden van een blob) aan de slag en later is mislukt, wordt deze niet automatisch opnieuw nadat u een andere taak van hetzelfde type gestart. Alle taken moeten nu automatisch opnieuw proberen, ongeacht hoeveel taken u hebt in de wachtrij geplaatst.
* Editors geopend voor de zojuist gemaakte blob-containers in GPV2- en Blob Storage-accounts heeft geen een Toegangslaag-kolom. Dit probleem is opgelost. [#109](https://github.com/Microsoft/AzureStorageExplorer/issues/109)
* Een kolom van de Toegangslaag soms niet weergegeven wanneer een opslagaccount of blob-container is koppelen via SAS. De kolom altijd nu worden weergegeven, maar met een lege waarde als er geen Toegangslaag zijn. [#160](https://github.com/Microsoft/AzureStorageExplorer/issues/160)
* Instellen van de Toegangslaag van een onlangs geüpload blok-blob is uitgeschakeld. Dit probleem is opgelost. [#171](https://github.com/Microsoft/AzureStorageExplorer/issues/171)
* Als de knop 'Houden tabblad openen' is aangeroepen met behulp van toetsenbord, zou de toetsenbordfocus verloren zijn. Nu wordt de focus verplaatsen naar het tabblad dat geopend is bewaard. [#163](https://github.com/Microsoft/AzureStorageExplorer/issues/163)
* Voice-over is niet een bruikbaar beschrijving van de huidige operator geeft voor een query in de opbouwfunctie voor query's. Het is nu meer beschrijvende. [#207](https://github.com/Microsoft/AzureStorageExplorer/issues/207)
* De koppelingen paginering voor de verschillende editors zijn niet beschrijvende. Ze zijn gewijzigd voor een meer beschrijvende worden. [#205](https://github.com/Microsoft/AzureStorageExplorer/issues/205)
* In het dialoogvenster entiteit toevoegen is VoiceOver niet aankondigen welke kolom een invoer-element is onderdeel van. De naam van de huidige kolom is nu opgenomen in de beschrijving van het element. [#206](https://github.com/Microsoft/AzureStorageExplorer/issues/206)
* Keuzerondjes en selectievakjes heeft geen een zichtbare rand wanneer de focus op. Dit probleem is opgelost. [#237](https://github.com/Microsoft/AzureStorageExplorer/issues/237)

### <a name="known-issues"></a>Bekende problemen
* Bij het gebruik van emulators, zoals Azure Storage-Emulator of Azurite, moet u deze luistert voor verbindingen op hun standaardpoorten. Anders pas Opslagverkenner weer verbinding te maken met.
* Als u Visual Studio voor Mac en ooit een aangepaste AAD-configuratie hebt gemaakt, is het wellicht niet mogelijk om aan te melden. Verwijder de inhoud van het probleem te omzeilen, ~ /. IdentityService/AadConfigurations. Als u in dat geval heeft niet de blokkering opheffen u, stuur een reactie op [dit probleem](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite is nog niet volledig geïmplementeerd alle Storage-API's. Als gevolg hiervan kunnen er onverwachte fouten of problemen bij het gebruik van Azurite voor opslag.
* In zeldzame gevallen kan de focus structuur ophalen vastgelopen op Snelweergavetoegang. Registreer de focus, kunt u Alles vernieuwen.
* Uploaden vanaf uw OneDrive-map werkt niet vanwege een fout in NodeJS. De fout is opgelost, maar nog niet is geïntegreerd in Electron.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter tijdelijke oplossing beschreven [hier](https://github.com/Azure/azure-storage-node/issues/317).
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde storage-account van Azure Stack.
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Voor Linux-gebruikers, moet u voor het installeren van [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-110"></a>Versie 1.1.0
09-05-2018

### <a name="new"></a>Nieuw
* Storage Explorer biedt nu ondersteuning voor het gebruik van Azurite. Opmerking: de verbinding met Azurite wordt vastgelegd op de eindpunten van de standaard-ontwikkeling.
* Storage Explorer biedt nu ondersteuning voor laag voor alleen-Blob en GPV2-Opslagaccounts. Meer informatie over Toegangslagen [hier](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers).
* Een begintijd is niet langer vereist bij het genereren van een SAS.

### <a name="fixes"></a>Oplossingen
* Bij het ophalen van abonnementen voor US Government-accounts is verbroken. Dit probleem is opgelost. [#61](https://github.com/Microsoft/AzureStorageExplorer/issues/61)
* Het verlooptijdstip voor toegangsbeleid is correct niet opgeslagen. Dit probleem is opgelost. [#50](https://github.com/Microsoft/AzureStorageExplorer/issues/50)
* Bij het genereren van SAS-URL voor een item in een container, is niet de naam van het item wordt toegevoegd aan de URL. Dit probleem is opgelost. [#44](https://github.com/Microsoft/AzureStorageExplorer/issues/44)
* Bij het maken van een SAS is verlopen keren dat in het verleden is soms de standaardwaarde. Dit is vanwege Storage Explorer met behulp van die de laatste begin- en tijd als standaardwaarden gebruikt. Telkens wanneer u de SAS-dialoogvenster opent, wordt nu een nieuwe set standaardwaarden gegenereerd. [#35](https://github.com/Microsoft/AzureStorageExplorer/issues/35)
* Bij het kopiëren tussen Opslagaccounts, is een 24-uurs SAS gegenereerd. Als het exemplaar heeft geduurd meer dan 24 uur, klikt u vervolgens mislukken de kopie. We hebben de SAS is de afgelopen week om te verminderen de kans op een kopie is mislukt vanwege een verlopen SAS verhoogd. [#62](https://github.com/Microsoft/AzureStorageExplorer/issues/62)
* Voor sommige activiteiten werkt te klikken op 'Annuleren' niet altijd. Dit probleem is opgelost. [#125](https://github.com/Microsoft/AzureStorageExplorer/issues/125)
* Voor sommige activiteiten zijn de overdrachtssnelheid van de is onjuist. Dit probleem is opgelost. [#124](https://github.com/Microsoft/AzureStorageExplorer/issues/124)
* De spelling van 'Vorige' in het menu weergave is onjuist. Het is nu correct gespeld. [#71](https://github.com/Microsoft/AzureStorageExplorer/issues/71)
* De laatste pagina van het Windows-installatieprogramma heeft een knop 'Volgende'. Het is gewijzigd in een knop 'Voltooid'. [#70](https://github.com/Microsoft/AzureStorageExplorer/issues/70)
* Tabblad focus is niet zichtbaar voor knoppen in dialoogvensters bij het gebruik van het thema HC zwart. Het is nu zichtbaar. [#64](https://github.com/Microsoft/AzureStorageExplorer/issues/64)
* Het hoofdlettergebruik van 'Automatisch oplossen' voor acties in het activiteitenlogboek is onjuist. Nu is juist. [#51](https://github.com/Microsoft/AzureStorageExplorer/issues/51)
* Wanneer een entiteit uit een tabel is verwijderd, wordt in het dialoogvenster waarin u wordt gevraagd om bevestiging een foutpictogram weergegeven. Het dialoogvenster gebruikt nu een waarschuwingspictogram weergegeven. [#148](https://github.com/Microsoft/AzureStorageExplorer/issues/148)

### <a name="known-issues"></a>Bekende problemen
* Als u Visual Studio voor Mac en ooit een aangepaste AAD-configuratie hebt gemaakt, is het wellicht niet mogelijk om aan te melden. Verwijder de inhoud van het probleem te omzeilen, ~ /. IdentityService/AadConfigurations. Als u in dat geval heeft niet de blokkering opheffen u, stuur een reactie op [dit probleem](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite is nog niet volledig geïmplementeerd alle Storage-API's. Als gevolg hiervan kunnen er onverwachte fouten of problemen bij het gebruik van Azurite voor opslag.
* In zeldzame gevallen kan de focus structuur ophalen vastgelopen op Snelweergavetoegang. Registreer de focus, kunt u Alles vernieuwen.
* Uploaden vanaf uw OneDrive-map werkt niet vanwege een fout in NodeJS. De fout is opgelost, maar nog niet is geïntegreerd in Electron.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter tijdelijke oplossing beschreven [hier](https://github.com/Azure/azure-storage-node/issues/317).
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde storage-account van Azure Stack.
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Voor Linux-gebruikers, moet u voor het installeren van [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```


## <a name="version-100"></a>Versie 1.0.0
16-04-2018

### <a name="new"></a>Nieuw
* Verbeterde verificatie waarmee Storage Explorer de store te gebruiken hetzelfde account als Visual Studio 2017. Deze functie wilt gebruiken, moet u re-aanmelden bij uw accounts en opnieuw instellen van uw gefilterde abonnementen.
* Voor Azure Stack-accounts ondersteund door AAD wordt Storage Explorer nu Azure Stack-abonnementen ophalen als 'Azure Stack als doel' is ingeschakeld. U is niet meer nodig hebt om een aangepaste aanmelding-omgeving te maken.
* Enkele tips zijn toegevoegd om sneller te navigeren. Hieronder vallen bij het omschakelen van verschillende vensters en verplaatsen tussen editors. Zie de weergave-menu voor meer informatie.
* Storage Explorer feedback is nu aanwezig op GitHub. U kunt onze pagina met problemen bereiken door te klikken op de knop Feedback onderaan links of door te gaan [ https://github.com/Microsoft/AzureStorageExplorer/issues ](https://github.com/Microsoft/AzureStorageExplorer/issues). U kunt voorstellen, rapporteren van problemen, vragen of laat u een andere vorm van feedback.
* Als u SSL-certificaat problemen ondervindt en kan de strijdige certificaat niet vinden, kunt u nu Storage Explorer starten vanuit de opdrachtregel met de `--ignore-certificate-errors` vlag. Wanneer met deze markering wordt gestart, worden de Storage Explorer SSL-certificaatfouten genegeerd.
* Er is nu een downloadoptie '' in het contextmenu voor de blob en bestand.
* Verbeterde toegankelijkheid en ondersteuning voor schermlezers. Als u zich op toegankelijkheidsfuncties baseert, Zie onze [toegankelijkheid documentatie](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-accessibility) voor meer informatie.
* Storage Explorer gebruikt nu Electron 1.8.3

### <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken
* Storage Explorer is overgeschakeld naar een nieuwe verificatiebibliotheek. Als onderdeel van de switch aan de bibliotheek moet u voor re-aanmelden bij uw accounts en opnieuw instellen van uw gefilterde abonnementen
* De methode die wordt gebruikt voor het versleutelen van gevoelige gegevens is gewijzigd. Dit kan resulteren in enkele van uw Snelweergavetoegang-items die moeten worden moet opnieuw worden toegevoegd en/of enkele van de resources die opnieuw worden gekoppeld die zijn gekoppeld.

### <a name="fixes"></a>Oplossingen
* Sommige gebruikers achter een proxy's zou hebben groep blob uploads of downloads onderbroken door een 'kan niet omzetten' foutbericht weergegeven. Dit probleem is opgelost.
* Als het aanmelden is vereist wanneer met behulp van een directe koppeling te klikken op de vraag 'Aanmelden' zou een pop-up leeg. Dit probleem is opgelost.
* Op Linux als Storage Explorer niet kan worden gestart vanwege het vastlopen van een proces GPU ontvangt u nu een bericht van het vastlopen van gemeld dat u de '--disable-gpu' switch- en Storage Explorer wordt vervolgens automatisch opnieuw opgestart met de switch die is ingeschakeld.
* Ongeldige toegangsbeleid zijn moeilijk te identiteit in het dialoogvenster toegangsbeleid. Ongeldige toegangsbeleid id's worden nu beschreven in het rood voor meer zichtbaarheid.
* Het activiteitenlogboek hoeft soms grote gebieden van spaties tussen de verschillende onderdelen van een activiteit. Dit probleem is opgelost.
* Als u nog een timestamp-component in een ongeldige status en wordt een poging gedaan om te wijzigen van een andere component, zou in de tabel query-editor de editor blokkeren. De editor wordt nu de component timestamp herstellen naar de laatste geldige status wanneer een wijziging in een andere component wordt gedetecteerd.
* Als u onderbroken tijdens het typen in uw zoekopdracht in de structuurweergave wordt weergegeven, de zoekopdracht beginnen en focus zou worden gestolen van het tekstvak. Nu moet u expliciet starten door op de Enter-toets te drukken of door te klikken op de knop start search zoeken.
* De opdracht 'Shared Access Signature ophalen' wordt soms worden uitgeschakeld wanneer de rechtermuisknop te klikken op een bestand in een bestandsshare. Dit probleem is opgelost.
* Als het structuurknooppunt voor de resource met de focus tijdens de zoekopdracht is gefilterd, kan u niet tabblad in de resourcestructuur van de en gebruik de pijltoetsen om te navigeren van de resourcestructuur van de. Nu, als het knooppunt gerichte resource verborgen is, het eerste knooppunt in de resourcestructuur van de zal worden automatisch gericht.
* Scheidingsteken voor extra zou soms worden weergegeven in de werkbalk van de editor. Dit probleem is opgelost.
* In het tekstvak breadcrumb overflow soms. Dit probleem is opgelost.
* De Blob en bestandsshare editors vernieuwd soms voortdurend wanneer veel bestanden tegelijk uploaden. Dit probleem is opgelost.
* De functie 'Mapstatistieken' heeft geen doel in de weergave beheer van Share-momentopnamen. Dit is nu uitgeschakeld.
* Op Linux niet het menu bestand weergegeven. Dit probleem is opgelost.
* Tijdens het uploaden van een map op een bestandsshare, standaard, zijn alleen de inhoud van de map geüpload. Nu is het standaardgedrag voor het uploaden van de inhoud van de map naar een overeenkomende map in de bestandsshare.
* De volgorde van de knoppen in dialoogvensters voor verschillende had is omgekeerd. Dit probleem is opgelost.
* Verschillende aan beveiliging gerelateerde problemen.

### <a name="known-issues"></a>Bekende problemen
* In zeldzame gevallen kan de focus structuur ophalen vastgelopen op Snelweergavetoegang. Registreer de focus, kunt u Alles vernieuwen.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter omzeilen hier beschreven.
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde storage-account van Azure Stack.
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Voor Linux-gebruikers, moet u voor het installeren van [.NET Core 2.0](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-096"></a>Versie 0.9.6
28-02-2018

### <a name="fixes"></a>Oplossingen
* Een probleem wordt voorkomen dat verwachte blobs/bestanden die worden vermeld in de editor. Dit probleem is opgelost.
* Een probleem veroorzaakt schakelen tussen momentopnameweergaven naar items onjuist worden weergegeven. Dit probleem is opgelost.

### <a name="known-issues"></a>Bekende problemen
* Storage Explorer biedt geen ondersteuning voor AD FS-accounts.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter tijdelijke oplossing beschreven [hier](https://github.com/Azure/azure-storage-node/issues/317).
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Het instellingenvenster account kan worden weergegeven die u wilt opnieuw invoeren van referenties om te filteren op abonnementen.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde storage-account van Azure Stack.
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-095"></a>Versie 0.9.5
06-02-2018

### <a name="new"></a>Nieuw

* Ondersteuning voor momentopnamen van bestandsshares:
    * Momentopnamen maken en beheren voor uw bestandsshares.
    * Eenvoudig schakelen tussen de momentopnamen van uw bestandsshares kunt verkennen.
    * Vorige versies van uw bestanden te herstellen.
* Preview-versie-ondersteuning voor Azure Data Lake Store:
    * Verbinding maken met uw ADLS-resources van meerdere accounts.
    * Verbinding maken met en ADLS-resources met behulp van ADL-URI's delen.
    * Basic bestand/map bewerkingen recursief uitvoeren.
    * Afzonderlijke mappen vastmaken aan Snelweergavetoegang.
    * Mapstatistieken weergeven.

### <a name="fixes"></a>Oplossingen
* Prestatieverbeteringen voor opstarten.
* Oplossingen voor verschillende fouten.

### <a name="known-issues"></a>Bekende problemen
* Storage Explorer biedt geen ondersteuning voor AD FS-accounts.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter omzeilen hier beschreven.
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Het instellingenvenster account kan worden weergegeven die u wilt opnieuw invoeren van referenties om te filteren op abonnementen.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde storage-account van Azure Stack.
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer.exe --disable-gpu
    ```

* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-094-and-093"></a>Versie 0.9.4 en 0.9.3
21-01-2018

### <a name="new"></a>Nieuw
* Uw bestaande Storage Explorer-venster wordt niet opnieuw worden gebruikt wanneer:
    * Rechtstreekse koppelingen die zijn gegenereerd in Storage Explorer te openen.
    * Storage Explorer openen vanuit de portal.
    * Storage Explorer openen vanuit Azure Storage VS Code-extensie (binnenkort beschikbaar).
* De mogelijkheid is toegevoegd om een nieuwe Storage Explorer-venster binnen Storage Explorer te openen.
    * Voor Windows is er een optie 'Nieuw venster' onder het Menu bestand en in het contextmenu van de taakbalk.
    * Er is een optie 'Nieuw venster' onder het Menu van de App voor Mac.

### <a name="fixes"></a>Oplossingen
* Er is een beveiligingsprobleem opgelost. Voer een upgrade uit naar 0.9.4 zo snel mogelijk.
* Oude activiteiten zijn niet op de juiste wijze worden opgeschoond. Dit invloed op de prestaties van langlopende taken. Ze zijn nu wordt opgeschoond correct.
* Acties met betrekking tot grote aantallen bestanden en mappen kunnen van tijd tot tijd Storage Explorer om te blokkeren. Aanvragen naar Azure voor bestandsshares zijn nu beperkt om te beperken van systeem resource worden gebruikt.

### <a name="known-issues"></a>Bekende problemen
* Storage Explorer biedt geen ondersteuning voor AD FS-accounts.
* Sneltoetsen voor 'Weergave Explorer' en 'Accountbeheer weergave' moet Ctrl / Cmd + Shift + E en Ctrl / Cmd + Shift + A respectievelijk.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter omzeilen hier beschreven.
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Het instellingenvenster account kan worden weergegeven die u wilt opnieuw invoeren van referenties om te filteren op abonnementen.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde storage-account van Azure Stack.
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer --disable-gpu
    ```

* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-092"></a>Versie 0.9.2
11/01/2017

### <a name="hotfixes"></a>Hotfixes
* Er zijn onverwachte gegevens-wijzigingen zijn mogelijk bij het bewerken van Edm.DateTime waarden voor de tabelentiteiten, afhankelijk van de lokale tijdzone. De editor gebruikt nu een vak tekst zonder opmaak, consistent, nauwkeurige controle over Edm.DateTime waarden geven.
* Uploaden/downloaden van een reeks blobs wanneer gekoppeld met de naam en sleutel kan niet worden gestart. Dit probleem is opgelost.
* Eerder Storage Explorer zou alleen vraagt u om te verifiëren van een verouderde account als een of meer van de account van de abonnementen is geselecteerd. Nu wordt Storage Explorer u gevraagd, zelfs als het account volledig is gefilterd.
* Het domein van opslageindpunten voor Azure US Government is onjuist. Het is opgelost.
* De knop toepassen Accounts beheren in het venster was soms moeilijk te klikken. Dit mag niet meer gebeuren.

### <a name="new"></a>Nieuw
* Preview-versie-ondersteuning voor Azure Cosmos DB:
    * [Online documentatie](./cosmos-db/storage-explorer.md)
    * Databases en verzamelingen maken
    * Gegevens bewerken
    * Query's uitvoeren, maken of verwijderen van documenten
    * Opgeslagen procedures, door gebruiker gedefinieerde functies en triggers bijwerken
    * Verbindingsreeksen gebruiken om te verbinden en beheren van uw databases
* De prestaties van het uploaden/downloaden van veel kleine blobs zijn verbeterd.
* Een "Opnieuw alle" actie toegevoegd als er fouten in een blob uploaden groep of blob downloaden.
* Storage Explorer pauzeert nu iteratie tijdens blob uploaden/downloaden als wordt gedetecteerd dat de netwerkverbinding is verbroken. U kunt vervolgens iteratie hervatten nadat de netwerkverbinding hersteld is.
* De mogelijkheid tot ' sluiten ","Sluiten anderen"en"Afsluiten"tabbladen via contextmenu toegevoegd.
* Storage Explorer gebruikt nu systeemeigen dialoogvensters en systeemeigen contextmenu's.
* Storage Explorer is nu beter toegankelijk. Verbeteringen zijn onder andere:
    * Verbeterde ondersteuning voor schermlezers, voor NVDA op Windows en voor Voice-over op Mac
    * Verbeterde hoog contrast thema 's
    * TAB-toets en het toetsenbord de toetsenbordfocus wordt opgelost

### <a name="fixes"></a>Oplossingen
* Als u probeert te openen of downloaden van een blob met een ongeldige naam van de Windows-bestand, kan de bewerking mislukken. Storage Explorer wordt nu detecteren of de naam van een blob ongeldig is en vraag als u wilt deze coderen of overslaan van de blob. Storage Explorer detecteert ook als een bestandsnaam op om te worden gecodeerd en de vraag wordt weergegeven als wilt decoderen deze voordat u uploadt.
* Tijdens het uploaden van blob, wordt de editor voor de doel-blob-container soms niet correct vernieuwd. Dit probleem is opgelost.
* De ondersteuning voor verschillende vormen van tekenreeksen voor databaseverbindingen en SAS-URI's is teruggedraaid. We alle bekende problemen hebt opgelost, maar Geef feedback verzenden als u meer problemen.
* De melding van updates is verbroken voor bepaalde gebruikers in 0.9.0. Dit probleem is opgelost en degenen die beïnvloed door de fout, kunt u handmatig de meest recente versie van Storage Explorer downloaden [hier](https://azure.microsoft.com/features/storage-explorer/).

### <a name="known-issues"></a>Bekende problemen
* Storage Explorer biedt geen ondersteuning voor AD FS-accounts.
* Sneltoetsen voor 'Weergave Explorer' en 'Accountbeheer weergave' moet Ctrl / Cmd + Shift + E en Ctrl / Cmd + Shift + A respectievelijk.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter omzeilen hier beschreven.
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Het instellingenvenster account kan worden weergegeven die u wilt opnieuw invoeren van referenties om te filteren op abonnementen.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde storage-account van Azure Stack.
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer --disable-gpu
    ```

* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-091-and-090"></a>Versie 0.9.1 tot en en 0.9.0
10/20/2017
### <a name="new"></a>Nieuw
* Preview-versie-ondersteuning voor Azure Cosmos DB:
    * [Online documentatie](./cosmos-db/storage-explorer.md)
    * Databases en verzamelingen maken
    * Gegevens bewerken
    * Query's uitvoeren, maken of verwijderen van documenten
    * Opgeslagen procedures, door gebruiker gedefinieerde functies en triggers bijwerken
    * Verbindingsreeksen gebruiken om te verbinden en beheren van uw databases
* De prestaties van het uploaden/downloaden van veel kleine blobs zijn verbeterd.
* Een "Opnieuw alle" actie toegevoegd als er fouten in een blob uploaden groep of blob downloaden.
* Storage Explorer pauzeert nu iteratie tijdens blob uploaden/downloaden als wordt gedetecteerd dat de netwerkverbinding is verbroken. U kunt vervolgens iteratie hervatten nadat de netwerkverbinding hersteld is.
* De mogelijkheid tot ' sluiten ","Sluiten anderen"en"Afsluiten"tabbladen via contextmenu toegevoegd.
* Storage Explorer gebruikt nu systeemeigen dialoogvensters en systeemeigen contextmenu's.
* Storage Explorer is nu beter toegankelijk. Verbeteringen zijn onder andere:
    * Verbeterde ondersteuning voor schermlezers, voor NVDA op Windows en voor Voice-over op Mac
    * Verbeterde hoog contrast thema 's
    * TAB-toets en het toetsenbord de toetsenbordfocus wordt opgelost

### <a name="fixes"></a>Oplossingen
* Als u probeert te openen of downloaden van een blob met een ongeldige naam van de Windows-bestand, kan de bewerking mislukken. Storage Explorer wordt nu detecteren of de naam van een blob ongeldig is en vraag als u wilt deze coderen of overslaan van de blob. Storage Explorer detecteert ook als een bestandsnaam op om te worden gecodeerd en de vraag wordt weergegeven als wilt decoderen deze voordat u uploadt.
* Tijdens het uploaden van blob, wordt de editor voor de doel-blob-container soms niet correct vernieuwd. Dit probleem is opgelost.
* De ondersteuning voor verschillende vormen van tekenreeksen voor databaseverbindingen en SAS-URI's is teruggedraaid. We alle bekende problemen hebt opgelost, maar Geef feedback verzenden als u meer problemen.
* De melding van updates is verbroken voor bepaalde gebruikers in 0.9.0. Dit probleem is opgelost en degenen die beïnvloed door de fout, kunt u handmatig de meest recente versie van Storage Explorer downloaden [hier](https://azure.microsoft.com/features/storage-explorer/)

### <a name="known-issues"></a>Bekende problemen
* Storage Explorer biedt geen ondersteuning voor AD FS-accounts.
* Sneltoetsen voor 'Weergave Explorer' en 'Accountbeheer weergave' moet Ctrl / Cmd + Shift + E en Ctrl / Cmd + Shift + A respectievelijk.
* Wanneer die gericht is op Azure Stack, mislukken bepaalde bestanden als toevoeg-blobs uploaden.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is omdat we de annuleren filter omzeilen hier beschreven.
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Het instellingenvenster account kan worden weergegeven die u wilt opnieuw invoeren van referenties om te filteren op abonnementen.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde storage-account van Azure Stack.
* De Electron-shell die door de Storage Explorer heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Storage Explorer is een leeg (leeg) hoofdvenster weergeven, kunt u proberen Storage Explorer starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door toe te voegen de `--disable-gpu` schakelen:

    ```
    ./StorageExplorer --disable-gpu
    ```

* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-0816"></a>Versie 0.8.16
8/21/2017

### <a name="new"></a>Nieuw
* Bij het openen van een blob wordt Storage Explorer u gevraagd naar het gedownloade bestand uploaden als een wijziging wordt gedetecteerd
* Verbeterde Azure Stack-aanmeldingservaring
* De prestaties van het uploaden/downloaden van veel kleine bestanden op hetzelfde moment verbeterd


### <a name="fixes"></a>Oplossingen
* Voor bepaalde blobtypen resulteert kiezen tijdens een conflict uploaden "vervangen" soms in het uploaden wordt opnieuw opgestart.
* In versie 0.8.15, zou de uploads soms achterstallige in 99%.
* Bij het uploaden van bestanden naar een bestandsshare, als u wilt uploaden naar een map die niet nog bestaat, mislukt het uploaden.
* Storage Explorer is niet correct genereren van tijdstempels voor handtekeningen voor gedeelde toegang en tabel query's.


### <a name="known-issues"></a>Bekende problemen
* Met behulp van een naam en verbindingsreeks van sleutel werkt momenteel niet. Het wordt opgelost in de volgende release. Tot die tijd kunt u met de naam en sleutel koppelen.
* Als u probeert een bestand met een ongeldige naam van de Windows-bestand te openen, leidt het downloaden van het tot een bestand is niet gevonden fout.
* Na het klikken op 'Annuleren' voor een taak, duren het even voor die taak annuleren. Dit is een beperking van de Azure Storage-knooppunt-bibliotheek.
* Nadat het uploaden van een blob is voltooid, wordt het tabblad die het uploaden wordt gestart vernieuwd. Dit is een wijziging ten opzichte van vorige gedrag, en wordt ook leiden tot u teruggaat naar de hoofdmap van de container in.
* Als u de verkeerde PINCODE/Smartcard-certificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Het instellingenvenster account kan worden weergegeven die u wilt opnieuw invoeren van referenties om te filteren op abonnementen.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde storage-account van Azure Stack.
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is bijgewerkt: dit kan worden gedaan door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* Voor gebruikers op Ubuntu 17.04, moet u GConf installeren, kunt u dit doen door te voeren van de volgende opdrachten en vervolgens de computer opnieuw op te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

### <a name="version-0814"></a>Versie 0.8.14
06/22/2017

### <a name="new"></a>Nieuw

* Bijgewerkte Electron versie 1.7.2 om te profiteren van verschillende essentiële beveiligingsupdates
* U kunt nu snel toegang tot de online gids voor probleemoplossing in het menu help
* Storage Explorer probleemoplossing [handleiding][2]
* [Instructies][3] verbinding wilt maken met een Azure Stack-abonnement

### <a name="known-issues"></a>Bekende problemen

* Knoppen op het dialoogvenster voor bevestiging van verwijderen map zich niet registreren met de muis klikt op op Linux. werk is ongeveer het gebruik van de Enter-toets
* Als u ervoor de verkeerde PINCODE/Smartcard-certificaat kiest wordt moet u opnieuw op te starten om Opslagverkenner vergeet de beslissing
* Met meer dan 3 groepen blobs of op hetzelfde moment uploaden van bestanden kan leiden tot fouten
* Het instellingenvenster account kan worden weergegeven die u opnieuw invoeren van referenties wilt om abonnementen filteren
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde storage-account van Azure Stack.
* Ubuntu 14.04 installeren moet gcc-versie is bijgewerkt of stappen voor het bijwerken worden bijgewerkt: hieronder:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

### <a name="version-0813"></a>Versie 0.8.13
12/05/2017

#### <a name="new"></a>Nieuw

* Storage Explorer probleemoplossing [handleiding][2]
* [Instructies][3] verbinding wilt maken met een Azure Stack-abonnement

#### <a name="fixes"></a>Oplossingen

* Opgelost: Uploaden van het bestand heeft een hoge kans dat een out-of geheugenfout veroorzaakt
* Opgelost: U kunt nu aanmelden met een PINCODE/Smartcard
* Opgelost: Open in de Portal nu werkt met Azure China 21Vianet, Azure Duitsland, Azure US Government en Azure Stack
* Opgelost: Tijdens het uploaden van een map naar een blobcontainer, zou een fout 'Ongeldige bewerking' soms optreden
* Opgelost: Alles selecteren is bij het beheren van momentopnamen uitgeschakeld
* Opgelost: De metagegevens van de basis blob mogelijk overschreven nadat u hebt bekeken, de eigenschappen van de momentopnamen ervan verwijderd

#### <a name="known-issues"></a>Bekende problemen

* Als u ervoor de verkeerde PINCODE/Smartcard-certificaat kiest wordt moet u opnieuw op te starten om Opslagverkenner vergeet de beslissing
* Terwijl het in- of uitzoomen op schaal, het zoomniveau kan tijdelijk opnieuw worden ingesteld op het standaard beveiligingsniveau
* Met meer dan 3 groepen blobs of op hetzelfde moment uploaden van bestanden kan leiden tot fouten
* Het instellingenvenster account kan worden weergegeven die u opnieuw invoeren van referenties wilt om abonnementen filteren
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde storage-account van Azure Stack.
* Ubuntu 14.04 installeren moet gcc-versie is bijgewerkt of stappen voor het bijwerken worden bijgewerkt: hieronder:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-0812-and-0811-and-0810"></a>Versie 0.8.12 en 0.8.11 en 0.8.10
04/07/2017

#### <a name="new"></a>Nieuw

* Storage Explorer wordt nu automatisch gesloten wanneer u een update vanuit de melding van updates installeert
* In-place snelle toegang biedt een verbeterde ervaring voor het werken met uw vaak gebruikte resources
* In de Blob-Container-editor kunt nu u zien welke virtuele machine een lease blob behoort
* U kunt nu het deelvenster links samenvouwen
* Detectie wordt nu uitgevoerd op hetzelfde moment als download
* Statistieken in de Blob-Container, bestandsshare en tabel editors gebruiken om te zien van de grootte van de resource of de selectie
* U kunt nu aan Azure Active Directory (AAD) voor aanmelding op basis van Azure Stack-accounts.
* U kunt nu archiefbestanden meer dan 32MB uploaden naar Premium storage-accounts
* Verbeterde Toegankelijkheidsondersteuning voor
* U kunt nu toevoegen de vertrouwde basis-64 gecodeerd X.509 SSL-certificaten door te gaan bewerken -&gt; SSL-certificaten -&gt; certificaten importeren

#### <a name="fixes"></a>Oplossingen

* Opgelost: na het vernieuwen van een account referenties, de structuurweergave wordt soms niet automatisch vernieuwd
* Opgelost: genereren van een SAS voor de emulator wachtrijen en tabellen zou leiden tot een ongeldige URL
* Vaste: premium storage-accounts kunnen nu worden uitgebreid als een proxy is ingeschakeld
* Opgelost: de knop toepassen op de pagina accounts management werkt niet als u had 1 of 0 accounts die zijn geselecteerd
* Vaste: het uploaden van blobs waarvoor oplossingen mogelijk mislukt - opgelost in 0.8.11
* Opgelost: verzenden van feedback is onderverdeeld in 0.8.11 - 0.8.12, vast

#### <a name="known-issues"></a>Bekende problemen

* Na de upgrade naar 0.8.10, moet u om alle uw referenties te vernieuwen.
* Terwijl het in- of uitzoomen op schaal, het zoomniveau kan tijdelijk opnieuw instellen op het standaard beveiligingsniveau.
* Met meer dan 3 groepen blobs of op hetzelfde moment uploaden van bestanden, kan fouten veroorzaken.
* Het instellingenvenster account kan worden weergegeven dat u moet referenties invoeren om te filteren van abonnementen.
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde storage-account van Azure Stack.
* Ubuntu 14.04 installeren moet gcc-versie is bijgewerkt of stappen voor het bijwerken worden bijgewerkt: hieronder:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-089-and-088"></a>Versie 0.8.9 en 0.8.8
02/23/2017

>[!VIDEO https://www.youtube.com/embed/R6gonK3cYAc?ecver=1]

>[!VIDEO https://www.youtube.com/embed/SrRPCm94mfE?ecver=1]


#### <a name="new"></a>Nieuw

* Storage Explorer 0.8.9 worden automatisch de meest recente versie voor updates gedownload.
* Hotfix: met behulp van een portal zou gegenereerde SAS-URI naar een opslagaccount koppelen leiden tot een fout.
* U kunt nu maken, beheren en blob-momentopnamen te bevorderen.
* U kunt nu aanmelden bij Azure China 21Vianet, Azure Duitsland en Azure US Government-accounts.
* U kunt nu het zoomniveau wijzigen. Gebruik de opties in het menu Weergave inzoomen, uitzoomen en zoomen opnieuw instellen.
* Unicode-tekens worden nu ondersteund in de metagegevens van de gebruiker voor bestanden en blobs.
* Verbeterde toegankelijkheid.
* Opmerkingen bij de release van de volgende versie kunnen worden weergegeven in de melding van updates. U kunt ook de huidige release-opmerkingen in het menu Help weergeven.

#### <a name="fixes"></a>Oplossingen

* Vaste: het versienummer nu juist wordt weergegeven in het Configuratiescherm op Windows
* Vaste: zoeken is niet langer beperkt tot 50.000 knooppunten
* Vaste: uploaden naar een bestandsshare hebben altijd ingeschakeld als de doelmap nog niet bestaat.
* Vaste: verbeterde stabiliteit voor lange uploaden en downloaden

#### <a name="known-issues"></a>Bekende problemen

* Terwijl het in- of uitzoomen op schaal, het zoomniveau kan tijdelijk opnieuw instellen op het standaard beveiligingsniveau.
* Snelle toegang werkt alleen met abonnementen op basis van items. Lokale bronnen of bronnen die zijn gekoppeld via de sleutel of SAS-token worden niet ondersteund in deze release.
* Het duurt Snelweergavetoegang een paar seconden om te navigeren naar de doelresource, afhankelijk van hoeveel bronnen die u hebt.
* Met meer dan 3 groepen blobs of op hetzelfde moment uploaden van bestanden, kan fouten veroorzaken.

12/16/2016
### <a name="version-087"></a>Versie 0.8.7

>[!VIDEO https://www.youtube.com/embed/Me4Y4jxoer8?ecver=1]

#### <a name="new"></a>Nieuw

* U kunt kiezen hoe u het oplossen van conflicten aan het begin van de sessie van een update, download of kopieer in het venster activiteiten
* Beweeg de muisaanwijzer over een tabblad om te zien van het volledige pad van de storage-resource
* Wanneer u op een tabblad klikt, synchroniseert het met de locatie in het navigatiedeelvenster links

#### <a name="fixes"></a>Oplossingen

* Opgelost: Storage Explorer is nu een vertrouwde app op Mac
* Opgelost: Ubuntu 14.04 wordt opnieuw ondersteund
* Opgelost: Soms de account toevoegen gebruikersinterface knippert bij het laden van abonnementen
* Opgelost: Soms niet alle opslagresources zijn vermeld in het navigatiedeelvenster links
* Opgelost: Het actiedeelvenster soms leeg acties worden weergegeven
* Opgelost: De grootte van het venster van de laatste gesloten sessie wordt nu behouden
* Opgelost: U kunt meerdere tabbladen voor dezelfde resource met behulp van het contextmenu openen

#### <a name="known-issues"></a>Bekende problemen

* Snelle toegang werkt alleen met abonnementen op basis van items. Lokale bronnen of bronnen die zijn gekoppeld via de sleutel of SAS-token worden niet ondersteund in deze release
* Het duurt Snelweergavetoegang een paar seconden om te navigeren naar de doelresource, afhankelijk van hoeveel bronnen die u hebt
* Met meer dan 3 groepen blobs of op hetzelfde moment uploaden van bestanden kan leiden tot fouten
* Search ingangen doorzoeken op knooppunten, ongeveer 50.000 hierna prestaties wordt mogelijk beïnvloed of niet-verwerkte uitzondering veroorzaakt
* Voor het eerst met behulp van de Storage Explorer op macOS, ziet u mogelijk meerdere prompts machtiging voor toegang tot sleutelhanger van de gebruiker wordt gevraagd. Het is raadzaam dat altijd toestaan selecteren, zodat de prompt opnieuw niet weergegeven

11/18/2016
### <a name="version-086"></a>Versie 0.8.6

#### <a name="new"></a>Nieuw

* U kunt nu pincode services snel toegang krijgen tot het vaakst wordt gebruikt voor eenvoudige navigatie
* U kunt nu meerdere editors openen in verschillende tabbladen. Één klik om een tijdelijke tabblad; te openen Dubbelklik om een permanente tabblad te openen. U kunt ook klikken op de tijdelijke tabblad gemakkelijk een permanente tabblad
* We hebben aangebracht merkbare prestaties en stabiliteitsverbeteringen voor wordt geüpload en gedownload, met name voor grote bestanden op snel virtuele machines
* Lege "virtuele" mappen kunnen nu worden gemaakt in de blob-containers
* Geïntroduceerd afgebakende zoekopdracht met onze nieuwe zoekfunctie voor verbeterde subtekenreeks, opnieuw, zodat u hebt nu twee opties voor het zoeken in:
    * Globale zoeken: Geef een zoekterm in het Zoektekstvak
    * Afgebakende zoekopdracht - Klik op het pictogram met Vergrootglas naast een knooppunt, en vervolgens een zoekterm toevoegen aan het einde van het pad, of met de rechtermuisknop op en selecteer 'Zoeken vanaf hier'
* We hebben verschillende thema's toegevoegd: Licht (standaard), donker, Hoog Contrast zwart en Hoog Contrast wit. Ga naar bewerken -&gt; thema's aan uw voorkeur thema wijzigen
* U kunt Blob en bestand eigenschappen wijzigen
* We bieden nu ondersteuning voor codering (base64) en ongecodeerde Wachtrijberichten
* Op Linux, een 64-bits besturingssysteem is nu vereist. Voor deze release ondersteunen we alleen 64-bits Ubuntu 16.04.1 LTS
* We hebben onze logo bijgewerkt!

#### <a name="fixes"></a>Oplossingen

* Opgelost: Scherm problemen blokkeren
* Opgelost: Verbeterde beveiliging
* Opgelost: Soms dubbele gekoppelde accounts kunnen worden weergegeven.
* Opgelost: Een blob met een niet-gedefinieerde type inhoud kan een uitzondering gegenereerd.
* Opgelost: Openen van het deelvenster Query op een lege tabel is niet mogelijk
* Opgelost: Is afhankelijk van fouten in het zoekvak
* Opgelost: Vergroot het aantal resources die zijn geladen uit 50 tot 100 wanneer te klikken op "Meer Load"
* Opgelost: Op de eerste keer uitvoert als een account is aangemeld bij, selecteren we nu alle abonnementen voor dat account standaard

#### <a name="known-issues"></a>Bekende problemen

* Deze versie van de Storage Explorer niet wordt uitgevoerd op Ubuntu 14.04
* Als u wilt openen meerdere tabbladen voor dezelfde resource, continu Klik niet op dezelfde resource. Klik op een andere resource en vervolgens gaat u terug en klik vervolgens op de oorspronkelijke bron opnieuw openen in een ander tabblad
* Snelle toegang werkt alleen met abonnementen op basis van items. Lokale bronnen of bronnen die zijn gekoppeld via de sleutel of SAS-token worden niet ondersteund in deze release
* Het duurt Snelweergavetoegang een paar seconden om te navigeren naar de doelresource, afhankelijk van hoeveel bronnen die u hebt
* Met meer dan 3 groepen blobs of op hetzelfde moment uploaden van bestanden kan leiden tot fouten
* Search ingangen doorzoeken op knooppunten, ongeveer 50.000 hierna prestaties wordt mogelijk beïnvloed of niet-verwerkte uitzondering veroorzaakt

10/03/2016
### <a name="version-085"></a>Versie 0.8.5

#### <a name="new"></a>Nieuw

* Kan nu Portal gegenereerde SAS-sleutels gebruiken om te koppelen aan de Storage-Accounts en resources

#### <a name="fixes"></a>Oplossingen

* Opgelost: racevoorwaarde tijdens de zoekopdracht soms veroorzaakt knooppunten worden niet uit te breiden
* Opgelost: Bij het verbinden met de Storage-Accounts met de naam en sleutel werkt 'HTTP gebruiken' niet
* Opgelost: SAS-sleutels (die speciaal Portal gegenereerd) geretourneerd met een fout "afsluitende schuine streep"
* Vaste: tabel problemen met importeren
    * Soms zijn partitiesleutel en rijsleutel teruggedraaid
    * Kan niet 'null' partitiesleutels lezen

#### <a name="known-issues"></a>Bekende problemen

* Search-ingangen doorzoeken op knooppunten, ongeveer 50.000 hierna prestaties worden mogelijk beïnvloed
* Azure Stack ondersteunt op dit moment geen bestanden, zodat u probeert om uit te breiden van bestanden wordt een fout

09/12/2016
### <a name="version-084"></a>Versie 0.8.4

>[!VIDEO https://www.youtube.com/embed/cr5tOGyGrIQ?ecver=1]

#### <a name="new"></a>Nieuw

* Directe koppelingen naar storage-accounts, containers, wachtrijen, tabellen of bestandsshares voor het delen van genereren en ondersteuning voor eenvoudige toegang tot uw resources - Windows- en Mac OS
* Zoeken naar uw blob-containers, tabellen, wachtrijen, -bestandsshares of storage-accounts in het zoekvak
* U kunt nu in de tabel-opbouwfunctie voor query's van de EU groeperen
* Wijzig de naam en kopiëren/plakken blob-containers, bestandsshares, tabellen, blobs, blob-mappen, bestanden en mappen uit in het SAS-gekoppelde accounts en -containers
* Wijzigen van de naam en het kopiëren van blob-containers en bestandsshares nu behouden, eigenschappen en metagegevens

#### <a name="fixes"></a>Oplossingen

* Vaste: kan geen tabelentiteiten bewerken als ze Booleaanse of binaire eigenschappen bevatten

#### <a name="known-issues"></a>Bekende problemen

* Search-ingangen doorzoeken op knooppunten, ongeveer 50.000 hierna prestaties worden mogelijk beïnvloed

08/03/2016
### <a name="version-083"></a>Versie 0.8.3

>[!VIDEO https://www.youtube.com/embed/HeGW-jkSd9Y?ecver=1]

#### <a name="new"></a>Nieuw

* Wijzig de naam van containers, tabellen, -bestandsshares
* Verbeterde ervaring voor de opbouwfunctie voor Query
* Mogelijkheid om te slaan en te laden van query 's
* Directe koppelingen naar storage-accounts of containers,-wachtrijen, tabellen, of bestandsshares voor het delen en eenvoudig toegang tot uw resources (alleen Windows - macOS ondersteuning binnenkort beschikbaar.)
* Mogelijkheid om te beheren en CORS-regels configureren

#### <a name="fixes"></a>Oplossingen

* Opgelost: Microsoft-Accounts moet opnieuw worden geverifieerd om de 8-12 uur

#### <a name="known-issues"></a>Bekende problemen

* Soms wordt de gebruikersinterface kan er uitzien bevroren - maximaliseren van het venster helpt dit probleem oplossen
* macOS-installatie mogelijk verhoogde machtigingen
* Het instellingenvenster account kan worden weergegeven dat u referenties invoeren moet om abonnementen filteren
* Naam van bestandsshares, blob-containers en tabellen behouden niet metagegevens of andere eigenschappen van de container, zoals het quotum voor de bestandsshare, niveau openbare toegang of beleidsregels voor toegang
* Naam wijzigen van blobs (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een wijzigen
* Kopiëren of verwijderen van resources werkt niet binnen de SAS-gekoppelde accounts

07/07/2016
### <a name="version-082"></a>Versie 0.8.2

>[!VIDEO https://www.youtube.com/embed/nYgKbRUNYZA?ecver=1]

#### <a name="new"></a>Nieuw

* Storage-Accounts zijn gegroepeerd op abonnementen. opslag en bronnen die zijn gekoppeld via de sleutel of SA's worden weergegeven onder het knooppunt (lokale en bijgevoegde)
* Meld u af van accounts in het Configuratiescherm "Azure-Accountinstellingen"
* Proxy-instellingen voor inschakelen en beheren van aanmelding configureren
* Maken en blob-lease verbreken
* Open blob-containers, wachtrijen, tabellen en bestanden met één klik

#### <a name="fixes"></a>Oplossingen

* Opgelost: berichten in de wachtrij ingevoegd met .NET of Java-bibliotheken zijn niet correct gedecodeerd op basis van base64
* Vaste: $metrics tabellen worden niet weergegeven voor Blob Storage-accounts
* Vaste: tabellen knooppunt werkt niet voor lokale (ontwikkeling)-opslag

#### <a name="known-issues"></a>Bekende problemen

* macOS-installatie mogelijk verhoogde machtigingen

06/15/2016
### <a name="version-080"></a>Versie 0.8.0

>[!VIDEO https://www.youtube.com/embed/ycfQhKztSIY?ecver=1]

>[!VIDEO https://www.youtube.com/embed/k4_kOUCZ0WA?ecver=1]

>[!VIDEO https://www.youtube.com/embed/3zEXJcGdl_k?ecver=1]

#### <a name="new"></a>Nieuw

* Share-ondersteuning: weergeven, uploaden, downloaden, het kopiëren van bestanden en mappen, SAS URI's (maken en verbinding maken)
* Verbeterde gebruikerservaring voor het verbinden met opslag met SAS URI's of account sleutels
* De resultaten van de query exporteren
* Tabel kolom opnieuw ordenen en aanpassen
* Blob-containers bekijken $logs en tabellen met $metrics voor Opslagaccounts met ingeschakelde metrische gegevens
* Verbeterde exporteren en importeren van gedrag, bevat nu type eigenschapswaarde

#### <a name="fixes"></a>Oplossingen

* Vaste: uploaden of downloaden van grote blobs kan leiden tot onvolledige uploaden/downloaden
* Vaste: bewerken, toe te voegen of importeren van een entiteit met een numerieke tekenreekswaarde ("1") zullen converteren naar double
* Opgelost: Kan niet uitbreiden van het knooppunt van de tabel in de lokale ontwikkelomgeving

#### <a name="known-issues"></a>Bekende problemen

* tabellen met $metrics zijn niet zichtbaar zijn voor Blob Storage-accounts
* Berichten in de wachtrij toegevoegd via een programma mogelijk niet correct weergegeven als de berichten zijn gecodeerd met Base64-codering

05/17/2016
### <a name="version-07201605090"></a>Versie 0.7.20160509.0

#### <a name="new"></a>Nieuw

* Betere foutafhandeling voor app-crashes

#### <a name="fixes"></a>Oplossingen

* Probleem opgelost waarbij informatiebalk berichten soms niet worden weergegeven als aanmeldingsreferenties vereist zijn

#### <a name="known-issues"></a>Bekende problemen

* Tabellen: Toevoegen, bewerken of importeren van een entiteit die een eigenschap met een ambiguously numerieke waarde, zoals "1" of "1.0", en de gebruiker probeert te verzenden als een `Edm.String`, de waarde wordt terugkeren via de client-API als een Edm.Double

03/31/2016

### <a name="version-07201603250"></a>Versie 0.7.20160325.0

>[!VIDEO https://www.youtube.com/embed/imbgBRHX65A?ecver=1]

>[!VIDEO https://www.youtube.com/embed/ceX-P8XZ-s8?ecver=1]

#### <a name="new"></a>Nieuw

* Ondersteuning voor tabel: weergeven, uitvoeren van query's, export, importeren en CRUD-bewerkingen voor entiteiten
* Ondersteuning voor de wachtrij: weergeven, toevoegen, dequeueing berichten
* Genereren van SAS-URI's voor Storage-Accounts
* Verbinding maken met Opslagaccounts met SAS URI 's
* Updatemeldingen voor toekomstige updates voor Storage Explorer
* Bijgewerkte uiterlijk

#### <a name="fixes"></a>Oplossingen

* Verbeteringen in de prestaties en betrouwbaarheid

### <a name="known-issues-amp-mitigations"></a>Bekende problemen &amp; oplossingen

* Downloaden van een grote blob-bestanden niet correct werkt: het is raadzaam met behulp van AzCopy terwijl wij dit probleem lossen
* Accountreferenties wordt niet worden opgehaald of in de cache opgeslagen als de basismap kan niet worden gevonden of kan niet worden geschreven
* Als we toevoegen, bewerken of importeren van een entiteit die een eigenschap met een ambiguously numerieke waarde, zoals "1" of "1.0", en de gebruiker probeert te verzenden als een `Edm.String`, de waarde wordt terugkeren via de client-API als een Edm.Double
* Bij het importeren van CSV-bestanden met meerdere regels records, kunnen de gegevens ophalen opgedeeld of versleuteld

02/03/2016

### <a name="version-07201601291"></a>Versie 0.7.20160129.1

#### <a name="fixes"></a>Oplossingen

* Verbeterde algehele prestaties bij het uploaden, downloaden en kopiëren van blobs

01/14/2016

### <a name="version-07201601050"></a>Versie 0.7.20160105.0

#### <a name="new"></a>Nieuw

* Linux-ondersteuning (pariteit functies voor OS x)
* Blob-containers met Shared Access Signatures (SAS)-sleutel toevoegen
* Opslagaccounts voor Azure China 21Vianet toevoegen
* Storage-Accounts met aangepaste eindpunten toevoegen
* Openen en de inhoud van tekst en afbeeldingen blobs weergeven
* Blob-eigenschappen en metagegevens weergeven en bewerken

#### <a name="fixes"></a>Oplossingen

* Opgelost: uploaden of downloaden van een groot aantal blobs (500 +) kan soms ertoe leiden dat de app een wit scherm
* Opgelost: bij het instellen van blob-container openbaar toegangsniveau, de nieuwe waarde wordt niet bijgewerkt totdat u de focus op de container voor het opnieuw instellen. Ook het dialoogvenster altijd standaard ingesteld op 'Geen openbare toegang' en niet de werkelijke huidige waarde.
* Betere algehele toetsenbord toegankelijkheid en de UI-ondersteuning
* Breadcrumbs geschiedenis tekst kan teruglopen wanneer het is lang met witruimte
* SAS-dialoogvenster ondersteunt validatie voor invoer
* Lokale opslag blijft beschikbaar, zelfs als de referenties van gebruiker zijn verlopen
* Wanneer een geopende blob-container wordt verwijderd, is de explorer blob aan de rechterkant gesloten

#### <a name="known-issues"></a>Bekende problemen

* Linux installeren moet gcc-versie is bijgewerkt of stappen voor het bijwerken worden bijgewerkt: hieronder:
    * `sudo add-apt-repository ppa:ubuntu-toolchain-r/test`
    * `sudo apt-get update`
    * `sudo apt-get upgrade`
    * `sudo apt-get dist-upgrade`

11/18/2015
### <a name="version-07201511160"></a>Versie 0.7.20151116.0

#### <a name="new"></a>Nieuw

* Mac OS en Windows-versies
* Aanmelden bij uw Storage-Accounts weergeven: gebruik van uw organisatie-Account, Microsoft-Account, 2FA, enzovoort.
* Lokale opslag (storage emulator, gebruik alleen-Windows)
* Ondersteuning voor Azure Resource Manager en klassieke resource
* Maken en verwijderen van blobs, wachtrijen en tabellen
* Zoeken naar specifieke blobs, wachtrijen en tabellen
* Bekijk de inhoud van de blob-containers
* Navigeren door mappen
* Uploaden, downloaden en verwijderen van blobs en mappen
* Blob-eigenschappen en metagegevens weergeven en bewerken
* SAS-sleutels genereren
* Beheren en maken van opgeslagen toegang beleid (SAP)
* Blobs op voorvoegsel zoeken
* Sleep 'n bestanden uploaden of downloaden

#### <a name="known-issues"></a>Bekende problemen

* Bij het instellen van blob-container openbaar toegangsniveau, is de nieuwe waarde niet bijgewerkt, totdat u de focus op de container voor het opnieuw instellen
* Wanneer u het dialoogvenster om in te stellen de niveau openbare toegang opent, weergegeven het 'Geen openbare toegang' altijd als de standaard- en niet de werkelijke huidige waarde
* Naam van gedownloade blobs niet wijzigen
* Activiteit logboekvermeldingen wordt soms "zitten" in een actieve status wanneer een fout optreedt, en de fout wordt niet weergegeven.
* Soms vastloopt of wordt volledig wit bij het uploaden of downloaden van een groot aantal blobs
* Soms annuleren van een kopieerbewerking werkt niet
* Tijdens het maken van een container (wachtrij-blob/tabel), als u een ongeldige naam invoeren en doorgaan met het maken van een andere onder een andere container-type u kan niet de focus instellen op het nieuwe type
* Kan de nieuwe map maken of wijzig de map




[2]: https://support.microsoft.com/en-us/help/4021389/storage-explorer-troubleshooting-guide
[3]: vs-azure-tools-storage-manage-with-storage-explorer.md
