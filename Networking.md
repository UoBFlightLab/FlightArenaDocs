# Networking

The Flight Arena has its own Local Area Network (LAN) and a set of associated wireless networks. The
Arena LAN does not have any internet access. For internet access use one of the blue colour-coded
ethernet cables which are connected to the UWE LAN. Other ethernet cables will connect you to the
Arena LAN.

## WiFi

There are two principal WiFi networks attached to the Arena LAN: `dronehub5g` and `dronehubwep`. As
the name suggests, `dronehub5g` uses the 5 GHz bands.

The `dronehub5g` network has no password. 

## Vicon

There are some important networking points for the Vicon system. Firstly, the Vicon PC **must** have
IP address `192.168.10.1`. It then acts as a pseudo-DHCP server to set the IP addresses for the 
cameras. The cameras are set to sequenctial IP addresses following the Vicon PC: *i.e.*
`192.168.10.2`, `192.168.10.3`... Therefore, the lower range of IP addresses **must** be kept clear
as Tracker will not make any attempt to avoid other devices on the network.

As of writing it should occupy the range `192.168.10.2` to `192.168.10.18` for 16 cameras

## Linux PCs

The Linux desktop PCs are assigned static IPs from `192.168.10.86` (`flying86`), `192.168.10.87` (`flying87`) and `192.168.10.88`
(`flying88`) from right to left across the desks.

## Flight Arena Server

A local Kubernetes control plane runs on another desktop machine at `192.168.10.80`, and is accessible at `flyingserver.local`

## DHCP Allocation

There exists a router attached to the network which runs the DHCP for the system. It can be accessed at `http://192.168.10.252/` with default password 

A DHCP server runs on the network allocating IPs from the range TODO to TODO
