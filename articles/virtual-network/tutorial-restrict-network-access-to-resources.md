---
title: Toegang tot het netwerk beperken tot bronnen in PaaS - Azure-portal | Microsoft Docs
description: Informatie over het beperken van toegang tot het netwerk voor Azure-resources, zoals Azure Storage en Azure SQL Database met de service-eindpunten van een virtueel netwerk met de Azure portal te beperken.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: ''
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/14/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 9a64a5c1f63dc05cba6fdfa310b694e34bdba7d1
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="restrict-network-access-to-paas-resources-with-virtual-network-service-endpoints-using-the-azure-portal"></a>Toegang tot het netwerk beperken tot PaaS-resources met de service-eindpunten van een virtueel netwerk met de Azure portal

Service-eindpunten voor virtueel netwerk kunnen u de netwerktoegang tot bepaalde bronnen Azure-service naar een virtueel netwerksubnet beperken. U kunt ook internettoegang tot de resources verwijderen. Service-eindpunten bieden rechtstreekse verbinding met uw virtuele netwerk met de ondersteunde Azure-services, zodat u kunt persoonlijke adresruimte van het virtuele netwerk gebruiken voor toegang tot de Azure-services. Verkeer dat is bestemd voor Azure-resources via de service-eindpunten altijd blijft op de Microsoft Azure-backbone-netwerk. In dit artikel leert u hoe:

> [!div class="checklist"]
> * Een virtueel netwerk maken met één subnet
> * Toevoegen van een subnet en een service-eindpunt inschakelen
> * Een Azure-resource maken en toegang tot het netwerk laat van alleen een subnet
> * Een virtuele machine (VM) implementeren voor elk subnet
> * Toegang tot een resource van een subnet bevestigen
> * Controleer de toegang is geweigerd tot een bron van een subnet en het internet

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Aanmelden bij de Azure portal op http://portal.azure.com.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

1. Selecteer **+ maken van een resource** op het bovenste linkerbovenhoek van de Azure-portal.
2. Selecteer **Networking**, en selecteer vervolgens **virtueel netwerk**.
3. Voer, of selecteert, worden de volgende informatie en selecteer vervolgens **maken**:

    |Instelling|Waarde|
    |----|----|
    |Naam| myVirtualNetwork |
    |Adresruimte| 10.0.0.0/16|
    |Abonnement| Selecteer uw abonnement|
    |Resourcegroep | Selecteer **nieuw** en voer *myResourceGroup*.|
    |Locatie| Selecteer **VS-Oost** |
    |Subnetnaam| Openbaar|
    |Subnet-adresbereik| 10.0.0.0/24|
    |Service-eindpunten| Uitgeschakeld|

    ![Geef algemene informatie over het virtuele netwerk](./media/tutorial-restrict-network-access-to-resources/create-virtual-network.png)


## <a name="enable-a-service-endpoint"></a>Een service-eindpunt inschakelen

1. In de **zoeken in resources, services en -documenten** vak aan de bovenkant van de portal *myVirtualNetwork.* Wanneer **myVirtualNetwork** wordt weergegeven in de zoekresultaten, selecteer deze.
2. Voeg een subnet toe aan het virtuele netwerk. Onder **instellingen**, selecteer **subnetten**, en selecteer vervolgens **+ Subnet**, zoals wordt weergegeven in de volgende afbeelding:

    ![Subnet toevoegen](./media/tutorial-restrict-network-access-to-resources/add-subnet.png) 

3. Onder **subnet toevoegen**, selecteer of Voer de volgende informatie en selecteer vervolgens **OK**:

    |Instelling|Waarde|
    |----|----|
    |Naam| Privé |
    |Adresbereik| 10.0.1.0/24|
    |Service-eindpunten| Selecteer **Microsoft.Storage** onder **Services**|

## <a name="restrict-network-access-to-and-from-a-subnet"></a>Beperken van toegang tot het netwerk naar en van een subnet

