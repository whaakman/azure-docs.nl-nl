---
title: Azure Storage Explorer probleemoplossingsgids | Microsoft Docs
description: Overzicht van de twee foutopsporingsfunctie van Azure
services: virtual-machines
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/08/2017
ms.author: delhan
ms.openlocfilehash: 3187939fa813f941c2fe12a359df474a6c487c71
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a>Azure Storage Explorer probleemoplossingsgids

Microsoft Azure Opslagverkenner (Preview) is een zelfstandige app waardoor u eenvoudig werken met Azure Storage-gegevens op Windows, Mac OS- en Linux. De app kan verbinding maken met de Storage-accounts die worden gehost op Azure, soevereine Clouds en Azure-Stack.

Deze handleiding bevat een overzicht van oplossingen voor algemene problemen gezien in Opslagverkenner.

## <a name="sign-in-issues"></a>Problemen met aanmelden

Alleen Azure Active Directory (AAD) accounts worden ondersteund. Als u een AD FS-account gebruikt, wordt verwacht dat het aanmelden bij Opslagverkenner niet werkt. Voordat u doorgaat, probeer uw toepassing opnieuw te starten en zien of de problemen te corrigeren.

### <a name="error-self-signed-certificate-in-certificate-chain"></a>Fout: Zelf-ondertekend certificaat in de certificaatketen

Er zijn diverse redenen waarom u deze fout kan optreden, en de twee meest voorkomende redenen zijn als volgt:

1. De app is verbonden via een transparante proxy-, wat betekent dat een server (zoals de bedrijfsserver van uw) onderscheppen HTTPS-verkeer, ontsleutelen van deze en vervolgens versleutelen met behulp van een zelfondertekend certificaat.

2. U kunt een toepassing, zoals antivirussoftware, die is injecteren van een zelfondertekend SSL-certificaat in de HTTPS-berichten die u ontvangt worden uitgevoerd.

Wanneer Opslagverkenner een van de problemen tegenkomt, kan het niet meer te weten of het ontvangen bericht voor HTTPS is geknoeid. Als u een kopie van het zelfondertekend certificaat hebt, kunt u Opslagverkenner vertrouwen. Als u die het certificaat is injecteren weet, als volgt te werk:

