---
title: Voorbeeld - ISO 27001-App Service-omgeving/SQL Database-workload blauwdruk - stappen implementeren
description: Stappen van de ISO 27001-App Service-omgeving/SQL Database-workload blauwdruk voorbeeld implementeren.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 80c98170fc136c20d8489cec5d145f96e207bc9f
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58201905"
---
# <a name="deploy-the-azure-blueprints-iso-27001-app-service-environmentsql-database-workload-blueprint-sample"></a>Het voorbeeld van Azure blauwdrukken ISO 27001-App Service Environment/SQL Database-workload blauwdruk implementeren

Voor het implementeren van het voorbeeld van Azure blauwdrukken ISO 27001-App Service Environment/SQL Database-workload blauwdruk, moeten de volgende stappen worden genomen:

> [!div class="checklist"]
> - Implementeer de [ISO 27001 gedeelde Services](../iso27001-shared/index.md) blauwdruk voorbeeld
> - Een nieuwe blauwdruk maken van de steekproef
> - Markeer uw kopie van het voorbeeld als **gepubliceerd**
> - Uw exemplaar van de blauwdruk toewijzen aan een bestaand abonnement

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free) aan voordat u begint.

## <a name="deploy-the-iso-27001-shared-services-blueprint-sample"></a>Het voorbeeld van de blauwdruk ISO 27001 gedeelde Services implementeren

Voordat dit voorbeeld blauwdruk kan worden geïmplementeerd, de [ISO 27001 gedeelde Services](../iso27001-shared/index.md) blauwdruk voorbeeld moet worden geïmplementeerd in het doelabonnement. Zonder een geslaagde implementatie van het voorbeeld van de blauwdruk ISO 27001 gedeelde Services, in dit voorbeeld blauwdruk ontbreken infrastructuurafhankelijkheden en mislukken tijdens de implementatie.

> [!IMPORTANT]
> In dit voorbeeld blauwdruk moet worden toegewezen in hetzelfde abonnement als de [ISO 27001 gedeelde Services](../iso27001-shared/index.md) blauwdruk voorbeeld.

## <a name="create-blueprint-from-sample"></a>Blauwdruk maken vanuit voorbeeld

Eerst de blauwdruk-voorbeeld implementeren door het maken van een nieuwe blauwdruk in uw omgeving met behulp van het voorbeeld als een starter.

1. Selecteer **alle services** en zoek en selecteer **beleid** in het linkerdeelvenster. Op de **beleid** weergeeft, schakelt **blauwdrukken**.

1. Uit de **aan de slag** pagina aan de linkerkant, selecteer de **maken** knop onder _een blauwdruk maken_.

1. Zoek de **ISO 27001: As-omgeving/SQL-Workload** blauwdruk voorbeeld onder _andere voorbeelden_ en selecteer **dit voorbeeld gebruiken**.

1. Voer de _basisbeginselen_ van de blauwdruk-voorbeeld:

   - **De blauwdruknaam van de**: Geef een naam voor uw exemplaar van de ISO 27001-as-omgeving/SQL werkbelasting blauwdruk voorbeeld.
   - **Definitielocatie**: Gebruik het beletselteken en de beheergroep om op te slaan, uw kopie van het voorbeeld te selecteren.

1. Selecteer de _artefacten_ tabblad aan de bovenkant van de pagina of **volgende: Artefacten** aan de onderkant van de pagina.

1. Bekijk de lijst met artefacten die gezamenlijk de blauwdruk-voorbeeld. Veel van de artefacten hebben parameters die Definieer later. Selecteer **concept opslaan** wanneer u klaar bent met het voorbeeld van de blauwdruk controleren.

## <a name="publish-the-sample-copy"></a>De voorbeeld-kopie publiceren

Nu is uw kopie van het voorbeeld van de blauwdruk gemaakt in uw omgeving. Deze gemaakt **Draft** modus en moet **gepubliceerd** voordat deze kunnen worden toegewezen en geïmplementeerd. De kopie van de blauwdruk voorbeeld kan worden aangepast aan uw omgeving en behoeften, maar deze wijziging van de ISO 27001-standaard verplaatsen kan.

