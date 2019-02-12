---
title: Instellen van Pacemaker op Red Hat Enterprise Linux in Azure | Microsoft Docs
description: Pacemaker op Red Hat Enterprise Linux in Azure instellen
services: virtual-machines-windows,virtual-network,storage
documentationcenter: saponazure
author: mssedusch
manager: timlt
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/17/2018
ms.author: sedusch
ms.openlocfilehash: 3c4f1be97308062adc28a0b49ee36c3492906d63
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55990204"
---
# <a name="setting-up-pacemaker-on-red-hat-enterprise-linux-in-azure"></a>Pacemaker op Red Hat Enterprise Linux in Azure instellen

[planning-guide]:planning-guide.md
[deployment-guide]:deployment-guide.md
[dbms-guide]:dbms-guide.md
[sap-hana-ha]:sap-hana-high-availability.md
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2009879]:https://launchpad.support.sap.com/#/notes/2009879
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1999351]:https://launchpad.support.sap.com/#/notes/1999351

[virtual-machines-linux-maintenance]:../../linux/maintenance-and-updates.md#memory-preserving-maintenance

> [!NOTE]
> Pacemaker op Red Hat Enterprise Linux maakt gebruik van de Azure omheining-Agent op een clusterknooppunt omheining indien nodig. Een failover kan maximaal 15 minuten duren, als een resource stoppen is mislukt of als de clusterknooppunten kunnen niet worden gecommuniceerd die elkaar meer. Lees voor meer informatie, [virtuele Azure-machine die wordt uitgevoerd als lid van een hoge beschikbaarheid voor RHEL-cluster een zeer lang duren worden omheinde of de eerste optie mislukt / tijden-out voordat de virtuele machine wordt afgesloten](https://access.redhat.com/solutions/3408711)

Lees eerst de volgende SAP-opmerkingen en documenten:

* SAP-notitie [1928533], heeft:
  * De lijst met Azure VM-grootten die worden ondersteund voor de implementatie van SAP-software.
  * Informatie over belangrijke capaciteit voor Azure VM-grootten.
  * De ondersteunde SAP-software en besturingssysteem (OS) en combinaties van de database.
  * De vereiste SAP kernelversie voor Windows en Linux op Microsoft Azure.
* SAP-notitie [2015553] bevat vereisten voor SAP-ondersteunde SAP software-implementaties in Azure.
* SAP-notitie [2002167] heeft aanbevolen instellingen voor Red Hat Enterprise Linux OS
* SAP-notitie [2009879] bevat richtlijnen voor SAP HANA voor Red Hat Enterprise Linux
* SAP-notitie [2178632] vindt u meer informatie over alle bewaking metrische gegevens die zijn gerapporteerd voor SAP in Azure.
* SAP-notitie [2191498] heeft de vereiste versie van de SAP-Host-Agent voor Linux in Azure.
* SAP-notitie [2243692] bevat informatie over de licentieverlening voor SAP op Linux in Azure.
* SAP-notitie [1999351] bevat aanvullende informatie over probleemoplossing voor de Azure uitgebreide controle-extensie voor SAP.
* [SAP-Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) bevat alle SAP-opmerkingen voor Linux vereist.
* [Azure virtuele Machines, planning en implementatie van SAP op Linux][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP op Linux (in dit artikel)][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP op Linux][dbms-guide]
* [SAP HANA-systeemreplicatie in pacemaker cluster](https://access.redhat.com/articles/3004101)
* Algemene RHEL-documentatie
  * [Overzicht van de invoegtoepassing voor hoge beschikbaarheid](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index)
  * [Beheer van de invoegtoepassing voor hoge beschikbaarheid](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index)
  * [Naslaginformatie over hoge beschikbaarheid-invoegtoepassing](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index)
* Azure specifieke RHEL-documentatie:
  * [Ondersteuningsbeleid voor RHEL Clusters met hoge beschikbaarheid - Microsoft Azure Virtual Machines als leden van het Cluster](https://access.redhat.com/articles/3131341)
  * [Installeren en configureren van een Red Hat Enterprise Linux 7.4 (en hoger) Cluster met hoge beschikbaarheid voor Microsoft Azure](https://access.redhat.com/articles/3252491)

## <a name="cluster-installation"></a>Clusterinstallatie van

![Pacemaker op RHEL-overzicht](./media/high-availability-guide-rhel-pacemaker/pacemaker-rhel.png)

De volgende items worden voorafgegaan door een **[A]** : van toepassing op alle knooppunten **[1]** - alleen van toepassing op knooppunt 1 of **[2]** - alleen van toepassing op knooppunt 2.

1. **[A]** Register

   Registreren van uw virtuele machines en deze koppelen aan een pool met opslagplaatsen voor RHEL 7.

   <pre><code>sudo subscription-manager register
   # List the available pools
   sudo subscription-manager list --available --matches '*SAP*'
   sudo subscription-manager attach --pool=&lt;pool id&gt;
   </code></pre>

1. **[A]**  Inschakelen RHEL for SAP-opslagplaatsen

   Om te installeren de vereiste pakketten, schakel de volgende opslagplaatsen.

   <pre><code>sudo subscription-manager repos --disable "*"
   sudo subscription-manager repos --enable=rhel-7-server-rpms
   sudo subscription-manager repos --enable=rhel-ha-for-rhel-7-server-rpms
   sudo subscription-manager repos --enable="rhel-sap-for-rhel-7-server-rpms"
   </code></pre>

1. **[A]**  RHEL HA-invoegtoepassing installeren

   <pre><code>sudo yum install -y pcs pacemaker fence-agents-azure-arm nmap-ncat
   </code></pre>

1. **[A]**  Omzetten van de hostnaam instellen

   U kunt een DNS-server gebruiken of aanpassen van de/etc/hosts op alle knooppunten. In dit voorbeeld laat zien hoe u het bestand/etc/hosts gebruikt.
   Vervang het IP-adres en de hostnaam in de volgende opdrachten. Het voordeel van het gebruik van/etc/hosts is dat uw cluster onafhankelijk van DNS, wat erop kan een single point of fouten te worden.

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   Voeg de volgende regels/etc/hosts. De IP-adres en hostnaam zodat deze overeenkomen met uw omgeving wijzigen

   <pre><code># IP address of the first cluster node
   <b>10.0.0.6 prod-cl1-0</b>
   # IP address of the second cluster node
   <b>10.0.0.7 prod-cl1-1</b>
   </code></pre>

1. **[A]**  Hacluster wachtwoord wijzigen naar hetzelfde wachtwoord

   <pre><code>sudo passwd hacluster
   </code></pre>

1. **[A]**  Firewallregels voor pacemaker toevoegen

   De volgende firewallregels toevoegen aan alle clustercommunicatie tussen de clusterknooppunten.

   <pre><code>sudo firewall-cmd --add-service=high-availability --permanent
   sudo firewall-cmd --add-service=high-availability
   </code></pre>

1. **[A]**  Basiscluster services inschakelen

   Voer de volgende opdrachten voor het inschakelen van de service Pacemaker en start de App.

   <pre><code>sudo systemctl start pcsd.service
   sudo systemctl enable pcsd.service
   </code></pre>

1. **[1]**  Cluster Pacemaker maken

   Voer de volgende opdrachten om te verifiëren van de knooppunten en maken van het cluster. Stel het token op 30000 waarmee onderhoud met statusbehoud geheugen. Zie voor meer informatie, [in dit artikel voor Linux][virtual-machines-linux-maintenance].

   <pre><code>sudo pcs cluster auth <b>prod-cl1-0</b> <b>prod-cl1-1</b> -u hacluster
   sudo pcs cluster setup --name <b>nw1-azr</b> <b>prod-cl1-0</b> <b>prod-cl1-1</b> --token 30000
   sudo pcs cluster start --all
   
   # Run the following command until the status of both nodes is online
   sudo pcs status
   
   # Cluster name: nw1-azr
   # WARNING: no stonith devices and stonith-enabled is not false
   # Stack: corosync
   # Current DC: <b>prod-cl1-1</b> (version 1.1.18-11.el7_5.3-2b07d5c5a9) - partition with quorum
   # Last updated: Fri Aug 17 09:18:24 2018
   # Last change: Fri Aug 17 09:17:46 2018 by hacluster via crmd on <b>prod-cl1-1</b>
   #
   # 2 nodes configured
   # 0 resources configured
   #
   # Online: [ <b>prod-cl1-0</b> <b>prod-cl1-1</b> ]
   #
   # No resources
   #
   #
   # Daemon Status:
   #   corosync: active/disabled
   #   pacemaker: active/disabled
   #   pcsd: active/enabled
   </code></pre>

1. **[A]**  Verwachte stemmen instellen

   <pre><code>sudo pcs quorum expected-votes 2
   </code></pre>

## <a name="create-stonith-device"></a>Stonith instellen-apparaat maken

Het stonith instellen-apparaat maakt gebruik van een Service-Principal te autoriseren op basis van Microsoft Azure. Volg deze stappen voor het maken van een Service-Principal.

1. Ga naar <https://portal.azure.com>
1. Open de Azure Active Directory-blade  
   Ga naar eigenschappen en noteer de map-ID. Dit is de **tenant-ID**.
1. Klik op App-registraties
1. Klik op Add.
1. Voer een naam in, selecteert u het Type toepassing 'Web-app/API', voer een aanmeldings-URL (bijvoorbeeld http://localhost) en klik op maken
1. De aanmeldings-URL wordt niet gebruikt en kan geldige URL zijn
1. Selecteer de nieuwe App en sleutels op in het tabblad instellingen
1. Voer een beschrijving in voor een nieuwe sleutel, selecteer 'Verloopt nooit' en klik op Opslaan
1. Noteer de waarde in. Deze wordt gebruikt als de **wachtwoord** voor de Service-Principal
1. Noteer de toepassings-ID. Deze wordt gebruikt als de gebruikersnaam (**aanmeldings-ID** in de onderstaande stappen) van de Service-Principal

### <a name="1-create-a-custom-role-for-the-fence-agent"></a>**[1]**  Een aangepaste rol maken voor de agent omheining

De Service-Principal heeft geen machtigingen voor toegang tot uw Azure-resources standaard. U hoeft op te geven van de Service-Principal machtigingen voor starten en stoppen (toewijzing ongedaan maken) alle virtuele machines van het cluster. Als u de aangepaste rol die niet al hebt gemaakt, kunt u maken met behulp van [PowerShell](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell) of [Azure CLI](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-cli)

Gebruik de volgende inhoud voor het invoerbestand. U moet de inhoud voor uw abonnementen die is aangepast, c276fc76-9cd4-44c9-99a7-4fd71546436e en e91d47c4-76f3-4271-a796-21b4ecfe3624 vervangen door de id's van uw abonnement. Als u slechts één abonnement hebt, verwijdert u de tweede vermelding in AssignableScopes.

```json
{
  "Name": "Linux Fence Agent Role",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows to deallocate and start virtual machines",
  "Actions": [
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/deallocate/action",
    "Microsoft.Compute/virtualMachines/start/action"
  ],
  "NotActions": [
  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"
  ]
}
```

### <a name="a-assign-the-custom-role-to-the-service-principal"></a>**[A]**  De aangepaste rol toewijzen aan de Service-Principal

De aangepaste rol 'Linux omheining Agent rol' die is gemaakt in het vorige hoofdstuk aan de Service-Principal toewijzen. De rol van eigenaar niet meer gebruiken.

1. Ga naar https://portal.azure.com
1. Open de blade alle resources
1. Selecteer de virtuele machine van het eerste clusterknooppunt
1. Klik op de Access control (IAM)
1. Klik op de roltoewijzing toevoegen
1. Selecteer de rol 'Linux omheining Agent rol'
1. Voer de naam van de toepassing die u hierboven hebt gemaakt
1. Op Opslaan klikken

Herhaal de bovenstaande stappen voor het tweede clusterknooppunt.

### <a name="1-create-the-stonith-devices"></a>**[1]**  Maken de apparaten stonith instellen

Nadat u de machtigingen voor de virtuele machines hebt bewerkt, kunt u de apparaten stonith instellen in het cluster configureren.

<pre><code>
sudo pcs property set stonith-timeout=900
</code></pre>

Gebruik de volgende opdracht om de omheining-apparaat te configureren.

> [!NOTE]
> Optie 'pcmk_host_map' in de opdracht is alleen vereist als de namen van de RHEL-hosts en de namen van de Azure-knooppunt niet zijn identiek zijn. Raadpleeg de sectie vet weergegeven in de opdracht.

<pre><code>sudo pcs stonith create rsc_st_azure fence_azure_arm login="<b>login ID</b>" passwd="<b>password</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant ID</b>" subscriptionId="<b>subscription id</b>" <b>pcmk_host_map="prod-cl1-0:10.0.0.6;prod-cl1-1:10.0.0.7"</b> power_timeout=240 pcmk_reboot_timeout=900</code></pre>

### <a name="1-enable-the-use-of-a-stonith-device"></a>**[1]**  Het gebruik van een apparaat stonith instellen inschakelen

<pre><code>sudo pcs property set stonith-enabled=true
</code></pre>

## <a name="next-steps"></a>Volgende stappen

* [Azure virtuele Machines, planning en implementatie van SAP][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP][dbms-guide]
* Zie voor meer informatie over het opzetten van hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP HANA op Azure Virtual machines, [hoge beschikbaarheid van SAP HANA op Azure Virtual Machines (VM's)][sap-hana-ha]
