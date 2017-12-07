---
title: Azure Service Fabric-container beveiliging | Microsoft Docs
description: Ontdek hoe u veilige containerservices.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 7c18c2b3b0d271f2dbe4f247c132850b49d8f1d9
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="container-security"></a>Beveiliging van de container

Service Fabric biedt een mechanisme voor services binnen een container voor toegang tot een certificaat dat is geïnstalleerd op de knooppunten in een Windows- of Linux-cluster (versie 5.7 of hoger). Service Fabric ondersteunt bovendien ook gMSA (groep beheerde serviceaccounts) voor Windows-containers. 

## <a name="certificate-management-for-containers"></a>Certificaatbeheer voor containers

U kunt uw containerservices beveiligen door te geven van een certificaat. Het certificaat moet worden geïnstalleerd in de hoofdmap van LocalMachine op alle knooppunten van het cluster. Gegevens van het certificaat is opgegeven in het toepassingsmanifest onder de `ContainerHostPolicies` label als het volgende codefragment bevat:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

Voor windows-clusters, wanneer de toepassing wordt gestart, wordt de runtime leest de certificaten en een PFX-bestand en het wachtwoord voor elk certificaat gegenereerd. Deze PFX-bestand en dit wachtwoord zijn toegankelijk in de container met de volgende omgevingsvariabelen: 

* **Certificates_ServicePackageName_CodePackageName_CertName_PFX**
* **Certificates_ServicePackageName_CodePackageName_CertName_Password**

Voor Linux-clusters, de certificates(PEM) gewoon gekopieerd uit de store naar de container door X509StoreName is opgegeven. De bijbehorende omgevingsvariabelen op linux zijn:

* **Certificates_ServicePackageName_CodePackageName_CertName_PEM**
* **Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey**

U kunt ook als u al hebt de certificaten in de vereiste vorm en gewoon willen zou toegang tot het in de container, u kunt een gegevenspakket in uw apppakket maken en geeft u de volgende binnen uw toepassingsmanifest:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
   <CertificateRef Name="MyCert1" DataPackageRef="[DataPackageName]" DataPackageVersion="[Version]" RelativePath="[Relative Path to certificate inside DataPackage]" Password="[password]" IsPasswordEncrypted="[true/false]"/>
 ```

De containerservice of het proces is verantwoordelijk voor het importeren van het certificaatbestand in de container. Om het certificaat importeert, kunt u `setupentrypoint.sh` scripts of het uitvoeren van aangepaste code in het proces van de container. Hier volgt de voorbeeldcode in C# voor het importeren van het PFX-bestand:

```c#
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


## <a name="set-up-gmsa-for-windows-containers"></a>Instellen van gMSA voor Windows-containers

Voor het instellen van gMSA (groep beheerde serviceaccounts), een referentie specification-bestand (`credspec`) op alle knooppunten in het cluster is geplaatst. Het bestand kan worden gekopieerd op alle knooppunten met behulp van een VM-extensie.  De `credspec` -bestand moet bevatten de gegevens van de gMSA-account. Voor meer informatie over de `credspec` bestand, Zie [serviceaccounts](https://github.com/MicrosoftDocs/Virtualization-Documentation/tree/live/windows-server-container-tools/ServiceAccounts). De referentie-specificatie en de `Hostname` code zijn opgegeven in het toepassingsmanifest. De `Hostname` label moet overeenkomen met de naam van de gMSA-account die compatibel is met de container.  De `Hostname` code ervoor dat de container zichzelf te verifiëren bij andere services in het domein Kerberos-verificatie gebruiken.  Een voorbeeld voor het opgeven van de `Hostname` en de `credspec` in de toepassing manifest wordt weergegeven in het volgende fragment:

```xml
<Policies>
  <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="process" Hostname="gMSAAccountName">
    <SecurityOption Value="credentialspec=file://WebApplication1.json"/>
  </ContainerHostPolicies>
</Policies>
```
## <a name="next-steps"></a>Volgende stappen

* [Een Windows-container implementeren op Service Fabric op Windows Server 2016](service-fabric-get-started-containers.md)
* [Implementeert een Docker-container op Service Fabric op Linux](service-fabric-get-started-containers-linux.md)
