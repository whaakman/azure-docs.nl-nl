---
title: Gedeelde virtuele Azure-Machine-installatiekopieën voor Windows met behulp van portal maken | Microsoft Docs
description: Meer informatie over het gebruik van Azure portal maken en delen van installatiekopieën voor virtuele machines.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 04/29/2019
ms.author: cynthn
ms.custom: ''
ms.openlocfilehash: 50714f3fe6d30b4b93a0b42383b6f4634c86eafa
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148053"
---
# <a name="create-a-shared-image-gallery-using-the-azure-portal"></a>Maken van een galerie met installatiekopieën van de gedeelde met behulp van de Azure portal

Een [gedeelde afbeeldingengalerie](shared-image-galleries.md) vereenvoudigt de aangepaste installatiekopie voor uw organisatie te delen. Aangepaste installatiekopieën zijn soortgelijk aan Marketplace-installatiekopieën, maar u kunt deze zelf maken. Aangepaste installatiekopieën kunnen worden gebruikt om de bootstrap-implementatietaken zoals het vooraf laden van toepassingen, toepassingsconfiguraties en andere besturingssysteemconfiguraties. 

De galerie met installatiekopieën gedeeld kunt u uw aangepaste VM-installatiekopieën delen met anderen in uw organisatie, binnen of tussen regio's binnen een AAD-tenant. Kies welke installatiekopieën die u wilt delen, welke regio's die u wilt maken ze beschikbaar zijn in, en die u wilt delen. U kunt meerdere galerieën kunt maken, zodat u kunt gedeelde afbeeldingen logisch groeperen. 

De galerie is een op het hoogste niveau resource die volledig op rollen gebaseerd toegangsbeheer (RBAC) biedt. Installatiekopieën kunnen worden bijgehouden, en u kunt kiezen voor het repliceren van elke versie van de installatiekopie naar een andere set Azure-regio's. De galerie werkt alleen met beheerde installatiekopieën.

De galerie met installatiekopieën van gedeelde functie heeft meerdere resourcetypen. Er wordt met behulp van of het bouwen van deze in dit artikel:

| Resource | Description|
|----------|------------|
| **Beheerde installatiekopie** | Dit is een basic-installatiekopie die kan worden alleen wordt gebruikt of worden gebruikt voor het maken van een **installatiekopieversie** in een galerie met installatiekopieën. Beheerde installatiekopieën zijn gemaakt op basis van gegeneraliseerde VM's. Een beheerde installatiekopie is een speciaal type VHD die kan worden gebruikt voor het maken van meerdere virtuele machines en kan nu worden gebruikt om gedeelde installatiekopie versies te maken. |
| **Galerie met installatiekopieën** | De Azure Marketplace, zoals een **afbeeldingengalerie** is een opslagplaats voor het beheren en delen van afbeeldingen, maar u bepaalt wie toegang heeft. |
| **De definitie van installatiekopie** | Afbeeldingen worden gedefinieerd in een galerie en informatie over de installatiekopie en de vereisten voor het gebruik van deze intern uitvoeren. Dit omvat of de installatiekopie Windows of Linux, release-opmerkingen en vereisten voor minimale en maximale hoeveelheid geheugen is. Het is een definitie van een type van de installatiekopie. |
| **De versie van installatiekopie** | Een **installatiekopieversie** is wat u gebruikt om een virtuele machine maken wanneer u een galerie. U kunt meerdere versies van een installatiekopie kunt hebben, indien nodig voor uw omgeving. Een beheerde installatiekopie, bijvoorbeeld wanneer u een **installatiekopieversie** voor het maken van een virtuele machine, de versie van de installatiekopie die wordt gebruikt voor het maken van nieuwe schijven voor de virtuele machine. Versies van een installatiekopie kunnen meerdere keren worden gebruikt. |


## <a name="before-you-begin"></a>Voordat u begint

