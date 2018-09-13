---
title: App Service implementeren in een offline-omgeving in Azure Stack | Microsoft Docs
description: Gedetailleerde instructies over het App Service implementeren in een niet-verbonden Azure Stack-omgeving beveiligd door AD FS.
services: azure-stack
documentationcenter: ''
author: apwestgarth
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2018
ms.author: anwestg
ms.openlocfilehash: 4b6ad2e237d73d5d8ca414bffc3a679ee552105f
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44718726"
---
# <a name="add-an-app-service-resource-provider-to-a-disconnected-azure-stack-environment-secured-by-ad-fs"></a>Een App Service-resourceprovider toevoegen aan een niet-verbonden Azure Stack-omgeving beveiligd door AD FS

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

> [!IMPORTANT]
> De update 1807 toepassen op uw geïntegreerde Azure Stack-systeem of de meest recente Azure Stack development kit implementeren voordat u Azure App Service 1.3 implementeert.
>
>

Door de instructies in dit artikel te volgen, kunt u de [App Service-resourceprovider](azure-stack-app-service-overview.md) in een Azure Stack-omgeving die is:

- niet verbonden met Internet
- beveiligd door Active Directory Federation Services (AD FS).

De App Service-resourceprovider toevoegen aan uw offline Azure Stack-implementatie, moet u deze op het hoogste niveau taken uitvoeren:

1. Voltooi de [vereiste stappen](azure-stack-app-service-before-you-get-started.md) (zoals de aanschaf van certificaten, die kan een paar dagen duren voor het ontvangen van).
2. [Downloaden en uitpakken van de installatie en Help-bestanden](azure-stack-app-service-before-you-get-started.md) op een computer die is verbonden met Internet.
3. Een offline-installatiepakket maken.
4. Voer het installatiebestand appservice.exe.

## <a name="create-an-offline-installation-package"></a>Een offline-installatiepakket maken

Voor het implementeren van App Service in een omgeving zonder verbinding, moet u eerst een offline-installatiepakket maken op een computer die verbonden met Internet.

1. Voer het installatieprogramma AppService.exe op een computer die verbonden met Internet.

2. Klik op **Geavanceerd** > **offline-installatiepakket maken**.

    ![App Service-installatieprogramma][1]

3. Het App Service-installatieprogramma maakt een offline-installatiepakket en het pad naar deze wordt weergegeven. U kunt klikken op **map openen** de map in de Verkenner te openen.

    ![App Service-installatieprogramma](media/azure-stack-app-service-deploy-offline/image02.png)

4. Kopieer het installatieprogramma (AppService.exe) en de offline-installatiepakket naar uw Azure Stack-hostmachine.

## <a name="complete-the-offline-installation-of-app-service-on-azure-stack"></a>Voltooi de offline-installatie van App Service in Azure Stack

1. Appservice.exe als beheerder uitvoeren vanaf een computer die de Azure Stack Admin Azure Resource Manager-eindpunt kan bereiken.

2. Klik op **Geavanceerd** > **offline-installatie voltooien**.

    ![App Service-installatieprogramma][2]

3. Blader naar de locatie van het offline-installatie-pakket dat u eerder hebt gemaakt en klik vervolgens op **volgende**.

    ![App Service-installatieprogramma](media/azure-stack-app-service-deploy-offline/image04.png)

4. Controleer en accepteer de licentievoorwaarden voor Microsoft-Software en klik vervolgens op **volgende**.

5. Controleer en accepteer de licentievoorwaarden van derden en klik vervolgens op **volgende**.

6. Zorg ervoor dat de App Service-cloud-configuratie-informatie juist is. Als u de standaardinstellingen tijdens de implementatie van Azure Stack Development Kit gebruikt, kunt u hier de standaardwaarden accepteren. Als u de opties aangepast wanneer u Azure Stack geïmplementeerd of op een geïntegreerd systeem implementeert, moet u de waarden in dit venster om weer te geven die bewerken. Als u de domein-achtervoegsel mycloud.com gebruikt, kunt u voor uw Azure Stack Tenant Azure Resource Manager-eindpunt, moet wijzigen voor beheer. <region>. mycloud.com. Nadat u uw gegevens bevestigen, klikt u op **volgende**.

    ![App Service-installatieprogramma][3]

