---
title: Installeren van Python en de SDK - Azure
description: Informatie over het installeren van Python en de SDK om te gebruiken met Azure.
services: 
documentationcenter: python
author: lmazuel
manager: wpickett
editor: 
ms.assetid: f36294be-daeb-4caf-9129-fce18130f552
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 09/06/2016
ms.author: lmazuel
ms.openlocfilehash: e69fff29be5b12c3c0004b4101eba69c7da87d3d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="installing-python-and-the-sdk"></a>Python en de SDK installeren
Python kan gemakkelijk worden ingesteld op Windows en is voorgeïnstalleerd op Mac, Linux, en [Bash voor Windows](https://msdn.microsoft.com/commandline/wsl/about). Deze handleiding helpt u bij de installatie en het ophalen van de machine klaar voor gebruik met Azure.

## <a name="whats-in-the-python-azure-sdk"></a>Wat is er in de Python Azure SDK?
De Azure SDK voor Python omvat onderdelen waarmee u kunt ontwikkelen, implementeren en beheren van toepassingen voor Azure Python. De Azure SDK voor Python bevat met name de volgende:

* **Management-bibliotheken**. Deze klassenbibliotheken bieden een interface met het beheren van een Azure-resources, zoals opslagaccounts, virtuele machines.
* **Runtime-bibliotheken**. Deze klassenbibliotheken bieden een interface voor toegang tot Azure-functies, zoals opslag en service bus.

## <a name="which-python-and-which-version-to-use"></a>Welke Python en welke versie moet worden gebruikt
Er zijn verschillende versies van Python interpreters beschikbaar - voorbeelden zijn:

* CPython - de standard en meest gebruikte Python-interpreter
* PyPy - snel en compatibele alternatieve implementatie op CPython
* IronPython - Python-interpreter die wordt uitgevoerd op .net/CLR
* Jython - Python-interpreter die wordt uitgevoerd op de virtuele Java-Machine

**CPython** v2.7 of v3.3 + en PyPy 5.4.0 zijn getest en worden ondersteund voor de Azure Python SDK.

## <a name="where-to-get-python"></a>Waar downloaden Python?
Er zijn verschillende manieren waarop u CPython:

* Rechtstreeks vanuit [www.python.org][www.python.org]
* Van een betrouwbare distro zoals [www.continuum.io][www.continuum.io], [www.enthought.com] [ www.enthought.com] of [www.activestate.com][www.activestate.com]
* Het bouwen van bron!

Tenzij u een specifieke reden hebt, raden we aan de eerste twee opties.

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a>SDK-installatie op Windows, Linux en Mac OS (clientbibliotheken)
Als u Python geïnstalleerd hebt, kunt u pip een bundel van de clientbibliotheken installeren in uw bestaande Python 2.7 of Python 3.3 +-omgeving. Hiermee downloadt u de pakketten uit de [Python Package Index] [ Python Package Index] (PyPI).

Mogelijk moet u beheerdersrechten:

* Linux- en Mac OS, gebruiken de `sudo` opdracht: `sudo pip install azure-mgmt-compute`.
* Windows: open de PowerShell-opdracht met een opdrachtprompt als beheerder

U kunt afzonderlijk elke wisselaar voor elke Azure-service installeren:

```console
   $ pip install azure-batch          # Install the latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install the latest Storage management library
```

Preview-pakketten kunnen worden geïnstalleerd met de `--pre` vlag:

```console
   $ pip install --pre azure-mgmt-compute # installs only the latest Compute Management library
```

U kunt ook een set van Azure-bibliotheken installeren in een enkele regel met de `azure` meta-pakket. Omdat niet alle pakketten in dit meta-pakket worden gepubliceerd als stabiel nog, de `azure` meta-pakket is nog steeds in preview.
Echter, de pakketten core vanuit code kwaliteit/volledigheid perspectieven kunnen worden overwogen 'stabiele' op dit moment

* het officiële label als zodanig synchroon met de andere talen zo snel mogelijk.
  We hebben geen plannen op verdere grote wijzigingen tot die tijd.

Omdat het een preview-versie, moet u de `--pre` vlag:

```console
   $ pip install --pre azure
```

of rechtstreeks

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a>Meer pakketten ophalen
De [Python Package Index] [ Python Package Index] (PyPI) is een geavanceerde selectie van Python-bibliotheken.  Als u kiest voor het installeren van een Distro, hebt u al meest interessante bits voor verschillende scenario's van webontwikkeling voor technische computergebruik.

## <a name="python-tools-for-visual-studio"></a>Python Tools for Visual Studio
[Python-Tools voor Visual Studio][Python-Tools voor Visual Studio] (PTVS) is een gratis/OSS-invoegtoepassing van Microsoft die tegenover in een volwaardig Python IDE verandert:

![How-naar-installatie-python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

Met behulp van de PTVS is optioneel, maar wordt aanbevolen omdat dit u biedt ondersteuning voor Python en Web Project/oplossing, foutopsporing, profilering, interactieve venster, bewerken van sjablonen en Intellisense.

PTVS ook eenvoudig te implementeren in Microsoft Azure, met ondersteuning voor implementatie naar [Cloudservices](cloud-services/cloud-services-python-ptvs.md) en [Websites](app-service/app-service-web-overview.md).

PTVS werkt met de bestaande installatie van Visual Studio 2013 of 2015 2017.  Zie voor documentatie, downloads en discussies [Python-Tools voor Visual Studio].  

## <a name="python-azure-scenarios-for-linux-and-macos"></a>Python Azure scenario's voor Linux en Mac OS
Voor Linux- of Mac OS, belangrijkste Azure scenario's die worden ondersteund:

1. Azure-Services gebruiken met behulp van de clientbibliotheken voor Python
2. Uw app uitvoert in een virtuele Linux-machine
3. Ontwikkelen en te publiceren naar Azure Websites met Git

Het eerste scenario kunt u uitgebreide web-apps die van de Azure-PaaS-mogelijkheden, zoals gebruikmaken auteur [blobopslag](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [opslag in de wachtrij](storage/queues/storage-python-how-to-use-queue-storage.md), [tabel opslag](cosmos-db/table-storage-how-to-use-python.md) enzovoort Pythonic wrappers voor de REST API's van Azure. Deze identiek werken op Windows, Mac en Linux.  U kunt ook deze clientbibliotheken gebruiken vanuit uw lokale ontwikkelcomputer of een Linux-VM uitgevoerd op Azure.

Voor het scenario met virtuele machine u gewoon een Linux-VM van uw keuze (Ubuntu, CentOS, Suse) starten en uitvoeren of beheren wat u leuk.  Een voorbeeld: u kunt uitvoeren [IPython] [ IPython] REPL/laptop op uw Windows of Mac/Linux machine en wijs een Linux- of Windows multi-proc VM is de Engine voor het IPython uitgevoerd op Azure, uw browser.

Zie voor informatie over het instellen van een Linux-VM, het [maken van een virtuele Machine waarop Linux](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) zelfstudie.

Git-implementatie gebruikt, kunt u een Python-webtoepassing ontwikkelen en deze publiceren naar een Azure-Website op een ander besturingssysteem.  Wanneer u uw opslagplaats naar Azure pushen, maakt automatisch een virtuele omgeving en pip installeert de vereiste pakketten.

Zie voor meer informatie over het gebruik van elk WSGI compatibel framework [Python configureren met Azure Websites](app-service/web-sites-python-configure.md).

## <a name="additional-software-and-resources"></a>Aanvullende Software en Resources:
* [Azure SDK voor Python ReadTheDocs](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [Azure SDK voor Python GitHub](https://github.com/Azure/azure-sdk-for-python)
* [Officiële Azure-voorbeelden voor Python](https://azure.microsoft.com/documentation/samples/?platform=python)
* [Continue Analytics Python-distributie][Continuum Analytics Python Distribution]
* [Enthought Python-distributie][Enthought Python Distribution]
* [ActiveState Python-distributie][ActiveState Python Distribution]
* [SciPy - een suite van wetenschappelijke Python-bibliotheken][SciPy - A suite of Scientific Python libraries]
* [NumPy - een bibliotheek cijfers voor Python][NumPy - A numerics library for Python]
* [Django-Project - een framework volwassen web/CMS][Django Project - A mature web framework/CMS]
* [IPython - een geavanceerde REPL/laptop voor Python][IPython - an advanced REPL/Notebook for Python]
* [Python-Tools voor Visual Studio op GitHub][Python Tools for Visual Studio on GitHub]
* [Python Developer Center](/develop/python/)

[Continuum Analytics Python Distribution]: http://continuum.io
[Enthought Python Distribution]: http://www.enthought.com
[ActiveState Python Distribution]: http://www.activestate.com
[www.python.org]: http://www.python.org
[www.continuum.io]: http://continuum.io
[www.enthought.com]: http://www.enthought.com
[www.activestate.com]: http://www.activestate.com
[SciPy - A suite of Scientific Python libraries]: http://www.scipy.org
[NumPy - A numerics library for Python]: http://www.numpy.org
[Django Project - A mature web framework/CMS]: http://www.djangoproject.com
[IPython - an advanced REPL/Notebook for Python]: http://ipython.org
[IPython]: http://ipython.org
[Python-Tools voor Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio on GitHub]: https://github.com/microsoft/ptvs
[Python Package Index]: http://pypi.python.org/pypi
[Microsoft Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281
[Microsoft Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990
[blob storage]:storage/blobs/storage-python-how-to-use-blob-storage.md
