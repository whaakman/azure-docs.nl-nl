---
title: Omgevingen integreren met Azure-pijplijnen in Azure DevTest Labs | Microsoft Docs
description: Informatie over het integreren van Azure DevTest Labs-omgevingen in uw Azure DevOps, continue integratie (CI) en pijplijnen voor continue levering (CD).
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2019
ms.author: spelluru
ms.openlocfilehash: 24391e9e8541f12c434ade837b8f4944711ae375
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58887203"
---
# <a name="integrate-environments-into-your-azure-devops-cicd-pipelines"></a>Omgevingen integreren in uw Azure DevOps-CI/CD-pijplijnen
U kunt de extensie Azure DevTest Labs-taken die is geïnstalleerd in Azure DevOps-Services (voorheen bekend als Visual Studio Team Services) om eenvoudig te integreren uw continue integratie (CI) / continue levering (CD) build-en-release-pijplijn met Azure DevTest Labs. Deze extensies maken het gemakkelijker om snel te implementeren een [omgeving](devtest-lab-test-env.md) voor een specifieke taak testen en vervolgens te verwijderen wanneer de test is voltooid. 

In dit artikel laat zien hoe maken en implementeren van een omgeving, waarna de omgeving, allemaal op één volledige pijplijn verwijderen. U zou het elk van deze taken doorgaans afzonderlijk uitvoeren in uw eigen aangepaste pijplijn voor het build-testen-implementeren. De extensies die in dit artikel gebruikt zijn naast deze [DTL VM-taken maken/verwijderen](devtest-lab-integrate-ci-cd-vsts.md):

- Een omgeving maken
- Een omgeving verwijderen

## <a name="before-you-begin"></a>Voordat u begint
Voordat u uw CI/CD-pijplijn met Azure DevTest Labs integreren kunt, installeren [Azure DevTest Labs-taken](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) -extensie van Visual Studio Marketplace. 

## <a name="create-and-configure-the-lab-for-environments"></a>Maken en configureren van de testomgeving voor omgevingen
In deze sectie wordt beschreven hoe u maken en configureren van een lab waarin de Azure-omgeving om te worden geïmplementeerd.

