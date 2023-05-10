# A2B_learnings

### Summary + Dataframe

An A2B (Automotive Audio Bus) data frame consists of multiple fields, including synchronization, address, control, and data. The A2B protocol is a time-division multiplexed (TDM) protocol, which means that the data frame is divided into multiple time slots, each carrying data for a specific audio channel or control information.

An A2B data frame typically has the following structure:

    - Sync: A synchronization field that includes the Sync signal (or clock) to synchronize the data transmission between the master and slave devices. The Sync field is usually a dedicated signal line.
    
    - Address: The address field identifies the specific slave device the data is intended for or the origin of the data. The address is typically a 7-bit value, allowing for up to 127 unique slave addresses in an A2B network.
    
    - Control: The control field contains information used to manage the A2B bus, such as frame type, error checking, or other control signals. This field may also include information about the audio sample format, sample rate, or other audio-specific parameters.
    
    - Data: The data field contains the actual audio data transmitted between the devices. In a TDM protocol like A2B, the data field is subdivided into multiple time slots, each carrying data for one audio channel.

Here's an example of an A2B data frame with TDM8 configuration, 16-bit word length, and 32-bit frame length:

    - Word Length: Word length refers to the number of bits used to represent an audio sample. Common word lengths are 16-bit, 24-bit, and 32-bit. The word length determines the resolution of the audio.

    - Frame Length: Frame length, also known as the frame size, refers to the number of bits in a complete audio frame. In a TDM system, a frame consists of multiple audio channels (e.g., TDM8 has 8 channels) with one audio sample per channel.


``` bash

| Sync | Ch1   | Ch2   | Ch3   | Ch4   | Ch5   | Ch6   | Ch7   | Ch8   |
|------|-------|-------|-------|-------|-------|-------|-------|-------|
| 1    | 16bit | 16bit | 16bit | 16bit | 16bit | 16bit | 16bit | 16bit |

```

- Sync: represents the synchronization pulse:
    - The Sync signal will be a separate signal outside the data stream, and it is used to synchronize the data transmission.
- Ch1 - Ch8: represent the 16-bit audio data samples for each of the 8 channels. 
    - Each data frame has 8 channels (TDM8), 
    - Word length for each channel is 16 bits. 
    - Each frame has a total length of 32 bits, so there will be padding in each channel slot to fill the remaining 16 bits. 
        - The padding can be either zeros or ones, depending on the specific implementation.

### Sigma Studio Configuration:

#### A2B Main Node:
---
    - Sync Mode: 50% Duty Cycle
    - Sync Polarity: Rising Edge
    - DRXn Sampling BCLK: Rising Edge
    - DTXn Change BCLK: Falling Edge
    - TDM Channel Size: 16-bit
    - TDM Mode: TDM8
    - Sync: Enabled
    - Early Sync: Disabled (Depends on the TDF8534 or other amplifier)
        - When "Early Sync" is enabled, the frame sync signal's rising or falling edge (depending on the sync polarity setting) will be aligned with the first bit clock edge of the TDM frame. This means that the frame sync signal and the bit clock will change simultaneously at the start of the frame.
        - When "Early Sync" is disabled, the frame sync signal's edge will occur one bit clock cycle before the first bit clock edge of the TDM frame. This setting provides a one-bit clock delay between the frame sync signal and the start of the frame's data.
    - Rx Interleave: Enabled
    - Tx Interleave: Enabled

#### A2B Sub Node:
---
    - Sync Mode: 50% Duty Cycle
    - Sync Polarity: Rising Edge
    - DRXn Sampling BCLK: Rising Edge
    - DTXn Change BCLK: Falling Edge
    - TDM Channel Size: 16-bit
    - TDM Mode: TDM8
    - Sync: Enabled
    - Early Sync: Disabled (Depends)
    - Rx Interleave: Enabled
    - Tx Interleave: Enabled


