---
title: Een Azure Stack back-up met behulp van de ASDK valideren | Microsoft Docs
description: Klik hier voor meer informatie over het valideren van een Azure Stack geïntegreerde systemen back-up met behulp van de ASDK.
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 02/15/2019
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 02/15/2019
ms.openlocfilehash: 31c5d068c8fcd0b6edea7cff63098131d848a14e
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56416375"
---
# <a name="use-the-asdk-to-validate-an-azure-stack-backup"></a>De ASDK gebruiken om te valideren van een back-up van Azure Stack
Na het implementeren van Azure Stack en de inrichting van Gebruikersresources, zoals aanbiedingen, plannen, quota en abonnementen, moet u [back-up van Azure Stack-infrastructuur inschakelen](../azure-stack-backup-enable-backup-console.md). Plannen en uitvoeren van back-ups regelmatig infrastructuur zorgt ervoor dat infrastructuur voor beheergegevens is niet verloren gaan als er een catastrofale hardware- of -fout-service.

> [!TIP]
> We raden u [uitvoeren van een on-demand back-up](../azure-stack-backup-back-up-azure-stack.md) voordat u begint met deze procedure om te controleren of u hebt een kopie van de meest recente beschikbare infrastructuur-gegevens. Zorg ervoor dat u het vastleggen van de back-up-ID nadat de back-up is voltooid. Deze ID is vereist tijdens het cloudherstel. 

Back-ups van Azure Stack-infrastructuur bevatten belangrijke gegevens over uw cloud dat kan worden hersteld tijdens het opnieuw implementeren van Azure Stack. U kunt de ASDK gebruiken voor het valideren van deze back-ups zonder enige impact op uw productie-cloud. 

Valideren van back-ups op ASDK wordt ondersteund voor de volgende scenario's:

|Scenario|Doel|
|-----|-----|
|Infrastructuur voor back-ups van een geïntegreerde oplossing valideren.|Validatie van korte levensduur hebben dat de gegevens in de back-up geldig is.|
|Meer informatie over het van de end-to-end-herstelwerkstroom.|Gebruik ASDK het valideren van de volledige back-up en herstellen van ervaring.|
|     |     |

De volgende scenario **is niet** ondersteund bij het valideren van back-ups op de ASDK:

|Scenario|Doel|
|-----|-----|
|ASDK bouwen voor het maken van back-up en herstellen.|Back-upgegevens herstellen van een eerdere versie van de ASDK naar een nieuwere versie.|
|     |     |


## <a name="cloud-recovery-deployment"></a>Cloudimplementatie voor herstel
Infrastructuur voor back-ups van uw implementatie geïntegreerde systemen kunnen worden gevalideerd door het uitvoeren van een cloudimplementatie voor herstel van de ASDK. In dit type implementatie, specifieke service-gegevens van back-up hersteld nadat de ASDK is geïnstalleerd op de hostcomputer.

### <a name="prereqs"></a>Vereisten voor cloud-herstel
Voordat u begint met een cloudimplementatie voor herstel van de ASDK, zorg ervoor dat u de volgende informatie hebt:

**Vereisten voor het installatieprogramma van UI**

*Huidige UI-installatieprogramma biedt alleen ondersteuning voor versleutelingssleutel*

|Vereiste|Description|
|-----|-----|
|Pad van de back-upshare|Het UNC-sharepad van de meest recente Azure Stack back-up die wordt gebruikt voor het herstellen van informatie over de Azure Stack-infrastructuur. Deze lokale share wordt gemaakt tijdens het implementatieproces voor cloud-herstel.|
|Back-ID om terug te zetten|De back-up-ID, in de alfanumerieke vorm van 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', waarmee u de back-up kunnen worden hersteld tijdens het cloudherstel.|
|IP-adres van tijd|Een geldige tijd IP-adres van, zoals 132.163.97.2, is vereist voor Azure Stack-implementatie.|
|Wachtwoord voor het externe certificaat|Het wachtwoord voor het externe certificaat gebruikt door Azure Stack. De back-up van de CA bevat externe certificaten die moeten worden hersteld met dit wachtwoord.|
|Back-up van versleutelingssleutel|Vereist als u een upgrade hebt uitgevoerd naar Azure Stack-versie 1901 of hoger en back-instellingen nog steeds is geconfigureerd met een versleutelingssleutel gemaakt. Versleutelingssleutel is afgeschaft vanaf 1901. Het installatieprogramma ondersteunt de versleutelingssleutel in achterwaartse compatibiliteitsmodus voor ten minste 3 releases. Wanneer u back-upinstellingen voor het gebruik van een certificaat hebt bijgewerkt, kunt u verwijzen naar de volgende tabel voor de vereiste informatie.|

