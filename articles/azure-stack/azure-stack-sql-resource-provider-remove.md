---
title: Verwijderen van de SQL-resourceprovider op Azure-Stack | Microsoft Docs
description: Meer informatie over hoe u de SQL-resourceprovider van uw Azure-Stack-implementatie kunt verwijderen.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 9f90201cad0f74923460c2f25eff4de98dc6690a
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294777"
---
# <a name="removing-the-mysql-resource-provider"></a>Verwijderen van de MySQL-resourceprovider  
Voordat u de SQL-resourceprovider verwijdert, is het essentieel voor het eerst alle afhankelijkheden te verwijderen.

## <a name="remove-the-mysql-resource-provider"></a>Verwijder de MySQL-resourceprovider 

1. Controleer of u een bestaande SQL-provider bronafhankelijkheden hebt verwijderd.

  > [!NOTE]
  > Verwijderen van de SQL-resourceprovider wordt voortgezet zelfs als afhankelijke resources de resourceprovider momenteel gebruikt. 
  
2. Zorg ervoor dat u het oorspronkelijke implementatiepakket dat u voor deze versie van de adapter SQL resource provider hebt gedownload.
3. Opnieuw uit het script voor implementatie met de volgende parameters:
    - Gebruik de - parameter verwijderen
    - De IP-adres of de DNS-naam van het bevoegde eindpunt.
    - De referentie voor de beheerder van de cloud, nodig voor toegang tot de bevoegde eindpunt.
    - De referenties voor de Azure-Stack-admin-serviceaccount. Dezelfde referenties gebruiken dat u gebruikt voor het implementeren van Azure-Stack.

## <a name="next-steps"></a>Volgende stappen
[App-Services bieden als PaaS](azure-stack-app-service-overview.md)
