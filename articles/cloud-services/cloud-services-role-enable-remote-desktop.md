---
title: Extern bureaublad in een Azure-Cloud-Service inschakelen | Microsoft Docs
description: Het configureren van uw azure-cloud-servicetoepassing voor het toestaan van extern bureaublad-verbindingen
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: d3110ee8-6526-4585-aba5-d0bc9a713e9b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 413e72e9a39fcde84f56bfc61a6bc72dbadf1c97
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Verbinding met extern bureaublad voor een rol in Azure Cloudservices inschakelen

> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [Klassieke Azure Portal](cloud-services-role-enable-remote-desktop.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)

U kunt een verbinding met extern bureaublad in uw rol tijdens het ontwikkelen van inschakelen door de extern bureaublad-modules in de servicedefinitie van de of u kunt Extern bureaublad via de extern bureaublad-uitbreiding inschakelen. De gewenste aanpak is het gebruik van de extern bureaublad-extensie, zoals u extern bureaublad inschakelen kunt, zelfs nadat de toepassing zonder opnieuw te implementeren van uw toepassing wordt geïmplementeerd.

## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a>Extern bureaublad configureren via de klassieke Azure portal
De klassieke Azure portal maakt gebruik van de aanpak van extern bureaublad-extensie zodat u extern bureaublad inschakelen kunt, zelfs nadat de toepassing wordt geïmplementeerd. De **configureren** voor uw cloudservice op de pagina kunt u extern bureaublad inschakelen door het lokale Administrator-account gebruikt om verbinding met de virtuele machines te wijzigen, het certificaat in verificatie gebruikt en de verloopdatum instellen.

1. Klik op **Cloudservices**, klik op de naam van de cloudservice en klik vervolgens op **configureren**.
2. Klik op de **externe** knop onderaan.

    ![Externe cloudservices](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)

   > [!WARNING]
   > Alle exemplaren van de functie wordt opnieuw gestart wanneer u eerst Extern bureaublad inschakelen en klik op OK (vinkje). Als u wilt voorkomen dat de computer opnieuw is opgestart, moet het certificaat dat wordt gebruikt voor het versleutelen van het wachtwoord worden geïnstalleerd op de rol. Om te voorkomen dat een herstart [upload een certificaat voor de cloudservice](cloud-services-configure-ssl-certificate.md#step-3-upload-a-certificate) en keer vervolgens terug naar dit dialoogvenster.

3. In **rollen**, selecteer de rol die u wilt bijwerken of selecteer **alle** voor alle functies.
4. Breng een van de volgende wijzigingen:

   * Als u extern bureaublad, schakelt de **extern bureaublad inschakelen** selectievakje. Schakel het selectievakje voor het uitschakelen van extern bureaublad.
   * Maak een account te gebruiken in extern bureaublad-verbindingen met de rolinstanties.
   * Werk het wachtwoord voor het bestaande account.
   * Een geüploade certificaat wilt gebruiken voor verificatie selecteren (uploaden van het certificaat met behulp van **uploaden** op de **certificaten** pagina) of een nieuw certificaat maken.
   * Wijzig de vervaldatum voor de configuratie van extern bureaublad.

5. Wanneer u klaar bent met de updates voor uw configuratie, klikt u op **OK** (vinkje).

## <a name="remote-into-role-instances"></a>De afstand in rolinstanties
Zodra de extern bureaublad is ingeschakeld op de rollen kunt u extern in een rolinstantie via verschillende hulpprogramma's.

Verbinding maken met een rolinstantie vanuit de klassieke Azure portal:

1. Klik op **exemplaren** openen de **exemplaren** pagina.
2. Selecteer een rolexemplaar met extern bureaublad die zijn geconfigureerd.
3. Klik op **Connect**, en volg de instructies voor het openen van het bureaublad.
4. Klik op **Open** en vervolgens **Connect** starten van de extern bureaublad-verbinding.

### <a name="use-visual-studio-to-remote-into-a-role-instance"></a>Gebruik Visual Studio op afstand verbinding met een rolinstantie
In Visual Studio Server Explorer:

1. Vouw de **Azure** > **Cloudservices** > **[cloudservicenaam]** knooppunt.
2. Vouw ofwel **fasering** of **productie**.
3. Vouw de afzonderlijke functie.
4. Met de rechtermuisknop op een van de rolinstanties, klikt u op **verbinding maken met behulp van extern bureaublad...** , en voer vervolgens de gebruikersnaam en wachtwoord.

![Server explorer extern bureaublad](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)

### <a name="use-powershell-to-get-the-rdp-file"></a>PowerShell gebruiken voor het ophalen van het RDP-bestand
U kunt de [Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet voor het ophalen van het RDP-bestand. U kunt het RDP-bestand vervolgens verbinding met extern bureaublad gebruiken voor toegang tot de cloudservice.

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a>Programmatisch download het RDP-bestand met de Service Management REST API
U kunt de [RDP-bestand downloaden](https://msdn.microsoft.com/library/jj157183.aspx) REST-bewerking voor het downloaden van het RDP-bestand.

## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a>Extern bureaublad configureren in het servicedefinitiebestand
Deze methode kunt u extern bureaublad inschakelen voor de toepassing tijdens de ontwikkeling. Deze methode moet de versleutelde wachtwoorden worden opgeslagen in de serviceconfiguratie bestands- en updates van de configuratie van extern bureaublad vereist een nieuwe installatie van de toepassing. Als u wilt voorkomen dat deze de nadelen moet u de extern bureaublad-extensie op basis van aanpak die hierboven worden beschreven.  

U kunt Visual Studio om [een verbinding met extern bureaublad inschakelen](../vs-azure-tools-remote-desktop-roles.md) met de service definitie bestand benadering.  
De volgende stappen beschrijven de wijzigingen nodig voor de service model-bestanden op de extern bureaublad inschakelen. Visual Studio wordt automatisch deze wijzigingen aanbrengt bij het publiceren van.

### <a name="set-up-the-connection-in-the-service-model"></a>De verbinding in het servicemodel instellen
Gebruik de **invoer** element voor het importeren van de **RemoteAccess** module en de **RemoteForwarder** module die u wilt de [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) het bestand.

Het servicedefinitiebestand moet er ongeveer als het volgende voorbeeld met de `<Imports>` element toegevoegd.

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
De [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) bestand moet vergelijkbaar met het volgende voorbeeld worden uitgevoerd, wordt de `<ConfigurationSettings>` en `<Certificates>` elementen. Het opgegeven certificaat moet [geüpload naar de cloudservice](cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>Aanvullende resources
[Cloud-Services configureren hoe](cloud-services-how-to-configure.md)
[extern bureaublad-services FAQ - Cloud](cloud-services-faq.md)
