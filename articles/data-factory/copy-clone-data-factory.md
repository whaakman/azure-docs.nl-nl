---
title: Kopiëren of klonen van een data factory in Azure Data Factory | Microsoft Docs
description: Meer informatie over het kopiëren of klonen van een data factory in Azure Data Factory
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: douglasl
ms.openlocfilehash: a1071bb690fc5714890116c757c3036916bf1700
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57314092"
---
# <a name="copy-or-clone-a-data-factory-in-azure-data-factory"></a>Kopiëren of klonen van een data factory in Azure Data Factory

In dit artikel wordt beschreven hoe u kopiëren of klonen van een data factory in Azure Data Factory.

## <a name="use-cases-for-cloning-a-data-factory"></a>Gebruiksvoorbeelden voor het klonen van een data factory

Hier volgen enkele van de omstandigheden waarin u wellicht vindt u het handig om te kopiëren of klonen van een data factory:

-   **Naam van resources**. Azure biedt geen ondersteuning voor wijzigen van resources. Als u wijzigen van een data factory wilt, kunt u klonen van de data factory met een andere naam en verwijder vervolgens het bestaande bestand.

-   **Foutopsporing wijzigingen** wanneer de foutopsporing-functies zijn niet voldoende. Soms om uw wijzigingen hebt getest, kunt u uw wijzigingen in een andere factory testen voordat u deze toepast op uw belangrijkste weergavenaam. In de meeste gevallen kunt u foutopsporing. Wijzigingen in de triggers, zoals de werking van uw wijzigingen op als een trigger wordt automatisch gestart, of gedurende een bepaalde periode, mogelijk niet testable eenvoudig zonder in te checken. In dergelijke gevallen vormen de factory klonen en het toepassen van uw wijzigingen er een logische combinatie. Omdat Azure Data Factory voornamelijk gekenmerkt door het aantal uitvoeringen kosten, leidt de tweede factory niet tot extra kosten in rekening gebracht.

## <a name="how-to-clone-a-data-factory"></a>Het klonen van een data factory

1. De gebruikersinterface van Data Factory in Azure portal kunt u de volledige nettolading van uw data factory in Resource Manager-sjabloon, samen met een parameterbestand dat u alle waarden die u wijzigen kunt wilt wanneer u uw factory kloont exporteren.

1. Als een vereiste moet u uw doel data factory maken vanuit Azure portal.

1. Als u een SelfHosted IntegrationRuntime in uw bron-factory hebt, moet u vooraf te maken met dezelfde naam in de fabriek doel. Als u delen van de IRs zelfgehoste tussen verschillende factory's wilt, kunt u het patroon gepubliceerd [hier](author-visually.md#best-practices-for-git-integration).

1. Als u in de GIT-modus, telkens wanneer u vanuit de portal publiceren, worden de fabrieksinstellingen van Resource Manager-sjabloon wordt opgeslagen in GIT in de vertakking adf_publish van de opslagplaats.

1. Voor andere scenario's, het Resource Manager-sjabloon kan worden gedownload door te klikken op de **Resource Manager-sjabloon exporteren** knop in de portal.

1. Nadat u de Resource Manager-sjabloon hebt gedownload, kunt u deze kunt implementeren via standaard implementatiemethoden voor Resource Manager-sjabloon.

1. Uit veiligheidsoverwegingen bevat de gegenereerde Resource Manager-sjabloon geen een geheime informatie, zoals wachtwoorden voor de gekoppelde services. Als gevolg hiervan moet u opgeven van deze wachtwoorden als implementatieparameters. Als parameters geven niet gewenst is, hebt u de verbindingsreeksen en wachtwoorden van de gekoppelde services ophalen van de Azure Key Vault.

## <a name="next-steps"></a>Volgende stappen

Bekijk de gids voor het maken van een data factory in Azure portal in [een data factory maken met behulp van de Azure Data Factory-UI](quickstart-create-data-factory-portal.md).
