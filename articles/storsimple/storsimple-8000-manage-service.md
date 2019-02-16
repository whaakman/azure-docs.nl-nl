---
title: De StorSimple Device Manager-service in Azure implementeren | Microsoft Docs
description: Uitleg over het maken en verwijderen van de StorSimple Device Manager-service in Azure portal en wordt beschreven hoe u voor het beheren van de serviceregistratiesleutel.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2018
ms.author: alkohli
ms.openlocfilehash: eb1fe69a7fb99949ac95291c33e76c1a32bf5439
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56310054"
---
# <a name="deploy-the-storsimple-device-manager-service-for-storsimple-8000-series-devices"></a>De service StorSimple Device Manager voor apparaten uit de StorSimple 8000-serie implementeren

## <a name="overview"></a>Overzicht

De StorSimple Device Manager-service wordt uitgevoerd in Microsoft Azure en verbindt met meerdere StorSimple-apparaten. Nadat u de service hebt gemaakt, kunt u deze kunt gebruiken voor het beheren van de apparaten die zijn verbonden met de StorSimple Device Manager-service vanaf een centrale locatie, waardoor de administratieve overhead worden geminimaliseerd.

Deze zelfstudie beschrijft de stappen die nodig zijn voor het maken, verwijderen, de migratie van de service en het beheer van de serviceregistratiesleutel. De informatie in dit artikel is alleen van toepassing op apparaten uit de StorSimple 8000-serie. Ga voor meer informatie over virtuele StorSimple-matrices naar [implementeren van een StorSimple Device Manager-service voor uw StorSimple Virtual Array](storsimple-virtual-array-manage-service.md).

> [!NOTE]
> -  De Azure-portal biedt ondersteuning voor apparaten met Update 5.0 of hoger. Als uw apparaat niet actueel te houden is, onmiddellijk Update 5 installeren. Ga voor meer informatie naar [Update 5 installeren](storsimple-8000-install-update-5.md). 
> - Als u een StorSimple Cloud Appliance (8010/8020), kunt u een cloudapparaat niet bijwerken. Gebruik de nieuwste versie van software voor het maken van een nieuw cloudapparaat met Update 5.0 en vervolgens een failover uitvoeren naar de nieuwe cloudapparaat hebt gemaakt. 
> - Alle apparaten met Update 4.0 of lager te maken met verminderde functionaliteit. 

## <a name="create-a-service"></a>Een service maken
Voor het maken van een StorSimple Device Manager-service, moet u beschikken over:

* Een abonnement met een Enterprise Agreement
* Een actieve Microsoft Azure storage-account
* De informatie die wordt gebruikt voor toegangsbeheer

Alleen de abonnementen, met een Enterprise Agreement zijn toegestaan. U kunt ook een standaardopslagaccount genereren wanneer u de service maakt.

Een enkele service kunt meerdere apparaten beheren. Een apparaat kan niet echter meerdere services omvatten. Een grote onderneming kan meerdere exemplaren van de service te werken met verschillende abonnementen, organisaties, of zelfs implementatie locaties hebben. 

> [!NOTE]
> U moet afzonderlijke exemplaren van StorSimple Device Manager-service voor het beheer van apparaten uit de StorSimple 8000-serie en virtuele StorSimple-matrices.

Voer de volgende stappen uit voor het maken van een service.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-8000-create-new-service.md)]


Voor elke service StorSimple Device Manager bestaat de volgende kenmerken:

* **Naam** – de naam die is toegewezen aan uw StorSimple Device Manager-service wanneer deze is gemaakt. **Naam van de service kan niet worden gewijzigd nadat de service is gemaakt. Dit geldt ook voor andere entiteiten, zoals apparaten, volumes, volumecontainers en back-upbeleid dat kunnen niet worden gewijzigd in de Azure-portal.**
* **Status** – de status van de service, die kan worden **Active**, **maken**, of **Online**.
* **Locatie** – de geografische locatie waar het StorSimple-apparaat wordt geïmplementeerd.
* **Abonnement** – het factureringsabonnement die is gekoppeld aan uw service.

