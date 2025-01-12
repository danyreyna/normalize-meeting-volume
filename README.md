# Normalize volume in online calls

Even out sound levels when some participants are very loud and some very low in Zoom, Teams, etc.

## Install BlackHole

Repository: https://github.com/ExistentialAudio/BlackHole

```commandline
brew install blackhole-2ch
```

## Add an aggregate audio device

Use `BlackHole`'s inputs and your computer speakers or headphones as a single device.
Original instructions from `BlackHole`: https://github.com/ExistentialAudio/BlackHole/wiki/Aggregate-Device

1. Open `Audio Midi Setup`.
2. Click `+` then `Create Aggregate Device`.
3. Check `Use` for your desired output, e.g. `MacBook Pro Speakers`.
4. Check `Use` for `BlackHole 2ch`.
5. Ensure `Drift Correction` is enabled for `BlackHole 2ch`.
6. Ensure your output device is listed first.
7. Optional: Rename from `Aggregate Device` to, for example, `speakers + 2ch aggregate`.
8. Optional: Label the Input and Output Channels. For example:
   - `bh in 1`
   - `bh in 2`
   - `mb out 1`
   - `mb out 2`
   - `bh out 1`
   - `bh out 2`

## Install and launch REAPER

```commandline
brew install reaper
```

## Select the aggregate device 

1. Go to `REAPER Preferences / Audio / Device`.
2. Select the aggregate device in the `Audio Device` dropdown. 

## Open the `calls.RPP` project

- It should have two tracks, one with BlackHole's Input 1 and the other with Input 2.
- Each track should be using the `calls.RfxChain` FX chain.
- Ensure each track has "Record Armed" enabled.
- Ensure each track has "Record Monitoring: ON".
- In the Master track's `ROUTE`, ensure its `Audio Hardware Outputs` is set to your speakers or headphones.
  - For example, for speakers it uses an output named "1 (MacBook Pro Spea) / 2 (MacBook Pro Spea)".

## Adjust Zoom Audio Settings

1. Redirect Zoom's output to BlackHole.
   1. Go to `Settings / Audio`.
   2. Under `Speaker`, select `BlackHole 2ch`.
2. Ensure Zoom's audio doesn't exceed 0dB in Reaper's Master Track meter to avoid distortion and clipping.
   1. Click `Test speaker`.
   2. Adjust the `Output volume`, 50% seems to be the sweet spot.

## FX chain

### ReaEQ: Low shelf and high pass

- Low shelf adds warmth to vocals, making them sound fuller.
- High pass reduces low frequency sounds to remove unwanted background noise and improve vocal clarity.

---

- Band with type: "Low Shelf"
  - Frequency (Hz): 200
  - Gain (dB): 1.0
  - Bandwidth (oct): 2.00
- Band with type: "High Pass"
  - Frequency (Hz): 100
  - Gain (dB): 0.0
  - Bandwidth (oct): 2.00

### ReaComp: Three-stage compression

- A.K.A. cascade compression.

#### Heavy compression: catch the loudest peaks

- Threshold: -30dB
- Ratio: 6:1
- Attack: 10ms
- Release: 350ms
- Auto makeup gain: enabled

#### Medium compression: general leveling

- Threshold: -16dB
- Ratio: 2:1
- Attack: 5ms
- Release: 300ms
- Auto makeup gain: enabled

#### Light compression: finer control

- Threshold: -12dB
- Ratio: 1.5:1
- Attack: 10ms
- Release: 300ms
- Auto makeup gain: enabled

### ReaEQ: High shelf

- High shelf reduces harshness and sibilance.

---

- Type: "High Shelf" enabled
- Frequency (Hz): 7000
- Gain (dB): -1.5
- Bandwidth (oct): 2.00

### ReaLimit

- Final safety net to prevent clipping.

---

- Threshold: -4dB
- Brickwall ceiling: -0.5dB
- Release: 150 dB/sec
