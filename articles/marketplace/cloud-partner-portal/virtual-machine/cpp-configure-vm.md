---
title: De Microsoft Azure gehoste virtuele machine configureren voor Azure Marketplace | Microsoft Docs
description: Wordt uitgelegd hoe u het formaat van, bijwerken en een virtuele machine die wordt gehost op Azure te generaliseren.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: c1db8c99b1bd3f9bbb768572ca1f5f7a4e1e0de4
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639694"
---
# <a name="configure-the-azure-hosted-vm"></a>Configureer de VM wordt gehost op Azure

In dit artikel wordt uitgelegd hoe u het formaat van, bijwerken en een virtuele machine (VM) die worden gehost op Azure te generaliseren.  Deze stappen zijn nodig om voor te bereiden van uw virtuele machine worden geïmplementeerd vanaf de Azure Marketplace.


## <a name="sizing-the-vhds"></a>Grootte van de VHD 's

<!--TD: Check if the following assertion is true. I didn't understand the original content. --> Als u een van de virtuele machines die vooraf zijn geconfigureerd met een besturingssysteem (en eventueel extra services) hebt geselecteerd, wordt u al een standaard Azure-VM-grootte, verzameld zoals beschreven in [VM-SKU's tabblad](./cpp-skus-tab.md).  Dit is de aanbevolen methode.  Echter, als u een besturingssysteem handmatig installeert, klikt u vervolgens u moet het formaat van uw primaire VHD in uw VM-installatiekopie:

- Voor Windows, het besturingssysteem VHD moet worden gemaakt als een 127-128 GB vaste indeling VHD. 
- Voor Linux, deze VHD moet worden gemaakt als een 30-50 GB vaste indeling VHD.

Als de fysieke grootte kleiner is dan 127-128 GB is, moet de VHD schaars zijn. De basis Windows en SQL Server-installatiekopieën die zijn opgegeven al aan deze vereisten voldoet, dus veranderen niet de indeling of de grootte van de verkregen VHD. 

Gegevensschijven mogen even groot zijn als 1 TB. Bij het bepalen van de schijfgrootte, houd er rekening mee dat klanten VHD's binnen een installatiekopie kunnen niet op het moment van implementatie aangepast. VHD's met gegevensschijven moeten worden gemaakt als een vaste indeling VHD. Ze moeten ook schaars zijn. Gegevensschijven kunnen in eerste instantie leeg zijn of gegevens bevatten.


## <a name="install-the-most-current-updates"></a>Installeer de meest recente updates

De basisinstallatiekopieën van besturingssysteem VM's bevatten de meest recente updates tot aan de publicatiedatum. Voordat u publiceert de besturingssysteem-VHD die u hebt gemaakt, zorg ervoor dat u het besturingssysteem en alle geïnstalleerde services met alle de meest recente beveiliging en het onderhoud patches bijwerken.

