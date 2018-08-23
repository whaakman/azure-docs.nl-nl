---
title: Certificaten voor Azure Service Fabric-toepassingen configureren in Linux | Microsoft Docs
description: Certificaten voor uw app configureren met de Service Fabric-runtime op een Linux-cluster
services: service-fabric
documentationcenter: NA
author: JimacoMS2
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/26/2018
ms.author: v-jamebr
ms.openlocfilehash: 97f33a1c0c42b534dafd1e4ed378b655b339395a
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2018
ms.locfileid: "42054318"
---
# <a name="certificates-and-security-on-linux-clusters"></a>Certificaten en beveiliging op Linux-clusters

In dit artikel bevat informatie over het configureren van X.509-certificaten op Linux-clusters.

## <a name="location-and-format-of-x509-certificates-on-linux-nodes"></a>Locatie en de opmaak van X.509-certificaten op Linux-knooppunten

Service Fabric in het algemeen wordt verwacht dat X.509-certificaten aanwezig zijn in de */var/lib/sfcerts* map op de knooppunten van de Linux-cluster. Dit geldt van clustercertificaten, certificaten, enzovoort. In sommige gevallen kunt u een locatie met andere dan de *var/lib/sfcerts* map voor certificaten. Bijvoorbeeld, met betrouwbare Services die zijn gebouwd met behulp van de Service Fabric Java SDK, kunt u een andere locatie via het configuratiepakket (Settings.xml) voor sommige certificaten toepassingsspecifieke opgeven. Zie voor meer informatie, [certificaten waarnaar wordt verwezen in de configuratiepakket (Settings.xml)](#certificates-referenced-in-the-configuration-package-settingsxml).

Service Fabric voor Linux-clusters, wordt verwacht dat de certificaten aanwezig zijn als een van beide een .pem-bestand dat het certificaat en de persoonlijke sleutel bevat of als een .crt-bestand dat het certificaat bevat en een .key-bestand dat de persoonlijke sleutel bevat. Alle bestanden moeten zich in de PEM-indeling. 

Als u uw certificaat geïnstalleerd vanuit Azure Key Vault met behulp van een [Resource Manager-sjabloon](./service-fabric-cluster-creation-create-template.md) of [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/?view=latest#service_fabric) opdrachten, het certificaat is geïnstalleerd in de juiste indeling in de */var/ lib/sfcerts* Active directory op elk knooppunt. Als u een certificaat via een andere methode installeren, moet u ervoor zorgen dat het certificaat correct is geïnstalleerd op de clusterknooppunten.

## <a name="certificates-referenced-in-the-application-manifest"></a>Certificaten waarnaar wordt verwezen in het toepassingsmanifest

Certificaten die zijn opgegeven in de toepassing, bijvoorbeeld manifest via de [ **SecretsCertificate** ](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-model-schema-elements#secretscertificate-element) of [ **EndpointCertificate** ](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-service-model-schema-elements#endpointcertificate-element)elementen, moet aanwezig zijn in de */var/lib/sfcerts* directory. De elementen die worden gebruikt om op te geven van certificaten in het toepassingsmanifest hebben geen een padkenmerk, zodat de certificaten aanwezig in de standaarddirectory zijn. Deze elementen nemen een optionele **X509StoreName** kenmerk. De standaardwaarde is "Mijn", die naar verwijst de */var/lib/sfcerts* map op Linux-knooppunten. Een andere waarde is niet gedefinieerd op een Linux-cluster. Het is raadzaam dat u weglaat de **X509StoreName** kenmerk voor apps die worden uitgevoerd op Linux-clusters. 

## <a name="certificates-referenced-in-the-configuration-package-settingsxml"></a>Certificaten waarnaar wordt verwezen in de configuratiepakket (Settings.xml)

Voor sommige services kunt u X.509-certificaten in de [ConfigPackage](./service-fabric-application-and-service-manifests.md) (standaard Settings.xml). Dit is bijvoorbeeld het geval wanneer u certificaten voor het beveiligen van RPC-kanalen voor Reliable Services-services die zijn gebouwd met de Service Fabric .NET Core of de Java-SDK's declareren. Er zijn twee manieren om certificaten in het configuratiepakket voor de verwijzing. Ondersteuning voor varieert tussen de .NET Core- en Java-SDK's.

### <a name="using-x509-securitycredentialstype"></a>Met behulp van X509 SecurityCredentialsType

Met de .NET of Java-SDK's, kunt u **X509** voor de **SecurityCredentialsType**. Dit komt overeen met de `X509Credentials` ([.NET](https://msdn.microsoft.com/library/system.fabric.x509credentials.aspx)/[Java](https://docs.microsoft.com/java/api/system.fabric._x509_credentials)) type `SecurityCredentials` ([.NET](https://msdn.microsoft.com/library/system.fabric.securitycredentials.aspx)/[Java](https://docs.microsoft.com/java/api/system.fabric._security_credentials)).

De **X509** verwijzing wordt gezocht naar het certificaat in een certificaatarchief. Het volgende XML-bestand ziet u de parameters voor de locatie van het certificaat opgeven:

```xml
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateStoreName" Value="My" />
```

Voor een service op Linux, **LocalMachine /**/**mijn** verwijst naar de standaardlocatie voor certificaten, de */var/lib/sfcerts* directory. Voor Linux, alle andere combinaties van **CertificateStoreLocation** en **NaamCertificaatarchief** zijn gedefinieerd. 

Geef altijd **LocalMachine /** voor de **CertificateStoreLocation** parameter. Er is niet nodig om op te geven de **NaamCertificaatarchief** parameter omdat deze standaard ingesteld op 'My'. Met een **X509** ter referentie de certificaatbestanden moeten zich in de */var/lib/sfcerts* map op het clusterknooppunt.  

De volgende XML bevat een **TransportSettings** sectie op basis van deze stijl:

```xml
<Section Name="HelloWorldStatefulTransportSettings">
    <Parameter Name="MaxMessageSize" Value="10000000" />
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
    <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
    <Parameter Name="CertificateRemoteThumbprints" Value="9FEF3950642138446CC364A396E1E881DB76B483" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
    <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
</Section>
```

### <a name="using-x5092-securitycredentialstype"></a>Met behulp van X509_2 SecurityCredentialsType

Met de Java SDK, kunt u **X509_2** voor de **SecurityCredentialsType**. Dit komt overeen met de `X509Credentials2` ([Java](https://docs.microsoft.com/java/api/system.fabric._x509_credentials2)) type `SecurityCredentials` ([Java](https://docs.microsoft.com/java/api/system.fabric._security_credentials)). 

Met een **X509_2** -verwijzing, u een padparameter opgeven, zodat u kunt het certificaat te in een map met andere dan zoeken */var/lib/sfcerts*.  Het volgende XML-bestand ziet u de parameters voor de locatie van het certificaat opgeven: 

```xml
     <Parameter Name="SecurityCredentialsType" Value="X509_2" />
     <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
```

De volgende XML bevat een **TransportSettings** sectie op basis van deze stijl.

```xml
<!--Section name should always end with "TransportSettings".-->
<!--Here we are using a prefix "HelloWorldStateless".-->
<Section Name="HelloWorldStatelessTransportSettings">
    <Parameter Name="MaxMessageSize" Value="10000000" />
    <Parameter Name="SecurityCredentialsType" Value="X509_2" />
    <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
    <Parameter Name="CertificateProtectionLevel" Value="EncryptandSign" />
    <Parameter Name="CertificateRemoteThumbprints" Value="BD1C71E248B8C6834C151174DECDBDC02DE1D954" />
</Section>
```

> [!NOTE]
> Het certificaat is opgegeven als een .crt-bestand in het voorgaande XML-bestand. Dit betekent dat er is ook een .key-bestand met de persoonlijke sleutel op dezelfde locatie.

## <a name="configure-a-reliable-services-app-to-run-on-linux-clusters"></a>Een Reliable Services-app uit te voeren op Linux-clusters configureren

De Service Fabric-SDK's kunt u communiceren met de Service Fabric-runtime-API's gebruikmaken van het platform. Wanneer u een toepassing die gebruikmaakt van deze functionaliteit op een beveiligd Linux-clusters uitvoert, moet u uw toepassing configureren met een certificaat die kan worden gebruikt om te valideren met de Service Fabric-runtime. Toepassingen die Service Fabric Reliable Service-services die zijn geschreven met behulp van de .NET Core of de Java-SDK's bevatten vereist deze configuratie. 

Voor het configureren van een toepassing toevoegen een [ **SecretsCertificate** ](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-model-schema-elements#secretscertificate-element) element onder de **certificaten** tag, dat zich bevindt onder het **ApplicationManifest**  code in de *ApplicationManifest.xml* bestand. Het volgende XML-bestand ziet u een waarnaar wordt verwezen door de vingerafdruk van certificaat: 

```xml
   <Certificates>
       <SecretsCertificate X509FindType="FindByThumbprint" X509FindValue="0A00AA0AAAA0AAA00A000000A0AA00A0AAAA00" />
   </Certificates>   
```

U kunt verwijzen naar het clustercertificaat of een certificaat dat u op elk clusterknooppunt installeert. Op Linux, de certificaatbestanden moet aanwezig zijn in de */var/lib/sfcerts* directory. Zie voor meer informatie, [locatie en de opmaak van X.509-certificaten op Linux-knooppunten](#location-and-format-of-x509-certificates-on-linux-nodes).



