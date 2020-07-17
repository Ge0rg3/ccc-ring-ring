<redacted_email>

# Ring Ring!

## Flag
Flag: flag{we_love_steg}

## Briefing
We received a strange call from an ally, and believe they sent over a flag. Take a look at the pcap and see what you can find.

By g30rg3.

## Infrastructure
N/A - only a static file.

## Risks
N/A - only a static file.

## Walkthrough
The user first opens the pcap in Wireshark and identifies it as a voice call by the RTP packets.
They can first look at the call metadata, but there is nothing to be seen there.

In Wireshark, you can listen to a VOIP call natively by selecting Telephony -> Voip Calls -> Play Streams.
The user hears two rings, then some a voice that reads "I heard the sender say something about transcoding Steganography. I wonder if this may help."
They should also notice that the third ring looks and sounds very different to the other 3.

A Google search for Transcoding Steganography reveals a whitepaper and some research on hiding data within VOIP calls by reducing the VOIP payload size and inserting the hidden data in its place.

Going to the packet at the start of the third ring (~380) will show a PNG header in the RTP Payload data. Scrolling through the following packets sent from that same address show different PNG header chunk names. From this, the user can see that the PNG is hidden in the end of the RTP payloads from this position.

The user can then extract these trailing values from the remaining packets until the PNG footer chunk is found. This can be done either automatically, or manually (the png only takes up around 20 packets). The PNG will then reveal the flag.
