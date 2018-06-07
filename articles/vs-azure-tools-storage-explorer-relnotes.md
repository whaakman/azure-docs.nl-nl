---
title: Opmerkingen bij de release van Microsoft Azure Storage Explorer
description: Releaseopmerkingen voor Microsoft Azure Storage Explorer
services: storage
documentationcenter: na
author: cawa
manager: paulyuk
editor: ''
ms.assetid: ''
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2017
ms.author: cawa
ms.openlocfilehash: 2878fb737f5daa875b91aefc77c6b8bc495f917e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34657567"
---
# <a name="microsoft-azure-storage-explorer-release-notes"></a>Opmerkingen bij de release van Microsoft Azure Storage Explorer

Dit artikel bevat de releaseopmerkingen voor Azure Storage Explorer 1.0.0 release, evenals de releaseopmerkingen voor eerdere versies.

[Microsoft Azure Storage Explorer](./vs-azure-tools-storage-manage-with-storage-explorer.md) is een zelfstandige app waardoor u eenvoudig werken met Azure Storage-gegevens op Windows-, Mac OS- en Linux.

## <a name="version-110"></a>Versie 1.1.0
09-05/2018

### <a name="download-azure-storage-explorer-110"></a>Azure Storage Explorer 1.1.0 downloaden
- [Azure Opslagverkenner 1.1.0 voor Windows](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Azure Opslagverkenner 1.1.0 voor Mac](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Azure Opslagverkenner 1.1.0 voor Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>Nieuw
* Opslagverkenner ondersteunt nu het gebruik van Azurite. Opmerking: de verbinding met Azurite is vastgelegd op de Standaardeindpunten voor ontwikkeling.
* Opslagverkenner ondersteunt nu Toegangslagen voor Blob alleen en GPV2 Storage-Accounts. Meer informatie over Toegangslagen [hier](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-storage-tiers).
* Een begintijd is niet langer vereist bij het genereren van een SAS.

### <a name="fixes"></a>Oplossingen
* Ophalen van de abonnementen voor US Government-accounts is verbroken. Dit probleem is opgelost. [#61](https://github.com/Microsoft/AzureStorageExplorer/issues/61)
* De verlooptijd voor toegangsbeleid is correct niet opgeslagen. Dit probleem is opgelost. [#50](https://github.com/Microsoft/AzureStorageExplorer/issues/50)
* Bij het genereren van een SAS-URL voor een item in een container, is niet de naam van het item wordt toegevoegd aan de URL. Dit probleem is opgelost. [#44](https://github.com/Microsoft/AzureStorageExplorer/issues/44)
* Bij het maken van een SAS zou verstrijken tijden dat in het verleden soms de standaardwaarde is. Dit is vanwege Storage Explorer met behulp van die de laatste begin-en verloopdatum als standaardwaarden gebruikt. Telkens wanneer u het dialoogvenster SAS opent, wordt nu een nieuwe reeks standaardwaarden gegenereerd. [#35](https://github.com/Microsoft/AzureStorageExplorer/issues/35)
* Bij het kopiëren tussen Opslagaccounts, is een 24-uurs SAS gegenereerd. Als u de kopie geduurd meer dan 24 uur, zou de kopie mislukken. We hebt de SAS van voor de afgelopen week om te verminderen de kans op een kopie is mislukt vanwege een verlopen SAS verhoogd. [#62](https://github.com/Microsoft/AzureStorageExplorer/issues/62)
* Voor sommige activiteiten werkt te klikken op 'Annuleren' altijd niet. Dit probleem is opgelost. [#125](https://github.com/Microsoft/AzureStorageExplorer/issues/125)
* Voor sommige activiteiten zijn de overdrachtssnelheid is onjuist. Dit probleem is opgelost. [#124](https://github.com/Microsoft/AzureStorageExplorer/issues/124)
* De spelling van 'Vorige' in het menu weergave is onjuist. Nu is juist gespeld. [#71](https://github.com/Microsoft/AzureStorageExplorer/issues/71)
* De laatste pagina van het Windows-installatieprogramma heeft een knop 'Volgende'. Het is gewijzigd in een knop 'Voltooid'. [#70](https://github.com/Microsoft/AzureStorageExplorer/issues/70)
* Tabblad focus is niet zichtbaar voor knoppen in dialoogvensters bij gebruik van het thema CH zwart. Het is nu zichtbaar. [#64](https://github.com/Microsoft/AzureStorageExplorer/issues/64)
* Het hoofdlettergebruik van 'Automatisch oplossen' voor acties in het gebeurtenissenlogboek is onjuist. Het is nu juist is. [#51](https://github.com/Microsoft/AzureStorageExplorer/issues/51)
* Bij het verwijderen van een entiteit uit een tabel, wordt in het dialoogvenster waarin u om bevestiging wordt gevraagd een foutpictogram weergegeven. Het dialoogvenster gebruikt nu een waarschuwingspictogram weergegeven. [#148](https://github.com/Microsoft/AzureStorageExplorer/issues/148)

### <a name="known-issues"></a>Bekende problemen
* Als u tegenover voor Mac gebruiken en ooit een aangepaste AAD-configuratie hebt gemaakt, is het wellicht niet mogelijk om aan te melden. Verwijder de inhoud van het probleem te verhelpen, ~ /. IdentityService/AadConfigurations. Als u in dat geval heeft niet de blokkering opheffen u, stuur een reactie op [dit probleem](https://github.com/Microsoft/AzureStorageExplorer/issues/97).
* Azurite is nog niet volledig geïmplementeerd alle Storage-API's. Als gevolg hiervan kunnen er onverwachte fouten of gedrag Azurite voor ontwikkeling opslag gebruikt.
* In zeldzame gevallen kan de focus structuur ophalen achtergebleven op Snelweergavetoegang. Registreer de focus, kunt u Alles vernieuwen.
* Uploaden van de map OneDrive werkt niet vanwege een fout in NodeJS. De fout is opgelost, maar nog niet is geïntegreerd in Electron.
* Wanneer u ontwikkelt voor Azure-Stack, mislukken uploaden van bepaalde bestanden zoals toevoeg-blobs.
* Wanneer u op 'Annuleren' voor een taak, duurt het even voor die taak te annuleren. Dit is omdat we de annuleren filter tijdelijke oplossing gebruiken [hier](https://github.com/Azure/azure-storage-node/issues/317). 
* Als u de verkeerde PINCODE/smartcardcertificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Naam van de BLOB's (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam te wijzigen.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde Azure-Stack storage-account.
* De Electron shell die wordt gebruikt door Opslagverkenner heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Opslagverkenner is leeg (leeg) hoofdvenster weer te geven, kunt u proberen Storage Explorer te starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door het toevoegen van de `--disable-gpu` switch:

```
./StorageExplorer.exe --disable-gpu
```

* Voor Linux-gebruikers, moet u installeren [.NET Core 2.0](https://docs.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is up-to-date - kunt u dit doen door de volgende opdrachten uit te voeren en de computer opnieuw te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* U moet installeren GConf voor gebruikers op Ubuntu 17.04 - kunt u dit doen door de volgende opdrachten uit te voeren en de computer opnieuw te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="previous-releases"></a>Eerdere versies

* [Versie 1.0.0](#version-100)
* [Versie 0.9.6](#version-096)
* [Versie 0.9.5](#version-095)
* [Versie 0.9.4 en 0.9.3](#version-094-and-093)
* [Versie 0.9.2](#version-092)
* [Versie 0.9.1 en 0.9.0](#version-091-and-090)
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

## <a name="version-100"></a>Versie 1.0.0
16-04/2018

### <a name="download-azure-storage-explorer-100"></a>Azure Storage Explorer 1.0.0 downloaden
- [Azure Opslagverkenner 1.0.0 voor Windows](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Azure Opslagverkenner 1.0.0 voor Mac](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Azure Opslagverkenner 1.0.0 voor Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>Nieuw
* Verbeterde verificatie waarmee Opslagverkenner de store te gebruiken hetzelfde account als Visual Studio 2017. Deze functie wilt gebruiken, moet u aanmelding aan uw accounts en uw gefilterde abonnementen opnieuw instellen.
* Voor back-up AAD Azure Stack-accounts, zal Opslagverkenner nu Stack Azure-abonnementen ophalen als 'Doel Azure Stack' is ingeschakeld. Niet langer hoeft te maken van een aangepaste aanmelding-omgeving.
* Verschillende snelkoppelingen zijn toegevoegd aan het sneller navigeren in te schakelen. Deze omvatten diverse deelvensters schakelen en verplaatsen tussen editors. Zie het menu Beeld voor meer informatie.
* Storage Explorer feedback zich nu op GitHub. U kunt onze pagina met problemen bereiken door te klikken op de knop Feedback in de onderste left of door te gaan [ https://github.com/Microsoft/AzureStorageExplorer/issues ](https://github.com/Microsoft/AzureStorageExplorer/issues). U kunt suggesties, rapporteren van problemen, vragen stellen of een andere vorm van feedback laat.
* Als u in de SSL-certificaat problemen worden uitgevoerd en kan de strijdige certificaat niet vinden, kunt u nu Opslagverkenner starten vanaf de opdrachtregel met de `--ignore-certificate-errors` vlag. Wanneer met deze markering wordt gestart, wordt in Opslagverkenner SSL certificaatfouten negeren.
* Er is nu een downloadoptie '' in het contextmenu voor blob- en bestand.
* Betere toegankelijkheid en ondersteuning voor schermlezers. Als u gebruik maakt van toegankelijkheidsfuncties, raadpleegt u onze [toegankelijkheid documentatie](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-explorer-accessibility) voor meer informatie.
* Opslagverkenner gebruikt nu Electron 1.8.3

### <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken
* Opslagverkenner is overgeschakeld naar een nieuwe verificatiebibliotheek. Als onderdeel van de switch aan de bibliotheek moet u aanmelding aan uw accounts en uw gefilterde abonnementen opnieuw instellen
* De methode die wordt gebruikt voor het versleutelen van gevoelige gegevens is gewijzigd. Dit kan leiden tot sommige objecten Snelweergavetoegang hoeven moet opnieuw worden toegevoegd en/of u gekoppeld resources hoeven opnieuw worden gekoppeld.

### <a name="fixes"></a>Oplossingen
* Sommige gebruikers achter proxy's zou hebben groep blob uploads of downloads onderbroken door een 'kan niet omzetten' foutbericht. Dit probleem is opgelost.
* Als het aanmelden is vereist, terwijl met behulp van een directe koppeling te klikken op de prompt Sign In zou pop-updialoogvenster een leeg. Dit probleem is opgelost.
* Op Linux, als Opslagverkenner kan niet starten vanwege het vastlopen van een proces GPU ontvangt u nu een bericht van de crash, adviseert gebruik van de '--uitschakelen gpu' switch- en Storage Explorer wordt vervolgens automatisch opnieuw opgestart met de schakeloptie ingeschakeld.
* Ongeldige toegangsbeleid zijn moeilijk te identiteit in het dialoogvenster toegangsbeleid. Ongeldige toegangsbeleid id's worden nu in rood voor meer zichtbaarheid beschreven.
* Het activiteitenlogboek moet soms grote gebieden met witruimte tussen de verschillende onderdelen van een activiteit. Dit probleem is opgelost.
* Als u een component timestamp links in een ongeldige status en vervolgens heeft geprobeerd te wijzigen van een andere component zou in de tabel query-editor de editor blokkeren. De editor wordt nu de component timestamp herstellen in de laatste geldige status wanneer een wijziging in een andere-component wordt gedetecteerd.
* Als u onderbroken terwijl u in uw zoekopdracht in de structuurweergave, zou u gezocht en focus vanaf het tekstvak zou worden gestolen. Nu moet u expliciet starten door op de Enter-toets te drukken of door te klikken op de knop start zoeken zoeken.
* De opdracht 'Shared Access Signature ophalen', soms uitgeschakeld wanneer de rechtermuisknop te klikken op een bestand in een bestandsshare. Dit probleem is opgelost.
* Als het structuurknooppunt resource met de focus is gefilterd tijdens de zoekopdracht, kan u niet tabblad in de resourcestructuur en gebruik de pijltoetsen om de resourcestructuur van de te bladeren. Nu als het structuurknooppunt gerichte resource is verborgen, zal het eerste knooppunt in de resourcestructuur van de worden automatisch gericht.
* Een extra scheidingsteken zou soms zijn zichtbaar in de werkbalk. Dit probleem is opgelost.
* In het tekstvak breadcrumb overflow soms. Dit probleem is opgelost.
* De Blob en bestandsshare editors zou soms voortdurend vernieuwd wanneer veel bestanden tegelijk uploaden. Dit probleem is opgelost.
* De functie 'Map statistieken' is geen doel in de weergave Bestandsbeheer Share momentopnamen. Dit is nu uitgeschakeld.
* Het menu bestand op Linux, niet worden weergegeven. Dit probleem is opgelost.
* Tijdens het uploaden van een map naar een bestandsshare, standaard, wordt alleen de inhoud van de map zijn geüpload. Nu is het standaardgedrag voor het uploaden van de inhoud van de map in een overeenkomende map in de bestandsshare.
* De volgorde van de knoppen in verschillende dialoogvensters had omgekeerd. Dit probleem is opgelost.
* Verschillende oplossingen voor beveiliging.

### <a name="known-issues"></a>Bekende problemen
* In zeldzame gevallen kan de focus structuur ophalen achtergebleven op Snelweergavetoegang. Registreer de focus, kunt u Alles vernieuwen.
* Wanneer u ontwikkelt voor Azure-Stack, mislukken uploaden van bepaalde bestanden zoals toevoeg-blobs.
* Wanneer u op 'Annuleren' voor een taak, duurt het even voor die taak te annuleren. Dit is omdat we de annuleren filter tijdelijke oplossing hier gebruiken. 
* Als u de verkeerde PINCODE/smartcardcertificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Naam van de BLOB's (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam te wijzigen.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde Azure-Stack storage-account.
* De Electron shell die wordt gebruikt door Opslagverkenner heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Opslagverkenner is leeg (leeg) hoofdvenster weer te geven, kunt u proberen Storage Explorer te starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door het toevoegen van de `--disable-gpu` switch:

```
./StorageExplorer.exe --disable-gpu
```

* Voor Linux-gebruikers, moet u installeren [.NET Core 2.0](https://docs.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore2x).
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is up-to-date - kunt u dit doen door de volgende opdrachten uit te voeren en de computer opnieuw te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* U moet installeren GConf voor gebruikers op Ubuntu 17.04 - kunt u dit doen door de volgende opdrachten uit te voeren en de computer opnieuw te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-096"></a>Versie 0.9.6
28-02/2018

### <a name="download-azure-storage-explorer-096-preview"></a>Azure Storage Explorer 0.9.6 (Preview) downloaden
- [Azure Opslagverkenner (Preview) 0.9.6 voor Windows](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Azure Opslagverkenner (Preview) 0.9.6 voor Mac](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Azure Opslagverkenner (Preview) 0.9.6 voor Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="fixes"></a>Oplossingen
* Een probleem niet wordt vermeld in de editor verwachte blobs en-bestanden. Dit probleem is opgelost.
* Een probleem veroorzaakt schakelen tussen momentopname weergaven items onjuist wordt weergegeven. Dit probleem is opgelost.

### <a name="known-issues"></a>Bekende problemen
* Opslagverkenner biedt geen ondersteuning voor AD FS-accounts.
* Wanneer u ontwikkelt voor Azure-Stack, mislukken uploaden van bepaalde bestanden zoals toevoeg-blobs.
* Wanneer u op 'Annuleren' voor een taak, duurt het even voor die taak te annuleren. Dit is omdat we de annuleren filter tijdelijke oplossing gebruiken [hier](https://github.com/Azure/azure-storage-node/issues/317).
* Als u de verkeerde PINCODE/smartcardcertificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Het paneel met toepassingsinstellingen account kan worden weergegeven dat u moet referenties voor het filteren van abonnementen opnieuw invoeren.
* Naam van de BLOB's (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam te wijzigen.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde Azure-Stack storage-account.
* De Electron shell die wordt gebruikt door Opslagverkenner heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Opslagverkenner is leeg (leeg) hoofdvenster weer te geven, kunt u proberen Storage Explorer te starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door het toevoegen van de `--disable-gpu` switch:

```
./StorageExplorer.exe --disable-gpu
```

* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is up-to-date - kunt u dit doen door de volgende opdrachten uit te voeren en de computer opnieuw te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* U moet installeren GConf voor gebruikers op Ubuntu 17.04 - kunt u dit doen door de volgende opdrachten uit te voeren en de computer opnieuw te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-095"></a>Versie 0.9.5
06-02/2018

### <a name="download-azure-storage-explorer-095-preview"></a>Azure Storage Explorer 0.9.5 (Preview) downloaden
- [Azure Opslagverkenner (Preview) 0.9.5 voor Windows](https://go.microsoft.com/fwlink/?LinkId=708343)
- [Azure Opslagverkenner (Preview) 0.9.5 voor Mac](https://go.microsoft.com/fwlink/?LinkId=708342)
- [Azure Opslagverkenner (Preview) 0.9.5 voor Linux](https://go.microsoft.com/fwlink/?LinkId=722418)

### <a name="new"></a>Nieuw

* Ondersteuning voor bestandsshares momentopnamen:
    * Momentopnamen maken en beheren voor uw bestandsshares.
    * Eenvoudig schakelen tussen momentopnamen van uw bestandsshares als kunt verkennen.
    * Vorige versies van uw bestanden te herstellen.
* Preview-ondersteuning voor Azure Data Lake Store:
    * Verbinding maken met uw ADLS-resources via meerdere accounts.
    * Verbinding maken met en ADLS-resources met behulp van ADL URI's delen.
    * Basic bestandsmap operations recursief uitvoeren.
    * Pincode afzonderlijke mappen voor Snelweergavetoegang.
    * De statistieken van de map weergeven.

### <a name="fixes"></a>Oplossingen
* Starten van de prestaties verbeterd.
* Verschillende oplossingen voor problemen.

### <a name="known-issues"></a>Bekende problemen
* Opslagverkenner biedt geen ondersteuning voor AD FS-accounts.
* Wanneer u ontwikkelt voor Azure-Stack, mislukken uploaden van bepaalde bestanden zoals toevoeg-blobs.
* Wanneer u op 'Annuleren' voor een taak, duurt het even voor die taak te annuleren. Dit is omdat we de annuleren filter tijdelijke oplossing hier gebruiken.
* Als u de verkeerde PINCODE/smartcardcertificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Het paneel met toepassingsinstellingen account kan worden weergegeven dat u moet referenties voor het filteren van abonnementen opnieuw invoeren.
* Naam van de BLOB's (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam te wijzigen.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde Azure-Stack storage-account.
* De Electron shell die wordt gebruikt door Opslagverkenner heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Opslagverkenner is leeg (leeg) hoofdvenster weer te geven, kunt u proberen Storage Explorer te starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door het toevoegen van de `--disable-gpu` switch:

```
./StorageExplorer.exe --disable-gpu
```

* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is up-to-date - kunt u dit doen door de volgende opdrachten uit te voeren en de computer opnieuw te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* U moet installeren GConf voor gebruikers op Ubuntu 17.04 - kunt u dit doen door de volgende opdrachten uit te voeren en de computer opnieuw te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-094-and-093"></a>Versie 0.9.4 en 0.9.3
01/21/2018

### <a name="download-azure-storage-explorer-094-preview"></a>Azure Storage Explorer 0.9.4 (Preview) downloaden
* [Download de Azure Opslagverkenner (Preview) 0.9.4 voor Windows](https://go.microsoft.com/fwlink/?LinkId=809306)
* [Download de Azure Opslagverkenner (Preview) 0.9.4 voor Mac](https://go.microsoft.com/fwlink/?LinkId=809307)
* [Azure Opslagverkenner (Preview) 0.9.4 voor Linux downloaden](https://go.microsoft.com/fwlink/?LinkId=809308)

### <a name="new"></a>Nieuw
* Uw bestaande Storage Explorer-venster wordt niet opnieuw worden gebruikt wanneer:
    * Directe koppelingen gegenereerd in Opslagverkenner openen.
    * Opslagverkenner het openen van de portal.
    * Het openen van Storage Explorer van Azure Storage tegenover Code-uitbreiding (binnenkort).
* De mogelijkheid om een nieuwe Storage Explorer-venster openen vanuit Opslagverkenner is toegevoegd.
    * Voor Windows is er een optie 'Nieuw venster' in het contextmenu van de taakbalk en Menu bestand.
    * Voor Mac is er een optie 'Nieuw venster' onder App-Menu.

### <a name="fixes"></a>Oplossingen
* Een beveiligingsprobleem opgelost. Upgrade naar 0.9.4 op vroegste gemak.
* Oude activiteiten zijn niet op de juiste wijze worden opgeruimd. Dit van invloed op de prestaties van langdurige taken. Ze worden nu wordt opgeschoond correct.
* Acties met betrekking tot groot aantal bestanden en mappen, zou soms Opslagverkenner blokkeren. Aanvragen voor Azure voor bestandsshares zijn nu trottled system resource gebruik beperken.

### <a name="known-issues"></a>Bekende problemen
* Opslagverkenner biedt geen ondersteuning voor AD FS-accounts.
* Sneltoetsen voor 'Weergave Explorer' en 'Accountbeheer weergave' moet Ctrl / Cmd + Shift + E en Ctrl / Cmd + Shift + A respectievelijk.
* Wanneer u ontwikkelt voor Azure-Stack, mislukken uploaden van bepaalde bestanden zoals toevoeg-blobs.
* Wanneer u op 'Annuleren' voor een taak, duurt het even voor die taak te annuleren. Dit is omdat we de annuleren filter tijdelijke oplossing hier gebruiken.
* Als u de verkeerde PINCODE/smartcardcertificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Het paneel met toepassingsinstellingen account kan worden weergegeven dat u moet referenties voor het filteren van abonnementen opnieuw invoeren.
* Naam van de BLOB's (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam te wijzigen.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde Azure-Stack storage-account.
* De Electron shell die wordt gebruikt door Opslagverkenner heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Opslagverkenner is leeg (leeg) hoofdvenster weer te geven, kunt u proberen Storage Explorer te starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door het toevoegen van de `--disable-gpu` switch:
```
./StorageExplorer --disable-gpu
```
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is up-to-date - kunt u dit doen door de volgende opdrachten uit te voeren en de computer opnieuw te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* U moet installeren GConf voor gebruikers op Ubuntu 17.04 - kunt u dit doen door de volgende opdrachten uit te voeren en de computer opnieuw te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-092"></a>Versie 0.9.2
11/01/2017

### <a name="hotfixes"></a>Hotfixes
* Er zijn onverwachte gegevenswijzigingen zijn mogelijk bij het bewerken van Edm.DateTime waarden voor de tabelentiteiten, afhankelijk van de lokale tijdzone. De editor gebruikt nu een vak tekst zonder opmaak waardoor nauwkeurige, consistente controle hebt over Edm.DateTime waarden.
* Een groep van blobs wanneer gekoppeld met de naam en sleutel uploaden/downloaden zou niet starten. Dit probleem is opgelost.
* Eerder Opslagverkenner zouden alleen gevraagd of u wilt een verlopen account verifiëren als een of meer van de account abonnementen is geselecteerd. Nu wordt Opslagverkenner u gevraagd zelfs als het account is volledig wordt gefilterd.
* Het domein eindpunten voor het Azure US Government is onjuist. Het is opgelost.
* De knop toepassen op het deelvenster Accounts beheren was soms moeilijk te klikken. Dit mag niet langer gebeuren.

### <a name="new"></a>Nieuw
* Preview-ondersteuning voor Azure Cosmos DB:
    * [Online documentatie](./cosmos-db/storage-explorer.md)
    * Databases en verzamelingen maken
    * Gegevens bewerken
    * Een query, maken of verwijderen van documenten
    * Opgeslagen procedures, door de gebruiker gedefinieerde functies of triggers bijwerken
    * Gebruik verbindingsreeksen met verbinding maken met en beheren van uw databases
* Verbetert de prestaties van veel kleine blobs uploaden/downloaden.
* Een 'Opnieuw alle' actie toegevoegd als er fouten in een blob uploaden groep of blob downloaden.
* Opslagverkenner pauzeert nu herhaling tijdens blob uploaden/downloaden als wordt gedetecteerd dat de netwerkverbinding is verbroken. U kunt vervolgens herhaling hervatten zodra de netwerkverbinding hersteld is.
* De mogelijkheid 'Sluiten alle', "Sluiten anderen" en "Sluiten" tabbladen via contextmenu toegevoegd.
* Opslagverkenner gebruikt nu systeemeigen dialoogvensters en systeemeigen context menu's.
* Opslagverkenner is nu beter toegankelijk. Verbeteringen zijn onder andere:
    * Verbeterde ondersteuning voor schermlezers, voor NVDA in Windows en voor Voice-over op Mac
    * Verbeterde hoog contrast thema 's
    * TAB-toets en toetsenbord toetsenbordfocus wordt opgelost

### <a name="fixes"></a>Oplossingen
* Als u probeert te openen of downloaden van een blob met een ongeldige naam van de Windows-bestand, mislukken de bewerking. Opslagverkenner wordt nu detecteren of een blob-naam ongeldig is en vraag of u wilt deze coderen noch overslaan van de blob. Opslagverkenner detecteert ook als een bestandsnaam worden gecodeerd en vraag u weergegeven als wilt voordat u uploadt worden ontsleuteld.
* Tijdens het uploaden van blob de editor voor de doel-blob-container zou soms niet goed worden vernieuwd. Dit probleem is opgelost.
* De ondersteuning voor verschillende soorten verbindingsreeksen en SAS-URI's opgelost. We alle bekende problemen hebt opgelost, maar Geef feedback verzenden als u meer problemen.
* Het updatebericht dat is verbroken voor sommige gebruikers in 0.9.0. Dit probleem is opgelost en die van invloed op een door de fout, kunt u handmatig de nieuwste versie van Opslagverkenner downloaden [hier](https://azure.microsoft.com/features/storage-explorer/).

### <a name="known-issues"></a>Bekende problemen
* Opslagverkenner biedt geen ondersteuning voor AD FS-accounts.
* Sneltoetsen voor 'Weergave Explorer' en 'Accountbeheer weergave' moet Ctrl / Cmd + Shift + E en Ctrl / Cmd + Shift + A respectievelijk.
* Wanneer u ontwikkelt voor Azure-Stack, mislukken uploaden van bepaalde bestanden zoals toevoeg-blobs.
* Wanneer u op 'Annuleren' voor een taak, duurt het even voor die taak te annuleren. Dit is omdat we de annuleren filter tijdelijke oplossing hier gebruiken.
* Als u de verkeerde PINCODE/smartcardcertificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Het paneel met toepassingsinstellingen account kan worden weergegeven dat u moet referenties voor het filteren van abonnementen opnieuw invoeren.
* Naam van de BLOB's (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam te wijzigen.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde Azure-Stack storage-account.
* De Electron shell die wordt gebruikt door Opslagverkenner heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Opslagverkenner is leeg (leeg) hoofdvenster weer te geven, kunt u proberen Storage Explorer te starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door het toevoegen van de `--disable-gpu` switch:
```
./StorageExplorer --disable-gpu
```
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is up-to-date - kunt u dit doen door de volgende opdrachten uit te voeren en de computer opnieuw te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* U moet installeren GConf voor gebruikers op Ubuntu 17.04 - kunt u dit doen door de volgende opdrachten uit te voeren en de computer opnieuw te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-091-and-090"></a>Versie 0.9.1 en 0.9.0
10/20/2017
### <a name="new"></a>Nieuw
* Preview-ondersteuning voor Azure Cosmos DB:
    * [Online documentatie](./cosmos-db/storage-explorer.md)
    * Databases en verzamelingen maken
    * Gegevens bewerken
    * Een query, maken of verwijderen van documenten
    * Opgeslagen procedures, door de gebruiker gedefinieerde functies of triggers bijwerken
    * Gebruik verbindingsreeksen met verbinding maken met en beheren van uw databases
* Verbetert de prestaties van veel kleine blobs uploaden/downloaden.
* Een 'Opnieuw alle' actie toegevoegd als er fouten in een blob uploaden groep of blob downloaden.
* Opslagverkenner pauzeert nu herhaling tijdens blob uploaden/downloaden als wordt gedetecteerd dat de netwerkverbinding is verbroken. U kunt vervolgens herhaling hervatten zodra de netwerkverbinding hersteld is.
* De mogelijkheid 'Sluiten alle', "Sluiten anderen" en "Sluiten" tabbladen via contextmenu toegevoegd.
* Opslagverkenner gebruikt nu systeemeigen dialoogvensters en systeemeigen context menu's.
* Opslagverkenner is nu beter toegankelijk. Verbeteringen zijn onder andere:
    * Verbeterde ondersteuning voor schermlezers, voor NVDA in Windows en voor Voice-over op Mac
    * Verbeterde hoog contrast thema 's
    * TAB-toets en toetsenbord toetsenbordfocus wordt opgelost

### <a name="fixes"></a>Oplossingen
* Als u probeert te openen of downloaden van een blob met een ongeldige naam van de Windows-bestand, mislukken de bewerking. Opslagverkenner wordt nu detecteren of een blob-naam ongeldig is en vraag of u wilt deze coderen noch overslaan van de blob. Opslagverkenner detecteert ook als een bestandsnaam worden gecodeerd en vraag u weergegeven als wilt voordat u uploadt worden ontsleuteld.
* Tijdens het uploaden van blob de editor voor de doel-blob-container zou soms niet goed worden vernieuwd. Dit probleem is opgelost.
* De ondersteuning voor verschillende soorten verbindingsreeksen en SAS-URI's opgelost. We alle bekende problemen hebt opgelost, maar Geef feedback verzenden als u meer problemen.
* Het updatebericht dat is verbroken voor sommige gebruikers in 0.9.0. Dit probleem is opgelost en die van invloed op een door de fout, kunt u handmatig de nieuwste versie van Opslagverkenner downloaden [hier](https://azure.microsoft.com/features/storage-explorer/)

### <a name="known-issues"></a>Bekende problemen
* Opslagverkenner biedt geen ondersteuning voor AD FS-accounts.
* Sneltoetsen voor 'Weergave Explorer' en 'Accountbeheer weergave' moet Ctrl / Cmd + Shift + E en Ctrl / Cmd + Shift + A respectievelijk.
* Wanneer u ontwikkelt voor Azure-Stack, mislukken uploaden van bepaalde bestanden zoals toevoeg-blobs.
* Wanneer u op 'Annuleren' voor een taak, duurt het even voor die taak te annuleren. Dit is omdat we de annuleren filter tijdelijke oplossing hier gebruiken.
* Als u de verkeerde PINCODE/smartcardcertificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Het paneel met toepassingsinstellingen account kan worden weergegeven dat u moet referenties voor het filteren van abonnementen opnieuw invoeren.
* Naam van de BLOB's (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam te wijzigen.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde Azure-Stack storage-account.
* De Electron shell die wordt gebruikt door Opslagverkenner heeft problemen met sommige hardwareversnelling GPU (graphics processing unit). Als u Opslagverkenner is leeg (leeg) hoofdvenster weer te geven, kunt u proberen Storage Explorer te starten vanaf de opdrachtregel en GPU-versnelling uitschakelen door het toevoegen van de `--disable-gpu` switch:
```
./StorageExplorer --disable-gpu
```
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is up-to-date - kunt u dit doen door de volgende opdrachten uit te voeren en de computer opnieuw te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* U moet installeren GConf voor gebruikers op Ubuntu 17.04 - kunt u dit doen door de volgende opdrachten uit te voeren en de computer opnieuw te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

## <a name="version-0816"></a>Versie 0.8.16
8/21/2017

### <a name="new"></a>Nieuw
* Wanneer u een blob opent, wordt Opslagverkenner u gevraagd het gedownloade bestand uploaden als een wijziging wordt gedetecteerd
* Verbeterde Azure Stack-aanmeldingservaring aanpast
* De prestaties van uploaden/downloaden van veel kleine bestanden op hetzelfde moment verbeterd


### <a name="fixes"></a>Oplossingen
* Voor bepaalde typen blob zou kiezen tijdens een conflict uploaden "vervangen" soms leiden tot het uploaden wordt opnieuw opgestart.
* In versie 0.8.15, zou soms uploads achterstallige voor 99%.
* Wanneer het uploaden van bestanden naar een bestandsshare, als u wilt uploaden naar een map die niet nog bestaan, mislukt het uploaden.
* Opslagverkenner is niet juist genereren van tijdstempels voor handtekeningen voor gedeelde toegang en tabel query's.


### <a name="known-issues"></a>Bekende problemen
* Met behulp van een naam en sleutel verbindingsreeks werkt momenteel niet. Het wordt opgelost in de volgende release. Tot die tijd kunt u met de naam en sleutel koppelen.
* Als u probeert een bestand met een ongeldige naam van de Windows-bestand te openen, wordt het downloaden van de leidt tot een bestand is niet gevonden fout.
* Wanneer u op 'Annuleren' voor een taak, duurt het even voor die taak te annuleren. Dit is een beperking van de bibliotheek Azure Storage-knooppunt.
* Nadat het uploaden van een blob is voltooid, wordt het tabblad die het uploaden wordt gestart vernieuwd. Dit is een wijziging ten opzichte van vorige gedrag en ook wordt u terug naar de hoofdmap van de container die u in staat worden genomen.
* Als u de verkeerde PINCODE/smartcardcertificaat kiest, moet u opnieuw opstarten om Opslagverkenner besluit vergeet.
* Het paneel met toepassingsinstellingen account kan worden weergegeven dat u moet referenties voor het filteren van abonnementen opnieuw invoeren.
* Naam van de BLOB's (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam te wijzigen.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde Azure-Stack storage-account.
* Voor gebruikers op Ubuntu 14.04, moet u ervoor zorgen GCC is up-to-date - kunt u dit doen door de volgende opdrachten uit te voeren en de computer opnieuw te starten:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

* U moet installeren GConf voor gebruikers op Ubuntu 17.04 - kunt u dit doen door de volgende opdrachten uit te voeren en de computer opnieuw te starten:

    ```
    sudo apt-get install libgconf-2-4
    ```

### <a name="version-0814"></a>Versie 0.8.14
06/22/2017

### <a name="new"></a>Nieuw

* Bijgewerkte Electron versie 1.7.2 om te profiteren van verschillende kritieke beveiligingsupdates
* U kunt nu snel toegang tot de online gids voor probleemoplossing in het menu help
* Opslagverkenner probleemoplossing [handleiding][2]
* [Instructies] [ 3] voor het verbinden met een Azure-Stack-abonnement

### <a name="known-issues"></a>Bekende problemen

* Knoppen op het dialoogvenster voor bevestiging map verwijderen registreren niet bij de muisklikken op Linux. Tijdelijke oplossing is met de Enter-toets
* Als u ervoor het verkeerde PINCODE/smartcardcertificaat kiest wordt moet u opnieuw opstarten om de beslissing vergeet Opslagverkenner
* Als meer dan 3 groepen van blobs of tegelijk uploaden van bestanden, kan er fouten optreden
* Het paneel met toepassingsinstellingen account kan tonen, moet u referenties opnieuw invoeren om abonnementen filteren
* Naam van de BLOB's (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam te wijzigen.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde Azure-Stack storage-account.
* Ubuntu 14.04 installeren moet gcc-versie is bijgewerkt of stappen voor het upgraden zijn bijgewerkt: hieronder:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```

### <a name="version-0813"></a>Versie 0.8.13
12/05/2017

#### <a name="new"></a>Nieuw

* Opslagverkenner probleemoplossing [handleiding][2]
* [Instructies] [ 3] voor het verbinden met een Azure-Stack-abonnement

#### <a name="fixes"></a>Oplossingen

* Vaste: Uploaden bestand heeft een hoog risico dat een out-of geheugenfout
* Vaste: U kunt nu aanmelden met PINCODE/Smartcard
* Vaste: Open in de Portal nu werkt met Azure China, Duitse Azure, Azure US Government en Azure-Stack
* Vaste: Tijdens het uploaden van een map naar een blob-container, 'Ongeldige bewerking' zou soms treedt er een fout
* Vaste: Alles selecteren is uitgeschakeld terwijl het beheer van momentopnamen
* Vaste: De metagegevens van de basis blob mogelijk overschreven na de eigenschappen van de momentopnamen weer te geven

#### <a name="known-issues"></a>Bekende problemen

* Als u ervoor het verkeerde PINCODE/smartcardcertificaat kiest wordt moet u opnieuw opstarten om de beslissing vergeet Opslagverkenner
* Terwijl ingezoomd in- of uitzoomen, het zoomniveau tijdelijk worden op het standaardniveau mogelijk opnieuw ingesteld
* Als meer dan 3 groepen van blobs of tegelijk uploaden van bestanden, kan er fouten optreden
* Het paneel met toepassingsinstellingen account kan tonen, moet u referenties opnieuw invoeren om abonnementen filteren
* Naam van de BLOB's (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam te wijzigen.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde Azure-Stack storage-account.
* Ubuntu 14.04 installeren moet gcc-versie is bijgewerkt of stappen voor het upgraden zijn bijgewerkt: hieronder:

    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get dist-upgrade
    ```


### <a name="version-0812-and-0811-and-0810"></a>Versie 0.8.12 en 0.8.11 en 0.8.10
04/07/2017

#### <a name="new"></a>Nieuw

* Opslagverkenner wordt nu automatisch gesloten wanneer u een update vanuit de melding van updates installeert
* In-place snelle toegang biedt een verbeterde ervaring voor het werken met uw vaak gebruikte resources
* In de editor Blob-Container nu ziet u welke virtuele machine een geleaste blob behoort
* U kunt nu het deelvenster linkerkant samenvouwen
* Detectie wordt nu uitgevoerd op hetzelfde moment als download
* Statistieken in de Blob-Container, bestandsshare en tabel editors gebruiken om te zien van de grootte van de resource of selectie
* U kunt nu aanmelden voor Azure Active Directory (AAD) op basis van Azure-Stack-accounts.
* U kunt nu archiefbestanden meer dan 32MB uploaden naar Premium storage-accounts
* Verbeterde Toegankelijkheidsondersteuning voor
* U kunt nu toevoegen vertrouwde Base64-gecodeerd x.509-SSL-certificaten gaat bewerken -&gt; SSL-certificaten -&gt; certificaten importeren

#### <a name="fixes"></a>Oplossingen

* Vaste: na het vernieuwen van de referenties van een account, de structuurweergave zou soms niet automatisch vernieuwd
* Vaste: genereren van een SAS voor emulator wachtrijen en tabellen zou leiden tot een ongeldige URL
* Vaste: premium storage-accounts kunnen nu worden uitgebreid terwijl een proxy is ingeschakeld
* Vaste: de knop toepassen op de pagina accounts management werkt niet als u had 1 of 0 accounts die zijn geselecteerd
* Vaste: het uploaden van BLOB's waarvoor oplossingen mislukken - 0.8.11, vast
* Vaste: feedback sturen is onderverdeeld in 0.8.11 - 0.8.12, vast

#### <a name="known-issues"></a>Bekende problemen

* Na de upgrade naar 0.8.10, moet u al uw referenties te vernieuwen.
* Terwijl ingezoomd in- of uitzoomen, het zoomniveau kan tijdelijk worden opnieuw ingesteld op het standaardniveau.
* Als meer dan 3 groepen van blobs of tegelijk uploaden van bestanden kan fouten veroorzaken.
* Het paneel met toepassingsinstellingen account kan worden weergegeven dat u moet referenties opnieuw om het filteren van abonnementen.
* Naam van de BLOB's (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam te wijzigen.
* Hoewel Azure Stack momenteel geen bestandsshares ondersteunt, wordt een knooppunt bestandsshares nog steeds wordt weergegeven onder een gekoppelde Azure-Stack storage-account.
* Ubuntu 14.04 installeren moet gcc-versie is bijgewerkt of stappen voor het upgraden zijn bijgewerkt: hieronder:

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

* Opslagverkenner 0.8.9 downloadt automatisch de meest recente versie voor updates.
* Hotfix: met behulp van een portal zou gegenereerde SAS-URI te koppelen van een opslagaccount leiden tot een fout opgetreden.
* U kunt nu maken, beheren en blob momentopnamen promoveren.
* U kunt nu aanmelden bij Azure China, Duitse Azure en Azure US Government-accounts.
* U kunt nu het zoomniveau wijzigen. Gebruik de opties in het menu Beeld inzoomen, uitzoomen en zoomen opnieuw instellen.
* Unicode-tekens worden nu ondersteund in de metagegevens van de gebruiker voor blobs en bestanden.
* Toegankelijkheid is verbeterd.
* Opmerkingen bij de release van de volgende versie wordt op het updatebericht dat weergegeven. U kunt ook de huidige release-opmerkingen in het menu Help weergeven.

#### <a name="fixes"></a>Oplossingen

* Vaste: het versienummer nu juist wordt weergegeven in het Configuratiescherm in Windows
* Vaste: search is niet langer beperkt tot 50.000 knooppunten
* Vaste: uploaden naar een bestandsshare permanent worden geactiveerd als de doelmap al niet bestaat
* Vaste: verbeterde stabiliteit voor lange uploaden en downloaden

#### <a name="known-issues"></a>Bekende problemen

* Terwijl ingezoomd in- of uitzoomen, het zoomniveau kan tijdelijk worden opnieuw ingesteld op het standaardniveau.
* Snelle toegang werkt alleen met een abonnement op basis van items. Lokale resources of resources via de sleutel- of SAS-token is gekoppeld, worden niet ondersteund in deze release.
* Snelweergavetoegang kan een paar seconden om te navigeren naar de doelresource, afhankelijk van hoeveel resources die u hebt het duren.
* Als meer dan 3 groepen van blobs of tegelijk uploaden van bestanden kan fouten veroorzaken.

12/16/2016
### <a name="version-087"></a>Versie 0.8.7

>[!VIDEO https://www.youtube.com/embed/Me4Y4jxoer8?ecver=1]

#### <a name="new"></a>Nieuw

* U kunt het oplossen van conflicten aan het begin van een update, downloaden of kopiëren-sessie in het venster activiteiten
* Beweeg de muisaanwijzer over een tabblad om te zien van het volledige pad van de opslagbronnen
* Wanneer u op een tabblad klikt, synchroniseert het met de locatie in het navigatiedeelvenster links

#### <a name="fixes"></a>Oplossingen

* Vaste: Opslagverkenner is nu een vertrouwd app op Mac
* Vaste: Ubuntu 14.04 wordt opnieuw ondersteund
* Vaste: Soms de account toevoegen UI knippert bij het laden van abonnementen
* Vaste: Soms niet alle opslagbronnen zijn vermeld in het navigatiedeelvenster links
* Vaste: In het actievenster soms weergegeven leeg acties
* Vaste: De grootte van het venster van de vorige sessie gesloten wordt nu bewaard
* Vaste: Kunt u meerdere tabbladen voor dezelfde resource via het contextmenu openen

#### <a name="known-issues"></a>Bekende problemen

* Snelle toegang werkt alleen met een abonnement op basis van items. Lokale resources of resources via de sleutel- of SAS-token is gekoppeld, worden niet ondersteund in deze release
* Het duurt Snelweergavetoegang een paar seconden om te navigeren naar de doelresource, afhankelijk van hoeveel resources die u hebt
* Als meer dan 3 groepen van blobs of tegelijk uploaden van bestanden, kan er fouten optreden
* Zoeken ingangen zoeken over ongeveer 50.000 knooppunten - hierna prestaties kan van invloed zijn of niet-verwerkte uitzondering veroorzaken
* Voor de eerste keer met Opslagverkenner op Mac OS, ziet u mogelijk meerdere prompts dat om toestemming voor toegang tot sleutelhanger van de gebruiker wordt gevraagd. We raden dat u altijd toestaan selecteren zodat de prompt opnieuw niet weergegeven

11/18/2016
### <a name="version-086"></a>Versie 0.8.6

#### <a name="new"></a>Nieuw

* U kunt nu de services voor Snelweergavetoegang pincode doorgaans gebruikt voor eenvoudige navigatie
* U kunt nu meerdere editors openen in andere tabbladen. Één Klik hier om een tijdelijke tabblad; Dubbelklik om een permanente tabblad te openen. U kunt ook klikken op op het tabblad tijdelijke zodat het een permanente tabblad
* We hebben aangebracht merkbare prestaties en stabiliteitsverbeteringen voor uploadt en downloads, met name voor grote bestanden op snelle machines
* Lege 'virtuele' mappen kunnen nu worden gemaakt in de blob-containers
* We hebben afgebakende zoekopdracht met onze nieuwe verbeterde subtekenreeks search, zodat u nu twee opties voor het zoeken hebt opnieuw zijn geïntroduceerd:
    * Globale zoeken: Geef een zoekterm in het Zoektekstvak
    * Afgebakende zoekopdracht - Klik op het pictogram Vergrootglas naast een knooppunt, en vervolgens een zoekterm toevoegen aan het einde van het pad, of met de rechtermuisknop op en selecteer 'Zoeken vanaf hier'
* We hebben verschillende thema's toegevoegd: licht (standaard), donker, Hoog Contrast zwart en Hoog Contrast wit. Ga naar Edit -&gt; thema's te wijzigen van uw voorkeur thema's
* U kunt eigenschappen van Blob en de bestandsnaam wijzigen
* Er wordt nu ondersteuning gecodeerde (base64) en ongecodeerde Wachtrijberichten
* Op Linux, een 64-bits besturingssysteem is nu vereist. Voor deze release wordt alleen ondersteund 64-bits Ubuntu 16.04.1 TNS
* We hebben onze logo bijgewerkt!

#### <a name="fixes"></a>Oplossingen

* Opgelost: Blokkering problemen scherm
* Vaste: Verbeterde beveiliging
* Vaste: Soms dubbele gekoppelde accounts kunnen worden weergegeven
* Vaste: Een blob met een niet-gedefinieerde type inhoud kan genereren een uitzondering
* Vaste: Openen van het paneel Query op een lege tabel was het niet mogelijk
* Vaste: Varieert fouten in de zoekopdracht
* Vaste: Vergroot het aantal resources geladen tussen 50 en 100 wanneer te klikken op 'Meer Load'
* Vaste: Op de eerste keer uitvoert als een account is aangemeld, we nu alle abonnementen voor dat account standaard selecteren

#### <a name="known-issues"></a>Bekende problemen

* Deze versie van de Opslagverkenner kan niet worden uitgevoerd op Ubuntu 14.04
* Als u wilt openen meerdere tabbladen voor dezelfde resource, continu Klik niet op dezelfde resource. Klik op een andere bron en ga vervolgens terug en klik vervolgens op de oorspronkelijke bron opnieuw openen in een ander tabblad
* Snelle toegang werkt alleen met een abonnement op basis van items. Lokale resources of resources via de sleutel- of SAS-token is gekoppeld, worden niet ondersteund in deze release
* Het duurt Snelweergavetoegang een paar seconden om te navigeren naar de doelresource, afhankelijk van hoeveel resources die u hebt
* Als meer dan 3 groepen van blobs of tegelijk uploaden van bestanden, kan er fouten optreden
* Zoeken ingangen zoeken over ongeveer 50.000 knooppunten - hierna prestaties kan van invloed zijn of niet-verwerkte uitzondering veroorzaken

10/03/2016
### <a name="version-085"></a>Versie 0.8.5

#### <a name="new"></a>Nieuw

* Kan SAS Portal gegenereerde sleutels nu gebruiken om te koppelen aan de Storage-Accounts en resources

#### <a name="fixes"></a>Oplossingen

* Vaste: race condition tijdens de zoekopdracht soms veroorzaakt knooppunten worden niet-uitvouwbare
* Vaste: 'HTTP gebruiken' werkt niet bij het verbinden met de Storage-Accounts met de naam en sleutel
* Vaste: SAS-sleutels (die speciaal Portal gegenereerde) retourneren een foutmelding 'afsluitende slash'
* Vaste: tabel importeren problemen
    * Soms zijn partitiesleutel en de rijsleutel teruggedraaid
    * Kan niet 'null' partitiesleutels lezen

#### <a name="known-issues"></a>Bekende problemen

* Zoeken ingangen zoeken over ongeveer 50.000 knooppunten - hierna gevolgen voor de prestaties
* Azure Stack ondersteunt momenteel geen bestanden, zodat u probeert uit te breiden bestanden wordt een fout opgetreden

09/12/2016
### <a name="version-084"></a>Versie 0.8.4

>[!VIDEO https://www.youtube.com/embed/cr5tOGyGrIQ?ecver=1]

#### <a name="new"></a>Nieuw

* Directe koppelingen naar de storage-accounts, containers, tabellen, wachtrijen of bestandsshares voor het delen van genereren en eenvoudig toegang krijgen tot uw resources - Windows en Mac OS-ondersteuning
* Zoeken naar de blob-containers, tabellen, wachtrijen, bestandsshares of storage-accounts in het zoekvak
* U kunt nu componenten in de opbouwfunctie voor query's tabel groeperen
* Wijzig de naam en de blob-containers, bestandsshares, tabellen, blobs, blob-mappen, bestanden en mappen uit binnen SAS gekoppelde accounts en containers kopiëren en plakken
* Naam en het kopiëren van blob-containers en bestandsshares nu behouden eigenschappen en metagegevens

#### <a name="fixes"></a>Oplossingen

* Vaste: kan niet worden bewerkt tabelentiteiten als ze Booleaanse of binaire eigenschappen bevatten

#### <a name="known-issues"></a>Bekende problemen

* Zoeken ingangen zoeken over ongeveer 50.000 knooppunten - hierna gevolgen voor de prestaties

08/03/2016
### <a name="version-083"></a>Versie 0.8.3

>[!VIDEO https://www.youtube.com/embed/HeGW-jkSd9Y?ecver=1]

#### <a name="new"></a>Nieuw

* Wijzig de naam van containers, tabellen, bestandsshares
* Verbeterde ervaring voor de opbouwfunctie voor Query
* Mogelijkheid opslaan en laden van query 's
* Directe koppelingen naar de storage-accounts of containers, wachtrijen, tabellen of bestandsshares voor het delen en eenvoudige toegang tot uw resources (alleen Windows - Mac OS ondersteunen binnenkort beschikbaar.)
* Mogelijkheid om te beheren en configureren van CORS-regels

#### <a name="fixes"></a>Oplossingen

* Vaste: Microsoft-Accounts hernieuwde verificatie vereist is om de 8-12 uur

#### <a name="known-issues"></a>Bekende problemen

* Soms lijkt het alsof de gebruikersinterface bevroren - maximaliseren van het venster helpt dit probleem oplossen
* Mac OS-installatie mogelijk verhoogde machtigingen
* Paneel met toepassingsinstellingen account kan tonen, moet u referenties opnieuw invoeren om abonnementen filteren
* Naam van bestandsshares, blob-containers en tabellen behouden niet metagegevens of andere eigenschappen van de container, zoals het quotum voor de bestandsshare, openbare toegangsniveau of beleidsregels voor toegang
* Naam van de BLOB's (afzonderlijk of in een nieuwe naam blob-container) behoudt niet momentopnamen. Alle andere eigenschappen en metagegevens voor blobs, bestanden en entiteiten blijven behouden tijdens een naam wijzigen
* Kopiëren of verwijderen van resources werkt niet in SAS gekoppelde accounts

07/07/2016
### <a name="version-082"></a>Versie 0.8.2

>[!VIDEO https://www.youtube.com/embed/nYgKbRUNYZA?ecver=1]

#### <a name="new"></a>Nieuw

* Storage-Accounts zijn gegroepeerd op abonnementen. opslag van de ontwikkeling en resources die zijn aangesloten via een sleutel- of SAS worden weergegeven onder het knooppunt (lokaal en bijlage)
* Afmelden bij accounts in Configuratiescherm van de 'Azure Accountinstellingen'
* Configureer proxy-instellingen voor inschakelen en beheren van aanmeldingspagina
* Maken en de blob-leases opsplitsen
* Open blob-containers, wachtrijen, tabellen en bestanden met één klik

#### <a name="fixes"></a>Oplossingen

* Vaste: Wachtrijberichten ingevoegd met .NET of Java-bibliotheken zijn niet goed gedecodeerd van base64
* Vaste: $metrics tabellen worden niet weergegeven voor Blob Storage-accounts
* Vaste: tabellen knooppunt werkt niet voor lokale opslag van (ontwikkeling)

#### <a name="known-issues"></a>Bekende problemen

* Mac OS-installatie mogelijk verhoogde machtigingen

06/15/2016
### <a name="version-080"></a>Versie 0.8.0

>[!VIDEO https://www.youtube.com/embed/ycfQhKztSIY?ecver=1]

>[!VIDEO https://www.youtube.com/embed/k4_kOUCZ0WA?ecver=1]

>[!VIDEO https://www.youtube.com/embed/3zEXJcGdl_k?ecver=1]

#### <a name="new"></a>Nieuw

* Share-ondersteuning: weergeven, uploaden, downloaden, kopiëren van bestanden en mappen, SAS URI's (maken en verbinding maken)
* Verbeterde gebruikerservaring voor het verbinden met opslag met SAS URI's of account sleutels
* De resultaten van de query exporteren
* De tabel kolom volgorde en aanpassen
* $Logs blob-containers en $metrics tabellen bekijken voor Storage-Accounts met ingeschakelde metrische gegevens
* Verbeterde exporteren en importeren van gedrag, bevat nu het soort waarde

#### <a name="fixes"></a>Oplossingen

* Vaste: uploaden of grote blobs downloaden kan leiden tot onvolledige uploads/downloads
* Vaste: bewerken, toevoegen of importeren van een entity met een numerieke waarde ('1') wordt converteren naar double
* Vaste: Kan niet uitbreiden van het knooppunt van de tabel in de lokale ontwikkelingsomgeving

#### <a name="known-issues"></a>Bekende problemen

* $metrics tabellen zijn niet zichtbaar voor Blob Storage-accounts
* Berichten in wachtrij plaatsen toegevoegd via een programma mogelijk niet correct weergegeven als de berichten zijn gecodeerd met Base64-codering

05/17/2016
### <a name="version-07201605090"></a>Versie 0.7.20160509.0

#### <a name="new"></a>Nieuw

* Betere foutafhandeling voor de app vastloopt

#### <a name="fixes"></a>Oplossingen

* Vaste bug waar informatiebalk berichten soms worden niet weergegeven als aanmeldingsreferenties vereist zijn

#### <a name="known-issues"></a>Bekende problemen

* Tabellen: Toevoegen, bewerken of importeren van een entiteit die een eigenschap met een ambiguously numerieke waarde, zoals '1' of "1.0", en de gebruiker heeft probeert te verzenden als een `Edm.String`, de waarde Open opnieuw via de client-API als een Edm.Double

03/31/2016

### <a name="version-07201603250"></a>Versie 0.7.20160325.0

>[!VIDEO https://www.youtube.com/embed/imbgBRHX65A?ecver=1]

>[!VIDEO https://www.youtube.com/embed/ceX-P8XZ-s8?ecver=1]

#### <a name="new"></a>Nieuw

* Tabel ondersteuning: weergeven, query, export, importeren en CRUD-bewerkingen voor entiteiten
* Wachtrij-ondersteuning: bekijken, toe te voegen, dequeueing berichten
* Genereren van SAS-URI's voor Opslagaccounts
* Verbinding maken met de Storage-Accounts met SAS URI 's
* Meldingen voor toekomstige updates voor Opslagverkenner updates
* Bijgewerkte uiterlijk

#### <a name="fixes"></a>Oplossingen

* Verbeteringen in prestaties en betrouwbaarheid

### <a name="known-issues-amp-mitigations"></a>Bekende problemen &amp; oplossingen

* Downloaden van bestanden van de grote blob werkt niet correct - wordt u aangeraden AzCopy terwijl we dit probleem op te lossen
* Accountreferenties wordt niet opgehaald of in de cache opgeslagen als de basismap kan niet worden gevonden of kan niet naar worden geschreven
* Als we zijn toevoegen, bewerken of importeren van een entiteit die heeft een eigenschap met een ambiguously numerieke waarde, zoals "1" of "1.0", en de gebruiker probeert te verzenden als een `Edm.String`, de waarde Open opnieuw via de client-API als een Edm.Double
* Bij het importeren van CSV-bestanden met meerdere regels registreert kunnen de gegevens ophalen afgekapt of versleuteld

02/03/2016

### <a name="version-07201601291"></a>Versie 0.7.20160129.1

#### <a name="fixes"></a>Oplossingen

* Verbeterde prestaties bij het uploaden, downloaden en blobs kopiëren

01/14/2016

### <a name="version-07201601050"></a>Versie 0.7.20160105.0

#### <a name="new"></a>Nieuw

* Linux-ondersteuning (pariteit functies OSX)
* Blob-containers met Shared Access Signatures (SAS)-sleutel niet toevoegen
* Storage-Accounts voor Azure China toevoegen
* Storage-Accounts met aangepaste eindpunten toevoegen
* Openen en weergeven van de inhoud tekst en afbeeldingen blobs
* Blob-eigenschappen en metagegevens weergeven en bewerken

#### <a name="fixes"></a>Oplossingen

* Vaste: uploaden of downloaden van een groot aantal blobs (500 +) soms veroorzaakt de app te hebben van een wit scherm
* Vaste: bij het instellen van blob-container openbaar toegankelijk niveau, de nieuwe waarde is niet bijgewerkt totdat u de focus op de container opnieuw instelt. Ook het dialoogvenster altijd wordt standaard ingesteld op 'Geen openbare toegang', en niet de huidige waarde.
* Ondersteuning voor betere algehele toetsenbord toegankelijkheid en -gebruikersinterface
* Breadcrumbs tekstomloop geschiedenis wanneer het lang met witruimte
* Dialoogvenster SAS ondersteunt validatie voor invoer
* Lokale opslag blijft beschikbaar zijn, zelfs als de gebruikersreferenties zijn verlopen
* Wanneer een geopende blob-container wordt verwijderd, is de blob-explorer aan de rechterkant gesloten

#### <a name="known-issues"></a>Bekende problemen

* Linux-installatie moet gcc-versie is bijgewerkt of stappen voor het upgraden zijn bijgewerkt: hieronder:
    * `sudo add-apt-repository ppa:ubuntu-toolchain-r/test`
    * `sudo apt-get update`
    * `sudo apt-get upgrade`
    * `sudo apt-get dist-upgrade`

11/18/2015
### <a name="version-07201511160"></a>Versie 0.7.20151116.0

#### <a name="new"></a>Nieuw

* Mac OS en Windows-versies
* Aanmelden bij uw Storage-Accounts weergeven: gebruik uw Org-Account, Microsoft-Account, 2FA, enzovoort.
* Opslag voor lokale ontwikkeling (opslagemulator, gebruiken alleen-Windows)
* Ondersteuning voor Azure Resource Manager en Classic resource
* Maken en verwijderen van blobs, wachtrijen of tabellen
* Zoeken naar specifieke blobs, wachtrijen of tabellen
* Bekijk de inhoud van de blob-containers
* Navigeren door mappen
* Uploaden, downloaden en verwijderen van blobs en mappen
* Blob-eigenschappen en metagegevens weergeven en bewerken
* SAS sleutels genereren
* Beheren en opgeslagen toegang beleid (SAP) maken
* Blobs op voorvoegsel zoeken
* Sleep n neerzetten van bestanden te uploaden of downloaden

#### <a name="known-issues"></a>Bekende problemen

* Bij het instellen van blob-container openbaar toegankelijk niveau, wordt de nieuwe waarde niet bijgewerkt totdat u de focus op de container voor het opnieuw instellen
* Wanneer u het dialoogvenster het openbare toegang instellen opent, altijd worden 'Geen openbare toegang' als de standaard, en niet de huidige waarde
* Gedownloade blobs hernoemen niet
* Logboekvermeldingen activiteit wordt soms 'hangen' een onderhanden status wanneer een fout optreedt, en de fout wordt niet weergegeven.
* Soms vastloopt of schakelt u volledig wit wanneer u probeert te uploaden of een groot aantal blobs downloaden
* Soms annuleren van een kopieerbewerking werkt niet
* Tijdens het maken van een container (wachtrij-blob/tabelnaam) als u een ongeldige naam invoeren en doorgaan met het maken van een andere onder een andere containertype u kan niet de focus instelt op het nieuwe type
* Kan geen nieuwe map maken of wijzig de map




[2]: https://support.microsoft.com/en-us/help/4021389/storage-explorer-troubleshooting-guide
[3]: vs-azure-tools-storage-manage-with-storage-explorer.md
