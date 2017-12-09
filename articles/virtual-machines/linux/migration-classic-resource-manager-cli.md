---
title: Virtuele machines migreren naar Resource Manager, met Azure CLI | Microsoft Docs
description: Dit artikel begeleidt u bij de migratie platform ondersteund van resources van klassieke in Azure Resource Manager met Azure CLI
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d6f5a877-05b6-4127-a545-3f5bede4e479
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 607ab59dbeb414c69a6272d0aeb00299296bca6a
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-cli"></a>Migreren IaaS-middelen van klassiek naar Azure Resource Manager met behulp van Azure CLI
Deze stappen ziet u het gebruik van Azure-opdrachtregelinterface (CLI)-opdrachten voor het migreren van infrastructuur als een dienst (IaaS) resources van het klassieke implementatiemodel naar het Azure Resource Manager-implementatiemodel. In het artikel moet de [Azure CLI 1.0](../../cli-install-nodejs.md). Omdat Azure CLI 2.0 alleen van toepassing op Azure Resource Manager-resources is, kan deze niet worden gebruikt voor de migratie.

> [!NOTE]
> Alle bewerkingen die hier wordt beschreven, zijn de idempotent. Als er een probleem dan een niet-ondersteunde functie of een configuratiefout, raden wij aan dat u opnieuw de voorbereiden proberen afgebroken of doorgevoerd, bewerking. Het platform wordt en probeer het opnieuw.
> 
> 

<br>
Hier volgt een stroomdiagram voor het identificeren van de volgorde waarin stappen moeten worden uitgevoerd tijdens een migratie

