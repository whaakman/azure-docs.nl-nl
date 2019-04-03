---
title: Een volume maken voor Azure NetApp Files | Microsoft Docs
description: Dit artikel beschrijft hoe u een volume kunt maken voor Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to-article
ms.date: 3/17/2019
ms.author: b-juche
ms.openlocfilehash: c2dd8b636223e0484e36eed9fcc616dc6f19cf3a
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58877972"
---
# <a name="create-a-volume-for-azure-netapp-files"></a>Een volume maken voor Azure NetApp Files

Elke capaciteit van toepassingen kan maximaal 500 volumes hebben. Capaciteitsgebruik van een volume wordt in mindering gebracht op de ingerichte capaciteit van de pool. Azure NetApp Files ondersteunt SMBv3- en NFS-volumes. 

## <a name="before-you-begin"></a>Voordat u begint 
U dient al een capaciteitspool te hebben ingesteld.   
[Een capaciteitspool instellen](azure-netapp-files-set-up-capacity-pool.md)   
Er moet een subnet zijn gedelegeerd aan Azure NetApp Files.  
[Een subnet delegeren aan Azure NetApp Files](azure-netapp-files-delegate-subnet.md)

## <a name="create-an-nfs-volume"></a>Maken van een NFS-volume

1.  Klik op de **Volumes** blade op de blade van de capaciteit van toepassingen. 

    ![Navigeer naar de Volumes](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2.  Klik op **+ Volume toevoegen** om een volume te maken.  
    De maken die een venster van het Volume wordt weergegeven.

3.  Klik in het maken van een venster van het Volume, **maken** en geef informatie op voor de volgende velden:   
    * **Volumenaam**      
        Geef de naam op voor het volume dat u wilt maken.   

        De naam moet uniek zijn binnen de resourcegroep. De naam moet minstens drie tekens bevatten.  U kunt geen alfanumerieke tekens gebruiken.

    * **Capaciteitspool**  
        Geef de capaciteit van toepassingen waar u het volume moet worden gemaakt.

    * **Quota**  
        Geef de hoeveelheid logische opslag op die u wilt toewijzen aan het volume.  

        Het veld **Beschikbare quotum** toont hoeveel ongebruikte ruimte er is in de gekozen capaciteitspool, die u kunt gebruiken om een nieuw volume te maken. De grootte van het nieuwe volume mag niet groter zijn dan het beschikbare quotum.  

    * **Virtueel netwerk**  
        Geef het virtuele Azure-netwerk (Vnet) op dat u wilt gebruiken om het volume te benaderen.  

        Het opgegeven VNet moet een subnet bevatten dat is gedelegeerd aan Azure NetApp Files. De Azure NetApp Files-service is alleen toegankelijk vanuit hetzelfde VNet, of vanuit een VNet in dezelfde regio als het volume via VNet-peering. U kunt ook toegang tot het volume van uw on-premises netwerk via Expressroute.   

    * **Subnet**  
        Geef het subnet op dat u wilt gebruiken voor het volume.  
        Het opgegeven subnet moet zijn gedelegeerd aan Azure NetApp Files. 
        
        Als u geen subnet hebt gedelegeerd, kunt u klikken op **Nieuwe maken** op de pagina Een volume maken. Geef vervolgens op de pagina Subnet maken de subnetgegevens op, en selecteer **Microsoft.NetApp/volumes** om het subnet te delegeren aan Azure NetApp Files. In elk Vnet kan slechts één subnet worden overgedragen naar Azure NetApp bestanden.   
 
        ![ Een volume maken](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Subnet maken](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

4. Klik op **Protocol**en selecteer vervolgens **NFS** als het protocoltype voor het volume.   
    * Geef de **bestandspad** die wordt gebruikt voor het maken van het pad voor exporteren voor het nieuwe volume. Het exportpad wordt gebruikt om het volume te koppelen en benaderen.

        Het bestandspad mag alleen letters, cijfers en afbreekstreepjes ('-') bevatten. Het bestandspad moet tussen de 16 en 40 tekens lang zijn.  

    * U kunt desgewenst [export-beleid voor het NFS-volume configureren](azure-netapp-files-configure-export-policy.md)

    ![NFS-protocol opgeven](../media/azure-netapp-files/azure-netapp-files-protocol-nfs.png)

5. Klik op **beoordelen en maken** om te controleren van de details van het volume.  Klik vervolgens op **maken** het NFS-volume te maken.

    Het volume dat u hebt gemaakt, wordt weergegeven op de pagina Volumes. 
 
    Een volume neemt het abonnement, de resourcegroep en de locatiekenmerken over van de bijbehorende capaciteitspool. U kunt de implementatiestatus van het volume controleren vanuit het tabblad Meldingen.

## <a name="create-an-smb-volume"></a>Een SMB-volume maken

Azure NetApp-bestanden ondersteunt SMBv3-volumes. U moet Active Directory-verbindingen maken voordat u een SMB-volume toevoegt. 

### <a name="create-an-active-directory-connection"></a>Maak een Active Directory-verbinding

1. Uw account NetApp, klik op **Active Directory-verbindingen**, klikt u vervolgens op **Join**.  

    ![Active Directory Connections](../media/azure-netapp-files/azure-netapp-files-active-directory-connections.png)

2. Geef in het Active Directory Join-venster de volgende informatie:

    * **Primair DNS**   
        Dit is de domain controller IP-adres voor de gewenste Active Directory Domain Services voor gebruik met Azure NetApp-bestanden. 
    * **Secundair DNS**  
        Dit is het domain controller-IP-adres voor de secundaire Active Directory Domain Services voor gebruik met Azure NetApp-bestanden. 
    * **Domain**  
        Dit is de domeinnaam van uw Active Directory Domain Services die u wilt deelnemen.
    * **Het voorvoegsel voor SMB-server (computeraccount)**  
        Dit is het naamvoorvoegsel voor het computeraccount in Active Directory die Azure NetApp bestanden voor het maken van nieuwe accounts wilt gebruiken.

        Bijvoorbeeld, als de naamgevingsnorm die uw organisatie worden gebruikt voor bestandsservers NAS-01,..., NAS-02 voert NAS-045, dan hebt u 'NAS' voor het voorvoegsel. 

        De service maakt extra computeraccounts in Active Directory zo nodig.

    * **Pad van de organisatie-eenheid**  
        Dit is de LDAP-pad voor de organisatie-eenheid (OE) waar de computeraccounts van SMB-server worden gemaakt. Dat wil zeggen, OU = tweede niveau, OU = eerste niveau. 
    * Referenties, inclusief uw **gebruikersnaam** en **wachtwoord**

    ![Active Directory toevoegen](../media/azure-netapp-files/azure-netapp-files-join-active-directory.png)

3. Klik op **Deelnemen**.  

    De Active Directory-verbinding die u hebt gemaakt weergegeven.

    ![Active Directory Connections](../media/azure-netapp-files/azure-netapp-files-active-directory-connections-created.png)

### <a name="add-an-smb-volume"></a>Een SMB-volume toevoegen

1. Klik op de **Volumes** blade op de blade van de capaciteit van toepassingen. 

    ![Navigeer naar de Volumes](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2. Klik op **+ Volume toevoegen** om een volume te maken.  
    De maken die een venster van het Volume wordt weergegeven.

3. Klik in het maken van een venster van het Volume, **maken** en geef informatie op voor de volgende velden:   
    * **Volumenaam**      
        Geef de naam op voor het volume dat u wilt maken.   

        De naam moet uniek zijn binnen de resourcegroep. De naam moet minstens drie tekens bevatten.  U kunt geen alfanumerieke tekens gebruiken.

    * **Pad naar bestand**  
        Geef het bestandspad op dat wordt gebruikt om het exportpad voor het nieuwe volume te maken. Het exportpad wordt gebruikt om het volume te koppelen en benaderen.   
     
        Het bestandspad mag alleen letters, cijfers en afbreekstreepjes ('-') bevatten. Het bestandspad moet tussen de 16 en 40 tekens lang zijn.  

    * **Capaciteitspool**  
        Geef de capaciteit van toepassingen waar u het volume moet worden gemaakt.

    * **Quota**  
        Geef de hoeveelheid logische opslag op die u wilt toewijzen aan het volume.  

        Het veld **Beschikbare quotum** toont hoeveel ongebruikte ruimte er is in de gekozen capaciteitspool, die u kunt gebruiken om een nieuw volume te maken. De grootte van het nieuwe volume mag niet groter zijn dan het beschikbare quotum.  

    * **Virtueel netwerk**  
        Geef het virtuele Azure-netwerk (Vnet) op dat u wilt gebruiken om het volume te benaderen.  

        Het opgegeven VNet moet een subnet bevatten dat is gedelegeerd aan Azure NetApp Files. De Azure NetApp Files-service is alleen toegankelijk vanuit hetzelfde VNet, of vanuit een VNet in dezelfde regio als het volume via VNet-peering. U kunt ook toegang tot het volume van uw on-premises netwerk via Expressroute.   

    * **Subnet**  
        Geef het subnet op dat u wilt gebruiken voor het volume.  
        Het opgegeven subnet moet zijn gedelegeerd aan Azure NetApp Files. 
        
        Als u geen subnet hebt gedelegeerd, kunt u klikken op **Nieuwe maken** op de pagina Een volume maken. Geef vervolgens op de pagina Subnet maken de subnetgegevens op, en selecteer **Microsoft.NetApp/volumes** om het subnet te delegeren aan Azure NetApp Files. In elk Vnet kan slechts één subnet worden overgedragen naar Azure NetApp bestanden.   
 
        ![ Een volume maken](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Subnet maken](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

4. Klik op **Protocol** en voltooi de volgende informatie:  
    * Selecteer **SMB** als het protocoltype voor het volume. 
    * Selecteer uw **Active Directory** verbinding van de vervolgkeuzelijst.
    * Geef de naam van het gedeelde volume in **sharenaam**.

    ![SMB-protocol opgeven](../media/azure-netapp-files/azure-netapp-files-protocol-smb.png)

5. Klik op **beoordelen en maken** om te controleren van de details van het volume.  Klik vervolgens op **maken** om de SMB-volume te maken.

    Het volume dat u hebt gemaakt, wordt weergegeven op de pagina Volumes. 
 
    Een volume neemt het abonnement, de resourcegroep en de locatiekenmerken over van de bijbehorende capaciteitspool. U kunt de implementatiestatus van het volume controleren vanuit het tabblad Meldingen.

## <a name="next-steps"></a>Volgende stappen  

* [Een volume voor Windows- of Linux-VM's koppelen of ontkoppelen](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Exportbeleid voor een NFS-volume configureren](azure-netapp-files-configure-export-policy.md)
* [Meer informatie over de integratie van virtuele netwerken voor Azure-services](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)
