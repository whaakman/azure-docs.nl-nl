---
title: Maken van een StorSimple-ondersteuningspakket | Microsoft Docs
description: Informatie over het maken, te ontsleutelen en te bewerken van een ondersteuningspakket voor uw StorSimple-apparaat.
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: eac76f5f-5db1-4c92-af8c-54053b91e66c
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 32d20e7a8adcfc646c592213fe7395b87a93c985
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-a-storsimple-support-package"></a>Maken en beheren van een pakket StorSimple-ondersteuning
## <a name="overview"></a>Overzicht
Een StorSimple-ondersteuningspakket is een mechanisme voor eenvoudig te gebruiken dat alle relevante Logboeken om te helpen bij het oplossen van eventuele problemen StorSimple-apparaat met Microsoft Support verzamelt. De logboeken van de verzamelde zijn versleuteld en gecomprimeerd.

Deze zelfstudie bevat stapsgewijze instructies voor het maken en beheren van het ondersteuningspakket.

## <a name="create-and-upload-a-support-package-in-the-azure-classic-portal"></a>Maken en uploaden van een ondersteuningspakket in de klassieke Azure portal
U kunt maken en een ondersteuningspakket uploaden naar de website van Microsoft Support via de **onderhoud** pagina van de service in de klassieke Azure portal.

> [!NOTE]
> Het uploaden is een sleutel ondersteuning vereist. De ondersteuningstechnicus dient dit voor u in een e-mailbericht.
> 
> 

Een versleutelde en gecomprimeerde support-pakket (.cab-bestand) is gemaakt en geüpload naar de site ondersteuning. Vervolgens kan de ondersteuningstechnicus van dit pakket opgehaald van de site ondersteuning voor het oplossen van het probleem.

Voer de volgende stappen uit in de klassieke portal een ondersteuningspakket maken.

#### <a name="to-create-a-support-package-in-the-azure-classic-portal"></a>Een ondersteuningspakket maken in de klassieke Azure portal
1. Selecteer **apparaten** > **onderhoud**.
2. In de **ondersteuningspakket** sectie **ondersteuningspakket maken en uploaden**.
3. In de **ondersteuningspakket maken en uploaden** dialoogvenster de volgende handelingen uit:
   
    ![Ondersteuningspakket maken](./media/storsimple-create-manage-support-package/IC740923.png)
   
   * In de **ondersteuning sleutel** tekst en voer de sleutel. De ondersteuningstechnicus Microsoft moet deze sleutel naar u verzenden via e-mail.
   * Schakel het selectievakje in als u toestemming geven om automatisch de support-pakket te uploaden naar de website van Microsoft Support.
   * Klik op het vinkje ![Vinkje](./media/storsimple-create-manage-support-package/IC740895.png).

## <a name="manually-create-a-support-package"></a>Handmatig een ondersteuningspakket maken
In sommige gevallen moet u handmatig maken van het ondersteuningspakket via Windows PowerShell voor StorSimple. Bijvoorbeeld:

* Als u wilt dat gevoelige gegevens verwijderen uit uw logboekbestanden vóór delen met Microsoft Support.
* Als u problemen bij het uploaden van het pakket vanwege problemen met de netwerkverbinding ondervindt.

U kunt uw handmatig gegenereerde ondersteuningspakket delen met Microsoft Support via e-mail. Voer de volgende stappen uit om een ondersteuningspakket maken in Windows PowerShell voor StorSimple.

#### <a name="to-create-a-support-package-in-windows-powershell-for-storsimple"></a>Een ondersteuningspakket maken in Windows PowerShell voor StorSimple
1. Voor het starten van een Windows PowerShell-sessie als een beheerder op de externe computer die verbinding maken met uw StorSimple-apparaat wordt gebruikt, voer de volgende opdracht:
   
    `Start PowerShell`
