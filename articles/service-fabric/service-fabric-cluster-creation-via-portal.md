---
title: Service Fabric-cluster maken in de Azure portal | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een beveiligde Service Fabric-cluster in Azure met behulp van de Azure portal en Azure Key Vault instelt.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: vturecek
ms.assetid: 426c3d13-127a-49eb-a54c-6bde7c87a83b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/09/2018
ms.author: aljo
ms.openlocfilehash: d3efc14886d82290241e9868ea9b0e8262efddea
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="create-a-service-fabric-cluster-in-azure-using-the-azure-portal"></a>Maken van een Service Fabric-cluster in Azure met Azure portal
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure Portal](service-fabric-cluster-creation-via-portal.md)
> 
> 

Dit is een stapsgewijze handleiding die u bij de stappen helpt voor het instellen van een Service Fabric-cluster (Linux of Windows) in Azure met behulp van de Azure-portal. Deze handleiding doorloopt u de volgende stappen uit:

* Een cluster in Azure maken via de Azure portal.
* Beheerders met behulp van certificaten verifiëren.

> [!NOTE]
> Voor meer geavanceerde beveiligingsopties, zoals gebruikersverificatie met Azure Active Directory en het instellen van certificaten voor Toepassingsbeveiliging, [maken van het cluster met Azure Resource Manager][create-cluster-arm].
> 
> 