Als u wilt het voorbeeld in dit artikel hebt voltooid, moet u een bestaande beheerde installatiekopie hebben. U kunt volgen [zelfstudie: Een aangepaste installatiekopie van een Azure-VM maken met Azure PowerShell](tutorial-custom-images.md) maken indien nodig. Wanneer het uitvoeren van dit artikel vervangen door de resourcegroep en VM-namen van waar nodig.


[!INCLUDE [virtual-machines-common-shared-images-portal](../../../includes/virtual-machines-common-shared-images-portal.md)]
 
## <a name="create-vms-from-an-image"></a>Virtuele machines maken van een installatiekopie

Als de versie van de installatiekopie is voltooid, kunt u een of meer nieuwe virtuele machines maken. 

Dit voorbeeld maakt u een virtuele machine met de naam *myVMfromImage*, in de *myResourceGroup* in de *VS-Oost* datacenter.

1. Selecteer op de pagina voor de versie van uw installatiekopie **maken VM** in het menu aan de bovenkant van de pagina.
1. Voor **resourcegroep**, selecteer **nieuw** en het type *myResourceGroup* voor de naam.
1. In **virtuele-machinenaam**, type *myVM*.
1. Voor **regio**, selecteer *VS-Oost*.
1. Voor **Beschikbaarheidsopties**, laat de standaardwaarde *geen infrastructuur redundantie vereist*.
1. De waarde voor **installatiekopie** automatisch moeten worden ingevuld als u op de pagina voor de versie van de installatiekopie is gestart.
1. Voor **grootte**, kiest u een VM-grootte uit de lijst met beschikbare grootten en klik op "Selecteren".
1. Geeft onder **Administrator-account**, een gebruikersnaam op, bijvoorbeeld *azureuser*, en een wachtwoord. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](faq.md#what-are-the-password-requirements-when-creating-a-vm).
1. Als u wilt toestaan van externe toegang tot de virtuele machine, onder **openbare binnenkomende poorten**, kiest u **geselecteerde poorten toestaat** en selecteer vervolgens **RDP (3389)** in de vervolgkeuzelijst. Als u niet dat externe toegang tot de virtuele machine toestaan wilt, laat u **geen** voor geselecteerde **openbare binnenkomende poorten**.
1. Wanneer u klaar bent, selecteert u de **revisie + maken** knop aan de onderkant van de pagina.
1. Nadat de virtuele machine de validatietests doorstaat, selecteert u **maken** aan de onderkant van de pagina om de implementatie te starten.


## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de VM niet meer nodig hebt, kunt u de resourcegroep, de machine zelf én alle gerelateerde resources verwijderen. Wanneer u dit wilt doen, selecteert u de resourcegroep voor de virtuele machine en selecteert u **Verwijderen**. Vervolgens bevestigt u de naam van de resourcegroep die u wilt verwijderen.

Als u verwijderen van afzonderlijke resources wilt, moet u ze in omgekeerde volgorde te verwijderen. Als u wilt verwijderen van de definitie van een installatiekopie, moet u bijvoorbeeld alle van de versies van een installatiekopie gemaakt op basis van die installatiekopie verwijderen.

## <a name="next-steps"></a>Volgende stappen

U kunt ook de galerie met installatiekopieën van gedeelde resources met behulp van sjablonen maken. Er zijn verschillende Azure-Snelstartsjablonen beschikbaar: 

- [Een gedeelde galerie met installatiekopieën maken](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [De definitie van een installatiekopie in een gedeelde galerie met installatiekopieën maken](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [De versie van een installatiekopie in een gedeelde galerie met installatiekopieën maken](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Een virtuele machine maken vanaf de versie van installatiekopie](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

Zie voor meer informatie over gedeelde installatiekopie galerieën, de [overzicht](shared-image-galleries.md). Als u problemen ondervindt, raadpleegt u [probleemoplossing gedeeld afbeeldingsgalerieën](troubleshooting-shared-images.md).

