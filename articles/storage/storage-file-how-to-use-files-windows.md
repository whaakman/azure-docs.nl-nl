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
ms.date: 05/27/2017
ms.author: renash
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: a6d3a6f6e3457c84c5a7dc7d3601ef9495c060fe
ms.contentlocale: nl-nl
ms.lasthandoff: 07/12/2017

---

# <a name="mount-an-azure-file-share-and-access-the-share-in-windows"></a>Een Azure-bestandsshare koppelen en de share openen in Windows
[Azure File Storage](storage-dotnet-how-to-use-files.md) is het eenvoudig te gebruiken cloudbestandssysteem van Microsoft. Azure-bestandsshares kunnen worden gekoppeld in Windows en Windows Server. In dit artikel ziet u drie verschillende manieren om een Azure-bestandsshare in Windows te koppelen: met de File Explorer-gebruikersinterface, via PowerShell en via de opdrachtprompt. 

Om een Azure-bestandsshare te koppelen buiten de Azure-regio waarin deze wordt gehost, bijvoorbeeld on-premises of in een andere Azure-regio, moet het besturingssysteem ondersteuning bieden voor SMB 3.x. In de volgende tabel wordt de SMB-versie van recente Windows-versies weergegeven:

| Windows-versie | SMB-versie | Ondersteunt koppelen vanuit Azure VM | Ondersteunt koppelen vanuit on-premises | Minimaal aanbevolen KB |
|----|----|----|----|----|
| Windows 10 versie 1703 | SMB 3.1.1 | Ja | Ja | |
| Windows Server 2016 | SMB 3.1.1 | Ja | Ja | [KB4015438](https://support.microsoft.com/help/4015438) |
| Windows 10 versie 1607 | SMB 3.1.1 | Ja | Ja | [KB4015438](https://support.microsoft.com/help/4015438) | 
| Windows 10 versie 1511 | SMB 3.1.1 | Ja | Ja | [KB4013198](https://support.microsoft.com/help/4013198) |
| Windows 10 versie 1507 | SMB 3.1.1 | Ja | Ja | [KB4012606](https://support.microsoft.com/help/4012606) | 
| Windows 8.1 | SMB 3.0.2 | Ja | Ja | [KB4012216](https://support.microsoft.com/help/4012216) |
| Windows Server 2012 R2 | SMB 3.0.2 | Ja | Ja | [KB4012216](https://support.microsoft.com/help/4012216) |
| Windows Server 2012 | SMB 3.0 | Ja | Ja | [KB4012214](https://support.microsoft.com/help/4012214) |
| Windows 7 | SMB 2.1 | Ja | Nee | [KB4012215](https://support.microsoft.com/help/4012215) |
| Windows Server 2008 R2 | SMB 2.1 | Ja | Nee | [KB4012215](https://support.microsoft.com/help/4012215) |

> [!Note]  
> We raden altijd aan de meest recente KB voor uw versie van Windows te nemen. De minimaal aanbevolen KB is bedoeld om het meest recente pakket met SMB-fixes te bieden aan IT-beheerders die niet houden van updates.

## <a name="aprerequisites-for-mounting-azure-file-share-with-windows"></a></a>Vereisten voor het koppelen van een Azure-bestandsshare met Windows 
* **Naam van het opslagaccount**: voor het koppelen van een Azure-bestandsshare hebt u de naam van het opslagaccount nodig.

* **Sleutel van het opslagaccount**: voor het koppelen van een Azure-bestandsshare hebt u de primaire (of secundaire) opslagsleutel nodig. SAS-sleutels worden momenteel niet ondersteund voor koppelen.

* **Zorg ervoor dat poort 445 is geopend**: Azure File Storage maakt gebruik van het SMB-protocol. SMB communiceert via TCP-poort 445 - controleer of de TCP-poort 445 van de clientcomputer niet door uw firewall wordt geblokkeerd.

## <a name="mount-the-azure-file-share-with-file-explorer"></a>De Azure-bestandsshare koppelen met de Verkenner
> [!Note]  
> Houd er rekening mee dat de volgende instructies worden weergegeven in Windows 10 en enigszins kunnen verschillen in oudere versies. 

1. **Open de Verkenner**: dit kan worden gedaan door deze te openen vanuit het menu Start of door op de snelkoppeling Win + E te drukken.

2. **Ga naar het item 'Deze pc' aan de linkerkant van het venster. Hiermee wijzigt u de menu's die beschikbaar zijn in het lint. Selecteer in het menu Computer 'Netwerkverbinding maken'**.
    
    ![Een schermafbeelding van de vervolgkeuzelijst 'Netwerkverbinding maken'](media/storage-file-how-to-use-files-windows/1_MountOnWindows10.png)

3. **Kopieer het UNC-pad van het deelvenster 'Verbinding maken' in Azure Portal**: een gedetailleerde beschrijving van het zoeken van deze informatie vindt u [hier](storage-file-how-to-use-files-portal.md#connect-to-file-share).

    ![Het UNC-pad in het deelvenster Verbinding maken van Azure File Storage](media/storage-file-how-to-use-files-windows/portal_netuse_connect.png)

4. **Selecteer de stationsletter en voer het UNC-pad in.** 
    
    ![Een schermafbeelding van het dialoogvenster 'Netwerkverbinding maken'](media/storage-file-how-to-use-files-windows/2_MountOnWindows10.png)

5. **Gebruik de opslagaccountnaam voorafgegaan door `Azure\` als de gebruikersnaam en een toegangssleutel als het wachtwoord.**
    
    ![Een schermafbeelding van het dialoogvenster voor netwerkreferenties](media/storage-file-how-to-use-files-windows/3_MountOnWindows10.png)

6. **Gebruik de Azure-bestandsshare naar wens**.
    
    ![De Azure-bestandsshare is nu gekoppeld](media/storage-file-how-to-use-files-windows/4_MountOnWindows10.png)

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
Raadpleeg de volgende koppelingen voor meer informatie over Azure File Storage.

* [Veelgestelde vragen](storage-files-faq.md)
* [Problemen oplossen](storage-troubleshoot-file-connection-problems.md)

### <a name="conceptual-articles-and-videos"></a>Conceptuele artikelen en video's
* [Azure File Storage: een naadloos SMB-bestandssysteem voor Windows en Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Azure File Storage gebruiken met Linux](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-azure-file-storage"></a>Hulpprogramma-ondersteuning voor Azure File Storage
* [Azure PowerShell gebruiken met Azure Storage](storage-powershell-guide-full.md)
* [AzCopy gebruiken met Microsoft Azure Storage](storage-use-azcopy.md)
* [De Azure CLI gebruiken met Azure Storage](storage-azure-cli.md#create-and-manage-file-shares)
* [Problemen met betrekking tot Azure File Storage oplossen](https://docs.microsoft.com/azure/storage/storage-troubleshoot-file-connection-problems)

### <a name="blog-posts"></a>Blogberichten
* [Azure File storage is now generally available (Azure File Storage is nu algemeen beschikbaar)](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Inside Azure File Storage (Een kijkje achter de schermen van Azure File Storage)](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Introducing Microsoft Azure File Service (Introductie van Microsoft Azure File-service)](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Gegevens migreren naar Azure File Storage](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Naslaginformatie
* [Naslaginformatie over de Storage-clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Naslaginformatie over REST API voor bestandsservices](http://msdn.microsoft.com/library/azure/dn167006.aspx)
