---
title: Referentiepunten voor prestaties voor Azure NetApp Files | Microsoft Docs
description: Beschrijft de resultaten van de prestaties benchmark-tests voor Azure NetApp bestanden op het volumeniveau van het.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: b-juche
ms.openlocfilehash: 14081daf1f45a84bc8ad19bf0239db1281d9e624
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449507"
---
# <a name="performance-benchmarks-for-azure-netapp-files"></a>Prestatiebenchmarks voor Azure NetApp Files

Dit artikel beschrijft de resultaten van de prestaties benchmark-tests voor Azure NetApp bestanden op het volumeniveau van het. 

## <a name="sample-application-used-for-the-tests"></a>Voorbeeldtoepassing die wordt gebruikt voor de tests

Prestatietests zijn uitgevoerd met een voorbeeldtoepassing met behulp van Azure NetApp bestanden. De toepassing heeft de volgende kenmerken: 

* Een Linux-gebaseerde toepassing die is gebouwd voor de cloud
* Kan worden lineair geschaald met extra virtuele machines (VM's) om te verhogen compute-kracht, indien nodig
* Snelle toegankelijkheid van de data lake is vereist
* I/o-patronen die soms willekeurige en soms sequentiële heeft 
    * Een willekeurige patroon is de lage latentie voor grote hoeveelheden i/o vereist. 
    * Een opeenvolgend patroon is vereist voor grote hoeveelheden bandbreedte. 

## <a name="about-the-workload-generator"></a>Over de generator werkbelasting

De resultaten zijn afkomstig van Vdbench samenvatting bestanden. [Vdbench](https://www.oracle.com/technetwork/server-storage/vdbench-downloads-1901681.html) is een opdrachtregelprogramma waarmee schijf-i/o-werkbelastingen voor het valideren van de prestaties van opslagruimte worden gegenereerd. De configuratie van de client-server gebruikt, is schaalbaar.  Het bevat een enkele gemengde master/client en 14 toegewezen client VM's.

## <a name="about-the-tests"></a>Over de tests

De tests zijn ontworpen voor het identificeren van de limieten die mogelijk van toepassing zijn op de voorbeeldtoepassing en de reactietijd die curven tot aan de limieten.  

De volgende tests zijn uitgevoerd: 

* 100% 8-KiB willekeurige lezen
* 100% 8 KiB willekeurig schrijven
* 100% 64 KiB sequentieel lezen
* 100% 64 KiB sequentiële schrijfbewerkingen
* 50% 64 KiB sequentieel lezen, 50% 64 KiB sequentiële schrijfbewerkingen
* 50% 8-KiB willekeurige lezen, 50% 8 KiB willekeurig schrijven

## <a name="bandwidth"></a>Bandbreedte

Azure Files met NetApp biedt meerdere [serviceniveaus](azure-netapp-files-service-levels.md). Elk serviceniveau biedt een andere hoeveelheid bandbreedte per TiB ingerichte capaciteit (volume quotum). De limiet voor bandbreedte voor een volume wordt ingericht op basis van de combinatie van het serviceniveau en het volume-quotum. De limiet voor bandbreedte is slechts één factor bij het bepalen van de werkelijke hoeveelheid doorvoer die zal worden gerealiseerd.  

4\.500 MiB is momenteel de hoogste doorvoer die door een workload op basis van een enkel volume in de testfase is bereikt.  Met het serviceniveau Premium, wordt een volume-quotum van 70.31 TiB voldoende bandbreedte om te realiseren van deze doorvoer per de berekening van de onderstaande inrichten: 

![Bandbreedte-formule](../media/azure-netapp-files/azure-netapp-files-bandwidth-formula.png)

![Quotum en het serviceniveau](../media/azure-netapp-files/azure-netapp-files-quota-service-level.png)

## <a name="throughput-intensive-workloads"></a>Doorvoer-intensieve workloads

De test doorvoer gebruikt Vdbench en een combinatie van 12xD32s V3 storage-VM's. De voorbeeld-volume in de test bereikt de volgende doorvoerwaarden:

![Doorvoer testen](../media/azure-netapp-files/azure-netapp-files-throughput-test.png)

## <a name="io-intensive-workloads"></a>I/O-intensieve workloads

De i/o-test gebruikt Vdbench en een combinatie van 12xD32s V3 storage-VM's. De voorbeeld-volume in de test bereikt de volgende i/o-getallen:

![I/o-test](../media/azure-netapp-files/azure-netapp-files-io-test.png)

## <a name="latency"></a>Latentie

De afstand tussen de test-VM's en het Azure NetApp Files-volume heeft een invloed op de i/o-prestaties.  De onderstaande tabel vergelijkt de IOPS-waarde ten opzichte van latentie respons-curven voor twee verschillende sets virtuele machines.  Een set van virtuele machines is in de buurt van Azure NetApp bestanden en de andere set verdere is afwezig.  De latentie voor de verdere set van virtuele machines heeft een invloed op de hoeveelheid IOP's op een bepaald niveau van parallelle uitvoering behaald.  Leesbewerkingen op basis van een volume kunnen ongeacht 300.000 IOPS overschrijden, zoals hieronder weergegeven: 

![Latentie van onderzoek](../media/azure-netapp-files/azure-netapp-files-latency-study.png)

## <a name="summary"></a>Samenvatting

Latentie van gevoelige werkbelastingen (databases) kunnen een reactietijd van één milliseconde hebben. De transactionele uitvoering kan meer dan 300 kB IOPS zijn voor één volume.

Doorvoer-gevoelige-toepassingen (voor het streamen en imaging) kunnen hebben 4.5GiB / s-doorvoer.

## <a name="example-scripts"></a>Voorbeeldscripts

De volgende voorbeeldscripts zijn voor demonstratiedoeleinden doel alleen.  Ze zijn niet moet worden gebruikt voor productiedoeleinden.  

    #
    #This script makes the following assumptions about the environment
    #VM Naming Convention:
    #   VM naming convention: vdbench-vnet1-1 .. vdbench-vnet1-x
    #
    #VM Count:
    #   The script is written for 24 VM's, 
    #   If you wish to test with a different number of vm's replace {1..24} with {1..Some Number}
    #
    #Volume mount point:
    #    The volumes in this script are mounted at the following mount points on all virtual machines
    #    /mnt/vdb1 ... /mnt/vdb6
    #   
    #Virtual machines must have
    #   make sure that vdbench is present on all vms in the environment and that java has been installed on each vm as well as nfs-utils
    #
    #The following tunables were configured on all virtual machines in the environment
    #    No special tunables were used to extract the results identified in this paper.
    #    Even rsize and wsize were left at the default (64K)
    #
    #Special Notes (thread counts) 
    #   You can hone in on the amount of I/O you hope to achieve in your testing using littles law,
    #   Run each test with a thread count of 1 to determnine the best possible latency.
    #   Thread count equals latency in seconds * desired I/O rate.  
    #   If you find a minimum latency of 1.14ms for example, divide that by 1000 to convert 1.3ms to .0013 seconds.
    #   
    #   If your goal from a single machine is for example 3,500 IOPS, a thread count of 4.0 is required.
    
    #   thread count 4.0 == .0013s * 5,000ops
    #
    #    
    #Special Notes: Increased window size:
    #   As round-trip time increases; corresponding increases are required in TCP Window Size – think Littles law as shown above. 
    #   Check the values of the following and adjust as needed: net.core.rmem_max, net.core.rmem_default, net.ipv4.tcp_rmem
    #
    #Run the utility like this:
    #    vdbench -f vnet1-SeqMix-workload
    #
    #When the utility is finished its run, grep 'avg' from the summary file to see overall run performance.
     
    
    
    #LUN CONFIG FILES
    for vnet in 1; do
        echo "sd=default,size=693g" > vnet${vnet}-luns-nfs
        for vm in {1..24}; do
            for vol in {1..6}; do
                echo "sd=sd-${vm}-${vol},host=vm${vm},lun=/mnt/vdb${vol}/file-${vm}-${vol},openflags=o_direct" >> vnet${vnet}-luns-nfs
            done
        done
    done
    
    
    #HOST CONFIG FILES
    for vnet in 1; do
        echo "hd=default,jvms=1,shell=ssh" > vnet${vnet}-hosts-nfs
        for vm in {1..24}; do
            echo "hd=vm${vm},system=vdbench-vnet${vnet}-${vm},user=root" >> vnet${vnet}-hosts-nfs
        done
    done
    
    #VDBENCH WORK SCRIPTS
    for vnet in 1; do
        for pattern in FillWrite SeqRead SeqWrite RndRead RndWrite SeqMix RndMix; do
            echo "include=vnet${vnet}-hosts-nfs" > vnet${vnet}-${pattern}-workload
            echo "include=vnet${vnet}-luns-nfs" >> vnet${vnet}-${pattern}-workload
        done 
        #Fill Write File
        echo "wd=FillWrite,sd=sd*,rdpct=0,seekpct=eof,xfersize=64k" >> vnet${vnet}-FillWrite-workload
        echo "* Run Defaults" >> vnet${vnet}-FillWrite-workload
        echo 'rd=Initialize-LUNs-Full,wd=FillWrite,sd=("sd*"),iorate=max,interval=1,forthreads=1,elapsed=259200' >> vnet${vnet}-FillWrite-workload
    
        #The actual workload files - feel free to add more or use less.
        #   The thread counts shown below were used to generate the performance collateral, 
        #   feel free to change to suite your needs.
    
        echo "wd=SeqWrite,sd=sd*,rdpct=0,seekpct=0,xfersize=64k" >> vnet${vnet}-SeqWrite-workload
        echo "wd=SeqRead,sd=sd*,rdpct=100,seekpct=0,xfersize=64k" >> vnet${vnet}-SeqRead-workload
        echo "wd=RndRead,sd=sd*,rdpct=100,seekpct=100,xfersize=8k" >> vnet${vnet}-RndRead-workload
        echo "wd=RndWrite,sd=sd*,rdpct=0,seekpct=100,xfersize=8k" >> vnet${vnet}-RndWrite-workload
        echo "wd=SeqMix,sd=sd*,rdpct=50,seekpct=0,xfersize=64k" >> vnet${vnet}-SeqMix-workload
        echo "wd=RndMix,sd=sd*,rdpct=50,seekpct=100,xfersize=8k" >> vnet${vnet}-RndMix-workload
        for pattern in SeqRead SeqWrite RndRead RndWrite SeqMix RndMix; do
            echo "* Run Defaults" >> vnet${vnet}-${pattern}-workload
            #echo "rd=default,curve=(50,100),iorate=curve,warmup=60,elapsed=600,iorate=curve" >> vnet${vnet}-${pattern}-workload
            echo "rd=default,warmup=60,elapsed=600,iorate=max" >> vnet${vnet}-${pattern}-workload
            echo "* Run Definitions" >> vnet${vnet}-${pattern}-workload
        done
        echo 'rd=RndRead,wd=RndRead,sd=("sd*"),threads=25' >> vnet${vnet}-RndRead-workload
        echo 'rd=RndWrite,wd=RndWrite,sd=("sd*"),threads=20' >> vnet${vnet}-RndWrite-workload
        echo 'rd=SeqRead-4,wd=SeqRead,sd=("sd*"),threads=20' >> vnet${vnet}-SeqRead-workload
        echo 'rd=SeqWrite-26,wd=SeqWrite,sd=("sd*"),threads=26' >> vnet${vnet}-SeqWrite-workload
        echo 'rd=SeqWrite-32,wd=SeqWrite,sd=("sd*"),threads=32' >> vnet${vnet}-SeqWrite-workload
        echo 'rd=SeqWrite-40,wd=SeqWrite,sd=("sd*"),threads=40' >> vnet${vnet}-SeqWrite-workload
        echo 'rd=RndMix,wd=RndMix,sd=("sd*"),threads=25' >> vnet${vnet}-RndMix-workload
        echo 'rd=SeqMix-6,wd=SeqMix,sd=("sd*"),threads=6' >> vnet${vnet}-SeqMix-workload
        echo 'rd=SeqMix-7,wd=SeqMix,sd=("sd*"),threads=7' >> vnet${vnet}-SeqMix-workload
    done
