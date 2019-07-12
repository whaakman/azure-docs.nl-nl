---
title: 'Een virtueel netwerk maken: quickstart - Azure-portal'
titlesuffix: Azure Virtual Network
description: In deze snelstart leert u hoe u een virtueel netwerk maakt met het Azure Portal. Een virtueel netwerk kan Azure-resources, zoals virtuele machines, veilig communiceren met elkaar en met internet
services: virtual-network
documentationcenter: virtual-network
author: KumudD
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can securely communicate with each other and with the internet.
ms.service: virtual-network
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 07/08/2019
ms.author: kumud
ms.openlocfilehash: bbc40ae358a6ac7f58e01de997728db21c7eb3bc
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839717"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-portal"></a>Quickstart: Een virtueel netwerk maken met Azure Portal

Een virtueel netwerk is de fundamentele bouwstenen voor uw particuliere netwerk in Azure. Hiermee kunt Azure-resources, zoals virtuele machines (VM's), om te communiceren veilig met elkaar en met internet. In deze snelstartgids leert u hoe u een virtueel netwerk maken met de Azure-portal. Klik, u kunt twee virtuele machines implementeren in het virtuele netwerk, veilig communiceren tussen de twee virtuele machines en vanaf internet verbinding maken met de virtuele machines.


Als u nog geen abonnement op Azure hebt, maak dan nu een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

1. Selecteer linksboven in het scherm **Een resource maken** > **Netwerken** > **Virtueel netwerk**.

1. Typ of selecteer in **Virtueel netwerk maken** de volgende gegevens:

    | Instelling | Waarde |
    | ------- | ----- |
    | Name | Voer *myVirtualNetwork* in. |
    | Adresruimte | Voer *10.1.0.0/16* in. |
    | Subscription | Selecteer uw abonnement.|
    | Resource group | Selecteer **Nieuwe maken**, voer *myResourceGroup* in en selecteer vervolgens **OK**. |
    | Location | Selecteer **US - oost**.|
    | Subnet - naam | Voer *myVirtualSubnet* in. |
    | Subnet - adresbereik | Voer *10.1.0.0/24* in. |

1. Laat de overige als standaard en selecteer **maken**.

## <a name="create-virtual-machines"></a>Virtuele machines maken

Twee virtuele machines in het virtuele netwerk maken:

### <a name="create-the-first-vm"></a>De eerste VM maken

1. Selecteer aan de linkerbovenhoek van het scherm, **een resource maken** > **Compute** > **Windows Server 2019 Datacenter**.

1. Typ of selecteer in **Een virtuele machine maken - Basisprincipes** de volgende gegevens:

    | Instelling | Waarde |
    | ------- | ----- |
    | **PROJECTGEGEVENS** | |
    | Subscription | Selecteer uw abonnement. |
    | Resource group | Selecteer **myResourceGroup**. U hebt deze gemaakt in de vorige sectie. |
    | **EXEMPLAARDETAILS** |  |
    | Naam van de virtuele machine | Voer *myVm1* in. |
    | Regio | Selecteer **US - oost**. |
    | Beschikbaarheidsopties | Laat de standaardwaarde **Geen infrastructuurredundantie vereist** staan. |
    | Image | Laat de standaardwaarde **Windows Server 2019 Datacenter**. |
    | Size | Laat de standaardwaarde **Standard DS1 v2** staan. |
    | **ADMINISTRATOR-ACCOUNT** |  |
    | Gebruikersnaam | Voer een gebruikersnaam van uw keuze. |
    | Wachtwoord | Voer een wachtwoord naar keuze in. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    | Wachtwoord bevestigen | Voer het wachtwoord opnieuw in. |
    | **REGELS VOOR BINNENKOMENDE POORT** |  |
    | Openbare poorten voor inkomend verkeer | Laat de standaardwaarde **Geen** staan. |
    | **GELD BESPAREN** |  |
    | Hebt u al een Windows-licentie? | Laat de standaardwaarde **Nee** staan. |

1. Selecteer **Volgende : Schijven**.

1. Handhaaf de standaardinstellingen in **Een virtuele machine maken - schijven** en selecteer **Volgende: Netwerken**.

1. Selecteer in **Een virtuele machine maken - Netwerken** de volgende gegevens:

    | Instelling | Waarde |
    | ------- | ----- |
    | Virtueel netwerk | Laat de standaardwaarde **myVirtualNetwork** staan. |
    | Subnet | Handhaaf de standaardinstelling **myVirtualSubnet (10.1.0.0/24)** . |
    | Openbare IP | Handhaaf de standaardinstelling **(new) myVm-ip**. |
    | Openbare poorten voor inkomend verkeer | Selecteer **Geselecteerde poorten toestaan**. |
    | Binnenkomende poorten selecteren | Selecteer **HTTP** en **RDP**.

1. Selecteer **Volgende: Beheer**.

1. Selecteer in **Een virtuele machine maken - Beheer** voor **Opslagaccount voor diagnose** de optie **Nieuw**.

1. Typ of selecteer in **Opslagaccount maken** de volgende gegevens:

    | Instelling | Waarde |
    | ------- | ----- |
    | Name | Voer *myvmstorageaccount* in. Als deze naam wordt gebruikt, maakt u een unieke naam.|
    | Soort account | Laat de standaardwaarde **Storage (algemeen gebruik v1)** staan. |
    | Prestaties | Laat de standaardwaarde **Standard** staan. |
    | Replicatie | Laat de standaardwaarde **Lokaal redundante opslag (LRS)** staan. |

1. Selecteer **OK**

1. Selecteer **Controleren + maken**. Er wordt de **revisie + maken** pagina waar Azure de configuratie valideert.

1. Als u ziet de **validatie geslaagd** bericht, selecteer **maken**.

### <a name="create-the-second-vm"></a>De tweede VM maken

1. Voer bovenstaande stappen 1 tot en met 9 uit.

    > [!NOTE]
    > In stap 2 voor de **naam van de virtuele machine** geeft u *myVm2* op.
    >
    > In stap 7 voor **Opslagaccount voor diagnose** selecteert u **myvmstorageaccount**.

1. Selecteer **Controleren + maken**. De pagina **Beoordelen en maken** wordt weergegeven en de configuratie wordt gevalideerd in Azure.

1. Als u ziet de **validatie geslaagd** bericht, selecteer **maken**.

## <a name="connect-to-a-vm-from-the-internet"></a>Verbinding maken met een virtuele machine via internet

Nadat u hebt gemaakt *myVm1*, verbinding maken met internet.

1. Voer in de zoekbalk van de portal *myVm1* in.

1. Selecteer de knop **Verbinding maken**.

    ![Verbinding maken met een virtuele machine](./media/quick-create-portal/connect-to-virtual-machine.png)

    Na het selecteren van de knop **Verbinden** wordt **Verbinden met virtuele machine** geopend.

1. Selecteer **RDP-bestand downloaden**. Azure maakt een Remote Desktop Protocol-bestand ( *.rdp*) en downloadt het bestand naar uw computer.

1. Open het gedownloade *RDP*-bestand.

    1. Selecteer **Verbinding maken** wanneer hierom wordt gevraagd.

    1. Voer de gebruikersnaam en wachtwoord die u hebt opgegeven bij het maken van de virtuele machine.

        > [!NOTE]
        > Mogelijk moet u **Meer opties** > **Een ander account gebruiken** selecteren om de referenties op te geven die u hebt ingevoerd tijdens het maken van de VM.

1. Selecteer **OK**.

1. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Als er een certificaatwaarschuwing wordt weergegeven, selecteert u **Ja** of **Doorgaan**.

1. Wanneer het VM-bureaublad wordt weergegeven, minimaliseert u het om terug te gaan naar het lokale bureaublad.

## <a name="communicate-between-vms"></a>Communiceren tussen VM's

1. Open PowerShell in het externe bureaublad van *myVm1*.

1. Voer `ping myVm2` in.

    U ontvangt een bericht ongeveer als volgt uit:

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

    Met deze opdracht kunnen ICMP via de Windows-firewall binnenkomende:

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

    U ontvangt antwoorden van *myVm1*, omdat u ICMP hebt toegestaan via de Windows firewall op de *myVm1* VM in stap 3.

1. Sluit de externe bureaubladverbinding met *myVm2*.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met behulp van het virtuele netwerk en de virtuele machines, verwijdert u de resourcegroep en alle resources die deze bevat:

1. Voer *myResourceGroup* in de **zoeken** vak aan de bovenkant van de portal en selecteer **myResourceGroup** uit de lijst met zoekresultaten.

1. Selecteer **Resourcegroep verwijderen**.

1. Voer *myResourceGroup* in voor **TYP DE RESOURCEGROEPNAAM** en selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een standaard virtueel netwerk en twee virtuele machines gemaakt. U heeft verbinding gemaakt met één virtuele machine vanaf het internet en veilig worden uitgewisseld tussen de twee virtuele machines. Zie [Een virtueel netwerk beheren](manage-virtual-network.md) voor meer informatie over instellingen voor virtuele netwerken.

Standaard kan Azure onbeperkte veilige communicatie tussen VM's. Daarentegen zijn alleen inkomende verbindingen met extern bureaublad met Windows-VM's via internet toegestaan. Ga naar de zelfstudie [Netwerkverkeer filteren](tutorial-filter-network-traffic.md) voor meer informatie over het configureren van verschillende typen VM-netwerkcommunicatie.
