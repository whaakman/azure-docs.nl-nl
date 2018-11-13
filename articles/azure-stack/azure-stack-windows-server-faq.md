---
title: Azure Stack-WindowsServer gerelateerde Veelgestelde vragen | Microsoft Docs
description: Lijst met veelgestelde vragen over Azure Stack Marketplace voor WindowsServer
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2018
ms.author: sethm
ms.reviewer: avishwan
ms.openlocfilehash: 91404f01a1a675ac59898336ef8aa81e1d2638b6
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51579231"
---
# <a name="windows-server-in-azure-stack-marketplace-faq"></a>Windows-Server in Azure Stack Marketplace Veelgestelde vragen

In dit artikel vindt u antwoorden op enkele veelgestelde vragen over Windows Server-installatiekopieën in de [Azure Stack Marketplace](azure-stack-marketplace.md).

## <a name="marketplace-items"></a>Items voor de Marketplace

### <a name="how-do-i-update-to-a-newer-windows-image"></a>Hoe kan ik naar een nieuwere Windows-installatiekopie bijwerken?

Bepaal eerst als een Azure Resource Manager-sjablonen raadpleegt u specifieke versies. Als dit het geval is, werkt u deze sjablonen of houden van oudere versies van een installatiekopie. Het is raadzaam om te gebruiken **versie: nieuwste**.

Vervolgens als een virtuele-Machineschaalsets naar een specifieke versie verwijst, moet u bedenken of deze later wordt aangepast en beslissen of oudere versies wilt behouden. Als geen van deze voorwaarden van toepassing is, verwijdert u oudere installatiekopieën in de Marketplace voor het downloaden van de nieuwere. Marketplace-beheer gebruiken om dit te doen als dat is hoe de oorspronkelijke is gedownload. Download vervolgens de nieuwere versie.

### <a name="what-are-the-licensing-options-for-windows-server-marketplace-images-on-azure-stack"></a>Wat zijn de licentie-opties voor Windows Server-Marketplace-installatiekopieën in Azure Stack?

Microsoft biedt twee versies van Windows Server-installatiekopieën via de Azure Stack Marketplace:

- **Betalen naarmate u**: deze installatiekopieën worden uitgevoerd met de volledige prijs Windows-meters. 
   Wie: EA-klanten die gebruikmaken van de *verbruik factureringsmodel*; CSP's die niet willen gebruiken SPLA-licentieverlening.
- **Bring Your Own License (BYOL)**: basic meters deze installatiekopieën worden uitgevoerd.
   Wie: EA-klanten met een Windows Server-licentie; CSP's die gebruikmaken van SPLA-licentieverlening.

Azure Hybrid Use Benefit (AHUB) wordt niet ondersteund in Azure Stack. Klanten die een licentie via het capaciteitsmodel '' moeten de BYOL-installatiekopie gebruiken. Als u wilt testen met de Azure Stack Development Kit (ASDK), kunt u een van deze opties.

### <a name="what-if-i-downloaded-the-wrong-version-to-offer-my-tenantsusers"></a>Wat gebeurt er als ik de verkeerde versie te bieden van mijn tenants/gebruikers gedownload?

De verkeerde versie eerst verwijderen via Marketplace-beheer. Wacht totdat deze is volledig worden voltooid (Kijk naar de meldingen voor de voltooiing, niet de Marketplace beheerblade). Download vervolgens de juiste versie.

### <a name="what-if-my-user-incorrectly-checked-the-i-have-a-license-box-in-previous-windows-builds-and-they-dont-have-a-license"></a>Wat gebeurt er als mijn gebruiker het selectievakje 'Ik heb een licentie' in de vorige Windows niet correct ingeschakeld bouwt en ze niet beschikken over een licentie?

