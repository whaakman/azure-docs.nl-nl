---
title: Maken en configureren van Enterprise-beveiligingspakket clusters in Azure HDInsight
description: Meer informatie over het maken en configureren van Enterprise-beveiligingspakket clusters in Azure HDInsight
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: howto
ms.date: 05/09/2019
ms.openlocfilehash: e9cb9a902cf60fbd3b297a72a7dfa836ee18c835
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67484585"
---
# <a name="create-and-configure-enterprise-security-package-clusters-in-azure-hdinsight"></a>Maken en configureren van Enterprise-beveiligingspakket clusters in Azure HDInsight

De Enterprise-beveiligingspakket voor Azure HDInsight hebt u toegang tot Active Directory gebaseerde verificatie, ondersteuning voor meerdere gebruikers en rollen gebaseerd toegangsbeheer voor uw Apache Hadoop-clusters in Azure. ESP HDInsight-clusters kunnen organisaties, die aan strikte zakelijke beveiligingsbeleid voldoen, voor het verwerken van gevoelige gegevens veilig.

Het doel van deze handleiding is ingeschakeld om de benodigde bronnen correct wordt geconfigureerd zodat de on-premises gebruikers kunnen zich aanmelden bij een ESP HDInsight-cluster. In dit artikel worden de stappen die nodig zijn voor het maken van een Azure HDInsight-Cluster van Enterprise-beveiligingspakket is ingeschakeld. De stappen wordt uitgelegd hoe een Windows IaaS-VM maken met Active Directory & Services DNS (Domain Name) ingeschakeld. Deze server fungeert als vervanging voor uw **werkelijke** on-premises omgeving en kunt u Volg de stappen in de setup- en configuratiestappen zodat u ze later in uw eigen omgeving kunt herhalen. Deze handleiding helpt u bij het maken van een hybride identiteit-omgeving met behulp van synchronisatie van wachtwoordhashes met Azure Active Directory ook.

Deze handleiding is bedoeld als aanvulling op [gebruik Enterprise-beveiligingspakket in HDInsight](apache-domain-joined-architecture.md)

Voordat u dit proces gebruikt in uw eigen omgeving, instellen Active Directory en Services DNS (Domain Name). Schakel ook Azure Active Directory en on-premises-sync-gebruikersaccounts met Azure Active Directory.

![Architectuurdiagram](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image002.png)

## <a name="create-on-premises-environment"></a>On-premises omgeving maken

Overzicht: In deze sectie maakt u een snelle implementatie van Azure-sjabloon gebruikt voor het maken van nieuwe virtuele machines, Services DNS (Domain Name) en een nieuw AD-Forest configureren.

