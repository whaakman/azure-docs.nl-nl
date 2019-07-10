---
title: Voorbeeld - UK-OFFICIAL en UK NHS blauwdruk - stappen implementeren
description: Stappen voor het Verenigd Koninkrijk officiële en UK NHS blauwdruk voorbeelden implementeren.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/26/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 43aae882f27031d3e51ac8a4f5a68d243a973d6d
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67453201"
---
# <a name="deploy-the-uk-official-and-uk-nhs-blueprint-samples"></a>De officiële groot-Brittannië en UK NHS blauwdruk voorbeelden implementeren

Voor het implementeren van de officiële groot-Brittannië en UK NHS blauwdruk voorbeelden, moeten de volgende stappen worden genomen:

> [!div class="checklist"]
> - Een nieuwe blauwdruk maken van de steekproef
> - Markeer uw kopie van het voorbeeld als **gepubliceerd**
> - Uw exemplaar van de blauwdruk toewijzen aan een bestaand abonnement

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free) aan voordat u begint.

## <a name="create-blueprint-from-sample"></a>Blauwdruk maken vanuit voorbeeld

Eerst de blauwdruk-voorbeeld implementeren door het maken van een nieuwe blauwdruk in uw omgeving met behulp van het voorbeeld als een starter.

1. Selecteer **alle services** en zoek en selecteer **beleid** in het linkerdeelvenster. Op de **beleid** weergeeft, schakelt **blauwdrukken**.

1. Uit de **aan de slag** pagina aan de linkerkant, selecteer de **maken** knop onder _een blauwdruk maken_.

1. Zoek de **UK-OFFICIAL** of **UK NHS** blauwdruk voorbeeld onder _andere voorbeelden_ en selecteer **dit voorbeeld gebruiken**.

1. Voer de _basisbeginselen_ van de blauwdruk-voorbeeld:

   - **De blauwdruknaam van de**: Geef een naam voor uw exemplaar van de blauwdruk-voorbeeld.
   - **Definitielocatie**: Gebruik het beletselteken en de beheergroep om op te slaan, uw kopie van het voorbeeld te selecteren.

1. Selecteer de _artefacten_ tabblad aan de bovenkant van de pagina of **volgende: Artefacten** aan de onderkant van de pagina.

1. Bekijk de lijst met artefacten die gezamenlijk de blauwdruk-voorbeeld. Veel van de artefacten hebben parameters die Definieer later. Selecteer **concept opslaan** wanneer u klaar bent met het voorbeeld van de blauwdruk controleren.

## <a name="publish-the-sample-copy"></a>De voorbeeld-kopie publiceren

Nu is uw kopie van het voorbeeld van de blauwdruk gemaakt in uw omgeving. Deze gemaakt **Draft** modus en moet **gepubliceerd** voordat deze kunnen worden toegewezen en geïmplementeerd. De kopie van de blauwdruk voorbeeld kan worden aangepast aan uw omgeving en behoeften, maar deze wijziging van de standaard verplaatsen kan.

1. Selecteer **alle services** en zoek en selecteer **beleid** in het linkerdeelvenster. Op de **beleid** weergeeft, schakelt **blauwdrukken**.

1. Selecteer de **blauwdruk definities** pagina aan de linkerkant. De filters gebruiken om uw kopie van het voorbeeld van de blauwdruk zoeken en selecteer dit.

1. Selecteer **blauwdruk publiceren** aan de bovenkant van de pagina. In de nieuwe pagina aan de rechterkant, bieden een **versie** voor uw exemplaar van de blauwdruk-voorbeeld. Deze eigenschap is handig voor als u later een wijziging aanbrengt. Geef **notities wijzigen** zoals "eerste versie gepubliceerd vanuit de voorbeeld-UK-OFFICIAL of groot-Brittannië NHS blauwdruk." Selecteer vervolgens **publiceren** aan de onderkant van de pagina.

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

Naam van het assemblyartefact|Artefacttype|Parameternaam|Description|
|-|-|-|-|
|Blauwdruk initiatief voor UK-OFFICIAL of groot-Brittannië NHS|Beleidstoewijzing |Resourcetypen om te controleren, logboeken met diagnostische gegevens (beleid: Blauwdruk initiatief voor UK-OFFICIAL of groot-Brittannië NHS) |Lijst met resourcetypen om te controleren als de instelling voor diagnostische logboeken Opmerking ingeschakeld.  Zie voor de acceptabele waarden [ondersteunde services, schema's en categorieën voor diagnostische logboeken van Azure](../../../../azure-monitor/platform/diagnostic-logs-schema.md). |
|[Preview]: Log Analytics-Agent voor Linux-VM's implementeren |Beleidstoewijzing |Optioneel: Lijst met VM-installatiekopieën die zijn Linux-besturingssysteem toe te voegen aan het bereik wordt ondersteund (beleid: [Preview]: Log Analytics-Agent voor Linux-VM's implementeren) |(Optioneel) Standaardwaarde is _geen_. Zie voor meer informatie, [een Log Analytics-werkruimte maken in Azure portal](../../../../azure-monitor/learn/quick-create-workspace.md). |
|[Preview]: Log Analytics-Agent voor Windows-VM's implementeren |Beleidstoewijzing |Optioneel: Lijst met VM-installatiekopieën waarop ondersteunde Windows-besturingssysteem toe te voegen aan het bereik (beleid: [Preview]: Log Analytics-Agent voor Windows-VM's implementeren) |(Optioneel) Standaardwaarde is _geen_. Zie voor meer informatie, [een Log Analytics-werkruimte maken in Azure portal](../../../../azure-monitor/learn/quick-create-workspace.md). |

## <a name="next-steps"></a>Volgende stappen

Nu dat u de stappen voor het implementeren van de officiële groot-Brittannië en UK NHS blauwdruk voorbeelden hebben doorgenomen, gaat u naar de volgende artikelen voor meer informatie over het overzicht en de toewijzing van het besturingselement:

> [!div class="nextstepaction"]
> [Groot-Brittannië officiële en UK NHS blauwdrukken - overzicht](./index.md)
> [UK-OFFICIAL en UK NHS blauwdrukken - toewijzing beheren](./control-mapping.md)

Aanvullende artikelen over blauwdrukken en het gebruik hiervan:

- Meer informatie over de [levenscyclus van een blauwdruk](../../concepts/lifecycle.md).
- Meer informatie over hoe u [statische en dynamische parameters](../../concepts/parameters.md) gebruikt.
- Meer informatie over hoe u de [blauwdrukvolgorde](../../concepts/sequencing-order.md) aanpast.
- Meer informatie over hoe u gebruikmaakt van [resourcevergrendeling in blauwdrukken](../../concepts/resource-locking.md).
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](../../how-to/update-existing-assignments.md).