1. Selecteer **+ maken van een resource** op het bovenste linkerbovenhoek van de Azure-portal.
2. Selecteer **Networking**, en selecteer vervolgens **netwerkbeveiligingsgroep**.
Onder **maken van een netwerkbeveiligingsgroep**, invoert, of selecteert, worden de volgende informatie en selecteer vervolgens **maken**:

    |Instelling|Waarde|
    |----|----|
    |Naam| myNsgPrivate |
    |Abonnement| Selecteer uw abonnement|
    |Resourcegroep | Selecteer **gebruik bestaande** en selecteer *myResourceGroup*.|
    |Locatie| Selecteer **VS-Oost** |

4. Nadat de netwerkbeveiligingsgroep is gemaakt, voert u *myNsgPrivate*, in de **zoeken in resources, services en -documenten** vak aan de bovenkant van de portal. Wanneer **myNsgPrivate** wordt weergegeven in de zoekresultaten, selecteer deze.
5. Onder **instellingen**, selecteer **uitgaande beveiligingsregels**.
6. Selecteer **+ toevoegen**.
7. Maak een regel waarmee uitgaande toegang tot het openbare IP-adressen toegewezen aan de Azure Storage-service. Voer, of selecteert, worden de volgende informatie en selecteer vervolgens **OK**:

    |Instelling|Waarde|
    |----|----|
    |Bron| Selecteer **VirtualNetwork** |
    |Poortbereiken bron| * |
    |Doel | Selecteer **Tag-Service**|
    |Bestemming servicetag | Selecteer **opslag**|
    |Poortbereik van doel| * |
    |Protocol|Alle|
    |Bewerking|Toestaan|
    |Prioriteit|100|
    |Naam|Allow-Storage-All|
8. Een regel maken waarmee een beveiligingsregel standaard uitgaande toegang tot alle openbare IP-adressen overschrijft. Stap 6 en 7 opnieuw uitvoeren, met behulp van de volgende waarden:

    |Instelling|Waarde|
    |----|----|
    |Bron| Selecteer **VirtualNetwork** |
    |Poortbereiken bron| * |
    |Doel | Selecteer **Tag-Service**|
    |Bestemming servicetag| Selecteer **Internet**|
    |Poortbereik van doel| * |
    |Protocol|Alle|
    |Bewerking|Weigeren|
    |Prioriteit|110|
    |Naam|Deny-Internet-All|

9. Onder **instellingen**, selecteer **inkomende beveiligingsregels**.
10. Selecteer **+ toevoegen**.
11. Maak een regel waarmee Remote Desktop Protocol (RDP) verkeer inkomend met het subnet vanaf elke locatie. De regel wordt genegeerd voor beveiliging met een standaardregel waarmee weigert alle binnenkomend verkeer van het internet. Extern bureaublad-verbindingen zijn toegestaan voor het subnet zodat verbinding kan worden getest in een later stadium. Stap 6 en 7 opnieuw uitvoeren, met behulp van de volgende waarden:

    |Instelling|Waarde|
    |----|----|
    |Bron| Alle |
    |Poortbereiken bron| * |
    |Doel | Selecteer **Tag-Service**|
    |Bestemming servicetag| Selecteer **VirtualNetwork**|
    |Poortbereik van doel| 3389 |
    |Protocol|Alle|
    |Bewerking|Toestaan|
    |Prioriteit|120|
    |Naam|Allow-RDP-All|

12. Onder **instellingen**, selecteer **subnetten**.
13. Selecteer **+ koppelen**
14. Onder **subnet koppelen**, selecteer **virtueel netwerk** en selecteer vervolgens **myVirtualNetwork** onder **Kies een virtueel netwerk**.
15. Onder **Kies subnet**, selecteer **persoonlijke**, en selecteer vervolgens **OK**.

## <a name="restrict-network-access-to-a-resource"></a>Beperken van toegang tot het netwerk aan een resource

