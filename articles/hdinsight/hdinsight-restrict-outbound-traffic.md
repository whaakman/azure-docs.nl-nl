---
title: Beperking van verkeer uitgaand netwerkverkeer voor Azure HDInsight-clusters configureren
description: Informatie over het configureren van de beperking van verkeer uitgaand netwerkverkeer voor Azure HDInsight-clusters.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: howto
ms.date: 05/30/2019
ms.openlocfilehash: 542813e0f82a1a52142a2b82bea3fdb101fdec28
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67077164"
---
# <a name="configure-outbound-network-traffic-for-azure-hdinsight-clusters-using-firewall-preview"></a>Configureren van uitgaand netwerkverkeer voor Azure HDInsight-clusters met behulp van de Firewall (Preview)

In dit artikel bevat de stappen waarmee u voor het beveiligen van het uitgaande verkeer van uw HDInsight-cluster met behulp van Azure-Firewall. De onderstaande stappen wordt ervan uitgegaan dat u een Azure-Firewall voor een bestaand cluster wilt configureren. Als u een nieuw cluster implementeert en achter een firewall bevindt, eerst maken van uw HDInsight-cluster en het subnet en volg de stappen in deze handleiding.

## <a name="background"></a>Achtergrond

Azure HDInsight-clusters worden normaal gesproken geïmplementeerd in uw eigen virtuele netwerk. Het cluster heeft afhankelijkheden op services buiten dit virtuele netwerk waarvoor toegang tot het netwerk te laten functioneren.

Er zijn verschillende afhankelijkheden waarmee binnenkomend verkeer vereist. Het inkomend managementverkeer kan niet worden verzonden via een firewall-apparaat. De bronadressen voor dit verkeer bekend zijn en worden gepubliceerd [hier](hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip). U kunt ook een Netwerkbeveiligingsgroep (NSG) regels maken met deze informatie voor het beveiligen van inkomend verkeer op de clusters.

De afhankelijkheden van HDInsight uitgaand verkeer zijn bijna volledig gedefinieerd met FQDN's, waarvoor geen statische IP-adressen achter deze. Het ontbreken van statische adressen betekent dat Netwerkbeveiligingsgroepen (nsg's) kan niet worden gebruikt voor het vergrendelen van het uitgaande verkeer van een cluster. De adressen wijzigen vaak genoeg dat kan niet een regels op basis van de huidige naamomzetting instellen en die gebruiken voor het instellen van NSG-regels.

