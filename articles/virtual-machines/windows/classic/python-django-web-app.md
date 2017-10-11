---
title: Django-web-app op een Windows Server Azure VM | Microsoft Docs
description: Informatie over het hosten van een website op basis van Django in Azure met behulp van een VM van Windows Server 2012 R2 Datacenter met het klassieke implementatiemodel.
services: virtual-machines-windows
documentationcenter: python
author: huguesv
manager: wpickett
editor: 
tags: azure-service-management
ms.assetid: e36484d1-afbf-47f5-b755-5e65397dc1c3
ms.service: virtual-machines-windows
ms.workload: web
ms.tgt_pltfrm: vm-windows
ms.devlang: python
ms.topic: article
ms.date: 05/31/2017
ms.author: huvalo
ms.openlocfilehash: 283a296fb39863c2801be1093cc4f56904786abd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="django-hello-world-web-app-on-a-windows-server-vm"></a>Django Hallo wereld-web-app op een virtuele machine van Windows Server

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Azure Resource Manager en het klassieke implementatiemodel](../../../resource-manager-deployment-model.md). Dit artikel wordt beschreven voor het klassieke implementatiemodel. U wordt aangeraden de meeste nieuwe implementaties het Resource Manager-model gebruiken.

Deze zelfstudie laat zien hoe u voor het hosten van een website op basis van Django in Windows Server in Azure Virtual Machines. In de zelfstudie gaan we ervan uit geen ervaring met Azure. Wanneer u de zelfstudie hebt voltooid, hebt u een toepassing op basis van de Django up en wordt uitgevoerd in de cloud.

Leer hoe u het volgende doet:

* Een virtuele machine van Azure naar host Django instellen. Hoewel deze zelfstudie wordt uitgelegd hoe u dit doen voor **Windows Server**, kunt u dezelfde doen voor een Linux-VM wordt gehost in Azure.
* Maak een nieuwe Django-toepassing in Windows.

De zelfstudie laat zien hoe een basic Hallo wereld-webtoepassing bouwen. De toepassing wordt gehost in Azure een virtuele machine.

De volgende schermafbeelding ziet de voltooide toepassing:

![Een browservenster weergegeven Hallo wereld-pagina in Azure][1]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-to-host-django"></a>Maken en een virtuele machine van Azure naar host Django instellen

1. Zie voor informatie over het maken van een virtuele machine van Azure met de distributie van Windows Server 2012 R2 Datacenter [maken van een virtuele machine waarop Windows wordt uitgevoerd in de Azure portal](tutorial.md).
2. Instellen van Azure naar poort 80 verkeer via het web naar poort 80 op de virtuele machine:
   
   1. In de Azure-portal, gaat u naar het dashboard en selecteer de zojuist gemaakte virtuele machine.
   2. Klik op **Eindpunten** en vervolgens op **Toevoegen**.

     ![Een eindpunt toevoegen](./media/python-django-web-app/django-helloworld-add-endpoint-new-portal.png)

   3. Op de **eindpunt toevoegen** pagina voor **naam**, voer **HTTP**. De openbare en persoonlijke TCP-poorten instellen op **80**.

     ![Naam en openbare en particuliere poort instellen](./media/python-django-web-app/django-helloworld-add-endpoint-set-ports-new-portal.png)

   4. Klik op **OK**.
     
3. Selecteer in het dashboard van de VM. Met Remote Desktop Protocol (RDP) extern aanmelden bij de zojuist gemaakte virtuele machine van Azure, klikt u op **Connect**.  

> [!IMPORTANT] 
> De volgende instructies wordt ervan uitgegaan dat u de virtuele machine correct ondertekend. Ze ook wordt ervan uitgegaan dat dat u opdrachten in de virtuele machine en niet op uw lokale computer uitvoert.

