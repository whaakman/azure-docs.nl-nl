---
title: Problemen met Azure Files oplossen in Windows | Microsoft Docs
description: Problemen met Azure Files oplossen in Windows
services: storage
author: jeffpatt24
tags: storage
ms.service: storage
ms.topic: article
ms.date: 10/30/2018
ms.author: jeffpatt
ms.component: files
ms.openlocfilehash: 5e730e52d55f6c8c2dd02f69e3efa67017af152b
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50242973"
---
# <a name="troubleshoot-azure-files-problems-in-windows"></a>Problemen met Azure Files oplossen in Windows

Dit artikel worden veelvoorkomende problemen met betrekking tot Microsoft Azure-bestanden wanneer u verbinding vanaf Windows-clients maakt. Het biedt ook mogelijke oorzaken en oplossingen voor deze problemen. Naast de stappen in dit artikel, kunt u ook gebruiken [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) om ervoor te zorgen dat de Windows client-omgeving juiste vereisten heeft. AzFileDiagnostics automatiseert de detectie van de meeste van de symptomen die in dit artikel worden vermeld en helpt bij het instellen van uw omgeving om de optimale prestaties. U kunt ook deze informatie vinden in de [probleemoplosser voor Azure-bestandsshares](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) waarmee de stappen om u te helpen met problemen die verbinding maken/toewijzing/koppelen Azure-bestandsshares.


<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a>Fout 53 of fout 67 87 fout bij het koppelen of ontkoppelen van een Azure-bestandsshare

Wanneer u probeert te koppelen van een bestandsshare van on-premises of in een ander datacenter, ontvangt u mogelijk de volgende fouten:

- Fout 53 is opgetreden. Het netwerkpad is niet gevonden.
- Systeemfout 67 is opgetreden. Naam van het netwerk kan niet worden gevonden.
- Systeemfout 87 is opgetreden. De parameter is onjuist.

### <a name="cause-1-unencrypted-communication-channel"></a>Oorzaak 1: De niet-versleuteld communicatiekanaal

Uit veiligheidsoverwegingen worden verbindingen met Azure-bestandsshares worden geblokkeerd als het communicatiekanaal is niet versleuteld en als de verbindingspoging is niet gemaakt in hetzelfde datacenter waar de Azure-bestandsshares zich bevinden. Niet-versleutelde verbindingen binnen hetzelfde datacenter kunnen ook worden geblokkeerd als de [veilige overdracht vereist](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) instelling is ingeschakeld op het storage-account. Communicatie-kanaalversleuteling geleverd alleen als de gebruiker clientbesturingssysteem biedt ondersteuning voor SMB-versleuteling.

Windows 8, Windows Server 2012 en latere versies van elk systeem onderhandelen over aanvragen met SMB 3.0, die ondersteuning biedt voor versleuteling.

### <a name="solution-for-cause-1"></a>Oplossing voor oorzaak 1

1. Controleer of de [veilige overdracht vereist](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) instelling is uitgeschakeld op het storage-account.
2. Verbinding maken vanaf een client die een van de volgende:

    - Voldoet aan de vereisten van Windows 8 en Windows Server 2012 of hoger
    - Verbinding maakt vanaf een virtuele machine in hetzelfde datacenter als de Azure storage-account dat wordt gebruikt voor de Azure-bestandsshare

### <a name="cause-2-port-445-is-blocked"></a>2 oorzaak: Poort 445 is geblokkeerd

Fout 53 of Systeemfout 67 kan zich voordoen als poort 445 uitgaande communicatie naar een Azure Files-datacenter wordt geblokkeerd. Overzicht van de ISP's die toestaan of weigeren van toegang via poort 445, Ga naar [TechNet](http://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx).

Als u wilt weten of dit de reden achter het bericht "System error 53" is, kunt u Portqry query uitvoeren op het eindpunt TCP:445. Als het eindpunt TCP:445 wordt weergegeven als gefilterd, worden de TCP-poort is geblokkeerd. Hier volgt een voorbeeld van een query:

  `g:\DataDump\Tools\Portqry>PortQry.exe -n [storage account name].file.core.windows.net -p TCP -e 445`

Als de TCP-poort 445 wordt geblokkeerd door een regel op het netwerkpad, ziet u de volgende uitvoer:

  `TCP port 445 (microsoft-ds service): FILTERED`