## <a name="delete-a-service"></a>Een service verwijderen

Voordat u een service verwijdert, controleert u dat er zijn geen verbonden apparaten worden gebruikt. Als de service gebruikt wordt, schakelt u de verbonden apparaten. De bewerking uitschakelen wordt de verbinding tussen het apparaat en de service-Server, maar behouden de apparaatgegevens in de cloud.

> [!IMPORTANT]
> Nadat een service is verwijderd, de bewerking kan niet ongedaan worden gemaakt. Een apparaat dat is met de service moet opnieuw naar de fabrieksinstellingen worden ingesteld voordat deze kan worden gebruikt met een andere service. In dit scenario is de lokale gegevens op het apparaat, evenals de configuratie, gaan verloren.

Voer de volgende stappen uit als een service wilt verwijderen.

### <a name="to-delete-a-service"></a>Verwijderen van een service

1. Zoeken naar de service die u wilt verwijderen. Klik op **Resources** pictogram en geef de juiste voorwaarden om te zoeken. Klik op de service die u wilt verwijderen in de lijst met zoekresultaten.

    ![Search-service verwijderen](./media/storsimple-8000-manage-service/deletessdevman1.png)

2. Hiermee gaat u naar de serviceblade StorSimple Device Manager. Klik op **Verwijderen**.

    ![Service verwijderen](./media/storsimple-8000-manage-service/deletessdevman2.png)

3. Klik op **Ja** in het bevestigingsbericht. Het duurt een paar minuten voor de service moet worden verwijderd.

    ![Verwijdering bevestigen](./media/storsimple-8000-manage-service/deletessdevman3.png)

## <a name="get-the-service-registration-key"></a>De serviceregistratiesleutel ophalen

Nadat u een service hebt gemaakt, moet u uw StorSimple-apparaat registreren bij de service. Voor het registreren van uw eerste StorSimple-apparaat, moet u de serviceregistratiesleutel. Om extra apparaten registreren met een bestaande StorSimple-service, moet u zowel de registratiesleutel en de versleutelingssleutel voor servicegegevens (die wordt gegenereerd op het eerste apparaat tijdens de registratie). Zie voor meer informatie over de versleutelingssleutel voor servicegegevens [StorSimple security](storsimple-8000-security.md). U kunt de registratiesleutel ophalen door het openen van **sleutels** op de blade StorSimple Device Manager.

De volgende stappen uitvoeren om de serviceregistratiesleutel ophalen.

[!INCLUDE [storsimple-8000-get-service-registration-key](../../includes/storsimple-8000-get-service-registration-key.md)]

Houd de serviceregistratiesleutel op een veilige locatie. U moet deze sleutel, evenals de versleutelingssleutel voor servicegegevens, extra apparaten registreren bij deze service. Nadat u de serviceregistratiesleutel, moet u uw apparaat via de Windows PowerShell voor StorSimple-interface configureren.