|     |     | 

**Vereisten voor het installatieprogramma van PowerShell**

*Installatieprogramma van de huidige PowerShell ondersteunt sleutel of ontsleuteling versleutelingscertificaat*

|Vereiste|Description|
|-----|-----|
|Pad van de back-upshare|Het UNC-sharepad van de meest recente Azure Stack back-up die wordt gebruikt voor het herstellen van informatie over de Azure Stack-infrastructuur. Deze lokale share wordt gemaakt tijdens het implementatieproces voor cloud-herstel.|
|Back-ID om terug te zetten|De back-up-ID, in de alfanumerieke vorm van 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', waarmee u de back-up kunnen worden hersteld tijdens het cloudherstel.|
|IP-adres van tijd|Een geldige tijd IP-adres van, zoals 132.163.97.2, is vereist voor Azure Stack-implementatie.|
|Wachtwoord voor het externe certificaat|Het wachtwoord voor het externe certificaat gebruikt door Azure Stack. De back-up van de CA bevat externe certificaten die moeten worden hersteld met dit wachtwoord.|
|Ontsleutelingswachtwoord voor certificering|Optioneel. Alleen vereist als de back-up is versleuteld met behulp van een certificaat. Het wachtwoord is voor de zelf-ondertekend van het certificaat (.pfx) dat de persoonlijke sleutel die nodig zijn voor het ontsleutelen van back-upgegevens bevat.|
|Back-up van versleutelingssleutel|Optioneel. Vereist als u een upgrade hebt uitgevoerd naar Azure Stack-versie 1901 of hoger en back-instellingen nog steeds is geconfigureerd met een versleutelingssleutel gemaakt. Het installatieprogramma ondersteunt de versleutelingssleutel in achterwaartse compatibiliteitsmodus voor ten minste 3 releases. Als u back-upinstellingen voor het gebruik van een certificaat hebt bijgewerkt, kunt u het wachtwoord moet opgeven voor het ontsleutelingscertificaat.|
|     |     | 

## <a name="prepare-the-host-computer"></a>De computer voorbereiden 
Zoals in een normale ASDK-implementatie, moet het systeemomgeving ASDK host worden voorbereid voor de installatie. Wanneer de computer development kit is voorbereid, zal het opstarten van de virtuele machine CloudBuilder.vhdx vaste schijf om te beginnen met ASDK implementatie.

Op de hostcomputer ASDK downloaden van een nieuwe cloudbuilder.vhdx overeenkomt met dezelfde versie van Azure-Stack die back-up is gemaakt en volg de instructies voor [voorbereiden van de hostcomputer ASDK](asdk-prepare-host.md).

Nadat de host-server opnieuw is opgestart vanaf de cloudbuilder.vhdx, moet u een bestandsshare maken en kopieer de back-upgegevens in. De bestandsshare moet toegankelijk zijn voor het uitvoeren van setup; account De beheerder in deze voorbeeld-PowerShell-opdrachten: 

```powershell
$shares = New-Item -Path "c:\" -Name "Shares" -ItemType "directory"
$azsbackupshare = New-Item -Path $shares.FullName -Name "AzSBackups" -ItemType "directory"
New-SmbShare -Path $azsbackupshare.FullName -FullAccess ($env:computername + "\Administrator")  -Name "AzSBackups"
```

