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
ms.date: 06/27/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: b73deebb10d0c81a06df9cd192eaa2ef28de744d
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37083036"
---
# <a name="remove-the-sql-resource-provider"></a>Verwijderen van de SQL-resourceprovider

Voordat u de SQL-resourceprovider verwijdert, moet u alle afhankelijkheden van de provider verwijderen. U moet ook een kopie van het implementatiepakket dat is gebruikt voor het installeren van de resourceprovider.

Er zijn verschillende Opschoningstaken doen voordat u de _DeploySqlProvider.ps1_ script voor het verwijderen van de resourceprovider.
De tenants zijn die verantwoordelijk is voor de volgende Opschoningstaken:

* Verwijder alle hun databases van de resourceprovider. (De tenant-databases niet verwijderen van de gegevens.)
* Hef de registratie van de naamruimte van de resource provider.

De beheerder is verantwoordelijk voor de volgende Opschoningstaken:

* Hiermee verwijdert u de hosting-servers van de SQL-resourceprovider.
* Hiermee verwijdert u de schema's die verwijzen naar de SQL-resourceprovider.
* Hiermee verwijdert u geen quota's die gekoppeld aan de SQL-resourceprovider zijn.

## <a name="to-remove-the-sql-resource-provider"></a>Verwijderen van de SQL-resourceprovider

1. Controleer of u alle bestaande SQL resource provider afhankelijkheden hebt verwijderd.

   > [!NOTE]
   > Verwijderen van de SQL-resourceprovider wordt voortgezet zelfs als afhankelijke resources de resourceprovider momenteel gebruikt.
  
2. Een kopie van de SQL-resourceprovider binaire krijgen en voer vervolgens de zelfstandig uitpakken om de inhoud naar een tijdelijke map te pakken.

3. Open een nieuw verhoogde PowerShell-console-venster en Ga naar de map waar u de binaire bestanden voor SQL resource provider hebt uitgepakt.

4. Voer het script DeploySqlProvider.ps1 is met de volgende parameters:

    * **Verwijder**. Hiermee verwijdert u de resourceprovider en alle bijbehorende resources.
    * **PrivilegedEndpoint**. De IP-adres of de DNS-naam van het bevoegde eindpunt.
    * **CloudAdminCredential**. De referentie voor de beheerder van de cloud, moet u toegang tot de bevoegde eindpunt.
    * **AzCredential**. De referentie voor de Azure-Stack-admin-serviceaccount. Gebruik dezelfde referenties die u gebruikt voor de implementatie van Azure-Stack.

## <a name="next-steps"></a>Volgende stappen

[App-Services bieden als PaaS](azure-stack-app-service-overview.md)