Stapsgewijze instructies voor het beperken van toegang tot het netwerk naar bronnen die zijn gemaakt via de Azure-services ingeschakeld voor service-eindpunten varieert op services. Zie de documentatie voor afzonderlijke services voor specifieke stappen voor elke service. De rest van dit artikel bevat stappen voor het beperken van toegang tot het netwerk voor een Azure Storage-account als voorbeeld.

### <a name="create-a-storage-account"></a>Een opslagaccount maken

1. Selecteer **+ maken van een resource** op het bovenste linkerbovenhoek van de Azure-portal.
2. Selecteer **opslag**, en selecteer vervolgens **opslagaccount - blob, bestand, tabel, wachtrij**.
3. Voer, of selecteert, worden de volgende informatie, de overige standaardinstellingen accepteren en selecteer **maken**:

    |Instelling|Waarde|
    |----|----|
    |Naam| Voer een naam die is uniek voor alle Azure locaties tussen 3 tot 24 tekens lang zijn, met behulp van alleen cijfers en kleine letters.|
    |Soort account|StorageV2 (algemeen v2)|
    |Replicatie| Lokaal redundante opslag (LRS)|
    |Abonnement| Selecteer uw abonnement|
    |Resourcegroep | Selecteer **gebruik bestaande** en selecteer *myResourceGroup*.|
    |Locatie| Selecteer **VS-Oost** |

### <a name="create-a-file-share-in-the-storage-account"></a>Een bestandsshare in de storage-account maken

1. Nadat het opslagaccount is gemaakt, voer de naam van het opslagaccount in de **zoeken in resources, services en -documenten** vak, aan de bovenkant van de portal. Wanneer de naam van uw storage-account wordt weergegeven in de zoekresultaten, selecteert u deze.
2. Selecteer **bestanden**, zoals wordt weergegeven in de volgende afbeelding:

    ![Storage-account](./media/tutorial-restrict-network-access-to-resources/storage-account.png) 
3. Selecteer **+ bestandsshare**onder **File-service**.
4. Voer *mijn-bestandsshare* onder **naam**, en selecteer vervolgens **OK**.
5. Sluit de **File-service** vak.

### <a name="enable-network-access-from-a-subnet"></a>Toegang tot het netwerk van een subnet inschakelen

Storage-accounts accepteren standaard netwerkverbindingen van clients in een netwerk. Als u toegang toestaan via alleen een specifiek subnet en weigeren van toegang tot het netwerk en alle andere netwerken, moet u de volgende stappen uitvoeren:

1. Onder **instellingen** selecteren voor het opslagaccount **Firewalls en virtuele netwerken**.
2. Onder **virtuele netwerken**, selecteer **netwerken geselecteerd**.
3. Selecteer **bestaand virtueel netwerk toevoegen**.
4. Onder **netwerken toevoegen**, selecteert u de volgende waarden en selecteer vervolgens **toevoegen**:

    |Instelling|Waarde|
    |----|----|
    |Abonnement| Selecteer uw abonnement.|
    |Virtuele netwerken|Selecteer **myVirtualNetwork**onder **virtuele netwerken**|
    |Subnetten| Selecteer **persoonlijke**onder **subnetten**|

    ![Firewalls en virtuele netwerken](./media/tutorial-restrict-network-access-to-resources/storage-firewalls-and-virtual-networks.png) 

5. Selecteer **Opslaan**.
6. Sluit de **Firewalls en virtuele netwerken** vak.
7. Onder **instellingen** selecteren voor het opslagaccount **toegangssleutels**, zoals wordt weergegeven in de volgende afbeelding:

      ![Firewalls en virtuele netwerken](./media/tutorial-restrict-network-access-to-resources/storage-access-key.png)

8. Opmerking de **sleutel** waarde, omdat u deze handmatig invoeren in een latere stap bij het toewijzen van de bestandsshare zijn toegewezen aan een stationsletter op een virtuele machine.

## <a name="create-virtual-machines"></a>Virtuele machines maken

