# Max/MSP Abstraction:   
## br.stutter.b.1.3



By Brian Riordan  
[guaguanco127@gmail.com](mailto:guaguanco127@gmail.com)  
[brianriordanmusic@gmail.com](mailto:brianriordanmusic@gmail.com)  
[https://www.brianriordanmusic.com/](https://www.brianriordanmusic.com/) 
  
Repository for br.stutter.b.1.3, with all related files, can be found here: [https://github.com/guaguanco127/br.stutter.b](https://github.com/guaguanco127/br.stutter.b)  
Additional programs can be found here: [https://github.com/guaguanco127/br.max](https://github.com/guaguanco127/br.max)

Versions 1.1 through 1.3 were updated with Max 9. Version 1.0 was created with Max/MSP 8.5.6. 

## Table of Contents 

[What's New in 1.3](#whats-new-in-13)  
[What's New in 1.2](#whats-new-in-12)  
[What's New in 1.1](#whats-new-in-11)  
[About](#About)   
[What is an abstraction?](#Abstraction)  
[How To Install](#Install)  
[How To Use](#Use)  
[State outlet](#State)  
[Example Patch](#Example) 
 
 

## What's New in 1.3

- **Mix Mode is now "Thru" / "Aux" (was "Insert" / "Gate"), and the two were swapped.** In earlier versions the names were the wrong way round compared with the other br.* effects. Now "Thru" (0, the default) lets the dry signal pass while the stutter is off, and "Aux" (1) is silent until you turn the stutter on. The sound at load is the same as before: the dry signal passes while the stutter is off.
- **The Mix Mode numbers flipped.** Before, 1 passed the dry signal; now 0 does. If you send a number into the Mix Mode inlet (inlet 7), swap 0 and 1. The State outlet now reports `mode 0` for Thru.
- Everything else is unchanged.

## What's New in 1.2

- **State outlet** (abstraction only): a new last outlet sends every setting as a named message the moment it changes (`on`, `speed`, `latent`, `mode`, `size1`, `size2`, `filtertype`, `filtershape`, `freq1`, `freq2`, `resonance`, `ampmode`, `panmode`). See [State outlet](https://github.com/guaguanco127/br.stutter.b/tree/main/MaxMSP%20Abstraction#State).
- Every inlet and the L/R outlets are unchanged, so 1.2 swaps in for 1.1 without rewiring.
- **New example patch:** _br.stutter.b.example.1.2 with a demo source, messages into every inlet and a State outlet tab.
- The controls have readable names (Stutter, Retrigger, Speed, Latent, Mix Mode, Size 1, Size 2, Filter Type, Filter Shape, Freq 1, Freq 2, Resonance, Amp Mode, Pan Mode), so presets, pattr and Live's automation show them clearly.

## What's New in 1.1

- **Click-free grain switching.** Each new grain is now captured into the silent player and crossfaded in. Before, the new grain was swapped into the player you were hearing and the crossfade cancelled itself, so every re-trigger could click. Latent mode re-triggers can no longer land on top of each other.
- **Backwards playback fixed.** Negative speeds no longer click on every re-trigger, and the Filter / Amp / Pan shapes still run forward in time (Up rises, Down falls) when the grain plays backwards.
- **No ticks on short grains.** Each grain loop fades in and out over about 5 ms instead of 1% of the loop, so very short grains no longer tick on every repeat.
- **Even-sounding amp envelopes.** Amp Mode shapes now move in decibels (from -40 dB up to full level) and can't click, even on Square or Rand Step.
- **Smoother filter.** Filter sweeps are even in pitch between Freq 1 and Freq 2, and sudden jumps (Square, Rand Step, re-triggers, fast knob moves) are crossfaded so they don't pop. The filter stops using CPU when Filter Type is Off.
- **Constant-power stereo panning.** Panning no longer drops a channel at the edges: both channels are panned and folded together, and pan moves are smoothed so Alt, Rand Step and Rand Start don't click.
- **Loads the way it looks.** All menus and dials are applied as soon as the device loads, so the sound matches the controls without touching anything.
- **Simpler.** Amp Width and Spread were removed (Amp shapes are symmetric, panning always uses the full stereo field).
- **Lower CPU.**

## <a name="About"></a>About

This is an abstraction for Max/MSP that is built around the Max/MSP stutter~ object. This contains all features as br.stutter.a but with extras. This effect adds LFOs in sync with each grain that can manipulate a filter, amplitude, and panning.

For simpler version of this effect, try [br.stutter.a](https://github.com/guaguanco127/br.stutter.a) or, for a more complext version of this effect, try [br.stutter.c](https://github.com/guaguanco127/br.stutter.c)
  
**On/Off:** When Stutter is turned on, the signal is interrupted and a history of the signal is repeated based on the grain size. 

**Button:** While the stutter is on, pressing the button acts like a re-trigger, capturing another grain of the live signal that was fed into the stutter at the time, regardless of what the stutter was just playing. There is no feedback option. 

**Speed:** The playback speed of the grains. 1.0 is the default and is normal speed. Negative speeds play the grains backwards. The range is a float from -32.0 and 32.0. 

**Latent Mode:** When "Latent" is on, the stutter waits to record the next grain of sound before switching to the next stutter sound. The latency is equal to the next grain size. This is a valuable feature because it captures what comes next, as opposed to what already has come and gone. Turning the latent mode off captures the previous grain size prior to pressing the re-trigger button. The default is on. 

**Mix Mode:** There are two mix modes, "Thru" and "Aux", with "Thru" being the default. "Thru" lets the dry, unaffected signal pass through while the stutter is turned off; the dry signal mutes once the stutter is turned on. This is best when the stutter sits in an effects chain. "Aux" passes no dry signal while the stutter is off, so you only hear the grains once the effect is turned on. This is useful on an auxiliary (send/return) effect track. 

**Size:** The size, in milliseconds, of the current grain that is playing. "Size 1" is compared with "Size 2" and a random size is chosen between these two parameters. The range is between 5 ms and 1000 ms. This is only a meter that tells you its size, you cannot adjust it. 

**Size 1:** The first potential size timings. "Size 1" is compared with "Size 2" and a random size is chosen between these two parameters. The range is between 5 ms and 1000 ms. The default is set to 5 ms.

**Size 2:** The second potential size timings. "Size 1" is compared with "Size 2" and a random size is chosen between these two parameters. The range is between 5 ms and 1000 ms. The default is set to 1000 ms.

**Filter Type:** Defines how the grains will be filtered based on the "Filter Shape." "Off" is the default, and the two different filter types are "Lowpass" and "Bandpass" 

**Filter Shape:** The shape of the filter envelope for the duration of each grain. "Sine" is a basic sine ton going up then down. "Up" is a upward phasing sawtooth wave. "Tri" is a triange shape that is the opposite direction of the sine tone. "Down" is the opposite of the "Up" as it is a downward phasing sawtoth wave. "Square" alternates between "Freq 1" and "Freq 2" without phasing in between. "Rand Step" randomly selects a different filter frequency and "Rand Ramp" randomly envelopes to the next filter frequency. The range of the filter is between "Freq 1" and "Freq 2" 

**Freq 1:** The first of the two frequency ranges that dictate the range of the filter shape envelope. Can be set above or below "Freq 2" 

**Freq 2:** The second of the two frequency ranges that dictate the range of the filter shape envelope. Can be set above or below "Freq 1" 

**Ressonance:** The ressonance of the filter. 0. is the default, and the range is between 0.0 and 0.85

**Amp Mode:** Defines how amplitude envelopes will be applied to each grain. Sine moves up then down, up moves from silence up to full amplitude for the duration of the grain, tri moves down then up, down starts at full amplitude then down to silence, rand step is a random amplitude for the duration of each grain, and rand ramp is an envelope that ramps up and down randomly across the duration of the grain.


**Pan Mode:** Defines how the grains will be panned in the stereo field. The different modes are sine tone, right (each grain moves from left to right), triangle, left (each grain moves from right to left), alt (each grain alternates between hard left then right), rand step (each grain starts from a random position) rand ramp (each grain pans in a random direction for its duration before moving differently each time), and rand start (each re-trigger plays from a different random panning position. 




## <a name="Install"></a>How To Install 

1. Make sure you have Max 9 installed in your computer. And, make sure you are using a Max patch that is inside of a folder.  

2. Copy and paste br.stutter.b.abs.1.3.maxpat inside of the same folder as the Max patch you are using.   

3. In the Max patch you are using, create an object called br.stutter.b.abs.1.3 

4. Alternatively, you could also create this inside of a bpatcher object and use all of the preset UI objects featured inside the abstraction. To do this, create a bpatcher object. Then, go inside of its inspector, select "choose" next to "Patcher File" and select the br.stutter.b.abs.1.3.maxpat located within the same folder as your project. 

## <a name="Use"></a>How To Use

The first two inlets are for the left and the right stereo signals. The first two outlets are the left and right outputs; the third is the State outlet.

Every control has its own inlet. Sending a value to an inlet moves its on-screen control too, so the display always matches the sound. Hover over an inlet in Max to see the same information.

| Inlet | Control | Type | Range | Default |
|---|---|---|---|---|
| 1 | Left audio in | Signal | | |
| 2 | Right audio in | Signal | | |
| 3 | Stutter On/Off | Int | 0 = Off, 1 = On | 0 |
| 4 | Retrigger | Bang |  |  |
| 5 | Speed | Float | -32 - 32 | 1 |
| 6 | Latent Mode | Int | 0 = Off, 1 = On | 1 |
| 7 | Mix Mode | Int | 0 = Thru, 1 = Aux | 0 |
| 8 | Size 1 | Float | 5 - 1000 ms | 5 |
| 9 | Size 2 | Float | 5 - 1000 ms | 1000 |
| 10 | Filter Type | Int | 0 = Off, 1 = Lowpass, 2 = Bandpass | 0 |
| 11 | Filter Shape | Int | 0 = Sine, 1 = Up, 2 = Tri, 3 = Down, 4 = Square, 5 = Rand Step, 6 = Rand Ramp | 0 |
| 12 | Freq 1 | Float | 40 - 20000 Hz | 80 |
| 13 | Freq 2 | Float | 40 - 20000 Hz | 8000 |
| 14 | Resonance | Float | 0 - 0.85 | 0 |
| 15 | Amp Mode | Int | 0 = Off, 1 = Sine, 2 = Up, 3 = Tri, 4 = Down, 5 = Square, 6 = Rand Step, 7 = Rand Ramp | 0 |
| 16 | Pan Mode | Int | 0 = Off, 1 = Sine, 2 = Right, 3 = Tri, 4 = Left, 5 = Alt, 6 = Rand Step, 7 = Rand Ramp, 8 = Rand Start | 0 |

| Outlet | Output | Type |
|---|---|---|
| 1 | Left Out | Signal |
| 2 | Right Out | Signal |
| 3 | State | Messages: `<name> <value>` (see [State outlet](#State)) |

**Upgrading from 1.0:** the Amp Width and Pan Spread inlets were removed, so Pan Mode moved from inlet 17 to inlet 16. Reconnect anything that was patched into the last two inlets.

## <a name="State"></a>State outlet

The last outlet sends the current settings as named messages the moment they change, for example `on 1`, `speed -1.`, `size1 50.`. Clicking a control, numbers into the inlets and preset recalls all show up; repeats are filtered out. Use it to keep a display, Mira or another patch in sync. Pick them out by name with [route on speed latent mode size1 size2 filtertype filtershape freq1 freq2 resonance ampmode panmode], not by position, so your patch keeps working if a later version adds controls.

| Name | Control | Values |
|---|---|---|
| on | Stutter | 0 = Off, 1 = On |
| speed | Speed | -32 - 32 |
| latent | Latent | 0 = Off, 1 = On |
| mode | Mix Mode | 0 = Thru, 1 = Aux |
| size1 | Size 1 | 5 - 1000 ms |
| size2 | Size 2 | 5 - 1000 ms |
| filtertype | Filter Type | menu index (see the inlet table) |
| filtershape | Filter Shape | menu index |
| freq1 | Freq 1 | 40 - 20000 Hz |
| freq2 | Freq 2 | 40 - 20000 Hz |
| resonance | Resonance | 0 - 0.85 |
| ampmode | Amp Mode | menu index |
| panmode | Pan Mode | menu index |

Retrigger is a button, not a setting, so it is not reported. Menus report their index.

## <a name="Example"></a>Example Patch

Open _br.stutter.b.example.1.3.maxpat (keep it in the same folder as the abstraction). Turn on the audio with the toggle, then raise the gain slider, which starts muted.

- **Source:** the demo saw plucks (220 Hz left, 330 Hz right) start when the patch opens; turn on the mic / line in 1 + 2 toggle to use your own sound.
- **Stutter:** turn it on (panel or toggle), press Retrigger for new grains, try the Speed and Size messages.
- **Filter / Amp / Pan:** try the Filter Type, Amp Mode and Pan Mode messages while it stutters.
- **State outlet tab:** the numbers follow every setting as you change it on the panel or with the messages.

## Version History  

Version 1.3 (10-09-2026) renamed Mix Mode to Thru / Aux and fixed its swapped values (Thru, 0, is now the default).  
Version 1.2 (10-09-2026) added a State outlet and an example patch to the abstraction, and readable control names.  
Version 1.1 was updated with Max 9 (see What's New in 1.1).  
Version 1.0 was created with Max/MSP 8.5.6.

## <a name="Credits"></a>Credits

Built around stutter~ (Cycling '74).
