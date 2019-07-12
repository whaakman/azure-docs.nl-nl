---
title: Red Hat Enterprise Linux-installatiekopieën in Azure | Microsoft Docs
description: Meer informatie over Red Hat Enterprise Linux-installatiekopieën in Microsoft Azure
services: virtual-machines-linux
documentationcenter: ''
author: BorisB2015
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 6/6/2019
ms.author: borisb
ms.openlocfilehash: f7ae82b0376489e21b35e4e94dce32805bea69c6
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67708388"
---
# <a name="red-hat-enterprise-linux-images-in-azure"></a>Red Hat Enterprise Linux-installatiekopieën in Azure
Dit artikel beschrijft de beschikbare installatiekopieën van Red Hat Enterprise Linux (RHEL) in de Azure Marketplace, samen met beleid voor hun namen en retentie.

Informatie over het beleid voor Red Hat-ondersteuning voor alle versies van RHEL vindt u op de [Red Hat Enterprise Linux-levenscyclus](https://access.redhat.com/support/policy/updates/errata) pagina.

>[!Important]
> RHEL-installatiekopieën die momenteel beschikbaar zijn in de Azure marketplace ondersteuning voor Bring-Your-Own-abonnement (BYOS) of betalen per gebruik (betalen per gebruik) modellen-licentieverlening. De [Azure Hybrid Use Benefit](../windows/hybrid-use-benefit-licensing.md) en dynamische schakelen tussen BYOS en betalen per gebruik wordt niet ondersteund. Switch licentiemodus vereist de virtuele machine uit de bijbehorende installatiekopie opnieuw te implementeren.

>[!Note]
> Vul een ondersteuningsticket met Microsoft voor problemen met betrekking tot de RHEL-installatiekopieën in de Azure marketplace-galerie.

## <a name="images-available-in-the-ui"></a>Installatiekopieën die beschikbaar zijn in de gebruikersinterface
Wanneer u een voor 'Red Hat"in de Marketplace, of zoekopdracht wanneer u een resource in de gebruikersinterface van Azure portal maakt, ziet u een subset van de RHEL-installatiekopieën beschikbaar en gerelateerde Red Hat-producten. U kunt altijd de volledige set beschikbare VM-installatiekopieën met de Azure CLI/PowerShell/API verkrijgen.

Als u wilt zien van de volledige set beschikbaar Red Hat-afbeeldingen in Azure, de volgende opdracht uitvoeren

```azurecli-interactive
az vm image list --publisher RedHat --all
```

## <a name="naming-convention"></a>Naamgevingsregels
VM-installatiekopieën in Azure zijn geordend op uitgever, aanbieding, SKU en versie. De combinatie van uitgever: aanbieding: SKU:Version is de afbeelding URN en unieke identificatie van de afbeelding die moet worden gebruikt.

Bijvoorbeeld, `RedHat:RHEL:7-RAW:7.6.2018103108` verwijst naar een RHEL 7.6 raw-gepartitioneerde installatiekopie gebruikt die op 31 oktober 2018 zijn gemaakt.

Hieronder ziet u een voorbeeld van hoe u een 7.6 RHEL VM maakt.
```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:7-RAW:7.6.2018103108 --no-wait
```

### <a name="the-latest-moniker"></a>De 'laatste' moniker
Azure REST API maakt gebruik van moniker 'nieuwste' voor de versie in plaats van de specifieke versie. Met behulp van 'nieuwste', wordt de meest recente installatiekopie inrichten voor de opgegeven uitgever, aanbieding en SKU.

Bijvoorbeeld, `RedHat:RHEL:7-RAW:latest` verwijst naar de meest recente RHEL 7 familie raw-gepartitioneerde installatiekopie beschikbaar.

```azurecli-interactive
az vm create --name RhelVM --resource-group TestRG --image RedHat:RHEL:7-RAW:latest --no-wait
```

>[!NOTE]
> In het algemeen de vergelijking van versies om te bepalen van de meest recente volgt de regels van de [CompareTo methode](https://msdn.microsoft.com/library/a5ts8tb6.aspx).

### <a name="current-naming-convention"></a>Huidige naamconventie
Alle momenteel gepubliceerde RHEL-installatiekopieën gebruiken het betalen per gebruik-model en zijn verbonden met [Red Hat Update Infrastructure (RHUI) in Azure](https://aka.ms/rhui-update). Een nieuwe naamconventie is vastgesteld voor RHEL 7 familie installatiekopieën waarin de schijf partitioneren scheme (onbewerkte gegevens, LVM) is opgegeven in de SKU in plaats van de versie. De versie van de RHEL-installatiekopie bevat een 7-RAW of 7 LVM. Op dit moment is niet de RHEL-6-familie naamgeving gewijzigd.

Er zijn 2 typen RHEL 7 installatiekopie SKU's in deze naamconventie: SKU's met de secundaire versie en SKU's die niet. Als u gebruiken een ONBEWERKTE 7 of 7 LVM-SKU wilt, kunt u de RHEL secundaire versie die u wilt implementeren in de versie opgeven. Als u ervoor de 'laatste' versie kiest, moet u de nieuwste secundaire versie van RHEL ingericht.

>[!NOTE]
> In de RHEL for SAP reeks afbeeldingen blijft de RHEL-versie vast. De naamconventie bevat daarom een bepaalde versie in de SKU.

>[!NOTE]
> RHEL 6 reeks afbeeldingen zijn niet verplaatst naar de nieuwe naamgeving.

## <a name="extended-update-support-eus"></a>Uitgebreide ondersteuning (EUS)
Aangezien van April 2019, RHEL-installatiekopieën beschikbaar zijn die gekoppeld aan de opslagplaatsen uitgebreid Update ondersteuning (EUS) standaard. Meer informatie over RHEL EUS zijn beschikbaar in [van Red Hat documentatie](https://access.redhat.com/articles/rhel-eus).

Vindt u instructies over het overschakelen van uw virtuele machine naar EUS en meer informatie over het einde van de levenscyclus datums EUS [hier](https://aka.ms/rhui-update#rhel-eus-and-version-locking-rhel-vms).

>[!NOTE]
> EUS wordt niet ondersteund op RHEL extra's. Dit betekent dat als u een pakket dat is gewoonlijk beschikbaar in het kanaal RHEL extra's installeert, kunt u niet zich om dit te doen terwijl op EUS. De levenscyclus van Red Hat extra's wordt toegelicht [hier](https://access.redhat.com/support/policy/updates/extras/).

### <a name="for-customers-that-want-to-use-eus-images"></a>Voor klanten die willen EUS installatiekopieën gebruiken:
Klanten die de installatiekopieën die zijn gekoppeld aan EUS opslagplaatsen gebruiken, moeten de RHEL-installatiekopie die een aantal van de secundaire versie van RHEL in de SKU bevat gebruiken. Deze installatiekopieën worden onbewerkte gepartitioneerd (dat wil zeggen niet LVM).

Bijvoorbeeld, ziet u mogelijk de volgende 2 7.4 RHEL-installatiekopieën die beschikbaar:
```bash
RedHat:RHEL:7-RAW:7.4.2018010506
RedHat:RHEL:7.4:7.4.2019041718
```
In dit geval `RedHat:RHEL:7.4:7.4.2019041718` wordt standaard gekoppeld aan EUS opslagplaatsen en `RedHat:RHEL:7-RAW:7.4.2018010506` wordt standaard gekoppeld aan niet-EUS opslagplaatsen.

### <a name="for-customers-that-dont-want-to-use-eus-images"></a>Voor klanten die niet wilt dat EUS installatiekopieën kunt gebruiken:
Als u niet dat een installatiekopie gebruikt die is verbonden met EUS standaard wilt, implementeren met behulp van een installatiekopie die een secundaire versienummer in de SKU niet bevat.

#### <a name="rhel-images-with-eus"></a>RHEL-installatiekopieën met EUS
De volgende tabel is van toepassing voor RHEL-installatiekopieën met een secundaire versie in de SKU.

>[!NOTE]
> Op het moment van schrijven hebben alleen RHEL 7.4 en later secundaire versies EUS ondersteunen. EUS wordt niet meer ondersteund voor RHEL < = 7.3.

Secundaire versie |Voorbeeld van de installatiekopie van EUS              |EUS status                                                   |
:-------------|:------------------------------|:------------------------------------------------------------|
RHEL 7.4      |RedHat:RHEL:7.4:7.4.2019041718 | Installatiekopieën van April 2019 gepubliceerd en later worden EUS standaard|
RHEL 7.5      |RedHat:RHEL:7.5:7.5.2019060305 | Installatiekopieën van juni 2019 gepubliceerd en later worden EUS standaard |
RHEL 7,6      |RedHat:RHEL:7.6:7.6.2019052206 | Installatiekopieën mei 2019 gepubliceerd en later worden EUS standaard  |
RHEL 8.0      |N/A                            | Er is geen EUS afbeeldingen momenteel die momenteel beschikbaar                 |


## <a name="list-of-rhel-images-available"></a>Lijst met RHEL-installatiekopieën beschikbaar
De volgende aanbiedingen zijn dat SKU 's zijn momenteel beschikbaar voor algemeen gebruik:

Aanbieding| SKU | Partitionering | Inrichten | Opmerkingen
:----|:----|:-------------|:-------------|:-----
RHEL          | 7-ONBEWERKTE    | RAW    | Linux-agent | RHEL 7-familie van afbeeldingen. <br> Niet gekoppeld aan EUS opslagplaatsen standaard.
|             | 7-LVM    | LVM    | Linux-agent | RHEL 7-familie van afbeeldingen. <br> Niet gekoppeld aan EUS opslagplaatsen standaard.
|             | 7-RAW-CI | RAW-CI | Cloud-init  | RHEL 7-familie van afbeeldingen. <br> Niet gekoppeld aan EUS opslagplaatsen standaard.
|             | 6.7      | RAW    | Linux-agent | RHEL 6.7 afbeeldingen, oude naamgevingsconventie
|             | 6.8      | RAW    | Linux-agent | Hetzelfde als hierboven voor RHEL 6,8
|             | 6.9      | RAW    | Linux-agent | Hetzelfde als hierboven voor RHEL 6,9
|             | 6.10     | RAW    | Linux-agent | Hetzelfde als hierboven voor RHEL 6.10
|             | 7.2      | RAW    | Linux-agent | Hetzelfde als hierboven voor RHEL 7.2
|             | 7.3      | RAW    | Linux-agent | Hetzelfde als hierboven voor RHEL 7.3
|             | 7.4      | RAW    | Linux-agent | Hetzelfde als hierboven voor RHEL 7.4. <br> Gekoppeld aan EUS opslagplaatsen standaard vanaf April 2019
|             | 7.5      | RAW    | Linux-agent | Hetzelfde als hierboven voor RHEL 7.5. <br> Gekoppeld aan EUS opslagplaatsen standaard vanaf juni 2019
|             | 7.6      | RAW    | Linux-agent | Hetzelfde als hierboven voor RHEL 7.6. <br> Gekoppeld aan EUS opslagplaatsen standaard vanaf mei 2019
RHEL-SAP      | 7.4      | LVM    | Linux-agent | RHEL 7.4 voor SAP HANA- en Business-Apps
|             | 7.5      | LVM    | Linux-agent | RHEL 7.5 voor SAP HANA- en Business-Apps
RHEL-SAP-HANA | 6.7      | RAW    | Linux-agent | RHEL 6.7 voor SAP HANA
|             | 7.2      | LVM    | Linux-agent | RHEL 7.2 voor SAP HANA
|             | 7.3      | LVM    | Linux-agent | 7\.3 RHEL for SAP HANA
RHEL-SAP-APPS | 6.8      | RAW    | Linux-agent | 6,8 RHEL for SAP Business Applications
|             | 7.3      | LVM    | Linux-agent | 7\.3 RHEL for SAP Business Applications

### <a name="old-naming-convention"></a>Oude naamgeving
Installatiekopieën van de RHEL 7-familie en de RHEL-6-familie van installatiekopieën gebruikt specifieke versies in hun SKU's omhoog totdat de naming convention wijziging hierboven is uitgelegd.

Numerieke SKU's vindt u in de lijst van de volledige installatiekopie. Microsoft en Red Hat maakt nieuwe numerieke SKU's wanneer een nieuwe secundaire versie afkomstig is uit.

### <a name="other-available-offers-and-skus"></a>Andere beschikbare aanbiedingen en SKU 's
De volledige lijst van beschikbare aanbiedingen en SKU's omvat mogelijk extra afbeeldingen dan wat wordt weergegeven in de bovenstaande tabel bijvoorbeeld `RedHat:rhel-ocp-marketplace:rhel74:7.4.1`. Deze aanbiedingen kunnen worden gebruikt voor het bieden van ondersteuning van specifieke marketplace-oplossingen, of ze kon worden gepubliceerd voor voorbeelden en testdoeleinden. Ze kunnen worden gewijzigd of verwijderd op elk gewenst moment zonder waarschuwing. Gebruik deze waarde niet, tenzij hun aanwezigheid heeft openbaar is beschreven door Microsoft of Red Hat.

## <a name="publishing-policy"></a>Beleid voor publiceren
Microsoft en Red Hat bijwerken afbeeldingen als nieuwe secundaire versies worden vrijgegeven, zoals vereist is om de specifieke CVEs, of voor incidentele configuratie updates of wijzigingen. We streven ernaar om het bijgewerkte afbeeldingen bieden zo snel mogelijk - binnen drie werkdagen een release of beschikbaarheid van een oplossing CVE te volgen.

We alleen bijwerken de huidige secundaire versie in de familie voor een bepaalde installatiekopie. Met het uitbrengen van een nieuwere versie van de secundaire stoppen wij de oudere secundaire versie bijwerken. Bijvoorbeeld, bij de release van RHEL 7.6 wilt 7.5 RHEL-installatiekopieën niet meer worden bijgewerkt.

>[!NOTE]
> Actieve Azure-VM's ingericht vanuit RHEL betalen per gebruik afbeeldingen zijn verbonden met de Azure-RHUI en updates en oplossingen ontvangen kunnen zodra ze zijn vrijgegeven door Red Hat en gerepliceerd naar de Azure-RHUI (doorgaans in minder dan 24 uur na de officiële release van Red Hat) . Deze virtuele machines hoeven niet de installatiekopie van een nieuwe gepubliceerde voor het ophalen van de updates en klanten hebben volledige controle over wanneer de update starten.

## <a name="image-retention-policy"></a>Beleid voor het bewaren van installatiekopie
Onze huidige beleid is dat alle eerder gepubliceerde installatiekopieën. We behouden ons het recht om te verwijderen van installatiekopieën die bekend zijn bij problemen van welke aard dan ook. Bijvoorbeeld, kunnen installatiekopieën met onjuiste configuraties vanwege de volgende platform of updates van onderdelen worden verwijderd. Installatiekopieën die kunnen worden verwijderd volgt het huidige Marketplace-beleid om te bieden die meldingen tot 30 dagen voor de installatiekopie verwijderen.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de Azure Red Hat Update Infrastructure [hier](https://aka.ms/rhui-update).
* Informatie over het beleid voor Red Hat-ondersteuning voor alle versies van RHEL vindt u op de [Red Hat Enterprise Linux-levenscyclus](https://access.redhat.com/support/policy/updates/errata) pagina.
