---
title: Een Service Fabric-cluster maken in Azure portal | Microsoft Docs
description: Meer informatie over het instellen van een beveiligd Service Fabric-cluster in Azure met behulp van Azure portal en Azure Key Vault.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: vturecek
ms.assetid: 426c3d13-127a-49eb-a54c-6bde7c87a83b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/06/2018
ms.author: aljo
ms.openlocfilehash: f0c2108ee75f843e8285c5e2c5c55834643dc7da
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52620537"
---
# <a name="create-a-service-fabric-cluster-in-azure-using-the-azure-portal"></a>Een Service Fabric-cluster maken in Azure met behulp van de Azure-portal
> [!div class="op_single_selector"]
> * [Azure Resource Manager](service-fabric-cluster-creation-via-arm.md)
> * [Azure Portal](service-fabric-cluster-creation-via-portal.md)
> 
> 

Dit is een stapsgewijze handleiding die u helpt bij de stappen voor het instellen van een Service Fabric-cluster (Linux of Windows) in Azure met behulp van de Azure-portal. Deze handleiding leidt u door de volgende stappen uit:

* Een cluster maken in Azure via de Azure-portal.
* Beheerders met behulp van certificaten verifiëren.

> [!NOTE]
> Voor meer geavanceerde beveiligingsopties, zoals gebruikersverificatie met Azure Active Directory en het instellen van certificaten voor Toepassingsbeveiliging, [maken van uw cluster met behulp van Azure Resource Manager][create-cluster-arm].
> 
> 