Zie [converteren Windows Server-VM's met benefit terug naar betalen per gebruik](../virtual-machines/windows/hybrid-use-benefit-licensing.md#powershell-1).

### <a name="what-if-i-have-an-older-image-and-my-user-forgot-to-check-the-i-have-a-license-box-or-we-use-our-own-images-and-we-do-have-ea-entitlement"></a>Wat gebeurt er als ik heb een installatiekopie van een oudere en de gebruiker om te controleren of het selectievakje "Ik heb een licentie" of we onze eigen installatiekopieën gebruiken en we hebben wel EA rechten vergeten?

Zie [converteren van een bestaande virtuele machine met behulp van Azure Hybrid Benefit voor Windows Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md#convert-an-existing-vm-using-azure-hybrid-benefit-for-windows-server). Houd er rekening mee dat Azure Hybrid Benefit niet voor Azure Stack geldt, maar het effect van deze instelling is van toepassing.

### <a name="what-about-other-vms-that-use-windows-server-such-as-sql-or-machine-learning-server"></a>Hoe zit het met andere virtuele machines die gebruikmaken van Windows Server, zoals SQL of Machine Learning Server?

Deze installatiekopieën geldt de **licenseType** parameter, zodat ze betalen als u gebruikt. U kunt deze parameter instellen (Zie de vorige veelgestelde vragen over antwoord). Dit geldt alleen voor de Windows Server-software, niet tot gelaagde producten, zoals SQL, waarbij u moet uw eigen licentie. Betalen als u licenties is niet van toepassing op gelaagde softwareproducten.

### <a name="i-have-an-ea-and-i-create-my-own-images-how-do-i-make-sure-they-are-billed-correctly"></a>Ik heb een EA en maak ik mijn eigen afbeeldingen. hoe maak ik ervoor dat ze correct worden gefactureerd?

U kunt toevoegen **licenseType: Windows-_SERVER** in een Azure Resource Manager-sjabloon. Deze instelling moet worden toegevoegd aan elk blok van de bron virtuele machine.

## <a name="activation"></a>Activering

Voor het activeren van een Windows Server virtuele machine in Azure Stack, moeten de volgende voorwaarden waar zijn:

- De OEM heeft de juiste BIOS-markering instellen op elk hostsysteem in Azure Stack.
- U moet Windows Server 2012 R2 en Windows Server 2016 gebruiken [automatische activering van virtuele machines](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)). Key Management Service (KMS) en andere services voor volumeactivering worden niet ondersteund in Azure Stack.

### <a name="how-can-i-verify-that-my-virtual-machine-is-activated"></a>Hoe kan ik verifiëren dat mijn virtuele machine is geactiveerd?

Voer de volgende opdracht vanaf een opdrachtprompt met verhoogde bevoegdheid: 

```shell
slmgr /dlv
``` 

Als het goed is geactiveerd, u deze duidelijk aangegeven ziet en de hostnaam van de weergegeven de `slmgr` uitvoer. Niet afhankelijk van watermerken worden weergegeven als ze niet bijgewerkt worden mogelijk, of vanaf een andere virtuele machine achter uw worden weergegeven.

### <a name="my-vm-is-not-set-up-to-use-avma-how-can-i-fix-it"></a>Mijn virtuele machine is niet ingesteld voor het gebruik van AVMA, hoe kan ik doen?

Voer de volgende opdracht vanaf een opdrachtprompt met verhoogde bevoegdheid: 

```shell
slmgr /ipk <AVMA key> 
```

Zie het artikel [automatische activering van virtuele machines](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)) voor de sleutels voor uw installatiekopie.

### <a name="i-create-my-own-windows-server-images-how-can-i-make-sure-they-use-avma"></a>Maak ik mijn eigen installatiekopieën van Windows Server, hoe kan ik ervoor zorgen dat ze AVMA gebruiken?

Het is raadzaam dat u uitvoeren de `slmgr /ipk` vanaf de opdrachtregel met de juiste sleutel voordat u de `sysprep` opdracht. Of de AVMA-sleutel opnemen in een installatiebestand Unattend.exe.

### <a name="i-am-trying-to-use-my-windows-server-2016-image-created-on-azure-and-it-is-not-activating-or-using-kms-activation"></a>Ik wil mijn Windows Server 2016-installatiekopie gemaakt op Azure gebruiken en niet is geactiveerd of met behulp van de KMS-activering.

Voer de opdracht `slmgr /ipk` uit. Installatiekopieën van Azure die mogelijk niet correct terug te vallen AVMA, maar als ze het systeem Azure KMS bereiken kunnen, ze worden geactiveerd. Het is raadzaam dat u deze VM's zijn ingesteld voor het gebruik van AVMA waarborgen.

### <a name="i-have-performed-all-of-these-steps-but-my-virtual-machines-are-still-not-activating"></a>Ik al deze stappen hebt uitgevoerd, maar mijn virtuele machines nog steeds niet activeren.

Neem contact op met uw hardwareleverancier om te controleren dat de juiste BIOS-markeringen zijn geïnstalleerd.

### <a name="what-about-earlier-versions-of-windows-server"></a>Hoe zit het met eerdere versies van Windows Server?

[Automatische activering van virtuele machines](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn303421(v=ws.11)) wordt niet ondersteund in eerdere versies van Windows Server. U moet de virtuele machines handmatig activeren.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg voor meer informatie de volgende artikelen:

- [Het overzicht van Azure Stack Marketplace](azure-stack-marketplace.md)
- [Marketplace-items van Azure naar Azure Stack downloaden](azure-stack-download-azure-marketplace-item.md)