Voor Windows Server 2016, voert u de **controleren op Updates** opdracht.  Raadpleeg anders voor oudere versies van Windows [over het verkrijgen van een update via Windows Update](https://support.microsoft.com/help/3067639/how-to-get-an-update-through-windows-update).  Windows update wordt automatisch de meest recente van essentiële en belangrijke beveiligingsupdates geïnstalleerd.

Updates zijn vaak voor Linux-distributies, gedownload en geïnstalleerd via een opdrachtregel-hulpprogramma of een grafisch hulpprogramma.  Bijvoorbeeld, Ubuntu Linux biedt de [apt-get](http://manpages.ubuntu.com/manpages/cosmic/man8/apt-get.8.html) opdracht en de [-Updatebeheerder](http://manpages.ubuntu.com/manpages/cosmic/man8/update-manager.8.html) hulpprogramma voor het bijwerken van het besturingssysteem.


## <a name="perform-additional-security-checks"></a>Extra beveiligingscontroles uitvoeren

U moet een hoge mate van beveiliging behouden voor uw oplossing-installatiekopieën in de Azure Marketplace.  Het volgende artikel vindt u een controlelijst met beveiligingsconfiguraties en procedures om te helpen u bij deze doelstelling: [aanbevelingen voor beveiliging voor Azure Marketplace-installatiekopieën](https://docs.microsoft.com/azure/security/security-recommendations-azure-marketplace-images).  Sommige van deze aanbevelingen zijn specifiek voor Linux-installatiekopieën, maar het meest van toepassing op alle VM-installatiekopie. 


## <a name="perform-custom-configuration-and-scheduled-tasks"></a>Aangepaste configuratie en geplande taken uitvoeren

Als aanvullende configuratie nodig is, wordt de aanbevolen aanpak is het gebruik van een geplande taak die wordt uitgevoerd bij het opstarten alle laatste wijzigingen aanbrengen in de virtuele machine nadat deze is geïmplementeerd.  Houd ook rekening met de volgende aanbevelingen:
- Als het een taak eenmaal wordt uitgevoerd, is het aanbevolen dat de taak zelf verwijderen nadat deze is voltooid.
- Configuraties niet afhankelijk zijn van andere schijven dan C of D, omdat alleen deze twee stations die altijd gegarandeerd aanwezig zijn. Station C is de besturingssysteemschijf en station D is de tijdelijke lokale schijf.

Zie voor meer informatie over Linux-aanpassingen, [extensies en functies voor Linux virtuele machines](https://docs.microsoft.com/azure/virtual-machines/extensions/features-linux).


## <a name="generalize-the-image"></a>Generaliseren van de installatiekopie

Alle installatiekopieën in de Azure Marketplace moeten worden ingezet op een generieke manier herbruikbaar zijn. Als u wilt bereiken deze herbruikbaarheid, het besturingssysteem VHD moet worden *gegeneraliseerd*, een bewerking die alle instantiefout-id's en stuurprogramma's voor software van een virtuele machine worden verwijderd.

### <a name="windows"></a>Windows

Windows-OS-schijven zijn gegeneraliseerd met het [hulpprogramma sysprep](https://docs.microsoft.com/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview). Als u later bijwerken of opnieuw configureren van het besturingssysteem, moet u sysprep opnieuw uitvoeren. 

> [!WARNING]
>  Omdat updates automatisch uitgevoerd zodra u sysprep uitvoert, moet u de VM uitschakelen, totdat deze is geïmplementeerd.  Deze afsluiting zal voorkomen dat latere updates instantiefout wijzigingen aanbrengen in de OS VHD of services zijn geïnstalleerd.

Zie voor meer informatie over het uitvoeren van sysprep [stappen voor het generaliseren van een VHD] ()https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image#steps-to-generalize-a-vhd)

### <a name="linux"></a>Linux

Proces in twee stappen te volgen, wordt een Linux-VM generaliseren en opnieuw te implementeren als een afzonderlijke virtuele machine.  Zie voor meer informatie, [over het maken van een installatiekopie van een virtuele machine of VHD](../../../virtual-machines/linux/capture-image.md). 

#### <a name="remove-the-azure-linux-agent"></a>De Azure Linux-agent verwijderen
1.  Verbinding maken met uw Linux-VM met behulp van een SSH-client.
2.  Typ de volgende opdracht in het venster SSH: <br/>
    `sudo waagent -deprovision+user`
3.  Type `y` om door te gaan. (U kunt toevoegen de `-force` parameter aan de vorige opdracht te voorkomen dat deze bevestigingsstap.)
4.  Nadat de opdracht is voltooid, typt u `exit` te sluiten van de SSH-client.

<!-- TD: I need to add meat and/or references to the following steps -->
#### <a name="capture-the-image"></a>De installatiekopie vastleggen
1.  Ga naar de Azure portal, selecteer de resourcegroep (RG) en toewijzen van de virtuele machine ongedaan maken.
2.  Uw VHD is nu gegeneraliseerd en u kunt een nieuwe virtuele machine maken met behulp van deze VHD.


## <a name="create-one-or-more-copies"></a>Een of meer kopieën maken

Het maken van kopieën van virtuele machine is vaak handig is voor back-up, testen, aangepaste failover of taakverdeling, verschillende configuraties van een oplossing bieden, enzovoort. Zie voor meer informatie over het dupliceren en downloaden van een primaire VHD, zodat een niet-beheerde kloon:

- Virtuele Linux-machine: [downloaden van een Linux VHD van Azure](../../../virtual-machines/linux/download-vhd.md)
- Windows virtuele machine: [downloaden van een Windows VHD vanuit Azure](../../../virtual-machines/windows/download-vhd.md)


## <a name="next-steps"></a>Volgende stappen

Nadat de virtuele machine is geconfigureerd, bent u klaar om te [een virtuele machine implementeren vanuit een virtuele harde schijf](./cpp-deploy-vm-vhd.md).
