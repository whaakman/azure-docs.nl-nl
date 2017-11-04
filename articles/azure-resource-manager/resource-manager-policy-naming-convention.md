---
title: Azure-resource-beleid voor naamconventies | Microsoft Docs
description: Beschrijving van het Azure Resource Manager-beleid voor de naamgeving van de resource.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/03/2017
ms.author: tomfitz
ms.openlocfilehash: 955b04517c3ccdbe530eb982e6aa1255b69e1edd
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="apply-resource-policies-for-names-and-text"></a>Resource-beleid toepassen op namen en tekst
In dit artikel bevat verschillende [bronbeleid](resource-manager-policy.md) u kunt toepassen om vast te stellen conventies naming en tekst. Deze beleidsregels consistentie voor resourcenamen en labelwaarden. 

## <a name="set-naming-convention-with-wildcard"></a>Stel naamconventie met jokerteken
Het volgende voorbeeld ziet u het gebruik van jokertekens, die wordt ondersteund door de **zoals** voorwaarde. De voorwaarde aangeeft dat als de naam komt overeen met de genoemde patroon (namePrefix\*nameSuffix) klikt u vervolgens de aanvraag weigeren:

```json
{
  "if": {
    "not": {
      "field": "name",
      "like": "namePrefix*nameSuffix"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-naming-convention-with-pattern"></a>Stel de naamgevingsconventie met patroon

Resourcenamen overeenkomen met een patroon gebruikt u de voorwaarde van de overeenkomst. Het volgende voorbeeld vereist namen beginnen met `contoso` en zes aanvullende letters bevatten:

```json
{
  "if": {
    "not": {
      "field": "name",
      "match": "contoso??????"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-date-pattern-for-tag-value"></a>Patroon voor de labelwaarde datum instellen

Als u wilt een patroon van de datum van het gebruik van twee cijfers, streepjes, drie letters, streepjes en vier cijfers vereisen:

```json
{
  "if": {
    "field": "tags.date",
    "match": "##-???-####"
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-multiple-naming-patterns"></a>Meerdere naming patronen instellen

Als u meer dan één naamconventie toegestaan, gebruiken de **zet** en **niet** operators. In het volgende voorbeeld als de opgegeven naam komt niet overeen met een patroon wordt geweigerd.

```json
{
    "if": {
        "allOf": [
            {
                "not": {
                    "field": "name",
                    "match": "contoso??????"
                }
            },
            {
                "not": {
                    "field": "name",
                    "match": "contoso-???-##"
                }
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

## <a name="next-steps"></a>Volgende stappen
* Na het definiëren van een beleidsregel (zoals weergegeven in de voorgaande voorbeelden), moet u de beleidsdefinitie maken en toewijzen aan een bereik. Het bereik mag een abonnement, resourcegroep of resource. Als u wilt toewijzen beleid via de portal, Zie [gebruik Azure-portal toewijzen en beheren van bronbeleid](resource-manager-policy-portal.md). Als u wilt toewijzen beleid via REST API, PowerShell of Azure CLI, Zie [toewijzen en beheren van beleid via script](resource-manager-policy-create-assign.md). 
* Voor begeleiding bij de manier waarop ondernemingen Resource Manager effectief kunnen gebruiken voor het beheer van abonnementen, gaat u naar [Azure enterprise-platform - Prescriptieve abonnementsgovernance](resource-manager-subscription-governance.md).

