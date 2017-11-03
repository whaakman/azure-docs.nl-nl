---
title: Maken en een FreeBSD VM-installatiekopie uploaden | Microsoft Docs
description: Informatie over het maken en uploaden van een virtuele harde schijf (VHD) die het besturingssysteem FreeBSD voor het maken van een virtuele machine van Azure bevat
services: virtual-machines-linux
documentationcenter: 
author: thomas1206
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1ef30f32-61c1-4ba8-9542-801d7b18e9bf
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/08/2017
ms.author: huishao
ms.openlocfilehash: 0010e01d4333b96696680ec6fbbeee74b17f46a3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-upload-a-freebsd-vhd-to-azure"></a>Maken en een VHD FreeBSD uploaden naar Azure
In dit artikel leest u hoe maken en uploaden van een virtuele harde schijf (VHD) die het besturingssysteem FreeBSD bevat. Nadat u deze uploaden, kunt u deze als uw eigen installatiekopie maken van een virtuele machine (VM) in Azure.

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Zie voor meer informatie over het uploaden van een VHD met het Resource Manager-model [hier](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u de volgende items hebt:

* **Een Azure-abonnement**--als u geen account hebt, kunt u een in een paar minuten. Als u een MSDN-abonnement hebt, raadpleegt u [maandelijkse Azure-tegoed voor Visual Studio-abonnees](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Anders wordt informatie over hoe [maken van een gratis proefaccount](https://azure.microsoft.com/pricing/free-trial/).  
* **Azure PowerShell-hulpprogramma's**--het Azure PowerShell-module moet worden geïnstalleerd en geconfigureerd voor het gebruik van uw Azure-abonnement. Zie voor het downloaden van de module [Azure downloadt](https://azure.microsoft.com/downloads/). Een zelfstudie waarin wordt beschreven hoe u wilt installeren en configureren van de module is hier beschikbaar. Gebruik de [Azure downloadt](https://azure.microsoft.com/downloads/) cmdlet om de VHD te uploaden.
* **FreeBSD-besturingssysteem is geïnstalleerd in een .vhd-bestand**--er is een ondersteund besturingssysteem FreeBSD moet worden geïnstalleerd op een virtuele harde schijf. Er bestaan meerdere hulpprogramma's voor het VHD-bestanden maken. Bijvoorbeeld, kunt u een oplossing voor netwerkvirtualisatie zoals Hyper-V te maken van het VHD-bestand en het besturingssysteem te installeren. Zie voor instructies over het installeren en gebruiken van Hyper-V [Hyper-V installeren en een virtuele machine maken](http://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> De nieuwe VHDX-indeling wordt niet ondersteund in Azure. U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de cmdlet [convert-vhd](https://technet.microsoft.com/library/hh848454.aspx). Er is bovendien een [zelfstudie in MSDN over het gebruik van FreeBSD met Hyper-V](http://blogs.msdn.com/b/kylie/archive/2014/12/25/running-freebsd-on-hyper-v.aspx).
>
>

Deze taak omvat de volgende vijf stappen:

## <a name="step-1-prepare-the-image-for-upload"></a>Stap 1: Voorbereiden op de installatiekopie uploaden
Op de virtuele machine waar u het besturingssysteem FreeBSD hebt geïnstalleerd, voert u de volgende procedures:

1. Schakel DHCP in.

        # echo 'ifconfig_hn0="SYNCDHCP"' >> /etc/rc.conf
        # service netif restart
2. SSH inschakelen.

    Zorg ervoor dat de SSH-server is geïnstalleerd en geconfigureerd om te starten tijdens het opstarten. Dit is standaard ingeschakeld na de installatie van FreeBSD schijf. 
3. Instellen van een seriële console.

        # echo 'console="comconsole vidconsole"' >> /boot/loader.conf
        # echo 'comconsole_speed="115200"' >> /boot/loader.conf
4. Sudo installeren.

    Het root-account is uitgeschakeld in Azure. Dit betekent dat u wilt gebruikmaken van sudo van een onbevoegde gebruiker opdrachten uit te voeren met verhoogde bevoegdheden.

        # pkg install sudo
   
5. Vereisten voor Azure-Agent.

        # pkg install python27  
        # pkg install Py27-setuptools  
        # ln -s /usr/local/bin/python2.7 /usr/bin/python   
        # pkg install git
6. Azure-Agent installeren.

    De nieuwste versie van de Azure-Agent altijd vindt u op [github](https://github.com/Azure/WALinuxAgent/releases). De versie 2.0.10 officieel ondersteunt FreeBSD 10 & 10.1, en de versie 2.1.4 + (inclusief 2.2.x) ondersteunt officieel FreeBSD 10.2 en latere versies.

        # git clone https://github.com/Azure/WALinuxAgent.git  
        # cd WALinuxAgent  
        # git tag  
        …
        WALinuxAgent-2.0.16
        …
        v2.1.4
        v2.1.4.rc0
        v2.1.4.rc1

    Gebruik voor 2.0, gaan we 2.0.16 als een voorbeeld:

        # git checkout WALinuxAgent-2.0.16
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  

    2.1, gaan we gebruik 2.1.4 als voor een voorbeeld:

        # git checkout v2.1.4
        # python setup.py install  
        # ln -sf /usr/local/sbin/waagent /usr/sbin/waagent  
        # ln -sf /usr/local/sbin/waagent2.0 /usr/sbin/waagent2.0

   > [!IMPORTANT]
   > Nadat u Azure-Agent hebt geïnstalleerd, is het een goed idee om te controleren of deze wordt uitgevoerd:
   >
   >

        # waagent -version
        WALinuxAgent-2.1.4 running on freebsd 10.3
        Python: 2.7.11
        # ps auxw | grep waagent
        root   639   0.0  0.5 104620 17520 u0- I    05:17    0:00.20 python /usr/local/sbin/waagent -daemon (python2.7)
        # cat /var/log/waagent.log
7. Inrichting ervan ongedaan maakt het systeem.

    Het systeem om deze op te schonen, waardoor het geschikt is voor reprovisioning inrichting ervan ongedaan. De volgende opdracht worden ook de laatste ingerichte gebruikersaccount en de bijbehorende gegevens verwijderd:

        # echo "y" |  /usr/local/sbin/waagent -deprovision+user  
        # echo  'waagent_enable="YES"' >> /etc/rc.conf

    U kunt nu uw virtuele machine afsluiten.

## <a name="step-2-create-a-storage-account-in-azure"></a>Stap 2: Een opslagaccount maken in Azure
U moet een opslagaccount in Azure een .vhd-bestand uploaden, zodat deze kan worden gebruikt voor het maken van een virtuele machine. U kunt de klassieke Azure portal gebruiken om een opslagaccount te maken.

1. Meld u aan bij de [klassieke Azure-portal](https://manage.windowsazure.com).
2. Selecteer op de opdrachtbalk **nieuw**.
3. Selecteer **gegevensservices** > **opslag** > **snelle invoer**.

    ![Snel een opslagaccount maken](./media/freebsd-create-upload-vhd/Storage-quick-create.png)
4. Vul de velden als volgt in:

   * In de **URL** veld, typ de subdomeinnaam van een moet worden gebruikt in de URL van de opslag. De vermelding kan van 3 tot 24 cijfers en kleine letters bevatten. Deze naam wordt de hostnaam van de in de URL die wordt gebruikt voor het oplossen van Azure Blob storage, Azure Queue storage of Azure Table storage-resources voor het abonnement.
   * In de **locatie/Affiniteitsgroep** vervolgkeuzelijst, kies de **locatie of affiniteitsgroep** voor het opslagaccount. Een affiniteitsgroep kunt u uw cloudservices en opslag in hetzelfde Datacenter geplaatst.
   * In de **replicatie** veld, te bepalen of u **geografisch redundante** replicatie voor het opslagaccount. Geo-replicatie is standaard ingeschakeld. Deze optie worden uw gegevens gerepliceerd naar een secundaire locatie, zonder kosten, kunt u, zodat uw opslag failover naar die locatie als er een storing optreedt op de primaire locatie. De secundaire locatie wordt automatisch toegewezen en kan niet worden gewijzigd. Als u meer controle over de locatie van uw cloud-gebaseerde opslag vanwege wettelijke vereisten of organisatie beleid nodig hebt, kunt u geo-replicatie kunt uitschakelen. Let echter dat als u later geo-replicatie inschakelt, een eenmalige gegevensoverdracht vast bedrag brengt voor replicatie van uw bestaande gegevens naar de secundaire locatie. Storage-services zonder geo-replicatie worden aangeboden met korting. Meer informatie over het beheren van geo-replicatie van opslagaccounts vindt u hier: [Azure Storage-replicatie](../../../storage/common/storage-redundancy.md).

     ![Voer de details van opslag](./media/freebsd-create-upload-vhd/Storage-create-account.png)
5. Selecteer **Storage-Account maken**. Het account wordt nu weergegeven onder **opslag**.

    ![Storage-account is gemaakt](./media/freebsd-create-upload-vhd/Storagenewaccount.png)
6. Maak vervolgens een container voor uw geüploade VHD-bestanden. Selecteer de naam van het opslagaccount en selecteer vervolgens **Containers**.

    ![Details van de Storage-account](./media/freebsd-create-upload-vhd/storageaccount_detail.png)
7. Selecteer **maken van een Container**.

    ![Details van de Storage-account](./media/freebsd-create-upload-vhd/storageaccount_container.png)
8. In de **naam** veld, typ een naam voor de container. Klik in de **toegang** vervolgkeuzelijst, selecteer welk type toegangsbeleid dat u wilt.

    ![Containernaam](./media/freebsd-create-upload-vhd/storageaccount_containervalues.png)

   > [!NOTE]
   > Standaard is de container privé is, en alleen toegankelijk zijn voor de accounteigenaar. Als u openbare leestoegang tot de blobs in de container, maar niet aan de eigenschappen van container en metagegevens, gebruikt de **openbare Blob** optie. Als volledige openbare leestoegang voor de container en blobs, gebruikt de **openbare Container** optie.
   >
   >

## <a name="step-3-prepare-the-connection-to-azure"></a>Stap 3: De verbinding met Azure voorbereiden
Voordat u een .vhd-bestand uploadt kunt, moet u een beveiligde verbinding tussen uw computer en uw Azure-abonnement. U kunt de Azure Active Directory (Azure AD)-methode of het certificaat gebruiken om dat te doen.

### <a name="use-the-azure-ad-method-to-upload-a-vhd-file"></a>Gebruik de Azure AD-methode voor het uploaden van een .vhd-bestand
1. Open de Azure PowerShell-console.
2. Typ de volgende opdracht:  
    `Add-AzureAccount`

    Deze opdracht opent een aanmeldingspagina-venster waar u zich met uw werk- of schoolaccount aanmelden kunt.

    ![PowerShell-venster](./media/freebsd-create-upload-vhd/add_azureaccount.png)
3. Azure worden geverifieerd en de referentie-informatie wordt opgeslagen. Vervolgens wordt het venster gesloten.

### <a name="use-the-certificate-method-to-upload-a-vhd-file"></a>Gebruik de methode certificaat een .vhd-bestand uploaden
1. Open de Azure PowerShell-console.
2. Type: `Get-AzurePublishSettingsFile`.
3. Een browservenster wordt geopend en u wordt gevraagd om het .publishsettings-bestand downloaden. Dit bestand bevat informatie en een certificaat voor uw Azure-abonnement.

    ![Browser-downloadpagina](./media/freebsd-create-upload-vhd/Browser_download_GetPublishSettingsFile.png)
4. Sla het .publishsettings-bestand.
5. Type: `Import-AzurePublishSettingsFile <PathToFile>`, waarbij `<PathToFile>` het volledige pad naar het .publishsettings-bestand.

   Zie voor meer informatie [aan de slag met Azure-cmdlets](http://msdn.microsoft.com/library/windowsazure/jj554332.aspx).

   Zie voor meer informatie over het installeren en configureren van PowerShell [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

## <a name="step-4-upload-the-vhd-file"></a>Stap 4: Het VHD-bestand uploaden
Als u de VHD-bestand uploadt, kunt u deze overal opnemen in de Blob-opslag. Hieronder volgen enkele termen die u gebruikt wanneer u het bestand uploaden:

* **BlobStorageURL** is de URL voor het opslagaccount dat u in stap 2 hebt gemaakt.
* **YourImagesFolder** is de Blob storage-container waarin u wilt uw afbeeldingen op te slaan.
* **VHDName** wordt het label dat wordt weergegeven in de klassieke Azure portal voor het identificeren van de virtuele harde schijf.
* **PathToVHDFile** is het volledige pad en de naam van het VHD-bestand.

In het venster Azure PowerShell gebruikt u in de vorige stap, type:

        Add-AzureVhd -Destination "<BlobStorageURL>/<YourImagesFolder>/<VHDName>.vhd" -LocalFilePath <PathToVHDFile>

## <a name="step-5-create-a-vm-with-the-uploaded-vhd-file"></a>Stap 5: Een virtuele machine maken met het geüploade VHD-bestand
Nadat u de VHD-bestand uploadt, kunt u deze als een afbeelding toevoegen aan de lijst met aangepaste installatiekopieën die zijn gekoppeld aan uw abonnement en een virtuele machine maken met deze aangepaste installatiekopie.

1. In het venster Azure PowerShell gebruikt u in de vorige stap, type:

        Add-AzureVMImage -ImageName <Your Image's Name> -MediaLocation <location of the VHD> -OS <Type of the OS on the VHD>

   > [!NOTE]
   > Linux gebruiken als het type besturingssysteem. De huidige versie van Azure PowerShell accepteert alleen 'Linux' of 'Windows' als parameter.
   >
   >
2. Nadat u de vorige stappen hebt voltooid, de nieuwe installatiekopie wordt weergegeven wanneer u ervoor kiest de **installatiekopieën** tabblad in de klassieke Azure portal.  

    ![Een afbeelding kiezen](./media/freebsd-create-upload-vhd/addfreebsdimage.png)
3. Maak een virtuele machine uit de galerie. Deze nieuwe installatiekopie is nu beschikbaar onder **Mijn afbeeldingen**.
4. De nieuwe installatiekopie selecteren. Doorloop vervolgens de aanwijzingen voor het instellen van een hostnaam, een wachtwoord, een SSH-sleutel, enzovoort.

    ![Aangepaste installatiekopie](./media/freebsd-create-upload-vhd/createfreebsdimageinazure.png)
5. Nadat u de inrichting hebt voltooid, ziet u uw FreeBSD VM worden uitgevoerd in Azure.

    ![Afbeelding van FreeBSD in azure](./media/freebsd-create-upload-vhd/freebsdimageinazure.png)