## <a id="setup"></a>Python en Django WFastCGI installeren
> [!NOTE]
> Als u wilt downloaden met behulp van Internet Explorer, mogelijk moet Internet Explorer configureren **verbeterde beveiliging** instellingen. Om dit te doen, klikt u op **Start** > **Systeembeheer** > **Serverbeheer** > **lokale Server**. Klik op **verbeterde beveiliging van Internet Explorer**, en selecteer vervolgens **uit**.

1. Installeer de nieuwste versie van Python 2.7 of 3.4 van Python [python.org][python.org].
2. De met behulp van pip wfastcgi en django-pakketten installeren.
   
    Gebruik de volgende opdracht voor Python 2.7:
   
        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django
   
    Gebruik de volgende opdracht voor Python 3.4:
   
        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="install-iis-with-fastcgi"></a>IIS met FastCGI installeren
* Internet Information Services (IIS) installeren met de ondersteuning van FastCGI. Dit kan enige tijd duren om uit te voeren.
   
        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="create-a-new-django-application"></a>Maak een nieuwe Django-toepassing
1. In C:\inetpub\wwwroot, om een nieuwe Django-project maakt, voer de volgende opdracht:
   
   Gebruik de volgende opdracht voor Python 2.7:
   
       C:\Python27\Scripts\django-admin.exe startproject helloworld
   
   Gebruik de volgende opdracht voor Python 3.4:
   
       C:\Python34\Scripts\django-admin.exe startproject helloworld
   
   ![Het resultaat van de opdracht New-AzureService](./media/python-django-web-app/django-helloworld-cmd-new-azure-service.png)
2. De `django-admin` opdracht genereert een basisstructuur voor Django-gebaseerde websites:
   
   * `helloworld\manage.py`helpt u te hosten en stoppen met het hosten van uw website op basis van een Django.
   * `helloworld\helloworld\settings.py`Django-instellingen voor de toepassing is.
   * `helloworld\helloworld\urls.py`heeft de code van de toewijzing tussen elke URL en de weergave.
3. Maak een nieuw bestand met de naam views.py in de map C:\inetpub\wwwroot\helloworld\helloworld. Dit bestand heeft de weergave die de pagina "Hallo wereld geeft". Voer de volgende opdrachten in de code-editor:
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. De inhoud van het bestand urls.py vervangen door de volgende opdrachten:
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-iis"></a>Instellen van IIS
1. In het bestand applicationhost.config globale ontgrendelen de sectie-handlers.  Hierdoor kan het bestand web.config, het Python-handler moet worden gebruikt. Deze opdracht toevoegen:
   
        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers
2. WFastCGI activeren. Hiermee wordt een toepassing op het bestand applicationhost.config globale, dat naar uw uitvoerbare Python-interpreter en het script wfastcgi.py verwijst toegevoegd.
   
    In Python 2.7:
   
        C:\python27\scripts\wfastcgi-enable
   
    In Python 3.4:
   
        C:\python34\scripts\wfastcgi-enable
3. In C:\inetpub\wwwroot\helloworld, maakt u een web.config-bestand. De waarde van de `scriptProcessor` kenmerk moet overeenkomen met de uitvoer van de vorige stap. Zie voor meer informatie over de instelling wfastcgi [pypi wfastcgi][wfastcgi].
   
   In Python 2.7:
   
        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>
   
   In Python 3.4:
   
        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>
4. De locatie van de standaardwebsite van IIS om te verwijzen naar de projectmap Django bijwerken:
   
        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"
5. Laad de webpagina die in uw browser.

![Een browservenster weergegeven Hallo wereld-pagina op Azure][1]

## <a name="shut-down-your-azure-virtual-machine"></a>De virtuele machine van Azure afsluiten
Wanneer u met deze zelfstudie bent klaar, raden we aan u afgesloten of verwijder de Azure virtuele machine die u hebt gemaakt voor de zelfstudie. Hiermee wordt vrijgemaakt bronnen voor andere zelfstudies en kunt u voorkomen dat Azure gebruikskosten aangaan.

[1]: ./media/python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
