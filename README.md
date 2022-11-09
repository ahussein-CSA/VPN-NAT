# VPN-NAT

# VPN NAT deployment

### Prepared On:  October 2021

## Business Context
Customer's security only allows connection from a specific public IPs in their on-premise firewall. VPN established will force the traffic to come from a private ip address. Private IP address of workloads coming from Azure needs to be natted to specific public IP.

## System Overview


![image](https://media.microsoft.ghe.com/user/3415/files/7f27e604-b3c4-42fd-b0a2-35253d7d3c3f)

#### Steps:

1-Created a connection between my on-premise(home edge router) and Azure.
2- Setup the [VPN gateway NAT](https://docs.microsoft.com/en-us/azure/vpn-gateway/nat-howto)  
       My Azure vNET CIDR block is 172.16.1.0/22 , using a default subnet where my VMs are :  172.16.1.0/24
3- Created a NAT egress rule on the vpn gateway to translate my internal CIDR of the subnet 172.16.1.0/24 to 50.50.50.0/24  (screen shots below)
![image](https://media.microsoft.ghe.com/user/3415/files/7f674a0f-cf1d-4eb0-a903-da4184858624)
![image](https://media.microsoft.ghe.com/user/3415/files/5311c934-d2a7-4954-8bef-52e668c0d9da)

4- Tested the setup by accessing  home internal server through edgerouter and from logs you can see that when NAT rule is not in place , the request came from my Azure VM: 172.16.1.4 , and when the NAT rule (egress) in place it came from 50.50.50.4

![image](https://media.microsoft.ghe.com/user/3415/files/91b9612f-aa19-4f2b-a051-99dfd151e4a1)



## Technical Challenges, Lesson Learned
##### [Limitation on VPN gateway](https://docs.microsoft.com/en-us/azure/vpn-gateway/nat-howto#limitations)
* Major issue is this cannot be applied across different VNETs
      * An example create VPN connection between different VNETs that belong to different tenants within Azure.
* NAT is supported on the the following SKUs: "VpnGw2-5", and "VpnGw2AZ-5AZ".
    
## Related Material
 * [VPN gateway NAT](https://docs.microsoft.com/en-us/azure/vpn-gateway/nat-howto)  
 * [Limitation on VPN gateway](https://docs.microsoft.com/en-us/azure/vpn-gateway/nat-howto#limitations)
