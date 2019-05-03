---
title: 'Dubbele rijen verwijderen: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de module dubbele rijen verwijderen in Azure Machine Learning-service op mogelijke dubbele waarden verwijderen uit een gegevensset.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: dce90d911085c1f7330a2e0952bb9576c1d765fa
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029293"
---
# <a name="remove-duplicate-rows-module"></a>Dubbele rijen module verwijderen

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module om mogelijke dubbele waarden verwijderen uit een gegevensset.

Bijvoorbeeld, wordt ervan uitgegaan dat uw gegevens uitziet en meerdere records voor patiënten vertegenwoordigt. 

| PatientID | Intialen| Geslacht|Leeftijd|Toegelaten|
|----|----|----|----|----|
|1|F.M.| M| 53| jan|
|2| F.A.M.| M| 53| jan|
|3| F.A.M.| M| 24| jan|
|3| F.M.| M| 24| feb|
|4| F.M.| M| 23| feb|
| | F.M.| M| 23| |
|5| F.A.M.| M| 53| |
|6| F.A.M.| M| NaN| |
|7| F.A.M.| M| NaN| |

In dit voorbeeld is duidelijk, meerdere kolommen met mogelijk dubbele gegevens. Of ze daadwerkelijk dubbele waarden zijn, is afhankelijk van uw kennis van de gegevens. 

+ Bijvoorbeeld, kent u mogelijk dat veel patiënten dezelfde naam hebben. U wouldn't voorkomen dat dubbele waarden met behulp van een naamkolom, alleen de **ID** kolom. Op die manier kunnen alleen de rijen met dubbele id-waarden eruit worden gefilterd, ongeacht of de patiënt dezelfde naam hebben.

+ U kunt ook besluiten u dubbele vermeldingen in het veld ID toestaat en een andere combinatie van bestanden gebruiken om unieke records, zoals de naam van de eerste, laatste naam, leeftijd en geslacht te zoeken.  

Om in te stellen de criteria voor of een rij dubbel of niet is, geeft u één kolom of een set kolommen die worden gebruikt als **sleutels**. Twee rijen worden beschouwd als dubbele records alleen wanneer de waarden in **alle** sleutelkolommen gelijk zijn. Als een rij heeft een ontbrekende waarde voor **sleutels**, ze komen niet in aanmerking dubbele rijen. Bijvoorbeeld, als geslacht en leeftijd zijn ingesteld als de sleutels in bovenstaande tabel rij 6 en 7 zijn niet dubbele rijen gegeven hebben ze ontbrekende waarde in leeftijd.

Wanneer u de module uitvoert, maakt u een gegevensset candidate en retourneert een set van rijen die u hebt geen dubbele waarden in de set kolommen die u hebt opgegeven.

> [!IMPORTANT]
> De brongegevensset is niet gewijzigd; Deze module maakt een nieuwe gegevensset die is gefilterd, als u wilt uitsluiten van duplicaten, op basis van de criteria die u opgeeft.

## <a name="how-to-use-remove-duplicate-rows"></a>Het gebruik van dubbele rijen verwijderen

1. De module toevoegen aan uw experiment. U vindt de **dubbele rijen verwijderen** module onder **gegevenstransformatie**, **manipulatie**.  

2. Verbinding maken met de gegevensset die u wilt controleren op dubbele rijen.

3. In de **eigenschappen** deelvenster onder **sleutel kolom selectie filterexpressie**, klikt u op **kolomkiezer lancering**, kiest u kolommen die worden gebruikt bij het identificeren van duplicaten.

    In deze context **sleutel** betekent niet dat een unieke id. Alle kolommen die u hebt geselecteerd met behulp van de kolomkiezer zijn aangewezen als **sleutelkolommen**. Alle niet-geselecteerde kolommen worden beschouwd als niet-sleutelkolommen. De combinatie van kolommen die u als sleutels selecteert bepaalt de uniekheid van de records. (Het zien als een SQL-instructie die gebruikmaakt van meerdere equalities joins.)

    Voorbeelden:

    + 'Ik wil ervoor zorgen dat de id's uniek zijn': Kies alleen de kolom-ID.
    + 'Ik wil ervoor zorgen dat de combinatie van de voornaam, achternaam en ID uniek is': Selecteer alle drie kolommen.

4. Gebruik de **behouden van de eerste dubbele rij** selectievakje om aan te geven welke rij moet worden geretourneerd wanneer er dubbele waarden zijn gevonden:

    + Indien geselecteerd, de eerste rij wordt geretourneerd en anderen verwijderd. 
    + Als u deze optie uitschakelt, wordt de laatste rij van de dubbele wordt opgeslagen in de resultaten en anderen worden genegeerd. 

5. Voer het experiment uit.

6. Om de resultaten bekijken, met de rechtermuisknop op de module, selecteert u **resultaten gegevensset**, en klikt u op **Visualize**. 

> [!TIP]
> Als de resultaten moeilijk zijn te begrijpen of als u uitsluiten van sommige kolommen in aanmerking wilt, u kolommen verwijderen met behulp van kunt de [Select Columns in Dataset](./select-columns-in-dataset.md) module.

## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 