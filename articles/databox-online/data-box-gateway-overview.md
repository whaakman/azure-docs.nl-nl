---
title: Overzicht van Microsoft Azure Data Box Gateway | Microsoft Docs
description: Beschrijft Azure Data Box Gateway, een opslagoplossing voor virtuele apparaten waarmee u gegevens naar Azure kunt overdragen
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: overview
ms.custom: ''
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 9c12674a66582ede04b4cf9d311238d61816afec
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46969543"
---
# <a name="what-is-azure-data-box-gateway-preview"></a>Wat is Azure Data Box Gateway (preview-versie)? 

Azure Data Box Gateway is een opslagoplossing waarmee u naadloos gegevens naar Azure kunt verzenden. Dit artikel biedt een overzicht van de Azure Data Box Gateway-oplossing, voordelen, belangrijkste mogelijkheden en de scenario’s waarin u dit apparaat kunt implementeren. 

Data Box Gateway is een virtueel apparaat op basis van een virtuele machine die in uw gevirtualiseerde omgeving of hypervisor is ingericht. Het virtuele apparaat bevindt zich op uw locatie en u schrijft gegevens ernaar met behulp van het NFS- en SMB-protocol. Het apparaat draagt uw gegevens vervolgens over naar Azure blok-blob, pagina-blob of Azure Files. 

