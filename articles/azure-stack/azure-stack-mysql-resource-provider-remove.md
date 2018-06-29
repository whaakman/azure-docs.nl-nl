---
title: Verwijderen van de resourceprovider MySQL op Azure-Stack | Microsoft Docs
description: Meer informatie over hoe u de MySQL-resourceprovider van uw Azure-Stack-implementatie kunt verwijderen.
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
ms.date: 06/27/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: d3a615e3b92a62709a787d0463dfa3148f14d07e
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37085794"
---
# <a name="remove-the-mysql-resource-provider"></a>Verwijder de MySQL-resourceprovider

Voordat u de MySQL-resourceprovider verwijdert, moet u alle afhankelijkheden van de provider verwijderen. U moet ook een kopie van het implementatiepakket dat is gebruikt voor het installeren van de resourceprovider.

## <a name="dependency-cleanup"></a>Afhankelijkheid opschonen

Er zijn verschillende Opschoningstaken moet doen voordat u de DeployMySqlProvider.ps1 script uitvoeren om te verwijderen van de resourceprovider.

De tenants zijn die verantwoordelijk is voor de volgende Opschoningstaken:

* Verwijder alle hun databases van de resourceprovider. (De tenant-databases niet verwijderen van de gegevens.)
* Registratie verwijderen uit de naamruimte van de provider.

De beheerder is verantwoordelijk voor de volgende Opschoningstaken:

* Hiermee verwijdert u de hosting-servers van de MySQL-Adapter.
* Hiermee verwijdert u de schema's die verwijzen naar de MySQL-Adapter.
* Hiermee verwijdert u geen quota's die gekoppeld aan de MySQL-Adapter zijn.

## <a name="to-remove-the-mysql-resource-provider"></a>Verwijderen van de MySQL-resourceprovider

1. Controleer of u alle bestaande MySQL resource provider afhankelijkheden hebt verwijderd.

   >[!NOTE]
   >Verwijderen van de MySQL-resourceprovider wordt voortgezet zelfs als afhankelijke resources de resourceprovider momenteel gebruikt.
  
2. Een kopie van de resourceprovider MySQL binaire krijgen en voer vervolgens de zelfstandig uitpakken om de inhoud naar een tijdelijke map te pakken.
3. Een kopie van de SQL-resourceprovider binaire krijgen en voer vervolgens de zelfstandig uitpakken om de inhoud naar een tijdelijke map te pakken.
4. Open een nieuw verhoogde PowerShell-console-venster en Ga naar de map waar u de binaire bestanden van MySQL resource provider hebt uitgepakt.
5. Voer het script DeployMySqlProvider.ps1 is met de volgende parameters:
    - **Verwijder**. Hiermee verwijdert u de resourceprovider en alle bijbehorende resources.
    - **PrivilegedEndpoint**. De IP-adres of de DNS-naam van het bevoegde eindpunt.
    - **CloudAdminCredential**. De referentie voor de beheerder van de cloud, moet u toegang tot de bevoegde eindpunt.
    - **DirectoryTenantID**
    - **AzCredential**. De referentie voor de Azure-Stack-admin-serviceaccount. Gebruik dezelfde referenties die u gebruikt voor de implementatie van Azure-Stack.

## <a name="next-steps"></a>Volgende stappen

[App-Services bieden als PaaS](azure-stack-app-service-overview.md)
