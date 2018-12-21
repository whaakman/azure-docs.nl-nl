---
title: 'Een virtueel netwerk maken: snelstart - Azure Portal | Microsoft Docs'
description: In deze snelstart leert u hoe u een virtueel netwerk maakt met het Azure Portal. Met een virtueel netwerk kunnen Azure-resources, zoals virtuele machines, privé met elkaar en met internet communiceren.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can communicate with privately with each other and with the internet.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 11/30/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: d627d54686cb40714e9e8574ceae56a8900cb45e
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53269461"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-portal"></a>Snelstart: Een virtueel netwerk maken met Azure Portal

Met een virtueel netwerk kunnen Azure-resources, zoals virtuele machines (VM's), privé met elkaar en met internet communiceren. In deze snelstart leert u hoe u een virtueel netwerk maakt. Nadat u een virtueel netwerk hebt gemaakt, implementeert u twee virtuele machines in het virtuele netwerk. Vervolgens maakt u verbinding met de virtuele machines via internet en is er privécommunicatie tussen de twee virtuele machines mogelijk.

Als u nog geen abonnement op Azure hebt, maak dan nu een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

1. Selecteer **Een resource maken** > **Netwerken** > **Virtueel netwerk** linksboven in het scherm.

1. In **Virtueel netwerk maken** typt of selecteert u de volgende gegevens:

    | Instelling | Waarde |
    | ------- | ----- |
    | Naam | Voer *myVirtualNetwork* in. |
    | Adresruimte | Voer *10.1.0.0/16* in. |
    | Abonnement | Selecteer uw abonnement.|
    | Resourcegroep | Selecteer **Nieuwe maken**, voer *myResourceGroup* in en selecteer vervolgens **OK**. |
    | Locatie | Selecteer **US - oost**.|
    | Subnet - naam | Voer *myVirtualSubnet* in. |
    | Subnet - adresbereik | Voer *10.1.0.0/24* in. |

1. Laat de overige standaardwaarden staan en selecteer **Maken**.

## <a name="create-virtual-machines"></a>Virtuele machines maken

Twee virtuele machines in het virtuele netwerk maken:

### <a name="create-the-first-vm"></a>De eerste VM maken

1. Selecteer **Een resource maken** > **Compute** > **Windows Server 2016 Datacenter** linksboven in het scherm.

1. In **Een virtuele machine maken - Basisprincipes** typt of selecteert u deze gegevens:

    | Instelling | Waarde |
    | ------- | ----- |
    | **PROJECTGEGEVENS** | |
    | Abonnement | Selecteer uw abonnement. |
    | Resourcegroep | Selecteer **MyResourceGroup**. U hebt deze in de laatste sectie gemaakt. |
    | **INSTANTIEDETAILS** |  |
    | Naam van de virtuele machine | Voer *myVm1* in. |
    | Regio | Selecteer **US - oost**. |
    | Beschikbaarheidsopties | Handhaaf de standaardinstelling **Geen infrastructuurredundantie vereist**. |
    | Installatiekopie | Handhaaf de standaardinstelling **Windows Server 2016 Datacenter**. |
    | Grootte | Handhaaf de standaardinstelling **Standard DS1 v2**. |
    | **ADMINISTRATOR-ACCOUNT** |  |
    | Gebruikersnaam | Voer een gebruikersnaam naar keuze in. |
    | Wachtwoord | Voer een wachtwoord naar keuze in. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    | Wachtwoord bevestigen | Voer het wachtwoord opnieuw in. |
    | **REGELS VOOR BINNENKOMENDE POORT** |  |
    | Openbare poorten voor inkomend verkeer | Handhaaf de standaardinstelling **Geen**. |
    | **GELD BESPAREN** |  |
    | Hebt u al een Windows Server-licentie? | Handhaaf de standaardinstelling **Nee**. |

1. Selecteer **Volgende: Schijven**.

1. Handhaaf de standaardinstellingen in **Een virtuele machine maken - schijven** en selecteer **Volgende: Netwerken**.

1. Selecteer in **Een virtuele machine maken - Netwerken** de volgende gegevens:

    | Instelling | Waarde |
    | ------- | ----- |
    | Virtueel netwerk | Handhaaf de standaardinstelling **myVirtualNetwork**. |
    | Subnet | Handhaaf de standaardinstelling **myVirtualSubnet (10.1.0.0/24)**. |
    | Openbare IP | Handhaaf de standaardinstelling **(new) myVm-ip**. |
    | Netwerkbeveiligingspoorten | Selecteer **Geselecteerde poorten toestaan**. |
    | Poorten voor inkomend verkeer selecteren | Selecteer **HTTP** en **RDP**.

1. Selecteer **Volgende: Beheer**.

1. Selecteer in **Een virtuele machine maken - Beheer** voor **Opslagaccount voor diagnose** de optie **Nieuw**.

1. In **Opslagaccount maken** typt of selecteert u de volgende gegevens:

    | Instelling | Waarde |
    | ------- | ----- |
    | Naam | Voer *myvmstorageaccount* in. |
    | Soort account | Handhaaf de standaardinstelling **Storage (algemeen gebruik v1)**. |
    | Prestaties | Handhaaf de standaardinstelling **Standard**. |
    | Replicatie | Handhaaf de standaardinstelling **Lokaal redundante opslag (LRS)**. |

1. Selecteer **OK**

1. Selecteer **Controleren + maken**. De pagina **Beoordelen en maken** wordt weergegeven en Azure valideert de configuratie.

1. Wanneer u de tekst **Validatie voltooid** ziet, selecteert u **Maken**.

### <a name="create-the-second-vm"></a>De tweede VM maken

1. Voer bovenstaande stappen 1 tot en met 9 uit.

    > [!NOTE]
    > In stap 2 voor de **naam van de virtuele machine** geeft u *myVm2* op.
    >
    > In stap 7 voor **Opslagaccount voor diagnose** selecteert u **myvmstorageaccount**.

1. Selecteer **Controleren + maken**. De pagina **Beoordelen en maken** wordt weergegeven en Azure valideert de configuratie.

1. Wanneer u de tekst **Validatie voltooid** ziet, selecteert u **Maken**.

## <a name="connect-to-a-vm-from-the-internet"></a>Verbinding maken met een virtuele machine via internet

Nadat u *myVm1* hebt gemaakt, maakt u hiermee verbinding via internet.

1. Voer in de zoekbalk van de portal *myVm1* in.

1. Selecteer de knop **Verbinding maken**.

    ![Verbinding maken met een virtuele machine](./media/quick-create-portal/connect-to-virtual-machine.png)

    Na het selecteren van de knop **Verbinden** wordt **Verbinden met virtuele machine** geopend.

1. Selecteer **RDP-bestand downloaden**. Azure maakt een Remote Desktop Protocol-bestand (*.rdp*) en downloadt het bestand naar uw computer.

1. Open het gedownloade *RDP*-bestand.

    1. Selecteer **Verbinding maken** wanneer hierom wordt gevraagd.

    1. Voer de gebruikersnaam en het wachtwoord in die u hebt opgegeven bij het maken van de virtuele machine.

        > [!NOTE]
        > Mogelijk moet u **Meer opties** > **Een ander account gebruiken** selecteren om de referenties op te geven die u hebt ingevoerd tijdens het maken van de VM.

1. Selecteer **OK**.

1. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Als er een certificaatwaarschuwing wordt weergegeven, selecteert u **Ja** of **Doorgaan**.

1. Wanneer het VM-bureaublad wordt weergegeven, minimaliseert u het om terug te gaan naar het lokale bureaublad.

## <a name="communicate-between-vms"></a>Communiceren tussen VM's

1. Open PowerShell in het externe bureaublad van *myVm1*.

1. Voer `ping myVm2` in.

    Er wordt een dergelijk bericht weergegeven:

    ```powershell
    Pinging myVm2.0v0zze1s0uiedpvtxz5z0r0cxg.bx.internal.clouda
    Request timed out.
    Request timed out.
    Request timed out.
    Request timed out.

    Ping statistics for 10.1.0.5:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
    ```

    De `ping` mislukt omdat `ping` gebruikmaakt van het Internet Control Message Protocol (ICMP). ICMP wordt standaard niet toegestaan via de Windows-firewall.

1. Voer de volgende opdracht in om *myVm2* toe te staan *myVm1* in een latere stap te pingen:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

    Met die opdracht wordt binnenkomend verkeer van ICMP via de Windows firewall toegestaan:

1. Sluit de externe bureaubladverbinding met *myVm1*.

1. Voer de stappen in [Verbinding maken met een virtuele machine via internet](#connect-to-a-vm-from-the-internet) opnieuw uit, maar maak nu verbinding met *myVm2*.

1. Voer `ping myvm1` in vanaf een opdrachtprompt.

    Er wordt een dergelijk bericht weergegeven:

    ```powershell
    Pinging myVm1.0v0zze1s0uiedpvtxz5z0r0cxg.bx.internal.cloudapp.net [10.1.0.4] with 32 bytes of data:
    Reply from 10.1.0.4: bytes=32 time=1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128

    Ping statistics for 10.1.0.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 1ms, Average = 0ms
    ```

    U ontvangt antwoorden van *myVm1* omdat u ICMP hebt toegestaan via de Windows-firewall op de VM *myVm1* in de vorige stap.

1. Sluit de externe bureaubladverbinding met *myVm2*.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met het virtuele netwerk en de VM's, verwijdert u de resourcegroep en alle resources die deze bevat:

1. Voer *myResourceGroup* in het vak **Zoeken** bovenaan de portal in.

1. Wanneer u **myResourceGroup** ziet in de zoekresultaten, selecteert u deze.

1. Selecteer **Resourcegroep verwijderen**.

1. Voer *myResourceGroup* in voor **TYP DE RESOURCEGROEPNAAM** en selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een standaard virtueel netwerk en twee virtuele machines gemaakt. U hebt met één virtuele machine verbinding gemaakt via internet en er is er privécommunicatie tussen de twee virtuele machines geweest. Zie [Een virtueel netwerk beheren](manage-virtual-network.md) voor meer informatie over instellingen voor virtuele netwerken.

Azure staat standaard onbeperkte privécommunicatie tussen virtuele machines toe. Daarentegen zijn alleen inkomende verbindingen met extern bureaublad met Windows-VM's via internet toegestaan. Ga naar de zelfstudie [Netwerkverkeer filteren](tutorial-filter-network-traffic.md) voor meer informatie over het configureren van verschillende typen VM-netwerkcommunicatie.