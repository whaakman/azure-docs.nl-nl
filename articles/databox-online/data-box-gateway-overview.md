---
title: Overzicht van Microsoft Azure Data Box Gateway | Microsoft Docs
description: Beschrijft Azure Data Box Gateway, een opslagoplossing voor virtuele apparaten waarmee u gegevens naar Azure kunt overdragen
services: databox
author: alkohli
ms.service: databox
ms.topic: overview
ms.date: 03/20/2019
ms.author: alkohli
ms.openlocfilehash: e7a65a6b49544783ed3f40194e2338540819536b
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400960"
---
# <a name="what-is-azure-data-box-gateway"></a>Wat is Azure Data Box-Gateway?

Azure Data Box Gateway is een opslagoplossing waarmee u naadloos gegevens naar Azure kunt verzenden. Dit artikel biedt een overzicht van de Azure Data Box Gateway-oplossing, voordelen, belangrijkste mogelijkheden en de scenario’s waarin u dit apparaat kunt implementeren.

Data Box Gateway is een virtueel apparaat op basis van een virtuele machine die in uw gevirtualiseerde omgeving of hypervisor is ingericht. Het virtuele apparaat bevindt zich op uw locatie en u schrijft gegevens ernaar met behulp van het NFS- en SMB-protocol. Het apparaat draagt uw gegevens vervolgens over naar Azure blok-blob, pagina-blob of Azure Files.

## <a name="use-cases"></a>Gebruiksvoorbeelden

Data Box Gateway kan worden gebruikt voor het overdragen van gegevens naar de cloud, zoals voor archivering in de cloud, herstel na noodgevallen of als uw gegevens in de cloud moeten worden verwerkt. Hier volgen de verschillende scenario’s waarin Data Box Gateway kan worden gebruikt voor gegevensoverdracht.

- **Archivering in de cloud** - Kopieer honderden TB’s aan gegevens veilig en efficiënt naar Azure-opslag met behulp van Data Box Gateway. De gegevens kunnen in één keer of voortdurend worden opgenomen voor archiveringsdoeleinden.

- **Continue gegevensopname** -continu gegevens opnemen in het apparaat om te kopiëren naar de cloud, ongeacht de grootte van de gegevens. Als de gegevens worden geschreven naar het gateway-apparaat, wordt het apparaat de gegevens geüpload naar Azure Storage.  

- **Bulksgewijs overdracht gevolgd door incrementele overdracht voor de eerste** -gebruik Data Box voor het bulksgewijs in offline modus (basiswaarde) en Data Box Gateway overdragen voor incrementele overdrachten (doorlopende feed) via het netwerk.

## <a name="benefits"></a>Voordelen

Data Box Gateway biedt de volgende voordelen:

- **Gemakkelijke gegevensoverdracht** - Het verplaatsen van gegevens in en uit Azure-opslag is net zo gemakkelijk als het werken met een lokale netwerkshare.  
- **Hoge prestaties** - Maakt overdracht van netwerkgegevens minder complex met snelle gegevensoverdrachten naar en vanuit Azure.
- **Snelle toegang en gegevensopname met hoge snelheid tijdens kantooruren** - Data Box-Gateway heeft een lokale cache die u als de grootte van de lokale capaciteit definieert wanneer het virtuele apparaat is ingericht. De grootte van de gegevensschijf moet worden opgegeven volgens de [minimale vereisten voor virtuele apparaten](data-box-gateway-system-requirements.md#specifications-for-the-virtual-device). De lokale cache biedt de volgende voordelen:
    - De lokale cache biedt de mogelijkheid tot gegevensopname met een hoge snelheid. Wanneer een grote hoeveelheid gegevens wordt opgenomen tijdens piekuren, kan de cache de gegevens bevatten en naar de cloud uploaden.
    - De lokale cache biedt snelle leestoegang tot een bepaalde drempelwaarde. Totdat het apparaat voor 50 tot 60 % vol is, zijn alle leesbewerkingen van het apparaat toegankelijk vanuit de cache, zodat deze sneller kunnen plaatsvinden. Zodra de gebruikte ruimte op het apparaat boven deze drempelwaarde gaat, vervolgens het apparaat wordt gestart om lokale bestanden te verwijderen.
 
- **Beperkt bandbreedtegebruik**: Gegevens kunnen zelfs naar Azure worden geschreven wanneer het netwerk wordt beperkt voor minder gebruik tijdens piekuren.  

## <a name="key-capabilities"></a>Belangrijkste mogelijkheden

Data Box Gateway biedt de volgende mogelijkheden:

|Mogelijkheid |Description  |
|---------|---------|
|Snelheid     | Volledig geautomatiseerde en zeer geoptimaliseerde gegevensoverdracht en bandbreedte.|
|Ondersteunde protocollen     | Ondersteuning voor het standaard SMB- en NFS-protocol voor gegevensopname. <br> Ga naar [Systeemvereisten voor Data Box Gateway](data-box-gateway-system-requirements.md) voor meer informatie over ondersteunde versies.|
|Toegang tot gegevens     | Zodra de gegevens verzonden door apparaat zich in de cloud, kan het verder worden gewijzigd door rechtstreeks toegang hebben tot de cloud-API's.|
|Snelle toegang     | Lokale cache op het apparaat voor snelle toegang tot laatst gebruikte bestanden.|
|Offline upload     | Modus zonder verbinding ondersteunt scenario’s voor offline uploaden.|
|Gegevensvernieuwing     | Mogelijkheid om lokale bestanden te vernieuwen met de meest recente uit de cloud.|
|Versleuteling    | BitLocker-ondersteuning om gegevens lokaal te versleutelen en de gegevensoverdracht naar de cloud via *HTTPS* te beveiligen       |
|Flexibiliteit     | Ingebouwde netwerkflexibiliteit        |


## <a name="specifications"></a>Specificaties

Het virtuele Data Box Gateway-apparaat heeft de volgende specificaties:

| Specificaties                                          | Description              |
|---------------------------------------------------------|--------------------------|
| Virtuele processors (kernen)   | Minimaal 4 |
| Geheugen  |Minimaal 8 GB|
| Beschikbaarheid|Eén knooppunt|
| Disks|Besturingssysteemschijf: 250 GB <br> Gegevensschijf: minimaal 2 TB, thin-provisioned en moet worden ondersteund door SSD's|
| Netwerkinterfaces |1 of meer virtuele netwerkinterfaces|
| Systeemeigen protocollen voor het delen van bestanden|SMB en NFS  |
| Beveiliging|Verificatie voor het ontgrendelen van toegang tot apparaten en gegevens <br> Gegevens worden tijdens overdracht versleuteld met AES-256-bits-versleuteling|
| Beheer|Lokale webgebruikersinterface - Eerste installatie, diagnostische gegevens en energiebeheer van apparaat <br> Azure Portal - Dagelijks beheer van Data Box Gateway-apparaten       |

## <a name="components"></a>Onderdelen

De Data Box Gateway-oplossing bestaat uit een Data Box Gateway-resource, een virtueel Data Box Gateway-apparaat en een lokale webinterface.

- **Virtueel Data Box Gateway-apparaat** - Een apparaat op basis van een virtuele machine die in uw gevirtualiseerde omgeving of hypervisor is ingericht en waarmee u gegevens naar Azure kunt verzenden.
    
- **Data Box Gateway-resource** - Een resource in de Azure Portal waarmee u een Data Box Gateway-apparaat kunt beheren via een webinterface waartoe u toegang hebt vanaf verschillende geografische locaties. Gebruik de gegevens in het Gateway-resource op het apparaat, shares, gebruikers en waarschuwingen weergeven en beheren. Zie voor meer informatie over het [beheren met behulp van Azure portal](data-box-gateway-manage-shares.md).

- **Data Box lokale webinterface** - diagnose uitvoeren, afsluiten en opnieuw opstarten van het apparaat, een ondersteuningspakket genereren met de lokale webgebruikersinterface of neem contact op met Microsoft Support om een serviceaanvraag. Zie voor meer informatie over het [beheren met behulp van de lokale webgebruikersinterface](data-box-gateway-manage-access-power-connectivity-mode.md).

## <a name="region-availability"></a>Beschikbaarheid in regio’s

De Gateway in het fysieke apparaat, Azure-resource en doelopslagaccount waarnaar u gegevens worden overgedragen hoeft niet alle te worden in dezelfde regio.

- **De beschikbaarheid van resources** -voor deze release de gegevens in het Gateway-resource beschikbaar is in de volgende regio's die ondersteuning bieden voor openbare cloud:
    - **Verenigde Staten** -VS-Oost
    - **Europese Unie** - West-Europa
    - **Azië en Stille Oceaan** -Zuidoost-Azië

    Data Box-Gateway kan ook worden geïmplementeerd in de Azure Government-Cloud. Zie voor meer informatie, [wat is Azure Government?](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome).

- **Doelopslagaccounts**: De opslagaccounts waarin de gegevens worden opgeslagen, zijn beschikbaar in alle Azure-regio’s.

    De regio’s waar de opslagaccounts Data Box-gegevens opslaan, moeten zich voor optimale prestaties dicht bij het apparaat bevinden. Een opslagaccount dat zich ver van het apparaat vandaan bevindt, resulteert in lange latenties en tragere prestaties.


## <a name="next-steps"></a>Volgende stappen

- Lees de [Systeemvereisten voor Data Box Gateway](data-box-gateway-system-requirements.md).
- Begrijp de [limieten voor Data Box Gateway](data-box-gateway-limits.md).
- Implementeer [Azure Data Box Gateway](data-box-gateway-deploy-prep.md) in de Azure Portal.