1. Selecteer **alle services** en zoek en selecteer **beleid** in het linkerdeelvenster. Op de **beleid** weergeeft, schakelt **blauwdrukken**.

1. Selecteer de **blauwdruk definities** pagina aan de linkerkant. De filters gebruiken om uw kopie van het voorbeeld van de blauwdruk zoeken en selecteer dit.

1. Selecteer **blauwdruk publiceren** aan de bovenkant van de pagina. In de nieuwe pagina aan de rechterkant, bieden een **versie** voor uw exemplaar van de blauwdruk-voorbeeld. Deze eigenschap is handig voor als u later een wijziging aanbrengt. Geef **notities wijzigen** zoals "eerste versie gepubliceerd vanuit de voorbeeld-ISO 27001 blauwdruk." Selecteer vervolgens **publiceren** aan de onderkant van de pagina.

## <a name="assign-the-sample-copy"></a>De voorbeeld-kopie toewijzen

Nadat de kopie van het voorbeeld van de blauwdruk is **gepubliceerd**, deze kan worden toegewezen aan een abonnement in de beheergroep is opgeslagen op. Deze stap is waar de parameters worden opgegeven voor elke implementatie van de kopie van het voorbeeld van de blauwdruk uniek te maken.

1. Selecteer **alle services** en zoek en selecteer **beleid** in het linkerdeelvenster. Op de **beleid** weergeeft, schakelt **blauwdrukken**.

1. Selecteer de **blauwdruk definities** pagina aan de linkerkant. De filters gebruiken om uw kopie van het voorbeeld van de blauwdruk zoeken en selecteer dit.

1. Selecteer **blauwdruk toewijzen** aan de bovenkant van de pagina van de blauwdruk-definitie.

