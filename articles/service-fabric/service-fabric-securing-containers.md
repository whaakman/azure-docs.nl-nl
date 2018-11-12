---
title: Certificaten importeren in een container die wordt uitgevoerd op Azure Service Fabric | Microsoft Docs
description: Ontdek hoe u certificaatbestanden importeren in een Service Fabric containerservice.
services: service-fabric
documentationcenter: .net
author: TylerMSFT
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: twhitney, subramar
ms.openlocfilehash: d49c16741f581b2ad09dc173e8380fdf77391dbe
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2018
ms.locfileid: "51299058"
---
# <a name="import-a-certificate-file-into-a-container-running-on-service-fabric"></a>Een certificaatbestand importeren in een container die wordt uitgevoerd in Service Fabric

U kunt uw containerservices beveiligen door een certificaat op te geven. Service Fabric biedt een mechanisme voor services binnen een container voor toegang tot een certificaat dat is geïnstalleerd op de knooppunten in een Windows- of Linux-cluster (versie 5.7 of hoger). Het certificaat moet worden geïnstalleerd in LocalMachine / op alle knooppunten van het cluster. Informatie over het certificaat is opgegeven in het toepassingsmanifest onder de `ContainerHostPolicies` voor labelen als het volgende codefragment bevat:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

Voor Windows-clusters, bij het starten van de toepassing, de runtime leest de certificaten en genereert een PFX-bestand en het wachtwoord voor elk certificaat. Dit PFX-bestand en het wachtwoord zijn toegankelijk in de container met behulp van de volgende omgevingsvariabelen: 

* Certificates_ServicePackageName_CodePackageName_CertName_PFX
* Certificates_ServicePackageName_CodePackageName_CertName_Password

Voor Linux-clusters, zijn de certificaten (PEM) gekopieerd uit de store naar de container door X509StoreName is opgegeven. De bijbehorende omgevingsvariabelen in Linux zijn:

* Certificates_ServicePackageName_CodePackageName_CertName_PEM
* Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey

U kunt ook als u al hebt de certificaten in het vereiste formulier en toegang in de container wilt, kunt u een gegevenspakket maken in uw app-pakket en geeft u de volgende in uw manifest van de toepassing:

```xml
<ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
  <CertificateRef Name="MyCert1" DataPackageRef="[DataPackageName]" DataPackageVersion="[Version]" RelativePath="[Relative Path to certificate inside DataPackage]" Password="[password]" IsPasswordEncrypted="[true/false]"/>
 ```

De containerservice of het proces is verantwoordelijk voor het importeren van de certificaatbestanden in de container. Als u wilt het certificaat importeert, kunt u `setupentrypoint.sh` scripts of aangepaste code in het proces van de container uit te voeren. Hier volgt de voorbeeldcode in C# voor het importeren van het PFX-bestand:

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
Dit PFX-certificaat kan worden gebruikt voor het verifiëren van de toepassing of service of beveiligde communicatie met andere services. Standaard zijn de bestanden ACLed alleen aan het systeem. U kunt ACL naar andere accounts zoals vereist door de service.

Lees de volgende stap, de volgende artikelen:

* [Een Windows-container implementeren in Service Fabric in Windows Server 2016](service-fabric-get-started-containers.md)
* [Een Docker-container implementeren in Service Fabric in Linux](service-fabric-get-started-containers-linux.md)
