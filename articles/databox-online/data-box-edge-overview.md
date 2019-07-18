---
title: Overzicht van Microsoft Azure Data Box Edge | Microsoft Docs
description: Beschrijft Azure Data Box Edge, een opslag oplossing die gebruikmaakt van een fysiek apparaat voor overdracht op basis van een netwerk naar Azure.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 07/17/2019
ms.author: alkohli
ms.openlocfilehash: 69580f956b603423ef302353953a45ad5d00391e
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68305422"
---
# <a name="what-is-azure-data-box-edge"></a>Wat is Azure Data Box Edge? 

Azure Data Box Edge is een Edge-rekenapparaat met AI dat mogelijkheden voor netwerkgebaseerde gegevensoverdracht biedt. In dit artikel staat een overzicht van de Data Box Edge-oplossing, voordelen, belangrijkste mogelijkheden en de scenario’s waarin u dit apparaat kunt implementeren. 

Data Box Edge is een hardware-as-a-service-oplossing. Micro soft stuurt u een door een Cloud beheerd apparaat met een ingebouwd veld Programmeer bare Gate-matrix (FPGA) waarmee de AI-interferentie wordt versneld en alle mogelijkheden van een opslag gateway. 

## <a name="use-cases"></a>Gebruiksvoorbeelden

Hier volgen de verschillende scenario's waarin Data Box Edge kunnen worden gebruikt voor snelle Machine Learning (ML) bij de rand en de voor verwerking van gegevens voordat deze naar Azure worden verzonden.

- Ingrijpen **met Azure machine learning** -met data Box Edge kunt u ml-modellen uitvoeren om snelle resultaten te krijgen waarmee kan worden gereageerd voordat de gegevens naar de cloud worden verzonden. De volledige gegevensset kan eventueel worden overgedragen om uw ML-modellen verder te trainen en te verbeteren. Zie voor meer informatie over het gebruik van de met Azure ML versnelde modellen op het Data Box Edge-apparaat [Azure ml-versnelde modellen implementeren op Data Box Edge](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-fpga-web-service#deploy-to-a-local-edge-server).

- **Gegevens vooraf verwerken** : transformatie gegevens voordat deze naar Azure worden verzonden om een meer actie bare gegevensset te maken. Voorverwerking kan worden gebruikt om: 

    - Gegevens samen te voegen.
    - Wijzig gegevens, bijvoorbeeld om persoonlijke gegevens te verwijderen.
    - Subset van gegevens voor het optimaliseren van opslag en band breedte, of voor verdere analyse.
    - IoT-gebeurtenissen te analyseren en erop te reageren. 

- **Gegevens via een netwerk naar Azure overdragen**: Gebruik Data Box Edge om gegevens gemakkelijk en snel naar Azure over te dragen voor verdere berekeningen en analyses of voor archivering. 


## <a name="key-capabilities"></a>Belangrijkste mogelijkheden

Data Box Edge biedt de volgende mogelijkheden:

|Mogelijkheid |Description  |
|---------|---------|
|Versneld AI-interferentie| Ingeschakeld door de ingebouwde FPGA.|
|Berekenen       |Gegevens kunnen worden geanalyseerd, verwerkt of gefilterd.|
|Hoge prestaties | High Performance Compute en gegevens overdracht.|
|Toegang tot gegevens     | Rechtstreekse gegevenstoegang vanuit Azure Storage Blobs en Azure Files met behulp van cloud-API’s voor aanvullende gegevensverwerking in de cloud. Lokale cache op het apparaat wordt gebruikt voor snelle toegang tot de meest recent gebruikte bestanden.|
|Door de Cloud beheerde     |Het apparaat en de service worden beheerd via de Azure Portal.  |
|Offline upload     | Modus zonder verbinding ondersteunt scenario’s voor offline uploaden.|
|Ondersteunde protocollen     | Ondersteuning voor het standaard SMB- en NFS-protocol voor gegevensopname. <br> Ga naar [Systeemvereisten voor Data Box Edge](data-box-edge-system-requirements.md) voor meer informatie over ondersteunde versies.|
|Gegevensvernieuwing     | Mogelijkheid om lokale bestanden te vernieuwen met de meest recente uit de cloud.|
|Versleuteling    | BitLocker-ondersteuning om gegevens lokaal te versleutelen en de gegevensoverdracht naar de cloud via *HTTPS* te beveiligen.|
|Bandbreedte regeling| Beperking voor het beperken van het bandbreedte gebruik tijdens piek uren.|


## <a name="components"></a>Onderdelen

De Data Box Edge-oplossing bestaat uit een Data Box Edge-resource, een fysiek Data Box Edge-apparaat en een lokale webinterface.

* **Fysiek Data Box Edge-apparaat**: Een 1U rackserver die door Microsoft wordt geleverd en die kan worden geconfigureerd om gegevens naar Azure te verzenden. 
    
* **Data Box Edge-resource**: Een resource in de Azure-portal waarmee u een Data Box Edge-apparaat kunt beheren via een webinterface waartoe u toegang hebt vanaf verschillende geografische locaties. Gebruik de Data Box Edge-resource om resources te maken en beheren, apparaten en waarschuwingen te bekijken en beheren, en shares te beheren.  

    <!--![The Data Box Edge service in Azure portal](media/data-box-overview/data-box-Edge-service1.png)-->

    Ga voor meer informatie naar [een bestelling maken voor uw data Box edge-apparaat](data-box-edge-deploy-prep.md#create-a-new-resource).

* **Lokale Data Box-webinterface**: Gebruik de lokale webinterface om diagnoses uit te voeren, het Data Box Edge-apparaat uit te schakelen of opnieuw op te starten, logboeken met kopieerbewerkingen te bekijken en contact op te nemen met Microsoft Ondersteuning om een serviceaanvraag in te dienen.

    <!--![The Data Box Edge local web UI](media/data-box-Edge-overview/data-box-Edge-local-web-ui.png)-->

    Ga naar [De webgebaseerde gebruikersinterface gebruiken om uw Data Box te beheren](data-box-edge-manage-access-power-connectivity-mode.md) voor informatie over het gebruik van de webgebaseerde gebruikersinterface.


## <a name="region-availability"></a>Beschikbaarheid in regio’s

Het fysieke Data Box Edge-apparaat, de Azure-resource en het doelopslagaccount waarnaar u gegevens overdraagt hoeven zich niet allemaal in dezelfde regio te bevinden.

- **Beschik baarheid van resources** : als u een lijst wilt weer geven met alle regio's waar de data Box Edge resource beschikbaar is, gaat u naar [Azure-producten die beschikbaar zijn per regio](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all). Data Box Edge kan ook in de Azure Government Cloud worden geïmplementeerd. Zie [Wat is Azure Government?](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome)voor meer informatie.
    
- **Doelopslagaccounts**: De opslagaccounts waarin de gegevens worden opgeslagen, zijn beschikbaar in alle Azure-regio’s. De regio's waar het opslag accounts archief Data Box Edge gegevens moeten zich bevinden dicht bij de locatie waar het apparaat zich bevindt voor optimale prestaties. Een opslagaccount dat zich ver van het apparaat vandaan bevindt, resulteert in lange latenties en tragere prestaties. 


## <a name="next-steps"></a>Volgende stappen

- De [Systeemvereisten voor Data Box Edge](data-box-edge-system-requirements.md) lezen.
- De [limieten voor Data Box Edge](data-box-edge-limits.md) begrijpen.
- [Azure Data Box Edge](data-box-edge-deploy-prep.md) in de Azure-portal implementeren.




