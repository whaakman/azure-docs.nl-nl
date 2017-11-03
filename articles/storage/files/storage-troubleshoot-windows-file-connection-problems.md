---
title: Oplossen van problemen in Windows Azure-bestanden | Microsoft Docs
description: Het oplossen van problemen in Windows Azure-bestanden
services: storage
documentationcenter: 
author: genlin
manager: willchen
editor: na
tags: storage
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2017
ms.author: genli
ms.openlocfilehash: 5aacc8a920c9343c5efa89128aabb1505fc2d9aa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshoot-azure-files-problems-in-windows"></a>Oplossen van problemen in Windows Azure-bestanden

Dit artikel worden veelvoorkomende problemen die betrekking op Microsoft Azure-bestanden hebben wanneer u verbinding vanaf een Windows-clients maakt. Het bevat ook mogelijke oorzaken en oplossingen voor deze problemen. Naast de stappen voor probleemoplossing in dit artikel kunt u ook gebruiken [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) om ervoor te zorgen dat de Windows client-omgeving juist vereisten. AzFileDiagnostics automatiseert detectie van de meeste van de problemen die worden vermeld in dit artikel en helpt bij het instellen van uw omgeving om optimale prestaties te behalen. U kunt ook deze informatie vinden in de [Azure-bestandsshares probleemoplosser](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) die bevat stappen om u te helpen problemen verbinden/toewijzing/koppelen Azure-bestandsshares.


<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a>Fout 53, fout 67 of fout 87 wanneer u koppelen of ontkoppelen van een Azure-bestandsshare

Wanneer u een bestandsshare te koppelen van on-premises of vanuit een ander datacenter, kan de volgende fouten worden weergegeven:

- Systeemfout 53 is opgetreden. Het netwerkpad is niet gevonden.
- Systeemfout 67 is opgetreden. Naam van het netwerk kan niet worden gevonden.
- Systeemfout 87 is opgetreden. De parameter is onjuist.

### <a name="cause-1-unencrypted-communication-channel"></a>Oorzaak 1: De niet-versleutelde communicatiekanaal

Verbindingen met de Azure-bestandsshares worden uit veiligheidsoverwegingen geblokkeerd als het communicatiekanaal is niet versleuteld en als de verbinding is niet geprobeerd uit hetzelfde datacenter waarin de Azure-bestandsshares zich bevinden. Alleen als de gebruiker clientbesturingssysteem SMB-versleuteling ondersteunt wordt de communicatie kanaalversleuteling geleverd.

Windows 8, Windows Server 2012 en latere versies van elk systeem te onderhandelen over aanvragen met SMB 3.0, die ondersteuning biedt voor versleuteling.

### <a name="solution-for-cause-1"></a>Oplossing voor oorzaak 1

Verbinding maken vanaf een client die een van de volgende biedt:

- Voldoet aan de vereisten van Windows 8 en Windows Server 2012 of hoger
- Verbinding maakt vanaf een virtuele machine in hetzelfde datacenter als de Azure storage-account dat wordt gebruikt voor de Azure-bestandsshare

### <a name="cause-2-port-445-is-blocked"></a>2 oorzaak: Poort 445 is geblokkeerd

Systeemfout 53 of Systeemfout 67 kan zich voordoen als de poort 445 uitgaande communicatie naar een datacenter Azure Files is geblokkeerd. De samenvatting van internetproviders die toestaan of weigeren van toegang via poort 445, Ga naar [TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).

