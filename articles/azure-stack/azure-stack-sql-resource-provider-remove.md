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
ms.date: 09/13/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 42d8a5a8073d2650b9e023305472f28d4f1c738f
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45580096"
---
# <a name="remove-the-sql-resource-provider"></a>Verwijderen van de SQL-resourceprovider

Voordat u de SQL-resourceprovider verwijdert, moet u alle afhankelijkheden van de provider verwijderen. U moet ook een kopie van het implementatiepakket dat is gebruikt voor het installeren van de resourceprovider.

Er zijn verschillende opschoningstaken uitvoeren voordat u de _DeploySqlProvider.ps1_ script voor het verwijderen van de resourceprovider.
De tenants zijn verantwoordelijk voor de volgende Opschoningstaken:

* Verwijder alle hun databases uit de resourceprovider. (Als het verwijderen van de tenant-databases, niet de gegevens verwijderd.)
* De registratie ongedaan maken van de naamruimte van de resource provider.

De beheerder is verantwoordelijk voor de volgende Opschoningstaken:

* Hiermee verwijdert de host-servers uit de SQL-resourceprovider.
* Hiermee verwijdert u de schema's die verwijzen naar de SQL-resourceprovider.
* Hiermee verwijdert u geen quota's die gekoppeld aan de SQL-resourceprovider zijn.

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