Als u wilt testen toegang tot het netwerk naar een opslagaccount, moet u een virtuele machine implementeren met elk subnet.

### <a name="create-the-first-virtual-machine"></a>De eerste virtuele machine maken

1. Selecteer **+ maken van een resource** gevonden in de linkerbovenhoek hoek van de Azure-portal.
2. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**.
3. Voer, of selecteert, worden de volgende informatie en selecteer vervolgens **OK**:

    |Instelling|Waarde|
    |----|----|
    |Naam| myVmPublic|
    |Gebruikersnaam|Voer een gebruikersnaam van uw keuze.|
    |Wachtwoord| Voer een wachtwoord van uw keuze. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Abonnement| Selecteer uw abonnement.|
    |Resourcegroep| Selecteer **gebruik bestaande** en selecteer **myResourceGroup**.|
    |Locatie| Selecteer **VS-Oost**.|

    ![Geef algemene informatie over een virtuele machine](./media/tutorial-restrict-network-access-to-resources/virtual-machine-basics.png)
4. Selecteer een grootte voor de virtuele machine en selecteer vervolgens **Selecteer**.
5. Onder **instellingen**, selecteer **netwerk** en selecteer vervolgens **myVirtualNetwork**. Selecteer vervolgens **Subnet**, en selecteer **openbare**, zoals wordt weergegeven in de volgende afbeelding:

    ![Een virtueel netwerk selecteren](./media/tutorial-restrict-network-access-to-resources/virtual-machine-settings.png)
6. Op de **samenvatting** pagina **maken** implementatie van virtuele machine te starten. De virtuele machine duurt een paar minuten om te implementeren, maar u kunt doorgaan met de volgende stap bij het maken van de virtuele machine.

### <a name="create-the-second-virtual-machine"></a>Het tweede virtuele machine maken

Stappen 1-6 opnieuw, maar in stap 3, de virtuele machine naam *myVmPrivate* en selecteert u in stap 5, de **persoonlijke** subnet.

De virtuele machine duurt een paar minuten om te implementeren. Ga niet door met de volgende stap totdat deze gemaakt is en de instellingen in de portal geopend.

## <a name="confirm-access-to-storage-account"></a>Toegang tot de storage-account bevestigen

1. Eenmaal de *myVmPrivate* VM is gemaakt, Azure Hiermee opent u de instellingen voor deze. Verbinding maken met de virtuele machine door het selecteren van de **Connect** knop, zoals wordt weergegeven in de volgende afbeelding:

    ![Verbinding maken met een virtuele machine](./media/tutorial-restrict-network-access-to-resources/connect-to-virtual-machine.png)

