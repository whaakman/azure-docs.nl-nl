---
title: Voorbeeld - PCI-DSS v3.2.1 blauwdruk - stappen implementeren
description: Stappen van de Payment Card Industry Data Security Standard v3.2.1 blauwdruk voorbeeld implementeren.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/24/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: b4e9435e5f569a076fc5beb6441b9da935b87f3a
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561369"
---
# <a name="deploy-the-pci-dss-v321-blueprint-sample"></a>De PCI-DSS v3.2.1 blauwdruk voorbeeld implementeren

Voor het implementeren van het voorbeeld van Azure blauwdrukken PCI-DSS v3.2.1 blauwdruk, moeten de volgende stappen worden genomen:

> [!div class="checklist"]
> - Een nieuwe blauwdruk maken van de steekproef
> - Markeer uw kopie van het voorbeeld als **gepubliceerd**
> - Uw exemplaar van de blauwdruk toewijzen aan een bestaand abonnement

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free) aan voordat u begint.

## <a name="create-blueprint-from-sample"></a>Blauwdruk maken vanuit voorbeeld

Eerst de blauwdruk-voorbeeld implementeren door het maken van een nieuwe blauwdruk in uw omgeving met behulp van het voorbeeld als een starter.

1. Selecteer **alle services** en zoek en selecteer **beleid** in het linkerdeelvenster. Op de **beleid** weergeeft, schakelt **blauwdrukken**.

1. Uit de **aan de slag** pagina aan de linkerkant, selecteer de **maken** knop onder _een blauwdruk maken_.

1. Zoek de **PCI-DSS v3.2.1** blauwdruk voorbeeld onder _andere voorbeelden_ en selecteer **dit voorbeeld gebruiken**.

1. Voer de _basisbeginselen_ van de blauwdruk-voorbeeld:

   - **De blauwdruknaam van de**: Geef een naam voor uw exemplaar van het PCI-DSS v3.2.1 blauwdruk voorbeeld.
   - **Definitielocatie**: Gebruik het beletselteken en de beheergroep om op te slaan, uw kopie van het voorbeeld te selecteren.

1. Selecteer de _artefacten_ tabblad aan de bovenkant van de pagina of **volgende: Artefacten** aan de onderkant van de pagina.

1. Bekijk de lijst met artefacten die gezamenlijk de blauwdruk-voorbeeld. Veel van de artefacten hebben parameters die Definieer later. Selecteer **concept opslaan** wanneer u klaar bent met het voorbeeld van de blauwdruk controleren.

## <a name="publish-the-sample-copy"></a>De voorbeeld-kopie publiceren

Nu is uw kopie van het voorbeeld van de blauwdruk gemaakt in uw omgeving. Deze gemaakt **Draft** modus en moet **gepubliceerd** voordat deze kunnen worden toegewezen en geïmplementeerd. De kopie van de blauwdruk voorbeeld kan worden aangepast aan uw omgeving en behoeften, maar deze wijziging weg van de PCI-DSS v3.2.1 standard verplaatsen kan.

1. Selecteer **alle services** en zoek en selecteer **beleid** in het linkerdeelvenster. Op de **beleid** weergeeft, schakelt **blauwdrukken**.

1. Selecteer de **blauwdruk definities** pagina aan de linkerkant. De filters gebruiken om uw kopie van het voorbeeld van de blauwdruk zoeken en selecteer dit.

1. Selecteer **blauwdruk publiceren** aan de bovenkant van de pagina. In de nieuwe pagina aan de rechterkant, bieden een **versie** voor uw exemplaar van de blauwdruk-voorbeeld. Deze eigenschap is handig voor als u later een wijziging aanbrengt. Geef **notities wijzigen** zoals "eerste versie gepubliceerd vanuit de voorbeeld-PCI-DSS v3.2.1 blauwdruk." Selecteer vervolgens **publiceren** aan de onderkant van de pagina.

## <a name="assign-the-sample-copy"></a>De voorbeeld-kopie toewijzen

Nadat de kopie van het voorbeeld van de blauwdruk is **gepubliceerd**, deze kan worden toegewezen aan een abonnement in de beheergroep is opgeslagen op. Deze stap is waar de parameters worden opgegeven voor elke implementatie van de kopie van het voorbeeld van de blauwdruk uniek te maken.

1. Selecteer **alle services** en zoek en selecteer **beleid** in het linkerdeelvenster. Op de **beleid** weergeeft, schakelt **blauwdrukken**.

1. Selecteer de **blauwdruk definities** pagina aan de linkerkant. De filters gebruiken om uw kopie van het voorbeeld van de blauwdruk zoeken en selecteer dit.

1. Selecteer **blauwdruk toewijzen** aan de bovenkant van de pagina van de blauwdruk-definitie.