1. Ga naar [een Azure-VM maken met een nieuw AD-Forest](https://azure.microsoft.com/resources/templates/active-directory-new-domain/)om de sjabloon voor de snelle implementatie weer te geven.

1. Klik op **implementeren in Azure**.
1. Aanmelden bij uw Azure-abonnement.
1. Op de **een Azure-VM maken met een nieuw AD-Forest** scherm, voer de volgende stappen uit:
    1. Selecteer het abonnement waar u de resources die zijn geïmplementeerd vanuit de **abonnement** vervolgkeuzelijst.
    1. Selecteer **nieuw** naast **resourcegroep** en voer de naam **OnPremADVRG**
    1. Voer de volgende details voor de rest van de sjabloonvelden:

        * **Locatie**: US - centraal
        * **Gebruikersnaam van beheerder**: HDIFabrikamAdmin
        * **Beheerderswachtwoord**: < YOUR_PASSWORD >
        * **Domein**: HDIFabrikam.com
        * **DNS-voorvoegsel**: hdifabrikam

        ![Sjabloon voor virtuele machines van Azure en AD-Forest maken](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-azure-vm-ad-forest.png)

    1. Klik op **aankoop**
    1. Controleer de implementatie en wacht totdat deze is voltooid.
    1. Controleer of de resources zijn gemaakt onder de juiste resourcegroep `OnPremADVRG`.

## <a name="configure-users-and-groups-for-cluster-access"></a>Gebruikers en groepen gebruiken voor toegang tot het cluster configureren

Overzicht: In deze sectie maakt u de gebruikers die toegang tot het HDInsight-cluster aan het einde van deze handleiding.

1. Verbinding maken met de domeincontroller met behulp van extern bureaublad.
    1. Als u de sjabloon die worden vermeld op het begin gebruikt, de domeincontroller is een virtuele machine met de naam **adVM** in de `OnPremADVRG` resourcegroep.
    1. Ga naar de Azure portal > **resourcegroepen** > **OnPremADVRG** > **adVM** > **Connect**.
    1. Klik op de **RDP** tabblad en klik vervolgens op **RDP-bestand downloaden**.
    1. Sla het bestand op uw computer en open het.
    1. Wanneer u hierom wordt gevraagd om referenties op te geven, gebruikt u `HDIFabrikam\HDIFabrikamAdmin` als de gebruikersnaam en voer het wachtwoord die u hebt gekozen voor het beheeraccount.

1. Zodra uw extern bureaublad-sessie wordt geopend op de virtuele machine van de domeincontroller, start u de **Active Directory: gebruikers en Computers** uit de **Serverbeheer** dashboard. Klik op **extra** in de rechterbovenhoek en vervolgens **Active Directory: gebruikers en Computers** in de vervolgkeuzelijst.

    ![Serverbeheer openen Active Directory-beheer](./media/apache-domain-joined-create-configure-enterprise-security-cluster/server-manager-active-directory-screen.png)

1. Maak twee nieuwe gebruikers, **HDIAdmin**, **HDIUser**. Deze twee gebruikers wordt gebruikt voor aanmelding bij HDInsight-clusters.

    1. In de **Active Directory: gebruikers en Computers** scherm, klikt u op **actie** > **nieuw** > **gebruiker**.

        ![Nieuwe Active Directory-gebruiker maken](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-new-user.png)

    1. In de **Nieuw Object: gebruiker** scherm, voert u `HDIUser` als de **aanmeldingsnaam van gebruiker** en klikt u op **volgende**.

        ![Eerste gebruiker met beheerdersrechten maken](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image020.png)

    1. Voer het gewenste wachtwoord voor het nieuwe account in het pop-upvenster dat wordt weergegeven. Schakel het selectievakje in met de melding dat **wachtwoord verloopt nooit**. HDIClick **OK**.
    1. Klik op **voltooien** om het nieuwe account te maken.
    1. Maken van een andere gebruiker `HDIAdmin`.

        ![Tweede gebruiker met beheerdersrechten maken](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image024.png)

1. In de **Active Directory: gebruikers en Computers** scherm, klikt u op **actie** > **nieuw** > **groep**. Maak `HDIUserGroup` als een nieuwe groep.

    ![Nieuwe Active Directory-groep maken](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-new-group.png)

    ![nieuwe group2 maken](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image028.png)

1. Voeg de **HDIUser** in de vorige stap hebt gemaakt de **HDIUserGroup** als een lid.

    1. Klik met de rechtermuisknop op de **HDIUserGroup** en klikt u op **eigenschappen**.
    1. Ga naar **leden** tabblad en klik op **toevoegen**.
    1. ENTER `HDIUser` in het vak **Geef de objectnamen op** en klikt u op **OK**.
    1. Herhaal de vorige stappen voor het andere account `HDIAdmin`

        ![leden toevoegen aan groep](./media/apache-domain-joined-create-configure-enterprise-security-cluster/active-directory-add-users-to-group.png)

U hebt nu uw Active Directory-omgeving, samen met twee gebruikers en een groep voor toegang tot het HDInsight-cluster gemaakt.

Deze gebruikers worden gesynchroniseerd met Azure AD.

### <a name="create-a-new-azure-active-directory"></a>Maak een nieuwe Azure Active Directory

1. Meld u aan bij Azure Portal.
1. Klik op **een resource maken** en het type **directory**. Selecteer **Azure Active Directory** > **maken**.
1. Voer **HDIFabrikam** onder **organisatienaam**.
1. Voer **HDIFabrikamoutlook** onder **initiële domeinnaam**.
1. Klik op **Create**.
1. Aan de linkerkant in Azure portal, klikt u op **Azure Active Directory**.
1. Klik indien nodig, op **schakelen tussen mappen** te wijzigen naar de nieuwe map die u hebt gemaakt **HDIFabrikamoutlook**.
1. Onder **beheren** klikt u op **aangepaste-domeinnamen** > **aangepast domein toevoegen**.
1. Voer **HDIFabrikam.com** onder **aangepaste domeinnaam** en klikt u op **domein toevoegen**.

![Maak een nieuwe azure active directory](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-new-directory.png)

![Maak een nieuw aangepast domein](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-custom-domain.png)

## <a name="configure-your-azure-ad-tenant"></a>Uw Azure AD-tenant configureren

Overzicht: Nu u uw Azure AD-tenant configureert zodat u, gebruikers en groepen van on-premises synchroniseren kunt AD naar de cloud.

1. Maak een AD-tenant-beheerder.
    1. Meld u aan bij Azure portal en selecteer uw Azure AD-tenant **HDIFabrikam**
    1. Selecteer **gebruikers** onder **beheren** en vervolgens **nieuwe gebruiker**.
    1. Voer de volgende details in voor de nieuwe gebruiker:

        * Naam: fabrikamazureadmin
        * Gebruikersnaam: fabrikamazureadmin@hdifabrikam.com
        * Wachtwoord: een beveiligd wachtwoord van uw keuze

    1. Klik op de **groepen** sectie, zoek **AAD DC Administrators**, en klikt u op **Selecteer**.

        ![Groepen](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image038.png)

    1. Klik op de **maprol** sectie en selecteer **hoofdbeheerder** in aan de rechterkant. Klik op **OK**.

        ![Directory-rol](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image040.png)

    1. Voer een wachtwoord voor de gebruiker. Klik op **Create**.

1. Als u wilt wijzigen van het wachtwoord voor de nieuwe gebruiker <fabrikamazureadmin@hdifabrikam.com>. Aanmelden bij Azure portal middels die de identiteits- en vervolgens u wordt gevraagd het wachtwoord te wijzigen.

## <a name="sync-on-premises-users-to-azure-ad"></a>Synchronisatie van on-premises gebruikers met Azure AD

### <a name="download-and-install-microsoft-azure-active-directory-connect"></a>Download en installeer Microsoft Azure Active Directory connect

1. [Azure AD Connect downloaden](https://www.microsoft.com/download/details.aspx?id=47594).

1. Installatie van Microsoft Azure Active Directory connect op de domeincontroller.
    1. Open het uitvoerbare bestand dat u hebt gedownload in de vorige stap en gaat akkoord met de licentievoorwaarden. Klik op **Doorgaan**.

        ![Azure AD Connect](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image052.png)

    1. Klik op **expresinstellingen gebruiken** en de installatie te voltooien.

        ![Expresinstellingen gebruiken](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image054.png)

### <a name="configure-sync-with-on-premises-domain-controller"></a>Synchronisatie met on-premises domeincontroller configureren

1. Op de **verbinding maken met Azure AD** scherm, voert u de gebruikersnaam en het wachtwoord van de globale beheerder voor Azure AD. Klik op **volgende**. Dit is de gebruikersnaam `fabrikamazureadmin@hdifabrikam.com` die u hebt gemaakt bij het configureren van uw AD-tenant.
    ![Verbinding maken met Azure AD](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image058.png)
1. Op de **verbinding maken met Active Directory Domain Services** scherm, voert u de gebruikersnaam en het wachtwoord voor een enterprisebeheerdersaccount. Klik op **volgende**. Dit is de gebruikersnaam `HDIFabrikam\HDIFabrikamAdmin` en het wachtwoord dat overeenkomt met die u eerder hebt gemaakt.

   ![Verbinding maken met Active Directory Domain Services](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image060.png)
1. Op de **configuratie van aanmelding bij Azure AD** pagina, klikt u op **volgende**.
    ![Azure AD-aanmelden-configuratie](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image062.png)
1. Op de gereed voor het configureren van scherm, klikt u op **installeren**.
    ![install](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image064.png)
1. Wanneer de **configuratie voltooid** scherm wordt weergegeven, klikt u op **afsluiten**.
    ![Configuratie is voltooid](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image078.png)

1. Controleer of als de gebruikers die u hebt gemaakt op de IAAS Active Directory worden gesynchroniseerd met Azure Active Directory nadat de synchronisatie voltooid is.
    1. Meld u aan bij Azure Portal.
    1. Selecteer **Azure Active Directory** > **HDIFabrikam** > **gebruikers**.

### <a name="create-an-user-assigned-managed-identity"></a>Maken van een gebruiker toegewezen beheerde identiteit

Maak een op gebruiker toegewezen beheerde identiteit die wordt gebruikt voor het configureren van Azure Active Directory Domain Services (Azure AD-DS). Zie voor meer informatie over het maken van een gebruiker toegewezen beheerde identiteit [maken, list, delete of wijs een rol aan een gebruiker toegewezen beheerde identiteit met Azure portal](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md).

1. Meld u aan bij Azure Portal.
1. Klik op **een resource maken** en het type **beheerde identiteit**. Selecteer **gebruiker toegewezen beheerde identiteit** > **maken**.
1. Voer **HDIFabrikamManagedIdentity** als de **resourcenaam**.
1. Selecteer uw abonnement.
1. Onder **resourcegroep** klikt u op **nieuw** en voer **HDIFabrikam CentralUS**.
1. Selecteer **VS-midden** onder **locatie**.
1. Klik op **Create**.

![Maak een nieuwe gebruiker toegewezen beheerde identiteit](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image082.png)

### <a name="enable-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services inschakelen

Zie voor meer informatie, [inschakelen Azure Active Directory Domain Services met behulp van de Azure-portal](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started).

1. Maak het Virtueelnetwerk naar Azure AD DS-host. Voer de volgende powershell-code.

    ```powershell
    Connect-AzAccount
    Get-AzSubscription
    Set-AzContext -Subscription 'SUBSCRIPTION_ID'
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS' -Location 'Central US' -Name 'HDIFabrikam-AADDSVNET' -AddressPrefix 10.1.0.0/16
    $subnetConfig = Add-AzVirtualNetworkSubnetConfig -Name 'AADDS-subnet' -AddressPrefix 10.1.0.0/24 -VirtualNetwork $virtualNetwork
    $virtualNetwork | Set-AzVirtualNetwork
    ```

1. Meld u aan bij Azure Portal.
1. Klik op **resource maken**, voer **domeinservices** en selecteer **Azure AD Domain Services**.
1. Op de **basisbeginselen** scherm de volgende stappen uitvoeren:
    1. Onder **mapnaam** selecteert u de Azure Active Directory gemaakt voor dit artikel **HDIFabrikam**.
    1. Voer een **DNS-domeinnaam** van **HDIFabrikam.com**.
    1. Selecteer uw abonnement.
    1. Geef de resourcegroep **HDIFabrikam CentralUS** en de **locatie** van **VS-midden**.

        ![Azure ad ds-basisgegevens](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image084.png)

1. Op de **netwerk** volledig scherm, selecteert u het netwerk (**HDIFabrikam VNET**) en het subnet (**AADDS subnet**) die u hebt gemaakt met de vorige powershell-script. Of u kunt de **nieuw** optie voor het maken van een virtueel netwerk nu.

    ![netwerk selecteren](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image086.png)

1. Op de **beheerdersgroep** scherm ziet u een melding dat een groep met de naam **AAD DC Administrators** is al gemaakt voor het beheren van deze groep. U kunt (optioneel) het lidmaatschap van deze groep wijzigt, maar dit is niet vereist voor de stappen van dit artikel. Klik op **OK**.

    ![weergave-beheerdersgroep](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image088.png)

1. Op de **synchronisatie** scherm, volledige synchronisatie inschakelen door te selecteren **alle** en klik vervolgens op **OK**.

    ![synchronisatie inschakelen](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image090.png)

1. Op de **samenvatting** scherm, Controleer de details voor de Azure AD DS- en op **Ok**.

    ![Controleer of de details](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image092.png)

1. Nadat u Azure AD DS-inschakelt, wordt een lokale Domain Name Service (DNS)-server wordt uitgevoerd op de AD-virtuele Machines (VM's).

### <a name="configure-your-azure-ad-ds-virtual-network"></a>Uw virtuele netwerk van Azure AD DS-configureren

De stappen in deze sectie helpt u bij het configureren van het virtuele netwerk van Azure AD DS-(**HDIFabrikam AADDSVNET**) gebruik van uw aangepaste DNS-servers.

1. Ga naar de IP-adressen van uw aangepaste DNS-servers. Klik op de **HDIFabrikam.com** AD DS-bron, klikt u op **eigenschappen** onder **beheren**   en bekijkt u de IP-adressen die worden vermeld onder **IP Adres op virtueel netwerk**.

    ![Aangepaste DNS-IP-adressen voor Azure AD DS-zoeken](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image096.png)

1. Configureer **HDIFabrikam AADDSVNET** naar aangepaste IP-adressen `10.0.0.4` en `10.0.0.5`.

    1. Selecteer **DNS-Servers** onder de **instellingen** categorie. Klik vervolgens op het keuzerondje bij **aangepaste**, voer het eerste IP-adres (10.0.0.4) in het onderstaande tekstvak en klikt u op **opslaan**.
    1. Voeg extra IP-adressen (10.0.0.5) met dezelfde stappen.

1. In ons scenario is Azure AD DS-geconfigureerd voor gebruik van IP-adressen 10.0.0.4 en 10.0.0.5, instellen van hetzelfde IP-adres op AADDS VNet als weergeven in de onderstaande afbeelding.

    ![aangepaste dns-servers bekijken](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image098.png)

## <a name="securing-ldap-traffic"></a>LDAP-verkeer beveiligen

Lightweight Directory Access Protocol (LDAP) wordt gebruikt om te lezen uit en schrijven naar Active Directory. Kunt u LDAP-verkeer vertrouwelijke en beveiligd met behulp van Secure Sockets Layer (SSL) / Transport Layer Security (TLS)-technologie. U kunt LDAP via SSL (LDAPS) inschakelen door het installeren van een certificaat juist opgemaakt.

Zie voor meer informatie over secure LDAP [configureren veilige LDAP (LDAPS) voor een Azure AD Domain Services beheerde domein](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap).

In deze sectie maakt u een zelfondertekend certificaat maken, het certificaat downloaden en configureren van secure LDAP (LDAPS) voor de **hdifabrikam** Azure AD DS-domein worden beheerd.

Het volgende script maakt u een certificaat voor hdifabrikam. Het certificaat wordt opgeslagen onder het pad 'LocalMachine /'.

> [!Note] 
> Een hulpprogramma of de toepassing die wordt gemaakt van een geldige PKCS \#10 aanvragen kan worden gebruikt om de SSL-certificaat-aanvraag.

```powershell
$lifetime = Get-Date
New-SelfSignedCertificate -Subject hdifabrikam.com `
-NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
-Type SSLServerAuthentication -DnsName *.hdifabrikam.com, hdifabrikam.com
```

Controleren of het certificaat is geïnstalleerd op de computer\'s persoonlijke archief. Voltooi de volgende stappen:

1. Start Microsoft Management Console (MMC).
1. Voeg de module Certificaten die certificaten op de lokale computer beheert.
1. Vouw **certificaten (lokale Computer)** , vouw **persoonlijke**, en vouw vervolgens **certificaten**. Een nieuw certificaat moet bestaan in het persoonlijke archief. Dit certificaat wordt uitgegeven aan de volledig gekwalificeerde hostnaam.

    ![Controleer of het maken van certificaten](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image102.png)

1. In het rechterdeelvenster met de rechtermuisknop op het certificaat dat u hebt gemaakt in de vorige stap, wijst u **alle taken**, en klik vervolgens op **exporteren**.

1. Op de **persoonlijke sleutel exporteren** pagina, klikt u op **Ja, de persoonlijke, sleutel exporteren**. De persoonlijke sleutel is vereist voor de versleutelde berichten te lezen van de computer waar de sleutel moet worden geïmporteerd.

    ![persoonlijke sleutel exporteren](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image103.png)

1. Op de **bestandsindeling voor Export** pagina, laat u de standaardinstellingen en klik vervolgens op **volgende**. 
1. Op de **wachtwoord** pagina, typt u een wachtwoord voor de persoonlijke sleutel, selecteer **TripleDES SHA1** voor **versleuteling** en klikt u op **volgende**.
1. Op de **te exporteren bestand** pagina, typ het pad en de naam van het geëxporteerde certificaatbestand en klik vervolgens op **volgende**.
1. Naam van het bestand heeft moet .pfx-extensie, wordt dit bestand is geconfigureerd in Azure portal om veilig verbinding te maken.
1. Veilige LDAP (LDAPS) voor een beheerd domein van Azure AD Domain Services inschakelen.
    1. Selecteer het domein **HDIFabrikam.com** vanuit Azure portal.
    1. Klik op **beveiligde LDAP** onder **beheren**.
    1. Op de **Secure LDAP** scherm, klikt u op **inschakelen** onder **Secure LDAP**.
    1. Blader naar het PFX-certificaatbestand dat u hebt geëxporteerd op uw computer.
    1. Voer het certificaatwachtwoord.

    ![secure ldap inschakelen](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image113.png)

1. Nu dat u beveiligde LDAP ingeschakeld, zorgt ervoor dat deze bereikbaar is door in te schakelen poort 636.
    1. Klik op de netwerkbeveiligingsgroep **AADDS-HDIFabrikam.com-NSG** in de **HDIFabrikam CentralUS** resourcegroep.
    1. Onder **instellingen** klikt u op **inkomende beveiligingsregels** > **toevoegen**.
    1. Op de **inkomende beveiligingsregel toevoegen** scherm, voer de volgende eigenschappen en klikt u op **toevoegen**:

        | Eigenschap | Value |
        |---|---|
        | Source | Any |
        | Source port ranges | * |
        | Destination | Any |
        | Destination port range | 636 |
        | Protocol | Any |
        | Action | Allow |
        | Priority | \<Desired Number> |
        | Name | Port_LDAP_636 |

    ![beveiligingsregel voor binnenkomend verkeer](./media/apache-domain-joined-create-configure-enterprise-security-cluster/add-inbound-security-rule.png)

1. `HDIFabrikamManagedIdentity` is de gebruiker toegewezen beheerde identiteit, de rol Inzender voor HDInsight Domain Services is ingeschakeld voor de beheerde identiteit waarmee deze identiteit om te lezen, maken, wijzigen en verwijderen van bewerkingen voor domain services.

    ![Door gebruiker toegewezen beheerde identiteit maken](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image117.png)

## <a name="creating-enterprise-security-package-enabled-hdinsight-cluster"></a>Het maken van Enterprise-beveiligingspakket HDInsight-cluster ingeschakeld

Deze stap moet u de volgende vereisten:

1. Maak een nieuwe resourcegroep `HDIFabrikam-WestUS` op de locatie `West US`.
1. Maak een virtueel netwerk die als voor ESP host HDInsight-cluster ingeschakeld.

    ```powershell
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS' -Location 'West US' -Name 'HDIFabrikam-HDIVNet' -AddressPrefix 10.1.0.0/16
    $subnetConfig = Add-AzVirtualNetworkSubnetConfig -Name 'SparkSubnet' -AddressPrefix 10.1.0.0/24 -VirtualNetwork $virtualNetwork
    $virtualNetwork | Set-AzVirtualNetwork
    ```

1. Maken van een peer-relatie tussen het Virtueelnetwerk dat als host voor AADDS fungeert (`HDIFabrikam-AADDSVNET`) en het Virtueelnetwerk die als voor de ESP host HDInsight-cluster ingeschakeld (`HDIFabrikam-HDIVNet`). Gebruik de volgende powershell-code voor deze twee virtuele netwerken.

    ```powershell
    Add-AzVirtualNetworkPeering -Name 'HDIVNet-AADDSVNet' -RemoteVirtualNetworkId (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS').Id -VirtualNetwork (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS')

    Add-AzVirtualNetworkPeering -Name 'AADDSVNet-HDIVNet' -RemoteVirtualNetworkId (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS').Id -VirtualNetwork (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS')
    ```

1. Maak een nieuw account van de Azure Data Lake Storage Gen2 **Hdigen2store**, dat wil zeggen geconfigureerd met de identiteit van de gebruiker beheerde **HDIFabrikamManagedIdentity**. Zie voor meer informatie over het maken van Data Lake Storage Gen2 accounts ingeschakeld met de gebruiker beheerde identiteiten [gebruik Azure Data Lake Storage Gen2 met Azure HDInsight-clusters](../hdinsight-hadoop-use-data-lake-storage-gen2.md).

1. Aangepaste DNS instellen op de **HDIFabrikam AADDSVNET** virtueel netwerk.
    1. Ga naar de Azure portal > **resourcegroepen** > **OnPremADVRG** > **HDIFabrikam AADDSVNET**  >   **DNS-servers**.
    1. Selecteer **aangepaste** en voer `10.0.0.4` en `10.0.0.5`.
    1. Klik op **Opslaan**.

        ![aangepaste dns-instellingen opslaan](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image123.png)

1. Maak een nieuwe ESP ingeschakeld HDInsight Spark-cluster.
    1. Klik op **aangepast (grootte, instellingen en apps)** .
    2. Voer de gewenste gegevens voor de sectie 1 **basisbeginselen**. Zorg ervoor dat de **clustertype** is **Spark 2.3 (HDI 3.6)** en de **resourcegroep** is **HDIFabrikam CentralUS**

    1. Sectie 2 **beveiliging en netwerken**, voer de volgende stappen uit:
        1. Klik op **ingeschakeld** onder **Enterprise-beveiligingspakket**.
        1. Klik op **Cluster de gebruiker met beheerdersrechten** en selecteer de **HDIAdmin** account dat u eerder hebt gemaakt als de gebruiker met lokale beheerdersrechten. Klik op **Selecteren**.

        1. Klik op **toegang clustergroep** en selecteer vervolgens **HDIUserGroup**. Elke gebruiker die u aan deze groep in de toekomst toevoegt toegang kunnen krijgen tot HDInsight-clusters worden opgegeven.

            ![groep voor toegang tot cluster selecteren](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image129.jpg)

    1. De andere stappen van de configuratie van het cluster uitvoeren en controleer of de details op de **samenvatting voor Cluster**. Klik op **Create**.

1. Aanmelden bij de Ambari UI voor het nieuwe cluster op `https://CLUSTERNAME.azurehdinsight.net` met behulp van de gebruikersnaam van beheerder `hdiadmin@hdifabrikam.com` en het wachtwoord.

    ![aanmelden bij de Ambari](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image135.jpg)

1. Klik op **rollen** uit het clusterdashboard.
1. Op de **rollen** pagina, de groep invoeren **hdiusergroup** toe te wijzen aan de **Clusterbeheerder** rol onder **rollen toewijzen aan deze**.

    ![cluster-beheerdersrol toewijzen aan hdiusergroup](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image137.jpg)

1. Open uw SSH-client en meld u aan bij het cluster met behulp van de **hdiuser** die u eerder hebt gemaakt in de on-premises Active Directory.

    ![Meld u aan bij het cluster met SSH](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image139.jpg)

Als u zich kunt aanmelden met dit account, klikt u vervolgens u uw cluster hebt geconfigureerd ESP goed om te synchroniseren met uw on-premises active directory.

## <a name="next-steps"></a>Volgende stappen

* [Een inleiding tot Apache Hadoop-beveiliging met Enterprise Security Package](apache-domain-joined-introduction.md)
