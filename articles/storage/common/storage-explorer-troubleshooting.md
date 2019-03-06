---
title: Probleemoplossingsgids voor Azure Storage Explorer | Microsoft Docs
description: Overzicht van technieken voor foutopsporing voor Azure Storage Explorer
services: virtual-machines
author: Deland-Han
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.subservice: common
ms.openlocfilehash: 15ceaf1a75859ca53ddb946555880b360b29ee58
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57405688"
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Probleemoplossingsgids voor Azure Storage Explorer

Microsoft Azure Storage Explorer is een zelfstandige app waarmee u eenvoudig met Azure Storage-gegevens kunt werken via Windows, macOS en Linux. De app kan verbinding maken met de Storage-accounts die worden gehost op Azure, nationale Clouds en Azure Stack.

Deze handleiding bevat een overzicht van oplossingen voor problemen die regelmatig voorkomen in Storage Explorer.

## <a name="role-based-access-control-permission-issues"></a>Problemen met de machtigingen van de Role-based Access Control

[Op rollen gebaseerd toegangsbeheer (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) biedt verfijnd toegangsbeheer van Azure-resources door een combinatie van machtigingen in _rollen_. Hier volgen enkele suggesties die u volgen kunt om het ophalen van RBAC in Storage Explorer werkt.

### <a name="what-do-i-need-to-see-my-resources-in-storage-explorer"></a>Wat heb ik nodig om te zien van mijn resources in Storage Explorer?

Als u problemen met het openen van de storage-resources met RBAC ondervindt, is het mogelijk omdat u de juiste rollen nog niet zijn toegewezen. De volgende secties beschrijven de machtigingen die Storage Explorer is op dit moment is vereist voor toegang tot uw opslagresources.

Neem contact op met de beheerder van uw Azure-account als u niet zeker weet dat u hebt de juiste rollen of machtigingen.

#### <a name="read-listget-storage-accounts"></a>Lezen: Opslagaccounts opnemen in een lijst of ophalen

U moet machtiging aan de lijst met storage-accounts hebben. U kunt deze machtiging krijgen door de rol 'Lezer' wordt toegewezen.

#### <a name="list-storage-account-keys"></a>Opslagsleutels voor account opnemen in een lijst

Storage Explorer kunt accountsleutels ook gebruiken om aanvragen te verifiëren. U krijgt toegang tot sleutels met krachtige functies, zoals de rol 'Inzender'.

> [!NOTE]
> Toegangssleutels worden onbeperkte machtigingen verlenen aan iedereen die deze bevat. Daarom is het in het algemeen niet aanbevolen ze worden verstrekt aan de gebruikers-accounts. Als u intrekken van toegang tot sleutels wilt, kunt u opnieuw te genereren van de [Azure Portal](https://portal.azure.com/).

#### <a name="data-roles"></a>Gegevens-rollen

U moet ten minste één rol die verleent toegang tot gegevens lezen van resources worden toegewezen. Bijvoorbeeld, als u wilt weergeven of downloaden van blobs, moet u ten minste de rol 'Gegevenslezer voor Opslagblob'.

### <a name="why-do-i-need-a-management-layer-role-to-see-my-resources-in-storage-explorer"></a>Waarom moet ik een beheerrol laag om te zien van mijn resources in Storage Explorer?

Azure Storage bestaat uit twee lagen van access: _management_ en _gegevens_. Abonnementen en opslagaccounts zijn toegankelijk via de beheerlaag. Containers, blobs en andere gegevensbronnen die toegankelijk zijn via de gegevenslaag. Bijvoorbeeld, als u wilt voor een lijst van uw opslagaccounts van Azure, stuurt u een aanvraag naar het beheereindpunt. Als u een lijst met blob-containers in een account wilt, kunt u een aanvraag verzendt naar de juiste service-eindpunt.

RBAC-rollen kunnen machtigingen voor toegang tot laag management of gegevens bevatten. De rol 'Lezer', verleent u bijvoorbeeld alleen-lezen toegang management layer-resources.

Strikt genomen met de rol 'Lezer' bevat geen gegevens laag machtigingen en is niet nodig voor toegang tot de gegevenslaag.

Storage Explorer kunt eenvoudig toegang tot uw resources door te verzamelen van de benodigde informatie om te verbinden met uw Azure-resources voor u. Bijvoorbeeld, als u uw blob-containers, verzendt Storage Explorer een lijst met containers-aanvraag naar het eindpunt van blob service. Als u dit eindpunt, Opslagverkenner zoekt in de lijst met abonnementen en storage-accounts hebt toegang tot. Maar als u wilt zoeken in uw abonnementen en opslagaccounts, moet Storage Explorer ook toegang hebben tot de beheerlaag.

Als u een rol die machtigingen worden de management laag hebt, kan de Storage Explorer gegevens die nodig zijn om te verbinden met de gegevenslaag niet ophalen.

### <a name="what-if-i-cant-get-the-management-layer-permissions-i-need-from-my-administrator"></a>Wat gebeurt er als ik niet kan het beheer van laag machtigingen krijgen die ik nodig van mijn beheerder?

Er zijn geen nog een oplossing RBAC met betrekking tot op dit moment. Als tijdelijke oplossing, kunt u een SAS-URI op aanvragen [koppelen aan uw resource](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=linux#attach-a-service-by-using-a-shared-access-signature-sas).

## <a name="error-self-signed-certificate-in-certificate-chain-and-similar-errors"></a>Fout: Zelf-ondertekend certificaat in de certificaatketen (en vergelijkbare fouten)

Certificaatfouten worden veroorzaakt door een van de twee volgende situaties:

1. De app is verbonden via een 'transparante proxy', wat betekent dat een server (zoals de bedrijfsserver van uw) is HTTPS-verkeer onderschept, ontsleutelt en vervolgens versleutelt met behulp van een zelfondertekend certificaat.
2. U kunt een toepassing die in de HTTPS-berichten die u ontvangt een zelf-ondertekend SSL-certificaat injecteert worden uitgevoerd. Voorbeelden van toepassingen die certificaten invoeren bevat antivirusprogramma's en netwerk verkeer inspectie software.

Wanneer Storage Explorer ziet een zelf-ondertekend of niet-vertrouwd certificaat, kan het niet meer te weten of het ontvangen HTTPS-bericht is gewijzigd. Als u een kopie van de zelf-ondertekend certificaat hebt, kunt u Storage Explorer instrueren vertrouwen aan de hand van de volgende stappen uit:

1. Verkrijgen van een Base-64 gecodeerde X.509 (.cer)-exemplaar van het certificaat
2. Klik op **bewerken** > **SSL-certificaten** > **certificaten importeren**, en vervolgens de bestandskiezer om te zoeken, selecteer en open het cer-bestand

Dit probleem kan ook worden de resultaten van meerdere certificaten (hoofdmap en tussenliggende). Beide certificaten moeten worden toegevoegd aan de fout te verhelpen.

Als u waar het certificaat vandaan komt weet, kunt u proberen deze stappen om te zoeken:

1. Installeer Open SSL
    * [Windows](https://slproweb.com/products/Win32OpenSSL.html) (een van de lichte versies is voldoende)
    * Mac en Linux: moet worden opgenomen met het besturingssysteem
2. Voer Open SSL uit
    * Windows: open de map wilt installeren, klik op **/bin/**, en dubbelklik vervolgens op **openssl.exe**.
    * Mac en Linux: Voer **openssl** vanuit een terminal.
3. Voer `s_client -showcerts -connect microsoft.com:443` uit
4. Zoek naar zelfondertekende certificaten. Als u welke certificaten zelfondertekend zijn weet, zoek naar een willekeurige plaats het onderwerp `("s:")` en -verlenersleutel `("i:")` zijn hetzelfde.
5. Als u geen zelfondertekende certificaten hebt gevonden, voor elk adres, kopieert en plakt u alles vanaf **---BEGIN CERTIFICATE---** naar **---END CERTIFICATE---** naar een nieuw .cer-bestand.
6. Storage Explorer openen, klikt u op **bewerken** > **SSL-certificaten** > **certificaten importeren**, en vervolgens de bestandskiezer om te zoeken, selecteren, en Open de CER-bestanden die u hebt gemaakt.

Als u geen zelfondertekende certificaten met behulp van de voorgaande stappen niet kunt vinden, contact met ons opnemen via het hulpprogramma feedback voor meer informatie. U kunt ook kunt u Storage Explorer starten vanuit de opdrachtregel met de `--ignore-certificate-errors` vlag. Wanneer met deze markering wordt gestart, zal de Storage Explorer certificaatfouten negeren.

## <a name="sign-in-issues"></a>Aanmeldingsproblemen

### <a name="blank-sign-in-dialog"></a>Lege aanmeldingsdialoogvenster

Lege aanmelden dialoogvensters worden meestal veroorzaakt door AD FS Storage Explorer waarin wordt gevraagd om uit te voeren een omleiding, wordt niet ondersteund door Electron. U kunt proberen om dit probleem te omzeilen apparaat Code Flow gebruiken voor aanmelding bij. Volg de volgende stappen om dit te doen:

1. 'Ga naar experimentele'-"Apparaat Code-aanmelding gebruiken" >.
2. Open het dialoogvenster voor verbinding maken (ofwel via de plug-pictogram in de linkerbenedenhoek verticale balk of 'Account toevoegen' in het venster account).
3. Kies welke omgeving u aanmelden wilt bij.
4. Klik op de 'aanmelden' knop.
5. Volg de instructies in het volgende deelvenster.

Opmerking: deze functie is momenteel alleen beschikbaar in 1.7.0 Preview-versie.

Als u zelf hebt u problemen bij het aanmelden bij het account dat u gebruiken wilt, omdat de standaardbrowser is al aangemeld bij een ander account, kunt u ofwel:

1. De koppeling en de code handmatig kopiëren naar een persoonlijke sessie van uw browser.
2. De koppeling en code handmatig kopiëren naar een andere browser.

### <a name="reauthentication-loop-or-upn-change"></a>Herauthenticatie lus of UPN wijzigen

Als u zich in een lus opnieuw verifiëren of de UPN van een van uw accounts hebt gewijzigd, probeert u het volgende:

1. Alle accounts verwijderen en sluit vervolgens Storage Explorer
2. Verwijder de. IdentityService map vanaf uw computer. Op Windows, de map bevindt zich in `C:\users\<username>\AppData\Local`. U kunt de map in de hoofdmap van uw directory gebruiker vinden voor Mac en Linux.
3. Als u van Mac- of Linux gebruikmaakt, moet u ook de vermelding Microsoft.Developer.IdentityService verwijderen uit uw besturingssystemen keystore. Op de Mac is de sleutelopslag de toepassing 'Gnome Keychain'. Voor Linux, de toepassing wordt meestal 'Sleutelhanger' genoemd, maar de naam kan afwijken afhankelijk van uw distributie.

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

* Als u zich op Mac OS en het venster aanmelden is nooit wordt weergegeven boven de 'wachten op verificatie...' dialoogvenster, klikt u vervolgens probeert [stappen](#mac-keychain-errors)
* Start u Opslagverkenner opnieuw
* Als het verificatievenster leeg is, wacht u minstens één minuut voordat de verificatie-dialoogvenster wordt gesloten.
* Zorg ervoor dat uw proxy en de instellingen juist zijn geconfigureerd voor uw computer en de Storage Explorer-certificaat.
* Als u zich op Windows en toegang tot Visual Studio 2017 op dezelfde computer hebt en zich aanmelden, probeert u aanmelden bij Visual Studio 2017. Na een geslaagde aanmelding naar Visual Studio 2017 moet u het volgende kan Storage Explorer openen en uw account in het deelvenster account ziet.

Als geen van deze methoden werken [opent u een probleem op GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues).

### <a name="missing-subscriptions-and-broken-tenants"></a>Ontbrekende abonnementen en verbroken tenants

Als u nog geen abonnementen ophalen nadat u aanmelden, probeert u de volgende methoden voor het oplossen van problemen:

* Controleer of dat uw account toegang heeft tot de abonnementen die u verwacht. U kunt uw toegang controleren door aan te melden bij de portal voor de Azure-omgeving die u probeert te gebruiken.
* Zorg ervoor dat u bent aangemeld bij het gebruik van de juiste Azure-omgeving (Azure, Azure China 21Vianet, Azure Duitsland, Azure US Government, of aangepaste omgeving).
* Als u zich achter een proxy bevindt, zorg ervoor dat u de Storage Explorer-proxy correct hebt geconfigureerd.
* Probeer te verwijderen en opnieuw toe te voegen in het account.
* Als er een koppeling 'Informatie', en weten welke foutberichten worden gerapporteerd voor de tenants die mislukken. Als u niet zeker weet wat te doen met de fout u berichten en u kunt vervolgens Zie [opent u een probleem op GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues).

## <a name="cannot-remove-attached-account-or-storage-resource"></a>Kan de gekoppelde account of storage resource niet verwijderen

Als u zich niet kunt verwijderen van een gekoppelde account of een resource voor opslag via de gebruikersinterface, kunt u alle gekoppelde resources handmatig verwijderen door het verwijderen van de volgende mappen:

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

Als u hulpprogramma's voor netwerken, zoals Fiddler voor Windows hebt, kunt u mogelijk problemen als volgt:

* Als u om te werken via de proxy hebt, is het wellicht uw netwerken hulpprogramma verbinding maken via de proxy configureren.
* Controleer het poortnummer dat door uw netwerken tool gebruikt.
* Voer de URL van de lokale host en het poortnummer van de netwerken van het programma als proxy-instellingen in Storage Explorer. Indien correct uitgevoerd, begint uw netwerken hulpprogramma logboekregistratie netwerkaanvragen die zijn gesteld door Storage Explorer management en service-eindpunten. Voer bijvoorbeeld https://cawablobgrs.blob.core.windows.net/ op het eindpunt van de blob in een browser, en u ontvangt een antwoord lijkt op het volgende, die duidt op de resource bestaat, hoewel u geen toegang krijgt.

![Voorbeeld van code](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>Neem contact op met de serverbeheerder proxy

Als de proxy-instellingen juist zijn, hebt u mogelijk contact op met uw proxy-server-beheerder, en

* Zorg ervoor dat uw proxy het verkeer naar de Azure management of resource-eindpunten worden niet geblokkeerd.
* Controleer of de verificatieprotocol wordt gebruikt door de proxyserver. Storage Explorer biedt momenteel geen ondersteuning voor NTLM-proxy's.

## <a name="unable-to-retrieve-children-error-message"></a>Foutbericht "Kan niet naar items ophalen"

Als u met Azure via een proxy verbonden bent, of u de proxy-instellingen juist zijn. Als u bij de eigenaar van het abonnement of account toegang tot een resource krijgen, controleert u of u hebt gelezen of lijst met machtigingen voor die bron.

## <a name="connection-string-does-not-have-complete-configuration-settings"></a>Verbindingsreeks heeft geen volledige configuratie-instellingen

Als u dit foutbericht ontvangt, is het mogelijk dat u bent niet gemachtigd de benodigde om op te halen van de sleutels voor uw opslagaccount. Om te bevestigen of dit het geval is, gaat u naar de portal en zoek uw opslagaccount. U kunt dit snel doen door met de rechtermuisknop op het knooppunt voor uw opslagaccount en te klikken op 'Openen in Portal'. Als u dit doet, gaat u naar de blade 'Toegangssleutel'. Als u bent niet gemachtigd om sleutels weer te geven, ziet u een pagina met het bericht 'U hebt geen toegang'. U kunt dit probleem omzeilen, kunt u de accountsleutel ophalen van iemand anders en koppelen met de naam en sleutel, of u kunt vragen voor een SAS met de Storage-account en deze gebruiken om te koppelen van het Storage-account.

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

Voor Linux-distributies dan Ubuntu 16.04 moet u mogelijk enkele afhankelijkheden handmatig installeren. In het algemeen zijn de volgende pakketten vereist:

* [.NET Core 2.x](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x)
* `libsecret`
* `libgconf-2-4`
* Recente GCC

Afhankelijk van uw distributie, is het mogelijk dat er een andere pakketten die u wilt installeren. Opslagverkenner [opmerkingen bij de Release](https://go.microsoft.com/fwlink/?LinkId=838275&clcid=0x409) specifieke stappen voor het aantal distributies bevatten.

## <a name="next-steps"></a>Volgende stappen

Als geen van de oplossingen voor u, klikt u vervolgens werken [opent u een probleem op GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues). U kunt ook snel GitHub krijgen met behulp van de knop 'Probleem melden op GitHub' in de linkerbenedenhoek.

![Feedback](./media/storage-explorer-troubleshooting/feedback-button.PNG)
