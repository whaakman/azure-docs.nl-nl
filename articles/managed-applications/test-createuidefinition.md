---
title: Testen van de definitie van de gebruikersinterface voor Azure Managed Applications | Microsoft Docs
description: Beschrijft hoe u voor het testen van de gebruikerservaring voor het maken van uw Azure beheerde toepassingen via de portal.
services: managed-applications
documentationcenter: na
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/10/2018
ms.author: tomfitz
ms.openlocfilehash: 2c313538e297c5781b48fcfe9d0d5390f94c97f5
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/11/2018
ms.locfileid: "40043765"
---
# <a name="test-azure-portal-interface-for-your-managed-application"></a>Interface van de Azure portal voor uw beheerde toepassing testen
Na [maken van het bestand createUiDefinition.json](create-uidefinition-overview.md) voor uw Azure beheerde toepassingen, moet u voor het testen van de gebruikerservaring. Gebruik ter vereenvoudiging van testen van een script dat uw bestand in de portal wordt geladen. U hoeft niet te uw beheerde toepassing daadwerkelijk te implementeren.

## <a name="prerequisites"></a>Vereisten

* Een **createUiDefinition.json** bestand. Als u dit bestand hebt, kopieert u de [voorbeeldbestand](https://github.com/Azure/azure-quickstart-templates/blob/master/test/template-validation-tests/sample-template/createUIDefinition.json) en lokaal opslaan.

* Een Azure-abonnement. Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="download-test-script"></a>Testscript downloaden

Als u wilt testen uw interface in de portal, een van de volgende scripts te kopiëren naar uw lokale computer:

* [Sideloading-PowerShell-script](https://github.com/Azure/azure-quickstart-templates/blob/master/SideLoad-CreateUIDefinition.ps1)
* [Side-Loading van Azure CLI-script](https://github.com/Azure/azure-quickstart-templates/blob/master/sideload-createuidef.sh)

## <a name="run-script"></a>Script uitvoeren

Als u wilt zien van de gebruikersinterface in een bestand in de portal, het gedownloade script worden uitgevoerd. Het script maakt een opslagaccount in uw Azure-abonnement en uw createUiDefinition.json-bestand wordt geüpload naar het opslagaccount. Vervolgens wordt de portal wordt geopend en wordt het bestand geladen vanuit de storage-account.

Geef een locatie voor het opslagaccount en de map met uw bestand createUiDefinition.json opgeven. U hoeft alleen te bieden van de storage-account locatie van de eerste keer dat u het script uitvoert of als het opslagaccount is verwijderd.

Gebruik voor PowerShell:

```powershell
.\SideLoad-CreateUIDefinition.ps1 `
  -StorageResourceGroupLocation southcentralus `
  -ArtifactsStagingDirectory <path-to-folder-with-createuidefinition>
```

Gebruik voor Azure CLI:

```azurecli
./sideload-createuidef.sh \
  -l southcentralus \
  -a <path-to-folder-with-createuidefinition>
```

## <a name="test-your-interface"></a>Test uw interface

Het script wordt een nieuw tabblad geopend in uw browser. De portal met de interface voor het maken van de beheerde toepassing wordt weergegeven.

![Portal weergeven](./media/test-createuidefinition/view-portal.png)

Voordat u de velden invult, de Web-hulpprogramma's voor ontwikkelaars in uw browser te openen. De **Console** belangrijke berichten over de interface worden weergegeven.

![Selecteer de console](./media/test-createuidefinition/select-console.png)

Als de interfacedefinitie van de een fout opgetreden heeft, ziet u de beschrijving in de console.

![Fout weergeven](./media/test-createuidefinition/show-error.png)

Geef waarden op voor de velden. Wanneer u klaar bent, ziet u de waarden die worden doorgegeven aan de sjabloon.

![Waarden weergeven](./media/test-createuidefinition/show-json.png)

## <a name="test-your-solution-files"></a>De oplossingsbestanden van uw testen

Nu dat u hebt gecontroleerd dat de interface van de portal werkt zoals verwacht, is het tijd om te valideren dat uw createUiDefinition-bestand correct is geïntegreerd met uw mainTemplate.json-bestand. U kunt uitvoeren met een validatiescript testen de inhoud van de oplossingsbestanden van uw, inclusief het createUiDefinition-bestand. Het script valideert de JSON-syntaxis, voor regex-expressies op tekstvelden controleert en zorgt ervoor dat de uitvoerwaarden van de interface van de portal overeenkomen met de parameters van uw sjabloon. Zie voor meer informatie over het uitvoeren van dit script [statische validatiecontroles voor sjablonen uitvoeren](https://github.com/Azure/azure-quickstart-templates/tree/master/test/template-validation-tests).

## <a name="next-steps"></a>Volgende stappen

Nadat de interface van de portal wilt valideren, meer informatie over het maken van uw [Azure beheerde toepassing die beschikbaar zijn in de Marketplace](publish-marketplace-app.md).
