---
title: Testen van de definitie van de gebruikersinterface voor Azure Managed Applications | Microsoft Docs
description: Beschrijft hoe u voor het testen van de gebruikerservaring voor het maken van uw Azure beheerde toepassingen via de portal.
author: tfitzmac
ms.service: managed-applications
ms.topic: conceptual
ms.date: 05/26/2019
ms.author: tomfitz
ms.openlocfilehash: 99ca319910be2cb20214172826eb40361abe72f0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66257605"
---
# <a name="test-your-portal-interface-for-azure-managed-applications"></a>Testen van de interface van de portal voor Azure Managed Applications

Na [maken van het bestand createUiDefinition.json](create-uidefinition-overview.md) voor uw beheerde toepassing, moet u voor het testen van de gebruikerservaring. Gebruik ter vereenvoudiging van testen van een sandbox-omgeving die uw bestand in de portal wordt geladen. U hoeft niet te uw beheerde toepassing daadwerkelijk te implementeren. De sandbox geeft de gebruikersinterface in de huidige, volledig scherm portal-ervaring. Of u kunt een PowerShell-script gebruiken voor het testen van de interface, maar maakt gebruik van een verouderde weergave van de portal. Beide methoden worden weergegeven in dit artikel. De sandbox is de aanbevolen manier om een voorbeeld van de interface.

## <a name="prerequisites"></a>Vereisten

* Een **createUiDefinition.json** bestand. Als u dit bestand hebt, kopieert u de [voorbeeldbestand](https://github.com/Azure/azure-quickstart-templates/blob/master/100-marketplace-sample/createUiDefinition.json).

* Een Azure-abonnement. Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="use-sandbox"></a>Gebruik sandbox

1. Open de [maken van de gebruikersinterface van de definitie van Sandbox](https://portal.azure.com/?feature.customPortal=false&#blade/Microsoft_Azure_CreateUIDef/SandboxBlade).

   ![Sandbox weergeven](./media/test-createuidefinition/show-sandbox.png)

1. De definitie van de lege vervangen door de inhoud van uw createUiDefinition.json-bestand. Selecteer **Preview**.

   ![Selecteer voorbeeld](./media/test-createuidefinition/select-preview.png)

1. Het formulier dat u hebt gemaakt wordt weergegeven. U kunt de gebruikerservaring kunt doorlopen en vul de waarden.

   ![Formulier weergeven](./media/test-createuidefinition/show-ui-form.png)

### <a name="troubleshooting"></a>Problemen oplossen

Als uw formulier niet wordt weergegeven na het selecteren van **Preview**, moet u wellicht een syntaxisfout. Zoek naar de rode indicator op de juiste schuifbalk en navigeer naar het.

![Syntaxisfout weergeven](./media/test-createuidefinition/show-syntax-error.png)

Als uw formulier niet wordt weergegeven en in plaats daarvan u een pictogram van een cloud met tear neerzetten ziet, heeft uw formulier een fout, zoals een eigenschap ontbreekt. De Web-hulpprogramma's voor ontwikkelaars in uw browser openen. De **Console** belangrijke berichten over de interface worden weergegeven.

![Fout weergeven](./media/test-createuidefinition/show-error.png)

## <a name="use-test-script"></a>Testscript gebruiken

Als u wilt testen uw interface in de portal, een van de volgende scripts te kopiëren naar uw lokale computer:

* [Sideloading-PowerShell-script](https://github.com/Azure/azure-quickstart-templates/blob/master/SideLoad-CreateUIDefinition.ps1)
* [Side-Loading van Azure CLI-script](https://github.com/Azure/azure-quickstart-templates/blob/master/sideload-createuidef.sh)

Als u wilt zien van de gebruikersinterface in een bestand in de portal, het gedownloade script worden uitgevoerd. Het script maakt een opslagaccount in uw Azure-abonnement en uw createUiDefinition.json-bestand wordt geüpload naar het opslagaccount. Het opslagaccount dat de eerste keer dat u het script uitvoert wordt gemaakt of als het opslagaccount is verwijderd. Als het opslagaccount al in uw Azure-abonnement bestaat, wordt het door het script hergebruikt. Het script wordt geopend de portal en uw bestand wordt geladen vanuit de storage-account.

Geef een locatie voor het opslagaccount en de map met uw bestand createUiDefinition.json opgeven.

Gebruik voor PowerShell:

```powershell
.\SideLoad-CreateUIDefinition.ps1 `
  -StorageResourceGroupLocation southcentralus `
  -ArtifactsStagingDirectory .\100-Marketplace-Sample
```

Gebruik voor Azure CLI:

```azurecli
./sideload-createuidef.sh \
  -l southcentralus \
  -a .\100-Marketplace-Sample
```

Als uw createUiDefinition.json-bestand in dezelfde map als het script, en u het opslagaccount al hebt gemaakt, moet u niet voor deze parameters.

Gebruik voor PowerShell:

```powershell
.\SideLoad-CreateUIDefinition.ps1
```

Gebruik voor Azure CLI:

```azurecli
./sideload-createuidef.sh
```

Het script wordt een nieuw tabblad geopend in uw browser. De portal met de interface voor het maken van de beheerde toepassing wordt weergegeven.

![Portal weergeven](./media/test-createuidefinition/view-portal.png)

Geef waarden op voor de velden. Wanneer u klaar bent, ziet u de waarden die worden doorgegeven aan de sjabloon.

![Waarden weergeven](./media/test-createuidefinition/show-json.png)

U kunt deze waarden gebruiken als de parameter-bestand voor het testen van uw sjabloon voor de implementatie.

Als de portal loopt op het scherm Samenvatting vast, kan dit worden veroorzaakt door een bug in de sectie uitvoer. Bijvoorbeeld, u mogelijk verwijst naar een besturingselement dat niet bestaat. Als een parameter in de uitvoer leeg is, de parameter kan worden verwezen naar een eigenschap die niet bestaat. Bijvoorbeeld, de verwijzing naar het besturingselement is geldig, maar de eigenschapverwijzing is niet geldig.

## <a name="test-your-solution-files"></a>De oplossingsbestanden van uw testen

Nu dat u hebt gecontroleerd dat de interface van de portal werkt zoals verwacht, is het tijd om te valideren dat uw createUiDefinition-bestand correct is geïntegreerd met uw mainTemplate.json-bestand. U kunt uitvoeren met een validatiescript testen de inhoud van de oplossingsbestanden van uw, inclusief het createUiDefinition-bestand. Het script valideert de JSON-syntaxis, voor regex-expressies op tekstvelden controleert en zorgt ervoor dat de uitvoerwaarden van de interface van de portal overeenkomen met de parameters van uw sjabloon. Zie voor meer informatie over het uitvoeren van dit script [statische validatiecontroles voor sjablonen uitvoeren](https://github.com/Azure/azure-quickstart-templates/tree/master/test/template-validation-tests).

## <a name="next-steps"></a>Volgende stappen

Nadat de interface van de portal wilt valideren, meer informatie over het maken van uw [Azure beheerde toepassing die beschikbaar zijn in de Marketplace](publish-marketplace-app.md).
