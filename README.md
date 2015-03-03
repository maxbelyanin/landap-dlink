landap-dlink
============
# Local Area Network Device Acquisition Protocol (Ver. 1.41)
#1. Protocol description
All packets transmitted in this protocol are based on the way of broadcasting UDP packet. LANDAP servers (IP-camera or video server) receive packet with port number: 0xF600 (62976).
#2.1 Message Formats
**Two message types:**
  - **Request Message:** The packet sent by LANDAP client to request server doing a specific function.
  - **Reply Message:** The packet responded by LANDAP server regard with the request from client.

Any message consists of with two parts, header and body.

**\*Note**: All value in LANDAP (include the header and the body) are using “little endian” format.

**Header:** fixed 22 bytes as the following

Identifier|Sequence|OP code|MAC address|IP address|Req mode/RetCode|PVer  |Body Len|Body            |
----------|--------|-------|-----------|----------|----------------|------|--------|----------------
0(2b)     |2(2b)   |4(2b)  |6(6b)      |12(2b)    |16(2b)          |18(2b)|20(2b)  |22(variable byte)

- **Identifier:** Identify packet which is Request (0xFDFD) or Reply (0xFEEE)
- **Sequence:** Undefined
- **OP code:** operation code
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
- **Body Len:** the data length of the Body;
- **Body:** includes two types of content:
  - **Setting value structure:**  
Setting value structure consists of many fields which indicate the values of settings with different function. The property of each field consists of the name, the type and the value. The details of the definition of field are listed in each function.
  - **Authentication information:**  
Authentication information is required with all **Request Message** (except the discovery (**OPcode 161)**). Note: the maximum length of username and password is 31 character.

>>>>Base 64 encrypted (**User ID**)|Base 64 encrypted (**Password**)                                     
>>>>-------------------------------|--------------------------------
>>>>0(64b)                         |64(64b)

#2.2 Message Functions
The function is determined by **OP Code**  
**Basic functions** (all devices support):

Functions                                  |OP code    |Descriptions
-------------------------------------------|-----------|------------
3.1. Discovery                             |0x00A1(161)|Discover camera on current local area network and get its basic information
3.2. Set Basic Information                 |0x00A2(162)|Set basic setting of camera
3.3. User Verification                     |0x00A3(163)|Verify username password with specified camera
3.4. Change Administrator's ID and Password|0x00A4(164)|Change administrator‟s username or password of specified camera
3.5. Query Supported Optional Functions    |0x00A5(165)|Query the information of optional functions supported by specified camera

**Optional functions** (some devices may support):

Functions                                  |OP code    |Descriptions
-------------------------------------------|-----------|------------
3.6. Get Network Advanced Information      |0x00A6(166)|Get advanced network settings of specified camera
3.7. Set Network Advanced Settings         |0x00A7(167)|Set advanced network settings of specified camera
3.8. Get Wireless Information              |0x00A8(168)|Get wireless settings of specified camera
3.9. Set Wireless Settings                 |0x00A9(169)|Set wireless settings of specified camera
3.10. Reset Camera                         |0x00AA(170)|Reset specified camera
3.11. Reset Camera to Factory Settings     |0x00AB(171)|Reset settings of specified camera to factory settings
3.12. Get System Date-time                 |0x00AC(172)|Get system date and time of specified camera
3.13. Set System Date-time                 |0x00AD(173)|Set system date and time of of specified camera
3.14. Get Wireless Site-survey Information |0x00AE(174)|Get wireless site-survey of specified camera
3.15. Get Simple Wireless Settings         |0x00AF(175)|Get simple wireless settings of specified camera
3.16. Set Simple Wireless Settings         |0x00B0(176)|Set simple wireless settings of specified camera
3.17. Initiate Wireless Site-survey        |0x00B1(177)|Initiate camera site-survey function
3.18. Commit settings                      |0x00C0(192)|Force camera to apply the settings command being send by client previously
3.19. Start WPS procedure                  |0x00C1(193)|Initiate camera‟s WPS procedure
3.20. Polling WPS status                   |0x00C2(194)|Poll the status of WPS procedure
3.21. Get new WPS PIN Code                 |0x00C3(195)|Get a new WPS PIN Code generated by camera
3.22. Query Network Status                 |0x00C9(201)|Query network status of specified camera