## <a name="cluster-security"></a>Clusterbeveiliging 
Er worden in Service Fabric certificaten gebruikt voor verificatie en versleuteling om verschillende aspecten van een cluster en de bijbehorende toepassingen te beveiligen. Zie voor meer informatie over het gebruik van certificaten in Service Fabric [Service Fabric-clusterbeveiligingsscenario's][service-fabric-cluster-security].

Als dit de eerste keer dat u het maken van een service fabric-cluster of een cluster voor test-workloads implementeert, kunt u doorgaan met de volgende sectie (**-cluster maken in Azure Portal**) en het genereren van certificaten die nodig zijn voor systeem de clusters die testwerkbelastingen uitvoeren. Als u het instellen van een cluster voor werkbelastingen voor productie zijn en vervolgens doorgaan met lezen.

#### <a name="cluster-and-server-certificate-required"></a>Cluster en de server-certificaat (vereist)
Dit certificaat is vereist voor een cluster beveiligen en te voorkomen dat onbevoegde toegang toe. Het biedt beveiliging voor clusters in een aantal manieren:

* **Cluster-verificatie:** knooppunt-naar-knooppunt communicatie voor cluster federation verifieert. Alleen de knooppunten die hun identiteit met dit certificaat bewijzen kunnen kunnen deelnemen aan het cluster.
* **Server-verificatie:** verifieert de eindpunten voor clusterbeheer aan een management-client, zodat de management-client weet dat deze communiceert met de echt cluster. Dit certificaat biedt ook SSL voor de HTTPS-API en voor Service Fabric Explorer via HTTPS.

Het certificaat moet voldoen aan de volgende vereisten voor deze doeleinden:

* Het certificaat moet een persoonlijke sleutel bevatten.
* Het certificaat moet worden gemaakt voor sleuteluitwisseling, exporteerbaar naar een Personal Information Exchange (PFX)-bestand.
* Van het certificaat **onderwerpnaam moet overeenkomen met het domein** gebruikt voor toegang tot de Service Fabric-cluster. Dit is vereist voor SSL voor de HTTPS-eindpunten voor beheer van het cluster en de Service Fabric Explorer. U kunt een SSL-certificaat van een certificeringsinstantie (CA) kan niet ophalen voor de `.cloudapp.azure.com` domein. Een aangepaste domeinnaam voor uw cluster aan te schaffen. Wanneer u een certificaat van een CA aanvraagt moet de onderwerpnaam van het certificaat overeenkomen met de aangepaste domeinnaam voor uw cluster gebruikt.

#### <a name="client-authentication-certificates"></a>Certificaten voor clientverificatie
Aanvullende clientcertificaten verifiëren van beheerders voor cluster-beheertaken. Service Fabric heeft twee toegangsniveaus: **admin** en **alleen-lezengebruiker**. Ten minste moet één certificaat voor beheerderstoegang worden gebruikt. Voor extra beveiliging op gebruikersniveau toegang, moet een afzonderlijk certificaat worden opgegeven. Zie voor meer informatie over toegang tot rollen, [op rollen gebaseerd toegangsbeheer voor Service Fabric-clients][service-fabric-cluster-security-roles].

U hoeft niet te uploaden van certificaten voor clientverificatie naar Key Vault voor gebruik met Service Fabric. Deze certificaten moeten alleen worden opgegeven voor gebruikers die zijn gemachtigd voor Clusterbeheer. 

> [!NOTE]
> Azure Active Directory is de aanbevolen manier om clients voor bewerkingen voor beheer van een cluster te verifiëren. Voor het gebruik van Azure Active Directory, moet u [maken van een cluster met behulp van Azure Resource Manager][create-cluster-arm].
> 
> 

#### <a name="application-certificates-optional"></a>Toepassingscertificaten (optioneel)
Een willekeurig aantal extra certificaten kan worden geïnstalleerd op een cluster voor de toepassing uit veiligheidsoverwegingen. Voordat u uw cluster maakt, houd rekening met de scenario's voor Toepassingsbeveiliging waarvoor een certificaat worden geïnstalleerd op de knooppunten, zoals:

* Versleuteling en ontsleuteling van waarden van de configuratie van toepassing
* Versleuteling van gegevens tussen knooppunten tijdens de replicatie 

Toepassingscertificaten kunnen niet worden geconfigureerd wanneer [het maken van een cluster via Azure portal](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/service-fabric/service-fabric-cluster-creation-via-portal.md). Voor het configureren van toepassingscertificaten tijdens de installatie cluster, moet u [maken van een cluster met behulp van Azure Resource Manager][create-cluster-arm]. U kunt ook toepassingscertificaten toevoegen aan het cluster nadat deze is gemaakt.

## <a name="create-cluster-in-the-azure-portal"></a>Cluster maken in Azure portal

Het maken van een productiecluster om te voldoen aan de behoeften van uw toepassing een planning voor hulp bij die omvat, het is raadzaam dat u leest en begrijpt de [Service Fabric-Cluster planningsoverwegingen] [ service-fabric-cluster-capacity] document. 

### <a name="search-for-the-service-fabric-cluster-resource"></a>Zoeken naar de Service Fabric-cluster-bron

Meld u aan bij [Azure Portal][azure-portal].
Klik op **een resource maken** om toe te voegen een nieuwe resourcesjabloon. Zoek de sjabloon Service Fabric-Cluster in de **Marketplace** onder **Alles**.
Selecteer **Service Fabric-Cluster** in de lijst.

![zoeken naar de sjabloon van de Service Fabric-cluster in Azure portal.][SearchforServiceFabricClusterTemplate]

Navigeer naar de **Service Fabric-Cluster** blad en klik op **maken**.

De **maken van Service Fabric-cluster** blade bevat de volgende vier stappen:

### <a name="1-basics"></a>1. Basisbeginselen
![Schermopname van het maken van een nieuwe resourcegroep.][CreateRG]

In de blade grondbeginselen moet u de algemene gegevens opgeven voor uw cluster.

1. Voer de naam van het cluster.
2. Voer een **gebruikersnaam** en **wachtwoord** voor extern bureaublad voor de virtuele machines.
3. Zorg ervoor dat u selecteert de **abonnement** dat het die uw cluster moet worden geïmplementeerd, met name als u meerdere abonnementen hebt.
4. Maak een nieuwe **resourcegroep**. Het is raadzaam zodat deze dezelfde naam als het cluster, omdat het helpt bij het vinden ze later, vooral wanneer u probeert te wijzigingen aanbrengen in uw implementatie of verwijder uw cluster.
   
   > [!NOTE]
   > Hoewel u een bestaande resourcegroep gebruikt kunt, is het raadzaam om een nieuwe resourcegroep te maken. Dit maakt het eenvoudig verwijderen van clusters en alle resources die worden gebruikt.
   > 
   > 
5. Selecteer de **locatie** waarin u wilt maken van het cluster. Als u van plan bent een bestaand certificaat dat u al hebt geüpload naar een key vault gebruiken, moet u dezelfde regio waarin uw Key vault zich bevindt. 

### <a name="2-cluster-configuration"></a>2. Clusterconfiguratie
![Maken van een knooppunttype][CreateNodeType]

Configureer uw clusterknooppunten. Knooppunttypen definiëren de VM-grootten, het aantal virtuele machines en hun eigenschappen. Uw cluster kan meer dan één knooppunttype hebben, maar het primaire knooppunttype (de eerste optie die u in de portal definieert) moet ten minste vijf VM's, hebben als dit het knooppunttype is waar Service Fabric-systeemservices worden geplaatst. Configureer geen **plaatsingseigenschappen** omdat een eigenschap van de plaatsing standaard van 'NodeTypeName' wordt automatisch toegevoegd.

> [!NOTE]
> Een veelvoorkomend scenario voor meerdere typen is een toepassing met een front-end-service en een back-endservice. U plaatst u de front-endservice op kleinere virtuele machines (VM-grootten, zoals D2_V2) met open poorten met Internet, en de back-endservice op grotere virtuele machines (met VM-grootten, zoals D3_V2, D6_V2 en D15_V2) zonder internetverbinding poorten openen.
> 

1. Kies een naam voor het knooppunttype (1-12 tekens die alleen letters en cijfers bevatten).
2. De minimale **grootte** van virtuele machines voor het primaire knooppunt type wordt aangedreven door de **duurzaamheidslaag** u kiest voor het cluster. De standaardwaarde voor de duurzaamheidslaag is Brons. Zie voor meer informatie over duurzaamheid [u bij het kiezen van de Service Fabric-cluster duurzaamheid][service-fabric-cluster-durability].
3. Selecteer de **grootte van virtuele machine**. Virtuele machines uit de D-serie hebben SSD-stations en wordt sterk aanbevolen voor stateful toepassingen. Geen gebruik van een VM-SKU die gedeeltelijke kernen heeft of minder dan 10 GB aan beschikbare schijfruimte capaciteit hebben. Raadpleeg [service fabric-cluster overweging planningsdocument] [ service-fabric-cluster-capacity] voor hulp bij het selecteren van de VM-grootte.
4.  **Eén knooppuntcluster en clusters met drie knooppunten** zijn bedoeld voor test-gebruik. Ze worden niet ondersteund voor alle actieve productieworkloads.
5. Kies de **eerste VM-schaalset** voor het knooppunttype. U kunt omhoog of omlaag het aantal virtuele machines in een knooppunttype later schalen, maar op het primaire knooppunttype, de minimumwaarde is 5 voor werkbelastingen voor productie. Andere typen kunnen hebben een minimum van een virtuele machine. De minimale **getal** van virtuele machines voor het primaire knooppunt type stations de **betrouwbaarheid** van uw cluster.  
6. Configureer **aangepaste eindpunten**. Dit veld kunt u een door komma's gescheiden lijst met poorten die u beschikbaar via de Azure Load Balancer met het openbare Internet voor uw toepassingen wilt invoeren. Bijvoorbeeld, als u van plan bent een webtoepassing in uw cluster implementeert, voert u "80" hier toestaan van verkeer op poort 80 in uw cluster. Zie voor meer informatie over eindpunten [communiceren met toepassingen][service-fabric-connect-and-communicate-with-services]
7. **Inschakelen van omgekeerde proxy**.  De [Service Fabric reverse proxy](service-fabric-reverseproxy.md) helpt microservices die worden uitgevoerd in een Service Fabric-cluster detecteren en communiceren met andere services die http-eindpunten hebben.
8. Terug in de **clusterconfiguratie** blade onder **+ optionele instellingen weergeven**, cluster configureren **diagnostics**. Diagnostische gegevens zijn standaard ingeschakeld in uw cluster om te helpen bij het oplossen van problemen. Als u wilt uitschakelen, wijzigen van de diagnostische gegevens over de **Status** overzet naar **uit**. Het uitschakelen van diagnostische gegevens wordt **niet** aanbevolen. Als u al Application Insights-project dat wordt gemaakt hebt, geeft vervolgens de sleutel, zodat de toepassingstraceringen worden doorgestuurd naar het.
9. **DNS-service opnemen**.  De [DNS-service](service-fabric-dnsservice.md) een optionele service waarmee u andere services met behulp van het DNS-protocol.
10. Selecteer de **Fabric upgrademodus** u wilt instellen van uw cluster op. Selecteer **automatische**, als u wilt dat het systeem automatisch kiezen van de meest recente versie en een upgrade uitvoert voor het cluster toe. De modus instellen op **handmatig**, als u wilt kiezen van een ondersteunde versie. Voor meer informatie over de infrastructuur modus, Zie upgraden de [document upgraden van Service Fabric-Cluster.][service-fabric-cluster-upgrade]

> [!NOTE]
> We ondersteunen alleen clusters die ondersteunde versies van Service Fabric worden uitgevoerd. Door het selecteren van de **handmatig** modus u onderneemt op de verantwoordelijkheid voor uw cluster upgraden naar een ondersteunde versie.
> 

### <a name="3-security"></a>3. Beveiliging
![Schermopname van beveiligingsconfiguraties in Azure portal.][BasicSecurityConfigs]

Instellen van een beveiligde testcluster om gemakkelijk te maken voor u, verstrekt de **Basic** optie. Als u al een certificaat hebt en hebt geüpload naar uw [sleutelkluis](/azure/key-vault/) (en de key vault voor de implementatie ingeschakeld), gebruik vervolgens de **aangepaste** optie

#### <a name="basic-option"></a>Basic-optie
Volg de schermen voor het toevoegen of opnieuw gebruiken van een bestaande sleutelkluis en een certificaat toevoegen. Het toevoegen van het certificaat is een synchrone proces en daarom moet u wachten op het certificaat moet worden gemaakt.

Bestand tegen de komen om het scherm vandaan wordt genavigeerd totdat het vorige proces is voltooid.

![CreateKeyVault]

Nu dat de key vault is gemaakt, bewerkt u het toegangsbeleid voor uw key vault. 

![CreateKeyVault2]

Klik op de **toegangsbeleid bewerken**, klikt u vervolgens **geavanceerde toegangsbeleid weergeven** en toegang tot Azure Virtual Machines voor implementatie in te schakelen. Het verdient aanbeveling om de sjabloonimplementatie in te schakelen. Nadat u uw selecties hebt gemaakt, vergeet niet om op de **opslaan** knop en sluiten van de **toegangsbeleid** deelvenster.

![CreateKeyVault3]

Voer de naam van het certificaat en klik op **OK**.

![CreateKeyVault4]

#### <a name="custom-option"></a>Aangepaste optie
Als u al hebt uitgevoerd de stappen in deze sectie overslaan de **Basic** optie.

![SecurityCustomOption]

U moet de bronsleutelkluis, URL van het certificaat en certificaatgegevens van de vingerafdruk voor het voltooien van de pagina. Als u deze bij de hand hebt, opent u een ander browservenster en doe het volgende in de Azure-portal

1. Navigeer naar uw sleutelkluis-service.
2. Selecteer het tabblad 'Eigenschappen' en 'RESOURCE-ID' naar 'Bronsleutelkluis' in het browservenster kopiëren 

    ![CertInfo0]

3. Nu, selecteer het tabblad 'Certificaten'.
4. Klik op certificaatvingerafdruk, u naar de pagina-versies gaat.
5. Klik op de GUID's u onder de huidige versie ziet.

    ![CertInfo1]

6. U zou nu moeten op het scherm, zoals hieronder. Kopieer de vingerafdruk van het hexadecimale SHA-1 naar "Vingerafdruk" van het browservenster
7. Kopieer de id van het geheim aan de 'certificaat-URL' op andere browservenster.

    ![CertInfo2]

Controleer de **geavanceerde instellingen configureren** vak in te voeren van clientcertificaten voor **Beheerclient** en **alleen-lezen client**. In deze velden, voert u de vingerafdruk van het certificaat voor de Beheerclient en de vingerafdruk van het certificaat alleen-lezen-gebruiker, indien van toepassing. Wanneer beheerders verbinding maken met het cluster, krijgen ze toegang alleen als ze beschikken over een certificaat met een vingerafdruk die overeenkomt met de vingerafdruk van waarden die u hier opgeeft.  

### <a name="4-summary"></a>4. Samenvatting

U bent nu klaar om het cluster te implementeren. Voordat u dit doen, het certificaat downloaden, zoekt u naar binnen het grote blauwe informatief vak voor de koppeling. Zorg ervoor dat u het certificaat op een veilige plaats bewaren. u moet verbinding maken met uw cluster. Omdat het certificaat dat u hebt gedownload, heeft geen een wachtwoord, is het raadzaam dat u een toevoegen.

Als u wilt maken van het cluster hebt voltooid, klikt u op **maken**. Eventueel kunt u de sjabloon downloaden.

![Samenvatting]

U kunt de voortgang van het maken in de meldingen bekijken. (Klik op het belpictogram bij de statusbalk rechtsboven in uw scherm.) Als u tijdens het maken van het cluster op **Vastmaken aan Startboard** hebt geklikt, ziet u dat **Service Fabric-cluster implementeren** is vastgemaakt aan het **Startboard**. Dit proces zal enige tijd duren. 

Beheerbewerkingen wilt uitvoeren op uw cluster met behulp van Powershell of CLI, moet u verbinding maken met uw cluster, meer informatie over het op [verbinding maken met uw cluster](service-fabric-connect-to-secure-cluster.md).

## <a name="view-your-cluster-status"></a>De clusterstatus van uw bekijken
![Schermopname van het de clusterdetails in het dashboard.][ClusterDashboard]

Zodra het cluster is gemaakt, kunt u uw cluster in de portal controleren:

1. Ga naar **Bladeren** en klikt u op **Service Fabric-Clusters**.
2. Zoek uw cluster en klikt u erop.
3. U kunt de gegevens van uw cluster nu bekijken op het dashboard, waaronder het openbare eindpunt van het cluster en een koppeling naar Service Fabric Explorer.

De **knooppunt Monitor** sectie op de blade van het cluster-dashboard geeft het aantal virtuele machines die in orde niet in orde en. U vindt meer informatie over de status van het cluster op [Service Fabric health model inleiding][service-fabric-health-introduction].

> [!NOTE]
> Service Fabric-clusters is vereist voor een bepaald aantal knooppunten zodat u altijd beschikbaarheid en het behouden van status - aangeduid als 'onderhouden quorum'. Daarom is het doorgaans niet veilig afsluiten alle machines in het cluster, tenzij u eerst hebt uitgevoerd een [volledige back-up van uw staat][service-fabric-reliable-services-backup-restore].
> 
> 

## <a name="remote-connect-to-a-virtual-machine-scale-set-instance-or-a-cluster-node"></a>Extern verbinding maken met een virtuele-Machineschaalset-exemplaar of een clusterknooppunt
Elk van de NodeTypes die u opgeeft in de resultaten van uw cluster in een virtuele-Machineschaalset ophalen instellen. <!--See [Remote connect to a Virtual Machine Scale Set instance][remote-connect-to-a-vm-scale-set] for details. -->

## <a name="next-steps"></a>Volgende stappen
Op dit moment hebt u een beveiligd cluster met behulp van certificaten voor beheer van verificatie. Volgende [verbinding maken met uw cluster](service-fabric-connect-to-secure-cluster.md) en leer hoe u [toepassingsgeheimen beheren](service-fabric-application-secret-management.md).  Bovendien krijgt u informatie over [ondersteuningsopties voor Service Fabric](service-fabric-support.md).

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
