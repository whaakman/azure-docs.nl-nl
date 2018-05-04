---
title: Azure Storage Explorer probleemoplossingsgids | Microsoft Docs
description: Overzicht van de twee foutopsporingsfunctie van Azure
services: virtual-machines
documentationcenter: ''
author: Deland-Han
manager: cshepard
editor: ''
ms.assetid: ''
ms.service: virtual-machines
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/08/2017
ms.author: delhan
ms.openlocfilehash: f58fb5090aba3c5052d1bbdec76225d0ae50e8f2
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Azure Storage Explorer probleemoplossingsgids

Microsoft Azure Storage Explorer is een zelfstandige app waardoor u eenvoudig werken met Azure Storage-gegevens op Windows-, Mac OS- en Linux. De app kan verbinding maken met de Storage-accounts die worden gehost op Azure, nationale Clouds en Azure-Stack.

Deze handleiding bevat een overzicht van oplossingen voor algemene problemen gezien in Opslagverkenner.

## <a name="error-self-signed-certificate-in-certificate-chain-and-similar-errors"></a>Fout: Zelf-ondertekend certificaat in de certificaatketen (en vergelijkbare fouten)

Certificaatfouten worden veroorzaakt door een van de twee volgende situaties:

1. De app is verbonden via een transparante proxy-, wat betekent dat een server (zoals de bedrijfsserver van uw) onderscheppen HTTPS-verkeer, ontsleutelen van deze en vervolgens versleutelen met behulp van een zelfondertekend certificaat.
2. U kunt een toepassing die is injecteren van een zelfondertekend SSL-certificaat in de HTTPS-berichten die u ontvangt worden uitgevoerd. Voorbeelden van toepassingen die certificaten invoeren inclusief antivirus- en verkeer inspectie software.

Wanneer Opslagverkenner ziet een zelf ondertekend of niet-vertrouwd certificaat, kan het niet meer te weten of het ontvangen bericht voor HTTPS is gewijzigd. Als u een kopie van het zelfondertekend certificaat hebt, kunt u opdracht geven Opslagverkenner vertrouwen als volgt u de volgende stappen uit:

1. Verkrijgen van een Base-64 gecodeerde X.509 (.cer) kopie van het certificaat
2. Klik op **bewerken** > **SSL-certificaten** > **certificaten importeren**, en gebruik vervolgens de bestandskiezer om te zoeken, selecteert en opent u het cer-bestand

Dit probleem kan ook worden de resultaten van meerdere certificaten (hoofdmap en tussenliggende). Beide certificaten moeten worden toegevoegd aan de fout te verhelpen.

Als u waar het certificaat vandaan komt weet, kunt u proberen deze stappen om te zoeken:

