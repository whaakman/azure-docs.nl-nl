---
title: Python configureren met Azure App Service WebApps
description: Deze zelfstudie wordt beschreven opties voor het ontwerpen en configureren van een eenvoudige webserver Gateway Interface (WSGI) compatibele Python-toepassing in Azure App Service Web Apps.
services: app-service
documentationcenter: python
tags: python
author: huguesv
manager: erikre
editor: 
ms.assetid: fd00dc91-9935-4331-b955-4bd71e66d518
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/26/2016
ms.author: huvalo
ms.openlocfilehash: 86e19d5bb942937779665eb60d9dc0654c16747d
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="configuring-python-with-azure-app-service-web-apps"></a>Python configureren met Azure App Service WebApps
Deze zelfstudie wordt beschreven opties voor het ontwerpen en configureren van een eenvoudige Web Server Gateway Interface (WSGI) compatibele Python-toepassing op [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

Aanvullende functies van Git-implementatie, zoals virtuele omgeving en de installatie van het pakket met requirements.txt beschrijft.

## <a name="bottle-django-or-flask"></a>Bottle, Django of Flask?
Azure Marketplace bevat sjablonen voor de frameworks Bottle, Django en Flask. Als u uw eerste web-app in Azure App Service ontwikkelt, kunt u snel van de Azure-portal kunt maken:

* [Web-apps maken met Bottle](https://portal.azure.com/#create/PTVS.Bottle)
* [Web-apps maken met Django](https://portal.azure.com/#create/PTVS.Django)
* [Web-apps maken met Flask](https://portal.azure.com/#create/PTVS.Flask)

## <a name="web-app-creation-on-azure-portal"></a>Web-Apps maken in Azure portal
Deze zelfstudie wordt ervan uitgegaan van een bestaand Azure-abonnement en de toegang tot de Azure-portal.

Als u een bestaande web-app niet hebt, kunt u een van de [Azure-portal](https://portal.azure.com).  Klik op de knop Nieuw in de linkerbovenhoek en klik vervolgens op **Web en mobiel** > **Web-app**.

## <a name="git-publishing"></a>GIT-publicatie
Configureer Git-publicatie voor uw nieuwe web-app door de instructies te volgen in [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md) (Lokale Git-implementatie naar Azure App Service). Deze zelfstudie maakt gebruik van Git te maken, beheren en publiceren van uw Python-web-app in Azure App Service.

Zodra de Git-publicatie is ingesteld, wordt een Git-opslagplaats gemaakt en gekoppeld aan uw web-app. URL van de opslagplaats wordt weergegeven en kan worden gebruikt voor gegevens uit de lokale ontwikkelomgeving push naar de cloud. Zorg ervoor dat een Git-client is geïnstalleerd en gebruik de instructies voor de push-inhoud van uw web-app in Azure App Service voor het publiceren van toepassingen via Git.

## <a name="application-overview"></a>Toepassingsoverzicht
In de volgende secties worden de volgende bestanden worden gemaakt. Ze moeten worden geplaatst in de hoofdmap van de Git-opslagplaats.

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a>WSGI-Handler
WSGI is een Python-standaard beschreven door [PEP 3333](http://www.python.org/dev/peps/pep-3333/) definiëren van een interface tussen de webserver en Python. Dit biedt een gestandaardiseerde interface voor het schrijven van verschillende webtoepassingen en frameworks met behulp van Python. Populaire frameworks voor Python-web gebruik vandaag WSGI. Azure App Service Web Apps biedt die u ondersteuning voor dergelijke frameworks; Bovendien kunnen advanced-gebruikers ook schrijven hun eigen, zolang de aangepaste handler de specificatie WSGI richtlijnen volgt.

Hier volgt een voorbeeld van een `app.py` waarmee wordt gedefinieerd met een aangepaste handler:

    def wsgi_app(environ, start_response):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        start_response(status, response_headers)
        response_body = 'Hello World'
        yield response_body.encode()

    if __name__ == '__main__':
        from wsgiref.simple_server import make_server

        httpd = make_server('localhost', 5555, wsgi_app)
        httpd.serve_forever()

U kunt deze toepassing lokaal met uitvoeren `python app.py`, bladert u naar `http://localhost:5555` in uw webbrowser.

## <a name="virtual-environment"></a>Virtuele omgeving
Hoewel het voorgaande voorbeeld-app niet alle externe pakketten vereist, is het waarschijnlijk dat uw toepassing enkele vereist.

Azure Git-implementatie ondersteunt om te beheren externe pakketafhankelijkheden, het maken van virtuele omgevingen.

Als Azure een requirements.txt in de hoofdmap van de opslagplaats detecteert, wordt automatisch een virtuele omgeving met de naam gemaakt `env`. Dit gebeurt alleen bij de eerste implementatie of tijdens een implementatie na het geselecteerde Python runtime is gewijzigd.

U waarschijnlijk wilt maken van een virtuele omgeving lokaal voor ontwikkeling, maar niet opnemen in de Git-opslagplaats.

## <a name="package-management"></a>Pakketbeheer
Pakketten die worden vermeld in requirements.txt worden automatisch geïnstalleerd in de virtuele omgeving met behulp van pip. Dit gebeurt op elke implementatie, maar pip slaat de installatie als een pakket is al geïnstalleerd.

Voorbeeld `requirements.txt`:

    azure==0.8.4


## <a name="python-version"></a>Python-versie
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

Voorbeeld `runtime.txt`:

    python-2.7


## <a name="webconfig"></a>Web.config
U moet maken van een web.config-bestand om op te geven hoe de-server aanvragen moet verwerken.

Als er een web.x.y.config-bestand in de opslagplaats, waar x.y komt overeen met de geselecteerde Python-runtime, vervolgens kopieert Azure automatisch het juiste bestand als web.config.

De volgende voorbeelden van web.config is afhankelijk van een virtuele omgeving proxyscript die in de volgende sectie wordt beschreven.  Ze werken met de WSGI-handler die wordt gebruikt in het voorbeeld `app.py` hierboven.

Voorbeeld `web.config` voor Python 2.7:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\activate_this.py" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite"
                      url="handler.fcgi/{R:1}"
                      appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Voorbeeld `web.config` voor Python 3.4:

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\python.exe" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_venv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python34\python.exe|D:\Python34\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


Statische bestanden worden verwerkt door de webserver rechtstreeks, zonder tussenkomst van Python-code, voor betere prestaties.

De locatie van de statische bestanden op schijf moet overeenkomen met de locatie in de URL in de voorgaande voorbeelden. Dit betekent dat een aanvraag voor `http://pythonapp.azurewebsites.net/static/site.css` dient het bestand op de schijf op `\static\site.css`.

`WSGI_ALT_VIRTUALENV_HANDLER`is waar u de handler WSGI opgeven. In de voorgaande voorbeelden hieraan `app.wsgi_app` omdat de handler een functie met de naam is `wsgi_app` in `app.py` in de hoofdmap.

`PYTHONPATH`kan worden aangepast, maar als u alle afhankelijkheden in de virtuele omgeving door te geven ze in requirements.txt installeert, moet u dient niet te wijzigen.

## <a name="virtual-environment-proxy"></a>Virtuele omgeving Proxy
Het volgende script wordt gebruikt voor het ophalen van de handler WSGI en het activeren van de virtuele omgeving en log-fouten. Het is ontworpen voor algemene en die wordt gebruikt zonder wijzigingen.

Inhoud van `ptvs_virtualenv_proxy.py`:

     # ############################################################################
     #
     # Copyright (c) Microsoft Corporation. 
     #
     # This source code is subject to terms and conditions of the Apache License, Version 2.0. A 
     # copy of the license can be found in the License.html file at the root of this distribution. If 
     # you cannot locate the Apache License, Version 2.0, please send an email to 
     # vspython@microsoft.com. By using this source code in any fashion, you are agreeing to be bound 
     # by the terms of the Apache License, Version 2.0.
     #
     # You must not remove this notice, or any other, from this software.
     #
     # ###########################################################################

    import datetime
    import os
    import sys
    import traceback

    if sys.version_info[0] == 3:
        def to_str(value):
            return value.decode(sys.getfilesystemencoding())

        def execfile(path, global_dict):
            """Execute a file"""
            with open(path, 'r') as f:
                code = f.read()
            code = code.replace('\r\n', '\n') + '\n'
            exec(code, global_dict)
    else:
        def to_str(value):
            return value.encode(sys.getfilesystemencoding())

    def log(txt):
        """Logs fatal errors to a log file if WSGI_LOG env var is defined"""
        log_file = os.environ.get('WSGI_LOG')
        if log_file:
            f = open(log_file, 'a+')
            try:
                f.write('%s: %s' % (datetime.datetime.now(), txt))
            finally:
                f.close()

    ptvsd_secret = os.getenv('WSGI_PTVSD_SECRET')
    if ptvsd_secret:
        log('Enabling ptvsd ...\n')
        try:
            import ptvsd
            try:
                ptvsd.enable_attach(ptvsd_secret)
                log('ptvsd enabled.\n')
            except: 
                log('ptvsd.enable_attach failed\n')
        except ImportError:
            log('error importing ptvsd.\n')

    def get_wsgi_handler(handler_name):
        if not handler_name:
            raise Exception('WSGI_ALT_VIRTUALENV_HANDLER env var must be set')

        if not isinstance(handler_name, str):
            handler_name = to_str(handler_name)

        module_name, _, callable_name = handler_name.rpartition('.')
        should_call = callable_name.endswith('()')
        callable_name = callable_name[:-2] if should_call else callable_name
        name_list = [(callable_name, should_call)]
        handler = None
        last_tb = ''

        while module_name:
            try:
                handler = __import__(module_name, fromlist=[name_list[0][0]])
                last_tb = ''
                for name, should_call in name_list:
                    handler = getattr(handler, name)
                    if should_call:
                        handler = handler()
                break
            except ImportError:
                module_name, _, callable_name = module_name.rpartition('.')
                should_call = callable_name.endswith('()')
                callable_name = callable_name[:-2] if should_call else callable_name
                name_list.insert(0, (callable_name, should_call))
                handler = None
                last_tb = ': ' + traceback.format_exc()

        if handler is None:
            raise ValueError('"%s" could not be imported%s' % (handler_name, last_tb))

        return handler

    activate_this = os.getenv('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS')
    if not activate_this:
        raise Exception('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS is not set')

    def get_virtualenv_handler():
        log('Activating virtualenv with %s\n' % activate_this)
        execfile(activate_this, dict(__file__=activate_this))

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler

    def get_venv_handler():
        log('Activating venv with executable at %s\n' % activate_this)
        import site
        sys.executable = activate_this
        old_sys_path, sys.path = sys.path, []

        site.main()

        sys.path.insert(0, '')
        for item in old_sys_path:
            if item not in sys.path:
                sys.path.append(item)

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler


## <a name="customize-git-deployment"></a>Git-implementatie aanpassen
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]

## <a name="troubleshooting---package-installation"></a>Probleemoplossing - Installatie van pakketten
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a>Probleemoplossing - Virtuele omgeving
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Volgende stappen
Raadpleeg het [Python Developer Center](/develop/python/) voor meer informatie.

> [!NOTE]
> Als u aan de slag wilt met Azure App Service voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen](https://azure.microsoft.com/try/app-service/). Hier kunt u direct een tijdelijke web-app maken in App Service. U hebt geen creditcard nodig en u gaat geen verplichtingen aan.
> 
> 