Kopieer vervolgens uw meest recente Azure Stack-back-upbestanden naar de zojuist gemaakte share. De mapstructuur in de share moet zijn: `\\<ComputerName>\AzSBackups\MASBackup\<BackupID>\`.

Kopieer ten slotte de ontsleutelingscertificaat (.pfx) naar de map certificaat: `C:\CloudDeployment\Setup\Certificates\` en wijzig de naam van het bestand `BackupDecryptionCert.pfx`.

## <a name="deploy-the-asdk-in-cloud-recovery-mode"></a>De ASDK in de herstelmodus cloud implementeren

> [!IMPORTANT]
> 1. Het huidige installatieprogramma gebruikersinterface biedt alleen ondersteuning voor versleutelingssleutel. U kunt alleen back-ups van systemen die echter ook doorgaan met de versleutelingssleutel valideren. Als de back-up op een geïntegreerd systeem- of ASDK met behulp van certificaat is versleuteld, moet u het PowerShell-installatieprogramma (**InstallAzureStackPOC.ps1**). 
> 2. Het PowerShell-installatieprogramma (**InstallAzureStackPOC.ps1**) ondersteunt versleutelingssleutel of het certificaat.
> 3. ASDK installatie ondersteunt precies één netwerkinterfacekaart (NIC) voor netwerken. Als u meerdere NIC's hebt, zorg ervoor dat slechts één is ingeschakeld (en alle andere zijn uitgeschakeld) voordat u het implementatiescript uitvoert.

### <a name="use-the-installer-ui-to-deploy-the-asdk-in-recovery-mode"></a>Gebruik de gebruikersinterface voor het implementeren van de ASDK in de herstelmodus van het installatieprogramma
De stappen in deze sectie ziet u over het implementeren van de ASDK met behulp van een grafische gebruikersinterface (GUI) dat is opgegeven door te downloaden en uitvoeren van de **asdk installer.ps1** PowerShell-script.

> [!NOTE]
> De gebruikersinterface van het installatieprogramma voor de Azure Stack Development Kit is een open-source-script op basis van WCF en PowerShell.

> [!IMPORTANT]
> Het huidige installatieprogramma gebruikersinterface biedt alleen ondersteuning voor versleutelingssleutel.

1. Nadat de computer is naar de installatiekopie van het CloudBuilder.vhdx opgestart, melden zich aan met referenties van de opgegeven wanneer u [voorbereid de hostcomputer development kit](asdk-prepare-host.md) voor ASDK installatie. Dit moet hetzelfde zijn als de development kit host lokale administrator-referenties.
2. Open een PowerShell-console met verhoogde bevoegdheid en voer de  **&lt;stationsletter > \AzureStack_Installer\asdk-installer.ps1** PowerShell-script. Het script kan nu op een ander station dan C:\ in de afbeelding CloudBuilder.vhdx zijn. Klik op **herstellen**.

    ![Installatiescript voor ASDK](media/asdk-validate-backup/1.PNG) 

3. Voer uw Azure AD directory-gegevens (optioneel) en het lokale administrator-wachtwoord voor de hostcomputer ASDK op de pagina van de identiteit-provider en referenties. Klik op **volgende**.

    ![Pagina-id en -referenties](media/asdk-validate-backup/2.PNG) 

4. Selecteer de netwerkadapter moet worden gebruikt door de hostcomputer ASDK en klik op **volgende**. Alle andere netwerkinterfaces worden, uitgeschakeld tijdens de installatie van ASDK. 

    ![Netwerkinterface-adapter](media/asdk-validate-backup/3.PNG) 

5. Geef geldige tijd-server en IP-adressen van DNS-doorstuurserver op de pagina configuratie van het netwerk. Klik op **volgende**.

    ![De configuratiepagina van netwerk](media/asdk-validate-backup/4.PNG) 

6. Wanneer de network interface card eigenschappen zijn geverifieerd, klikt u op **volgende**. 

    ![Verificatie-instellingen netwerk](media/asdk-validate-backup/5.PNG) 

7. Geef de vereiste gegevens eerder beschreven in [sectie vereisten](#prereqs) op de pagina instellingen voor back-up en de gebruikersnaam en het wachtwoord moet worden gebruikt voor toegang tot de share. Klik op **volgende**: 

   ![Back-upinstellingen pagina](media/asdk-validate-backup/6.PNG) 

8. Bekijk het script voor implementatie moet worden gebruikt voor het implementeren van de ASDK op de pagina overzicht. Klik op **implementeren** om te beginnen met de implementatie. 

    ![Overzichtspagina](media/asdk-validate-backup/7.PNG) 


### <a name="use-powershell-to-deploy-the-asdk-in-recovery-mode"></a>PowerShell gebruiken voor het implementeren van de ASDK in de herstelmodus

Wijzig de volgende PowerShell-opdrachten voor uw omgeving en ze voor het implementeren van de ASDK in de modus voor cloud-herstel uit te voeren:

**Gebruik het script InstallAzureStackPOC.ps1 cloudherstel starten met de versleutelingssleutel.**

```powershell
cd C:\CloudDeployment\Setup     
$adminpass = Read-Host -AsSecureString -Prompt "Local Administrator password"
$certPass = Read-Host -AsSecureString -Prompt "Password for the external certificate"
$backupstorecredential = Read-Host -AsSecureString -Prompt "Credential for backup share"
$key = Read-Host -AsSecureString -Prompt "Your backup encryption key"

