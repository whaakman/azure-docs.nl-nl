---
title: Domein-HDInsight-clusters - Azure configureren | Microsoft Docs
description: Meer informatie over het instellen en configureren van domein HDInsight-clusters
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 0cbb49cc-0de1-4a1a-b658-99897caf827c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: saurinsh
ms.openlocfilehash: af75d63caca24f389345c964e2dc506a255bec19
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="configure-domain-joined-hdinsight-clusters-preview"></a>Domein-HDInsight-clusters (Preview) configureren

Meer informatie over het instellen van een Azure HDInsight-cluster met Azure Active Directory (Azure AD) en [Apache Zwerver](http://hortonworks.com/apache/ranger/) om te profiteren van sterke authenticatie- en uitgebreide op rollen gebaseerde toegang beleid voor toegangsbeheer (RBAC).  Domein HDInsight kan alleen worden geconfigureerd op Linux gebaseerde clusters. Zie voor meer informatie [introduceren domein HDInsight-clusters](apache-domain-joined-introduction.md).

> [!IMPORTANT]
> Oozie is niet ingeschakeld op HDInsight domein.

Dit artikel is de eerste zelfstudie van een serie:

* Maak een HDInsight-cluster dat is verbonden met Azure AD (via de mogelijkheid Azure Directory Domain Services) met Apache Zwerver ingeschakeld.
* Maken en Hive beleidsregels via Apache Zwerver en toestaan dat gebruikers (bijvoorbeeld gegevenswetenschappers) verbinding maken met Hive ODBC-hulpprogramma's, zoals Excel, Tableau enzovoort met. Microsoft werkt over het toevoegen van andere werkbelastingen verwerken, zoals HBase, Spark en Storm, aan het domein HDInsight snel.

Een voorbeeld van de laatste topologie ziet er als volgt uit:

![Domein-HDInsight-topologie](./media/apache-domain-joined-configure/hdinsight-domain-joined-topology.png)

Omdat Azure AD op dit moment biedt alleen ondersteuning voor klassieke virtuele netwerken (vnet's) en Linux gebaseerde HDInsight-clusters alleen ondersteuning voor VNets op basis van Azure Resource Manager, HDInsight Azure AD-integratie vereist twee VNets en een peering ertussen. Zie voor informatie vergelijking tussen de twee implementatiemodellen [Azure Resource Manager versus klassieke implementatie: begrijpen implementatiemodellen en de status van uw resources](../../azure-resource-manager/resource-manager-deployment-model.md). De twee VNets moet zich in dezelfde regio bevinden als de Azure AD DS.

Azure-service-namen moeten uniek zijn. De volgende namen worden gebruikt in deze zelfstudie. Contoso is een fictieve naam. U moet vervangen *contoso* met een andere naam wanneer u de zelfstudie doorloopt. 

**Namen:**

| Eigenschap | Waarde |
| --- | --- |
| Azure AD-VNet |contosoaadvnet |
| Azure AD-Vnet-resourcegroep |contosoaadrg |
| Azure AD-map |contosoaaddirectory |
| Azure AD-domeinnaam |Contoso (contoso.onmicrosoft.com) |
| HDInsight-VNet |contosohdivnet |
| HDInsight VNet resourcegroep |contosohdirg |
| HDInsight-cluster |contosohdicluster |

Deze zelfstudie bevat de stappen voor het configureren van een domein HDInsight-cluster. Elke sectie bevat koppelingen naar andere artikelen met achtergrondinformatie over.

## <a name="prerequisite"></a>Voorwaarde:
* Vertrouwd raken met [Azure AD Domain Services](https://azure.microsoft.com/services/active-directory-ds/) de [prijzen](https://azure.microsoft.com/pricing/details/active-directory-ds/) structuur.
* Zorg ervoor dat uw abonnement wilt plaatsen voor deze openbare preview. U kunt dit doen door te sturen een e-mail naar hdipreview@microsoft.com met uw abonnement-ID.
* Een SSL-certificaat dat is ondertekend door een instantie voor het ondertekenen van voor uw domein. Het certificaat is vereist voor het configureren van beveiligde LDAP. Zelfondertekende certificaten kunnen niet worden gebruikt.

## <a name="procedures"></a>Procedures
1. Een Azure classic VNet maken voor uw Azure AD.  
2. Maak en configureer Azure AD en Azure AD DS.
3. Een HDInsight-VNet maken in de Azure-resource management-modus.
4. De twee VNets peer.
5. Maak een HDInsight-cluster.

> [!NOTE]
> Deze zelfstudie wordt ervan uitgegaan dat u geen een Azure AD hebt. Als u hebt, kunt u het gedeelte in stap 2 overslaan.
> 
> 

## <a name="create-an-azure-virtual-network-classic"></a>Een Azure-netwerk (klassiek) maken
In deze sectie maakt u een virtueel netwerk (klassiek) met de Azure portal. In de volgende sectie schakelt u de Azure AD DS voor uw Azure AD in het virtuele netwerk. Zie voor meer informatie over de volgende procedure en gebruiken van andere methoden voor het virtueel netwerk maken [een virtueel netwerk (klassiek) maken met behulp van de Azure-portal](../../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

**Een klassieke VNet aanmaken**

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 
2. Klik op **nieuwe** > **Networking** > **virtueel netwerk**.
3. In **een implementatiemodel selecteren**, selecteer **klassieke**, en klik vervolgens op **maken**.
4. Typ of selecteer de volgende waarden:
   
   * **Naam**: contosoaadvnet
   * **Adresruimte**: 10.1.0.0/16
   * **De subnetnaam van het**: Subnet1
   * **Adresbereik van**: 10.1.0.0/24
   * **Abonnement**: (Selecteer een abonnement dat u gebruikt voor het maken van dit VNet.)
   * **ResourceGroup**: contosoaadrg
   * **Locatie**: (Selecteer een regio voor uw HDInsight-cluster.)
     
     > [!IMPORTANT]
     > U moet een locatie die ondersteuning biedt voor Azure AD DS. Zie [Producten beschikbaar per regio](https://azure.microsoft.com/en-us/regions/services/) voor meer informatie. 
     > 
     > Zowel het klassieke VNet en het Resource-Group VNet, moet in dezelfde regio bevinden als de Azure AD DS.
     > 
     > 
5. Klik op **Maken** om het VNet te maken.

## <a name="create-and-configure-azure-ad-ds-for-your-azure-ad"></a>Maken en configureren van Azure AD DS voor uw Azure AD
U wordt in deze sectie:

1. Maak een Azure AD.
2. Gebruikers van Azure AD maken. Deze gebruikers zijn Domeingebruikers. U kunt de eerste gebruiker gebruiken voor het configureren van het HDInsight-cluster met Azure AD.  De twee gebruikers zijn optioneel voor deze zelfstudie. Ze worden gebruikt [configureren Hive-beleid voor domein-HDInsight-clusters](apache-domain-joined-run-hive.md) wanneer u Apache Zwerver beleid configureert.
3. Maak de AAD-DC-beheerdersgroep en de Azure AD-gebruiker toevoegen aan de groep. U kunt deze gebruiker gebruiken voor het maken van de organisatie-eenheid.
4. Azure AD Domain Services (Azure AD DS) voor de Azure AD in te schakelen.
5. LDAPS configureren voor Azure AD. Lightweight Directory Access Protocol (LDAP) wordt gebruikt voor het lezen van en schrijven naar Azure AD.

Als u liever een bestaande Azure AD gebruikt, kunt u stap 1 en 2 overslaan.

**Maken van een Azure AD**

1. Van de [klassieke Azure-portal](https://manage.windowsazure.com), klikt u op **nieuw** > **App Services** > **Active Directory** > **Directory** > **aangepast maken**. 
2. Typ of selecteer de volgende waarden:
   
   * **Naam**: contosoaaddirectory
   * **Domeinnaam**: contoso.  Deze naam moet uniek zijn.
   * **Land of regio**: Selecteer uw land of regio.
3. Klik op **Voltooien**.

**Een Azure AD-gebruiker maken**

1. Van de [klassieke Azure-portal](https://manage.windowsazure.com), klikt u op **Active Directory** -> **contosoaaddirectory**. 
2. Klik op **gebruikers** in het menu bovenaan.
3. Klik op **gebruiker toevoegen**.
4. Voer **gebruikersnaam**, en klik vervolgens op **volgende**. 
5. Gebruikersprofiel; configureren In **rol**, selecteer **globale beheerder**; en klik vervolgens op **volgende**.  De rol globale beheerder is nodig voor het maken van de organisatie-eenheden.
6. Klik op **maken** ophalen van een tijdelijk wachtwoord.
7. Maak een kopie van het wachtwoord en klik vervolgens op **Complete**. Verderop in deze zelfstudie maakt u deze gebruiker globale beheerder gebruikt voor het maken van het HDInsight-cluster.

Volg dezelfde procedure voor het maken van twee meer gebruikers met de **gebruiker** rol, hiveuser1 en hiveuser2. De volgende gebruikers worden gebruikt voor [configureren Hive-beleid voor domein-HDInsight-clusters](apache-domain-joined-run-hive.md).

**DC beheerders van de AAD-groep maken en toevoegen van een Azure AD-gebruiker**

1. Van de [klassieke Azure-portal](https://manage.windowsazure.com), klikt u op **Active Directory** > **contosoaaddirectory**. 
2. Klik op **groepen** in het menu bovenaan.
3. Klik op **een groep toevoegen** of **groep toevoegen**.
4. Typ of selecteer de volgende waarden:
   
   * **Naam**: AAD DC-beheerders.  Naam van de groep niet worden gewijzigd.
   * **Groepstype**: beveiliging.
5. Klik op **Voltooien**.
6. Klik op **AAD DC beheerders** openen van de groep.
7. Klik op **leden toevoegen**.
8. Selecteer de eerste gebruiker die u in de vorige stap hebt gemaakt en klik vervolgens op **Complete**.
9. Herhaal de stappen voor het maken van een andere groep met de naam **HiveUsers**, en de twee Hive-gebruikers toevoegen aan de groep.

Zie voor meer informatie [Azure AD Domain Services (Preview): Maak de groep 'AAD DC Administrators'](../../active-directory-domain-services/active-directory-ds-getting-started.md).

**Azure AD DS voor uw Azure AD inschakelen**

1. Van de [klassieke Azure-portal](https://manage.windowsazure.com), klikt u op **Active Directory** > **contosoaaddirectory**. 
2. Klik op **configureren** in het menu bovenaan.
3. Schuif omlaag naar **domeinservices**, en stel de volgende waarden:
   
   * **Domeinservices inschakelen voor deze map**: Ja.
   * **DNS-domeinnaam van domeinservices**: dit betekent dat de standaard DNS-naam van de Azure-map. Bijvoorbeeld: contoso.onmicrosoft.com.
   * **Domeinservices verbinden met dit virtuele netwerk**: Selecteer het klassieke virtuele netwerk dat u eerder hebt gemaakt, dat wil zeggen **contosoaadvnet**.
4. Klik op **opslaan** van de onderkant van de pagina. U ziet **in behandeling...**  naast **domeinservices inschakelen voor deze map**.  
5. Wachten tot **in behandeling...**  verdwijnt, en **IP-adres** opgehaald ingevuld. Twee IP-adressen wordt ophalen ingevuld. Dit zijn de IP-adressen van de domeincontrollers die zijn ingericht met Domain Services. Elk IP-adres is zichtbaar nadat de corresponderende domeincontroller ingericht en gereed is. Noteer de twee IP-adressen. U moet ze later.

Zie voor meer informatie [inschakelen Azure Active Directory Domain Services met Azure portal](../../active-directory-domain-services/active-directory-ds-getting-started.md).

**Synchroniseren van wachtwoord**

Als u uw eigen domein gebruikt, moet u het wachtwoord te synchroniseren. Zie [wachtwoordsynchronisatie inschakelen voor Azure AD domain services voor een alleen-Azure AD-directory](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md).

**LDAPS configureren voor de Azure AD**

1. Ophalen van een SSL-certificaat dat is ondertekend door een instantie voor het ondertekenen van voor uw domein. Als u een zelfondertekend certificaat gebruiken wilt, kunt contact met hdipreview@microsoft.com voor een uitzondering.
2. Van de [klassieke Azure-portal](https://manage.windowsazure.com), klikt u op **Active Directory** > **contosoaaddirectory**. 
3. Klik op **configureren** in het menu bovenaan.
4. Schuif naar **domeinservices**.
5. Klik op **configureren certificaat**.
6. Volg de instructies voor het certificaatbestand en het wachtwoord opgeven. U ziet **in behandeling...**  naast **domeinservices inschakelen voor deze map**.  
7. Wachten tot **in behandeling...**  verdwijnt, en **Secure LDAP certificaat** is ingevuld.  Dit kan 10 minuten of langer duren.

> [!NOTE]
> Als een aantal achtergrondtaken zijn op de Azure AD DS wordt uitgevoerd, wordt er een fout opgetreden tijdens het uploaden certificaat - <i>er is een bewerking wordt uitgevoerd voor deze tenant. Probeer het later opnieuw</i>.  Als deze fout zich voordoet, probeer het na enige tijd opnieuw. Het tweede domain controller IP-adres kan worden ingericht tot 3 uur duren.
> 
> 

Zie voor meer informatie [configureren beveiligde LDAP (LDAPS) gebruiken voor een Azure AD Domain Services beheerd domein](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-resource-manager-vnet-for-hdinsight-cluster"></a>Een Resource Manager VNet voor HDInsight-cluster maken
In deze sectie maakt u een Azure Resource Manager VNet dat wordt gebruikt voor het HDInsight-cluster. Zie voor meer informatie over het maken van Azure VNET van andere methoden [een virtueel netwerk maken](../../virtual-network/virtual-networks-create-vnet-arm-pportal.md)

Nadat het VNet is gemaakt, configureert u het Resource Manager VNet voor het gebruik van dezelfde DNS-servers als voor de Azure AD-VNet. Als u de stappen in deze zelfstudie voor het maken van het klassieke VNet en de Azure AD hebt gevolgd, wordt de DNS-servers zijn 10.1.0.4 en 10.1.0.5.

**Een Resource Manager VNet aanmaken**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **nieuw**, **Networking**, en vervolgens **virtueel netwerk**. 
3. In **een implementatiemodel selecteren**, selecteer **Resource Manager**, en klik vervolgens op **maken**.
4. Typ of selecteer de volgende waarden:
   
   * **Naam**: contosohdivnet
   * **Adresruimte**: 10.2.0.0/16. Zorg ervoor dat het adresbereik niet overlappen met het IP-adresbereik van de klassieke VNet.
   * **De subnetnaam van het**: Subnet1
   * **Adresbereik van**: 10.2.0.0/24
   * **Abonnement**: (uw Azure-abonnement selecteren.)
   * **Resourcegroep**: contosohdirg
   * **Locatie**: (Selecteer dezelfde locatie als de Azure AD-VNet, dat wil zeggen contosoaadvnet.)
5. Klik op **Create**.

**DNS configureren voor het Resource Manager VNet**

1. Van de [Azure-portal](https://portal.azure.com), klikt u op **meer services** -> **virtuele netwerken**. Zorg ervoor dat geen u klikt op **virtuele netwerken (klassiek)**.
2. Klik op **contosohdivnet**.
3. Klik op **DNS-servers** vanaf de linkerkant van de nieuwe blade.
4. Klik op **aangepaste**, en voer de volgende waarden:
   
   * 10.1.0.4
   * 10.1.0.5
     
     Deze DNS-server IP-adressen moeten overeenkomen met de DNS-servers in het VNet in Azure AD (klassieke VNet).
5. Klik op **Opslaan**.

## <a name="peer-the-azure-ad-vnet-and-the-hdinsight-vnet"></a>Peer van de Azure AD-VNet en het HDInsight-VNet
**Als de twee VNet peer**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **meer services** in het menu links.
3. Klik op **virtuele netwerken**. Klik niet op **virtuele netwerken (klassiek)**.
4. Klik op **contosohdivnet**.  Dit is het HDInsight VNet.
5. Klik op **Peerings** in het menu links van de blade.
6. Klik op **toevoegen** in het menu bovenaan. Het openen van de **toevoegen peering** blade.
7. Op de **toevoegen peering** blade instellen of selecteert u de volgende waarden:
   
   * **Naam**: ContosoAADHDIVNetPeering
   * **Virtueel netwerk implementatiemodel**: klassieke
   * **Abonnement**: Selecteer de abonnementsnaam van uw voor het vnet klassieke (Azure AD) gebruikt.
   * **Virtueel netwerk**: contosoaadvnet.
   * **Toestaan van toegang tot het virtuele netwerk**: (controleren)
   * **Toestaan van doorgestuurd verkeer**: (Raadpleeg). Laat de twee selectievakjes uitgeschakeld.
8. Klik op **OK**.

## <a name="create-hdinsight-cluster"></a>HDInsight-cluster maken
In deze sectie maakt u een Linux gebaseerde Hadoop-cluster in HDInsight met behulp van de Azure-portal of [Azure Resource Manager-sjabloon](../../azure-resource-manager/resource-group-template-deploy.md). Voor andere methoden voor het maken van cluster en kennis van de instellingen, Zie [HDInsight-clusters maken](../hdinsight-hadoop-provision-linux-clusters.md). Zie voor meer informatie over het gebruik van Resource Manager-sjabloon maken van Hadoop-clusters in HDInsight [maken Hadoop-clusters in HDInsight met behulp van Resource Manager-sjablonen](../hdinsight-hadoop-create-windows-clusters-arm-templates.md)

**Maken van een domein HDInsight-cluster met de Azure portal**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **nieuw**, **Intelligence en analyse**, en vervolgens **HDInsight**.
3. Van de **nieuwe HDInsight-cluster** blade invoeren of selecteren van de volgende waarden:
   
   * **Clusternaam**: Voer een nieuwe clusternaam voor het domein HDInsight-cluster.
   * **Abonnement**: Selecteer een Azure-abonnement gebruikt voor het maken van dit cluster.
   * **Configuratie van het cluster**:
     
     * **Type cluster**: Hadoop. Domein HDInsight is momenteel alleen ondersteund op Hadoop-clusters.
     * **Besturingssysteem**: Linux.  HDInsight domein wordt alleen ondersteund op Linux gebaseerde HDInsight-clusters.
     * **Versie**: HDI 3.6. HDInsight domein wordt alleen ondersteund op HDInsight-cluster versie 3.6.
     * **Type cluster**: PREMIUM
       
       Klik op **Selecteer** de wijzigingen wilt opslaan.
   * **Referenties**: Configureer de referenties voor de gebruiker van het cluster en de SSH-gebruiker.
   * **Gegevensbron**: een nieuw opslagaccount maken of een bestaand opslagaccount gebruiken als het standaardopslagaccount voor het HDInsight-cluster. De locatie moet hetzelfde zijn als de twee VNets.  De locatie is ook de locatie van het HDInsight-cluster.
   * **Prijzen**: Selecteer het aantal worker-knooppunten van het cluster.
   * **Geavanceerde configuraties**: 
     
     * **Lid worden van domein & Vnet/Subnet**: 
       
       * **Domeininstellingen**: 
         
         * **Domeinnaam**: contoso.onmicrosoft.com
         * **Domeingebruikersnaam**: Voer de gebruikersnaam van een domein. Dit domein moet beschikken over de volgende bevoegdheden: machines toevoegen aan het domein op en plaats deze in de organisatie-eenheid die u tijdens het maken van het cluster opgeeft; Maken van de service-principals binnen de organisatie-eenheid die u tijdens het maken van het cluster opgeeft; Omgekeerde DNS-vermeldingen te maken. Deze domeingebruiker, worden de beheerder van dit domein HDInsight-cluster.
         * **Domeinwachtwoord**: Voer het wachtwoord voor gebruiker.
         * **Organisatie-eenheid**: Geef de DN-naam van de organisatie-eenheid die u wilt gebruiken met HDInsight-cluster. Bijvoorbeeld: OU = HDInsightOU, DC = contoso, DC = onmicrosoft gebruikt, DC = com. Als deze organisatie-eenheid niet bestaat, probeert het HDInsight-cluster te maken van deze organisatie-eenheid. Controleer of de organisatie-eenheid is al aanwezig of het domeinaccount machtigingen heeft voor een nieuwe maken. Als u het domeinaccount dat deel uitmaakt van de beheerders AADDC gebruikt, heeft dit vereiste machtigingen om de organisatie-eenheid maken.
         * **LDAPS URL**: ldaps://contoso.onmicrosoft.com:636
         * **Gebruikersgroep toegang**: Geef de beveiligingsgroep waarvan gebruikers die u wilt synchroniseren met het cluster. Bijvoorbeeld: HiveUsers.
           
           Klik op **Selecteer** de wijzigingen wilt opslaan.
           
           ![HDInsight-portal domein configureren domeininstelling](./media/apache-domain-joined-configure/hdinsight-domain-joined-portal-domain-setting.png)
       * **Virtueel netwerk**: contosohdivnet
       * **Subnet**: Subnet1
         
         Klik op **Selecteer** de wijzigingen wilt opslaan.        
         Klik op **Selecteer** de wijzigingen wilt opslaan.
   * **Resourcegroep**: Selecteer de resourcegroep die wordt gebruikt voor het HDInsight-VNet (contosohdirg).
4. Klik op **Create**.  

Een andere optie voor het maken van domein HDInsight-cluster is het gebruik van Azure Resource Management-sjabloon. De volgende procedure laat zien u hoe:

**Een domein HDInsight-cluster met een Resource Management-sjabloon maken**

1. Klik op de volgende afbeelding om te openen van een Resource Manager-sjabloon in de Azure portal. De Resource Manager-sjabloon bevindt zich in een openbare blob-container. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-domain-joined-hdinsight-cluster.json" target="_blank"><img src="./media/apache-domain-joined-configure/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Van de **Parameters** blade, voer de volgende waarden:
   
   * **Abonnement**: (Selecteer uw Azure-abonnement).
   * **Resourcegroep**: klik op **gebruik bestaande**, en geef dezelfde resourcegroep die u hebt gebruikt.  Bijvoorbeeld contosohdirg. 
   * **Locatie**: Geef de locatie van een resourcegroep.
   * **Clusternaam**: voer een naam in voor het Hadoop-cluster dat u maakt. Bijvoorbeeld contosohdicluster.
   * **Type cluster**: een cluster selecteren.  De standaardwaarde is **hadoop**.
   * **Locatie**: Selecteer een locatie voor het cluster.  Het standaardopslagaccount maakt gebruik van dezelfde locatie.
   * **Aantal Worker-knooppunten cluster**: Selecteer het aantal worker-knooppunten.
   * **Aanmeldgegevens voor het cluster**: de standaardaanmeldnaam is **admin**.
   * **SSH-gebruikersnaam en -wachtwoord**: de standaardgebruikersnaam is **sshuser**.  U kunt de naam wijzigen. 
   * **Virtueel netwerk-Id**: /subscriptions/&lt;abonnements-id > /resourceGroups/&lt;ResourceGroupName > /providers/Microsoft.Network/virtualNetworks/&lt;VNetName >
   * **Virtueel netwerksubnet**: /subscriptions/&lt;abonnements-id > /resourceGroups/&lt;ResourceGroupName > /providers/Microsoft.Network/virtualNetworks/&lt;VNetName >/subnetten/Subnet1
   * **Domeinnaam**: contoso.onmicrosoft.com
   * **DN-naam van organisatie-eenheid**: OU = HDInsightOU, DC = contoso, DC = onmicrosoft gebruikt, DC = com
   * **Gebruikers clustergroep DNs**: [\"HiveUsers\"]
   * **LDAPUrls**: ["ldaps://contoso.onmicrosoft.com:636"]
   * **DomainAdminUserName**: (Geef de gebruikersnaam van de domein-beheerder)
   * **DomainAdminPassword**: (Geef het beheerderswachtwoord voor het domein)
   * **Ik ga akkoord met de voorwaarden en bepalingen hierboven**: (controleren)
   * **Vastmaken aan dashboard**: (controleren)
3. Klik op **Kopen**. U ziet een nieuwe tegel met de titel **Implementatie van sjabloonimplementatie**. Het duurt ongeveer 20 minuten om een cluster te maken. Zodra het cluster is gemaakt, kunt u de cluster-blade in de portal om deze te openen.

Nadat u de zelfstudie hebt voltooid, kunt u het cluster verwijdert. Met HDInsight worden uw gegevens opgeslagen in Azure Storage zodat u een cluster veilig kunt verwijderen wanneer deze niet wordt gebruikt. Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt. Zie voor de instructies van het verwijderen van een cluster [beheren Hadoop-clusters in HDInsight met behulp van de Azure-portal](../hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Volgende stappen
* Zie [Configure Hive policies for Domain-joined HDInsight clusters](apache-domain-joined-run-hive.md) (Hive-beleid configureren voor aan een domein gekoppelde HDInsight-clusters) om Hive-beleid te configureren en Hive-query's uit te voeren.
* Zie voor het gebruik van SSH verbinding maken met domein HDInsight-clusters [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Linux, Unix of OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

