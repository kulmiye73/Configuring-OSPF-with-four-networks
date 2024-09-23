

# Configuring OSPF with four networks (Open Shortest Path First) on a Cisco Router

 		![image](https://github.com/user-attachments/assets/ee6d97e9-fb9b-4b8a-ad06-b3a774c9774b)


## I am configuring the Tacoma router to establish communication with the rest of the network.
Tacoma>
Tacoma>enable 
Tacoma #conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Tacoma(config)#int se0/2/1
Tacoma(config-if) #ip address 192.168.1.1 255.255.255.252
Tacoma(config-if) #no shut

%LINK-5-CHANGED: Interface Serial0/2/1, changed state to down
Tacoma(config-if) #cl
Tacoma(config-if) #clock r
Tacoma(config-if) #clock rate 56000
Tacoma (config-if) #exit

## I am configuring the Seattle router to establish communication with the rest of the network.

Seattle# Seattle>
Seattle>
Seattle>en
Seattle #conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Seattle(config)#int se0/1/1
Seattle(config-if) #ip address 192.168.2.2   255.255.255.252
Seattle(config-if) #no shut

Seattle(config-if) #
%LINK-5-CHANGED: Interface Serial0/1/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/1/1, changed state to up


Seattle#

Seattle#
Seattle #conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Seattle(config)#int se0/1/0
Seattle(config-if) #ip address 192.168.1.2 255.255.255.252
Seattle(config-if) #no shut

%LINK-5-CHANGED: Interface Serial0/1/0, changed state to down
Seattle(config-if) #exit

## I am configuring the Everet router to establish communication with the rest of the network.

Everet #conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Everet(config)#int se0/1/1
Everet(config-if) #ip address 192.168.1.2 255.255.255.252
Everet(config-if) #no shut

Everet(config-if) #
%LINK-5-CHANGED: Interface Serial0/1/1, changed state to up

Everet(config-if) #
%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/1/1, changed state to up



# # I am currently pinging all devices to verify network connectivity and ensure proper communication between them.

![image](https://github.com/user-attachments/assets/ac65e113-a30f-4297-b8ee-0423722fac87)

 
## Ping pc1 to pc3.

 ![image](https://github.com/user-attachments/assets/46dee34a-fa7c-40de-8214-c5b451d763cc)


# # The ping is unsuccessful because no routing protocols have been configured yet.



## To check protocol configuration and routing information on a Cisco device, you can use the following commands:
1.	### show Ip route: This command displays the routing table of the device, showing how the router is forwarding packets for different networks. It includes information about directly connected networks and routes learned through routing protocols such as OSPF, EIGRP, BGP, or static routes.
2.	### show Ip protocols: This command shows the active routing protocols configured on the device, such as OSPF, EIGRP, or RIP. It provides information about timers, network statements, and other protocol-specific configurations.
3.	### show Ip OSPF for OSPF configurations

   ![image](https://github.com/user-attachments/assets/0d98be7f-ed9f-4eed-a825-3b636bf98480)

 
## No Ip protocols and no Ip OSPF configuration


## I am now starting to configure OSPF protocols on all the routers within the same area.



Seattle #conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Seattle(config)#router ospf 10
Seattle(config-router) #network 172.16.10.0 0.0.0.255 area 0
Seattle(config-router) #network 172.16.20.0 0.0.0.255 area 0
Seattle(config-router) #network 10.10.10.0 0.0.0.255 area 0
Seattle(config-router) #network 192.168.1.0 0.0.0.3 area 0
Seattle(config-router) #network 192.168.2.0 0.0.0.3 area 0


Tacoma(config)#router ospf 10
Tacoma(config-router) #network 10.10.10.0 0.0.0.255 area 0
Tacoma(config-router) #network 172.16.10.0 0.0.0.255 area 0
Tacoma(config-router) #network 172.16.20.0 0.0.0.255 area 0
Tacoma(config-router) #network 192.168.1.0 0.0.0.3 area 0
Tacoma(config-router) #network 192.168.2.0 0.0.0.3 area 0

Tacoma(config-router) #





Everet>enable
Everet #conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Everet(config)#router ospf 10
Everet(config-router) #network 10.10.10.0 0.0.0.255 area 0
Everet(config-router) #network 192.168.1.0 0.0.0.3 area 0
Everet(config-router) #network 192.168.2.0 0.0.0.3 area 0
Everet(config-router) #network 172.16.10.0 0.0.0.255 area 0
Everet(config-router) #network 172.16.20.0 0.0.0.255 area 0
Everet(config-router) #




## I am now able to check IP protocols and routing configurations to verify the OSPF protocol setup.
## Go Seattle router and type show Ip protocols or Ip route
Seattle>
Seattle>enable
Seattle show Ip protocols

Routing Protocol is "ospf 10"
  Outgoing update filter list for all interfaces is not set 
  Incoming update filter list for all interfaces is not set 
  Router ID 192.168.1.30
  Number of areas in this router is 1. 1 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    172.16.10.0 0.0.0.255 area 0
    172.16.20.0 0.0.0.255 area 0
    10.10.10.0 0.0.0.255 area 0
    192.168.1.0 0.0.0.255 area 0
    192.168.1.0 0.0.0.3 area 0
    192.168.2.0 0.0.0.3 area 0
  Routing Information Sources:  
    Gateway         Distance      Last Update 
    192.168.1.10         110      00:16:48
    192.168.1.30         110      00:16:48
    192.168.1.41         110      00:26:29
  Distance: (default is 110)

Seattle show Ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level 1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/24 is sub netted, 1 subnets
O       10.10.10.0/24 [110/65] via 192.168.1.1, 00:27:05, Serial0/1/0
     172.16.0.0/16 is variably sub netted, 3 subnets, 2 masks
O       172.16.10.0/24 [110/65] via 192.168.2.2, 00:17:24, Serial0/1/1
C       172.16.20.0/24 is directly connected, GigabitEthernet0/0/0
L       172.16.20.254/32 is directly connected, GigabitEthernet0/0/0
     192.168.1.0/24 is variably sub netted, 2 subnets, 2 masks
C       192.168.1.0/30 is directly connected, Serial0/1/0
L       192.168.1.2/32 is directly connected, Serial0/1/0
     192.168.2.0/24 is variably sub netted, 2 subnets, 2 masks
C       192.168.2.0/30 is directly connected, Serial0/1/1
L       192.168.2.1/32 is directly connected, Serial0/1/1



## Let's ping PC1 from PC3 to verify their connectivity.

## This emphasizes the action and direction of the ping for clarity.

 ![image](https://github.com/user-attachments/assets/1c85ecb6-dada-428d-993f-fee82fd0e6bc)

![image](https://github.com/user-attachments/assets/5b3f74e2-0466-4093-9ac2-2fcb83d33ad5)

 
## The ping test was successful, which means the OSPF project was completed successfully!"







