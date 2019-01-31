---
title: Oplossen van System Center Data Protection Manager met Azure Backup
description: Oplossen van problemen in System Center Data Protection Manager.
services: backup
author: kasinh
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: kasinh
ms.openlocfilehash: 4108616e3ae41e2c88b74bb08d5f846c0035101f
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55293992"
---
# <a name="troubleshoot-system-center-data-protection-manager"></a>Problemen oplossen met System Center Data Protection Manager

Dit artikel wordt beschreven oplossingen voor problemen die u tegenkomen kunt tijdens het gebruik van Data Protection Manager.

Zie voor de meest recente releaseopmerkingen voor System Center Data Protection Manager de [documentatie voor System Center](https://docs.microsoft.com/system-center/dpm/dpm-release-notes?view=sc-dpm-2016). U kunt meer informatie over ondersteuning voor Data Protection Manager in [deze matrix](https://docs.microsoft.com/system-center/dpm/dpm-protection-matrix?view=sc-dpm-2016).


## <a name="error-replica-is-inconsistent"></a>Fout: Replica is inconsistent

Een replica is inconsistent voor de volgende redenen:
- De taak voor het maken van replica is mislukt.
- Er zijn problemen met het wijzigingslogboek.
- Het filter op paginaniveau volumebitmap bevat fouten.
- De bron-VM onverwacht wordt afgesloten.
- Het synchronisatielogboek overschrijdt.
- De replica is echt niet consistent.

U lost dit probleem, moet u de volgende acties uitvoeren:
- Als u wilt verwijderen van de inconsistent status, de consistentiecontrole handmatig uitvoeren of een dagelijkse consistentiecontrole plannen.
- Zorg ervoor dat u de nieuwste versie van Microsoft Azure Backup Server en Data Protection Manager.
- Zorg ervoor dat de **automatische consistentiecontrole** instelling is ingeschakeld.
- Probeer het opnieuw opstarten van de services vanaf de opdrachtprompt. Gebruik de `net stop dpmra` opdracht, gevolgd door `net start dpmra`.
- Zorg ervoor dat u aan de netwerkvereisten voor connectiviteit en bandbreedte voldoet.
- Controleer als de bron-VM onverwacht werd afgesloten.
- Zorg ervoor dat de schijf in orde is en of er voldoende ruimte voor de replica.
- Zorg ervoor dat er geen dubbele back-uptaken die gelijktijdig worden uitgevoerd.

## <a name="error-online-recovery-point-creation-failed"></a>Fout: Een of meer online herstelpunten zijn niet gemaakt

U lost dit probleem, moet u de volgende acties uitvoeren:
- Zorg ervoor dat u de nieuwste versie van de Azure backup-agent.
- Probeer handmatig een herstelpunt maken in het taakgebied beveiliging.
- Zorg ervoor dat u een consistentiecontrole uit op de gegevensbron worden uitgevoerd.
- Zorg ervoor dat u aan de netwerkvereisten voor connectiviteit en bandbreedte voldoet.
- Als de replicagegevens in een inconsistente status, maak een schijfherstelpunt van deze gegevensbron.
- Zorg ervoor dat de replica aanwezig zijn en niet ontbreekt.
- Zorg ervoor dat de replica voldoende ruimte voor het maken van het logboek update sequence USN (number).

## <a name="error-unable-to-configure-protection"></a>Fout: Beveiliging kan niet worden geconfigureerd

Deze fout treedt op wanneer de Data Protection Manager-server kan geen contact op met de beveiligde server. 

U lost dit probleem, moet u de volgende acties uitvoeren:
- Zorg ervoor dat u de nieuwste versie van de Azure backup-agent.
- Zorg ervoor dat er verbinding (netwerk/firewallproxy) tussen de Data Protection Manager-server en de beveiligde server.
- Als u een SQL-server beveiligt, zorg ervoor dat de **Aanmeldingseigenschappen** > **NT AUTHORITY\SYSTEM** eigenschap bevat de **sysadmin** instelling is ingeschakeld.

## <a name="error-server-not-registered-as-specified-in-vault-credential-file"></a>Fout: Server niet is geregistreerd zoals opgegeven in het kluisreferentiebestand

Deze fout treedt op tijdens het herstelproces voor Data Protection Manager/Azure Backup server-gegevens. Het kluisreferentiebestand dat wordt gebruikt bij het herstelproces behoort niet tot de Recovery Services-kluis voor de Data Protection Manager/Azure Backup-server.

U lost dit probleem, moet u deze stappen uitvoeren:
1. Download het kluisreferentiebestand van de Recovery Services-kluis waarbij de Data Protection Manager/Azure Backup-server is geregistreerd.
2. Probeer de server met de kluis registreren met behulp van het meest recent gedownloade kluisreferentiebestand.

## <a name="error-no-recoverable-data-or-selected-server-not-a-data-protection-manager-server"></a>Fout: Er is geen herstelbare gegevens of de geselecteerde server niet in een Data Protection Manager-server

Deze fout treedt op voor de volgende redenen:
- Er zijn geen andere back-up van Data Protection Manager/Azure-servers zijn geregistreerd bij de Recovery Services-kluis.
- De metagegevens van de nog niet hebt nog geüpload door de servers.
- De geselecteerde server is niet een back-up van Data Protection Manager/Azure-server.

Wanneer andere Data Protection Manager/Azure back-up-servers zijn geregistreerd bij de Recovery Services-kluis, moet u deze stappen om het probleem te verhelpen uitvoeren:
1. Zorg ervoor dat de meest recente Azure Backup-agent is geïnstalleerd.
2. Nadat u ervoor zorgen dat de meest recente agent is geïnstalleerd, wacht u een dag voordat u het herstelproces te starten. De back-up elke nacht een taak wordt de metagegevens voor alle van de beveiligde back-ups geüpload naar de cloud. De back-upgegevens is vervolgens beschikbaar voor herstel.

## <a name="error-provided-encryption-passphrase-doesnt-match-passphrase-for-server"></a>Fout: De opgegeven wachtwoordzin voor versleuteling komt niet overeen met de wachtwoordzin voor server

Deze fout treedt op tijdens het versleutelingsproces tijdens het herstellen van Data Protection Manager/Azure Backup server-gegevens. De wachtwoordzin voor versleuteling die wordt gebruikt in het herstelproces komt niet overeen met de wachtwoordzin voor versleuteling van de server. Als gevolg hiervan de agent kan de gegevens niet ontsleutelen en het herstel is mislukt.

> [!IMPORTANT]
> Als u vergeet of verliest de wachtwoordzin voor versleuteling, zijn er geen andere methoden om de gegevens te herstellen. De enige optie is om opnieuw te genereren van de wachtwoordzin. Gebruik de nieuwe wachtwoordzin voor het versleutelen van toekomstige back-upgegevens.
>
> Wanneer u gegevens herstelt, moet u altijd de dezelfde wachtwoordzin voor versleuteling die is gekoppeld aan de Data Protection Manager/Azure Backup-server opgeven. 
>
