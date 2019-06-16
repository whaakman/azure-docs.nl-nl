---
title: 'Lineaire regressie: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de lineaire regressie-module in Azure Machine Learning-service te maken van een lineair regressiemodel voor gebruik in een experiment.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 040f726a703d34a95bae7d5b7cdd766655c62a4e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65029713"
---
# <a name="linear-regression-module"></a>Lineaire regressie-module
Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module om een lineair regressiemodel voor gebruik in een experiment maken.  Lineaire regressie probeert een lineaire relatie tussen een of meer onafhankelijke variabelen en een numerieke resultaat of afhankelijke variabele tot stand brengen. 

U gebruik deze module om een lineaire regressiemethode definiëren en vervolgens een model met behulp van een gegevensset met gelabelde trainen. Het getrainde model kan vervolgens worden gebruikt om voorspellingen te doen.

## <a name="about-linear-regression"></a>Over lineaire regressie

Lineaire regressie is een algemene statistische methode, die is vastgesteld in machine learning en uitgebreid met veel nieuwe methoden voor het aanpassen van de regel en meten van de fout. In de meest eenvoudige zin verwijst regressie naar voorspelling van een numerieke doel. Lineaire regressie is nog steeds een goede keuze als u een eenvoudig model voor een eenvoudige predictive taak. Lineaire regressie doorgaans ook goed werkt voor high-dimensionale, sparse gegevenssets complexiteit ontbreekt.

Azure Machine Learning ondersteunt een groot aantal regressiemodellen, naast de lineaire regressie. Echter, de term 'regression' losjes kan worden geïnterpreteerd en bepaalde typen regressie opgegeven in andere hulpprogramma's worden niet ondersteund.

+ De klassieke regressieprobleem omvat een enkel onafhankelijk en een variabele afhankelijk zijn. Dit heet *eenvoudige regressie*.  Deze module biedt ondersteuning voor eenvoudige regressie.

+ *Meerdere lineaire regressie* bestaat uit twee of meer onafhankelijke variabelen die aan één afhankelijke variabele bijdragen. Problemen met waarin meerdere invoergegevens worden gebruikt om te voorspellen van een enkele numerieke uitkomst worden ook wel genoemd *multidimensionale lineaire regressie*.

    De **lineaire regressie** module deze problemen kunt oplossen, kunt u de meeste van de andere regressie-modules.

+ *Meerdere label regressie* is de taak van het voorspellen van meerdere afhankelijke variabelen binnen één model. In meerdere label logistieke regressie, kan bijvoorbeeld een voorbeeld van een worden toegewezen aan meerdere verschillende labels. (Dit verschilt van de taak van het voorspellen van meerdere niveaus in een variabele één klasse.)

    Dit type regressie wordt niet ondersteund in Azure Machine Learning. Om te voorspellen meerdere variabelen, maakt u een afzonderlijke cursist voor elke uitvoer die u wilt voorspellen.

Jarenlang hebben statistici steeds meer geavanceerde methoden voor het regressie is ontwikkelen. Dit geldt ook voor lineaire regressie. Deze module biedt ondersteuning voor twee methoden voor het meten van de fout en past de regel regressie: methode gewone kleinste kwadraten en leren met stochastische gradiëntdaling.

- **Leren met stochastische gradiëntdaling** is een methode die zo weinig mogelijk fout bij elke stap van het model training-proces. Er zijn veel variaties op leren met stochastische gradiëntdaling en de optimalisatie voor verschillende learning problemen uitgebreid zijn bestudeerd. Als u deze optie voor **oplossing methode**, u kunt verschillende parameters voor het beheren van de Stapgrootte learning snelheid, en enzovoorts bedekt. Deze optie ondersteunt ook gebruik van een geïntegreerde parameteropschoning.

- **Gewone kleinste kwadraten** is een van de meest gebruikte technieken in lineaire regressie. Bijvoorbeeld, is de kleinste kwadraten de methode die wordt gebruikt in de invoegtoepassing voor Microsoft Excel.

    Gewone kleinste kwadraten verwijst naar de functie verlies, die past het model door het minimaliseren van de kwadraten fout en fout als de som van het kwadraat van de afstand tussen de werkelijke waarde en de voorspelde regel berekent. Deze methode wordt ervan uitgegaan dat een sterke lineaire relatie tussen de invoer en de afhankelijke variabele.

## <a name="configure-linear-regression"></a>Lineaire regressie configureren

Deze module ondersteunt twee methoden voor het aanpassen van een regressiemodel met verschillende opties:

+ [Een regressiemodel met behulp van online leren met stochastische gradiëntdaling maken](#bkmk_GradientDescent)

    Leren met stochastische gradiëntdaling is een betere verlies-functie voor modellen die zijn meer complexe, of die te weinig trainingsgegevens gezien het aantal variabelen.



+ [Past een regressiemodel met behulp van gewone kleinste kwadraten](#bkmk_OrdinaryLeastSquares)

    Voor kleine gegevenssets is het raadzaam om te selecteren van gewone kleinste kwadraten. Dit geeft vergelijkbare resultaten naar Excel.

## <a name="bkmk_OrdinaryLeastSquares"></a> Een regressiemodel met behulp van gewone kleinste kwadraten maken

1. Voeg de **lineair regressiemodel** module aan uw experiment in de interface.

    U vindt deze module in de **Machine Learning** categorie. Vouw **Initialize Model**, vouw **regressie**, en sleep de **lineair regressiemodel** module naar het experimentcanvas.

2. In de **eigenschappen** deelvenster in de **oplossing methode** vervolgkeuzelijst **gewone kleinste kwadraten**. Deze optie geeft u de berekeningsmethode gebruikt om te vinden van de regressie-regel.

3. In **L2 regularisatie gewicht**, typt u de waarde moet worden gebruikt als het gewicht voor L2 regularisatie. U wordt aangeraden dat u een andere waarde dan nul om te voorkomen dat te gebruiken.

     Raadpleeg dit artikel voor meer informatie over de invloed van regularisatie op model aanpassing van labels: [L1 en L2 Regularisatiegraad voor Machine Learning](https://msdn.microsoft.com/magazine/dn904675.aspx)

4. Selecteer de optie **opnemen intercept term**, als u wilt weergeven van de term voor het snijpunt.

    Schakel deze optie als u niet nodig hebt om te controleren van de regressieformule.

5. Voor **willekeurig getal seed**, u kunt desgewenst een waarde op voor het seeden van de generator van willekeurige getallen die worden gebruikt door het model typen.

    Met behulp van een seed-waarde is handig als u wilt behouden dezelfde resultaten voor verschillende runs van het hetzelfde experiment. Anders is de standaardwaarde een waarde van de systeemklok te gebruiken.


7. Voeg de [Train Model](./train-model.md) module naar het experimentcanvas en verbinding maken met een gegevensset met gelabelde.

8. Voer het experiment uit.

## <a name="results-for-ordinary-least-squares-model"></a>Resultaten voor normale kleinste kwadraten model

Na de training is voltooid:

+ Als u de parameters van het model, met de rechtermuisknop op de uitvoer trainer en selecteer **Visualize**.

+ Om voorspellingen te maken, verbinding maken met het getrainde model op de [Score Model](./score-model.md) -module, samen met een gegevensset met de nieuwe waarden. 


## <a name="bkmk_GradientDescent"></a> Een regressiemodel met behulp van online leren met stochastische gradiëntdaling maken

1. Voeg de **lineair regressiemodel** module aan uw experiment in de interface.

    U vindt deze module in de **Machine Learning** categorie. Vouw **Initialize Model**, vouw **regressie**, en sleep de **lineair regressiemodel** module naar het experimentcanvas

2. In de **eigenschappen** deelvenster in de **oplossing methode** vervolgkeuzelijst Kies **Online leren met stochastische Gradiëntdaling** als de berekeningsmethode gebruikt om te vinden van de regressie-regel.

3. Voor **trainer aanmaakmodus**, aangeven of u wilt het model met een vooraf gedefinieerde set parameters te trainen of als u wilt het model met behulp van een parameteropschoning optimaliseren.

    + **Eén Parameter**: Als u hoe u wilt dat het netwerk lineaire regressie te configureren weet, kunt u een specifieke set waarden opgeven als argumenten.

   
4. Voor **Learning tarief**, de initiële leertempo opgeven voor de functie stochastische leren met stochastische gradiëntdaling te optimaliseren.

5. Voor **aantal training epoches**, typ een waarde die aangeeft hoe vaak het algoritme moet doorlopen van de voorbeelden. Dit aantal moet altijd grote bijeenkomst bereiken voor gegevenssets met een klein aantal voorbeelden.

6. **Functies normaliseren**: Als u hebt al de numerieke gegevens gebruikt voor het model te trainen genormaliseerd, kunt u deze optie uitschakelen. De module normaliseert standaard alle numerieke invoer voor een bereik tussen 0 en 1.

    > [!NOTE]
    > 
    > Vergeet niet om dezelfde methode normalisering van toepassing op nieuwe gegevens die worden gebruikt voor het scoren.

7. In **L2 regularisatie gewicht**, typt u de waarde moet worden gebruikt als het gewicht voor L2 regularisatie. U wordt aangeraden dat u een andere waarde dan nul om te voorkomen dat te gebruiken.

    Raadpleeg dit artikel voor meer informatie over de invloed van regularisatie op model aanpassing van labels: [L1 en L2 Regularisatiegraad voor Machine Learning](https://msdn.microsoft.com/magazine/dn904675.aspx)


9. Selecteer de optie **afname leertempo**, als u wilt dat het leertempo te verlagen als de herhalingen wordt uitgevoerd.  

10. Voor **willekeurig getal seed**, u kunt desgewenst een waarde op voor het seeden van de generator van willekeurige getallen die worden gebruikt door het model typen. Met behulp van een seed-waarde is handig als u wilt behouden dezelfde resultaten voor verschillende runs van het hetzelfde experiment.


12. Een gegevensset met gelabelde en een van de trainingsmodules toevoegen.

    Als u geen van een parameteropschoning gebruikmaakt, gebruikt u de [Train Model](train-model.md) module.

13. Voer het experiment uit.

## <a name="results-for-online-gradient-descent"></a>Resultaten voor online leren met stochastische gradiëntdaling

Na de training is voltooid:

+ Om voorspellingen te maken, verbinding maken met het getrainde model op de [Score Model](./score-model.md) -module, samen met de nieuwe invoergegevens.


## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 