## <a name="cluster-security"></a>Clusterbeveiliging 
Er worden in Service Fabric certificaten gebruikt voor verificatie en versleuteling om verschillende aspecten van een cluster en de bijbehorende toepassingen te beveiligen. Zie voor meer informatie over hoe de certificaten worden gebruikt in Service Fabric, [scenario's voor beveiliging van Service Fabric-cluster][service-fabric-cluster-security].

Als dit de eerste keer dat u een service fabric-cluster maakt of een cluster voor testwerkbelastingen implementeert, kunt u doorgaan met de volgende sectie (**cluster maken in de Azure Portal**) en het genereren van certificaten die nodig zijn voor systeem clusters die testwerkbelastingen uitvoeren. Als u een cluster voor productieworkloads instelt, gaat u verder lezen.

#### <a name="cluster-and-server-certificate-required"></a>Cluster en de server-certificaat (vereist)
Dit certificaat is vereist voor het beveiligen van een cluster en onbevoegde toegang tot deze voorkomen. Biedt clusterbeveiliging in een aantal manieren:

* **Cluster-verificatie:** knooppunt naar communicatie voor cluster federation verifieert. Alleen knooppunten die u hun identiteit met dit certificaat bewijzen kunnen kunnen deelnemen aan het cluster.
* **Server-verificatie:** verifieert de eindpunten van het cluster-beheer met een management-client, zodat de management-client, weet deze communiceert met het echte cluster. Dit certificaat biedt ook SSL voor de HTTPS-API en voor Service Fabric Explorer via HTTPS.

Het certificaat moet voldoen aan de volgende vereisten voor deze doeleinden:

* Het certificaat moet een persoonlijke sleutel bevatten.
* Het certificaat moet worden gemaakt voor sleuteluitwisseling, geëxporteerd naar een bestand Personal Information Exchange (.pfx).
* Het certificaat **onderwerpnaam moet overeenkomen met het domein** gebruikt voor toegang tot de Service Fabric-cluster. Dit is vereist voor SSL voor het HTTPS-eindpunten voor beheer en de Service Fabric Explorer van het cluster. U kunt een SSL-certificaat van een certificeringsinstantie (CA) kan niet ophalen voor de `.cloudapp.azure.com` domein. Verkrijgen van een aangepaste domeinnaam voor uw cluster. Wanneer u een certificaat bij een Certificeringsinstantie aanvraagt moet de onderwerpnaam van het certificaat overeenkomen met de aangepaste domeinnaam voor uw cluster gebruikt.

#### <a name="client-authentication-certificates"></a>Certificaten voor clientverificatie
Aanvullende clientcertificaten verifiëren van beheerders voor beheertaken cluster. Service Fabric heeft twee toegangsniveaus: **admin** en **alleen-lezengebruiker**. Ten minste moet één certificaat voor beheerderstoegang worden gebruikt. Voor extra beveiliging op gebruikersniveau toegang, moet u een afzonderlijk certificaat opgegeven. Zie voor meer informatie over toegang tot rollen [toegangsbeheer op basis van rollen voor Service Fabric-clients][service-fabric-cluster-security-roles].

U hoeft niet voor het uploaden van certificaten voor clientverificatie voor Sleutelkluis voor gebruik met Service Fabric. Deze certificaten hoeft alleen te worden opgegeven voor gebruikers die gemachtigd zijn voor Clusterbeheer. 

> [!NOTE]
> Azure Active Directory is de aanbevolen manier om clients voor clusterbeheerbewerkingen te verifiëren. Voor het gebruik van Azure Active Directory, moet u [maken van een cluster met Azure Resource Manager][create-cluster-arm].
> 
> 

#### <a name="application-certificates-optional"></a>Toepassingscertificaten (optioneel)
Een willekeurig aantal extra certificaten kan worden geïnstalleerd op een cluster om beveiligingsredenen toepassing. Voordat u uw cluster maakt, houd rekening met de scenario's voor Toepassingsbeveiliging waarvoor een certificaat worden geïnstalleerd op de knooppunten, zoals:

* Versleuteling en ontsleuteling van configuratiewaarden die van toepassing
* Versleuteling van gegevens over knooppunten tijdens de replicatie 

Toepassingscertificaten kunnen niet worden geconfigureerd wanneer u een cluster via de Azure portal. Voor het configureren van toepassingscertificaten tijdens de installatie cluster, moet u [maken van een cluster met Azure Resource Manager][create-cluster-arm]. U kunt ook toepassingscertificaten toevoegen aan het cluster nadat deze is gemaakt.

< /a "maken-cluster-portal" ></a>

## <a name="create-cluster-in-the-azure-portal"></a>Cluster maken in de Azure portal

Maken van een productiecluster om te voldoen aan de behoeften van uw toepassing een planning om u te helpen bij die omvat, is het raadzaam dat u lees- en begrijpen [Service Fabric-Cluster planningsoverwegingen] [ service-fabric-cluster-capacity] document. 

### <a name="search-for-the-service-fabric-cluster-resource"></a>Zoeken naar de Service Fabric-cluster-bron
![zoeken naar Service Fabric-cluster sjabloon op de Azure-portal.][SearchforServiceFabricClusterTemplate]

1. Meld u aan bij [Azure Portal][azure-portal].
2. Klik op **maken van een resource** toevoegen van een nieuwe resource-sjabloon. Zoeken naar de sjabloon Service Fabric-Cluster in de **Marketplace** onder **Alles**.
3. Selecteer **Service Fabric-Cluster** uit de lijst.
4. Navigeer naar de **Service Fabric-Cluster** blade, klikt u op **maken**,
5. De **maken Service Fabric-cluster** blade bevat de volgende vier stappen:

#### <a name="1-basics"></a>1. Basisbeginselen
![Schermopname van het maken van een nieuwe resourcegroep.][CreateRG]

In de blade grondbeginselen moet u de algemene informatie opgeven voor uw cluster.

1. Voer de naam van het cluster.
2. Voer een **gebruikersnaam** en **wachtwoord** voor extern bureaublad voor de virtuele machines.
3. Zorg ervoor dat u selecteert de **abonnement** dat het cluster worden geïmplementeerd op, met name als u meerdere abonnementen hebt.
4. Maak een **nieuwe resourcegroep**. Het is raadzaam zodat deze dezelfde naam als het cluster, omdat het helpt bij het vinden ze later, vooral wanneer u probeert te wijzigingen aanbrengen in uw implementatie of verwijder uw cluster.
   
   > [!NOTE]
   > Hoewel u een bestaande resourcegroep gebruiken kunt, is het raadzaam om een nieuwe resourcegroep te maken. Hierdoor kunt gemakkelijk verwijderen clusters en alle resources die wordt gebruikt.
   > 
   > 
5. Selecteer de **regio** in waarop u wilt maken van het cluster. Als u van plan bent een bestaand certificaat dat u al hebt geüpload naar een sleutelkluis te gebruiken, moet u dezelfde regio als uw sleutelkluis is in. 

#### <a name="2-cluster-configuration"></a>2. Clusterconfiguratie
![Een knooppunttype maken][CreateNodeType]

Configureer de clusterknooppunten. Knooppunttypen definiëren de VM-grootten, het aantal virtuele machines en hun eigenschappen. Het cluster kan meer dan één knooppunttype hebben moet, maar het primaire knooppunttype (de eerste optie die u in de portal definieert) ten minste vijf virtuele machines, omdat dit het knooppunttype waar de Service Fabric-systeemservices worden geplaatst. Configureer geen **plaatsingseigenschappen** omdat een standaardeigenschap van de plaatsing van 'NodeTypeName' wordt automatisch toegevoegd.

> [!NOTE]
> Een veelvoorkomend scenario voor typen met meerdere knooppunten is een toepassing met een front-end-service en een back-endservice. U plaatst u de front-end-service op kleinere virtuele machines (VM-grootten zoals D2_V2) met poorten openen met het Internet, en de back-endservice op grotere virtuele machines (met VM-grootten zoals D3_V2, D6_V2 en D15_V2) zonder internetverbinding poorten geopend.
> 
> 

1. Kies een naam voor uw knooppunttype (1-12 tekens die alleen letters en cijfers bevatten).
2. De minimale **grootte** van virtuele machines voor het primaire knooppunt type wordt aangedreven door de **duurzaamheid** laag die u voor het cluster kiest. De standaardwaarde voor de laag duurzaamheid is Brons. Zie voor meer informatie over duurzaamheid [het kiezen van de Service Fabric-cluster duurzaamheid][service-fabric-cluster-durability].
3. Selecteer de VM-grootte. D-reeks VMs SSD-stations hebben en worden sterk aanbevolen voor stateful toepassingen. Geen gebruik van een VM SKU gedeeltelijke kernen heeft of minder dan 10 GB aan beschikbare schijfcapaciteit. Raadpleeg [service fabric-cluster overweging document planning] [ service-fabric-cluster-capacity] voor hulp bij het selecteren van de VM-grootte.
4. Het aantal virtuele machines voor het knooppunttype kiezen. U kunt omhoog of omlaag het aantal virtuele machines in een knooppunttype later op schalen, maar op het primaire knooppunttype de minimumwaarde is 5 voor productieworkloads. Andere knooppunttypen kunnen minimaal één virtuele machine hebben. De minimale **getal** van virtuele machines voor het primaire knooppunt type stations de **betrouwbaarheid** van uw cluster.  
5. **Enkele knooppunten en clusters met drie knooppunten** -deze zijn bedoeld voor gebruik door alleen test. Ze worden niet ondersteund voor alle actieve productieworkloads.
6. Aangepaste eindpunten configureren. Dit veld kunt u een door komma's gescheiden lijst met poorten die u wilt weergeven via de Azure Load Balancer met het openbare Internet voor uw toepassingen invoeren. Bijvoorbeeld als u een webtoepassing met uw cluster implementeert wilt, voert u '80' hier voor verkeer op poort 80 in uw cluster. Zie voor meer informatie over eindpunten [communiceren met toepassingen][service-fabric-connect-and-communicate-with-services]
7. Configureren van cluster **diagnostics**. Diagnostische gegevens zijn standaard ingeschakeld op het cluster om te helpen bij het oplossen van problemen. Als u wilt uitschakelen, wijziging van diagnostische gegevens van de **Status** in-of uitschakelen op **uit**. Het uitschakelen van diagnostische gegevens is **niet** aanbevolen. Als u al Application Insights-project hebt gemaakt hebt en geeft u vervolgens de sleutel, zodat de toepassingstraceringen ernaar worden doorgestuurd.
8. Selecteer de Fabric upgrade modus die u instellen van uw cluster wilt op. Selecteer **automatische**, als u wilt dat het systeem automatisch kunnen worden opgepikt de meest recente versie en probeer het bijwerken van uw cluster naar het. De modus instellen op **handmatige**, als u wilt kiezen van een ondersteunde versie. Voor meer informatie over de Fabric-modus Zie upgrade de [service fabric-cluster upgrade-document.][service-fabric-cluster-upgrade]

> [!NOTE]
> Wij ondersteunen alleen clusters die met een ondersteunde versie van service Fabric. U selecteert de **handmatige** modus u onderneemt op de verantwoordelijkheid voor het upgraden van uw cluster naar een ondersteunde versie. > 
> 

#### <a name="3-security"></a>3. Beveiliging
![Schermopname van beveiligingsconfiguraties op Azure-portal.][BasicSecurityConfigs]

Instellen van een beveiligde testcluster om gemakkelijk te maken voor u, boden we de **Basic** optie. Als u al een certificaat hebben en deze geüpload naar uw keyvault (en de sleutelkluis voor implementatie is ingeschakeld), gebruikt u de **aangepaste** optie

##### <a name="basic-option"></a>Basic-optie
Volg de schermen als u wilt toevoegen of een bestaande keyvault hergebruiken en een certificaat toevoegen. Het toevoegen van het certificaat is een synchrone proces en u moet dus wacht tot het certificaat moet worden gemaakt.


Bestand tegen de komen om navigeren weg van het scherm totdat de vorige proces is voltooid.

![CreateKeyVault]

Nu dat het certificaat wordt toegevoegd aan uw keyvault, ziet u mogelijk het volgende scherm weergegeven waarin u het toegangsbeleid bewerken voor uw Keyvault wordt gevraagd. Klik op de **bewerken toegangsbeleid voor.** .

![CreateKeyVault2]

Klik op het geavanceerde toegangsbeleid en toegang tot de virtuele Machines voor implementatie. Het verdient aanbeveling om de sjabloonimplementatie in te schakelen. Nadat u uw selecties hebt gemaakt, vergeet niet te klikken op de **opslaan** knop en sluiten van de **toegangsbeleid** deelvenster.

![CreateKeyVault3]

U bent nu klaar om door te gaan met de rest van het proces van de cluster maken.

![CreateKeyVault4]

##### <a name="custom-option"></a>Aangepaste optie
Als u al hebt uitgevoerd de stappen in deze sectie overslaan de **Basic** optie.

![SecurityCustomOption]

U moet de CertificateThumbprint, SourceVault en de CertificateURL-informatie voor het voltooien van de beveiligingspagina. Als u deze kunt raadplegen geen hebt, opent u een ander browservenster en Ga als volgt


1. Navigeer naar uw keyvault, selecteert u het certificaat. 
2. Selecteer het tabblad 'Eigenschappen' en 'RESOURCE ID' naar 'Bron sleutelkluis' in het browservenster kopiëren 

    ![CertInfo0]

3. Nu, selecteer het tabblad 'Certificaten'.
4. Klik op de certificaatvingerafdruk die u naar de pagina-versies gaat.
5. Klik op de GUID's onder de huidige versie.

    ![CertInfo1]

6. Nu moet u op het scherm zoals hieronder. 'De vingerafdruk van het' naar 'De vingerafdruk van certificaat' in het browservenster kopiëren
7. Kopieer de geheim-id-informatie voor de 'certificaat-URL' op andere browservenster.


![CertInfo2]


Controleer de **geavanceerde instellingen configureren** vak in te voeren voor clientcertificaten voor **Beheerclient** en **alleen-lezen client**. Voer de vingerafdruk van het clientcertificaat admin en de vingerafdruk van het certificaat alleen-lezengebruiker in, indien van toepassing in deze velden. Wanneer beheerders probeert verbinding maken met het cluster, krijgen ze toegang alleen als ze beschikken over een certificaat met een vingerafdruk die overeenkomt met de vingerafdruk waarden die u hier opgeeft.  

#### <a name="4-summary"></a>4. Samenvatting

U bent nu klaar voor het implementeren van het cluster. Voordat u dat doet, downloadt u het certificaat, zoekt u naar binnen het grote blauw informatief vak voor de koppeling. Zorg ervoor dat het certificaat op een veilige plaats bewaren. u moet verbinding maken met uw cluster. Omdat er momenteel geen een wachtwoord voor het certificaat dat u hebt gedownload, is het raadzaam dat u een categorie toevoegen.

Als u wilt maken van het cluster hebt voltooid, klikt u op **maken**. Desgewenst kunt u de sjabloon downloaden. 

![Samenvatting]

U kunt de voortgang van het maken in de meldingen bekijken. (Klik op het belpictogram bij de statusbalk rechtsboven in uw scherm.) Als u tijdens het maken van het cluster op **Vastmaken aan Startboard** hebt geklikt, ziet u dat **Service Fabric-cluster implementeren** is vastgemaakt aan het **Startboard**.

Beheerbewerkingen wilt uitvoeren op het cluster met Powershell of CLI, moet u verbinding maken met uw cluster, voor meer informatie over het op [verbinding maken met uw cluster](service-fabric-connect-to-secure-cluster.md).

### <a name="view-your-cluster-status"></a>De clusterstatus van uw bekijken
![Schermopname van het clusterdetails in het dashboard.][ClusterDashboard]

Als uw cluster is gemaakt, kunt u uw cluster in de portal controleren:

1. Ga naar **Bladeren** en klik op **Service Fabric-Clusters**.
2. Zoek uw cluster en klik erop.
3. U kunt de gegevens van uw cluster nu bekijken op het dashboard, waaronder het openbare eindpunt van het cluster en een koppeling naar Service Fabric Explorer.

De **knooppunt Monitor** gedeelte van het cluster dashboard blade geeft het aantal virtuele machines die in orde is en niet in orde zijn. U vindt meer informatie over de status van het cluster op [Service Fabric health model inleiding][service-fabric-health-introduction].

> [!NOTE]
> Service Fabric-clusters vereisen een bepaald aantal knooppunten voor altijd voor beschikbaarheid onderhouden en het behouden van status - aangeduid als 'onderhouden quorum'. Daarom het is doorgaans niet veilig afsluiten alle machines in het cluster tenzij u eerst hebt uitgevoerd een [volledige back-up van de staat][service-fabric-reliable-services-backup-restore].
> 
> 

## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Extern verbinding maken met een virtuele-Machineschaalset-exemplaar of een clusterknooppunt
Elk van de NodeTypes geeft u in uw cluster resultaten in een virtuele-Machineschaalset ophalen van de installatie. <!--See [Remote connect to a Virtual Machine Scale Set instance][remote-connect-to-a-vm-scale-set] for details. -->

## <a name="next-steps"></a>Volgende stappen
U hebt op dit moment een beveiligde cluster dat gebruik van certificaten voor beheer-verificatie. Vervolgens [verbinding maken met uw cluster](service-fabric-connect-to-secure-cluster.md) en meer informatie over hoe [toepassing geheimen beheren](service-fabric-application-secret-management.md).  Ook meer informatie over [Service Fabric-ondersteuningsopties](service-fabric-support.md).

<!-- Links -->
[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[azure-portal]: https://portal.azure.com/
[key-vault-get-started]: ../key-vault/key-vault-get-started.md
[create-cluster-arm]: service-fabric-cluster-creation-via-arm.md
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-cluster-security-roles]: service-fabric-cluster-security-roles.md
[service-fabric-cluster-capacity]: service-fabric-cluster-capacity.md
[service-fabric-cluster-durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
[service-fabric-connect-and-communicate-with-services]: service-fabric-connect-and-communicate-with-services.md
[service-fabric-health-introduction]: service-fabric-health-introduction.md
[service-fabric-reliable-services-backup-restore]: service-fabric-reliable-services-backup-restore.md
<!--[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md#remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node -->
[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md
[service-fabric-cluster-upgrade]: service-fabric-cluster-upgrade.md

<!--Image references-->
[SearchforServiceFabricClusterTemplate]: ./media/service-fabric-cluster-creation-via-portal/SearchforServiceFabricClusterTemplate.png
[CreateRG]: ./media/service-fabric-cluster-creation-via-portal/CreateRG.png
[CreateNodeType]: ./media/service-fabric-cluster-creation-via-portal/NodeType.png
[BasicSecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/BasicSecurityConfigs.PNG
[CreateKeyVault]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault.PNG
[CreateKeyVault2]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault2.PNG
[CreateKeyVault3]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault3.PNG
[CreateKeyVault4]: ./media/service-fabric-cluster-creation-via-portal/CreateKeyVault4.PNG
[CertInfo0]: ./media/service-fabric-cluster-creation-via-portal/CertInfo0.PNG
[CertInfo1]: ./media/service-fabric-cluster-creation-via-portal/CertInfo1.PNG
[CertInfo2]: ./media/service-fabric-cluster-creation-via-portal/CertInfo2.PNG
[SecurityCustomOption]: ./media/service-fabric-cluster-creation-via-portal/SecurityCustomOption.PNG
[DownloadCert]: ./media/service-fabric-cluster-creation-via-portal/DownloadCert.PNG
[Samenvatting]: ./media/service-fabric-cluster-creation-via-portal/Summary.PNG
[SecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png
[Notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png
[ClusterDashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
