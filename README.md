landap-dlink
============
# Local Area Network Device Acquisition Protocol (Ver. 1.41)
#1. Protocol description
All packets transmitted in this protocol are based on the way of broadcasting UDP packet. LANDAP servers (IP-camera or video server) receive packet with port number: 0xF600 (62976).
#2.1 Message Formats
>**Two message types:**
>>**Request Message:** The packet sent by LANDAP client to request server doing a specific function.

>>**Reply Message:** The packet responded by LANDAP server regard with the request from client.

Any message consists of with two parts, header and body.

**\*Note**: All value in LANDAP (include the header and the body) are using “little endian” format.

**Header:** fixed 22 bytes as the following

Identifier|Sequence|OP code|MAC address|IP address|Req mode/RetCode|PVer  |Body Len|
----------|--------|-------|-----------|----------|----------------|------|--------|
0(2b)     |2(2b)   |4(2b)  |6(6b)      |12(2b)    |16(2b)          |18(2b)|20(2b)  |

- **Identifier:** Identify packet which is Request (0xFDFD) or Reply (0xFEEE)
- **Sequence:**
- **OP code:**
- **Req mode/RetCode:**
  - **Request Message (Req mode):**  
    - 0x0000: request for all camera:
      - when MAC is FF FF FF FF FF FF and IP is 00 00 00 00, all nodes should reply (Only valid for „Discovery‟ command(OP code is 161));
      - when MAC is some camera MAC, only the camera should reply.
    - 0x7777: request for a camera specified by MAC address.
  - **Reply Message (Ret Code):**
    - 0: the result is successful;
    - 1: the result is failed;
    - 2: fail in authorization;
- **PVer :** the LANDAP messages version which is currently 0x0001;
- **Body Len:** the data length of the Body


