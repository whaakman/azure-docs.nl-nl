---
title: Probleemoplossingsgids voor Azure Storage Explorer | Microsoft Docs
description: Overzicht van technieken voor foutopsporing voor Azure Storage Explorer
services: virtual-machines
author: Deland-Han
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: fd34ab7cd899549962663e8cee8ee2121c39c49e
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840388"
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Probleemoplossingsgids voor Azure Storage Explorer

Microsoft Azure Storage Explorer is een zelfstandige app waarmee u eenvoudig met Azure Storage-gegevens kunt werken via Windows, macOS en Linux. De app kan verbinding maken met de Storage-accounts die worden gehost op Azure, nationale Clouds en Azure Stack.

Deze handleiding bevat een overzicht van oplossingen voor problemen die regelmatig voorkomen in Storage Explorer.

## <a name="role-based-access-control-permission-issues"></a>Problemen met de machtigingen van de Role-based Access Control

[Op rollen gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) biedt verfijnd toegangsbeheer van Azure-resources door een combinatie van machtigingen in _rollen_. Hier volgen enkele suggesties die u volgen kunt om het ophalen van RBAC in Storage Explorer werkt.

### <a name="what-do-i-need-to-see-my-resources-in-storage-explorer"></a>Wat heb ik nodig om te zien van mijn resources in Storage Explorer?

Als u problemen met het openen van de storage-resources met RBAC ondervindt, is het mogelijk omdat u de juiste rollen nog niet zijn toegewezen. De volgende secties beschrijven de machtigingen die Storage Explorer is op dit moment is vereist voor toegang tot uw opslagresources.

Neem contact op met de beheerder van uw Azure-account als u niet zeker weet dat u hebt de juiste rollen of machtigingen.

#### <a name="read-listget-storage-accounts"></a>Lezen: Opslagaccount (s) een lijst of ophalen

U moet machtiging aan de lijst met storage-accounts hebben. U kunt deze machtiging krijgen door de rol 'Lezer' wordt toegewezen.

#### <a name="list-storage-account-keys"></a>Een lijst met Storage Accountsleutels

Storage Explorer kunt accountsleutels ook gebruiken om aanvragen te verifiëren. U krijgt toegang tot sleutels met krachtige functies, zoals de rol 'Inzender'.

