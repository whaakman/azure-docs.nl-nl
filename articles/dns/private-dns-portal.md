---
title: Een persoonlijke Azure DNS-zone met behulp van de Azure portal maken
description: In deze zelfstudie maakt en test u een DNS-privézone en -record in Azure DNS. Dit is een stapsgewijze handleiding voor het maken en beheren van uw eerste privé-DNS-zone en -record met behulp van de Azure portal.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 6/15/2019
ms.author: victorh
ms.openlocfilehash: e3772d8f385ed93c96f8aa2f7ee2ded8f46d4e00
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67147831"
---
# <a name="tutorial-create-an-azure-dns-private-zone-using-the-azure-portal"></a>Zelfstudie: Een persoonlijke Azure DNS-zone met behulp van de Azure portal maken

Deze zelfstudie leert u de stappen voor het maken van uw eerste privé-DNS-zone en -record met behulp van de Azure portal.

[!INCLUDE [private-dns-public-preview-notice](../../includes/private-dns-public-preview-notice.md)]

Een DNS-zone wordt gebruikt om de DNS-records voor een bepaald domein te hosten. Als u uw domein wilt hosten in Azure DNS, moet u een DNS-zone maken voor die domeinnaam. Alle DNS-records voor uw domein worden vervolgens gemaakt binnen deze DNS-zone. Als u een privé-DNS-zone wilt publiceren naar uw virtuele netwerk, geeft u de lijst op met virtuele netwerken die zijn toegestaan om records in de zone om te zetten.  Deze heten *gekoppelde* virtuele netwerken. Als automatische registratie is ingeschakeld, Azure DNS zonerecords ook bijgewerkt wanneer een virtuele machine wordt gemaakt, worden wijzigingen zijn ' IP-adres, of is verwijderd.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een privé-DNS-zone maken
> * Een virtueel netwerk maken
> * Het virtueel netwerk koppelen
> * Virtuele testmachines maken
> * Een extra DNS-record maken
> * De privézone testen

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Als u liever, u kunt deze zelfstudie volgen met behulp [Azure PowerShell](private-dns-getstarted-powershell.md) of [Azure CLI](private-dns-getstarted-cli.md).

## <a name="create-a-dns-private-zone"></a>Een privé-DNS-zone maken

Het volgende voorbeeld wordt een DNS-zone met de naam **private.contoso.com** in een resourcegroep met de naam **MyAzureResourceGroup**.

Een DNS-zone bevat de DNS-vermeldingen voor een domein. Als u uw domein wilt hosten in Azure DNS, maakt u een DNS-zone voor deze domeinnaam.

![Privé-DNS-zones zoeken](media/private-dns-portal/search-private-dns.png)

1. Typ op de portal zoekbalk **privé-dns-zones** in het zoekvak en druk op **Enter**.
1. Selecteer **privé-DNS-zone**.
2. Selecteer **privé-dns-zone maken**.

1. Op de **persoonlijke DNS-zone maken** pagina typt of selecteert u de volgende waarden:

   - **Resourcegroep**: Selecteer **nieuw**, voer *MyAzureResourceGroup*, en selecteer **OK**. De naam van de resourcegroep moet uniek zijn binnen het Azure-abonnement. 
   -  **Naam**: Type *private.contoso.com* voor dit voorbeeld.
1. Voor **resourcegroeplocatie**, selecteer **West-Centraal VS**.

1. Selecteer **Controleren + maken**.

1. Selecteer **Maken**.

Het maken van de zone kan een paar minuten duren.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

1. Selecteer in de linkerbovenhoek van de portal op de pagina **een resource maken**, klikt u vervolgens **netwerken**en selecteer vervolgens **virtueel netwerk**.
2. Voor **naam**, type **myAzureVNet**.
3. Voor **resourcegroep**, selecteer **MyAzureResourceGroup**.
4. Voor **locatie**, selecteer **West-Centraal VS**.
5. Accepteer de standaardwaarden en selecteer **maken**.

## <a name="link-the-virtual-network"></a>Het virtueel netwerk koppelen

Als u wilt de privé-DNS-zone met een virtueel netwerk koppelen, moet u de koppeling van een virtueel netwerk maken.

![Koppeling van virtueel netwerk toevoegen](media/private-dns-portal/dns-add-virtual-network-link.png)

1. Open de **MyAzureResourceGroup** resource-groep en selecteer de **private.contoso.com** privézone.
2. Selecteer in het linkerdeelvenster **virtuele netwerkkoppelingen**.
3. Selecteer **Toevoegen**.
4. Type **myLink** voor de **koppelingnaam**.
5. Voor **virtueel netwerk**, selecteer **myAzureVNet**.
6. Selecteer de **automatische registratie inschakelen** selectievakje.
7. Selecteer **OK**.

