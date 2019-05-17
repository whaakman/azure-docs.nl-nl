---
title: Beperking van verkeer uitgaand netwerkverkeer voor Azure HDInsight-clusters configureren
description: Informatie over het configureren van de beperking van verkeer uitgaand netwerkverkeer voor Azure HDInsight-clusters.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: howto
ms.date: 05/13/2019
ms.openlocfilehash: f244a67abab5c7f8cd14277f87f055ac6d48b8d2
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65762437"
---
# <a name="configure-outbound-network-traffic-restriction-for-azure-hdinsight-clusters"></a>Beperking van verkeer uitgaand netwerkverkeer voor Azure HDInsight-clusters configureren

In dit artikel bevat de stappen waarmee u voor het beveiligen van het uitgaande verkeer van uw HDInsight-cluster met behulp van Azure-Firewall. De onderstaande stappen wordt ervan uitgegaan dat u een Azure-Firewall voor een bestaand cluster wilt configureren. Als u een nieuw cluster implementeert en achter een firewall bevindt, eerst maken van uw HDInsight-cluster en het subnet en volg de stappen in deze handleiding.

## <a name="background"></a>Achtergrond

Azure HDInsight-clusters worden normaal gesproken geïmplementeerd in uw eigen virtuele netwerk. Het cluster heeft afhankelijkheden op services buiten dit virtuele netwerk waarvoor toegang tot het netwerk te laten functioneren.

Er zijn verschillende afhankelijkheden waarmee binnenkomend verkeer vereist. Het inkomend managementverkeer kan niet worden verzonden via een firewall-apparaat. De bronadressen voor dit verkeer bekend zijn en worden gepubliceerd [hier](hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip). U kunt ook een Netwerkbeveiligingsgroep (NSG) regels maken met deze informatie voor het beveiligen van inkomend verkeer op de clusters.

De afhankelijkheden van HDInsight uitgaand verkeer zijn bijna volledig gedefinieerd met FQDN's, waarvoor geen statische IP-adressen achter deze. Het ontbreken van statische adressen betekent dat Netwerkbeveiligingsgroepen (nsg's) kan niet worden gebruikt voor het vergrendelen van het uitgaande verkeer van een cluster. De adressen wijzigen vaak genoeg dat kan niet een regels op basis van de huidige naamomzetting instellen en die gebruiken voor het instellen van NSG-regels.

De oplossing voor beveiliging van de uitgaande adressen is het gebruik van een firewall-apparaat dat uitgaand verkeer op basis van domeinnamen kunt beheren. Firewall van Azure kunt beperken uitgaande HTTP en HTTPS-verkeer op basis van de FQDN-naam van de bestemming.

## <a name="configuring-azure-firewall-with-hdinsight"></a>Firewall voor Azure configureren met HDInsight

Een overzicht van de stappen voor het uitgaande verkeer van uw bestaande HDInsight met Azure Firewall vergrendelen zijn:
1. Service-eindpunten inschakelen.
1. Een firewall maken.
1. Toepassing regels toevoegen aan de firewall
1. Netwerkregels toevoegen aan de firewall.
1. Maak een routeringstabel.

### <a name="enable-service-endpoints"></a>Service-eindpunten inschakelen

Als u wilt overslaan van de firewall (bijvoorbeeld om kosten te besparen op de overdracht van gegevens) en vervolgens u de service-eindpunten voor SQL en storage op uw HDInsight-subnet inschakelen kunt. Wanneer u ingeschakeld voor Azure SQL-service-eindpunten hebt, moeten een Azure SQL-afhankelijkheden met uw cluster met service-eindpunten worden geconfigureerd.

Als u wilt inschakelen op de juiste service-eindpunten, voert u de volgende stappen uit:

1. Meld u aan bij Azure portal en selecteer het virtuele netwerk dat in uw HDInsight-cluster is geïmplementeerd.
1. Selecteer **subnetten** onder **instellingen**.
1. Selecteer het subnet waarop het cluster is geïmplementeerd.
1. Klik op het scherm om de subnetinstellingen te bewerken, **Microsoft.SQL** en/of **Microsoft.Storage** uit de **Service-eindpunten**  >   **Services** vervolgkeuzelijst.
1. Als u van een cluster ESP gebruikmaakt, dan moet u de **Microsoft.AzureActiveDirectory** service-eindpunt.
1. Klik op **Opslaan**.

### <a name="create-a-new-firewall-for-your-cluster"></a>Een nieuwe firewall voor uw cluster maken

1. Maak een subnet met de naam **AzureFirewallSubnet** in het virtuele netwerk waarin het cluster bestaat. 
1. Maak een nieuwe firewall **Test FW01** met behulp van de stappen in [zelfstudie: Implementeren en configureren van de Firewall van Azure met behulp van de Azure-portal](../firewall/tutorial-firewall-deploy-portal.md#deploy-the-firewall).
1. Selecteer de nieuwe firewall vanuit Azure portal. Klik op **regels** onder **instellingen** > **toepassing regelverzameling** > **toepassing regelverzamelingtoevoegen**.

    ![Titel: Verzameling met toepassingsregels toevoegen](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection.png)

### <a name="configure-the-firewall-with-application-rules"></a>De firewall configureren aan de regels van toepassing

Maak een regelverzameling toepassing waarmee het cluster te verzenden en ontvangen van belangrijke communicatie.

Selecteer de nieuwe firewall **Test FW01** vanuit Azure portal. Klik op **regels** onder **instellingen** > **toepassing regelverzameling** > **toepassing regelverzamelingtoevoegen**.

Op de **regelverzameling toepassing toevoegen** scherm, voer de volgende stappen uit:

1. Voer een **naam**, **prioriteit**, en klikt u op **toestaan** uit de **actie** in het vervolgkeuzemenu.
1. De volgende regels toevoegen:
    1. Een regel voor het HDInsight- en Windows Update-verkeer toestaan:
        1. In de **FQDN tags** sectie, bieden een **naam**, en stel **adressen van bron** naar `*`.
        1. Selecteer **HDInsight** en de **WindowsUpdate** uit de **FQDN Tags** in het vervolgkeuzemenu.
    1. Een regel waarmee Windows-Aanmeldingsactiviteit:
        1. In de **doel FQDN's** sectie, bieden een **naam**, en stel **adressen van bron** naar `*`.
        1. Voer `https:443` onder **Protocol: poort** en `login.windows.net` onder **FQDN's als doel**.
    1. Een regel voor het SQM-telemetrie toestaan:
        1. In de **doel FQDN's** sectie, bieden een **naam**, en stel **adressen van bron** naar `*`.
        1. Voer `https:443` onder **Protocol: poort** en `sqm.telemetry.microsoft.com` onder **FQDN's als doel**.
    1. Als uw cluster wordt ondersteund door WASB en u de bovenstaande service-eindpunten niet gebruikt, voegt u een regel voor WASB:
        1. In de **doel FQDN's** sectie, bieden een **naam**, en stel **adressen van bron** naar `*`.
        1. Voer `wasb` onder **Protocol: poort** en `*` onder **FQDN's als doel**.
1. Klik op **Toevoegen**.

![Titel: Details van verzameling regel invoeren](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection-details.png)

### <a name="configure-the-firewall-with-network-rules"></a>De firewall aan de netwerkregels van het configureren

De netwerkregels voor het correct configureren van uw HDInsight-cluster maken.

> [!Important]
> U kunt kiezen tussen het gebruik van SQL service-tags in de firewall met behulp van netwerkregels, zoals beschreven in deze sectie of een SQL-service eindpunt een beschreven in [de sectie over service-eindpunten](#enable-service-endpoints). Als u ervoor kiest SQL om labels te gebruiken in netwerkregels voor, kunt u aanmelden en controleren van de SQL-verkeer. Met behulp van een service-eindpunt heeft SQL-verkeer overslaan van de firewall.

1. Selecteer de nieuwe firewall **Test FW01** vanuit Azure portal.
1. Klik op **regels** onder **instellingen** > **netwerk regelverzameling** > **regelverzameling netwerk toevoegen**.
1. Op de **regelverzameling netwerk toevoegen** scherm, voert u een **naam**, **prioriteit**, en klikt u op **toestaan** uit de **actie** in het vervolgkeuzemenu.
1. Maak de volgende regels:
    1. Een regel van het netwerk waarmee het cluster uit te voeren van de klok synchroniseren met het NTP.
        1. In de **regels** sectie, bieden een **naam** en selecteer **eventuele** uit de **Protocol** vervolgkeuzelijst.
        1. Stel **bronadressen** en **doeladressen** naar `*`.
        1. Stel **doelpoorten** naar 123.
    1. Als u gebruikmaakt van Enterprise Security Package (ESP), voegt u een regel waarmee de communicatie met AAD-DS voor ESP-clusters.
        1. De twee IP-adressen voor uw domeincontrollers bepalen.
        1. In de volgende rij in de **regels** sectie, bieden een **naam** en selecteer **eventuele** uit de **Protocol** vervolgkeuzelijst.
        1. Stel **adressen van bron** `*`.
        1. Voer alle van de IP-adressen voor uw domeincontrollers in **doeladressen** gescheiden door komma's.
        1. Stel **doelpoorten** naar `*`.
    1. Als u van Azure Data Lake Storage gebruikmaakt, kunt u een regel om een SNI-probleem met ADLS Gen1 en Gen2 op te lossen toevoegen. Deze optie wordt het verkeer routeren naar de firewall, die tot hogere kosten voor veel gegevens geladen leiden kan, maar het verkeer dat is geregistreerd en controleerbare.
        1. Bepaal het IP-adres voor uw Data Lake Storage-account. U kunt een powershell-opdracht gebruiken zoals `[System.Net.DNS]::GetHostAddresses("STORAGEACCOUNTNAME.blob.core.windows.net")` de FQDN-naam omzetten in een IP-adres.
        1. In de volgende rij in de **regels** sectie, bieden een **naam** en selecteer **eventuele** uit de **Protocol** vervolgkeuzelijst.
        1. Stel **adressen van bron** `*`.
        1. Voer het IP-adres voor uw opslagaccount in **doeladressen**.
        1. Stel **doelpoorten** naar `*`.
    1. Een regel voor het inschakelen van de communicatie met de Key Management Service voor Windows-activering.
        1. In de volgende rij in de **regels** sectie, bieden een **naam** en selecteer **eventuele** uit de **Protocol** vervolgkeuzelijst.
        1. Stel **adressen van bron** `*`.
        1. Stel **doeladressen** naar `*`.
        1. Stel **doelpoorten** naar `1688`.
    1. Als u van Log Analytics gebruikmaakt, maakt u een regel om te communiceren met uw Log Analytics-werkruimte.
        1. In de volgende rij in de **regels** sectie, bieden een **naam** en selecteer **eventuele** uit de **Protocol** vervolgkeuzelijst.
        1. Stel **adressen van bron** `*`.
        1. Stel **doeladressen** naar `*`.
        1. Stel **doelpoorten** naar `12000`.
    1. Een service-label configureren voor SQL waarmee u kunt aanmelden en controleren van de SQL-verkeer.
        1. In de volgende rij in de **regels** sectie, bieden een **naam** en selecteer **eventuele** uit de **Protocol** vervolgkeuzelijst.
        1. Stel **adressen van bron** `*`.
        1. Stel **doeladressen** naar `*`.
        1. Selecteer **Sql** uit de **servicetags** vervolgkeuzelijst.
        1. Stel **doelpoorten** naar `1433,11000-11999,14000-14999`.
1. Klik op **toevoegen** om uit te voeren van het maken van uw netwerk-regelverzameling.

![Titel: Details van verzameling regel invoeren](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-network-rule-collection.png)

### <a name="create-and-configure-a-route-table"></a>Maken en configureren van een routetabel

Maak een routetabel met de volgende items:

1. 7-adressen uit [deze lijst met vereiste IP-adressen van de HDInsight management](../hdinsight/hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip) met de volgende hop **Internet**:
    1. Vier IP-adressen voor alle clusters in alle regio 's
    1. Twee IP-adressen die specifiek zijn voor de regio waarin het cluster is gemaakt
    1. Een IP-adres voor Azure van recursieve naamomzetting
1. Een virtueel apparaat route voor IP-adres 0.0.0.0/0 met de volgende hop wordt de Firewall van Azure privé IP-adres.

Bijvoorbeeld, voor het configureren van de routetabel voor een cluster dat is gemaakt in de Verenigde Staten van 'VS-midden', gebruikt u stappen te volgen:

1. Meld u aan bij Azure Portal.
1. Selecteer uw Azure firewall **Test FW01**. Kopieer de **privé IP-adres** weergegeven op de **overzicht** pagina. Voor dit voorbeeld gebruiken we een **-adres van 10.1.1.4 voorbeeld**
1. Maak een nieuwe routetabel.
1. Klik op **Routes** onder **instellingen**.
1. Klik op **toevoegen** routes voor de IP-adressen maken in de onderstaande tabel.

| Routenaam | Adresvoorvoegsel | Volgend hoptype | Volgend hopadres |
|---|---|---|---|
| 168.61.49.99 | 168.61.49.99/32 | Internet | N.V.T. |
| 23.99.5.239 | 23.99.5.239/32 | Internet | N.V.T. |
| 168.61.48.131 | 168.61.48.131/32 | Internet | N.V.T. |
| 138.91.141.162 | 138.91.141.162/32 | Internet | N.V.T. |
| 13.67.223.215 | 13.67.223.215/32 | Internet | N.V.T. |
| 40.86.83.253 | 40.86.83.253/32 | Internet | N.V.T. |
| 168.63.129.16 | 168.63.129.16/32 | Internet | N.V.T. |
| 0.0.0.0 | 0.0.0.0/0 | Virtueel apparaat | 10.1.1.4 |

![Titel: Een routetabel maken](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-route-table.png)

Voltooi de configuratie van de tabel route:

1. Toewijzen van de routetabel die u hebt gemaakt met uw HDInsight-subnet door te klikken op **subnetten** onder **instellingen** en vervolgens **koppelen**.
1. Op de **subnet koppelen** scherm, selecteert u het virtuele netwerk dat het cluster is gemaakt in en de **AzureFirewallSubnet** dat u hebt gemaakt voor gebruik met uw firewall.
1. Klik op **OK**.

![Titel: Een routetabel maken](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-route-table-associate-subnet.png)

## <a name="edge-node-application-traffic"></a>Edge-knooppunt toepassingsverkeer

De bovenstaande stappen kunnen het cluster om te werken zonder problemen. U moet nog steeds configureren van afhankelijkheden om te voldoen aan uw aangepaste toepassingen die worden uitgevoerd op de edge-knooppunten, indien van toepassing.

Afhankelijkheden van toepassingen moeten worden geïdentificeerd en toegevoegd aan de Azure-Firewall of de routetabel.

Routes moeten worden gemaakt voor de toepassingsverkeer om te voorkomen dat problemen met asymmetrische routering.

Als uw toepassingen andere afhankelijkheden hebben, moeten ze worden toegevoegd aan uw Azure-Firewall. Sta HTTP/HTTPS-verkeer en het netwerk van regels voor alle andere regels toepassing maken.

## <a name="logging"></a>Logboekregistratie

Firewall van Azure kunt u Logboeken verzenden naar een paar andere opslagsystemen. Voor instructies over het configureren van logboekregistratie voor uw firewall, volg de stappen in [zelfstudie: Azure-Firewall-logboeken en metrische gegevens controleren](../firewall/tutorial-diagnostics.md).

Wanneer u de instellingen voor logboekregistratie, hebt als u logboekregistratie van gegevens naar Log Analytics, kunt u geblokkeerd verkeer met een query, zoals het volgende bekijken:

```
AzureDiagnostics | where msg_s contains "Deny" | where TimeGenerated >= ago(1h)
```

Uw Azure-Firewall integreren met Azure Monitor-Logboeken is handig als eerst aan een toepassing werkt wanneer u zich niet bewust bent van alle van de afhankelijkheden van toepassing. U kunt meer informatie over Azure Monitor-logboeken van [analyseren logboekgegevens in Azure Monitor](../azure-monitor/log-query/log-query-overview.md)

## <a name="configure-another-network-virtual-appliance"></a>Configureren van een ander virtueel netwerkapparaat

>[!Important]
> De volgende informatie is **alleen** vereist als u wilt configureren van een virtueel netwerkapparaat (NVA) dan de Firewall van Azure.

De vorige instructies kunnen u Azure-Firewall configureren voor het beperken van het uitgaande verkeer van uw HDInsight-cluster. Firewall van Azure wordt automatisch geconfigureerd om verkeer te staan voor veel van de algemene belangrijke scenario's. Als u een ander virtueel netwerkapparaat gebruiken wilt, moet u het handmatig configureren van een aantal extra functies. De volgende Houd er rekening mee als uw uw virtueel netwerkapparaat configureren:

* Service-eindpunt kan services moeten worden geconfigureerd met service-eindpunten.
* IP-adres afhankelijkheden zijn voor niet-HTTP/S-verkeer (zowel TCP en UDP-verkeer).
* FQDN-naam HTTP/HTTPS-eindpunten kunnen worden geplaatst op uw apparaat van de NVA.
* Jokertekens HTTP/HTTPS-eindpunten zijn afhankelijkheden die kunnen verschillen op basis van een aantal kwalificaties.
* De routetabel die u met uw HDInsight-subnet maakt toewijzen.

### <a name="service-endpoint-capable-dependencies"></a>Service-eindpunt kan afhankelijkheden

| **Endpoint** |
|---|
| Azure SQL |
| Azure Storage |
| Azure Active Directory |

#### <a name="ip-address-dependencies"></a>Afhankelijkheden van de IP-adres

| **Endpoint** | **Details** |
|---|---|
| \*:123 | Controle van de klok NTP. Verkeer wordt gecontroleerd op meerdere eindpunten op poort 123 |
| IP-adressen gepubliceerd [hier](hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip) | Dit zijn HDInsight-service |
| AAD-DS privé-IP's voor ESP-clusters |
| \*: 16800 voor KMS-activering voor Windows |
| \*12000 voor Log Analytics |

#### <a name="fqdn-httphttps-dependencies"></a>FQDN-naam HTTP/HTTPS-afhankelijkheden

>[!Important]
> De onderstaande lijst verleent slechts enkele van de belangrijkste FQDN's. U kunt de volledige lijst met FQDN's ophalen voor het configureren van de NVA [in dit bestand](https://github.com/Azure-Samples/hdinsight-fqdn-lists/blob/master/HDInsightFQDNTags.json).

| **Endpoint**                                                          |
|---|
| azure.archive.ubuntu.com:80                                           |
| security.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |
| wawsinfraprodbay063.blob.core.windows.net:443                         |
| registry-1.docker.io:443                                              |
| auth.docker.IO:443                                                    |
| production.cloudflare.docker.com:443                                  |
| download.docker.com:443                                               |
| us.archive.ubuntu.com:80                                              |
| download.mono-project.com:80                                          |
| packages.treasuredata.com:80                                          |
| security.ubuntu.com:80                                                |
| azure.archive.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                |
| ocsp.digicert.com:80                                                |

## <a name="next-steps"></a>Volgende stappen

* [Azure HDInsight-architectuur voor virtueel netwerk](hdinsight-virtual-network-architecture.md)
