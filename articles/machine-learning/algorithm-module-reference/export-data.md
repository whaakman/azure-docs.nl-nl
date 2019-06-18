---
title: 'Gegevens exporteren: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de module gegevens exporteren in Azure Machine Learning-service op te slaan van resultaten, tussenliggende gegevens en werken vanuit uw experimenten in de cloud opslagdoelen buiten Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: c3744803f172edf9fbf2556a12677e8faef370c2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65028318"
---
# <a name="export-data-module"></a>De module gegevens exporteren

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Deze module gebruiken voor het opslaan van resultaten, tussenliggende gegevens en werken vanuit uw experimenten in de cloud opslagdoelen buiten Azure Machine Learning.

Deze module biedt ondersteuning voor exporteren of uw gegevens opslaan in de volgende cloudservices voor gegevens:


- **Exporteren naar Azure Blob-opslag**: Slaat gegevens op in de Blob-service in Azure. Gegevens in de Blob-service kan worden gedeeld openbaar of opgeslagen in de gegevensopslag van beveiligde toepassing.

  
## <a name="how-to-configure-export-data"></a>Het configureren van de gegevens exporteren

1. Voeg de **gegevens exporteren** module aan uw experiment in de interface. U vindt deze module in de **invoer en uitvoer** categorie.

2. Verbinding maken met **gegevens exporteren** naar de module met de gegevens die u wilt exporteren.

3. Dubbelklik op **gegevens exporteren** openen de **eigenschappen** deelvenster.

4. Voor **bestemming**, selecteer het type opslag in de cloud waar u uw gegevens kunt opslaan. Als u wijzigingen aanbrengt aan deze optie, worden alle andere eigenschappen worden opnieuw ingesteld. Dus zorg ervoor dat deze optie eerst kiezen.

5. Geef een account-servernaam en verificatiegegevens methode zijn vereist voor toegang tot het opgegeven opslagaccount.

    **Exporteren naar Azure Blob Storage** is de enige optie in de beperkte Preview-versie. Hieronder ziet u hoe het instellen van de module.
    1. De Azure blob-service is voor het opslaan van grote hoeveelheden gegevens, inclusief binaire gegevens. Er zijn twee soorten blob-opslag: blobs waarvoor aanmeldingsreferenties en openbare blobs.

    2. Voor **verificatietype**, kiest u **openbare (SAS)** als u weet dat de opslag biedt ondersteuning voor toegang via een SAS-URL.

          Een SAS-URL is een speciaal type URL die kan worden gegenereerd met behulp van een hulpprogramma voor Azure storage en is beschikbaar voor slechts een beperkte periode.  Deze bevat alle informatie die nodig voor verificatie en downloaden is.

        Voor **URI**typt of plakt u de volledige URI die het account en de openbare blob worden gedefinieerd.

        Voor de bestandsindeling van worden CSV en TSV ondersteund.

    3. Kies voor persoonlijke accounts **Account**, en geef de accountnaam en -sleutel, zodat het experiment naar het opslagaccount schrijven kunt.

         - **Accountnaam**: Typ of plak de naam van het account waar u de gegevens op te slaan. Bijvoorbeeld, als de volledige URL van het opslagaccount dat is `http://myshared.blob.core.windows.net`, typt u `myshared`.

        - **Accountsleutel**: Plak de toegangssleutel voor opslag die is gekoppeld aan het account.

        -  **Pad naar de container, map of blob**: Typ de naam van de blob waarin de geëxporteerde gegevens worden opgeslagen. Bijvoorbeeld, om op te slaan van de resultaten van uw experiment naar een nieuwe blob met de naam **results01.csv** in de container **voorspellingen** in een account met de naam **mymldata**, de volledige URL voor de BLOB zou worden `http://mymldata.blob.core.windows.net/predictions/results01.csv`.

            Daarom in het veld **pad naar de container, map of blob**, geeft u de container en de naam van de blob als volgt: `predictions/results01.csv`

        - Als u de naam van een blob die nog niet bestaat, wordt de blob in Azure gemaakt voor u.

       -  Bij het schrijven naar een bestaande blob, kunt u opgeven dat de huidige inhoud van de blob worden overschreven door de eigenschap **Azure-blobopslag schrijven modus**. Deze eigenschap is standaard ingesteld op **fout**, wat betekent dat er een foutmelding wordt gegenereerd wanneer er wordt een bestaand blobbestand met dezelfde naam gevonden.


    4. Voor **bestandsindeling voor blob-bestand**, selecteert u de indeling waarin de gegevens moeten worden opgeslagen.

        - **CSV**: Door komma's gescheiden waarden (CSV) zijn de standaardindeling voor opslag. Als u wilt exporteren kolomkoppen samen met de gegevens, selecteert u de optie **schrijven blob veldnamenrij**.  Zie voor meer informatie over de door komma's gescheiden indeling die wordt gebruikt in Azure Machine Learning, [converteren naar CSV](./convert-to-csv.md).

        - **TSV**: Door tabs gescheiden waarden (TSV)-indeling is compatibel met veel machine learning-hulpprogramma's. Als u wilt exporteren kolomkoppen samen met de gegevens, selecteert u de optie **schrijven blob veldnamenrij**.  

 
    5. **Resultaten in de cache gebruiken**: Selecteer deze optie als u wilt voorkomen dat de resultaten naar de blobbestand voor het herschrijven telkens wanneer die u het experiment uitvoert. Als er geen andere wijzigingen aangebracht in de moduleparameters, het experiment schrijft de resultaten alleen de eerste keer dat de module wordt uitgevoerd, of wanneer er wijzigingen in de gegevens.

    6. Voer het experiment uit.

## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 