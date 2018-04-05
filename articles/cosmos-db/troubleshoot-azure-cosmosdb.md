---
title: Azure Cosmos DB beheren in Azure Storage Explorer
description: Leer Azure Cosmos DB beheren in Azure Storage Explorer.
Keywords: Azure Cosmos DB, Azure Storage Explorer, MongoDB
services: cosmos-db
documentationcenter: ''
author: Jejiang
manager: omafnan
editor: ''
tags: Azure Cosmos DB
ms.assetid: ''
ms.service: cosmos-db
ms.custom: Azure Cosmos DB active
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/20/2018
ms.author: jejiang
ms.openlocfilehash: c593eb2b49d15d20a26ef735d1032d5dea45f125
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="azure-cosmos-db-in-storage-explorer-troubleshooting-guide-overview"></a>Overzicht van de gids voor probleemoplossing voor Azure Cosmos DB in Storage Explorer

[Azure Cosmos DB in Azure Storage Explorer](https://docs.microsoft.com/en-us/azure/cosmos-db/storage-explorer) is een zelfstandige app waarmee u verbinding kunt maken met Azure Cosmos DB-accounts die worden gehost in Azure en onafhankelijke clouds vanuit Windows, Mac OS of Linux. Het gebruik van Azure Cosmos DB stelt u in staat om Azure Cosmos DB-entiteiten te beheren, gegevens te manipuleren, en opgeslagen procedures en triggers bij te werken, samen met andere Azure-entiteiten zoals opslag-blobs en wachtrijen.

Deze handleiding bevat een overzicht van oplossingen voor problemen die regelmatig worden gezien bij Azure Cosmos DB in Storage Explorer.

- [Problemen met aanmelden](#Sign-in-issues)
  - [Zelfondertekend certificaat in de certificaatketen](#Self-signed-certificate-in-certificate-chain)
  - [Kan geen abonnementen ophalen](#Unable-to-retrieve-subscriptions)
  - [Kan geen overzicht van de verificatiepagina zien](#Unable-to-see-the-authentication-page)
  - [Kan account niet verwijderen](#Cannot-remove-account)
- [Probleem voor http/https-proxy](#Http/Https-proxy-issue)
- [Probleem met het knooppunt 'Ontwikkeling' onder het knooppunt 'Lokaal en gekoppeld'](#Development-node-under-Local-and-Attached-node-issue)
- [Fout met toevoegen van Microsoft Azure Cosmos DB-account in 'Lokaal en gekoppeld' knooppunt](#Attaching-Azure-Cosmos-DB-account-in-Local-and-Attached-node-error)
- [Fout met uitvouwen van Azure Cosmos DB-knooppunt](#Expand-Azure-Cosmos-DB-node-error)
- [Volgende stappen](#Next-steps)

<h2 id="Sign-in-issues">Problemen met aanmelden</h2>

Probeer uw toepassing opnieuw op te starten en kijk of de problemen kunnen worden opgelost voordat u doorgaat.

<h2 id="Self-signed-certificate-in-certificate-chain">Zelfondertekend certificaat in de certificaatketen</h2>

Er zijn enkele redenen waarom u deze fout mogelijk ziet. Dit zijn de twee meest voorkomende:

1. U bevindt zich achter een transparante proxy, wat betekent dat een van de betrokkenen (zoals uw IT-afdeling) HTTPS-verkeer onderschept, ontsleutelt en vervolgens versleutelt met behulp van een zelfondertekend certificaat.

2. U voert software uit, zoals antivirussoftware, die zelfondertekende SSL-certificaten injecteert in de HTTPS-berichten die u ontvangt.

Wanneer Storage Explorer een van deze zelfondertekende certificaten tegenkomt, kan de toepassing niet meer weten of met het ontvangen HTTPS-bericht is geknoeid. Als u echter een kopie van het zelfondertekende certificaat hebt, kunt u Storage Explorer instrueren om dit te vertrouwen. Als u niet zeker weet wie het certificaat injecteert, kunt u dit zelf proberen uit te zoeken met de volgende stappen:

1. Installeer Open SSL
     - [Windows](https://slproweb.com/products/Win32OpenSSL.html) (een van de lichte versies is prima)
     - Mac- en Linux: moet bij het besturingssysteem zijn inbegrepen
2. Voer Open SSL uit
    - Windows: ga naar de installatiemap, vervolgens naar **/bin/** en dubbelklik op **openssl.exe**.
    - Mac- en Linux: voer **openssl** uit vanaf een terminal
3. Voer `s_client -showcerts -connect microsoft.com:443` uit
4. Zoek naar zelfondertekende certificaten. Als u niet zeker weet welke certificaten zelfondertekend zijn, zoek dan overal waar het onderwerp ('s:') en de certificaatverlener ('i') hetzelfde zijn.
5.  Als u zelfondertekende certificaten hebt gevonden, kopieert en plakt u alles vanaf **---BEGIN CERTIFICATE---** tot en met **---END CERTIFICATE---** naar een nieuw .cer-bestand. Doe dit voor elk certificaat.
6.  Open Storage Explorer en ga naar **Bewerken** > **SSL-certificaten** > **Certificaten importeren**. Gebruik de bestandskiezer en zoek, selecteer en open de cer-bestanden die u hebt gemaakt.

Als u met behulp van de bovenstaande stappen geen zelfondertekende certificaten kunt vinden, stuur ons dan feedback, zodat we u verder kunnen helpen.

<h2 id="Unable-to-retrieve-subscriptions">Kan geen abonnementen ophalen</h2>

Als u uw abonnementen niet kunt ophalen nadat u zich hebt aangemeld:

- Controleer of uw account toegang heeft tot de abonnementen door u aan te melden bij [Azure Portal](http://portal.azure.com/)
- Zorg ervoor dat u bent aangemeld via de juiste omgeving ([Azure](http://portal.azure.com/), [Azure China](https://portal.azure.cn/), [Azure Duitsland](https://portal.microsoftazure.de/), [Azure US Government](http://portal.azure.us/) of Aangepaste omgeving/Microsoft Azure Stack)
- Als u zich achter een proxy bevindt, controleert u of de Storage Explorer-proxy correct is geconfigureerd
- Probeer het account te verwijderen en opnieuw toe te voegen
- Verwijder de volgende bestanden uit de basismap (zoals: C:\Users\ContosoUser) en probeer vervolgens het account opnieuw toe te voegen:
  - .adalcache
  - .devaccounts
  - .extaccounts
- Bekijk tijdens het aanmelden de console voor ontwikkelhulpprogramma's (F12) voor eventuele foutberichten

![console](./media/troubleshoot-cosmosdb/console.png)

<h2 id="Unable-to-see-the-authentication-page">Kan de verificatiepagina niet zien</h2>  

Als u de verificatiepagina niet kunt zien:

- Afhankelijk van de snelheid van uw verbinding kan het even duren voordat de aanmeldingspagina is geladen. Wacht minstens één minuut voordat u het dialoogvenster voor verificatie sluit.
- Als u zich achter een proxy bevindt, controleert u of de Storage Explorer-proxy correct is geconfigureerd
- Start de ontwikkelaarsconsole door op de F12-toets te drukken. Bekijk de antwoorden van de ontwikkelaarsconsole en ga na of u een aanwijzing kunt vinden waarom de verificatie niet werkt

<h2 id="Cannot-remove-account">Kan het account niet verwijderen</h2>

Als het niet lukt om een account te verwijderen of als de koppeling om opnieuw te verifiëren niet werkt:

- Verwijder de volgende bestanden uit de basismap en probeer vervolgens het account opnieuw toe te voegen:
  - .adalcache
  - .devaccounts
  - .extaccounts
- Als u aan SAS gekoppelde opslagbronnen wilt verwijderen, verwijder dan:
  - De map %AppData%/StorageExplorer voor Windows
  - /Users/<your_name>/Library/Applicaiton SUpport/StorageExplorer voor Mac
  - ~/.config/StorageExplorer voor Linux
  - **U moet echter al uw referenties opnieuw invoeren** als u deze bestanden verwijdert.


<h2 id="Http/Https-proxy-issue">Probleem met http/https-proxy</h2>

U kunt Microsoft Azure Cosmos DB-knooppunten in het linkerdeelvenster niet weergeven bij het configureren van een http/https-proxy in ASE. Dit is een bekend probleem en wordt opgelost in de volgende release. Op dit moment kunt u als tijdelijke oplossing Azure Cosmos DB Data Explorer in Azure Portal gebruiken. 

<h2 id="Development-node-under-Local-and-Attached-node-issue">Probleem met het knooppunt 'Ontwikkeling' onder het knooppunt 'Lokaal en gekoppeld'</h2>

Er is geen reactie na klikken op het knooppunt 'Ontwikkeling' onder het knooppunt 'Lokaal en gekoppeld' in het linkerdeelvenster.  Dit gedrag is verwacht. De lokale emulator van Microsoft Azure Cosmos DB wordt in de volgende release ondersteund.

![Knooppunt 'Ontwikkeling'](./media/troubleshoot-cosmosdb/development.png)

<h2 id="Attaching-Azure-Cosmos-DB-account-in-Local-and-Attached-node-error">Fout met toevoegen van Microsoft Azure Cosmos DB-account in het knooppunt 'Lokaal en gekoppeld'</h2>

Als u onderstaande fout ziet na het toevoegen van een Microsoft Azure Cosmos DB-account in het knooppunt 'Lokaal en gekoppeld', controleert u of u de juiste verbindingsreeks gebruikt.

![Fout met toevoegen van Microsoft Azure Cosmos DB-account in 'Lokaal en gekoppeld'](./media/troubleshoot-cosmosdb/attached-error.png)

<h2 id="Expand-Azure-Cosmos-DB-node-error">Fout met uitvouwen van Azure-Cosmos DB-knooppunt</h2>

Mogelijk ziet u onderstaande fout tijdens een poging de structuurknooppunten links uit te vouwen. 

![Uitvouwfout](./media/troubleshoot-cosmosdb/expand-error.png)

Probeer de volgende suggesties:

- Controleer of het Microsoft Azure Cosmos DB-account bezig is met inrichten en probeer het opnieuw wanneer het account is gemaakt.
- Als het account zich onder het knooppunt 'Snelle toegang' of 'Lokaal en gekoppelde' bevindt, controleert u of het account is verwijderd. Als dit het geval is, moet u het knooppunt handmatig verwijderen.

<h2 id="Next-steps">Volgende stappen</h2>

Als geen van de oplossingen voor u werken, stuurt u een e-mail naar het Azure Cosmos DB Dev Tooling Team ([cosmosdbtooling@microsoft.com](mailto:cosmosdbtooling@microsoft.com)) met informatie over het probleem. Wij gaan hiermee dan aan de slag.





