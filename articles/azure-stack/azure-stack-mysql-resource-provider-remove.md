---
title: Verwijderen van de resourceprovider van MySQL in Azure Stack | Microsoft Docs
description: Meer informatie over hoe u de MySQL-resourceprovider kunt verwijderen uit uw Azure Stack-implementatie.
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
ms.date: 10/16/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: dcd1c40717cb35fe4daa9ab9e2c66f334ffff5fe
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49361495"
---
# <a name="remove-the-mysql-resource-provider"></a>Verwijderen van de MySQL-resourceprovider

Voordat u de MySQL-resourceprovider verwijdert, moet u alle afhankelijkheden van de provider verwijderen. U moet ook een kopie van het implementatiepakket dat is gebruikt voor het installeren van de resourceprovider.

## <a name="dependency-cleanup"></a>Opschonen van de afhankelijkheid

Er zijn verschillende Opschoningstaken moet doen voordat u het script DeployMySqlProvider.ps1 als u wilt verwijderen van de resourceprovider uitvoert.

De tenants zijn verantwoordelijk voor de volgende Opschoningstaken:

* Verwijder alle hun databases uit de resourceprovider. (Als het verwijderen van de tenant-databases, niet de gegevens verwijderd.)
* De registratie ongedaan maken van de naamruimte van de provider.

De beheerder is verantwoordelijk voor de volgende Opschoningstaken:

* Hiermee verwijdert de host-servers uit de MySQL-Adapter.
* Hiermee verwijdert u de schema's die verwijzen naar de MySQL-Adapter.
* Hiermee verwijdert u geen quota's die gekoppeld aan de MySQL-Adapter zijn.

## <a name="to-remove-the-mysql-resource-provider"></a>Verwijderen van de MySQL-resourceprovider

1. Controleer of u alle bestaande MySQL resource provider afhankelijkheden hebt verwijderd.

   >[!NOTE]
   >Verwijderen van de MySQL-resourceprovider wordt voortgezet zelfs als afhankelijke resources zijn op dit moment met behulp van de resourceprovider.
  
2. Ontvang een kopie van de MySQL-resourceprovider binaire en voer vervolgens de zelfstandige extractor om de inhoud uitpakken naar een tijdelijke map.
3. Ontvang een kopie van de SQL-resourceprovider binaire en voer vervolgens de zelfstandige extractor om de inhoud uitpakken naar een tijdelijke map.
4. Open een nieuw verhoogde PowerShell-console-venster en Ga naar de map waar u de binaire bestanden voor MySQL resource provider hebt uitgepakt.
5. Voer het script DeployMySqlProvider.ps1 is met de volgende parameters:
    - **Verwijder**. Hiermee verwijdert u de resourceprovider en alle bijbehorende resources.
    - **PrivilegedEndpoint**. De IP-adres of de DNS-naam van het eindpunt van de bevoegdheden.
    - **Azure-omgeving**. De Azure-omgeving gebruikt voor het implementeren van Azure Stack. Alleen vereist voor Azure AD-implementaties.
    - **CloudAdminCredential**. De referentie voor de beheerder van de cloud, die nodig zijn voor toegang tot de bevoorrechte eindpunt.
    - **DirectoryTenantID**
    - **AzCredential**. De referentie voor de beheerdersaccount van de Azure Stack-service. Gebruik de dezelfde referenties die u gebruikt voor het implementeren van Azure Stack.

## <a name="next-steps"></a>Volgende stappen

[App-Services als PaaS aanbieden](azure-stack-app-service-overview.md)
