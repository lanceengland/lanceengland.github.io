# CIDR Cheatsheet

## Pre-requisites

Count in binary

Convert from binary to decimal

IPv4 addresses are represented with 32-bits, or 4 8-bit numbers. The numbers are seperated by a period (which is not part of the number).

number.number.number.number

10.1.0.0 (decimal)

00001010.00000001.00000000.00000000 (binary)

IP addresses represent a thing on the network. One of those 'things' is another network.

Subnets are a way to sub-divide networks into smaller networks, for security and effeciency reasons.

When creating subnets, you specific how many bits are 'locked' as the address of the network.

For example, 10.1.0.0/16 means the first 16 bits are served for the network address. Using the binary form of this (from above):

00001010.00000001.00000000.00000000 means this part is the network address -> 00001010.00000001 and this part is for the available range.00000000.00000000

The first address of a network range (the 0) is reseved for the network address, and the last address (255) is known as the broadcast address, where a packet will be delivered to every address in the network.

A common convention is to use /16 for the network (65,536 addresses) and /24 for the subnet (255 adresses).