2. Verbinding maken met de SSAdmin Console van uw apparaat in de Windows PowerShell-sessie:
   
   * Voer het volgende achter de opdrachtprompt:
     
       `$MS = New-PSSession -ComputerName <IP address for DATA 0> -Credential SSAdmin -ConfigurationName "SSAdminConsole"`
   * Voer het beheerderswachtwoord van uw apparaat in het dialoogvenster. Het standaardwachtwoord is:
     
      `Password1`
     
      ![In het dialoogvenster van PowerShell-referentie](./media/storsimple-create-manage-support-package/IC740962.png)
   * Selecteer **OK**.
   * Voer het volgende achter de opdrachtprompt:
     
      `Enter-PSSession $MS`
3. Voer de juiste opdracht in de sessie wordt geopend.
   
   * Voer voor netwerkshares die beveiligd met een wachtwoord zijn:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" –Credential "Username" -Force`
     
       U wordt gevraagd een wachtwoord, een pad naar de gedeelde netwerkmap en een wachtwoordzin voor versleuteling (omdat het ondersteuningspakket is versleuteld). Een ondersteuningspakket wordt vervolgens in de opgegeven map gemaakt.
   * Voor shares die niet beveiligd met een wachtwoord zijn, hoeft u niet de `-Credential` parameter. Voer het volgende:
     
       `Export-HcsSupportPackage –PackageTag "MySupportPackage" -Force`
     
       Het ondersteuningspakket wordt gemaakt voor beide domeincontrollers in de opgegeven gedeelde netwerkmap. Het is een versleutelde, gecomprimeerd bestand dat kan worden verzonden naar Microsoft Support voor het oplossen van problemen. Zie voor meer informatie [contact opnemen met Microsoft ondersteuning](storsimple-contact-microsoft-support.md).

### <a name="the-export-hcssupportpackage-cmdlet-parameters"></a>De parameters van de cmdlet Export-HcsSupportPackage
Met de cmdlet Export-HcsSupportPackage kunt u de volgende parameters.

| Parameter | Vereiste/optionele | Beschrijving |
| --- | --- | --- |
| `-Path` |Vereist |Gebruiken voor de locatie van de gedeelde netwerkmap waarin het ondersteuningspakket wordt geplaatst. |
| `-EncryptionPassphrase` |Vereist |Gebruiken voor een wachtwoordzin in om u te helpen bij het versleutelen van het ondersteuningspakket. |
| `-Credential` |Optioneel |Gebruik te voorzien van referenties voor toegang voor de gedeelde netwerkmap. |
| `-Force` |Optioneel |Gebruik voor de versleuteling wachtwoordzin bevestigingsstap overslaan. |
| `-PackageTag` |Optioneel |Gebruiken om op te geven van een map onder *pad* in het ondersteuningspakket wordt geplaatst. De standaardwaarde is [apparaatnaam]-[huidige datum en time:yyyy-MM-dd-HH-mm-ss]. |
| `-Scope` |Optioneel |Geef als **Cluster** (standaard) voor het maken van een ondersteuningspakket voor beide domeincontrollers. Als u maken van een pakket alleen voor de huidige controller wilt, geeft u **Controller**. |

## <a name="edit-a-support-package"></a>Een ondersteuningspakket bewerken
Nadat u een ondersteuningspakket hebt gegenereerd, moet u mogelijk om te bewerken van het pakket om gevoelige informatie te verwijderen. Dit kunnen bijvoorbeeld volumenamen apparaat IP-adressen en namen van back-up van de logboekbestanden.

> [!IMPORTANT]
> U kunt alleen een ondersteuningspakket die is gegenereerd via Windows PowerShell voor StorSimple bewerken. U kunt een pakket gemaakt in de klassieke Azure portal met de StorSimple Manager-service niet bewerken.
> 
> 

Ondersteuningspakket bewerken voordat u dit uploadt op de website Microsoft Support, het ondersteuningspakket met voor het eerst ontsleutelen, de bestanden bewerken en vervolgens opnieuw versleutelen. De volgende stappen uitvoeren.