> [!NOTE]
> Toegangssleutels worden onbeperkte machtigingen verlenen aan iedereen die deze bevat. Daarom wordt het in het algemeen niet aanbevolen ze worden verstrekt aan de gebruikers-accounts. Als u intrekken van toegang tot sleutels wilt, kunt u opnieuw te genereren van de [Azure Portal](https://portal.azure.com/).

#### <a name="data-roles"></a>Gegevens-rollen

U moet ten minste één rol die verleent toegang tot gegevens lezen van resources worden toegewezen. Bijvoorbeeld, als u wilt weergeven of downloaden van blobs, moet u ten minste de rol 'Gegevenslezer voor Opslagblob'.

### <a name="why-do-i-need-a-management-layer-role-to-see-my-resources-in-storage-explorer"></a>Waarom moet ik een beheerrol laag om te zien van mijn resources in Storage Explorer?

Azure Storage bestaat uit twee lagen van access: _management_ en _gegevens_. Abonnementen en opslagaccounts zijn toegankelijk via de beheerlaag. Containers, blobs en andere gegevensbronnen die toegankelijk zijn via de gegevenslaag. Bijvoorbeeld, als u wilt voor een lijst van uw opslagaccounts van Azure, stuurt u een aanvraag naar het beheereindpunt. Als u een lijst met blob-containers in een account wilt, kunt u een aanvraag verzendt naar de juiste service-eindpunt.

RBAC-rollen kunnen machtigingen voor toegang tot laag management of gegevens bevatten. De rol 'Lezer', verleent u bijvoorbeeld alleen-lezen toegang management layer-resources.

Strikt genomen met de rol 'Lezer' bevat geen gegevens laag machtigingen en is niet nodig voor toegang tot de gegevenslaag.

Storage Explorer kunt eenvoudig toegang tot uw resources door te verzamelen van de benodigde informatie om te verbinden met uw Azure-resources voor u. Bijvoorbeeld, als u uw blob-containers, verzendt Storage Explorer een lijst met containers-aanvraag naar het eindpunt van blob service. Als u dit eindpunt, Opslagverkenner zoekt in de lijst met abonnementen en storage-accounts hebt toegang tot. Maar als u wilt zoeken in uw abonnementen en opslagaccounts, moet Storage Explorer ook toegang hebben tot de beheerlaag.

Als u een rol die machtigingen worden de management laag hebt, kan de Storage Explorer gegevens die nodig zijn om te verbinden met de gegevenslaag niet ophalen.

### <a name="what-if-i-cant-get-the-management-layer-permissions-i-need-from-my-administrator"></a>Wat gebeurt er als ik niet kan het beheer van laag machtigingen krijgen die ik nodig van mijn beheerder?

Er zijn geen nog een oplossing RBAC met betrekking tot op dit moment. Als tijdelijke oplossing, kunt u een SAS-URI op aanvragen [koppelen aan uw resource](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=linux#use-a-sas-uri).

## <a name="error-self-signed-certificate-in-certificate-chain-and-similar-errors"></a>Fout: Zelf-ondertekend certificaat in de certificaatketen (en vergelijkbare fouten)

Certificaatfouten worden veroorzaakt door een van de twee volgende situaties:

1. De app is verbonden via een 'transparante proxy', wat betekent dat een server (zoals de bedrijfsserver van uw) is HTTPS-verkeer onderschept, ontsleutelt en vervolgens versleutelt met behulp van een zelfondertekend certificaat.
2. U voert een toepassing die een zelf-ondertekend SSL-certificaat injecteert in de HTTPS-berichten die u ontvangt. Voorbeelden van toepassingen die certificaten invoeren bevat antivirusprogramma's en netwerk verkeer inspectie software.

Wanneer Storage Explorer ziet een zelf-ondertekend of niet-vertrouwd certificaat, kan het niet meer te weten of het ontvangen HTTPS-bericht is gewijzigd. Als u een kopie van de zelf-ondertekend certificaat hebt, kunt u Storage Explorer instrueren vertrouwen aan de hand van de volgende stappen uit:

1. Verkrijgen van een Base-64 gecodeerde X.509 (.cer)-exemplaar van het certificaat
2. Klik op **bewerken** > **SSL-certificaten** > **certificaten importeren**, en vervolgens de bestandskiezer om te zoeken, selecteer en open het cer-bestand

Dit probleem kan ook worden de resultaten van meerdere certificaten (hoofdmap en tussenliggende). Beide certificaten moeten worden toegevoegd aan de fout te verhelpen.

Als u niet zeker weet waar het certificaat is afkomstig van bent, kunt u proberen deze stappen om te zoeken:

1. Installeer Open SSL
    * [Windows](https://slproweb.com/products/Win32OpenSSL.html) (een van de lichte versies is voldoende)
    * Mac en Linux: moet worden opgenomen met het besturingssysteem
2. Voer Open SSL uit
    * Windows: open de map wilt installeren, klik op **/bin/** , en dubbelklik vervolgens op **openssl.exe**.
    * Mac en Linux: Voer **openssl** vanuit een terminal.
3. Voer `s_client -showcerts -connect microsoft.com:443` uit
4. Zoek naar zelfondertekende certificaten. Als u niet zeker weet welke certificaten zelfondertekend zijn, zoek naar een willekeurige plaats het onderwerp `("s:")` en -verlenersleutel `("i:")` zijn hetzelfde.
5. Als u geen zelfondertekende certificaten hebt gevonden, voor elk adres, kopieert en plakt u alles vanaf **---BEGIN CERTIFICATE---** naar **---END CERTIFICATE---** naar een nieuw .cer-bestand.
6. Storage Explorer openen, klikt u op **bewerken** > **SSL-certificaten** > **certificaten importeren**, en vervolgens de bestandskiezer om te zoeken, selecteren, en Open de CER-bestanden die u hebt gemaakt.

Als u geen zelfondertekende certificaten met behulp van de voorgaande stappen niet kunt vinden, contact met ons opnemen via het hulpprogramma feedback voor meer informatie. U kunt ook Storage Explorer starten vanaf de opdrachtregel met de `--ignore-certificate-errors` vlag. Wanneer met deze markering wordt gestart, zal de Storage Explorer certificaatfouten negeren.

## <a name="sign-in-issues"></a>Aanmeldingsproblemen

### <a name="blank-sign-in-dialog"></a>Lege aanmeldingsdialoogvenster

Lege aanmelden dialoogvensters worden meestal veroorzaakt door AD FS Storage Explorer waarin wordt gevraagd om uit te voeren een omleiding, wordt niet ondersteund door Electron. U kunt dit probleem omzeilen, kunt u proberen apparaat Code Flow gebruiken voor aanmelding bij. Voer de volgende stappen uit om dit te doen:

1. Menu: Preview -> 'Apparaat Code-aanmelding gebruiken'.
2. Open het dialoogvenster voor verbinding maken (ofwel via de plug-pictogram in de linkerbenedenhoek verticale balk of 'Account toevoegen' in het venster account).
3. Kies welke omgeving u aanmelden wilt bij.
4. Klik op de knop 'Aanmelden'.
5. Volg de instructies in het volgende deelvenster.

Als u zelf hebt u problemen bij het aanmelden bij het account dat u gebruiken wilt, omdat de standaardbrowser is al aangemeld bij een ander account, kunt u ofwel:

1. De koppeling en de code handmatig kopiëren naar een persoonlijke sessie van uw browser.
2. De koppeling en code handmatig kopiëren naar een andere browser.

### <a name="reauthentication-loop-or-upn-change"></a>Herauthenticatie lus of UPN wijzigen

Als u zich in een lus opnieuw verifiëren of de UPN van een van uw accounts hebt gewijzigd, probeert u de volgende stappen uit:

1. Alle accounts verwijderen en sluit vervolgens Storage Explorer
2. Verwijder de. IdentityService map vanaf uw computer. Op Windows, de map bevindt zich in `C:\users\<username>\AppData\Local`. U kunt de map in de hoofdmap van uw directory gebruiker vinden voor Mac en Linux.
3. Als u Mac- of Linux, moet u ook de vermelding Microsoft.Developer.IdentityService verwijderen uit uw besturingssystemen keystore. Op de Mac is de sleutelopslag de toepassing 'Gnome Keychain'. Voor Linux, de toepassing wordt meestal 'Sleutelhanger' genoemd, maar de naam kan afwijken afhankelijk van uw distributie.

### <a name="conditional-access"></a>Voorwaardelijke toegang

Voorwaardelijke toegang wordt niet ondersteund wanneer Storage Explorer wordt gebruikt op Windows 10, Linux of macOS. Dit is vanwege een beperking in de AAD-bibliotheek die worden gebruikt door de Storage Explorer.

## <a name="mac-keychain-errors"></a>Mac-sleutelhanger fouten

De Mac OS Keychain krijgt soms in een status die ervoor zorgt problemen met de Storage-Explorer-verificatiebibliotheek dat. Als u de sleutelketen uit deze status, probeert u de volgende stappen uit:

1. Sluit de Storage Explorer.
2. Open sleutelhanger (**cmd + SPATIEBALK**, typt u in de sleutelketen, bereikt invoeren).
3. Selecteer de sleutelketen 'aanmelding'.
4. Klik op het hangslotpictogram als u wilt vergrendelen van de sleutelhanger (het hangslot wordt laten bewegen naar een vergrendelde positie als u klaar bent, duurt het enkele seconden, afhankelijk van welke apps die u hebt geopend).

    ![image](./media/storage-explorer-troubleshooting/unlockingkeychain.png)

5. Storage Explorer starten.
6. Een pop-upvenster weergegeven met de melding iets zoals 'Service hub wil toegang tot de sleutelketen'. Wanneer dit doet, voer het wachtwoord van uw Mac admin-account en klikt u op **altijd toestaan** (of **toestaan** als **altijd toestaan** is niet beschikbaar).
7. Probeert aan te melden.

### <a name="general-sign-in-troubleshooting-steps"></a>Algemene aanmelden stappen voor probleemoplossing

* Als u op Mac OS bent en het venster aanmelden is nooit wordt weergegeven boven de 'wachten op verificatie...' dialoogvenster, klikt u vervolgens probeert [stappen](#mac-keychain-errors)
* Start u Opslagverkenner opnieuw
* Als het verificatievenster leeg is, wacht u minstens één minuut voordat de verificatie-dialoogvenster wordt gesloten.
* Zorg ervoor dat uw proxy en de instellingen juist zijn geconfigureerd voor uw computer en de Storage Explorer-certificaat.
* Als u op Windows en toegang tot Visual Studio 2019 op dezelfde computer en meld u aan, probeert u aanmelden bij Visual Studio 2019. Na een geslaagde aanmelding bij Visual Studio 2019, kunt u Storage Explorer openen en uw account in het deelvenster account ziet.

Als geen van deze methoden werken [opent u een probleem op GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues).

### <a name="missing-subscriptions-and-broken-tenants"></a>Ontbrekende abonnementen en verbroken tenants

Als u niet uw abonnementen ophalen nadat u zich met succes hebt aangemeld, probeert u de volgende methoden:

* Controleer of dat uw account toegang heeft tot de abonnementen die u verwacht. U kunt uw toegang controleren door aan te melden bij de portal voor de Azure-omgeving die u probeert te gebruiken.
* Zorg ervoor dat u zich hebt aangemeld met behulp van de juiste Azure-omgeving (Azure, Azure China 21Vianet, Azure Duitsland, Azure US Government, of aangepaste omgeving).
* Als u zich achter een proxy bevindt, zorg ervoor dat u de Storage Explorer-proxy correct hebt geconfigureerd.
* Probeer te verwijderen en opnieuw toe te voegen in het account.
* Als er een koppeling 'Informatie', en weten welke foutberichten worden gerapporteerd voor de tenants die mislukken. Als you'ren't zeker wat te doen met de fout voor berichten van u en u kunt vervolgens Zie [opent u een probleem op GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues).

## <a name="cant-remove-attached-account-or-storage-resource"></a>Kan de gekoppelde account of storage resource niet verwijderen

Als u niet te verwijderen van een gekoppelde account of een resource voor opslag via de gebruikersinterface, kunt u alle gekoppelde resources handmatig verwijderen door het verwijderen van de volgende mappen:

* Windows: `%AppData%/StorageExplorer`
* macOS: `/Users/<your_name>/Library/Application Support/StorageExplorer`
* Linux: `~/.config/StorageExplorer`

> [!NOTE]
> Storage Explorer sluiten voordat u de bovenstaande mappen worden verwijderd.

> [!NOTE]
> Als u ooit een SSL-certificaten hebt geïmporteerd en vervolgens back-up van de inhoud van de `certs` directory. Later, kunt u de back-up uw SSL-certificaten opnieuw importeren.

## <a name="proxy-issues"></a>Proxy-problemen

Controleer eerst of de volgende informatie die u hebt ingevoerd juist zijn:

* De proxy-URL en het poortnummer
* Gebruikersnaam en het wachtwoord indien nodig door de proxy

> [!NOTE]
> Storage Explorer biedt geen ondersteuning voor proxy auto config-bestanden voor het configureren van proxy-instellingen.

### <a name="common-solutions"></a>Algemene oplossingen

Als u nog steeds problemen ondervindt, probeert u de volgende methoden:

* Als u verbinding met Internet maken kunt zonder gebruik van uw proxy, moet u controleren of Storage Explorer zonder proxy-instellingen die zijn ingeschakeld werkt. Als dit het geval is, is er mogelijk een probleem met de proxy-instellingen. Samen met uw proxybeheerder om de problemen te identificeren.
* Controleer of dat andere toepassingen met behulp van de proxy-server werkt zoals verwacht.
* Controleren of u verbinding naar de portal voor de Azure-omgeving die u probeert maken kunt te gebruiken
* Controleer of u kunt reacties van uw service-eindpunten te ontvangen. Voer een van uw eindpunt-URL's in uw browser. Als u verbinding maken kunt, ontvangt u een InvalidQueryParameterValue of een vergelijkbare XML-antwoord.
* Als iemand anders Storage Explorer ook met uw proxyserver gebruiken is, controleert u of dat ze verbinding kunnen maken. Als ze verbinding maken kunnen, hebt u mogelijk contact op met uw proxy-server.

### <a name="tools-for-diagnosing-issues"></a>Hulpprogramma's voor het oplossen van problemen

Als u hulpprogramma's voor netwerken, zoals Fiddler voor Windows hebt, kunt u als volgt de problemen onderzoeken:

* Als u om te werken via de proxy hebt, is het wellicht uw netwerken hulpprogramma verbinding maken via de proxy configureren.
* Controleer het poortnummer dat door uw netwerken tool gebruikt.
* Voer de URL van de lokale host en het poortnummer van de netwerken van het programma als proxy-instellingen in Storage Explorer. Indien correct uitgevoerd, begint uw netwerken hulpprogramma logboekregistratie netwerkaanvragen die zijn gesteld door Storage Explorer management en service-eindpunten. Voer bijvoorbeeld https://cawablobgrs.blob.core.windows.net/ voor het eindpunt van de blob in een browser, en u ontvangt een antwoord lijkt op het volgende voorbeeld, waarin stelt de resource bestaat, hoewel u geen toegang krijgt.

![Voorbeeld van code](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>Neem contact op met de serverbeheerder proxy

Als de proxy-instellingen juist zijn, hebt u mogelijk contact op met uw proxy-server-beheerder, en

* Zorg ervoor dat uw proxy het verkeer naar de Azure management of resource-eindpunten niet blokkeert.
* Controleer of de verificatieprotocol wordt gebruikt door de proxyserver. Storage Explorer ondersteunt momenteel geen NTLM-proxy's.

## <a name="unable-to-retrieve-children-error-message"></a>Foutbericht "Kan niet naar items ophalen"

Als u met Azure via een proxy verbonden bent, of u de proxy-instellingen juist zijn. Als u nu toegang verleend aan een resource van de eigenaar van het abonnement of account, controleert u of u hebt gelezen of lijst met machtigingen voor die bron.

## <a name="connection-string-doesnt-have-complete-configuration-settings"></a>Verbindingsreeks niet hebben voltooid configuratie-instellingen

Als u dit foutbericht ontvangt, is het mogelijk dat u hebt de benodigde machtigingen om op te halen van de sleutels voor uw Storage-account. Om te bevestigen of dit het geval is, gaat u naar de portal en zoek uw opslagaccount. U kunt dit snel doen door met de rechtermuisknop op het knooppunt voor uw opslagaccount en te klikken op 'Openen in Portal'. Als u dit doet, gaat u naar de blade 'Toegangssleutel'. Als u geen machtigingen voor het weergeven van sleutels, ziet u een pagina met het bericht 'U hebt geen toegang'. U kunt dit probleem omzeilen, kunt u de accountsleutel ophalen van iemand anders en koppelen met de naam en sleutel, of u kunt vragen voor een SAS met de Storage-account en deze gebruiken om te koppelen van het Storage-account.

Als u de accountsleutels ziet, moet u een probleem op GitHub bestand, zodat we kunt u het probleem is opgelost.

## <a name="issues-with-sas-url"></a>Problemen met SAS-URL

Als u verbinding met een service met behulp van een SAS-URL en deze fout optreedt maakt:

* Controleer of dat de URL biedt de benodigde machtigingen om te lezen of lijst met resources.
* Controleer of de URL is niet verlopen.
* Als de SAS-URL is gebaseerd op een toegangsbeleid, controleert u of het toegangsbeleid niet is ingetrokken.

Als u per ongeluk die is gekoppeld met behulp van een ongeldige SAS-URL en kan niet worden losgekoppeld, volgt u deze stappen:

1. Wanneer Storage Explorer wordt uitgevoerd, drukt u op F12 om de developer-hulpprogramma's-venster te openen.
2. Klik op het tabblad toepassingen, en klik vervolgens op lokale opslag > file:// in de boomstructuur aan de linkerkant.
3. Zoek de sleutel die is gekoppeld aan het type van de service van de problematische SAS-URI. Bijvoorbeeld, als de slechte SAS-URI een blob-container is, zoekt u naar de sleutel met de naam `StorageExplorer_AddStorageServiceSAS_v1_blob`.
4. De waarde van de sleutel moet een JSON-matrix. Het object dat is gekoppeld aan de ongeldige URI vinden en verwijderen.
5. Druk op Ctrl + R om opnieuw te laden van Storage Explorer.

## <a name="linux-dependencies"></a>Linux-afhankelijkheden

<!-- Storage Explorer 1.9.0 and later is available as a snap from the Snap Store. The Storage Explorer snap installs all of its dependencies with no extra hassle.

Storage Explorer requires the use of a password manager, which may need to be connected manually before Storage Explorer will work correctly. You can connect Storage Explorer to your system's password manager with the following command:

```bash
snap connect storage-explorer:password-manager-service :password-manager-service
```

You can also download the application .tar.gz file, but you'll have to install dependencies manually. -->

> [!IMPORTANT]
> Storage Explorer zoals beschreven in de..GZ downloaden wordt alleen ondersteund voor Ubuntu-distributies. Andere distributies zijn niet geverifieerd en vereisen mogelijk alternatieve of extra pakketten.

Deze pakketten zijn de meest algemene vereisten voor Storage Explorer op Linux:

* [.NET core 2.0-Runtime](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x)
* `libgconf-2-4`
* `libgnome-keyring0` of `libgnome-keyring-dev`
* `libgnome-keyring-common`

> [!NOTE]
> Storage Explorer versie 1.7.0 en eerder vereist .NET Core 2.0. Als u een nieuwere versie van .NET Core die zijn geïnstalleerd hebt en vervolgens u moet [vullen van de Storage Explorer](#patching-storage-explorer-for-newer-versions-of-net-core). Als u werkt met Storage Explorer 1.8.0 of groter zijn dan zou het mogelijk om te gebruiken om .NET Core 2.2. Oudere versies dan 2.2 zijn niet werken op dit moment geverifieerd.

# <a name="ubuntu-1904tab1904"></a>[Ubuntu 19.04](#tab/1904)

1. Download Storage Explorer.
2. Installeer de [.NET Core Runtime](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu19-04/runtime-current).
3. Voer de volgende opdracht uit:
   ```bash
   sudo apt-get install libgconf-2-4 libgnome-keyring0
   ```

# <a name="ubuntu-1804tab1804"></a>[Ubuntu 18.04](#tab/1804)

1. Download Storage Explorer.
2. Installeer de [.NET Core Runtime](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu18-04/runtime-current).
3. Voer de volgende opdracht uit:
   ```bash
   sudo apt-get install libgconf-2-4 libgnome-keyring-common libgnome-keyring0
   ```

# <a name="ubuntu-1604tab1604"></a>[Ubuntu 16.04](#tab/1604)

1. Download Storage Explorer
2. Installeer de [.NET Core Runtime](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu16-04/runtime-current).
3. Voer de volgende opdracht uit:
   ```bash
   sudo apt install libgnome-keyring-dev
   ```

# <a name="ubuntu-1404tab1404"></a>[Ubuntu 14.04](#tab/1404)

1. Download Storage Explorer
2. Installeer de [.NET Core Runtime](https://dotnet.microsoft.com/download/linux-package-manager/ubuntu14-04/runtime-current).
3. Voer de volgende opdracht uit:
   ```bash
   sudo apt install libgnome-keyring-dev
   ```

### <a name="patching-storage-explorer-for-newer-versions-of-net-core"></a>Storage Explorer patches voor nieuwere versies van .NET Core

Voor Storage Explorer 1.7.0 of ouder bent, moet u voor het vullen van de versie van .NET Core gebruikt door de Storage Explorer.

1. Download versie 1.5.43 van StreamJsonRpc [vanuit nuget](https://www.nuget.org/packages/StreamJsonRpc/1.5.43). Zoek naar de koppeling 'Downloaden' aan de rechterkant van de pagina.
2. Na het downloaden van het pakket, wijzigt u het de bestandsextensie van `.nupkg` naar `.zip`.
3. Pak het pakket.
4. Open de map `streamjsonrpc.1.5.43/lib/netstandard1.1/`.
5. Kopie `StreamJsonRpc.dll` naar de volgende locaties in de map Storage Explorer:
   * `StorageExplorer/resources/app/ServiceHub/Services/Microsoft.Developer.IdentityService/`
   * `StorageExplorer/resources/app/ServiceHub/Hosts/ServiceHub.Host.Core.CLR.x64/`

## <a name="open-in-explorer-from-azure-portal-doesnt-work"></a>Openen In Explorer van Azure portal werkt niet

Als de knop 'Openen In Explorer' in de Azure-portal niet voor u werkt, zorg er dan voor dat u een compatibele browser. De volgende browsers zijn getest op compatibiliteit.
* Microsoft Edge
* Mozilla Firefox
* Google Chrome
* Microsoft Internet Explorer

## <a name="next-steps"></a>Volgende stappen

Als geen van de oplossingen voor u, klikt u vervolgens werken [opent u een probleem op GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues). U kunt ook snel GitHub krijgen met behulp van de knop 'Probleem melden op GitHub' in de linkerbenedenhoek.

![Feedback](./media/storage-explorer-troubleshooting/feedback-button.PNG)