1. Open SSL installeren

    - [Windows](https://slproweb.com/products/Win32OpenSSL.html) (een van de lichte versies moet voldoende)

    - Mac- en Linux: moet worden opgenomen met het besturingssysteem

2. Open SSL uitvoeren

    - Windows: open de map wilt installeren, klik op **/bin/**, en dubbelklik vervolgens op **openssl.exe**.
    - Mac- en Linux: Voer **openssl** van een definitieve.

3. Uitvoeren van s_client - showcerts-microsoft.com:443 verbinding

4. Zoek naar de zelfondertekende certificaten. Als u niet zeker dat zelf ondertekend weet, zoek naar elke locatie het onderwerp ('s:') en de verlener ('i') zijn hetzelfde.

5. Wanneer u zelfondertekende certificaten hebt gevonden, voor elk adres kopieert en plakt u alles uit en inclusief **---BEGIN CERTIFICATE---** naar **---EINDCERTIFICAAT---** naar een nieuw .cer-bestand.

6. Open Opslagverkenner, klik op **bewerken** > **SSL-certificaten** > **certificaten importeren**, en gebruik vervolgens de bestandskiezer om te zoeken, selecteert, en Open het cer-bestanden die u hebt gemaakt.

Als u geen zelfondertekende certificaten met behulp van de bovenstaande stappen niet kunt vinden, contact met ons opnemen via het hulpprogramma feedback voor meer informatie.

### <a name="unable-to-retrieve-subscriptions"></a>Kan geen abonnementen ophalen

Als u niet ophalen van uw abonnementen, nadat u zich hebt aangemeld, volg deze stappen om dit probleem oplossen:

- Controleer of dat uw account toegang tot de abonnementen heeft wanneer u zich aanmeldt bij de Azure portal.

- Zorg ervoor dat u bent aangemeld met behulp van de juiste omgeving (Azure, Azure China, Duitse Azure, Azure US Government of aangepaste omgeving/Azure Stack).

- Als u zich achter een proxy, zorg er dan voor dat u de proxy Opslagverkenner juist hebt geconfigureerd.

- Verwijder en readding van het account.

- Probeer een van de volgende bestanden worden verwijderd uit de hoofddirectory (dat wil zeggen, C:\Users\ContosoUser) en vervolgens opnieuw toe te voegen het account:

    - .adalcache

    - .devaccounts

    - .extaccounts

- Bekijk de ontwikkelhulpprogramma's console (door op F12 wordt gedrukt) wanneer u voor eventuele foutberichten aanmeldt zich:

![Hulpprogramma's voor ontwikkelaars](./media/storage-explorer-troubleshooting/4022501_en_2.png)

### <a name="unable-to-see-the-authentication-page"></a>Kan niet voor een overzicht van de verificatiepagina

Als u niet wilt weergeven van de verificatiepagina als volgt te werk om dit probleem oplossen:

- Afhankelijk van de snelheid van uw verbinding duurt lang voordat de aanmeldingspagina te laden, wacht u minstens één minuut voordat u sluit het verificatiedialoogvenster.

- Als u zich achter een proxy, zorg er dan voor dat u de proxy Opslagverkenner juist hebt geconfigureerd.

- De developer-console weergeven door op de F12-toets te drukken. Bekijk de antwoorden van de developer-console en ontdek of voor u een aanwijzing Waarom vindt verificatie werkt niet.

### <a name="cannot-remove-account"></a>Kan het account niet verwijderen

Als u niet om een account te verwijderen, of als de koppeling opnieuw verifiëren heeft geen invloed, volg deze stappen om dit probleem oplossen:

- Probeer de volgende bestanden worden verwijderd uit de hoofddirectory en vervolgens readding het account:

    - .adalcache

    - .devaccounts

    - .extaccounts

- Als u wilt verwijderen SAS opslagbronnen aangesloten, verwijdert u de volgende bestanden:

    - De map %AppData%/StorageExplorer voor Windows

    - /Gebruikers/ < Uw_naam >/Library/Applicaiton ondersteuning/StorageExplorer voor Mac

    - ~/.config/StorageExplorer voor Linux

> [!NOTE]
>  U moet alle uw referenties opnieuw invoeren als u deze bestanden verwijdert.

## <a name="proxy-issues"></a>Proxy-problemen

Controleer eerst of dat de volgende informatie die u hebt ingevoerd juist zijn:

- De proxy-URL en het poortnummer

- Gebruikersnaam en wachtwoord, indien vereist door de proxy

### <a name="common-solutions"></a>Algemene oplossingen

Als u steeds problemen ondervindt nog, volg deze stappen voor het oplossen van deze:

- Als u verbinding met Internet maken kunt zonder gebruik van uw proxyserver, moet u controleren of Opslagverkenner zonder proxyinstellingen zijn ingeschakeld werkt. Als dit het geval is, is er mogelijk een probleem met uw proxy-instellingen. Werken met de proxy-beheerder om de problemen te identificeren.

- Controleer of andere toepassingen die gebruikmaken van de proxyserver werken zoals verwacht.

- Controleer of u verbinding kunt maken voor de Microsoft Azure-portal met behulp van uw webbrowser

- Controleer of u kunt reacties van uw service-eindpunten te ontvangen. Voer een van de eindpunt-URL's in uw browser. Als u verbinding maken kunt, ontvangt u een InvalidQueryParameterValue of een vergelijkbare XML-antwoord.

- Als iemand anders Opslagverkenner ook met de proxyserver gebruikt, controleert u of dat ze verbinding kunnen maken. Als ze verbinding maken kunnen, hebt u mogelijk contact op met uw proxy-server.

### <a name="tools-for-diagnosing-issues"></a>Hulpprogramma's voor het oplossen van problemen

Als u hulpprogramma's voor netwerken, zoals Fiddler voor Windows hebt, kunt u mogelijk problemen als volgt:

- Als u om te werken via de proxy hebt, moet u wellicht uw netwerken hulpprogramma om verbinding via de proxy te configureren.

- Controleer het poortnummer dat wordt gebruikt door uw netwerken hulpprogramma.

- Voer de URL van de lokale host en het poortnummer van de netwerken hulpprogramma als proxy-instellingen in Opslagverkenner. Als dit correct is uitgevoerd, start u uw netwerken hulpprogramma logboekregistratie netwerkaanvragen door Opslagverkenner voor beheer en service-eindpunten. Voer bijvoorbeeld https://cawablobgrs.blob.core.windows.net/ voor het eindpunt van de blob in een browser en ontvangt u een antwoord lijkt op het volgende voorbeeld, waarin wordt voorgesteld de bron bestaat, hoewel u geen toegang toe heeft.

![Voorbeeld van code](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a>Neem contact op met de serverbeheerder proxy

Als uw proxy-instellingen correct zijn, hebt u mogelijk contact opnemen met uw beheerder proxy-server en

- Zorg ervoor dat uw proxy geen verkeer naar Azure management of de resource-eindpunten blokkeert.

- Controleer of het verificatieprotocol dat wordt gebruikt door de proxyserver. Opslagverkenner biedt momenteel geen ondersteuning voor NTLM-proxy's.

## <a name="unable-to-retrieve-children-error-message"></a>Foutbericht 'Kan niet naar onderliggende elementen ophalen'

Als u met Azure via een proxy verbonden bent, moet u controleren of de proxyinstellingen juist zijn. Als u zijn toegang tot een resource verleend aan de eigenaar van het abonnement of account, Controleer of u hebt gelezen of lijst met machtigingen voor die bron.

### <a name="issues-with-sas-url"></a>Problemen met SAS-URL
Als u verbinding met een service met behulp van een SAS-URL en deze fout optreedt maakt:

- Controleer of dat de URL van de vereiste machtigingen voor lezen of lijst van bronnen biedt.

- Controleer of de URL is niet verlopen.

- Als de SAS-URL is gebaseerd op een toegangsbeleid, controleert u of het beleid voor toegang niet is ingetrokken.

Als uw per ongeluk een ongeldige SAS-URL die is gekoppeld en kan niet loskoppelen, volg dan deze stappen:
1.  Als u Opslagverkenner, drukt u op F12 om de hulpprogramma's voor ontwikkelaars venster te openen.
2.  Klik op het tabblad toepassing en klik vervolgens op van de lokale opslag > file:// in de structuur aan de linkerkant.
3.  Zoek de sleutel gekoppeld aan het type van de service van de problematisch SAS-URI. Bijvoorbeeld, als de onjuiste SAS-URI voor een blob-container, zoek naar de sleutel met de naam 'StorageExplorer_AddStorageServiceSAS_v1_blob'.
4.  De waarde van de sleutel moet een JSON-matrix. Het object dat is gekoppeld met ongeldige URI niet vinden en verwijderen.
5.  Druk op Ctrl + R om opnieuw te laden Opslagverkenner.


## <a name="next-steps"></a>Volgende stappen

Als geen van de oplossingen die voor u werken, Geef uw probleem via het hulpprogramma feedback met uw e-mailadres en zo veel meer informatie over het probleem dat is opgenomen als u kunnen, zodat we kunnen contact met u voor het oplossen van het probleem.

Om dit te doen, klikt u op **Help** menu en klik vervolgens op **Feedback verzenden**.

![Feedback](./media/storage-explorer-troubleshooting/4022503_en_1.png)