De oplossing voor beveiliging van de uitgaande adressen is het gebruik van een firewall-apparaat dat uitgaand verkeer op basis van domeinnamen kunt beheren. Firewall van Azure kunt beperken uitgaande HTTP en HTTPS-verkeer op basis van de FQDN-naam van de bestemming of [FQDN tags](https://docs.microsoft.com/azure/firewall/fqdn-tags).

## <a name="configuring-azure-firewall-with-hdinsight"></a>Firewall voor Azure configureren met HDInsight

Een overzicht van de stappen voor het uitgaande verkeer van uw bestaande HDInsight met Azure Firewall vergrendelen zijn:
1. Een firewall maken.
1. Toepassing regels toevoegen aan de firewall
1. Netwerkregels toevoegen aan de firewall.
1. Maak een routeringstabel.

### <a name="create-a-new-firewall-for-your-cluster"></a>Een nieuwe firewall voor uw cluster maken

1. Maak een subnet met de naam **AzureFirewallSubnet** in het virtuele netwerk waarin het cluster bestaat. 
1. Maak een nieuwe firewall **Test FW01** met behulp van de stappen in [zelfstudie: Implementeren en configureren van de Firewall van Azure met behulp van de Azure-portal](../firewall/tutorial-firewall-deploy-portal.md#deploy-the-firewall).

### <a name="configure-the-firewall-with-application-rules"></a>De firewall configureren aan de regels van toepassing

Maak een regelverzameling toepassing waarmee het cluster te verzenden en ontvangen van belangrijke communicatie.

Selecteer de nieuwe firewall **Test FW01** vanuit Azure portal. Klik op **regels** onder **instellingen** > **toepassing regelverzameling** > **toepassing regelverzamelingtoevoegen**.

![Titel: Verzameling van toepassing-regel toevoegen](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection.png)

Op de **regelverzameling toepassing toevoegen** scherm, voer de volgende stappen uit:

1. Voer een **naam**, **prioriteit**, en klikt u op **toestaan** uit de **actie** in het vervolgkeuzemenu en voer de volgende regels in de **FQDN Tags sectie** :

   | **Naam** | **Bronadres** | **Label van de FQDN-naam** | **Opmerkingen** |
   | --- | --- | --- | --- |
   | Rule_1 | * | HDInsight en Windows Update | Vereist voor HDI-services |

1. Voeg de volgende regels in de **doel FQDN's sectie** :

   | **Naam** | **Bronadres** | **Protocol:Port** | **Doel FQDN-namen** | **Opmerkingen** |
   | --- | --- | --- | --- | --- |
   | Rule_2 | * | https:443 | login.windows.net | Hiermee kunt Windows-Aanmeldingsactiviteit |
   | Rule_3 | * | https:443,http:80 | <storage_account_name.blob.core.windows.net> | Als uw cluster wordt ondersteund door WASB, voegt u een regel voor WASB. Voor gebruik van alleen-https-verbindingen maken of ['veilige overdracht vereist'](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) is ingeschakeld op het storage-account. |

1. Klik op **Toevoegen**.

   ![Titel: Details van verzameling regel invoeren](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection-details.png)

### <a name="configure-the-firewall-with-network-rules"></a>De firewall aan de netwerkregels van het configureren

De netwerkregels voor het correct configureren van uw HDInsight-cluster maken.

1. Selecteer de nieuwe firewall **Test FW01** vanuit Azure portal.
1. Klik op **regels** onder **instellingen** > **netwerk regelverzameling** > **regelverzameling netwerk toevoegen**.
1. Op de **regelverzameling netwerk toevoegen** scherm, voert u een **naam**, **prioriteit**, en klikt u op **toestaan** uit de **actie** in het vervolgkeuzemenu.
1. Maken van de volgende regels in de **IP-adressen** sectie:

   | **Naam** | **Protocol** | **Bronadres** | **Doeladres** | **Doelpoort** | **Opmerkingen** |
   | --- | --- | --- | --- | --- | --- |
   | Rule_1 | UDP | * | * | `123` | Time-service |
   | Rule_2 | Alle | * | DC_IP_Address_1, DC_IP_Address_2 | `*` | Als u gebruikmaakt van Enterprise Security Package (ESP), voegt u een regel in de sectie IP-adressen waarmee communicatie met AAD-DS voor ESP-clusters. IP-adressen van de domeincontrollers in de AAD-DS-sectie vindt in de portal | 
   | Rule_3 | TCP | * | IP-adres van uw Data Lake Storage-account | `*` | Als u van Azure Data Lake Storage gebruikmaakt, kunt u een regel toevoegen in de sectie IP-adressen om een SNI-probleem met ADLS Gen1 en Gen2. Deze optie wordt het verkeer routeren naar de firewall, die tot hogere kosten voor veel gegevens geladen leiden kan, maar het verkeer dat is geregistreerd en controleerbaar in firewall-Logboeken. Bepaal het IP-adres voor uw Data Lake Storage-account. U kunt een powershell-opdracht gebruiken zoals `[System.Net.DNS]::GetHostAddresses("STORAGEACCOUNTNAME.blob.core.windows.net")` de FQDN-naam omzetten in een IP-adres.|
   | Rule_4 | TCP | * | * | `12000` | (Optioneel) Als u van Log Analytics gebruikmaakt, maakt u een regel in de sectie IP-adressen voor het inschakelen van de communicatie met uw Log Analytics-werkruimte. |

1. Maken van de volgende regels in de **servicetags** sectie:

   | **Naam** | **Protocol** | **Bronadres** | **Service Tags** | **Doelpoort** | **Opmerkingen** |
   | --- | --- | --- | --- | --- | --- |
   | Rule_7 | TCP | * | * | `1433,11000-11999,14000-14999` | Een regel in de sectie labels van de Service configureren voor SQL waarmee u kunt aanmelden en SQL-verkeer, controleren, tenzij u Service-eindpunten geconfigureerd voor SQL Server op het HDInsight-subnet dat de firewall wordt overslaan. |

1. Klik op **toevoegen** om uit te voeren van het maken van uw netwerk-regelverzameling.

   ![Titel: Details van verzameling regel invoeren](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-network-rule-collection.png)

### <a name="create-and-configure-a-route-table"></a>Maken en configureren van een routetabel

Maak een routetabel met de volgende items:

1. Zes adressen [deze lijst met vereiste IP-adressen van de HDInsight management](../hdinsight/hdinsight-extend-hadoop-virtual-network.md#hdinsight-ip) met de volgende hop **Internet**:
    1. Vier IP-adressen voor alle clusters in alle regio 's
    1. Twee IP-adressen die specifiek zijn voor de regio waarin het cluster is gemaakt
1. Een virtueel apparaat route voor IP-adres 0.0.0.0/0 met de volgende hop wordt de Firewall van Azure privé IP-adres.

Bijvoorbeeld, voor het configureren van de routetabel voor een cluster dat is gemaakt in de Verenigde Staten van 'VS-midden', gebruikt u stappen te volgen:

1. Meld u aan bij Azure Portal.
1. Selecteer uw Azure firewall **Test FW01**. Kopieer de **privé IP-adres** weergegeven op de **overzicht** pagina. Voor dit voorbeeld gebruiken we een **-adres van 10.1.1.4 voorbeeld**
1. Maak een nieuwe routetabel.
1. Klik op **Routes** onder **instellingen**.
1. Klik op **toevoegen** routes voor de IP-adressen maken in de onderstaande tabel.

| Routenaam | Adresvoorvoegsel | Volgend hoptype | Adres van de volgende hop |
|---|---|---|---|
| 168.61.49.99 | 168.61.49.99/32 | Internet | N.V.T. |
| 23.99.5.239 | 23.99.5.239/32 | Internet | N.V.T. |
| 168.61.48.131 | 168.61.48.131/32 | Internet | N.V.T. |
| 138.91.141.162 | 138.91.141.162/32 | Internet | N.V.T. |
| 13.67.223.215 | 13.67.223.215/32 | Internet | N.V.T. |
| 40.86.83.253 | 40.86.83.253/32 | Internet | N.V.T. |
| 0.0.0.0 | 0.0.0.0/0 | Virtueel apparaat | 10.1.1.4 |

Voltooi de configuratie van de tabel route:

1. Toewijzen van de routetabel die u hebt gemaakt met uw HDInsight-subnet door te klikken op **subnetten** onder **instellingen** en vervolgens **koppelen**.
1. Op de **subnet koppelen** scherm, selecteert u het virtuele netwerk dat het cluster is gemaakt in en de **HDInsight-Subnet** u gebruikt voor uw HDInsight-cluster.
1. Klik op **OK**.

## <a name="edge-node-or-custom-application-traffic"></a>Edge-knooppunt of aangepaste toepassingsverkeer

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

## <a name="access-to-the-cluster"></a>Toegang tot het cluster
Nadat de installatie van de firewall is er, kunt u het interne eindpunt (`https://<clustername>-int.azurehdinsight.net`) voor toegang tot de Ambari van binnen het VNET. Gebruik het openbare eindpunt (`https://<clustername>.azurehdinsight.net`) of ssh eindpunt (`<clustername>-ssh.azurehdinsight.net`), zorg ervoor dat u hebt de juiste routes in de routetabel en NSG-regels instellen om te voorkomen dat de assymetric routeringsprobleem uitgelegd [hier](https://docs.microsoft.com/azure/firewall/integrate-lb).

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
