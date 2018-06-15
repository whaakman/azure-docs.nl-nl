---
title: Certificaten importeren in een container uitgevoerd op Azure Service Fabric | Microsoft Docs
description: Ontdek hoe u certificaatbestanden importeren in een Service Fabric-containerservice.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: f234a6f6ca56d1833aac53f490feb5f667a6bf1b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34208213"
---
# <a name="import-a-certificate-file-into-a-container-running-on-service-fabric"></a>Een certificaatbestand te importeren naar een container uitgevoerd op de Service Fabric

U kunt uw containerservices beveiligen door te geven van een certificaat. Service Fabric biedt een mechanisme voor services binnen een container voor toegang tot een certificaat dat is geïnstalleerd op de knooppunten in een Windows- of Linux-cluster (versie 5.7 of hoger). Het certificaat moet worden geïnstalleerd in de hoofdmap van LocalMachine op alle knooppunten van het cluster. Gegevens van het certificaat is opgegeven in het toepassingsmanifest onder de `ContainerHostPolicies` label als het volgende codefragment bevat:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

Voor Windows-clusters, wanneer de toepassing wordt gestart, wordt de runtime leest de certificaten en een PFX-bestand en het wachtwoord voor elk certificaat gegenereerd. Deze PFX-bestand en dit wachtwoord zijn toegankelijk in de container met de volgende omgevingsvariabelen: 

* Certificates_ServicePackageName_CodePackageName_CertName_PFX
* Certificates_ServicePackageName_CodePackageName_CertName_Password

Voor Linux-clusters, de certificaten (PEM) gekopieerd uit de store naar de container door X509StoreName is opgegeven. De bijbehorende omgevingsvariabelen op Linux zijn:

* Certificates_ServicePackageName_CodePackageName_CertName_PEM
* Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey

U kunt ook als u al hebt de certificaten in de vereiste vorm en om deze te openen in de container, u kunt een gegevenspakket in uw apppakket maken en geeft u de volgende binnen uw toepassingsmanifest:

```xml
<ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
  <CertificateRef Name="MyCert1" DataPackageRef="[DataPackageName]" DataPackageVersion="[Version]" RelativePath="[Relative Path to certificate inside DataPackage]" Password="[password]" IsPasswordEncrypted="[true/false]"/>
 ```

De containerservice of het proces is verantwoordelijk voor het importeren van het certificaatbestand in de container. Om het certificaat importeert, kunt u `setupentrypoint.sh` scripts of het uitvoeren van aangepaste code in het proces van de container. Hier is de voorbeeldcode in C# voor het importeren van het PFX-bestand:

```csharp
string certificateFilePath = Environment.GetEnvironmentVariable("Certificates_MyServicePackage_NodeContainerService.Code_MyCert1_PFX");
string passwordFilePath = Environment.GetEnvironmentVariable("Certificates_MyServicePackage_NodeContainerService.Code_MyCert1_Password");
X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
string password = File.ReadAllLines(passwordFilePath, Encoding.Default)[0];
password = password.Replace("\0", string.Empty);
X509Certificate2 cert = new X509Certificate2(certificateFilePath, password, X509KeyStorageFlags.MachineKeySet | X509KeyStorageFlags.PersistKeySet);
store.Open(OpenFlags.ReadWrite);
store.Add(cert);
store.Close();
```
Deze PFX-certificaat kan worden gebruikt voor het verifiëren van de toepassing of service of beveiligde communicatie met andere services. De bestanden zijn standaard ACLed alleen aan het systeem. U kunt ACL naar andere accounts zoals vereist door de service.

Lees de volgende artikelen een volgende stap:

* [Een Windows-container implementeren op Service Fabric op Windows Server 2016](service-fabric-get-started-containers.md)
* [Implementeert een Docker-container op Service Fabric op Linux](service-fabric-get-started-containers-linux.md)
