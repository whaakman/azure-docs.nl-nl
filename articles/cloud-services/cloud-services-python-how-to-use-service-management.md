---
title: Het gebruik van de servicebeheer-API (Python) - Functieoverzicht
description: Informatie over het programmatisch algemene service-beheertaken uitvoeren met Python.
services: cloud-services
documentationcenter: python
author: lmazuel
manager: wpickett
editor: 
ms.assetid: 61538ec0-1536-4a7e-ae89-95967fe35d73
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/30/2017
ms.author: lmazuel
ms.openlocfilehash: ca6e892e9f40204682be4ed00c413696f2022622
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="how-to-use-service-management-from-python"></a>Het gebruik van Service Management met Python
Deze handleiding wordt beschreven hoe u programmatisch algemene service-beheertaken uitvoeren met Python. De **ServiceManagementService** -klasse in de [Azure SDK voor Python](https://github.com/Azure/azure-sdk-for-python) ondersteunt programmatische toegang tot veel van de service management-gerelateerde functionaliteit die beschikbaar is in de [Azure Portal] [ management-portal] (zoals **maken, bijwerken en verwijderen van cloud-services, implementaties, data management-services en virtuele machines**). Deze functie kan nuttig zijn bij het bouwen van toepassingen die programmatische toegang tot het servicebeheer van de moeten worden gemaakt.

## <a name="WhatIs"></a>Wat is Service Management
De Service Management API biedt programmatisch toegang tot veel van de service management-functionaliteit beschikbaar via de [Azure-portal][management-portal]. De Azure SDK voor Python kunt u uw cloudservices en storage-accounts te beheren.

Voor het gebruik van de Service Management API, moet u [maken van een Azure-account](https://azure.microsoft.com/pricing/free-trial/).

## <a name="Concepts"></a>Concepten
De Azure SDK voor Python loopt de [Azure Service Management API][svc-mgmt-rest-api], namelijk een REST-API. Alle API-bewerkingen worden uitgevoerd over SSL en wederzijds geverifieerd met X.509 v3-certificatien. De beheerservice is toegankelijk via een service die wordt uitgevoerd in Azure of rechtstreeks toegankelijk via internet, vanuit elke toepassing die een HTTPS-verzoek kan verzenden en een HTTPS-antwoord kan ontvangen.

## <a name="Installation"></a>Installatie
Alle functies die worden beschreven in dit artikel zijn beschikbaar in de `azure-servicemanagement-legacy` pakket, die u kunt installeren met behulp van pip. Zie voor meer informatie over installatie (bijvoorbeeld als u niet bekend bent met Python) in dit artikel: [Python installeren en de Azure SDK](../python-how-to-install.md)

## <a name="Connect"></a>Hoe: verbinding maken met servicebeheer
Voor verbinding met het beheer van Service-eindpunt, moet u uw Azure-abonnement-ID en een geldig beheercertificaat. Kunt u uw abonnements-ID via de [Azure-portal][management-portal].

> [!NOTE]
> Het is nu mogelijk gebruik van certificaten die zijn gemaakt met OpenSSL wanneer u gebruikmaakt van Windows.  Hiervoor Python punt 2.7.4 of hoger. U wordt aangeraden gebruikers OpenSSL gebruiken in plaats van pfx, aangezien de ondersteuning voor pfx-certificaten waarschijnlijk in de toekomst verwijderd.
>
>

### <a name="management-certificates-on-windowsmaclinux-openssl"></a>Certificaten voor op Windows of Mac/Linux (OpenSSL)
U kunt [OpenSSL](http://www.openssl.org/) voor het maken van uw beheercertificaat.  U moet eigenlijk twee certificaten, één voor de server maken (een `.cer` bestand) en één voor de client (een `.pem` bestand). Maken van de `.pem` bestand, uitvoeren:

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Maken van de `.cer` certificaat, uitvoeren:

    openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

Zie voor meer informatie over Azure certificaten [certificaten voor Azure Cloud Services-overzicht](cloud-services-certs-create.md). Zie de documentatie op voor een volledige beschrijving van OpenSSL parameters [http://www.openssl.org/docs/apps/openssl.html](http://www.openssl.org/docs/apps/openssl.html).

Nadat u deze bestanden hebt gemaakt, moet u voor het uploaden van de `.cer` bestand naar Azure via de actie 'Uploaden' van het tabblad 'Instellingen' van de [Azure-portal][management-portal], en u moet een opslaglocatie Noteer de `.pem` bestand.

Wanneer u uw abonnements-ID hebt ontvangen, een certificaat gemaakt en geüpload de `.cer` bestand Azure, u verbinding kunt maken met het eindpunt van de Azure management door het doorgeven van de abonnements-id en het pad naar de `.pem` van het bestand in  **ServiceManagementService**:

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = '<path_to_.pem_certificate>'

    sms = ServiceManagementService(subscription_id, certificate_path)

In het voorgaande voorbeeld `sms` is een **ServiceManagementService** object. De **ServiceManagementService** klasse is de primaire klasse gebruikt voor het beheren van Azure-services.

### <a name="management-certificates-on-windows-makecert"></a>Certificaten beheren in Windows (MakeCert)
U kunt een zelfondertekende beheercertificaat maken op uw machine met `makecert.exe`.  Open een **Visual Studio-opdrachtprompt** als een **beheerder** en gebruikt u de volgende opdracht, vervangen *AzureCertificate* met de naam van het certificaat dat u wilt gebruiken.

    makecert -sky exchange -r -n "CN=AzureCertificate" -pe -a sha1 -len 2048 -ss My "AzureCertificate.cer"

De opdracht maakt u de `.cer` -bestand en installeert deze in de **persoonlijke** certificaatarchief. Zie voor meer informatie [certificaten voor Azure Cloud Services-overzicht](cloud-services-certs-create.md).

Nadat u het certificaat hebt gemaakt, moet u voor het uploaden van de `.cer` bestand naar Azure via de actie 'Uploaden' van het tabblad 'Instellingen' van de [Azure-portal][management-portal].

Wanneer u uw abonnements-ID hebt ontvangen, een certificaat gemaakt en geüpload de `.cer` bestand Azure, u verbinding kunt maken met het eindpunt van de Azure management door het doorgeven van de abonnements-id en de locatie van het certificaat in uw **persoonlijke**  certificaatarchief **ServiceManagementService** (opnieuw vervangen *AzureCertificate* met de naam van uw certificaat):

    from azure import *
    from azure.servicemanagement import *

    subscription_id = '<your_subscription_id>'
    certificate_path = 'CURRENT_USER\\my\\AzureCertificate'

    sms = ServiceManagementService(subscription_id, certificate_path)

In het voorgaande voorbeeld `sms` is een **ServiceManagementService** object. De **ServiceManagementService** klasse is de primaire klasse gebruikt voor het beheren van Azure-services.

## <a name="ListAvailableLocations"></a>Hoe: lijst met beschikbare locaties
U kunt de locaties die beschikbaar zijn voor het hosten van de services gebruiken de **lijst\_locaties** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_locations()
    for location in result:
        print(location.name)

Wanneer u een cloudservice of storage-service maakt, moet u een geldige locatie opgeeft. De **lijst\_locaties** methode retourneert altijd een bijgewerkte lijst van de momenteel beschikbare locaties. Op dit moment van schrijven zijn de beschikbare locaties:

* West-Europa
* Noord-Europa
* Zuidoost-Azië
* Oost-Azië
* VS - midden
* Noord-centraal VS
* VS - zuid/centraal
* VS - west
* VS - oost
* Japan - oost
* Japan - west
* Brazilië - zuid
* Australië - oost
* Australië - zuidoost

## <a name="CreateCloudService"></a>Hoe: Maak een cloudservice
Wanneer u een toepassing maakt en deze in Azure uitvoeren, de code en configuratie samen heten een Azure [cloudservice] [ cloud service] (ook wel een *gehoste service* in eerdere Azure versies). De **maken\_gehost\_service** methode kunt u een nieuwe gehoste service maken door de naam van een gehoste service (dit moet uniek zijn in Azure), een label (automatisch naar base64 gecodeerd), een beschrijving. en een locatie.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myhostedservice'
    label = 'myhostedservice'
    desc = 'my hosted service'
    location = 'West US'

    sms.create_hosted_service(name, label, desc, location)

U kunt alle gehoste services voor uw abonnement met de lijst de **lijst\_gehost\_services** methode:

    result = sms.list_hosted_services()

    for hosted_service in result:
        print('Service name: ' + hosted_service.service_name)
        print('Management URL: ' + hosted_service.url)
        print('Location: ' + hosted_service.hosted_service_properties.location)
        print('')

Als u informatie ophalen over een bepaalde gehoste service wilt, kunt u doen door de naam van de gehoste service voor de **ophalen\_gehost\_service\_eigenschappen** methode:

    hosted_service = sms.get_hosted_service_properties('myhostedservice')

    print('Service name: ' + hosted_service.service_name)
    print('Management URL: ' + hosted_service.url)
    print('Location: ' + hosted_service.hosted_service_properties.location)

Nadat u een cloudservice hebt gemaakt, kunt u uw code implementeren naar de service met de **maken\_implementatie** methode.

## <a name="DeleteCloudService"></a>Hoe: verwijderen van een service in de cloud
U kunt een cloudservice verwijderen door de naam van de service naar de **verwijderen\_gehost\_service** methode:

    sms.delete_hosted_service('myhostedservice')

Voordat u een service verwijderen kunt, moeten alle implementaties voor de service worden verwijderd. (Zie [hoe: verwijderen van een implementatie](#DeleteDeployment) voor meer informatie.)

## <a name="DeleteDeployment"></a>Hoe: verwijderen van een implementatie
Voor het verwijderen van een implementatie, gebruiken de **verwijderen\_implementatie** methode. Het volgende voorbeeld ziet u het verwijderen van een implementatie met de naam `v1`.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment('myhostedservice', 'v1')

## <a name="CreateStorageService"></a>Hoe: storage-service maken
Een [opslagservice](../storage/common/storage-create-storage-account.md) hebt u toegang tot Azure [Blobs](../storage/blobs/storage-python-how-to-use-blob-storage.md), [tabellen](../cosmos-db/table-storage-how-to-use-python.md), en [wachtrijen](../storage/queues/storage-python-how-to-use-queue-storage.md). U moet een naam voor de service (tussen 3 en 24 tekens in kleine letters en uniek zijn binnen Azure), een beschrijving, een label (maximaal 100 tekens, automatisch naar base64 gecodeerde) en een locatie voor het maken van een storage-service. Het volgende voorbeeld laat zien hoe een opslagservice maken door een locatie te geven.

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mystorageaccount'
    label = 'mystorageaccount'
    location = 'West US'
    desc = 'My storage account description.'

    result = sms.create_storage_account(name, desc, label, location=location)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

Opmerking in het voorgaande voorbeeld die de status van de **maken\_opslag\_account** bewerking kan worden opgehaald door het doorgeven van het resultaat geretourneerd door **maken\_opslag\_account** naar de **ophalen\_bewerking\_status** methode.  

Overzicht van uw storage-accounts en hun eigenschappen met de **lijst\_opslag\_accounts** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_storage_accounts()
    for account in result:
        print('Service name: ' + account.service_name)
        print('Location: ' + account.storage_service_properties.location)
        print('')

## <a name="DeleteStorageService"></a>Procedure: een opslagservice verwijderen
U kunt storage-service verwijderen door de naam van de storage-service naar de **verwijderen\_opslag\_account** methode. Storage-service verwijdert, worden alle gegevens die zijn opgeslagen in de service (blobs, tabellen en wachtrijen).

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_storage_account('mystorageaccount')

## <a name="ListOperatingSystems"></a>Hoe: lijst van beschikbare besturingssystemen
U kunt de besturingssystemen die beschikbaar zijn voor het hosten van de services gebruiken de **lijst\_besturingssysteem\_systemen** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.list_operating_systems()

    for os in result:
        print('OS: ' + os.label)
        print('Family: ' + os.family_label)
        print('Active: ' + str(os.is_active))

U kunt ook de **lijst\_besturingssysteem\_system\_families** methode, waarmee de besturingssystemen die door familie groepen:

    result = sms.list_operating_system_families()

    for family in result:
        print('Family: ' + family.label)
        for os in family.operating_systems:
            if os.is_active:
                print('OS: ' + os.label)
                print('Version: ' + os.version)
        print('')

## <a name="CreateVMImage"></a>Hoe: maken van een installatiekopie van besturingssysteem
Als een installatiekopie van besturingssysteem toevoegen aan de opslagplaats voor installatiekopieën, gebruiken de **toevoegen\_os\_installatiekopie** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'mycentos'
    label = 'mycentos'
    os = 'Linux' # Linux or Windows
    media_link = 'url_to_storage_blob_for_source_image_vhd'

    result = sms.add_os_image(label, media_link, name, os)

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

U kunt de installatiekopieën van besturingssystemen die beschikbaar zijn, gebruiken de **lijst\_os\_installatiekopieën** methode. Het bevat alle installatiekopieën van het platform en installatiekopieën van de gebruiker:

    result = sms.list_os_images()

    for image in result:
        print('Name: ' + image.name)
        print('Label: ' + image.label)
        print('OS: ' + image.os)
        print('Category: ' + image.category)
        print('Description: ' + image.description)
        print('Location: ' + image.location)
        print('Media link: ' + image.media_link)
        print('')

## <a name="DeleteVMImage"></a>Hoe: verwijderen van een installatiekopie van besturingssysteem
Gebruik voor het verwijderen van de gebruikersinstallatiekopie van een de **verwijderen\_os\_installatiekopie** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    result = sms.delete_os_image('mycentos')

    operation_result = sms.get_operation_status(result.request_id)
    print('Operation status: ' + operation_result.status)

## <a name="CreateVM"></a>Procedure: een virtuele machine maken
Voor het maken van een virtuele machine, moet u eerst maken een [cloudservice](#CreateCloudService).  Maak vervolgens de virtuele machine implementatie met de **maken\_virtuele\_machine\_implementatie** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    # Name of an os image as returned by list_os_images
    image_name = 'OpenLogic__OpenLogic-CentOS-62-20120531-en-us-30GB.vhd'

    # Destination storage account container/blob where the VM disk
    # will be created
    media_link = 'url_to_target_storage_blob_for_vm_hd'

    # Linux VM configuration, you can use WindowsConfigurationSet
    # for a Windows VM instead
    linux_config = LinuxConfigurationSet('myhostname', 'myuser', 'mypassword', True)

    os_hd = OSVirtualHardDisk(image_name, media_link)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=os_hd,
        role_size='Small')

## <a name="DeleteVM"></a>Procedure: een virtuele machine verwijderen
Als u wilt verwijderen van een virtuele machine, u eerst verwijderen de implementatie met behulp van de **verwijderen\_implementatie** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    sms.delete_deployment(service_name='myvm',
        deployment_name='myvm')

De cloudservice kan vervolgens worden verwijderd met behulp van de **verwijderen\_gehost\_service** methode:

    sms.delete_hosted_service(service_name='myvm')

## <a name="how-to-create-a-virtual-machine-from-a-captured-virtual-machine-image"></a>Procedure: Een virtuele Machine van de installatiekopie van een vastgelegde virtuele Machine maken
Voor het vastleggen van een VM-installatiekopie, belt u eerst de **vastleggen\_vm\_installatiekopie** methode:

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    # replace the below three parameters with actual values
    hosted_service_name = 'hs1'
    deployment_name = 'dep1'
    vm_name = 'vm1'

    image_name = vm_name + 'image'
    image = CaptureRoleAsVMImage    ('Specialized',
        image_name,
        image_name + 'label',
        image_name + 'description',
        'english',
        'mygroup')

    result = sms.capture_vm_image(
            hosted_service_name,
            deployment_name,
            vm_name,
            image
        )

Gebruik vervolgens om er zeker van te zijn dat u de installatiekopie hebt vastgelegd, de **lijst\_vm\_installatiekopieën** api, en zorg ervoor dat uw afbeelding wordt weergegeven in de resultaten:

    images = sms.list_vm_images()

Ten slotte de virtuele machine met de vastgelegde installatiekopie maakt, gebruikmaken van de **maken\_virtuele\_machine\_implementatie** methode zoals voordat, maar deze keer doorgegeven in de vm_image_name

    from azure import *
    from azure.servicemanagement import *

    sms = ServiceManagementService(subscription_id, certificate_path)

    name = 'myvm'
    location = 'West US'

    #Set the location
    sms.create_hosted_service(service_name=name,
        label=name,
        location=location)

    sms.create_virtual_machine_deployment(service_name=name,
        deployment_name=name,
        deployment_slot='production',
        label=name,
        role_name=name,
        system_config=linux_config,
        os_virtual_hard_disk=None,
        role_size='Small',
        vm_image_name = image_name)

Zie voor meer informatie over het vastleggen van een virtuele Linux-Machine in het klassieke implementatiemodel, [virtuele Linux-Machine vastleggen.](../virtual-machines/linux/classic/capture-image-classic.md).

Zie voor meer informatie over het vastleggen van een virtuele Windows-computer in het klassieke implementatiemodel, [virtuele Windows-Machine vastleggen.](../virtual-machines/windows/classic/capture-image-classic.md).

## <a name="What's Next"> </a>Volgende stappen
Nu dat u de basisprincipes van beheer-service hebt geleerd, kunt u toegang tot de [voltooid API-naslagdocumentatie voor de Azure Python SDK](http://azure-sdk-for-python.readthedocs.org/) en complexe taken gemakkelijk voor het beheren van uw toepassing python uitvoeren.

Raadpleeg het [Python Developer Center](/develop/python/) voor meer informatie.

[What is Service Management]: #WhatIs
[Concepts]: #Concepts
[How to: Connect to service management]: #Connect
[How to: List available locations]: #ListAvailableLocations
[How to: Create a cloud service]: #CreateCloudService
[How to: Delete a cloud service]: #DeleteCloudService
[How to: Create a deployment]: #CreateDeployment
[How to: Update a deployment]: #UpdateDeployment
[How to: Move deployments between staging and production]: #MoveDeployments
[How to: Delete a deployment]: #DeleteDeployment
[How to: Create a storage service]: #CreateStorageService
[How to: Delete a storage service]: #DeleteStorageService
[How to: List available operating systems]: #ListOperatingSystems
[How to: Create an operating system image]: #CreateVMImage
[How to: Delete an operating system image]: #DeleteVMImage
[How to: Create a virtual machine]: #CreateVM
[How to: Delete a virtual machine]: #DeleteVM
[Next Steps]: #NextSteps
[management-portal]: https://portal.azure.com/
[svc-mgmt-rest-api]: http://msdn.microsoft.com/library/windowsazure/ee460799.aspx


[cloud service]:/services/cloud-services/