> [!IMPORTANT]
> Data Box Gateway is in de previewfase. Lees de [gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing implementeert.

## <a name="use-cases"></a>Gebruiksvoorbeelden

Data Box Gateway kan worden gebruikt voor het overdragen van gegevens naar de cloud, zoals voor archivering in de cloud, herstel na noodgevallen of als uw gegevens in de cloud moeten worden verwerkt. Hier volgen de verschillende scenario’s waarin Data Box Gateway kan worden gebruikt voor gegevensoverdracht.

- **Archivering in de cloud** - Kopieer honderden TB’s aan gegevens veilig en efficiënt naar Azure-opslag met behulp van Data Box Gateway. De gegevens kunnen in één keer of voortdurend worden opgenomen voor archiveringsdoeleinden.

- **Gegevensverzameling** - Verzamel gegevens uit meerdere bronnen op één locatie in Azure Storage voor gegevensverwerking en -analyse.  

- **Integratie met on-premises werkbelastingen** - Integreer met lokale werkbelastingen, zoals back-ups en hersteltaken die gebruikmaken van cloudopslag en lokale toegang nodig hebben voor veelgebruikte bestanden. 

## <a name="benefits"></a>Voordelen

Data Box Gateway biedt de volgende voordelen:

- **Gemakkelijke gegevensoverdracht** - Het verplaatsen van gegevens in en uit Azure-opslag is net zo gemakkelijk als het werken met een lokale netwerkshare.  
- **Hoge prestaties** - Maakt overdracht van netwerkgegevens minder complex met snelle gegevensoverdrachten naar en vanuit Azure. 
- **Snelle toegang**: De recentste bestanden worden in de cache opgeslagen voor snelle toegang tot on-premises bestanden.  
- **Beperkt bandbreedtegebruik**: Gegevens kunnen zelfs naar Azure worden geschreven wanneer het netwerk wordt beperkt voor minder gebruik tijdens piekuren.  

## <a name="key-capabilities"></a>Belangrijkste mogelijkheden

Data Box Gateway biedt de volgende mogelijkheden:

|Mogelijkheid |Beschrijving  |
|---------|---------|
|Snelheid     | Volledig geautomatiseerde en zeer geoptimaliseerde gegevensoverdracht en bandbreedte.|
|Ondersteunde protocollen     | Ondersteuning voor het standaard SMB- en NFS-protocol voor gegevensopname. <br> Ga naar [Systeemvereisten voor Data Box Gateway](data-box-gateway-system-requirements.md) voor meer informatie over ondersteunde versies.|
|Toegang tot gegevens     | Rechtstreekse gegevenstoegang vanuit Azure Storage Blobs en Azure Files met behulp van cloud-API’s voor aanvullende gegevensverwerking in de cloud.|
|Snelle toegang     | Lokale cache op het apparaat voor snelle toegang tot laatst gebruikte bestanden.|
|Offline upload     | Modus zonder verbinding ondersteunt scenario’s voor offline uploaden.|
|Gegevensvernieuwing     | Mogelijkheid om lokale bestanden te vernieuwen met de meest recente uit de cloud.|
|Versleuteling    | BitLocker-ondersteuning om gegevens lokaal te versleutelen en de gegevensoverdracht naar de cloud via *HTTPS* te beveiligen       |
|Flexibiliteit     | Ingebouwde netwerkflexibiliteit        |


## <a name="specifications"></a>Specificaties

Het virtuele Data Box Gateway-apparaat heeft de volgende specificaties:

| Specificaties                                          | Beschrijving              |
|---------------------------------------------------------|--------------------------|
| Virtuele processors (kernen)   | Minimaal 4 |            
| Geheugen  | Minimaal 8 GB|
| Beschikbaarheid|Eén knooppunt|
| Disks| Besturingssysteemschijf: 250 GB <br> Gegevensschijf: minimaal 2 TB, thin-provisioned en moet worden ondersteund door SSD's|
| Netwerkinterfaces|1 of meer virtuele netwerkinterfaces|
| Systeemeigen protocollen voor het delen van bestanden|SMB en NFS  |
| Beveiliging| Verificatie voor het ontgrendelen van toegang tot apparaten en gegevens <br> Gegevens worden tijdens overdracht versleuteld met AES-256-bits-versleuteling|
| Beheer| Lokale webgebruikersinterface - Eerste installatie, diagnostische gegevens en energiebeheer van apparaat <br> Azure Portal - Dagelijks beheer van Data Box Gateway-apparaten       |


## <a name="components"></a>Onderdelen

De Data Box Gateway-oplossing bestaat uit een Data Box Gateway-resource, een virtueel Data Box Gateway-apparaat en een lokale webinterface.

* **Virtueel Data Box Gateway-apparaat** - Een apparaat op basis van een virtuele machine die in uw gevirtualiseerde omgeving of hypervisor is ingericht en waarmee u gegevens naar Azure kunt verzenden. 
    
* **Data Box Gateway-resource** - Een resource in de Azure Portal waarmee u een Data Box Gateway-apparaat kunt beheren via een webinterface waartoe u toegang hebt vanaf verschillende geografische locaties. Gebruik de Data Box Gateway-resource om resources te maken en beheren, apparaten en waarschuwingen te bekijken en beheren, en shares te beheren.  

    <!--![The Data Box Gateway service in Azure portal](media/data-box-overview/data-box-Gateway-service1.png)-->

    <!--For more information, go to [Use the Data Box Gateway service to administer your Data Box Gateway device](data-box-gateway-portal-ui-admin.md).-->

* **Lokale Data Box-webinterface**: Gebruik de lokale webinterface om diagnoses uit te voeren, het Data Box Gateway-apparaat uit te schakelen of opnieuw op te starten, logboeken met kopieerbewerkingen te bekijken en contact op te nemen met Microsoft Ondersteuning om een serviceaanvraag in te dienen.

    <!--![The Data Box Gateway local web UI](media/data-box-gateway-overview/data-box-gateway-local-web-ui.png)-->

    <!-- For information about using the web-based UI, go to [Use the web-based UI to administer your Data Box](data-box-gateway-portal-ui-admin.md).-->


## <a name="region-availability"></a>Beschikbaarheid in regio’s

Het fysieke Data Box Edge-apparaat, de Azure-resource en het doelopslagaccount waarnaar u gegevens overdraagt hoeven zich niet allemaal in dezelfde regio te bevinden.

- **Beschikbaarheid van resources**: Voor deze release is de Data Box Edge-resource beschikbaar in de volgende regio’s:
    - **Verenigde Staten** - West 2 en Oost
    - **Europese Unie** - West-Europa
    - **Azië en Stille Oceaan** - Zuidoost-Azië

- **Doelopslagaccounts**: De opslagaccounts waarin de gegevens worden opgeslagen, zijn beschikbaar in alle Azure-regio’s. 

    De regio’s waar de opslagaccounts Data Box-gegevens opslaan, moeten zich voor optimale prestaties dicht bij het apparaat bevinden. Een opslagaccount dat zich ver van het apparaat vandaan bevindt, resulteert in lange latenties en tragere prestaties. 


## <a name="next-steps"></a>Volgende stappen

- Lees de [Systeemvereisten voor Data Box Gateway](data-box-gateway-system-requirements.md).
- Begrijp de [limieten voor Data Box Gateway](data-box-gateway-limits.md).
- Implementeer [Azure Data Box Gateway](data-box-gateway-deploy-prep.md) in de Azure Portal.




