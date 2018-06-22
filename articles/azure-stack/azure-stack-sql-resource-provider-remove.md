---
title: Verwijderen van de SQL-resourceprovider op Azure-Stack | Microsoft Docs
description: Meer informatie over hoe u de SQL-resourceprovider verwijderen van uw Azure-Stack-implementatie.
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
ms.date: 06/20/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 150d1c40463aa04527bdd6e356a4c24ef68b02ef
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36301895"
---
# <a name="remove-the-sql-resource-provider"></a>Verwijderen van de SQL-resourceprovider

Voordat u de SQL-resourceprovider verwijdert, moet u alle afhankelijkheden van de provider verwijderen. U moet ook een kopie van het implementatiepakket dat is gebruikt voor het installeren van de resourceprovider.

## <a name="to-remove-the-sql-resource-provider"></a>Verwijderen van de SQL-resourceprovider

1. Controleer of u alle bestaande SQL resource provider afhankelijkheden hebt verwijderd.

   > [!NOTE]
   > Verwijderen van de SQL-resourceprovider wordt voortgezet zelfs als afhankelijke resources de resourceprovider momenteel gebruikt.
  
2. Een kopie van de SQL-resourceprovider binaire krijgen en voer vervolgens de zelfstandig uitpakken om de inhoud naar een tijdelijke map te pakken.

3. Open een nieuw verhoogde PowerShell-console-venster en Ga naar de map waar u de binaire bestanden voor SQL resource provider hebt uitgepakt.

4. Voer het script DeploySqlProvider.ps1 is met de volgende parameters:

    - **Verwijder**. Hiermee verwijdert u de resourceprovider en alle bijbehorende resources.
    - **PrivilegedEndpoint**. De IP-adres of de DNS-naam van het bevoegde eindpunt.
    - **CloudAdminCredential**. De referentie voor de beheerder van de cloud, moet u toegang tot de bevoegde eindpunt.
    - **AzCredential**. De referentie voor de Azure-Stack-admin-serviceaccount. Gebruik dezelfde referenties die u gebruikt voor de implementatie van Azure-Stack.

## <a name="next-steps"></a>Volgende stappen

[App-Services bieden als PaaS](azure-stack-app-service-overview.md)