1. Geef de parameterwaarden voor de blauwdruktoewijzing:

   - Basics

     - **Abonnementen**: Selecteer een of meer van de abonnementen die in de beheergroep die u zijn uw exemplaar van de blauwdruk monster opgeslagen. Als u meer dan één abonnement selecteert, wordt een toewijzing worden gemaakt voor elk gebruik van de ingevoerde parameters.
     - **Naam van de roltoewijzing**: De naam is ingevuld op basis van de naam van de blauwdruk.
       Indien nodig wijzigen in of laat is.
     - **Locatie**: Selecteer een regio voor de beheerde identiteit moet worden gemaakt. Azure Blueprint gebruikt deze beheerde identiteit om alle artefacten in de toegewezen blauwdruk te implementeren. Zie [Beheerde identiteiten voor Azure-resources](../../../../active-directory/managed-identities-azure-resources/overview.md) voor meer informatie.
     - **De versie van blauwdruk**: Kies een **gepubliceerd** versie van uw exemplaar van de blauwdruk-voorbeeld.

   - LOCK-toewijzing

     Selecteer de blauwdruk vergrendeling instellen voor uw omgeving. Zie voor meer informatie [Vergrendeling van blauwdrukresources](../../concepts/resource-locking.md).

   - Beheerde identiteit

     Laat de standaardwaarde _systeem toegewezen_ identiteitsoptie beheerd.

   - Artefact parameters

     De gedefinieerde parameters in deze sectie zijn van toepassing op het artefact waarin deze gedefinieerd. Deze parameters zijn [dynamische parameters](../../concepts/parameters.md#dynamic-parameters) omdat ze tijdens de toewijzing van de blauwdruk zijn gedefinieerd. Zie voor een volledige lijst of artefact parameters en de bijbehorende beschrijvingen, [artefact parameters tabel](#artifact-parameters-table).

1. Zodra alle parameters hebt ingevoerd, selecteert u **toewijzen** aan de onderkant van de pagina. De blauwdruktoewijzing is gemaakt en artefact-implementatie begint. De implementatie duurt ongeveer een uur. Als u wilt controleren op de status van implementatie, de blauwdruktoewijzing te openen.

> [!WARNING]
> De blauwdrukken voor Azure-service en de ingebouwde blauwdruk voorbeelden zijn **gratis**. Azure-resources zijn [prijs per product](https://azure.microsoft.com/pricing/). Gebruik de [prijscalculator](https://azure.microsoft.com/pricing/calculator/) om in te schatten van de kosten van het uitvoeren van de resources die zijn geïmplementeerd door deze blauwdruk-voorbeeld.

## <a name="artifact-parameters-table"></a>Tabel artefact-parameters

De volgende tabel geeft een lijst van de blauwdruk artefact parameters:

|Naam van het assemblyartefact|Artefacttype|Parameternaam|Description|
|-|-|-|-|
|[Preview]: PCI v3.2.1:2018 besturingselementen controleren en implementeren van specifieke VM-extensies ter ondersteuning van audit-vereisten|Beleidstoewijzing|Lijst met resourcetypen | Diagnostische controle-instelling voor geselecteerde resourcetypen. Standaardwaarde is dat alle resources zijn geselecteerd| 
|Toegestane locaties|Beleidstoewijzing|Lijst met toegestane locaties|Lijst met data center locaties toegestaan voor elke bron om te worden geïmplementeerd in. Deze lijst is wereldwijd kan worden aangepast aan de gewenste Azure-locaties. Selecteer de locaties die u wilt toestaan.| 
|Locaties toegestaan voor resourcegroepen|Beleidstoewijzing |Toegestane locatie |Dit beleid kunt u de locaties die uw organisatie kunt maken van resourcegroepen in beperken. Dit beleid wordt gebruikt om uw geografische nalevingsvereisten af te dwingen.| 
|Controle op SQL-servers implementeren|Beleidstoewijzing|Logboekbehoud in dagen|Retentiegerelateerde gegevens in het aantal dagen. Standaardwaarde is 180 maar PCI 365 vereist.| 
|Controle op SQL-servers implementeren|Beleidstoewijzing|Resourcegroepnaam voor opslagaccount|Meld u controle schrijfbewerkingen databasegebeurtenissen naar een auditlogboek in uw Azure Storage-account (een storage-account wordt gemaakt in elke regio waarin een SQL-Server wordt gemaakt die worden gedeeld door alle servers in die regio).| 

## <a name="next-steps"></a>Volgende stappen

Nu dat u de stappen voor het PCI-DSS v3.2.1 blauwdruk voorbeeld implementeren hebt doorgenomen, gaat u naar de volgende artikelen voor meer informatie over het overzicht en de toewijzing van het besturingselement:

> [!div class="nextstepaction"]
> [PCI-DSS v3.2.1 blauwdruk - overzicht](./index.md)
> [PCI-DSS v3.2.1 blauwdruk - besturingselement toewijzing](./control-mapping.md)

Aanvullende artikelen over blauwdrukken en het gebruik hiervan:

- Meer informatie over de [levenscyclus van een blauwdruk](../../concepts/lifecycle.md).
- Meer informatie over hoe u [statische en dynamische parameters](../../concepts/parameters.md) gebruikt.
- Meer informatie over hoe u de [blauwdrukvolgorde](../../concepts/sequencing-order.md) aanpast.
- Meer informatie over hoe u gebruikmaakt van [resourcevergrendeling in blauwdrukken](../../concepts/resource-locking.md).
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](../../how-to/update-existing-assignments.md).