Zie voor meer informatie over het gebruik van deze registratiesleutel [stap 3: Configureren en registreren van het apparaat via Windows PowerShell voor StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-the-service-registration-key"></a>De serviceregistratiesleutel opnieuw genereren
U moet een serviceregistratiesleutel opnieuw genereren als u bent verplicht om uit te voeren rouleren van de sleutel of als de lijst met servicebeheerders is gewijzigd. Wanneer u de sleutel opnieuw genereert, wordt de nieuwe sleutel alleen gebruikt voor latere apparaten registreren. De apparaten die al zijn geregistreerd, worden niet beïnvloed door dit proces.

De volgende stappen uitvoeren om een serviceregistratiesleutel opnieuw genereren.

### <a name="to-regenerate-the-service-registration-key"></a>De serviceregistratiesleutel opnieuw genereren
1. In de **StorSimple Device Manager** blade, Ga naar **Management &gt;**  **sleutels**.
    
    ![De blade Sleutels](./media/storsimple-8000-manage-service/regenregkey2.png)

2. In de **sleutels** blade, klikt u op **opnieuw genereren**.

    ![Klik op sleutel opnieuw genereren](./media/storsimple-8000-manage-service/regenregkey3.png)
3. In de **serviceregistratiesleutel opnieuw genereren** blade, Controleer de actie vereist als de sleutels zijn opnieuw gegenereerd. De volgende apparaten die zijn geregistreerd bij deze service gebruikt de nieuwe registratiesleutel. Klik op **opnieuw genereren** om te bevestigen. U krijgt een melding nadat het opnieuw genereren voltooid is.

    ![Controleer of de sleutel opnieuw genereren](./media/storsimple-8000-manage-service/regenregkey4.png)

4. Een nieuwe serviceregistratiesleutel wordt weergegeven.

5. Kopieer deze sleutel en bewaar het voor het registreren van nieuwe apparaten met deze service.



## <a name="change-the-service-data-encryption-key"></a>Wijzigen van de versleutelingssleutel voor servicegegevens
De versleutelingssleutel worden gebruikt voor het versleutelen van vertrouwelijke klantgegevens, zoals opslagaccountreferenties, die uit de StorSimple Manager-service worden verzonden naar het StorSimple-apparaat. U moet deze sleutels worden periodiek gewijzigd als uw IT-organisatie een beleid voor sleutelroulatie op de opslagapparaten heeft. Het proces voor sleutel kan zijn enigszins verschillen afhankelijk van of er is een enkel apparaat of meerdere apparaten die worden beheerd door de StorSimple Manager-service. Ga voor meer informatie naar [StorSimple beveiliging en gegevensbescherming](storsimple-8000-security.md).

Wijzigen van de versleutelingssleutel voor servicegegevens is een proces 3-stappen:

1. Met Windows PowerShell-scripts voor Azure Resource Manager, toestaan dat een apparaat te wijzigen van de versleutelingssleutel voor servicegegevens.
2. Windows PowerShell voor StorSimple gebruiken, start de service gegevens codering belangrijke wijziging.
3. Als u meer dan één StorSimple-apparaat hebt, werkt u de versleutelingssleutel voor servicegegevens op de andere apparaten.

### <a name="step-1-use-windows-powershell-script-to-authorize-a-device-to-change-the-service-data-encryption-key"></a>Stap 1: Windows PowerShell-script gebruiken om te autoriseren van een apparaat te wijzigen van de versleutelingssleutel voor servicegegevens
Normaal gesproken wordt de apparaatbeheerder aanvragen dat de service-beheerder toestaan dat een apparaat te wijzigen van de versleutelingssleutel. De servicebeheerder wordt vervolgens machtigen voor het apparaat om de sleutel te wijzigen.

Deze stap wordt uitgevoerd met behulp van het script op basis van Azure Resource Manager. De servicebeheerder kan een apparaat dat in aanmerking komende Autoriseer kunt selecteren. Het apparaat is vervolgens gemachtigd om de service gegevens codering belangrijke wijzigingsproces te starten. 

Ga voor meer informatie over het gebruik van het script naar [autoriseren-ServiceEncryptionRollover.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Authorize-ServiceEncryptionRollover.ps1)

#### <a name="which-devices-can-be-authorized-to-change-service-data-encryption-keys"></a>Welke apparaten kunnen worden geautoriseerd om te wijzigen van de versleutelingssleutel?
Een apparaat moet de volgende criteria voldoen voordat deze kan worden geautoriseerd om te starten van belangrijke wijzigingen in de service gegevens versleuteling:

* Het apparaat moet online in aanmerking komt voor de service gegevens codering belangrijke wijziging autorisatie.
* Als de belangrijkste wijziging is niet gestart, kunt u hetzelfde apparaat na 30 minuten opnieuw autoriseren.
* U kunt een ander apparaat autoriseren voorwaarde dat de wijziging niet door de eerder geautoriseerde apparaat gestart is. Nadat het nieuwe apparaat is geautoriseerd, kan de wijziging niet het oude apparaat starten.
* U kunt een apparaat kan niet toestaan, terwijl de rollover van de versleutelingssleutel voor servicegegevens uitgevoerd wordt.
* Wanneer sommige van de apparaten die zijn geregistreerd bij de service via de versleuteling hebt hersteld, terwijl anderen dit niet hebt gedaan, kunt u een apparaat autoriseren. 

### <a name="step-2-use-windows-powershell-for-storsimple-to-initiate-the-service-data-encryption-key-change"></a>Stap 2: Gebruik Windows PowerShell voor StorSimple om te starten van de versleutelingssleutel voor servicegegevens wijzigen
Deze stap wordt uitgevoerd in de Windows PowerShell voor StorSimple-interface op het geautoriseerde StorSimple-apparaat.

> [!NOTE]
> Er zijn geen bewerkingen kunnen worden uitgevoerd in de Azure-portal van uw StorSimple Manager-service totdat de rollover van ondertekeningssleutel is voltooid.


Als u van de seriële console van het apparaat gebruikmaakt verbinding maken met de Windows PowerShell-interface, moet u de volgende stappen uitvoeren.

#### <a name="to-initiate-the-service-data-encryption-key-change"></a>Starten van de service gegevens codering belangrijke wijziging
1. Selecteer optie 1 om aan te melden met volledige toegang.
2. Typ in de opdrachtprompt:
   
     `Invoke-HcsmServiceDataEncryptionKeyChange`
3. Nadat de cmdlet is voltooid, ontvangt u een nieuwe versleutelingssleutel voor servicegegevens. Kopieer en bewaar deze sleutel voor gebruik in stap 3 van dit proces. Deze sleutel wordt gebruikt om bij te werken van alle overige apparaten die zijn geregistreerd bij de StorSimple Manager-service.
   
   > [!NOTE]
   > Dit proces moet worden gestart binnen vier uur van een StorSimple-apparaat te autoriseren.
   > 
   > 
   
   Deze nieuwe sleutel wordt verzonden naar de service worden gepusht naar alle apparaten die zijn geregistreerd bij de service. Een waarschuwing wordt vervolgens weergegeven op het servicedashboard. Alle bewerkingen op de geregistreerde apparaten door de service wordt uitgeschakeld en moet de apparaatbeheerder vervolgens om bij te werken van de versleutelingssleutel voor servicegegevens op de andere apparaten. De i/o's (hosts die gegevens verzenden naar de cloud) wordt echter niet worden onderbroken.
   
   Hebt u één apparaat geregistreerd bij uw service, de rollover is nu voltooid en u kunt de volgende stap overslaan. Als u meerdere apparaten die zijn geregistreerd met uw service, gaat u verder met stap 3.

### <a name="step-3-update-the-service-data-encryption-key-on-other-storsimple-devices"></a>Stap 3: Bijwerken van de versleutelingssleutel voor servicegegevens op andere StorSimple-apparaten
Deze stappen moeten worden uitgevoerd in de Windows PowerShell-interface van uw StorSimple-apparaat hebt u meerdere apparaten die zijn geregistreerd bij de StorSimple Manager-service. De sleutel die u hebt verkregen in stap 2 moet worden gebruikt om bij te werken van alle de resterende StorSimple-apparaat geregistreerd bij de StorSimple Manager-service.

Voer de volgende stappen uit voor het bijwerken van de service-gegevensversleuteling op uw apparaat.

#### <a name="to-update-the-service-data-encryption-key-on-physical-devices"></a>De versleutelingssleutel voor servicegegevens op fysieke apparaten bijwerken
1. Windows PowerShell voor StorSimple gebruiken voor verbinding met de console. Selecteer optie 1 om aan te melden met volledige toegang.
2. Bij de opdrachtprompt, typt u:  `Invoke-HcsmServiceDataEncryptionKeyChange – ServiceDataEncryptionKey`
3. Geef de versleutelingssleutel voor servicegegevens die u hebt verkregen in [stap 2: Windows PowerShell voor StorSimple gebruiken om te starten van de service gegevens codering belangrijke wijziging](#to-initiate-the-service-data-encryption-key-change).

#### <a name="to-update-the-service-data-encryption-key-on-all-the-80108020-cloud-appliances"></a>Bijwerken van de versleutelingssleutel voor servicegegevens op alle 8010/8020-cloudapparaten
1. Downloaden en installeren [Update CloudApplianceServiceEncryptionKey.ps1](https://github.com/anoobbacker/storsimpledevicemgmttools/blob/master/Update-CloudApplianceServiceEncryptionKey.ps1) PowerShell-script. 
2. Open PowerShell en typ het volgende achter de opdrachtprompt:  `Update-CloudApplianceServiceEncryptionKey.ps1 -SubscriptionId [subscription] -TenantId [tenantid] -ResourceGroupName [resource group] -ManagerName [device manager]`

Met dit script zorgt ervoor dat versleutelingssleutel voor servicegegevens is ingesteld op alle 8010/8020-cloudapparaten onder Apparaatbeheer.

## <a name="supported-operations-on-devices-running-versions-prior-to-update-50"></a>Ondersteunde bewerkingen op apparaten met versies vóór Update 5.0
In de Azure-portal, worden alleen de StorSimple-apparaten met Update 5.0 en hoger ondersteund. De apparaten met oudere versies beperkt worden ondersteund. Nadat u hebt gemigreerd naar Azure portal, gebruikt u de volgende tabel om te begrijpen welke bewerkingen worden ondersteund op apparaten met versies vóór Update 5.0.

| Bewerking                                                                                                                       | Ondersteund      |
|---------------------------------------------------------------------------------------------------------------------------------|----------------|
| Een apparaat registreren                                                                                                               | Ja            |
| Instellingen voor apparaten zoals Algemeen, netwerk en beveiliging configureren                                                                | Ja            |
| Scannen, downloaden en installeren van updates                                                                                             | Ja            |
| Apparaat deactiveren                                                                                                               | Ja            |
| Apparaat verwijderen                                                                                                                   | Ja            |
| Maken, wijzigen en verwijderen van een volumecontainer                                                                                   | Nee             |
| Maken, wijzigen en verwijderen van een volume                                                                                             | Nee             |
| Maken, wijzigen en verwijderen van een back-upbeleid                                                                                      | Nee             |
| Een handmatige back-up maken                                                                                                            | Nee             |
| Een geplande back-up maken                                                                                                         | Niet van toepassing |
| Herstellen vanaf een back-upset                                                                                                        | Nee             |
| Klonen op een apparaat met Update 3.0 en hoger <br> Het bronapparaat wordt versie voordat u Update 3.0 uitgevoerd.                                | Ja            |
| Bij een apparaat met versies vóór Update 3.0 klonen                                                                          | Nee             |
| Failover als Bronapparaat <br> (vanaf een apparaat met versie vóór de Update 3.0 bij een apparaat met Update 3.0 en hoger)                                                               | Ja            |
| Failover als doelapparaat <br> (op een apparaat met softwareversie vóór Update 3.0)                                                                                   | Nee             |
| Een waarschuwing wissen                                                                                                                  | Ja            |
| Back-upbeleid, back-catalogus, volumes, volumecontainers, bewakingsgrafieken, taken en waarschuwingen die zijn gemaakt in de klassieke portal weergeven | Ja            |
| In-en uitschakelen apparaatcontrollers                                                                                              | Ja            |


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de [StorSimple-implementatieproces](storsimple-8000-deployment-walkthrough-u2.md).
* Meer informatie over [beheren van uw StorSimple-opslagaccount](storsimple-8000-manage-storage-accounts.md).
* Meer informatie over het [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).