![Schermafbeelding van de migratiestappen](../windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-prepare-for-migration"></a>Stap 1: Voorbereiden voor migratie
Hier volgen enkele aanbevolen procedures die wij adviseren kijkt u bij het evalueren van IaaS Resourcemigratie van klassiek naar Resource Manager:

* Lees de [lijst met niet-ondersteunde configuraties of functies](../windows/migration-classic-resource-manager-overview.md). Als u virtuele machines die gebruikmaken van niet-ondersteunde configuraties of functies hebt, raden wij u voor de functie of configuratieserver ondersteuning wordt aangekondigd. U kunt ook kunt u deze functie verwijderen of dat de configuratie verlaten migratie inschakelen als deze bij uw behoeften past.
* Als u scripts die uw infrastructuur en toepassingen vandaag implementeert geautomatiseerde, kunt u een vergelijkbare test-instellingen te maken met behulp van deze scripts voor de migratie. U kunt ook de voorbeeld-omgevingen instellen met behulp van de Azure-portal.

> [!IMPORTANT]
> Toepassingsgateways worden momenteel niet ondersteund voor migratie van klassiek naar Resource Manager. Verwijder de gateway voordat u een voorbereidingsbewerking voor het verplaatsen van het netwerk voor het migreren van een klassiek virtueel netwerk met een Application gateway. Nadat u de migratie hebt voltooid, sluit u de gateway in Azure Resource Manager. 
>
>ExpressRoute-gateways die verbinding maken met ExpressRoute-circuits in een ander abonnement kunnen niet automatisch worden gemigreerd. In dergelijke gevallen verwijderen van de ExpressRoute-gateway, migreren van het virtuele netwerk en de gateway opnieuw maken. Zie [migreren ExpressRoute-circuits en bijbehorende virtuele netwerken van het klassieke naar het Resource Manager-implementatiemodel](../../expressroute/expressroute-migration-classic-resource-manager.md) voor meer informatie.
> 
> 

## <a name="step-2-set-your-subscription-and-register-the-provider"></a>Stap 2: Stel uw abonnement en registreer de provider
Voor migratiescenario's, moet u uw omgeving voor zowel klassieke instellen en Resource Manager. [Azure CLI installeren](../../cli-install-nodejs.md) en [Selecteer uw abonnement](/cli/azure/authenticate-azure-cli).

Aanmelden bij uw account.

    azure login

Selecteer het Azure-abonnement met behulp van de volgende opdracht.

    azure account set "<azure-subscription-name>"

> [!NOTE]
> Registratie is een tijdstap, maar er moet gebeuren eenmaal voordat u de migratie. Zonder te registreren ziet u het volgende foutbericht weergegeven 
> 
> *BadRequest: Abonnement is niet geregistreerd voor migratie.* 
> 
> 

Registreren bij de resourceprovider voor migratie met behulp van de volgende opdracht. Houd er rekening mee dat in sommige gevallen kan deze opdracht time-out. De registratie is gelukt.

    azure provider register Microsoft.ClassicInfrastructureMigrate

Wacht vijf minuten om de registratie te voltooien. U kunt de status van de goedkeuring controleren met behulp van de volgende opdracht. Zorg ervoor dat RegistrationState `Registered` voordat u doorgaat.

    azure provider show Microsoft.ClassicInfrastructureMigrate

Schakel nu over CLI aan de `asm` modus.

    azure config mode asm

## <a name="step-3-make-sure-you-have-enough-azure-resource-manager-virtual-machine-vcpus-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Stap 3: Controleer of er voldoende virtuele Machine van Azure Resource Manager vcpu's in de Azure-regio van uw huidige implementatie of VNET
Voor deze stap moet u overschakelen naar `arm` modus. Dit doen met de volgende opdracht.

```
azure config mode arm
```

U kunt de volgende opdracht in de CLI gebruiken om te controleren van het huidige aantal vcpu's die u in Azure Resource Manager hebt. Zie voor meer informatie over quota vCPU, [limieten en de Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager)

```
azure vm list-usage -l "<Your VNET or Deployment's Azure region"
```

Als u klaar bent voor het verifiëren van deze stap kunt u terugschakelt naar `asm` modus.

    azure config mode asm


## <a name="step-4-option-1---migrate-virtual-machines-in-a-cloud-service"></a>Stap 4: Optie 1 - migreren van virtuele machines in een cloudservice
Haal de lijst van cloudservices met behulp van de volgende opdracht uit en kies vervolgens de cloudservice die u wilt migreren. Houd er rekening mee dat als de virtuele machines in de cloudservice zich in een virtueel netwerk of als ze web/worker rollen hebben, u een foutbericht weergegeven krijgt.

    azure service list

Voer de volgende opdracht om de implementatienaam van de voor de cloudservice ophalen uit de uitgebreide uitvoer. In de meeste gevallen is de implementatienaam van de hetzelfde als de naam van de cloudservice.

    azure service show <serviceName> -vv

Eerst valideren als u de cloudservice met de volgende opdrachten kunt migreren:

```shell
azure service deployment validate-migration <serviceName> <deploymentName> new "" "" ""
```

Bereid de virtuele machines in de cloudservice voor migratie. U hebben kunt kiezen uit twee opties.

Als u migreren van de virtuele machines met een virtueel netwerk met platform gemaakt wilt, gebruikt u de volgende opdracht.

    azure service deployment prepare-migration <serviceName> <deploymentName> new "" "" ""

Als u migreren naar een bestaand virtueel netwerk in het Resource Manager-implementatiemodel wilt, gebruikt u de volgende opdracht.

    azure service deployment prepare-migration <serviceName> <deploymentName> existing <destinationVNETResourceGroupName> <subnetName> <vnetName>

Nadat de prepare-bewerking voltooid is, kunt u zoeken via de uitgebreide uitvoer ophalen van de migratiestatus van de virtuele machines en ervoor te zorgen dat ze zich in de `Prepared` staat.

    azure vm show <vmName> -vv

Controleer de configuratie voor de voorbereide resources met behulp van de CLI of Azure portal. Als u niet klaar voor de migratie bent en u wilt terugkeren naar de oude status, gebruikt u de volgende opdracht.

    azure service deployment abort-migration <serviceName> <deploymentName>

Als de voorbereide configuratie er goed uitziet, kunt u verder gaan en het doorvoeren van de resources met behulp van de volgende opdracht.

    azure service deployment commit-migration <serviceName> <deploymentName>



## <a name="step-4-option-2----migrate-virtual-machines-in-a-virtual-network"></a>Stap 4: Optie 2: migreren van virtuele machines in een virtueel netwerk
Kies het virtuele netwerk dat u wilt migreren. Houd er rekening mee dat als het virtuele netwerk web/werkrollen of VM's met niet-ondersteunde configuraties bevat, u een validatiefoutbericht krijgt.

De virtuele netwerken in het abonnement ophalen met behulp van de volgende opdracht.

    azure network vnet list

De uitvoer ziet er ongeveer als volgt:

![Schermopname van de opdrachtregel met de naam van het volledige virtuele netwerk gemarkeerd.](../media/virtual-machines-linux-cli-migration-classic-resource-manager/vnet.png)

In het bovenstaande voorbeeld de **virtualNetworkName** is de volledige naam **'Groep classicubuntu16 classicubuntu16'**.

Eerst valideren als u het virtuele netwerk met de volgende opdracht kunt migreren:

```shell
azure network vnet validate-migration <virtualNetworkName>
```

Het virtuele netwerk van uw keuze voorbereiden voor migratie met behulp van de volgende opdracht.

    azure network vnet prepare-migration <virtualNetworkName>

Controleer de configuratie voor de voorbereide virtuele machines met behulp van de CLI of Azure portal. Als u niet klaar voor de migratie bent en u wilt terugkeren naar de oude status, gebruikt u de volgende opdracht.

    azure network vnet abort-migration <virtualNetworkName>

Als de voorbereide configuratie er goed uitziet, kunt u verder gaan en het doorvoeren van de resources met behulp van de volgende opdracht.

    azure network vnet commit-migration <virtualNetworkName>

## <a name="step-5-migrate-a-storage-account"></a>Stap 5: Een opslagaccount migreren
Zodra u bent met klaar de virtuele machines migreert, wordt aangeraden migreren van het opslagaccount.

Het opslagaccount voorbereiden voor migratie met behulp van de volgende opdracht

    azure storage account prepare-migration <storageAccountName>

Controleer de configuratie voor de voorbereide storage-account met behulp van de CLI of Azure portal. Als u niet klaar voor de migratie bent en u wilt terugkeren naar de oude status, gebruikt u de volgende opdracht.

    azure storage account abort-migration <storageAccountName>

Als de voorbereide configuratie er goed uitziet, kunt u verder gaan en het doorvoeren van de resources met behulp van de volgende opdracht.

    azure storage account commit-migration <storageAccountName>

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van de migratie van IaaS resources van klassieke in Azure Resource Manager-platform ondersteund](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Technische deep dive op platform ondersteund migratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Planning voor de migratie van IaaS-resources van het klassieke implementatiemodel naar Azure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [PowerShell gebruiken voor het migreren van IaaS-middelen van klassiek naar Azure Resource Manager](../windows/migration-classic-resource-manager-ps.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Community-hulpprogramma's voor hulp bij de migratie van IaaS-middelen van klassiek naar Azure Resource Manager](../windows/migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Bekijk de meest voorkomende migratiefouten](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Lees de veelgestelde vragen over IaaS Resourcemigratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
