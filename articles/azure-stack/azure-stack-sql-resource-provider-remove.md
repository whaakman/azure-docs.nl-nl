---
title: Verwijderen van de SQL-resourceprovider in Azure Stack | Microsoft Docs
description: Leer hoe u de SQL-resourceprovider verwijderen uit uw Azure Stack-implementatie.
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
ms.date: 11/14/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: b7af23ccdd379aac9959bb9993fc1781a44e705e
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51684023"
---
# <a name="remove-the-sql-resource-provider"></a>Verwijderen van de SQL-resourceprovider

Voordat u de SQL-resourceprovider verwijdert, moet u alle afhankelijkheden van de provider verwijderen. U moet ook een kopie van het implementatiepakket dat is gebruikt voor het installeren van de resourceprovider.

  |Minimale versie van Azure Stack|SQL RP-versie|
  |-----|-----|
  |Versie 1808 (1.1808.0.97)|[SQL RP versie 1.1.30.0](https://aka.ms/azurestacksqlrp11300)|
  |Versie 1804 (1.0.180513.1)|[SQL RP versie 1.1.24.0](https://aka.ms/azurestacksqlrp11240)
  |     |     |

## <a name="dependency-cleanup"></a>Opschonen van de afhankelijkheid

Er zijn verschillende Opschoningstaken moet doen voordat u het script DeploySqlProvider.ps1 als u wilt verwijderen van de resourceprovider uitvoert.

Azure Stack-tenantgebruikers zijn verantwoordelijk voor de volgende Opschoningstaken:

* Verwijder alle hun databases uit de resourceprovider. (Als het verwijderen van de tenant-databases, niet de gegevens verwijderd.)
* De registratie ongedaan maken van de naamruimte van de provider.

De Azure Stack-Operator is verantwoordelijk voor de volgende Opschoningstaken:

* Hiermee verwijdert de host-servers uit de MySQL-Adapter.
* Hiermee verwijdert u de schema's die verwijzen naar de MySQL-Adapter.
* Hiermee verwijdert u geen quota's die gekoppeld aan de MySQL-Adapter zijn.

## <a name="to-remove-the-sql-resource-provider"></a>Verwijderen van de SQL-resourceprovider

1. Controleer of u alle bestaande SQL-resource provider afhankelijkheden hebt verwijderd.

   > [!NOTE]
   > Verwijderen van de SQL-resourceprovider wordt voortgezet zelfs als afhankelijke resources zijn op dit moment met behulp van de resourceprovider.
  
2. Ontvang een kopie van de SQL-resourceprovider binaire en voer vervolgens de zelfstandige extractor om de inhoud uitpakken naar een tijdelijke map.

3. Open een nieuw verhoogde PowerShell-console-venster en Ga naar de map waar u de SQL-resource provider binaire bestanden hebt uitgepakt.

4. Voer het script DeploySqlProvider.ps1 is met de volgende parameters:

    * **Verwijder**. Hiermee verwijdert u de resourceprovider en alle bijbehorende resources.
    * **PrivilegedEndpoint**. De IP-adres of de DNS-naam van het eindpunt van de bevoegdheden.
    * **Azure-omgeving**. De Azure-omgeving gebruikt voor het implementeren van Azure Stack. Alleen vereist voor Azure AD-implementaties.
    * **CloudAdminCredential**. De referentie voor de beheerder van de cloud, die nodig zijn voor toegang tot de bevoorrechte eindpunt.
    * **AzCredential**. De referentie voor de beheerdersaccount van de Azure Stack-service. Gebruik de dezelfde referenties die u gebruikt voor het implementeren van Azure Stack.

## <a name="next-steps"></a>Volgende stappen

[App-Services als PaaS aanbieden](azure-stack-app-service-overview.md)
