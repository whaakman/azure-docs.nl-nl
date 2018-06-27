---
title: Certificaten voor Azure Service Fabric-toepassingen configureren op Linux | Microsoft Docs
description: Certificaten voor uw app met de Service Fabric-runtime op een Linux-cluster configureren
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
ms.openlocfilehash: 2d6d387ed12e7261d09669686c0710786a4302dd
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37025738"
---
# <a name="certificates-and-security-on-linux-clusters"></a>Certificaten en beveiliging op Linux-clusters

In dit artikel bevat informatie over het configureren van X.509-certificaten op Linux-clusters.

## <a name="location-and-format-of-x509-certificates-on-linux-nodes"></a>Locatie en indeling van het X.509-certificaten op Linux-knooppunten

Service Fabric verwacht in het algemeen X.509-certificaten aanwezig zijn in de */var/lib/sfcerts* map op de clusterknooppunten Linux. Dit geldt voor de clustercertificaten, clientcertificaten, enzovoort. In sommige gevallen kunt u een locatie met andere dan de *lib/var/sfcerts* map voor certificaten. U kunt bijvoorbeeld met Reliable Services gebouwd met behulp van de Service Fabric Java SDK, een andere locatie via het configuratiepakket (Settings.xml) voor sommige certificaten toepassingsspecifieke opgeven. Zie voor meer informatie, [certificaten waarnaar wordt verwezen in het configuratiepakket (Settings.xml)](#certificates-referenced-in-the-configuration-package-settingsxml).

Service Fabric verwacht voor Linux-clusters certificaten aanwezig zijn als beide een .pem-bestand dat het certificaat en de persoonlijke sleutel bevat of als een .crt-bestand met het certificaat en een .key-bestand dat de persoonlijke sleutel bevat. Alle bestanden moet PEM-indeling. 

Als u uw certificaat vanuit Azure Key Vault installeren met behulp van een [Resource Manager-sjabloon](./service-fabric-cluster-creation-via-arm.md#create-a-service-fabric-cluster-resource-manager-template) of [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/?view=latest#service_fabric) opdrachten, het certificaat is geïnstalleerd in de juiste indeling in de */var/ lib/sfcerts* map op elk knooppunt. Als u een certificaat via een andere methode installeren, moet u ervoor zorgen dat het certificaat correct is geïnstalleerd op clusterknooppunten.

## <a name="certificates-referenced-in-the-application-manifest"></a>Certificaten waarnaar wordt verwezen in het toepassingsmanifest

Certificaten die zijn opgegeven in de toepassing, bijvoorbeeld manifest via de [ **SecretsCertificate** ](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-model-schema-elements#secretscertificate-element) of [ **EndpointCertificate** ](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-service-model-schema-elements#endpointcertificate-element)elementen, moet aanwezig zijn in de */var/lib/sfcerts* directory. De elementen die worden gebruikt voor het opgeven van certificaten in het toepassingsmanifest rekening geen een padkenmerk, zodat de certificaten aanwezig in de standaardmap zijn. Deze elementen nemen een optionele **X509StoreName** kenmerk. De standaardwaarde is "Mijn", die naar wijst de */var/lib/sfcerts* directory op Linux-knooppunten. Een andere waarde is niet gedefinieerd op een Linux-cluster. Het is raadzaam dat u weglaten de **X509StoreName** kenmerk voor apps die worden uitgevoerd op Linux-clusters. 

## <a name="certificates-referenced-in-the-configuration-package-settingsxml"></a>Certificaten waarnaar wordt verwezen in het configuratiepakket (Settings.xml)

Voor sommige services kunt u X.509-certificaten in de [ConfigPackage](./service-fabric-application-and-service-manifests.md) (standaard Settings.xml). Dit is bijvoorbeeld het geval wanneer u certificaten gebruikt voor het beveiligen van RPC-kanalen voor Reliable Services services die zijn gebouwd met de Service Fabric .NET Core of de Java SDK's declareren. Er zijn twee manieren om verwijzing certificaten in het configuratiepakket. Er is ondersteuning van de .NET Core Java SDK's.

### <a name="using-x509-securitycredentialstype"></a>Met behulp van X509 SecurityCredentialsType

Met de .NET of Java SDK's, kunt u **X509** voor de **SecurityCredentialsType**. Dit komt overeen met de `X509Credentials` ([.NET](https://msdn.microsoft.com/library/system.fabric.x509credentials.aspx)/[Java](https://docs.microsoft.com/java/api/system.fabric._x509_credentials)) type `SecurityCredentials` ([.NET](https://msdn.microsoft.com/library/system.fabric.securitycredentials.aspx)/[Java](https://docs.microsoft.com/java/api/system.fabric._security_credentials)).

De **X509** verwijzing zoekt u het certificaat in een certificaatarchief. De volgende XML-code ziet u de parameters voor de locatie van het certificaat opgeven:

```xml
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateStoreName" Value="My" />
```

Voor een service die wordt uitgevoerd op Linux, **LocalMachine**/**mijn** verwijst naar de standaardlocatie voor certificaten, de */var/lib/sfcerts* directory. Voor elke combinatie van Linux **CertificateStoreLocation** en **NaamCertificaatarchief** zijn niet gedefinieerd. 

Geef altijd **LocalMachine** voor de **CertificateStoreLocation** parameter. Er is niet nodig om op te geven de **NaamCertificaatarchief** parameter omdat deze wordt standaard ingesteld op "Mijn". Met een **X509** -verwijzing, de certificaatbestanden moeten zich in de */var/lib/sfcerts* map op het clusterknooppunt.  

Het volgende XML bevat een **TransportSettings** sectie op basis van deze stijl:

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

Met de SDK voor Java, kunt u **X509_2** voor de **SecurityCredentialsType**. Dit komt overeen met de `X509Credentials2` ([Java](https://docs.microsoft.com/java/api/system.fabric._x509_credentials2)) type `SecurityCredentials` ([Java](https://docs.microsoft.com/java/api/system.fabric._security_credentials)). 

Met een **X509_2** -verwijzing, u een padparameter opgeven zodat u het certificaat in een map met andere dan vinden kunt */var/lib/sfcerts*.  De volgende XML-code ziet u de parameters voor de locatie van het certificaat opgeven: 

```xml
     <Parameter Name="SecurityCredentialsType" Value="X509_2" />
     <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
```

Het volgende XML bevat een **TransportSettings** sectie op basis van deze stijl.

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
> Het certificaat is opgegeven als een bestand .crt in de voorgaande XML. Dit betekent dat er is ook een .key-bestand bevat de persoonlijke sleutel op dezelfde locatie bevinden.

## <a name="configure-a-reliable-services-app-to-run-on-linux-clusters"></a>Een app Reliable Services uit te voeren op Linux-clusters configureren

De Service Fabric SDK's kunt u communiceren met de Service Fabric-runtime-API's voor het benutten van het platform. Wanneer u een toepassing die gebruikmaakt van deze functionaliteit op beveiligde Linux-clusters uitvoert, moet u uw toepassing configureren met een certificaat dat u deze gebruiken kunt om te valideren met de Service Fabric-runtime. Toepassingen die betrouwbare Service Fabric-Service-services die zijn geschreven met behulp van de .NET Core of de Java SDK's bevatten, moet deze configuratie. 

Toevoegen voor het configureren van een toepassing een [ **SecretsCertificate** ](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-model-schema-elements#secretscertificate-element) element onder de **certificaten** label, dat zich bevindt onder de **ApplicationManifest**  code in de *ApplicationManifest.xml* bestand. De volgende XML-code ziet u een waarnaar wordt verwezen door de vingerafdruk van certificaat: 

```xml
   <Certificates>
       <SecretsCertificate X509FindType="FindByThumbprint" X509FindValue="0A00AA0AAAA0AAA00A000000A0AA00A0AAAA00" />
   </Certificates>   
```

U kunt verwijzen naar het certificaat van het cluster of een certificaat dat u op elk clusterknooppunt installeert. Op Linux, het certificaatbestand moeten aanwezig zijn in de */var/lib/sfcerts* directory. Zie voor meer informatie, [locatie en indeling van het X.509-certificaten op Linux-knooppunten](#location-and-format-of-x509-certificates-on-linux-nodes).