1. Installeer Open SSL

    * [Windows](https://slproweb.com/products/Win32OpenSSL.html) (een van de lichte versies moet voldoende)
    * Mac- en Linux: moet worden opgenomen met het besturingssysteem
2. Voer Open SSL uit

    * Windows: open de map wilt installeren, klik op **/bin/**, en dubbelklik vervolgens op **openssl.exe**.
    * Mac- en Linux: Voer **openssl** van een definitieve.
3. Voer `s_client -showcerts -connect microsoft.com:443` uit
4. Zoek naar zelfondertekende certificaten. Als u welke zijn zelf-ondertekend weet, zoekt u naar een willekeurige plaats het onderwerp `("s:")` en de verlener `("i:")` zijn hetzelfde.
5. Wanneer u zelfondertekende certificaten hebt gevonden, voor elk adres kopieert en plakt u alles uit en inclusief **---BEGIN CERTIFICATE---** naar **---EINDCERTIFICAAT---** naar een nieuw .cer-bestand.
6. Open Opslagverkenner, klik op **bewerken** > **SSL-certificaten** > **certificaten importeren**, en gebruik vervolgens de bestandskiezer om te zoeken, selecteert, en Open het cer-bestanden die u hebt gemaakt.

Als u geen zelfondertekende certificaten met behulp van de voorgaande stappen niet kunt vinden, contact met ons opnemen via het hulpprogramma feedback voor meer informatie. U kunt ook kiezen Opslagverkenner starten vanaf de opdrachtregel met de `--ignore-certificate-errors` vlag. Wanneer met deze markering wordt gestart, wordt in Opslagverkenner certificaatfouten negeren.

## <a name="sign-in-issues"></a>Problemen met aanmelden

Als u zich niet aanmelden, probeert u de volgende methoden:

* Opnieuw opstarten Opslagverkenner
* Als het verificatievenster leeg is, wacht u minstens één minuut voordat u sluit het verificatiedialoogvenster.
* Zorg ervoor dat uw proxy- en certificaat instellingen correct zijn geconfigureerd voor de computer en de Opslagverkenner
* Als u in Windows en toegang tot Visual Studio 2017 op dezelfde computer en aanmelding, probeert u aanmelden bij Visual Studio 2017

Als geen van deze methoden werkt [opent u een probleem op GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues).

## <a name="unable-to-retrieve-subscriptions"></a>Kan geen abonnementen ophalen

Als u nog niet ophalen van uw abonnementen, nadat u zich hebt aangemeld, probeert u de volgende methoden:

* Controleer of dat uw account toegang heeft tot de abonnementen die u verwacht. U kunt controleren of u toegang aanmeldt bij de portal voor de Azure-omgeving die u probeert hebt te gebruiken.
* Zorg ervoor dat u zich aan met de juiste Azure omgeving (Azure, Azure China, Duitse Azure, Azure US Government of aangepaste omgeving).
* Als u zich achter een proxy, zorg er dan voor dat u de proxy Opslagverkenner juist hebt geconfigureerd.
* Verwijder en readding van het account.
* Bekijk de console ontwikkelhulpprogramma's (Help > Toggle ontwikkelhulpprogramma's) tijdens Opslagverkenner abonnementen wordt geladen. Zoeken naar foutberichten (rode tekst) of een bericht met de tekst 'kan niet worden geladen abonnementen voor de tenant." Als u alle betreffende berichten ziet [opent u een probleem op GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues).

## <a name="cannot-remove-attached-account-or-storage-resource"></a>Kan de gekoppelde account of storage resource niet verwijderen.

Als u niet verwijderen van een gekoppelde account of een opslagresource via de gebruikersinterface, kunt u alle gekoppelde resources handmatig verwijderen door de volgende mappen te verwijderen:

* Windows: `%AppData%/StorageExplorer`
* Mac OS: `/Users/<your_name>/Library/Applicaiton Support/StorageExplorer`
* Linux: `~/.config/StorageExplorer`

> [!NOTE]
>  Opslagverkenner sluiten voordat u de bovenstaande mappen verwijderen.

> [!NOTE]
>  Als u ooit de SSL-certificaten hebt geïmporteerd en vervolgens back-up van de inhoud van de `certs` directory. Later kunt u de back-up van uw SSL-certificaten opnieuw importeren.

## <a name="proxy-issues"></a>Proxy-problemen

Controleer eerst of dat de volgende informatie die u hebt ingevoerd juist zijn:

* De proxy-URL en het poortnummer * gebruikersnaam en wachtwoord, indien vereist door de proxy

### <a name="common-solutions"></a>Algemene oplossingen

Als u steeds problemen ondervindt nog, probeert u de volgende methoden:

* Als u verbinding met Internet maken kunt zonder gebruik van uw proxyserver, moet u controleren of Opslagverkenner zonder proxyinstellingen zijn ingeschakeld werkt. Als dit het geval is, is er mogelijk een probleem met uw proxy-instellingen. Werken met de proxy-beheerder om de problemen te identificeren.
* Controleer of andere toepassingen die gebruikmaken van de proxyserver werken zoals verwacht.
* Controleer of u verbinding kunt maken naar de portal voor de Azure-omgeving die u probeert te gebruiken
* Controleer of u kunt reacties van uw service-eindpunten te ontvangen. Voer een van de eindpunt-URL's in uw browser. Als u verbinding maken kunt, ontvangt u een InvalidQueryParameterValue of een vergelijkbare XML-antwoord.
* Als iemand anders Opslagverkenner ook met de proxyserver gebruikt, controleert u of dat ze verbinding kunnen maken. Als ze verbinding maken kunnen, hebt u mogelijk contact op met uw proxy-server.

### <a name="tools-for-diagnosing-issues"></a>Hulpprogramma's voor het oplossen van problemen

Als u hulpprogramma's voor netwerken, zoals Fiddler voor Windows hebt, kunt u mogelijk problemen als volgt:

* Als u om te werken via de proxy hebt, moet u wellicht uw netwerken hulpprogramma om verbinding via de proxy te configureren.
* Controleer het poortnummer dat wordt gebruikt door uw netwerken hulpprogramma.
* Voer de URL van de lokale host en het poortnummer van de netwerken hulpprogramma als proxy-instellingen in Opslagverkenner. Indien correct uitgevoerd, start u uw netwerken hulpprogramma logboekregistratie netwerkaanvragen door Opslagverkenner voor beheer en service-eindpunten. Voer bijvoorbeeld https://cawablobgrs.blob.core.windows.net/ voor het eindpunt van de blob in een browser, en u ontvangt een antwoord lijkt op het volgende voorbeeld, waarin wordt voorgesteld de bron bestaat, hoewel u geen toegang toe heeft.

![Voorbeeld van code](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>Neem contact op met de serverbeheerder proxy

Als uw proxy-instellingen correct zijn, hebt u mogelijk contact opnemen met uw beheerder proxy-server en

* Zorg ervoor dat uw proxy geen verkeer naar Azure management of de resource-eindpunten blokkeert.
* Controleer of het verificatieprotocol dat wordt gebruikt door de proxyserver. Opslagverkenner biedt momenteel geen ondersteuning voor NTLM-proxy's.

## <a name="unable-to-retrieve-children-error-message"></a>Foutbericht 'Kan niet naar onderliggende elementen ophalen'

Als u met Azure via een proxy verbonden bent, moet u controleren of de proxyinstellingen juist zijn. Als u zijn toegang tot een resource verleend aan de eigenaar van het abonnement of account, Controleer of u hebt gelezen of lijst met machtigingen voor die bron.

### <a name="issues-with-sas-url"></a>Problemen met SAS-URL
Als u verbinding met een service met behulp van een SAS-URL en deze fout optreedt maakt:

* Controleer of dat de URL van de vereiste machtigingen voor lezen of lijst van bronnen biedt.
* Controleer of de URL is niet verlopen.
* Als de SAS-URL is gebaseerd op een toegangsbeleid, controleert u of het beleid voor toegang niet is ingetrokken.

Als u per ongeluk gekoppeld met behulp van een ongeldige SAS-URL en kan niet worden losgekoppeld, voert u de volgende stappen uit:
1.  Als u Opslagverkenner, drukt u op F12 om de hulpprogramma's voor ontwikkelaars venster te openen.
2.  Klik op het tabblad toepassing en klik vervolgens op van de lokale opslag > file:// in de structuur aan de linkerkant.
3.  Zoek de sleutel gekoppeld aan het type van de service van de problematisch SAS-URI. Bijvoorbeeld, als de onjuiste SAS-URI voor een blob-container, zoekt u naar de sleutel met de naam `StorageExplorer_AddStorageServiceSAS_v1_blob`.
4.  De waarde van de sleutel moet een JSON-matrix. Het object dat is gekoppeld met ongeldige URI niet vinden en verwijderen.
5.  Druk op Ctrl + R om opnieuw te laden Opslagverkenner.

## <a name="linux-dependencies"></a>Linux-afhankelijkheden

Voor Linux-distributies dan Ubuntu 16.04, moet u wellicht handmatig enkele afhankelijkheden te installeren. In het algemeen zijn de volgende pakketten vereist:
* [.NET core 2.x](https://docs.microsoft.com/dotnet/core/linux-prerequisites?tabs=netcore2x)
* `libsecret`
* `libgconf-2-4`
* Up-to-date GCC

Afhankelijk van uw distro mogelijk zijn er andere pakketten die u wilt installeren. Opslagverkenner [releaseopmerkingen](https://go.microsoft.com/fwlink/?LinkId=838275&clcid=0x409) specifieke stappen voor een aantal distributies bevatten.

## <a name="next-steps"></a>Volgende stappen

Als geen van de oplossingen, klikt u vervolgens werken [opent u een probleem op GitHub](https://github.com/Microsoft/AzureStorageExplorer/issues). U kunt ook snel met GitHub ophalen met behulp van de knop 'Rapport probleem met GitHub' in de linkerbenedenhoek.

![Feedback](./media/storage-explorer-troubleshooting/feedback-button.PNG)
