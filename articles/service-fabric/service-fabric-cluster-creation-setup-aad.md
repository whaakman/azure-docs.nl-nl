---
title: Azure Active Directory instellen voor verificatie van Service Fabric | Microsoft Docs
description: Meer informatie over het instellen van Azure Active Directory (Azure AD) om clients voor Service Fabric-clusters te verifiëren.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/15/2018
ms.author: aljo
ms.openlocfilehash: aa470ca69b30733a9b41b03a8accadfa8f674ef0
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "40180831"
---
# <a name="set-up-azure-active-directory-for-client-authentication"></a>Azure Active Directory instellen voor clientverificatie

Voor clusters die worden uitgevoerd op Azure, Azure Active Directory (Azure AD) aanbevolen voor beveiligde toegang tot eindpunten voor beheer.  Dit artikel wordt beschreven hoe tot het instellen van Azure AD om clients voor een Service Fabric-cluster te verifiëren, die moet worden uitgevoerd voordat [het maken van het cluster](service-fabric-cluster-creation-via-arm.md).  Azure AD kan organisaties (bekend als tenants) voor het beheren van toegang tot toepassingen. Toepassingen worden onderverdeeld in die een webgebaseerde UI voor aanmelden en die met een systeemeigen client-ervaring. In dit artikel nemen we aan dat u al een tenant hebt gemaakt. Als u niet hebt gedaan, starten door te lezen [hoe u een Azure Active Directory-tenant verkrijgen][active-directory-howto-tenant].

## <a name="create-azure-ad-applications"></a>Azure AD-toepassingen maken
Een Service Fabric-cluster biedt verschillende toegangspunten bij de management-functionaliteit, met inbegrip van de webconsole [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] en [Visual Studio] [ service-fabric-manage-application-in-visual-studio]. Als gevolg hiervan, het maken van twee Azure AD-toepassingen voor het beheren van toegang tot het cluster: een webtoepassing en een systeemeigen toepassing.  Nadat de toepassingen die zijn gemaakt, u gebruikers toewijzen aan de alleen-lezen en beheerdersrollen.

Ter vereenvoudiging van enkele van de stappen voor het configureren van Azure AD met een Service Fabric-cluster, hebben we een set Windows PowerShell-scripts gemaakt.

> [!NOTE]
> U moet de volgende stappen uitvoeren voordat u het cluster maakt. Omdat de scripts verwacht clusternamen en eindpunten, worden de waarden moeten worden gepland en niet de waarden die u al hebt gemaakt.

1. [De scripts downloaden] [ sf-aad-ps-script-download] op uw computer.
2. Met de rechtermuisknop op het zip-bestand, selecteer **eigenschappen**, selecteer de **opheffen van blokkeringen** selectievakje en klik vervolgens op **toepassen**.
3. Pak het gecomprimeerde bestand uit.
4. Voer `SetupApplications.ps1`, en geef de tenant-id, de clusternaam en de WebApplicationReplyUrl als parameters. Bijvoorbeeld:

```PowerShell
.\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
```

> [!NOTE]
> Voor nationale clouds (Azure Government, China, Azure, Azure Duitsland), moet u ook opgeven de `-Location` parameter.

U kunt uw tenant-id vinden door het uitvoeren van de PowerShell-opdracht `Get-AzureSubscription`. Deze opdracht wordt uitgevoerd, wordt de tenant-id voor elk abonnement weergegeven.

Clusternaam is gebruikt voor het toevoegen van de Azure AD-toepassingen die zijn gemaakt door het script. Het hoeft niet precies overeenkomen met de naam van het daadwerkelijke cluster. Het is uitsluitend bedoeld om het Azure AD-artefacten worden toegewezen aan de Service Fabric-cluster dat ze worden gebruikt met eenvoudiger.

WebApplicationReplyUrl is de standaardeindpunt dat Azure AD aan uw gebruikers retourneert nadat ze klaar zijn aangemeld. Dit eindpunt als de Service Fabric Explorer-eindpunt voor uw cluster, dit is standaard ingesteld:

https://&lt;cluster_domain&gt;: 19080/Explorer

U wordt gevraagd of u zich aanmeldt bij een account dat beheerdersrechten voor de Azure AD-tenant heeft. Nadat u zich hebt aangemeld, wordt het script maakt de web- en systeemeigen toepassingen voor uw Service Fabric-cluster. Als u kijkt naar de toepassingen van de tenant in de [Azure-portal][azure-portal], ziet u twee nieuwe vermeldingen:

   * *Clusternaam*\_Cluster
   * *Clusternaam*\_Client

Het script af te drukken de JSON die wordt vereist door de Azure Resource Manager-sjabloon wanneer u het cluster in de volgende sectie, maakt dus is het een goed idee om Houd het PowerShell-venster geopend.

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

<a name="assign-roles"></a>

## <a name="assign-users-to-roles"></a>Gebruikers aan rollen toewijzen
Nadat u de toepassingen voor uw cluster hebt gemaakt, uw gebruikers toewijzen aan de rollen die worden ondersteund door Service Fabric: alleen-lezen en -beheerder. U kunt de rollen toewijzen met behulp van de [Azure-portal][azure-portal].

1. In de Azure-portal, selecteert u uw tenant in de rechterbovenhoek.

    ![Selecteer de knop tenant][select-tenant-button]
2. Selecteer **Azure Active Directory** op de tab linksboven en selecteer 'Enterprise Application'.
3. 'Alle Application' en selecteer vervolgens zoeken en selecteer de web-App heeft een naam, zoals `myTestCluster_Cluster`.
4. Klik op de **gebruikers en groepen** tabblad.

    ![Tabblad gebruikers en groepen][users-and-groups-tab]
5. Klik op de **gebruiker toevoegen** knop op de nieuwe pagina, selecteert u een gebruiker en de rol wilt toewijzen en klik op de **Selecteer** knop aan de onderkant van de pagina.

    ![Gebruikers toewijzen aan de pagina serverfuncties][assign-users-to-roles-page]
6. Klik op de **toewijzen** knop aan de onderkant van de pagina.

    ![Bevestiging van toewijzing toevoegen][assign-users-to-roles-confirm]

> [!NOTE]
> Zie voor meer informatie over functies in Service Fabric, [rollen gebaseerd toegangsbeheer voor Service Fabric-clients](service-fabric-cluster-security-roles.md).
>
>

## <a name="troubleshooting-help-in-setting-up-azure-active-directory"></a>Problemen bij het instellen van Azure Active Directory
Instellen van Azure AD en het gebruik van het kunnen lastig zijn, zodat hier een aantal tips zijn over wat u doen kunt om op te sporen van het probleem.

### <a name="service-fabric-explorer-prompts-you-to-select-a-certificate"></a>Service Fabric Explorer vraagt u om een certificaat te selecteren
#### <a name="problem"></a>Probleem
Nadat u zich hebt aangemeld met succes aan Azure AD in Service Fabric Explorer, geeft de browser naar de startpagina, maar een bericht u vraagt om een certificaat selecteren.

![Dialoogvenster SFX-certificaat][sfx-select-certificate-dialog]

#### <a name="reason"></a>Reden
De gebruiker is niet een rol in de Azure AD-toepassing voor cluster toegewezen. Azure AD-verificatie mislukt dus op Service Fabric-cluster. Service Fabric Explorer gebruikgemaakt van verificatie via certificaat.

#### <a name="solution"></a>Oplossing
Volg de instructies voor het instellen van Azure AD en gebruikersrollen toewijzen. Ook aangeraden u inschakelen 'Gebruikerstoewijzing vereist voor toegang tot app,' als `SetupApplications.ps1` heeft.

