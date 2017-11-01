---
title: Een Azure-bestandsshare koppelen en de share openen in Windows | Microsoft Docs
description: Een Azure-bestandsshare koppelen en de share openen in Windows.
services: storage
documentationcenter: na
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/19/2017
ms.author: renash
ms.openlocfilehash: 5134fab447f1d1842369aeda4ebc1948a5d78262
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/21/2017
---
# <a name="mount-an-azure-file-share-and-access-the-share-in-windows"></a>Een Azure-bestandsshare koppelen en de share openen in Windows
[Azure Files ](storage-files-introduction.md) is het eenvoudig te gebruiken cloudbestandssysteem van Microsoft. Azure-bestandsshares kunnen worden gekoppeld in Windows en Windows Server. In dit artikel ziet u drie verschillende manieren om een Azure-bestandsshare in Windows te koppelen: met de File Explorer-gebruikersinterface, via PowerShell en via de opdrachtprompt. 

Als u een Azure-bestandsshare wilt koppelen buiten de Azure-regio waarin deze wordt gehost, bijvoorbeeld on-premises of in een andere Azure-regio, moet het besturingssysteem ondersteuning bieden voor SMB 3.0. 

U kunt Azure-bestandsshares koppelen aan een Windows-installatie die wordt uitgevoerd in een virtuele machine in Azure of on-premises. In de onderstaande tabel ziet u welke versies van het besturingssysteem het koppelen van bestandsshares ondersteunen en in welke omgeving:

| Windows-versie        | SMB-versie | Koppelbaar in Azure-VM | Koppelbaar on-premises |
|------------------------|-------------|-----------------------|----------------------|
| Windows Server Semi-Annual-kanaal<sup>1</sup> | SMB 3.0 | Ja | Ja |
| Windows 10<sup>2</sup>  | SMB 3.0 | Ja | Ja |
| Windows Server 2016    | SMB 3.0     | Ja                   | Ja                  |
| Windows 8.1            | SMB 3.0     | Ja                   | Ja                  |
| Windows Server 2012 R2 | SMB 3.0     | Ja                   | Ja                  |
| Windows Server 2012    | SMB 3.0     | Ja                   | Ja                  |
| Windows 7              | SMB 2.1     | Ja                   | Nee                   |
| Windows Server 2008 R2 | SMB 2.1     | Ja                   | Nee                   |

<sup>1</sup>Windows Server-versie 1709.  
<sup>2</sup>Windows 10-versies 1507, 1607, 1703 en 1709.

> [!Note]  
> We raden altijd aan de meest recente KB voor uw versie van Windows te nemen.

## <a name="aprerequisites-for-mounting-azure-file-share-with-windows"></a></a>Vereisten voor het koppelen van een Azure-bestandsshare met Windows 
* **Naam van het opslagaccount**: voor het koppelen van een Azure-bestandsshare hebt u de naam van het opslagaccount nodig.

* **Sleutel van het opslagaccount**: voor het koppelen van een Azure-bestandsshare hebt u de primaire (of secundaire) opslagsleutel nodig. SAS-sleutels worden momenteel niet ondersteund voor koppelen.

* **Zorg ervoor dat poort 445 is geopend**: Azure Files maakt gebruik van het SMB-protocol. SMB communiceert via TCP-poort 445 - controleer of de TCP-poort 445 van de clientcomputer niet door uw firewall wordt geblokkeerd.

## <a name="mount-the-azure-file-share-with-file-explorer"></a>De Azure-bestandsshare koppelen met de Verkenner
> [!Note]  
> Houd er rekening mee dat de volgende instructies worden weergegeven in Windows 10 en enigszins kunnen verschillen in oudere versies. 

1. **Open de Verkenner**: dit kan worden gedaan door deze te openen vanuit het menu Start of door op de snelkoppeling Win + E te drukken.

2. **Ga naar het item 'Deze pc' aan de linkerkant van het venster. Hiermee wijzigt u de menu's die beschikbaar zijn in het lint. Selecteer in het menu Computer 'Netwerkverbinding maken'**.
    
    ![Een schermafbeelding van de vervolgkeuzelijst 'Netwerkverbinding maken'](./media/storage-how-to-use-files-windows/1_MountOnWindows10.png)

