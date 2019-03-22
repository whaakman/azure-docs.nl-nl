---
title: Een omgeving maken vanuit een voorbeeld van de blauwdruk
description: Een voorbeeld van een blauwdruk gebruik te maken van de blauwdrukdefinitie van een die stelt u twee resourcegroepen en configureert een roltoewijzing voor elk.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/05/2019
ms.topic: tutorial
ms.service: blueprints
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 68bd6e3942d2b2e2c8bcf39bfcff10540e564315
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57411169"
---
# <a name="create-an-environment-from-a-blueprint-sample"></a>Een omgeving maken vanuit een voorbeeld van de blauwdruk

Voorbeeld blauwdrukken vindt u voorbeelden van wat kan worden gedaan met behulp van Azure blauwdrukken. Elk is een voorbeeld met een bepaald doel of het doel, maar niet een volledige omgeving maken door zelf. Zijn bedoeld als uitgangspunt voor het verkennen met behulp van Azure blauwdrukken met verschillende combinaties van opgenomen artefacten, ontwerpen en parameters.

De volgende zelfstudie wordt gebruikgemaakt van de **resourcegroepen met RBAC** blauwdruk voorbeeld naar verschillende aspecten van de service blauwdrukken presenteren. De volgende stappen worden behandeld:

> [!div class="checklist"]
> - Een nieuwe blauwdrukdefinitie maken van de steekproef
> - Markeer uw kopie van het voorbeeld als **gepubliceerd**
> - Uw exemplaar van de blauwdruk toewijzen aan een bestaand abonnement
> - Controleren van geïmplementeerde resources voor de toewijzing
> - Toewijzing van de blauwdruk om de vergrendelingen te verwijderen

## <a name="prerequisites"></a>Vereisten

Als u wilt in deze zelfstudie hebt voltooid, wordt een Azure-abonnement nodig. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="create-blueprint-definition-from-sample"></a>Blauwdrukdefinitie maken vanuit voorbeeld

Eerst de blauwdruk-voorbeeld implementeren. Een nieuwe blauwdruk importeren in uw omgeving op basis van de steekproef worden gemaakt.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Uit de **aan de slag** pagina aan de linkerkant, selecteer de **maken** knop onder _een blauwdruk maken_.

1. Zoek de **resourcegroepen met RBAC** blauwdruk voorbeeld onder _andere voorbeelden_ en selecteer **dit voorbeeld gebruiken**.

1. Voer de _basisbeginselen_ van de blauwdruk-voorbeeld:

   - **De blauwdruknaam van de**: Geef een naam voor uw exemplaar van de blauwdruk-voorbeeld. Voor deze zelfstudie gebruiken we de naam _twee-rgs-met--roltoewijzingen_.
   - **Definitielocatie**: Gebruik het beletselteken en selecteer de beheergroep of het abonnement op te slaan, uw kopie van het voorbeeld op.

1. Selecteer de _artefacten_ tabblad aan de bovenkant van de pagina of **volgende: Artefacten** aan de onderkant van de pagina.

1. Bekijk de lijst met artefacten die gezamenlijk de blauwdruk-voorbeeld. In dit voorbeeld definieert twee resourcegroepen met weergavenamen van _ProdRG_ en _PreProdRG_. De uiteindelijke naam en locatie van elke resourcegroep worden ingesteld tijdens de blauwdruktoewijzing. De _ProdRG_ resourcegroep is toegewezen de _Inzender_ rol en de _PreProdRG_ resourcegroep is toegewezen de _eigenaar_ en  _Lezers_ rollen. De rollen toegewezen in het definitie statisch zijn, maar de gebruiker, app of groep die de rol is toegewezen tijdens de blauwdruktoewijzing is ingesteld.

1. Selecteer **concept opslaan** wanneer u klaar bent met het voorbeeld van de blauwdruk controleren.

Deze stap maakt u een kopie van de blauwdrukdefinitie voorbeeld in de geselecteerde beheergroep of het abonnement. De opgeslagen blauwdrukdefinitie wordt beheerd, zoals een blauwdruk helemaal is gemaakt. U kunt het voorbeeld opslaan naar uw management groep of abonnement zo vaak als nodig is. Elk exemplaar moet echter een unieke naam opgegeven.

Zodra de **opslaan blauwdrukdefinitie is** portalmelding wordt weergegeven, verplaatsen naar de volgende stap.

## <a name="publish-the-sample-copy"></a>De voorbeeld-kopie publiceren