## <a name="create-the-test-virtual-machines"></a>De virtuele testmachines maken

Maak nu twee virtuele machines, zodat u uw DNS-privézone kunt testen:

1. Selecteer in de linkerbovenhoek van de portal op de pagina **een resource maken**, en selecteer vervolgens **Windows Server 2016 Datacenter**.
1. Selecteer **MyAzureResourceGroup** voor de resourcegroep.
1. Type **myVM01** : voor de naam van de virtuele machine.
1. Selecteer **West-Centraal VS** voor de **regio**.
1. Type **azureadmin** voor de gebruikersnaam van beheerder.
2. Type **Azure12345678** voor het wachtwoord en Bevestig het wachtwoord.

5. Voor **openbare binnenkomende poorten**, selecteer **geselecteerde poorten toestaat**, en selecteer vervolgens **RDP (3389)** voor **binnenkomende poorten selecteren**.
10. Accepteer de overige standaardwaarden voor de pagina en klik vervolgens op **volgende: Disks >** .
11. Accepteer de standaardinstellingen op de **schijven** pagina en klik vervolgens op **volgende: Netwerken >** .
1. Zorg ervoor dat **myAzureVNet** is geselecteerd voor het virtuele netwerk.
1. Accepteer de overige standaardwaarden voor de pagina en klik vervolgens op **volgende: Management >** .
2. Voor **diagnostische gegevens over opstarten**, selecteer **uit**, accepteer de overige standaardwaarden en selecteer vervolgens **revisie + maken**.
1. Controleer de instellingen en klik vervolgens op **maken**.

Herhaal deze stappen en een andere virtuele machine met de naam **myVM02**.

Het duurt een paar minuten voor zowel virtuele machines om te voltooien.

## <a name="create-an-additional-dns-record"></a>Een extra DNS-record maken

 Het volgende voorbeeld wordt een record met de relatieve naam **db** in de DNS-Zone **private.contoso.com**, in de resourcegroep **MyAzureResourceGroup**. De volledig gekwalificeerde naam van de recordset is **db.private.contoso.com**. Het recordtype is 'A', met het IP-adres van **myVM01**.

1. Open de **MyAzureResourceGroup** resource-groep en selecteer de **private.contoso.com** privézone.
2. Selecteer **Recordset toevoegen**.
3. Voor **naam**, type **db**.
4. Voor **IP-adres**, typt u het IP-adres ziet u voor **myVM01**. Dit moet automatisch geregistreerd wanneer de virtuele machine gestart.
5. Selecteer **OK**.

## <a name="test-the-private-zone"></a>De privézone testen

Nu kunt u de naamomzetting voor testen uw **private.contoso.com** privézone.

### <a name="configure-vms-to-allow-inbound-icmp"></a>VM’s configureren voor het toestaan van inkomende ICMP

U kunt de pingopdracht gebruiken voor het testen van naamomzetting. Configureer de firewall dus op beide virtuele machines om inkomende ICMP-pakketten toe te staan.

1. Maak verbinding met myVM01 en open een Windows PowerShell-venster met beheerdersbevoegdheden.
2. Voer de volgende opdracht uit:

   ```powershell
   New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
   ```

Herhaal voor myVM02.

### <a name="ping-the-vms-by-name"></a>De VM's pingen op naam

1. Ping vanuit de myVM02 Windows PowerShell-opdrachtprompt myVM01 met de naam van de automatisch geregistreerde hostnaam:
   ```
   ping myVM01.private.contoso.com
   ```
   De uitvoer ziet er ongeveer als volgt uit:
   ```
   PS C:\> ping myvm01.private.contoso.com

   Pinging myvm01.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time=1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 1ms, Average = 0ms
   PS C:\>
   ```
2. Ping nu de **db**-naam die u eerder hebt gemaakt:
   ```
   ping db.private.contoso.com
   ```
   De uitvoer ziet er ongeveer als volgt uit:
   ```
   PS C:\> ping db.private.contoso.com

   Pinging db.private.contoso.com [10.2.0.4] with 32 bytes of data:
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128
   Reply from 10.2.0.4: bytes=32 time<1ms TTL=128

   Ping statistics for 10.2.0.4:
       Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
   Approximate round trip times in milli-seconds:
       Minimum = 0ms, Maximum = 0ms, Average = 0ms
   PS C:\>
   ```

## <a name="delete-all-resources"></a>Alle resources verwijderen

Verwijder de **MyAzureResourceGroup** als u deze niet langer nodig hebt om de resources die u in deze zelfstudie hebt gemaakt te verwijderen.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een DNS-privézone geïmplementeerd, een DNS-record gemaakt en de zone getest.
U kunt nu meer leren over DNS-privézones.

> [!div class="nextstepaction"]
> [Azure DNS gebruiken voor persoonlijke domeinen](private-dns-overview.md)