.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass `
 -BackupStorePath ("\\" + $env:COMPUTERNAME + "\AzSBackups") `
 -BackupEncryptionKeyBase64 $key `
 -BackupStoreCredential $backupstorecredential `
 -BackupId "<Backup ID to restore>" `
 -TimeServer "<Valid time server IP>" -ExternalCertPassword $certPass
```

**Gebruik het script InstallAzureStackPOC.ps1 initiëren van cloudherstel met ontsleutelingscertificaat.**

```powershell
cd C:\CloudDeployment\Setup     
$adminpass = Read-Host -AsSecureString -Prompt "Local Administrator password"
$certPass = Read-Host -AsSecureString -Prompt "Password for the external certificate"
$backupstorecredential = Read-Host -AsSecureString -Prompt "Credential for backup share"
$decryptioncertpassword  = Read-Host -AsSecureString -Prompt "Password for the decryption certificate"

.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass `
 -BackupStorePath ("\\" + $env:COMPUTERNAME + "\AzSBackups") `
 -BackupDecryptionCertPassword $decryptioncertpassword `
 -BackupStoreCredential $backupstorecredential `
 -BackupId "<Backup ID to restore>" `
 -TimeServer "<Valid time server IP>" -ExternalCertPassword $certPass
```

## <a name="complete-cloud-recovery"></a>Volledige cloudherstel 
Na de implementatie van een geslaagde cloud-herstel u nodig hebt voor het herstellen met behulp van de **terugzetten AzureStack** cmdlet. 

Na het aanmelden als de Azure Stack-operators [PowerShell voor Azure Stack installeren](asdk-post-deploy.md#install-azure-stack-powershell) en voer de volgende opdrachten om op te geven van het certificaat en het wachtwoord moet worden gebruikt bij het herstellen van back-up:

**Modus voor herstel met certificaatbestand**

> [!NOTE] 
> Azure Stack-implementatie het ontsleutelingscertificaat voor opmaaktalen wordt om beveiligingsredenen niet bewaard is gebleven. U moet het ontsleutelingscertificaat en het bijbehorende wachtwoord opnieuw opgeven.

```powershell
$decryptioncertpassword = Read-Host -AsSecureString -Prompt "Password for the decryption certificate"
Restore-AzsBackup -ResourceId "<BackupID>" `
 -DecryptionCertPath "<path to decryption certificate with file name (.pfx)>" `
 -DecryptionCertPassword $decryptioncertpassword
```

**Modus voor herstel met versleutelingssleutel**
```powershell
$decryptioncertpassword = Read-Host -AsSecureString -Prompt "Password for the decryption certificate"
Restore-AzsBackup -ResourceId "<BackupID>"
```

Wacht 60 minuten nadat ASDK aanroepen van deze cmdlet voor het starten van de verificatie van de back-upgegevens in de cloud worden hersteld.

## <a name="next-steps"></a>Volgende stappen
[Azure Stack registreren](asdk-register.md)