1. [Een lab maken](devtest-lab-create-lab.md) als u er nog geen hebt. 
2. In het lab configureren en de sjabloon van een omgeving maken met de instructies in dit artikel: [Multi-VM-omgevingen en PaaS-resources maken met Azure Resource Manager-sjablonen](devtest-lab-create-environment-from-arm.md).
3. In dit voorbeeld gebruikt u een bestaande Azure-Snelstartsjabloon [ https://azure.microsoft.com/resources/templates/201-web-app-redis-cache-sql-database/ ](https://azure.microsoft.com/resources/templates/201-web-app-redis-cache-sql-database/).
4. Kopieer de **201-web-app-redis-cache-sql-database** map in de **ArmTemplate** map in de opslagplaats die in de stap 2 hebt geconfigureerd.

## <a name="create-a-release-definition"></a>Release-definitie maken
Voor het maken van de release-definitie, het volgende doen:

1.  Op de **Releases** tabblad van de **Build & Release hub**, selecteer de **plusteken (+)** knop.
2.  In de **release-definitie maken** venster de **leeg** sjabloon en selecteer vervolgens **volgende**.
3.  Selecteer **Kies Later**, en selecteer vervolgens **maken** een nieuwe release-definitie maken met een standaard-omgeving en er is geen gekoppelde artefacten.
4.  Selecteer het snelmenu openen in de versiedefinitie van de nieuwe, de **weglatingsteken (...)**  volgende op de omgevingsnaam van de en selecteer vervolgens **variabelen configureren**.
5.  In de **configureren - omgeving** venster voor de variabelen die u in de release-definitie-taken gebruikt, voer de volgende waarden:
1.  Voor **administratorLogin**, voer de naam van de SQL-beheerder.
2.  Voor **administratorLoginPassword**, voert u het wachtwoord moet worden gebruikt door de beheerder van de SQL-aanmelding. Gebruik "hangslot" verbergen en het wachtwoord kunt beveiligen.
3.  Voor **databaseName**, voer de naam van de SQL-Database.
4.  Deze variabelen zijn specifiek voor de voorbeeldomgevingen, verschillende omgevingen kunnen verschillende variabelen.

## <a name="create-an-environment"></a>Een omgeving maken
De volgende fase van de implementatie is het maken van de omgeving moet worden gebruikt voor ontwikkeling of voor testdoeleinden.

1. Selecteer in de release-definitie **toevoegen taken**.
2. Op de **taken** tabblad, voegt u een taak maken-omgeving van Azure DevTest Labs. De taak als volgt configureren:
    1. Voor **Azure RM-abonnement**, selecteert u een verbinding in de **beschikbare verbindingen van Azure Service** lijst of maak een meer beperkte machtigingen voor verbinding met uw Azure-abonnement. Zie voor meer informatie, [Azure Resource Manager-service-eindpunt](/devops/pipelines/library/service-endpoints).
2. Voor **Labnaam**, selecteert u de naam van het exemplaar dat u eerder hebt gemaakt, *.
3. Voor **naam van de opslagplaats**, selecteert u de opslagplaats waar naar het Resource Manager-sjabloon (201) is gepusht *.
4. Voor **sjabloonnaam**, selecteert u de naam van de omgeving die u in uw bron code opslagplaats * hebt opgeslagen. 
5. De **Labnaam**, **naam van de opslagplaats**, en **sjabloonnaam** de beschrijvende representaties van de Azure resource-id's zijn. Handmatig invoeren van de beschrijvende naam ontstaan, gebruikt u de vervolgkeuzelijsten om de gegevens te selecteren.
6. Voor **Omgevingsnaam**, voer een naam in voor het aanduiden van het exemplaar van de omgeving in de testomgeving.  Deze moet uniek zijn binnen het lab.
7. De **parameterbestand** en de **Parameters**, aangepaste parameters worden doorgegeven aan de omgeving toestaan. Een of beide kunnen worden gebruikt om in te stellen de parameterwaarden. In dit voorbeeld wordt de sectie Parameters worden gebruikt. Gebruik de namen van de variabelen die u hebt gedefinieerd in de omgeving, bijvoorbeeld: `-administratorLogin “$(administratorLogin)” -administratorLoginPassword “$(administratorLoginPassword)” -databaseName “$(databaseName)” -cacheSKUCapacity 1`
8. Gegevens in de sjabloon omgeving kan worden doorgegeven via in de sectie uitvoer van de sjabloon. Controleer **maken uitvoervariabelen op basis van de uitvoer van de sjabloon omgeving** hierdoor de gegevens kunnen worden gebruikt door andere taken. `$(Reference name.Output Name)` is het patroon te volgen. Bijvoorbeeld, als de naam van verwijzing DTL is en de uitvoernaam van de in de sjabloon is de locatie van de variabele zou worden `$(DTL.location)`.

## <a name="delete-the-environment"></a>De omgeving verwijderen
De laatste fase is om de omgeving die u hebt geïmplementeerd in uw Azure DevTest Labs-exemplaar te verwijderen. Normaal gesproken verwijdert u de omgeving nadat u de dev-taken uitvoeren of Voer de tests die u nodig hebt op de geïmplementeerde resources.

Selecteer in de release-definitie **toevoegen taken**, en klik vervolgens op de **implementeren** tabblad, voegt u een **Azure DevTest Labs-verwijderen omgeving** taak. Configureer deze als volgt:

1. Als u wilt verwijderen van de virtuele machine, Zie [Azure DevTest Labs-taken](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks):
    1. Voor **Azure RM-abonnement**, selecteert u een verbinding in de **beschikbare verbindingen van Azure Service** lijst of maak een meer beperkte machtigingen voor verbinding met uw Azure-abonnement. Zie voor meer informatie, [Azure Resource Manager-service-eindpunt](/devops/pipelines/library/service-endpoints).
    2. Voor **Labnaam**, selecteert u de testomgeving waarbij de omgeving bestaat.
    3. Voor **Omgevingsnaam**, voer de naam van de omgeving moet worden verwijderd.
2. Voer een naam voor de release-definitie en vervolgens op te slaan.

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen: 
- [Multi-VM-omgevingen maken met Resource Manager-sjablonen](devtest-lab-create-environment-from-arm.md).
- Snelstartgids voor Resource Manager-sjablonen voor het automatiseren van DevTest Labs van de [DevTest Labs GitHub-opslagplaats](https://github.com/Azure/azure-quickstart-templates).
- [Pagina voor het oplossen van VSTS](/devops/pipelines/troubleshooting)