### <a name="connection-with-powershell-fails-with-an-error-the-specified-credentials-are-invalid"></a>Verbinding met PowerShell mislukt met fout: "de opgegeven referenties zijn ongeldig."
#### <a name="problem"></a>Probleem
Als u PowerShell gebruiken om verbinding met het cluster met behulp van de beveiligingsmodus "AzureActiveDirectory", nadat u zich hebt aangemeld met succes aan Azure AD, mislukt de verbinding met een fout: "de opgegeven referenties zijn ongeldig."

#### <a name="solution"></a>Oplossing
Deze oplossing is hetzelfde als de vorige waarde.

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a>Service Fabric Explorer retourneert een fout opgetreden tijdens het aanmelden: "AADSTS50011"
#### <a name="problem"></a>Probleem
Wanneer u zich aanmeldt bij Azure AD in Service Fabric Explorer, de pagina een fout geretourneerd: ' AADSTS50011: het antwoordadres op dat &lt;url&gt; komt niet overeen met de antwoordadressen die is geconfigureerd voor de toepassing: &lt;guid&gt;. "

![Antwoordadres op dat u SFX komt niet overeen met][sfx-reply-address-not-match]

#### <a name="reason"></a>Reden
Het (web)-clustertoepassing met Service Fabric Explorer probeert te verifiëren bij Azure AD en biedt als onderdeel van de aanvraag de geretourneerde Omleidings-URL. Maar de URL is niet opgenomen in de Azure AD-toepassing **antwoord-URL** lijst.

#### <a name="solution"></a>Oplossing
Selecteer 'App-registraties' in AAD-pagina, selecteert u uw toepassing voor het cluster en selecteer vervolgens de **antwoord-URL's** knop. Op de pagina 'Antwoord-URL's ', de URL van de Service Fabric Explorer toevoegen aan de lijst of een van de items in de lijst met vervangen. Wanneer u klaar bent, sla de wijziging.

![Web application antwoord-url][web-application-reply-url]

### <a name="connect-the-cluster-by-using-azure-ad-authentication-via-powershell"></a>Verbinding maken met het cluster met behulp van Azure AD-verificatie via PowerShell
Als u wilt verbinding maken met de Service Fabric-cluster, gebruikt u het volgende voorbeeld van de PowerShell-opdracht:

```PowerShell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

Zie voor meer informatie, [cmdlet Connect-ServiceFabricCluster](https://docs.microsoft.com/powershell/module/servicefabric/connect-servicefabriccluster).

### <a name="can-i-reuse-the-same-azure-ad-tenant-in-multiple-clusters"></a>Kan ik dezelfde Azure AD-tenant in meerdere clusters gebruiken?
Ja. Maar vergeet niet de URL van de Service Fabric Explorer toevoegen aan uw (web)-clustertoepassing. Anders, Service Fabric Explorer werkt niet.

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a>Waarom kan ik nog steeds moet een certificaat voor Azure AD is ingeschakeld?
FabricClient en FabricGateway uit te voeren een wederzijdse verificatie. Tijdens de verificatie van Azure AD, Azure AD-integratie zorgt voor de identiteit van een client naar de server en het servercertificaat wordt gebruikt om te controleren of de identiteit van de server. Zie voor meer informatie over Service Fabric certificaten [X.509-certificaten en Service Fabric][x509-certificates-and-service-fabric].

## <a name="next-steps"></a>Volgende stappen
Na het instellen van Azure Active Directory-toepassingen en functies van de instelling voor gebruikers, [configureren en implementeren van een cluster](service-fabric-cluster-creation-via-arm.md).


<!-- Links -->
[azure-CLI]:https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest
[azure-portal]: https://portal.azure.com/
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]:../active-directory/develop/quickstart-create-new-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[select-tenant-button]: ./media/service-fabric-cluster-creation-via-arm/select-tenant-button.png
[users-and-groups-tab]: ./media/service-fabric-cluster-creation-via-arm/users-and-groups-tab.png
[assign-users-to-roles-page]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-page.png
[assign-users-to-roles-confirm]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-confirm.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png
