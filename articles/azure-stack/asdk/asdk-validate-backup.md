---
title: Een Azure Stack back-up met behulp van de ASDK valideren | Microsoft Docs
description: Klik hier voor meer informatie over het valideren van een Azure Stack geïntegreerde systemen back-up met behulp van de ASDK.
services: azure-stack
author: jeffgilb
manager: femila
cloud: azure-stack
ms.service: azure-stack
ms.topic: article
ms.date: 09/05/2018
ms.author: jeffgilb
ms.reviewer: hectorl
ms.lastreviewed: 09/05/2018
ms.openlocfilehash: 027d4a9f93032bfdd0f4cda96df74c92b5679540
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55251568"
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



### <a name="cloud-recovery-prerequisites"></a>Vereisten voor cloud-herstel
Voordat u begint met een cloudimplementatie voor herstel van de ASDK, zorg ervoor dat u de volgende informatie hebt:

|Vereiste|Beschrijving|
|-----|-----|
|Pad van de back-upshare.|Het UNC-sharepad van de meest recente Azure Stack back-up die wordt gebruikt voor het herstellen van informatie over de Azure Stack-infrastructuur. Deze lokale share wordt gemaakt tijdens het implementatieproces voor cloud-herstel.|
|Back-up van versleutelingssleutel.|De versleutelingssleutel die is gebruikt voor het plannen van de infrastructuur voor back-up wilt uitvoeren met behulp van de Azure Stack-beheerportal.|
|ID van de back-up te herstellen.|De back-up-ID, in de alfanumerieke vorm van 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', waarmee u de back-up kunnen worden hersteld tijdens het cloudherstel.|
|Tijd IP-adres.|Een geldige tijd IP-adres van, zoals 132.163.97.2, is vereist voor Azure Stack-implementatie.|
|Extern certificaatwachtwoord.|Het wachtwoord voor het externe certificaat gebruikt door Azure Stack. De back-up van de CA bevat externe certificaten die moeten worden hersteld met dit wachtwoord.|
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

## <a name="deploy-the-asdk-in-cloud-recovery-mode"></a>De ASDK in de herstelmodus cloud implementeren
De **InstallAzureStackPOC.ps1** script wordt gebruikt om te starten naar de cloudherstel. 

> [!IMPORTANT]
> ASDK installatie ondersteunt precies één netwerkinterfacekaart (NIC) voor netwerken. Als u meerdere NIC's hebt, zorg ervoor dat slechts één is ingeschakeld (en alle andere zijn uitgeschakeld) voordat u het implementatiescript uitvoert.

Wijzig de volgende PowerShell-opdrachten voor uw omgeving en ze voor het implementeren van de ASDK in de modus voor cloud-herstel uit te voeren:

```powershell
cd C:\CloudDeployment\Setup     
$adminPass = Get-Credential Administrator
$key = ConvertTo-SecureString "<Your backup encryption key>" -AsPlainText -Force ` 
$certPass = Read-Host -AsSecureString  

.\InstallAzureStackPOC.ps1 -AdminPassword $adminpass.Password -BackupStorePath ("\\" + $env:COMPUTERNAME + "\AzSBackups") `
-BackupEncryptionKeyBase64 $key -BackupStoreCredential $adminPass -BackupId "<Backup ID to restore>" `
-TimeServer "<Valid time server IP>" -ExternalCertPassword $certPass
```

## <a name="restore-infrastructure-data-from-backup"></a>Infrastructuur-gegevens herstellen vanaf back-up
Na de implementatie van een geslaagde cloud-herstel u nodig hebt voor het herstellen met behulp van de **terugzetten AzureStack** cmdlet. 

Na het aanmelden als de Azure Stack-operators [PowerShell voor Azure Stack installeren](asdk-post-deploy.md#install-azure-stack-powershell) en vervolgens vervangen door uw back-up-ID voor de `Name` parameter, voer de volgende opdracht uit:

```powershell
Restore-AzsBackup -Name "<BackupID>"
```
Wacht 60 minuten nadat ASDK aanroepen van deze cmdlet voor het starten van de verificatie van de back-upgegevens in de cloud worden hersteld.

## <a name="next-steps"></a>Volgende stappen
[Azure Stack registreren](asdk-register.md)