3. **Kopieer het UNC-pad van het deelvenster 'Verbinding maken' in Azure Portal**: een gedetailleerde beschrijving van het zoeken van deze informatie vindt u [hier](storage-how-to-use-files-portal.md#connect-to-file-share).

    ![Het UNC-pad in het deelvenster Verbinding maken van Azure Files](./media/storage-how-to-use-files-windows/portal_netuse_connect.png)

4. **Selecteer de stationsletter en voer het UNC-pad in.** 
    
    ![Een schermafbeelding van het dialoogvenster 'Netwerkverbinding maken'](./media/storage-how-to-use-files-windows/2_MountOnWindows10.png)

5. **Gebruik de opslagaccountnaam voorafgegaan door `Azure\` als de gebruikersnaam en een toegangssleutel als het wachtwoord.**
    
    ![Een schermafbeelding van het dialoogvenster voor netwerkreferenties](./media/storage-how-to-use-files-windows/3_MountOnWindows10.png)

6. **Gebruik de Azure-bestandsshare naar wens**.
    
    ![De Azure-bestandsshare is nu gekoppeld](./media/storage-how-to-use-files-windows/4_MountOnWindows10.png)

7. **Wanneer u klaar bent om de Azure-bestandsshare te ontkoppelen (of de verbinding te verbreken), kunt u dit doen door met de rechtermuisknop in de Verkenner op de vermelding voor de share onder 'Netwerklocaties' te klikken en 'Verbinding verbreken' te selecteren**.

## <a name="mount-the-azure-file-share-with-powershell"></a>De Azure-bestandsshare koppelen met PowerShell
1. **Gebruik de volgende opdracht om de Azure-bestandsshare te koppelen**: vervang `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` door de juiste informatie.

    ```PowerShell
    $acctKey = ConvertTo-SecureString -String "<storage-account-key>" -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential -ArgumentList "Azure\<storage-account-name>", $acctKey
    New-PSDrive -Name <desired-drive-letter> -PSProvider FileSystem -Root "\\<storage-account-name>.file.core.windows.net\<share-name>" -Credential $credential
    ```

2. **Gebruik de Azure-bestandsshare naar wens**.

3. **Wanneer u klaar bent, ontkoppelt u de Azure-bestandsshare met de volgende opdracht**.

    ```PowerShell
    Remove-PSDrive -Name <desired-drive-letter>
    ```

> [!Note]  
> U kunt de parameter `-Persist` in `New-PSDrive` gebruiken om de Azure-bestandsshare zichtbaar maken voor de rest van het besturingssysteem als deze is gekoppeld.

## <a name="mount-the-azure-file-share-with-command-prompt"></a>De Azure-bestandsshare koppelen met de opdrachtprompt
1. **Gebruik de volgende opdracht om de Azure-bestandsshare te koppelen**: vervang `<storage-account-name>`, `<share-name>`, `<storage-account-key>`, `<desired-drive-letter>` door de juiste informatie.

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. **Gebruik de Azure-bestandsshare naar wens**.

3. **Wanneer u klaar bent, ontkoppelt u de Azure-bestandsshare met de volgende opdracht**.

    ```
    net use <desired-drive-letter>: /delete
    ```

> [!Note]  
> U kunt de Azure-bestandsshare configureren om automatisch opnieuw verbinding te maken bij opnieuw opstarten door de Windows-referenties persistent te maken. Met de volgende opdracht worden de referenties persistent gemaakt:
>   ```
>   cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>
>   ```

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende koppelingen voor meer informatie over Azure Files.

* [Veelgestelde vragen](../storage-files-faq.md)
* [Problemen oplossen in Windows](storage-troubleshoot-windows-file-connection-problems.md)      

### <a name="conceptual-articles-and-videos"></a>Conceptuele artikelen en video's
* [Azure Files: een naadloos SMB-bestandssysteem voor Windows en Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Azure Files gebruiken met Linux](../storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-azure-files"></a>Hulpprogramma-ondersteuning voor Azure Files
* [AzCopy gebruiken met Microsoft Azure Storage](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [De Azure CLI gebruiken met Azure Storage](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)
* [Problemen met betrekking tot Azure Files oplossen - Windows](storage-troubleshoot-windows-file-connection-problems.md)
* [Problemen met betrekking tot Azure Files oplossen - Linux](storage-troubleshoot-linux-file-connection-problems.md)

### <a name="blog-posts"></a>Blogberichten
* [Azure Files is nu algemeen beschikbaar](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Een kijkje achter de schermen van Azure Files](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Introducing Microsoft Azure File Service (Introductie van Microsoft Azure File-service)](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Gegevens migreren naar Azure File Storage](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Naslaginformatie
* [Naslaginformatie over de Storage-clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Naslaginformatie over REST API voor bestandsservices](http://msdn.microsoft.com/library/azure/dn167006.aspx)