7. Op de volgende pagina:
    1. Klik op de **Connect** naast de **Azure Stack-abonnementen** vak.
        - Geef uw beheerdersaccount. Bijvoorbeeld cloudadmin@azurestack.local. Voer uw wachtwoord in en klikt u op **aanmelden**.
    2. In de **Azure Stack-abonnementen** Schakel de **Providerabonnement standaard**.
    
    > [!NOTE]
    > App Service kan alleen worden geïmplementeerd in de **Providerabonnement standaard** op dit moment.  App Service wordt in een toekomstige update implementeren in het nieuwe abonnement meten die is geïntroduceerd in Azure Stack 1804 en alle bestaande implementaties worden gemigreerd naar het nieuwe abonnement ook.
    >
    >
    
    3. In de **Azure Stack-locaties** vak, selecteert u de locatie die overeenkomt met de regio die u naar implementeert. Selecteer bijvoorbeeld **lokale** als uw implementatie van de Azure Stack Development Kit.
    4. Klik op **Volgende**.

    ![App Service-installatieprogramma][4]

8. U hebt nu de mogelijkheid om te implementeren in een bestaand Virtueelnetwerk, zoals ingesteld door de stappen [hier](azure-stack-app-service-before-you-get-started.md#virtual-network), of het installatieprogramma van de App Service te maken van een Virtueelnetwerk en gekoppelde subnetten toe te staan.
    1. Selecteer **VNet maken met standaardinstellingen**, accepteer de standaardwaarden en klik vervolgens op **volgende**, of;
    2. Selecteer **bestaande VNet en subnetten gebruiken**.
        1. Selecteer de **resourcegroep** waarin het Virtueelnetwerk.
        2. Kies de juiste **Virtueelnetwerk** naam die u implementeren wilt in;
        3. Selecteer de juiste **Subnet** waarden voor elk van de subnetten van de vereiste rol;
        4. Klik op **Volgende**

    ![App Service-installatieprogramma][5]

9. Voer de gegevens voor de bestandsshare en klik vervolgens op **volgende**. Het adres van de bestandsshare moet gebruiken de FQDN-naam of IP-adres van de bestandsserver. Bijvoorbeeld, \\\appservicefileserver.local.cloudapp.azurestack.external\websites, of \\\10.0.0.1\websites

    > [!NOTE]
    > Het installatieprogramma wil test de verbinding met de bestandsshare voordat u doorgaat.  Als u hebt gekozen om te implementeren in een bestaand Virtueelnetwerk, het installatieprogramma mogelijk geen verbinding maken met de bestandsshare en een waarschuwing weergegeven waarin wordt gevraagd of u wilt doorgaan.  Controleer of de bestandsshare-informatie en blijven als ze juist zijn.
    >
    >

   ![App Service-installatieprogramma][8]

10. Op de volgende pagina:
    1. In de **Identiteitstoepassings-ID** voert u de GUID voor de toepassing die u voor de identiteit (van Azure AD).
    2. In de **Identiteitstoepassing certificaatbestand** vak, typ (of blader naar) de locatie van het certificaatbestand.
    3. In de **Identiteitstoepassing certificaatwachtwoord** vak, voer het wachtwoord voor het certificaat. Dit wachtwoord is degene die u hebt genoteerd als u het script gebruikt voor het maken van de certificaten.
    4. In de **basiscertificaatbestand voor Azure Resource Manager** vak, typ (of blader naar) de locatie van het certificaatbestand.
    5. Klik op **Volgende**.

    ![App Service-installatieprogramma][10]

11. Voor elk van de drie vakken bestand van het certificaat, klikt u op **Bladeren** en navigeer vervolgens naar het juiste certificaatbestand. U moet het wachtwoord opgeven voor elk certificaat. Deze certificaten zijn degene die u hebt gemaakt in de [maken vereiste certificaten stap](azure-stack-app-service-before-you-get-started.md#get-certificates). Klik op **volgende** na het invoeren van alle informatie.

    | Box | Voorbeeld van de naam van certificaat |
    | --- | --- |
    | **Standaard SSL-certificaatbestand van App Service** | \_.appservice.local.AzureStack.external.pfx |
    | **SSL-certificaatbestand voor App Service API** | api.appservice.local.AzureStack.external.pfx |
    | **App Service Publisher SSL-certificaatbestand** | ftp.appservice.local.AzureStack.external.pfx |

    Als u een andere domeinachtervoegsel gebruikt tijdens het maken van de certificaten, de bestandsnamen van uw certificaat niet gebruikt *lokale. AzureStack.external*. In plaats daarvan gebruik de informatie van uw aangepaste domein.

    ![App Service-installatieprogramma][11]

12. Geef de SQL Server-details voor de server-exemplaar dat is gebruikt voor het hosten van de App Service resource provider-databases, en klik vervolgens op **volgende**. Het installatieprogramma evalueert de eigenschappen van de SQL-verbinding. U **moet** voert u de interne IP- of de FQDN-naam voor de naam van de SQL Server.

    > [!NOTE]
    > Het installatieprogramma wil test de verbinding met de SQl-Server voordat u doorgaat.  Als u hebt gekozen om te implementeren in een bestaand Virtueelnetwerk, het installatieprogramma mogelijk geen verbinding maken met de SQL-Server en geeft een waarschuwing waarin wordt gevraagd of u wilt doorgaan.  Controleer of de SQL Server-gegevens en blijven als ze juist zijn.
    >
    > Het installatieprogramma wordt gecontroleerd dat de SQL-Server ingeschakeld op het niveau van de SQL Server database-containment is van Azure App Service op Azure Stack 1.3 en hoger.  Als dat niet het geval is, wordt u gevraagd met de volgende uitzondering:
    > ```sql
    >    Enable contained database authentication for SQL server by running below command on SQL server (Ctrl+C to copy)
    >    ***********************************************************
    >    sp_configure 'contained database authentication', 1;  
    >    GO  
    >    RECONFIGURE;  
    >    GO
    >    ***********************************************************
    > ```
    > Raadpleeg de [opmerkingen bij de release voor Azure App Service op Azure Stack 1.3](azure-stack-app-service-release-notes-update-three.md) voor meer informatie.
   
   ![App Service-installatieprogramma][12]

13. Bekijk de rolinstantie en SKU-opties. De standaardinstellingen worden ingevuld met het minimum aantal exemplaren en de minimale SKU voor elke rol in een ASDK-implementatie. Een overzicht van de vereisten voor vCPU en het geheugen wordt geboden om u te helpen bij uw implementatie plannen. Nadat u uw selecties, klikt u op **volgende**.

     > [!NOTE]
     > Volg de instructies in voor productie-implementaties, [planning van capaciteit voor Azure App Service-server-rollen in Azure Stack](azure-stack-app-service-capacity-planning.md).
     >
     >

    | Rol | Minimale exemplaren | Minimale SKU | Opmerkingen |
    | --- | --- | --- | --- |
    | Controller | 1 | Standard_A2 - (2 vCPU, 3584 MB) | Beheert en onderhoudt de status van de App Service-cloud. |
    | Beheer | 1 | Standard_A2 - (2 vCPUs, 3584 MB) | De App Service Azure Resource Manager en API-eindpunten, portal-extensies (admin, tenant, Functions-portal) en de gegevensservice beheert. Ter ondersteuning van failover, vergroot u de aanbevolen 2-exemplaren. |
    | Uitgever | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Inhoud via FTP en web deployment publiceert. |
    | FrontEnd | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Stuurt aanvragen door naar App Service-toepassingen. |
    | Gedeelde Werkrol | 1 | Standard_A1 - (1 vCPU, 1792 MB) | Hosts web of API-Apps en Azure Functions-apps. Mogelijk wilt u meer instanties toevoegen. Als operator kunt u definiëren van uw aanbieding en het kiezen van een SKU-laag. De lagen moeten minimaal één vCPU hebben. |

    ![App Service-installatieprogramma][14]

    > [!NOTE]
    > **Windows Server 2016 Core is geen ondersteund platform-installatiekopie voor gebruik met Azure App Service in Azure Stack.  Gebruik geen evaluatie installatiekopieën voor productie-implementaties.  Azure App Service in Azure Stack is vereist dat Microsoft.Net 3.5.1 SP1 op de installatiekopie die wordt gebruikt voor de implementatie is geactiveerd.   Marketplace publiceren Windows Server 2016 installatiekopieën nog geen deze functie is ingeschakeld.**

14. In de **Platforminstallatiekopie selecteren** vak, kiest u de installatiekopie van uw implementatie van Windows Server 2016 virtuele machine van de gegevensbronnen die beschikbaar zijn in de resourceprovider voor compute voor de App Service-cloud. Klik op **Volgende**.

15. Op de volgende pagina:
     1. Voer de gebruikersnaam voor Worker-rol virtuele machine-beheerder en het wachtwoord.
     2. Voer de gebruikersnaam van beheerder van overige rollen virtuele machine en het wachtwoord.
     3. Klik op **Volgende**.

    ![App Service-installatieprogramma][16]

16. Op de pagina Samenvatting:
    1. Controleer of de selecties die u hebt gemaakt. Als u wilt wijzigen, gebruikt u de **vorige** knoppen om naar de vorige pagina's te gaan.
    2. Als de configuratie juist zijn, selecteert u het selectievakje in.
    3. Voor het starten van de implementatie, klikt u op **volgende**.

    ![App Service-installatieprogramma][17]

17. Op de volgende pagina:
    1. De van de installatievoortgang bijhouden. App Service in Azure Stack duurt ongeveer 60 minuten om te implementeren op basis van de standaardselecties.
    2. Nadat het installatieprogramma met succes is voltooid, klikt u op **afsluiten**.

    ![App Service-installatieprogramma][18]

## <a name="validate-the-app-service-on-azure-stack-installation"></a>Valideren van de App-Service op Azure Stack-installatie

1. In de Azure Stack-beheerportal, gaat u naar **beheer - App Service**.

2. Schakel in het overzicht van de onder de status om te zien die de **Status** bevat **alle rollen zijn gereed**.

    ![App Service-beheer](media/azure-stack-app-service-deploy/image12.png)
    
> [!NOTE]
> Als u wilt implementeren in een bestaand virtueel netwerk en een interne IP-adres verbinding maken met de bestandsserver, moet u een uitgaande beveiligingsregel toevoegen voor het inschakelen van SMB-verkeer tussen het worker-subnet en de bestandsserver aan te geven.  Om dit te doen, gaat u naar de WorkersNsg in de beheerportal en voeg een uitgaande beveiligingsregel met de volgende eigenschappen toe:
> * Bron:
> * Poortbereik van bron: *
> * Bestemming: IP-adressen
> * Doel-IP-adresbereik: bereik van IP-adressen voor uw bestandsserver
> * Poortbereik van doel: 445
> * Protocol: TCP
> * Actie: toestaan
> * Prioriteit: 700
> * Naam: Outbound_Allow_SMB445
>

## <a name="test-drive-app-service-on-azure-stack"></a>Probeer de App Service in Azure Stack

Nadat u implementeert en registreer de resourceprovider App Service, het testen om ervoor te zorgen dat gebruikers web-apps en API apps kunnen implementeren.

> [!NOTE]
> U moet maken van een aanbieding die de naamruimte Microsoft.Web binnen het abonnement heeft. Vervolgens moet u een tenantabonnement hebben waarin ze zich op deze aanbieding abonneren. Zie voor meer informatie, [maken aanbieding](azure-stack-create-offer.md) en [plan maken](azure-stack-create-plan.md).
>
U *moet* een tenantabonnement te maken van toepassingen die gebruikmaken van App Service in Azure Stack hebt. De enige mogelijkheden die een servicebeheerder in de beheerportal uitvoeren kunt zijn met betrekking tot het beheer van de resource provider van App Service. Deze mogelijkheden omvatten capaciteit toe te voegen, implementatiebronnen configureren en werkrolniveaus en SKU's toe te voegen.
>
Vanaf de derde technical preview, voor het maken van web-API en Azure Functions apps, moet u de tenantportal te gebruiken en een tenantabonnement hebt.

1. Klik in de portal voor Azure Stack-tenant op **+ een resource maken** > **Web en mobiel** > **Web-App**.

2. Op de **Web-App** blade, typ een naam in de **Web-app** vak.

3. Onder **resourcegroep**, klikt u op **nieuw**. Typ een naam in de **resourcegroep** vak.

4. Klik op **App Service-plan/-locatie** > **Nieuw**.

5. Op de **App Service-plan** blade, typ een naam in de **App Service-plan** vak.

6. Klik op **prijscategorie** > **gratis gedeelde** of **gedeelde gedeelde** > **selecteren**  >   **OK** > **maken**.

7. In minder dan een minuut een tegel voor de nieuwe web-app wordt weergegeven op het dashboard. Klik op de tegel.

8. Op de **Web-App** blade, klikt u op **Bladeren** om de standaardwebsite voor deze app weer te geven.

## <a name="deploy-a-wordpress-dnn-or-django-website-optional"></a>Een WordPress, DNN of Django-website (optioneel) implementeren

1. Klik in de portal voor Azure Stack-tenant op **+**, gaat u naar de Azure Marketplace, een Django-website kunt implementeren en wachten op voltooiing. Een bestand system gebaseerde database maakt gebruik van het Django webplatform. Er is aanvullende resourceproviders, zoals SQL- of MySQL geen vereist.

2. Als u ook een MySQL-resourceprovider hebt geïmplementeerd, kunt u een WordPress-website vanuit de Marketplace kunt implementeren. Wanneer u wordt gevraagd voor databaseparameters, voert u de naam van de gebruiker als *User1@Server1*, met de gebruikersnaam en de naam van de server van uw keuze.

3. Als u ook een SQL Server-resourceprovider hebt geïmplementeerd, kunt u een website met DNN vanuit de Marketplace kunt implementeren. Wanneer u wordt gevraagd voor databaseparameters, kiest u een database in de computer met SQL Server die verbonden met de resourceprovider.

## <a name="next-steps"></a>Volgende stappen

U kunt ook proberen om andere [platform als een service (PaaS)-services](azure-stack-tools-paas-services.md).

- [SQL Server-resourceprovider](azure-stack-sql-resource-provider-deploy.md)
- [MySQL-resourceprovider](azure-stack-mysql-resource-provider-deploy.md)

<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[App_Service_Deployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525

<!--Image references-->
[1]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-advanced-create-package.png
[2]: ./media/azure-stack-app-service-deploy-offline/app-service-exe-advanced-complete-offline.png
[3]: ./media/azure-stack-app-service-deploy-offline/app-service-azure-stack-arm-endpoints.png
[4]: ./media/azure-stack-app-service-deploy-offline/app-service-azure-stack-subscription-information.png
[5]: ./media/azure-stack-app-service-deploy-offline/app-service-default-VNET-config.png
[6]: ./media/azure-stack-app-service-deploy-offline/app-service-custom-VNET-config.png
[7]: ./media/azure-stack-app-service-deploy-offline/app-service-custom-VNET-config-with-values.png
[8]: ./media/azure-stack-app-service-deploy-offline/app-service-fileshare-configuration.png
[9]: ./media/azure-stack-app-service-deploy-offline/app-service-fileshare-configuration-error.png
[10]: ./media/azure-stack-app-service-deploy-offline/app-service-identity-app.png
[11]: ./media/azure-stack-app-service-deploy-offline/app-service-certificates.png
[12]: ./media/azure-stack-app-service-deploy-offline/app-service-sql-configuration.png
[13]: ./media/azure-stack-app-service-deploy-offline/app-service-sql-configuration-error.png
[14]: ./media/azure-stack-app-service-deploy-offline/app-service-cloud-quantities.png
[15]: ./media/azure-stack-app-service-deploy-offline/app-service-windows-image-selection.png
[16]: ./media/azure-stack-app-service-deploy-offline/app-service-role-credentials.png
[17]: ./media/azure-stack-app-service-deploy-offline/app-service-azure-stack-deployment-summary.png
[18]: ./media/azure-stack-app-service-deploy-offline/app-service-deployment-progress.png
