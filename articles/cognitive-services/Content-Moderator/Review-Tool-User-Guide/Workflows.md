---
title: Definiëren en gebruiken van inhoud werkstromen via het beoordelingsprogramma - Content Moderator
titlesuffix: Azure Cognitive Services
description: U kunt het Azure Content Moderator workflow designer gebruiken om aangepaste werkstromen en drempelwaarden op basis van uw inhoud beleidsregels te definiëren.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/14/2019
ms.author: sajagtap
ms.openlocfilehash: 76990fb3b6ed1815ada724f28f8276bac1cf28d4
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58757402"
---
# <a name="define-and-use-moderation-workflows"></a>Definiëren en toezicht werkstromen gebruiken

In deze handleiding leert u hoe u kunt instellen en gebruiken [werkstromen](../review-api.md#workflows) op de [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com) website. Werkstromen zijn cloud-gebaseerde aangepaste filters die u gebruiken kunt voor het afhandelen van inhoud efficiënter. Werkstromen kunnen verbinding maken met tal van services voor het filteren van inhoud op verschillende manieren en vervolgens de juiste actie ondernemen. Deze handleiding wordt beschreven hoe u inhoud filteren en onlinebeoordelingen door mensen in een scenario voor het beheer van typische instellen met de Content Moderator-connector (die is opgenomen in de standaardinstelling).

## <a name="create-a-new-workflow"></a>Een nieuwe werkstroom maken

Ga naar de [Content Moderator-controlehulpprogramma](https://contentmoderator.cognitive.microsoft.com/) en meld u aan. Op de **instellingen** tabblad **werkstromen**.

![Werkstromen instellen](images/2-workflows-0.png)

Selecteer op het volgende scherm **werkstroom toevoegen**.

![Een werkstroom toevoegen](images/2-workflows-1.png)

### <a name="assign-a-name-and-description"></a>Toewijzen van een naam en beschrijving

Naam van uw werkstroom, voer een beschrijving in en kies of de werkstroom wordt afgehandeld, afbeeldingen of tekst.

![Naam van de werkstroom en beschrijving](images/image-workflow-create.PNG)

### <a name="define-evaluation-criteria"></a>Van evaluatiecriteria definiëren

In het volgende scherm, gaat u naar de **als** sectie. Kies in het bovenste vervolgkeuzemenu **voorwaarde**. Hiermee kunt u de voorwaarde waarop de werkstroom wordt maatregelen te configureren. Als u gebruiken van meerdere voorwaarden wilt, kiest u **combinatie** in plaats daarvan. 

Selecteer vervolgens een connector. In dit voorbeeld wordt **Content Moderator**. Afhankelijk van de connector die u kiest, krijgt u verschillende opties voor gegevensuitvoer. Zie de [Connectors](./configure.md#connectors) sectie van de handleiding voor de beoordeling van hulpprogramma voor meer informatie over het instellen van andere connectors.

![Selecteer de connector werkstroom](images/image-workflow-connect-to.PNG)

Kies de gewenste uitvoer te gebruiken en de voorwaarden om te controleren tegen instellen.

![Werkstroom voorwaarde definiëren](images/image-workflow-condition.PNG)

### <a name="define-the-action"></a>De actie definiëren

Ga naar de **vervolgens** sectie, waar u een actie selecteren. Het volgende voorbeeld maakt u een beoordeling van de installatiekopie en een label toegewezen. U kunt (optioneel) een alternatieve (Else)-pad toevoegen en een actie voor die ook ingesteld.

![Werkstroomactie definiëren](images/image-workflow-action.PNG)

### <a name="save-the-workflow"></a>De werkstroom opslaan

Noteer de Werkstroomnaam van de; u moet de naam voor het starten van een taak voor beheer met de werkstroom-API (Zie hieronder). Sla ten slotte de werkstroom met de **opslaan** knop aan de bovenkant van de pagina.

## <a name="test-the-workflow"></a>De werkstroom testen

Nu dat u een aangepaste werkstroom hebt gedefinieerd, kunt u deze met voorbeeldinhoud testen. Ga naar **werkstromen** en selecteert u de bijbehorende **werkstroom uitvoeren** knop.

![Werkstroom testen](images/image-workflow-execute.PNG)

Sla dit [voorbeeldafbeelding](https://moderatorsampleimages.blob.core.windows.net/samples/sample3.png) naar uw lokale schijf. Selecteer vervolgens **bestand(en) Kies** en de installatiekopie uploaden naar de werkstroom.

![een vrouw in een bepaalde bad kleur](images/sample-racy.PNG)

### <a name="track-progress"></a>Voortgang bijhouden

U kunt de voortgang van de werkstroom weergeven in het volgende pop-upvenster.

![Bijhouden van uitvoering](images/image-workflow-job.PNG)

### <a name="verify-workflow-action"></a>Controleer of de werkstroomactie

Ga naar de **installatiekopie** tabblad onder **bekijken** en controleer of er een beoordeling van de zojuist gemaakte installatiekopie.

![Afbeeldingen beoordelen](images/image-workflow-review.PNG)

## <a name="next-steps"></a>Volgende stappen

In deze handleiding hebt u geleerd hoe u kunt instellen en gebruiken van werkstromen voor beheer van de Content Moderator [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com). Hieronder staan de [REST-API-handleiding](../try-review-api-workflow.md) voor meer informatie over het maken van werkstromen via een programma.