1. Geef de parameterwaarden voor de blauwdruktoewijzing:

   - Basisbeginselen

     - **Abonnementen**: Selecteer een of meer van de abonnementen die in de beheergroep die u zijn uw exemplaar van de blauwdruk monster opgeslagen. Als u meer dan één abonnement selecteert, wordt een toewijzing worden gemaakt voor elk gebruik van de ingevoerde parameters.
     - **Naam van de roltoewijzing**: De naam is ingevuld op basis van de naam van de blauwdruk.
       Indien nodig wijzigen in of laat is.
     - **Locatie**: Selecteer een regio voor de beheerde identiteit moet worden gemaakt. Azure Blueprint gebruikt deze beheerde identiteit om alle artefacten in de toegewezen blauwdruk te implementeren. Zie [Beheerde identiteiten voor Azure-resources](../../../../active-directory/managed-identities-azure-resources/overview.md) voor meer informatie.
     - **De versie van blauwdruk**: Kies een **gepubliceerd** versie van uw exemplaar van de blauwdruk-voorbeeld.

   - Toewijzing vergrendelen

     Selecteer de blauwdruk vergrendeling instellen voor uw omgeving. Zie voor meer informatie [Vergrendeling van blauwdrukresources](../../concepts/resource-locking.md).

   - Beheerde identiteit

     Laat de standaardwaarde _systeem toegewezen_ identiteitsoptie beheerd.

   - Blauwdrukparameters

     De gedefinieerde parameters in deze sectie worden gebruikt door veel van de artefacten in het blauwdrukdefinitie van de consistentie kunt bieden.

     - **Naam van de organisatie**: Geef een korte-naam voor uw organisatie. Deze eigenschap wordt voornamelijk gebruikt voor de naamgeving van resources.
     - **Gedeelde Service abonnements-ID**: Abonnements-ID waar de [ISO 27001 gedeelde Services](../iso27001-shared/index.md) voorbeeld van de blauwdruk wordt toegewezen.
     - **Standaard subnet adresvoorvoegsel**: De CIDR-notatie voor het subnet van de standaard virtueel netwerk.
       Standaardwaarde is _10.1.0.0/16_.
     - **Locatie van de werkbelasting**: Hiermee bepaalt u welke locatie de artefacten zijn geïmplementeerd op. Niet alle services zijn beschikbaar in alle locaties. Artefacten implementeren van deze services bieden een parameteroptie voor voor de locatie die artefact dat moet implementeren.

   - Artefactparameters

     De gedefinieerde parameters in deze sectie zijn van toepassing op het artefact waarin deze gedefinieerd. Deze parameters zijn [dynamische parameters](../../concepts/parameters.md#dynamic-parameters) omdat ze tijdens de toewijzing van de blauwdruk zijn gedefinieerd. Zie voor een volledige lijst of artefact parameters en de bijbehorende beschrijvingen, [artefact parameters tabel](#artifact-parameters-table).

1. Zodra alle parameters hebt ingevoerd, selecteert u **toewijzen** aan de onderkant van de pagina. De blauwdruktoewijzing is gemaakt en artefact-implementatie begint. De implementatie duurt ongeveer een uur. Als u wilt controleren op de status van implementatie, de blauwdruktoewijzing te openen.

> [!WARNING]
> De blauwdrukken voor Azure-service en de ingebouwde blauwdruk voorbeelden zijn **gratis**. Azure-resources zijn [prijs per product](https://azure.microsoft.com/pricing/). Gebruik de [prijscalculator](https://azure.microsoft.com/pricing/calculator/) om in te schatten van de kosten van het uitvoeren van de resources die zijn geïmplementeerd door deze blauwdruk-voorbeeld.

## <a name="artifact-parameters-table"></a>Tabel artefact-parameters

De volgende tabel geeft een lijst van de blauwdruk artefact parameters:

|Naam van het artefact|Type artefact|Parameternaam|Description|
|-|-|-|-|
|Log Analytics-resourcegroep|Resourcegroep|Name|**Vergrendeld** -worden samengevoegd in de **organisatienaam** met `-workload-log-rg` uniek te maken de resourcegroep.|
|Log Analytics-resourcegroep|Resourcegroep|Locatie|**Vergrendeld** -de blauwdrukparameter wordt gebruikt.|
|Log Analytics-sjabloon|Resource Manager-sjabloon|Servicelaag|Hiermee stelt u de laag van de Log Analytics-werkruimte. Standaardwaarde is _PerNode_.|
|Log Analytics-sjabloon|Resource Manager-sjabloon|Behoud van logboeken in dagen|Het bewaren van gegevens in dagen. Standaardwaarde is _365_.|
|Log Analytics-sjabloon|Resource Manager-sjabloon|Locatie|De regio die wordt gebruikt voor het maken van de Log Analytics-werkruimte. Standaardwaarde is _VS-West 2_.|
|Netwerk-resourcegroep|Resourcegroep|Name|**Vergrendeld** -worden samengevoegd in de **organisatienaam** met `-workload-net-rg` uniek te maken de resourcegroep.|
|Netwerk-resourcegroep|Resourcegroep|Locatie|**Vergrendeld** -de blauwdrukparameter wordt gebruikt.|
|Sjabloon voor netwerkbeveiligingsgroep|Resource Manager-sjabloon|Behoud van logboeken in dagen|Het bewaren van gegevens in dagen. Standaardwaarde is _365_.|
|Sjabloon voor virtueel netwerk en routetabel|Resource Manager-sjabloon|Privé-IP-adres voor Azure-firewall|Hiermee configureert u de privé IP-adres van de [Azure firewall](../../../../firewall/overview.md). Moeten deel uitmaken van de CIDR-notatie die is gedefinieerd in _ISO 27001: Gedeelde Services_ artefact parameter **Azure Firewall subnet adresvoorvoegsel**. Standaardwaarde is _10.0.4.4_.|
|Sjabloon voor virtueel netwerk en routetabel|Resource Manager-sjabloon|Abonnements-id voor gedeelde services|De waarde die wordt gebruikt om in te schakelen van VNET-peering tussen een werkbelasting en gedeelde Services.|
|Sjabloon voor virtueel netwerk en routetabel|Resource Manager-sjabloon|Voorvoegsel van adres voor virtueel netwerk|De CIDR-notatie voor het virtuele netwerk. Standaardwaarde is _10.1.0.0/16_.|
|Sjabloon voor virtueel netwerk en routetabel|Resource Manager-sjabloon|Standaardvoorvoegsel van subnetadres|De CIDR-notatie voor het subnet van de standaard virtueel netwerk. Standaardwaarde is _10.1.0.0/16_.|
|Sjabloon voor virtueel netwerk en routetabel|Resource Manager-sjabloon|ADDS-IP-adres|IP-adres van de eerste VM is toegevoegd. Deze waarde wordt gebruikt als aangepaste VNET DNS.|
|Key Vault-resourcegroep|Resourcegroep|Name|**Vergrendeld** -worden samengevoegd in de **organisatienaam** met `-workload-kv-rg` uniek te maken de resourcegroep.|
|Key Vault-resourcegroep|Resourcegroep|Locatie|**Vergrendeld** -de blauwdrukparameter wordt gebruikt.|
|Key Vault-sjabloon|Resource Manager-sjabloon|AAD-object-id|De AAD-object-id van het account dat toegang tot de Key Vault-instantie is vereist. Er is geen standaard waarde en mag niet leeg. U vindt deze waarde uit de Azure portal door zoek en selecteer 'Gebruikers' onder _Services_. Gebruik de _naam_ in om te filteren op naam van de account en selecteert u dat account. Op de _gebruikersprofiel_ pagina, selecteer het pictogram 'Klikken om te kopiëren' bij de _Object-ID_.|
|Key Vault-sjabloon|Resource Manager-sjabloon|Behoud van logboeken in dagen|Het bewaren van gegevens in dagen. Standaardwaarde is _365_.|
|Key Vault-sjabloon|Resource Manager-sjabloon|Key Vault-SKU|Hiermee geeft u de SKU van de Sleutelkluis die is gemaakt. Standaardwaarde is _Premium_.|
|Key Vault-sjabloon|Resource Manager-sjabloon|Gebruikersnaam van Azure SQL Server-beheerder|De gebruikersnaam die wordt gebruikt voor toegang tot Azure SQL-Server. Moet overeenkomen met dezelfde waarde voor eigenschap in **Azure SQL Database-sjabloon**. Standaardwaarde is _sql-beheerder-gebruiker_.|
|Azure SQL Database-resourcegroep|Resourcegroep|Name|**Vergrendeld** -worden samengevoegd in de **organisatienaam** met `-workload-azsql-rg` uniek te maken de resourcegroep.|
|Azure SQL Database-resourcegroep|Resourcegroep|Locatie|**Vergrendeld** -de blauwdrukparameter wordt gebruikt.|
|Azure SQL Database-sjabloon|Resource Manager-sjabloon|Gebruikersnaam van Azure SQL Server-beheerder|Gebruikersnaam voor de Azure SQL-Server. Moet overeenkomen met dezelfde waarde voor eigenschap in **Key Vault sjabloon**. Standaardwaarde is _sql-beheerder-gebruiker_.|
|Azure SQL Database-sjabloon|Resource Manager-sjabloon|Beheerderswachtwoord voor Azure SQL-Server (Key Vault Resource-ID)|De Resource-ID van de Key Vault. Gebruik ' / subscription/{subscriptionId}/resourceGroups/{orgName}-workload-kv/providers/Microsoft.KeyVault/vaults/{orgName}-workload-kv ' en vervang `{subscriptionId}` door uw abonnements-ID en `{orgName}` met de  **Naam van de organisatie** blauwdruk parameter.|
|Azure SQL Database-sjabloon|Resource Manager-sjabloon|Beheerderswachtwoord voor Azure SQL-Server (Key Vault-geheim naam)|Gebruikersnaam van de SQL Server-beheerder. Moet overeenkomen met de waarde in **Key Vault sjabloon** eigenschap **gebruikersnaam van de Azure SQL Server-beheerder**.|
|Azure SQL Database-sjabloon|Resource Manager-sjabloon|Behoud van logboeken in dagen|Het bewaren van gegevens in dagen. Standaardwaarde is _365_.|
|Azure SQL Database-sjabloon|Resource Manager-sjabloon|Object-ID voor AAD-beheerder|AAD-object-ID van de gebruiker die wordt toegewezen als een Active Directory-beheerder. Er is geen standaard waarde en mag niet leeg. U vindt deze waarde uit de Azure portal door zoek en selecteer 'Gebruikers' onder _Services_. Gebruik de _naam_ in om te filteren op naam van de account en selecteert u dat account. Op de _gebruikersprofiel_ pagina, selecteer het pictogram 'Klikken om te kopiëren' bij de _Object-ID_.|
|Azure SQL Database-sjabloon|Resource Manager-sjabloon|Beheerdersaanmelding bij de AAD|Op dit moment kunnen niet Microsoft-accounts (zoals live.com of outlook.com) worden ingesteld als beheerder. Alleen gebruikers en beveiligingsgroepen binnen uw organisatie kunnen worden ingesteld als beheerder. Er is geen standaard waarde en mag niet leeg. U vindt deze waarde uit de Azure portal door zoek en selecteer 'Gebruikers' onder _Services_. Gebruik de _naam_ in om te filteren op naam van de account en selecteert u dat account. Op de _gebruikersprofiel_ pagina-, Kopieer de _gebruikersnaam_.|
|Resourcegroep voor App Service-omgeving|Resourcegroep|Name|**Vergrendeld** -worden samengevoegd in de **organisatienaam** met `-workload-ase-rg` uniek te maken de resourcegroep.|
|Resourcegroep voor App Service-omgeving|Resourcegroep|Locatie|**Vergrendeld** -de blauwdrukparameter wordt gebruikt.|
|Sjabloon voor App Service Environment|Resource Manager-sjabloon|Domeinnaam|De naam van de Active Directory die zijn gemaakt door het voorbeeld. Standaardwaarde is _contoso.com_.|
|Sjabloon voor App Service Environment|Resource Manager-sjabloon|Locatie van de as-omgeving|Locatie van App Service-omgeving. Standaardwaarde is _VS-West 2_.|
|Sjabloon voor App Service Environment|Resource Manager-sjabloon|Behoud van Application Gateway-logboeken in dagen|Het bewaren van gegevens in dagen. Standaardwaarde is _365_.|

## <a name="next-steps"></a>Volgende stappen

Nu dat u de stappen voor het implementeren van de ISO 27001-App Service-omgeving/SQL Database-workload blauwdruk voorbeeld hebt bekeken, gaat u naar de volgende artikelen voor meer informatie over de architectuur en het besturingselement toewijzing:

> [!div class="nextstepaction"]
> [ISO 27001-App Service-omgeving/SQL Database-workload blauwdruk - overzicht](./index.md)
> [ISO 27001-App Service-omgeving/SQL Database-workload blauwdruk - besturingselement toewijzing](./control-mapping.md)

Aanvullende artikelen over blauwdrukken en het gebruik hiervan:

- Meer informatie over de [levenscyclus van een blauwdruk](../../concepts/lifecycle.md).
- Meer informatie over hoe u [statische en dynamische parameters](../../concepts/parameters.md) gebruikt.
- Meer informatie over hoe u de [blauwdrukvolgorde](../../concepts/sequencing-order.md) aanpast.
- Meer informatie over hoe u gebruikmaakt van [resourcevergrendeling in blauwdrukken](../../concepts/resource-locking.md).
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](../../how-to/update-existing-assignments.md).