Nu is uw kopie van het voorbeeld van de blauwdruk gemaakt in uw omgeving. Deze gemaakt **Draft** modus en moet **gepubliceerd** voordat deze kunnen worden toegewezen en geïmplementeerd. De kopie van de blauwdruk voorbeeld kan worden aangepast aan uw omgeving en behoeften. Voor deze zelfstudie wordt niet we geen wijzigingen aanbrengen.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Selecteer de **blauwdruk definities** pagina aan de linkerkant. De filters gebruiken om te zoeken de _twee-rgs-met--roltoewijzingen_ blauwdruk definitie en selecteer dit.

1. Selecteer **blauwdruk publiceren** aan de bovenkant van de pagina. Geef in het nieuwe deelvenster aan de rechterkant **versie** als _1.0_ voor uw exemplaar van de blauwdruk-voorbeeld. Deze eigenschap is handig voor als u later een wijziging aanbrengt. Geef **notities wijzigen** zoals "eerste versie gepubliceerd vanuit de resourcegroepen met RBAC blauwdruk voorbeeld." Selecteer vervolgens **publiceren** aan de onderkant van de pagina.

Deze stap maakt het mogelijk is de blauwdruk toewijzen aan een abonnement. Zodra gepubliceerd, kunnen nog steeds wijzigingen worden aangebracht. Aanvullende wijzigingen publiceren met een nieuwe vereisen **versie** waarde voor het bijhouden van de verschillen tussen verschillende versies van dezelfde blauwdrukdefinitie voor.

Zodra de **publiceren is voltooid de definitie van blauwdruk** portalmelding wordt weergegeven, verplaatsen naar de volgende stap.

## <a name="assign-the-sample-copy"></a>De voorbeeld-kopie toewijzen

Nadat de kopie van het voorbeeld van de blauwdruk is **gepubliceerd**, deze kan worden toegewezen aan een abonnement in de beheergroep is opgeslagen op. Deze stap is waar de parameters worden opgegeven voor elke implementatie van de kopie van het voorbeeld van de blauwdruk uniek te maken.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Selecteer de **blauwdruk definities** pagina aan de linkerkant. De filters gebruiken om te zoeken de _twee-rgs-met--roltoewijzingen_ blauwdruk definitie en selecteer dit.

1. Selecteer **blauwdruk toewijzen** aan de bovenkant van de pagina van de blauwdruk-definitie.