Zie [Description of the Portqry.exe command-line utility](https://support.microsoft.com/help/310099) (Beschrijving van het opdrachtregelhulpprogramma Portqry.exe) voor meer informatie over het gebruik van Portqry.

### <a name="solution-for-cause-2"></a>Oplossing voor oorzaak 2

Werken met uw IT-afdeling poort 445 uitgaand naar openen [IP-adresbereiken Azure](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="cause-3-ntlmv1-is-enabled"></a>3 oorzaak: NTLMv1 is ingeschakeld

Fout 53 of systeemfout 87 kan optreden als NTLMv1-communicatie is ingeschakeld op de client. Azure Files ondersteunt alleen NTLMv2-authenticatie. Hiermee maakt u NTLMv1 ingeschakeld met een minder veilige-client. Communicatie wordt daarom geblokkeerd voor Azure Files. 

Om te bepalen of dit de oorzaak van de fout is, moet u controleren of de volgende subsleutel in het register is ingesteld op een waarde van 3:

**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**

Zie voor meer informatie de [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) onderwerp op TechNet.

### <a name="solution-for-cause-3"></a>Oplossing voor oorzaak 3

Herstellen de **LmCompatibilityLevel** waarde op de standaardwaarde van 3 in de volgende registersubsleutel:

  **HKLM\SYSTEM\CurrentControlSet\Control\Lsa**

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-to-process-this-command-when-you-copy-to-an-azure-file-share"></a>Fout 1816 "Onvoldoende quotum is beschikbaar voor het verwerken van deze opdracht" wanneer u kopieert naar een Azure-bestandsshare

### <a name="cause"></a>Oorzaak

Fout 1816 treedt op wanneer de bovenste limiet van gelijktijdige open ingangen die zijn toegestaan voor een bestand op de computer waar de bestandsshare is wordt gekoppeld is bereikt.

### <a name="solution"></a>Oplossing

Verminder het aantal gelijktijdige open ingangen door het aantal ingangen gesloten en probeer het vervolgens opnieuw. Zie voor meer informatie, [controlelijst voor de prestaties en schaalbaarheid van Microsoft Azure Storage](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-windows"></a>Trage bestand kopiëren van en naar Azure-bestanden in Windows

U ziet trage prestaties wanneer u bestanden overbrengen naar de Azure File-service.

- Als u een specifieke minimale i/o-grootte vereiste geen hebt, raden wij aan dat u 1 MiB als de i/o-grootte voor optimale prestaties.
-   Als u weet dat de uiteindelijke omvang van een bestand dat u met schrijfbewerkingen zijn uitbreiden en uw software geen compatibiliteitsproblemen wanneer de ongeschreven staart op het bestand nullen bevat, stelt u de grootte van tevoren in plaats van elke schrijven waardoor een uitbreiden schrijven.
-   Gebruik de juiste methode:
    -   Gebruik [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) de overdracht tussen twee bestandsshares.
    -   Gebruik [Robocopy](https://blogs.msdn.microsoft.com/granth/2009/12/07/multi-threaded-robocopy-for-faster-copies/) tussen bestandsshares op een on-premises computer.

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a>Overwegingen voor Windows 8.1 of Windows Server 2012 R2

Voor clients die worden uitgevoerd op Windows 8.1 of Windows Server 2012 R2, zorg ervoor dat de [KB3114025](https://support.microsoft.com/help/3114025) hotfix is geïnstalleerd. Deze hotfix verbetert de prestaties van maken en sluit verwerkt.

U kunt uitvoeren in het volgende script om te controleren of de hotfix is geïnstalleerd:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Als de hotfix is geïnstalleerd, wordt de volgende uitvoer weergegeven:

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> Windows Server 2012 R2-installatiekopieën in Azure Marketplace hebben hotfix KB3114025 standaard geïnstalleerd, vanaf December 2015.

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer"></a>Er is geen map met een stationsletter in **Mijn Computer**

Als u een Azure-bestandsshare als een beheerder met behulp van netgebruik toewijst, wordt de share blijkt te ontbreken.

### <a name="cause"></a>Oorzaak

Standaard wordt Windows Verkenner niet uitgevoerd als beheerder. Als u net use vanaf een administratieve opdrachtprompt uitvoert, kunt u het netwerkstation toewijzen als beheerder. Omdat toegewezen stations op de gebruiker gericht, wordt het gebruikersaccount dat is vastgelegd in de stations niet weergegeven als ze zijn gekoppeld onder een ander gebruikersaccount.

### <a name="solution"></a>Oplossing
De bestandsshare koppelen vanaf de opdrachtregel van een niet-beheerder. U kunt ook u kunt volgen [dit TechNet-onderwerp](https://technet.microsoft.com/library/ee844140.aspx) configureren de **EnableLinkedConnections** registerwaarde.

<a id="netuse"></a>
## <a name="net-use-command-fails-if-the-storage-account-contains-a-forward-slash"></a>Opdracht net use mislukt als het opslagaccount een schuine streep bevat

### <a name="cause"></a>Oorzaak

De opdracht net use interpreteert een slash (/) als een opdrachtregeloptie te gebruiken. Als de naam van uw gebruiker met een slash begint, mislukt de station-toewijzing.

### <a name="solution"></a>Oplossing

U kunt een van de volgende stappen gebruiken om het probleem te omzeilen:

- Voer de volgende PowerShell-opdracht uit:

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc" `

  In een batch-bestand, kunt u de opdracht uitvoeren op deze manier:

  `Echo new-smbMapping ... | powershell -command –`

- Put dubbele aanhalingstekens rond de sleutel voor dit probleem tijdelijk oplossen, tenzij het eerste teken is een slash. Als dit het geval is, gebruikt u de interactieve modus en voer uw wachtwoord afzonderlijk of opnieuw genereren van uw sleutels om op te halen van een sleutel die niet wordt gestart met een slash.

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-files-drive"></a>Toepassing of service heeft geen toegang tot een gekoppeld Azure Files-station

### <a name="cause"></a>Oorzaak

Schijven zijn per gebruiker gekoppeld. Als uw toepassing of service wordt uitgevoerd onder een ander gebruikersaccount dan het station gekoppeld, wordt het station niet weergegeven in de toepassing.

### <a name="solution"></a>Oplossing

Gebruik een van de volgende oplossingen:

-   Het station vanaf hetzelfde gebruikersaccount dat de toepassing bevat een koppeling. U kunt een hulpprogramma zoals PsExec gebruiken.
- Doorgeven van het storage-accountnaam en de sleutel in de gebruikersnaam en wachtwoordparameters van de net opdracht.
- Gebruik de opdracht cmdkey om toe te voegen van de referenties in Referentiebeheer. Dit uitvoeren vanaf een opdrachtregel in de service-accountcontext, hetzij via een interactieve aanmelding of met behulp van uitvoeren als.
  
  `cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>`
- De share rechtstreeks zonder een toegewezen stationsletter toewijzen. Sommige toepassingen kunnen niet opnieuw verbinding maken met de stationsletter correct, zodat het volledige UNC-pad mogelijk betrouwbaarder. 

  `net use * \\storage-account-name.file.core.windows.net\share`

Nadat u deze instructies volgt, mogelijk u het volgende foutbericht krijgt wanneer u net gebruiken voor het systeem-/ netwerkserviceaccount uitvoeren: 'Systeemfout 1312 is opgetreden. De sessie van een opgegeven gebruiker bestaat niet. Het is mogelijk al beëindigd." Als dit het geval is, zorg ervoor dat de gebruikersnaam die wordt doorgegeven aan net use domeininformatie bevat (bijvoorbeeld: "[storage-accountnaam]. file.core.windows .net ').

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>Fout 'U kopieert een bestand naar een bestemming die geen ondersteuning biedt voor versleuteling'

Wanneer een bestand wordt gekopieerd via het netwerk, wordt het bestand op de broncomputer ontsleuteld, verzonden als tekst zonder opmaak en opnieuw versleuteld op de bestemming. Echter, u de volgende fout kunt tegenkomen wanneer u probeert te kopiëren van een versleuteld bestand: "Kopieert u het bestand naar een bestemming die geen ondersteuning biedt voor versleuteling."

### <a name="cause"></a>Oorzaak
Dit probleem kan optreden als u van Encrypting File System (EFS gebruikmaakt). BitLocker-versleutelde bestanden kunnen worden gekopieerd naar Azure Files. Azure Files biedt echter geen ondersteuning voor NTFS EFS.

### <a name="workaround"></a>Tijdelijke oplossing
Als u wilt een bestand kopiëren via het netwerk, moet u het eerst ontsleutelen. Gebruik een van de volgende methoden:

- Gebruik de **kopiëren /d** opdracht. Hierdoor kan de versleutelde bestanden worden opgeslagen als ontsleutelde bestanden op de bestemming.
- Stel de volgende registersleutel:
  - Path = HKLM\Software\Policies\Microsoft\Windows\System
  - Waardetype = DWORD
  - Naam = CopyFileAllowDecryptedRemoteDestination
  - Waarde = 1

Let erop dat de registersleutel instellen van invloed is op alle kopieerbewerkingen die worden aangebracht in netwerkshares.

## <a name="slow-enumeration-of-files-and-folders"></a>Trage inventarisatie van bestanden en mappen

### <a name="cause"></a>Oorzaak

Dit probleem kan optreden als er geen voldoende cache op clientcomputer bij grote mappen.

### <a name="solution"></a>Oplossing

Dit probleem op te lossen aanpassen van de **DirectoryCacheEntrySizeMax** registerwaarde om toe te staan in cache plaatsen van grotere mapweergaven in de clientcomputer:

- Locatie: HKLM\System\CCS\Services\Lanmanworkstation\Parameters
- Waarde mane: DirectoryCacheEntrySizeMax 
- Type waarde: DWORD
 
 
U kunt bijvoorbeeld instellen op 0x100000 en controleren of de prestaties beter worden.


## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning.
Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om op te halen van uw probleem op te lossen snel.
