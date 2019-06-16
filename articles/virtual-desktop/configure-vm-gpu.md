---
title: GPU configureren voor het voorbeeld in Windows Virtual PC - Azure
description: Het inschakelen van GPU-versnelde rendering en codering in Windows Virtual Desktop Preview.
services: virtual-desktop
author: gundarev
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 05/06/2019
ms.author: denisgun
ms.openlocfilehash: a6a67c89253a1b16f9266d7917655d1b1104022e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65159568"
---
# <a name="configure-graphics-processing-unit-gpu-acceleration-for-windows-virtual-desktop-preview"></a>Grafische versnelling op GPU (unit) voor Windows Virtual Desktop Preview configureren

Windows virtuele bureaublad Preview biedt ondersteuning voor GPU-versnelde rendering en de codering voor verbeterde app-prestaties en schaalbaarheid. GPU-versnelling is met name cruciaal voor grafisch-intensieve apps.

Volg de instructies in dit artikel om een geoptimaliseerde GPU Azure-machine maken, toe te voegen aan uw groep host en configureer deze voor het GPU-versnelling gebruiken voor het weergeven en codering. In dit artikel wordt ervan uitgegaan dat u al een virtuele Windows-bureaublad-tenant die is geconfigureerd.

## <a name="select-a-gpu-optimized-azure-virtual-machine-size"></a>Selecteer een grootte van de virtuele machine van Azure geoptimaliseerde GPU

Azure biedt een aantal [GPU VM-grootten geoptimaliseerd](/azure/virtual-machines/windows/sizes-gpu). De juiste keuze voor uw pool host, is afhankelijk van een aantal factoren, met inbegrip van uw specifieke app-werkbelastingen, de gewenste kwaliteit van de gebruikerservaring, en de kosten. In het algemeen bieden grotere en meer mogelijkheden GPU's een betere gebruikerservaring op een bepaalde gebruiker dichtheid.

## <a name="create-a-host-pool-provision-your-virtual-machine-and-configure-an-app-group"></a>Een host-pool maakt, inrichten van uw virtuele machine en een app-groep configureren

Maak een nieuwe host-toepassingen met behulp van een virtuele machine van de grootte die u hebt geselecteerd. Zie voor instructies [zelfstudie: Een host-pool maken met Azure Marketplace](/azure/virtual-desktop/create-host-pools-azure-marketplace).

Windows virtuele bureaublad Preview biedt ondersteuning voor GPU-versnelde rendering en codering in de volgende besturingssystemen:

* Windows 10 versie 1511 of hoger
* Windows Server 2016 of hoger

U moet ook een app-groep configureren, of gebruikt u de standaard-desktop-app-groep (met de naam 'Desktop Application groeperen') die automatisch wordt gemaakt wanneer u een nieuwe host-groep maakt. Zie voor instructies [zelfstudie: App-groepen beheren voor Windows Virtual Desktop Preview](/azure/virtual-desktop/manage-app-groups).

>[!NOTE]
>Windows virtuele bureaublad Preview ondersteunt alleen het type 'Desktop' app-groep voor groepen met GPU host. App-groepen van het type 'RemoteApp' worden niet ondersteund voor groepen met GPU host.

## <a name="install-supported-graphics-drivers-in-your-virtual-machine"></a>Ondersteunde grafische stuurprogramma's installeren in uw virtuele machine

Als u wilt profiteren van de GPU-mogelijkheden van Azure uit de N-serie VM's in Windows Virtual Desktop Preview, moet u NVIDIA grafische stuurprogramma's installeren. Volg de instructies op [installeren NVIDIA GPU-stuurprogramma's op uit de N-serie VM's waarop Windows wordt uitgevoerd](/azure/virtual-machines/windows/n-series-driver-setup) stuurprogramma's te installeren, ofwel handmatig of met behulp van de [NVIDIA GPU-stuurprogramma extensie](/azure/virtual-machines/extensions/hpccompute-gpu-windows).