1. Geef de parameterwaarden voor de blauwdruktoewijzing:

   - Basisbeginselen

     - **Abonnementen**: Selecteer een of meer van de abonnementen die in de beheergroep die u zijn uw exemplaar van de blauwdruk monster opgeslagen. Als u meer dan één abonnement selecteert, wordt een toewijzing worden gemaakt voor elk gebruik van de ingevoerde parameters.
     - **Naam van de roltoewijzing**: De naam is ingevuld op basis van de naam van de blauwdrukdefinitie.
     - **Locatie**: Selecteer een regio voor de beheerde identiteit moet worden gemaakt. Azure Blueprint gebruikt deze beheerde identiteit om alle artefacten in de toegewezen blauwdruk te implementeren. Zie [Beheerde identiteiten voor Azure-resources](../../../active-directory/managed-identities-azure-resources/overview.md) voor meer informatie.
       Selecteer voor deze zelfstudie _VS-Oost 2_.
     - **De versie van blauwdruk**: Kies de **gepubliceerd** versie _1.0_ van uw exemplaar van de blauwdrukdefinitie voorbeeld.

   - Toewijzing vergrendelen

     Selecteer de _alleen-lezen_ blauwdruk lock-modus. Zie voor meer informatie [Vergrendeling van blauwdrukresources](../concepts/resource-locking.md).

   - Beheerde identiteit

     Laat de standaardwaarde _systeem toegewezen_ optie. Zie voor meer informatie, [beheerde identiteiten](../../../active-directory/managed-identities-azure-resources/overview.md).

   - Artefactparameters

     De gedefinieerde parameters in deze sectie zijn van toepassing op het artefact waarin deze gedefinieerd. Deze parameters zijn [dynamische parameters](../concepts/parameters.md#dynamic-parameters) omdat ze tijdens de toewijzing van de blauwdruk zijn gedefinieerd. Voor elke artefact, stelt u de waarde van parameter op wat is gedefinieerd de **waarde** kolom. Voor `{Your ID}`, selecteer uw Azure-gebruikersaccount.

     |Naam van het artefact|Type artefact|Parameternaam|Value|Description|
     |-|-|-|-|-|
     |Resourcegroep ProdRG|Resourcegroep|Name|ProductionRG|Bepaalt de naam van de eerste resourcegroep.|
     |Resourcegroep ProdRG|Resourcegroep|Locatie|US - west 2|Hiermee stelt u de locatie van de eerste resourcegroep.|
     |Inzender|Roltoewijzing|Gebruiker of groep|{De ID}|Bepaalt welke gebruiker of groep om te verlenen de _Inzender_ roltoewijzing binnen de eerste resourcegroep.|
     |Resourcegroep PreProdRG|Resourcegroep|Name|PreProductionRG|Bepaalt de naam van de tweede resourcegroep.|
     |Resourcegroep PreProdRG|Resourcegroep|Locatie|US - west|Hiermee stelt u de locatie van de tweede resourcegroep.|
     |Eigenaar|Roltoewijzing|Gebruiker of groep|{De ID}|Bepaalt welke gebruiker of groep om te verlenen de _eigenaar_ roltoewijzing in de tweede resourcegroep bevinden.|
     |Lezers|Roltoewijzing|Gebruiker of groep|{De ID}|Bepaalt welke gebruiker of groep om te verlenen de _lezers_ roltoewijzing in de tweede resourcegroep bevinden.|

1. Zodra alle parameters hebt ingevoerd, selecteert u **toewijzen** aan de onderkant van de pagina.

In deze stap implementeert u de gedefinieerde resources en configureert u de geselecteerde **vergrendeling toewijzing**. Blauwdruk vergrendelingen kunnen maximaal 30 minuten om toe te passen.

Zodra de **toewijzen blauwdrukdefinitie is** portalmelding wordt weergegeven, verplaatsen naar de volgende stap.

## <a name="inspect-resources-deployed-by-the-assignment"></a>Resources die zijn geïmplementeerd door de toewijzing controleren

De blauwdruktoewijzing maakt en volgt de artefacten die zijn gedefinieerd in het blauwdrukdefinitie van de. De status van de resources van de pagina van de toewijzing van blauwdruk en door te kijken naar de resources rechtstreeks ziet.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Selecteer de **toegewezen blauwdrukken** pagina aan de linkerkant. De filters gebruiken om te zoeken de _Assignment-two-rgs-with-role-assignments_ blauwdruk toewijzen en selecteer dit.

   Op deze pagina ziet er de toewijzing is voltooid en de lijst van de gemaakte resources samen met hun status van de blauwdruk vergrendeling. Als de toewijzing is bijgewerkt, de **toewijzingsbewerking** vervolgkeuzelijst bevat gedetailleerde informatie over de implementatie van elke versie van de definitie. Elke vermelde bron die is gemaakt, kan worden geklikt en wordt die eigenschap resources pagina geopend.

1. Selecteer de **ProductionRG** resourcegroep.

   We zien dat de naam van de resourcegroep is **ProductionRG** en niet de weergavenaam van het artefact _ProdRG_. Deze naam komt overeen met de waarde die tijdens de blauwdruktoewijzing is ingesteld.

1. Selecteer de **toegangsbeheer (IAM)** pagina aan de linkerkant en vervolgens de **roltoewijzingen** tabblad.

   Hier ziet u dat uw account heeft gekregen de _Inzender_ -rol op het bereik van _deze resource_. De _Assignment-two-rgs-with-role-assignments_ blauwdruktoewijzing is de _eigenaar_ rol die is gebruikt om de resourcegroep te maken. Deze machtigingen worden ook gebruikt voor het beheren van resources met geconfigureerde blauwdruk wordt vergrendeld.

1. Selecteer in de Azure portal breadcrumb **Assignment-two-rgs-with-role-assignments** om terug te gaan één pagina, selecteert u vervolgens de **PreProductionRG** resourcegroep.

1. Selecteer de **toegangsbeheer (IAM)** pagina aan de linkerkant en vervolgens de **roltoewijzingen** tabblad.

   Hier ziet u dat uw account heeft gekregen zowel de _eigenaar_ en _lezer_ rollen, zowel op het bereik van _deze resource_. De blauwdruktoewijzing is ook de _eigenaar_ rol als de eerste resourcegroep.

1. Selecteer de **weigeren toewijzingen** tabblad.

   De blauwdruktoewijzing gemaakt een [toewijzing weigeren](../../../role-based-access-control/deny-assignments.md) voor de geïmplementeerde resourcegroep om af te dwingen de _alleen-lezen_ blauwdruk lock-modus. De toewijzing weigeren voorkomt dat iemand met de juiste rechten heeft op de _roltoewijzingen_ tabblad uitvoeren van specifieke acties. Is van invloed op de toewijzing weigeren _alle principals_.

1. Selecteer de toewijzing weigeren en selecteer vervolgens de **geweigerd machtigingen** pagina aan de linkerkant.

   De toewijzing weigeren wordt voorkomen dat alle bewerkingen met de **\*** en **actie** configuratie, maar geeft leestoegang door uit te sluiten  **\* /lezen**via **NotActions**.

1. Selecteer in de Azure portal breadcrumb **PreProductionRG - toegangsbeheer (IAM)**. Selecteer vervolgens de **overzicht** pagina aan de linkerkant en vervolgens de **resourcegroep verwijderen** knop. Voer de naam _PreProductionRG_ om te bevestigen van het verwijderen en selecteer **verwijderen** aan de onderkant van het deelvenster.

   De portalmelding **PreProductionRG is mislukt voor resourcegroep verwijderen** wordt weergegeven. De fout gemeld dat terwijl uw account is gemachtigd om de resourcegroep te verwijderen, de toegang is geweigerd door de blauwdruktoewijzing. Houd er rekening mee dat we geselecteerd de _alleen-lezen_ blauwdruk vergrendelingsmodus tijdens de blauwdruktoewijzing. De blauwdruk vergrendeling voorkomt dat een account met machtigingen, zelfs _eigenaar_, uit de bron te verwijderen. Zie voor meer informatie [Vergrendeling van blauwdrukresources](../concepts/resource-locking.md).

Deze stappen laten zien dat onze resources zijn gemaakt, zoals gedefinieerd en de blauwdruk vergrendelingen voorkomen ongewenste verwijderen, zelfs bij een account met machtigingen dat.

## <a name="unassign-the-blueprint"></a>De blauwdruk intrekken

De laatste stap is het verwijderen van de toewijzing van de blauwdruk en de resources die deze geïmplementeerd.
De toewijzing verwijdert, niet de geïmplementeerde artefacten.

1. Selecteer **Alle services** in het linkerdeelvenster. Zoek en selecteer **Blauwdrukken**.

1. Selecteer de **toegewezen blauwdrukken** pagina aan de linkerkant. De filters gebruiken om te zoeken de _Assignment-two-rgs-with-role-assignments_ blauwdruk toewijzen en selecteer dit.

1. Selecteer de **toewijzing verwijderen blauwdruk** knop aan de bovenkant van de pagina. Lees de waarschuwing in het bevestigingsvenster en selecteer vervolgens **OK**.

   Met de blauwdruktoewijzing is verwijderd, worden ook de blauwdruk vergrendelingen verwijderd. De gemaakte resources kunnen opnieuw worden verwijderd door een account met machtigingen.

1. Selecteer **resourcegroepen** vanuit het menu van Azure, selecteert u vervolgens **ProductionRG**.

1. Selecteer de **toegangsbeheer (IAM)** pagina aan de linkerkant en vervolgens de **roltoewijzingen** tabblad.

De beveiliging voor elke resourcegroepen heeft nog steeds de geïmplementeerde roltoewijzingen, maar heeft niet langer de blauwdruktoewijzing _eigenaar_ toegang.

Zodra de **verwijderen blauwdruktoewijzing is voltooid** portalmelding wordt weergegeven, verplaatsen naar de volgende stap.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met deze zelfstudie, verwijdert u de volgende resources:

- Resourcegroep _ProductionRG_
- Resourcegroep _PreProductionRG_
- De definitie van blauwdruk _twee-rgs-met--roltoewijzingen_

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [levenscyclus van een blauwdruk](../concepts/lifecycle.md)
- Informatie over hoe u [statische en dynamische parameters](../concepts/parameters.md) gebruikt
- Ontdek hoe u gebruikmaakt van [resourcevergrendeling in blauwdrukken](../concepts/resource-locking.md)
- Meer informatie over hoe u de [blauwdrukvolgorde](../concepts/sequencing-order.md) aanpast
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](../how-to/update-existing-assignments.md)
- Problemen oplossen tijdens de toewijzing van een blauwdruk met [algemene probleemoplossing](../troubleshoot/general.md)