Om te begrijpen of dit de reden achter het bericht 'Systeemfout 53' is, kunt u Portqry query uitvoeren op het eindpunt TCP:445. Als het eindpunt TCP:445 wordt weergegeven als gefilterd, is de TCP-poort geblokkeerd. Hier volgt een voorbeeldquery:

  `g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Als TCP-poort 445 wordt geblokkeerd door een regel op het netwerkpad, ziet u de volgende uitvoer:

  `TCP port 445 (microsoft-ds service): FILTERED`

Zie voor meer informatie over het gebruik van Portqry [beschrijving van het opdrachtregelprogramma Portqry.exe](https://support.microsoft.com/help/310099).

### <a name="solution-for-cause-2"></a>Oplossing voor oorzaak 2

Werken met uw IT-afdeling open poort 445 uitgaand naar [Azure IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="cause-3-ntlmv1-is-enabled"></a>3 oorzaak: NTLMv1 is ingeschakeld.

Systeemfout 53 of systeemfout 87 kan optreden als NTLMv1-communicatie is ingeschakeld op de client. NTLMv2-authenticatie biedt ondersteuning voor Azure Files. NTLMv1 ingeschakeld met maakt een minder veilige client. Daarom is communicatie geblokkeerd voor Azure Files. 

Om te bepalen of dit de oorzaak van de fout is, moet u controleren of de volgende registersubsleutel is ingesteld op een waarde van 3:

**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**

Zie voor meer informatie de [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) onderwerp op TechNet.

### <a name="solution-for-cause-3"></a>Oplossing voor oorzaak 3

Herstellen de **LmCompatibilityLevel** waarde op de standaardwaarde van 3 in de volgende registersubsleutel:

  **HKLM\SYSTEM\CurrentControlSet\Control\Lsa**

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-to-process-this-command-when-you-copy-to-an-azure-file-share"></a>Fout 1816 'Onvoldoende quota is beschikbaar om deze opdracht te verwerken' als u kopieert naar een Azure-bestandsshare

### <a name="cause"></a>Oorzaak

Fout 1816 treedt op wanneer het bereiken van de bovengrens van gelijktijdige open ingangen die zijn toegestaan voor een bestand op de computer waar de bestandsshare is wordt gekoppeld.

### <a name="solution"></a>Oplossing

Verminder het aantal gelijktijdige open ingangen door te sluiten van een aantal ingangen en probeer het vervolgens opnieuw. Zie voor meer informatie [controlelijst voor prestaties en schaalbaarheid van Microsoft Azure Storage](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-windows"></a>Trage kopiëren naar en van bestanden in Windows Azure

U ziet trage prestaties wanneer u bestanden overbrengen naar de Azure File-service.

- Als u een specifieke i/o-grootte minimumvereiste hebt, raden wij aan 1 MB te gebruiken als de i/o-grootte voor optimale prestaties.
-   Als u weet dat de uiteindelijke omvang van een bestand dat u met schrijfbewerkingen uitbreidt en de software geen compatibiliteitsproblemen wanneer de unwritten staart op het bestand nullen bevat, stelt u de bestandsgrootte van tevoren in plaats van elke schrijfbewerking waardoor het terugschrijven van een uitbreiden.
-   Gebruik de juiste copy-methode:
    -   Gebruik [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) de overdracht tussen twee bestandsshares.
    -   Gebruik [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) tussen bestandsshares op een on-premises computer.

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a>Overwegingen voor Windows 8.1 of Windows Server 2012 R2

Voor clients met Windows 8.1 of Windows Server 2012 R2, zorg ervoor dat de [KB3114025](https://support.microsoft.com/help/3114025) hotfix is geïnstalleerd. Deze hotfix verbetert de prestaties van maken en sluit verwerkt.

U kunt uitvoeren in het volgende script om te controleren of de hotfix is geïnstalleerd:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Als de hotfix is geïnstalleerd, wordt de volgende uitvoer weergegeven:

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> Windows Server 2012 R2-afbeeldingen in Azure Marketplace hebben hotfix KB3114025 standaard geïnstalleerd, vanaf December 2015.

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer"></a>Er is geen map met een stationsletter in **Mijn Computer**

Als u een Azure-bestandsshare als een beheerder met behulp van net gebruik toewijst, wordt de share lijkt te ontbreken.

### <a name="cause"></a>Oorzaak

Windows Verkenner wordt niet uitgevoerd als beheerder. Als u net gebruik van een administratieve opdrachtprompt uitvoert, kunt u het netwerkstation toewijzen als beheerder. Omdat netwerkstations gebruikersgerichte zijn, wordt het gebruikersaccount dat is vastgelegd in de stations niet weergegeven als ze worden gekoppeld onder een ander gebruikersaccount.

### <a name="solution"></a>Oplossing
De bestandsshare koppelen vanuit een opdrachtregel niet-beheerders. U kunt ook kunt u [dit TechNet-onderwerp](https://technet.microsoft.com/library/ee844140.aspx) voor het configureren van de **EnableLinkedConnections** registerwaarde.

<a id="netuse"></a>
## <a name="net-use-command-fails-if-the-storage-account-contains-a-forward-slash"></a>Opdracht net use mislukt als het opslagaccount een slash bevat

### <a name="cause"></a>Oorzaak

Een slash (/) de opdracht net use geïnterpreteerd als een opdrachtregeloptie. Als de naam van uw gebruikersaccount wordt gestart met een slash, wordt de stationstoewijzing mislukt.

### <a name="solution"></a>Oplossing

U kunt een van de volgende stappen gebruiken om het probleem te omzeilen:

- Voer de volgende PowerShell-opdracht:

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc" `

  Vanuit een batch-bestand, kunt u de opdracht uitvoeren op deze manier:

  `Echo new-smbMapping ... | powershell -command –`

