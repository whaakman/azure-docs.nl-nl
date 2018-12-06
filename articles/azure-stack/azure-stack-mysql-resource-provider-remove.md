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
ms.date: 11/20/2018
ms.author: jeffgilb
ms.reviewer: quying
ms.openlocfilehash: 2740da5a51e95a327a868734a7f009dddf40219a
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52964931"
---
# <a name="remove-the-mysql-resource-provider"></a>Verwijderen van de MySQL-resourceprovider

Voordat u de MySQL-resourceprovider verwijdert, moet u alle afhankelijkheden van de provider verwijderen. U moet ook een kopie van het implementatiepakket dat is gebruikt voor het installeren van de resourceprovider.

> [!NOTE]
> U vindt de downloadkoppelingen voor de resource provider-installatieprogramma's in [implementeert de vereisten van de provider resource](./azure-stack-mysql-resource-provider-deploy.md#prerequisites).

Verwijderen van de MySQL-resourceprovider wordt niet tenant-databases verwijderd van de servers die als host fungeert.

## <a name="dependency-cleanup"></a>Opschonen van de afhankelijkheid

Er zijn verschillende Opschoningstaken moet doen voordat u het script DeployMySqlProvider.ps1 als u wilt verwijderen van de resourceprovider uitvoert.

De Azure Stack-Operator is verantwoordelijk voor de volgende Opschoningstaken:

* Verwijder de schema's die verwijzen naar de MySQL-Adapter.
* Quota's die gekoppeld aan de MySQL-Adapter zijn verwijderen.

## <a name="to-remove-the-mysql-resource-provider"></a>Verwijderen van de MySQL-resourceprovider

1. Controleer of u alle bestaande MySQL resource provider afhankelijkheden hebt verwijderd.

   > [!NOTE]
   > Verwijderen van de MySQL-resourceprovider wordt voortgezet zelfs als afhankelijke resources zijn op dit moment met behulp van de resourceprovider.
  
2. Een kopie van het installatiepakket van MySQL resource provider en voer vervolgens de zelfstandige extractor om de inhoud uitpakken naar een tijdelijke map.
3. Open een nieuw verhoogde PowerShell-console-venster en Ga naar de map waar u de installatiebestanden van MySQL resource provider hebt uitgepakt.
4. Voer het script DeployMySqlProvider.ps1 is met de volgende parameters:
    - **Verwijder**. Hiermee verwijdert u de resourceprovider en alle bijbehorende resources.
    - **PrivilegedEndpoint**. De IP-adres of de DNS-naam van het eindpunt van de bevoegdheden.
    - **Azure-omgeving**. De Azure-omgeving gebruikt voor het implementeren van Azure Stack. Alleen vereist voor Azure AD-implementaties.
    - **CloudAdminCredential**. De referentie voor de beheerder van de cloud, die nodig zijn voor toegang tot de bevoorrechte eindpunt.
    - **DirectoryTenantID**
    - **AzCredential**. De referentie voor de beheerdersaccount van de Azure Stack-service. Gebruik de dezelfde referenties die u gebruikt voor het implementeren van Azure Stack.

## <a name="next-steps"></a>Volgende stappen

[App-Services als PaaS aanbieden](azure-stack-app-service-overview.md)