Houd er rekening mee dat alleen [NVIDIA GRID-stuurprogramma's](/azure/virtual-machines/windows/n-series-driver-setup#nvidia-grid-drivers) gedistribueerd door Azure worden ondersteund voor Windows Virtual Desktop Preview.

Na het installeren van stuurprogramma's, een virtuele machine opnieuw worden opgestart. Gebruik de verificatiestappen uit in de bovenstaande instructies om te bevestigen dat grafische stuurprogramma's zijn geïnstalleerd.

## <a name="configure-gpu-accelerated-app-rendering"></a>Rendering van GPU-versnelde app configureren

Standaard, apps en pc's die worden uitgevoerd in meerdere sessieconfiguraties worden weergegeven met de CPU en geen gebruik van beschikbare GPU's voor rendering. Groepsbeleid voor de host om in te schakelen GPU-versnelde rendering configureren:

1. Verbinding maken met het bureaublad van de virtuele machine met een account met lokale administrator-bevoegdheden.
2. Open de Start menu en voert u 'gpedit.msc' om te openen van de Editor voor Groepsbeleid.
3. De structuur om te navigeren **Computerconfiguratie** > **Beheersjablonen** > **Windows-onderdelen**  >   **Extern bureaublad-Services** > **extern bureaublad-sessiehost** > **omgeving van de externe sessie**.
4. Selecteer beleid **gebruik van de hardware standaard grafische adapter voor alle sessies van extern bureaublad-Services** en dit beleid instellen op **ingeschakeld** GPU-rendering in de externe sessie inschakelen.

## <a name="configure-gpu-accelerated-frame-encoding"></a>GPU-versnelde frame-codering configureren

Extern bureaublad codeert alle afbeeldingen die worden weergegeven door apps en pc's (of gegenereerd met GPU of CPU) voor verzending naar extern bureaublad-clients. Standaard, extern bureaublad maakt geen gebruik van beschikbare GPU's voor deze codering. Groepsbeleid voor de host om in te schakelen GPU-versnelde frame-codering configureren. U doorgaat met de bovenstaande stappen:

1. Selecteer beleid **prioriteren H.264/AVC 444 grafische modus voor extern bureaublad-verbindingen** en dit beleid instellen op **ingeschakeld** om af te dwingen H.264/AVC 444 verbeterde codecs in de externe sessie.
2. Selecteer beleid **H.264/AVC configureren voor extern bureaublad-verbindingen codering hardware** en dit beleid instellen op **ingeschakeld** om in te schakelen hardware voor AVC/H.264-codering in de externe sessie.

    >[!NOTE]
    >In Windows Server 2016, stelt u optie **liever AVC Hardware codering** naar **altijd proberen**.

3. Nu dat het groepsbeleid zijn bewerkt, dwingen bijwerken van Groepsbeleid. Open de opdrachtprompt en typ:

    ```batch
    gpupdate.exe /force
    ```

4. Meld u af bij de extern bureaublad-sessie.

## <a name="verify-gpu-accelerated-app-rendering"></a>Controleer of de GPU-versnelde app-rendering

Om te controleren dat apps worden met behulp van de GPU voor rendering, probeert u het volgende:

* Gebruik de `nvidia-smi` hulpprogramma zoals beschreven in [controleren of de installatie van stuurprogramma](/azure/virtual-machines/windows/n-series-driver-setup#verify-driver-installation) om te controleren op GPU-gebruik bij het uitvoeren van uw apps.
* U kunt Taakbeheer om te controleren op GPU-gebruik gebruiken op ondersteunde besturingssysteemversies. Selecteer de GPU op het tabblad 'Prestaties' om te zien of apps in de GPU gebruikt.

## <a name="verify-gpu-accelerated-frame-encoding"></a>Controleer of de GPU-versnelde frame-codering

Om te controleren dat extern bureaublad wordt gebruikt door GPU-versnelde encoding:

1. Verbinding maken met het bureaublad van de virtuele machine met behulp van virtuele Windows-bureaublad-client.
2. Start Logboeken en navigeer naar het volgende knooppunt: **Logboeken toepassingen en Services** > **Microsoft** > **Windows** > **RemoteDesktopServices-RdpCoreTS**  >  **Operationele**
3. Om te bepalen als GPU-versnelde codering wordt gebruikt, zoek naar gebeurtenis-ID 170. Als u ziet ' AVC hardware encoder is ingeschakeld: 1" en vervolgens GPU-codering wordt gebruikt.
4. Om te bepalen als AVC 444-modus wordt gebruikt, zoek naar gebeurtenis-ID 162. Als u ziet ' AVC beschikbaar: Eerste 1-profiel: 2048' vervolgens AVC 444 wordt gebruikt.

## <a name="next-steps"></a>Volgende stappen

Deze instructies hebt u actief en werkend met GPU-versnelling op een host tijdens één sessie VM. Enkele aanvullende overwegingen voor het inschakelen van GPU-versnelling voor een grotere groep van de host:

* Overweeg het gebruik van de [NVIDIA GPU-stuurprogramma extensie](/azure/virtual-machines/extensions/hpccompute-gpu-windows) voor het installeren van stuurprogramma's en updates voor een aantal virtuele machines vereenvoudigen.
* Overweeg het gebruik van Active Directory-groepsbeleid voor het vereenvoudigen van de configuratie van beleid voor een aantal virtuele machines. Zie voor meer informatie over het implementeren van Groepsbeleid in Active Directory-domein [werken met groepsbeleidsobjecten](https://go.microsoft.com/fwlink/p/?LinkId=620889).