2. Na het selecteren van de **Connect** knop, een Remote Desktop Protocol (RDP)-bestand wordt gemaakt en gedownload op uw computer.  
3. Open het gedownloade rdp-bestand. Als u wordt gevraagd, selecteert u **Connect**. Geef de gebruikersnaam en wachtwoord die u hebt opgegeven bij het maken van de virtuele machine. U wilt selecteren **meer opties**, klikt u vervolgens **gebruik een ander account**, de referenties die u hebt opgegeven tijdens het maken van de virtuele machine opgeven. 
4. Selecteer **OK**.
5. Er wordt mogelijk een certificaatwaarschuwing weergegeven tijdens het aanmelden. Als u de waarschuwing ontvangt, selecteert u **Ja** of **doorgaan**, om door te gaan met de verbinding.
6. Op de *myVmPrivate* VM, de Azure-bestandsshare voor het station Z met behulp van PowerShell toewijzen. Voordat u de opdrachten die volgen, vervangt `<storage-account-key>` en `<storage-account-name>` met waarden die u verstrekt en opgehaald in [een opslagaccount maken](#create-a-storage-account).

    ```powershell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\my-file-share" -Credential $credential
    ```
    
    PowerShell retourneert de uitvoer lijkt op de volgende voorbeelduitvoer:

    ```powershell
    Name           Used (GB)     Free (GB) Provider      Root
    ----           ---------     --------- --------      ----
    Z                                      FileSystem    \\vnt.file.core.windows.net\my-f...
    ```

    De Azure-bestandsshare is toegewezen aan het station Z.

7. Bevestig dat de virtuele machine geen uitgaande verbinding naar andere openbare IP-adressen vanaf een opdrachtprompt heeft:

    ```
    ping bing.com
    ```
    
    U ontvangt geen antwoorden, omdat de netwerkbeveiligingsgroep die is gekoppeld aan de *persoonlijke* subnet kan geen uitgaande toegang tot de openbare IP-adressen dan de adressen die zijn toegewezen aan de Azure Storage-service.

8. Sluit de extern-bureaubladsessie naar de *myVmPrivate* VM.

## <a name="confirm-access-is-denied-to-storage-account"></a>Toegang is geweigerd voor storage-account bevestigen

1. Voer *myVmPublic* In de **zoeken in resources, services en -documenten** vak aan de bovenkant van de portal.
2. Wanneer **myVmPublic** wordt weergegeven in de zoekresultaten, selecteer deze.
3. Voltooi de stappen 1-6 in [toegang tot de storage-account bevestigen](#confirm-access-to-storage-account) voor de *myVmPublic* VM.

    Toegang is geweigerd en u ontvangt een `New-PSDrive : Access is denied` fout. Toegang is geweigerd omdat de *myVmPublic* VM is geïmplementeerd in de *openbare* subnet. De *openbare* subnet beschikt niet over een service-eindpunt dat is ingeschakeld voor Azure Storage en het opslagaccount kunt u alleen toegang tot het netwerk van de *persoonlijke* subnet, niet de *openbare*subnet.

4. Sluit de extern-bureaubladsessie naar de *myVmPublic* VM.

5. Vanaf uw computer, blader naar de Azure [portal](https://portal.azure.com).
6. Voer de naam van het opslagaccount dat u hebt gemaakt in de **zoeken in resources, services en -documenten** vak. Wanneer de naam van uw storage-account wordt weergegeven in de zoekresultaten, selecteert u deze.
7. Selecteer **Bestanden**.
8. Het foutbericht wordt weergegeven in de volgende afbeelding:

    ![Fout: toegang geweigerd](./media/tutorial-restrict-network-access-to-resources/access-denied-error.png)

    Toegang is geweigerd, omdat de computer zich niet in de *persoonlijke* subnet van de *MyVirtualNetwork* virtueel netwerk.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, verwijdert u de resourcegroep en alle resources die deze bevat:

1. Voer *myResourceGroup* in de **Search** vak aan de bovenkant van de portal. Wanneer er **myResourceGroup** selecteren in de zoekresultaten.
2. Selecteer **Resourcegroep verwijderen**.
3. Voer *myResourceGroup* voor **TYPE de naam van RESOURCEGROEP:** en selecteer **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een service-eindpunt voor een virtueel netwerksubnet ingeschakeld. Hebt u geleerd dat de service-eindpunten kunnen worden ingeschakeld voor resources met meerdere Azure-services worden geïmplementeerd. U hebt een Azure Storage-account en een beperkte netwerktoegang tot het opslagaccount alleen bronnen binnen een virtueel netwerksubnet gemaakt. Voordat u service-eindpunten virtuele netwerken in productie maakt, wordt aanbevolen dat u zorgvuldig vertrouwd raken met [service-eindpunten](virtual-network-service-endpoints-overview.md).

Als u meerdere virtuele netwerken in uw account hebt, kunt u twee virtuele netwerken met elkaar verbinden zodat de resources in elk virtueel netwerk met elkaar kunnen communiceren. Ga naar de volgende zelfstudie voor informatie over het verbinden van virtuele netwerken.

> [!div class="nextstepaction"]
> [Verbinding maken met virtuele netwerken](./tutorial-connect-virtual-networks-portal.md)
