---
title: Chapter 1 - Introduction to the NetX Duo Point-to-Point Protocol (PPP)
description: This chapter introduces the NetX Duo Point-to-Point Protocol (PPP) module.
---


Typically, NetX Duo applications connect to the actual physical network through Ethernet. This provides network access that is both fast and efficient. However, there are situations where the application does not have Ethernet access. In such cases, the application may still connect to the network through a serial interface connected directly to another network member. The most common software protocol used to manage such a connection is the Point-to-Point Protocol (PPP).

Although serial communication is relatively straightforward, the PPP is somewhat complex. The PPP is actually comprised of multiple protocols, such as the Link Control Protocol (LCP), Internet Protocol Control Protocol (IPCP), Password Authentication Protocol (PAP), and the Challenge-Handshake Authentication Protocol (CHAP). The LCP is the main protocol for PPP. This is where the basic components of the link are dynamically negotiated in a peer-to-peer fashion. Once the basic characteristics of the link have been successfully negotiated, the PAP and/or CHAP are used to ensure a connected peer is valid. If both peers are valid, the IPCP is then utilized to negotiate the IP addresses used by the peers. Once IPCP completes, PPP is then able to send and receive IP packets.

NetX Duo views the PPP primarily as a device driver. The *nx_ppp_driver* function is supplied to the NetX Duo IP create function, *nx_ip_create*. Otherwise, NetX Duo does not have any direct knowledge of PPP.

## PPP Serial Communication

The NetX Duo PPP package requires the application to provide a serial communication driver. The driver must support 8-bit characters and may also employ software flow control. It is the application's responsibility to initialize the driver, which should be done prior to creating the PPP instance.

In order to send PPP packets, a serial driver output byte routine must be provided to PPP (specified in the *nx_ppp_create* function). This serial driver byte output routine will be called repetitively in order to transmit the entire PPP packet. It is the serial driver's responsibility to buffer the output. On the receive side, the application's serial driver must call the PPP *nx_ppp_byte_receive* function whenever a new byte arrives. This is typically done from within the context of an Interrupt Service Routine (ISR). The *nx_ppp_byte_receive* function places the incoming byte into a circular buffer and alerts the PPP receive thread of its presence.

## PPP Over Ethernet Communication

NetX Duo PPP also can transmit PPP message over Ethernet, in this situation, the NetX Duo PPP package requires the application to provide an Ethernet communication driver.

In order to send PPP packets over Ethernet, an output routine must be provided to PPP (specified in the *nx_ppp_packet_send_set* function). This output routine will be called repetitively in order to transmit the entire PPP packet. On the receive side, the application's receiver must call the PPP *nx_ppp_packet_receive* function whenever a new packet arrives.

## PPP Packet

PPP utilizes AHDLC framing (a subset of HDLC) for encapsulating all PPP protocol control and user data. An AHDLC frame looks like the following:

|**Flag**|**Addr**|**Ctrl**|**Information**|**CRC**|**Flag**|
|--------|--------|--------|---------------|-------|--------|
|7E |FF|03|[0-1502 bytes]|2-byte| 7E|

Each and every PPP frame has this overall appearance. The first two bytes of the information field contain the PPP protocol type. Valid values are defined as follows:

- C021:  LCP
- 8021: IPCP
- C023: PAP
- C223: CHAP
- 0021: IP Data Packet

If the 0x0021 protocol type is present, the IP packet follows immediately. Otherwise, if one of the other protocols is present, the following bytes correspond to that particular protocol.

In order to ensure unique 0x7E beginning/end-of frame markers and to support software flow control, AHDLC uses escape sequences to represent various byte values. The 0x7D value specifies that the character following is encoded, which is basically the original character exclusive ORed with 0x20. For example, the 0x03 value for the Ctrl field in the header is represented by the two byte sequence: 7D 23. By default, values less than 0x20 are converted into an escape sequence, as well as 0x7E and 0x7D values found in the Information field. Note that escape sequences also apply to the CRC field.

## Link Control Protocol (LCP)

The LCP is the primary PPP protocol and is the first protocol to run. LCP is responsible for negotiating various PPP parameters, including the Maximum Receive Unit (MRU) and the Authentication Protocol (PAP, CHAP, or none) to use. Once both sides of LCP agree on PPP parameters, the authentication protocols—if any—then start running.

## Password Authentication Protocol (PAP)

The PAP is a relatively straightforward protocol that relies on a name and password being supplied by one side of the connection (as negotiated during LCP). The other side then verifies this information. If correct, an acceptance message is returned to the sender and PPP can then proceed to the IPCP state machine. Otherwise, if either the name or password is incorrect, the connection is rejected.

> **Note:** Both sides of the interface can request PAP, but PAP is typically used in only one direction.

## Challenge-Handshake Authentication Protocol (CHAP)

The CHAP is a more complex authentication protocol than PAP. The CHAP authenticator supplies its peer with a name and a value. The peer then uses the supplied name to find a shared "secret" between the two entities. A computation is then done over the ID, value, and the "secret." The result of this computation is returned in the response. If correct, PPP can then proceed to the IPCP state machine. Otherwise, if the result is incorrect, the connection is rejected.

Another interesting aspect of CHAP is that it can occur at random intervals after a connection has been established. This is used to prevent a connection from being hijacked – after it has been authenticated. If a challenge fails at one of these random times, the connection is immediately terminated.

> **Note:** Both sides of the interface can request CHAP, but CHAP is typically used in only one direction.

## Internet Protocol Control Protocol (IPCP)

The IPCP is the last protocol to execute before the PPP communication is available for NetX Duo IP data transfer. The main purpose of this protocol is for one peer to inform the other of its IP address. Once the IP address is setup, NetX Duo IP data transfer is enabled.

## Data Transfer

As mentioned previously, NetX Duo IP data packets reside in PPP frames with a protocol ID of 0x0021. All received data packets are placed in one or more NX_PACKET structures and transferred to the NetX Duo receive processing. On transmission, the NetX Duo packet contents are placed in an AHDLC frame and transmitted.

## PPP RFCs

NetX Duo PPP is compliant with RFC1332, RFC1334, RFC1661, RFC1994, and related RFCs.