---
title: Naslag informatie voor createUiDefinition-artefacten van Azure Managed Application
description: Dit artikel is een verwijzing naar createUiDefinition-artefact.
services: managed-applications
ms.service: managed-applications
ms.topic: conceptual
ms.author: lazinnat
author: lazinnat
ms.date: 07/11/2019
ms.openlocfilehash: 5168e977a7bc289ec8981ef1df8cddd5cc6df678
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68336476"
---
# <a name="reference-user-interface-elements-artifact"></a>Referentie: Artefact van elementen van gebruikers interface

Dit artikel bevat een verwijzing naar een *createUiDefinition. json* -artefact in azure Managed Applications. Zie elementen van de [gebruikers interface maken](create-uidefinition-elements.md)voor meer informatie over het ontwerpen van gebruikers interface-elementen.

## <a name="user-interface-elements"></a>Elementen van de gebruikersinterface

In de volgende JSON ziet u een voor beeld van het bestand *createUiDefinition. json* voor Azure Managed Applications:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
  "parameters": {
    "basics": [
      {}
    ],
    "steps": [
      {
        "name": "applicationSettings",
        "label": "Application Settings",
        "subLabel": {
          "preValidation": "Configure your application settings",
          "postValidation": "Done"
        },
        "bladeTitle": "Application Settings",
        "elements": [
          {
            "name": "funcname",
            "type": "Microsoft.Common.TextBox",
            "label": "Name of the function to be created",
            "toolTip": "Name of the function to be created",
            "visible": true,
            "constraints": {
              "required": true
            }
          },
          {
            "name": "storagename",
            "type": "Microsoft.Common.TextBox",
            "label": "Name of the storage to be created",
            "toolTip": "Name of the storage to be created",
            "visible": true,
            "constraints": {
              "required": true
            }
          },
          {
            "name": "zipFileBlobUri",
            "type": "Microsoft.Common.TextBox",
            "defaultValue": "https://github.com/Azure/azure-quickstart-templates/tree/master/101-custom-rp-with-function/artifacts/functionzip/functionpackage.zip",
            "label": "The Uri to the uploaded function zip file",
            "toolTip": "The Uri to the uploaded function zip file",
            "visible": true
          }
        ]
      }
    ],
    "outputs": {
      "funcname": "[steps('applicationSettings').funcname]",
      "storageName": "[steps('applicationSettings').storagename]",
      "zipFileBlobUri": "[steps('applicationSettings').zipFileBlobUri]"
    }
  }
}
```

## <a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Een beheerde toepassing maken met aangepaste acties en resources](tutorial-create-managed-app-with-custom-provider.md)
- [Referentielaag Artefact van implementatie sjabloon](reference-main-template-artifact.md)
- [Referentielaag Definitie artefact weer geven](reference-view-definition-artifact.md)