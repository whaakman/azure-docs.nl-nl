## Servicechaining - doorvoer via gekoppelde VNetten

Hoewel bij het gebruik van systeemroutes verkeer automatisch wordt gefaciliteerd voor uw implementatie, kunnen er gevallen zijn waarin u pakketten liever wilt routeren via een virtueel apparaat.
In dit scenario bevinden er zich twee VNetten in een abonnement; HubVNet en VNet1, zoals weergegeven in de onderstaande diagram. U implementeert Network Virtual Appliance (NVA) in VNet HubVNet. Nadat u een VNet-peering tot stand hebt gebracht tussen HubVNet en VNet1, kunt u door de gebruikers gedefinieerde routes instellen en de volgende hop naar NVA opgeven in het HubVNet.

![NVA-doorvoer](./media/virtual-networks-create-vnetpeering-scenario-transit-include/figure01.PNG)

Opmerking: omwille van de eenvoud, kunt u ervan uitgaan dat alle VNetten hier zich in hetzelfde abonnement bevinden. Maar dit werkt ook in een scenario met verschillende abonnementhouders. 

De sleuteleigenschap om doorvoerroutering in te schakelen, is de parameter 'Doorgestuurd verkeer toestaan'. Hierdoor is het mogelijk om verkeer van/naar de NVA in het gekoppelde VNet te accepteren en verzenden.  


<!--HONumber=ago16_HO4-->