#### <a name="to-edit-a-support-package-in-windows-powershell-for-storsimple"></a>Het bewerken van een ondersteuningspakket in Windows PowerShell voor StorSimple
1. Genereren van een ondersteuningspakket zoals is beschreven, worden eerder in [een ondersteuningspakket maken in Windows PowerShell voor StorSimple](#to-create-a-support-package-in-windows-powershell-for-storsimple).
2. [Het script downloaden](http://gallery.technet.microsoft.com/scriptcenter/Script-to-decrypt-a-a8d1ed65) lokaal op de client.
3. Importeer de module Windows PowerShell. Geef het pad naar de lokale map waarin u het script hebt gedownload. Geef voor het importeren van de module:
   
    `Import-module <Path to the folder that contains the Windows PowerShell script>`
4. Alle bestanden zijn *.aes* bestanden die worden gecomprimeerd en versleuteld. Decomprimeren en bestanden ontsleutelen, geeft u op:
   
    `Open-HcsSupportPackage <Path to the folder that contains support package files>`
   
    Houd er rekening mee dat de werkelijke bestandsextensies nu worden weergegeven voor alle bestanden.
   
    ![Voor ondersteuningspakket bewerken](./media/storsimple-create-manage-support-package/IC750706.png)
5. Wanneer u wordt gevraagd om de wachtwoordzin voor versleuteling, voert u de wachtwoordzin op die u hebt gebruikt toen u het ondersteuningspakket is gemaakt.
   
        cmdlet Open-HcsSupportPackage at command pipeline position 1
   
        Supply values for the following parameters:EncryptionPassphrase: ****
6. Blader naar de map waarin de logboekbestanden. Omdat de logboekbestanden worden nu gecomprimeerd en ontsleuteld, wordt deze oorspronkelijke bestandsextensies hebben. Deze bestanden wijzigt om klantspecifieke informatie, zoals volumenamen en IP-adressen van apparaten, verwijderen en de bestanden op te slaan.
7. Sluit de bestanden om ze met gzip wordt gecomprimeerd en versleuteld met AES-256. Dit is van de snelheid en beveiliging bij het overbrengen van het ondersteuningspakket via een netwerk. Als u wilt comprimeren en bestanden te versleutelen, voert u het volgende:
   
    `Close-HcsSupportPackage <Path to the folder that contains support package files>`
   
    ![Voor ondersteuningspakket bewerken](./media/storsimple-create-manage-support-package/IC750707.png)
8. Geef desgevraagd een wachtwoordzin voor versleuteling voor het ondersteuningspakket gewijzigde.
   
        cmdlet Close-HcsSupportPackage at command pipeline position 1
        Supply values for the following parameters:EncryptionPassphrase: ****
9. Noteer de nieuwe wachtwoordzin, zodat u het met Microsoft Support delen kunt wanneer dit wordt aangevraagd.

### <a name="example-editing-files-in-a-support-package-on-a-password-protected-share"></a>Voorbeeld: Bestanden in een ondersteuningspakket op een wachtwoord beveiligde share bewerken
Het volgende voorbeeld laat zien hoe ontsleutelen, bewerken en opnieuw versleutelen een ondersteuningspakket.

        PS C:\WINDOWS\system32> Import-module C:\Users\Default\StorSimple\SupportPackage\HCSSupportPackageTools.psm1

        PS C:\WINDOWS\system32> Open-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Open-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32> Close-HcsSupportPackage \\hcsfs\Logs\TD48\TD48Logs\C0-A\etw

        cmdlet Close-HcsSupportPackage at command pipeline position 1

        Supply values for the following parameters:

        EncryptionPassphrase: ****

        PS C:\WINDOWS\system32>

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [ondersteuningspakketten en apparaatlogboeken gebruiken om op te lossen de implementatie van uw apparaten](storsimple-troubleshoot-deployment.md#support-packages-and-device-logs-available-for-troubleshooting).
* Meer informatie over hoe [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