- Dubbele aanhalingstekens rond de sleutel voor dit probleem--omzeilen geplaatst tenzij de slash is het eerste onjuiste teken. Als het, de interactieve modus gebruiken en voer uw wachtwoord afzonderlijk of opnieuw genereren van uw sleutels als u een sleutel die niet met een slash begint.

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-files-drive"></a>Toepassing of service heeft geen toegang tot een gekoppeld station van de Azure-bestanden

### <a name="cause"></a>Oorzaak

Schijven zijn per gebruiker gekoppeld. Als uw toepassing of service wordt uitgevoerd onder een ander gebruikersaccount dan het station gekoppeld, wordt het station niet weergegeven in de toepassing.

### <a name="solution"></a>Oplossing

Gebruik een van de volgende oplossingen:

-   Het station koppelen vanuit dezelfde gebruikersaccount dat de toepassing bevat. U kunt een hulpprogramma zoals PsExec gebruiken.
- Geeft de naam van het opslagaccount en de sleutel in de gebruikersnaam en wachtwoordparameters van het net gebruik de opdracht.

Nadat u deze instructies opvolgt, verschijnt er het volgende foutbericht weergegeven wanneer u net gebruiken voor het serviceaccount/in het netwerk uitvoert: 'Systeemfout 1312 is opgetreden. De sessie van een opgegeven gebruiker bestaat niet. Het is mogelijk al beëindigd." Als dit het geval is, zorg ervoor dat de gebruikersnaam die wordt doorgegeven aan net gebruik domeingegevens bevat (bijvoorbeeld: ' [opslagaccountnaam]. file.core.windows .net ').

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>'U kopieert een bestand op een doel geen versleuteling ondersteunt' fout

Als een bestand wordt gekopieerd via het netwerk, wordt het bestand op de broncomputer is ontsleuteld, verzonden als leesbare tekst en opnieuw versleuteld op de bestemming. Echter kunt u de volgende fout tegenkomen wanneer u probeert een versleuteld bestand kopiëren: "Kopieert u het bestand naar een bestemming die geen ondersteuning biedt voor versleuteling."

### <a name="cause"></a>Oorzaak
Dit probleem kan optreden als u van Encrypting File System (EFS gebruikmaakt). BitLocker-versleutelde bestanden kunnen worden gekopieerd naar de Azure-bestanden. Azure Files biedt echter geen ondersteuning voor NTFS EFS.

### <a name="workaround"></a>Tijdelijke oplossing
Als u wilt een bestand via het netwerk kopieert, moet u het eerst ontsleutelen. Gebruik een van de volgende methoden:

- Gebruik de **kopiëren /d** opdracht. Hierdoor kan de versleutelde bestanden worden opgeslagen als de ontsleutelde bestanden op de bestemming.
- Stel de volgende registersleutel:
  - Pad = HKLM\Software\Policies\Microsoft\Windows\System
  - Waarde type DWORD =
  - Naam CopyFileAllowDecryptedRemoteDestination =
  - Waarde = 1

Let erop dat voor het instellen van de registersleutel is van invloed op alle kopieerbewerkingen die zijn aangebracht aan netwerkshares.

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.
Als u nog hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.
