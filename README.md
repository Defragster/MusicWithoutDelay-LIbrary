# MusicWithoutDelay Library(v3.0.0)
* [Quick start here](https://github.com/nathanRamaNoodles/MusicWithoutDelay-LIbrary#quick-start)
* This library was inspired by [Bhagman's RTTL Arduino sketch](https://github.com/bhagman/Tone/blob/master/examples/RTTTL/RTTTL.pde) and [Dzlonline's synth engine](https://github.com/dzlonline/the_synth)

## Compatibilty
This library should work for the following boards:

* Arduino Uno
* Nano
* ~~Mega2560~~(broken since 2011)
* Pro Mini
* All Teensy boards

## Updates
* This library has been upgraded to version 3.0.0
  * previous versions [found here](https://github.com/nathanRamaNoodles/MusicWithoutDelay-LIbrary/releases).
* Improvements
  * Added Volume control :)
  * Added Teensy Support
  * Option to Override Sustain for notes. **Note: you must call `overrideSustain`** otherwise instrument will choose automatically.
    * Default is `SUSTAIN`
    * Reverse effect `REV_SUSTAIN`
    * Slur effect `NONE`
  * Fixed issue with **"+"** sign in RTTL format.
    * Using a `+` instead of a `,`, will give a slur sound effect.
  * more to come...

## Demonstration

* v1.0.0 demo
  * Check out [this video](https://youtu.be/uoHhlrqZYDI) for a Legend of Zelda music demonstration.  In the video, an Arduino Nano outputs two voices. The Arduino uses a vibrating motor as a percussion instrument, and an RGB LED to add some fire to the show.  The delay function is not used at all :)
* v3.0.0 demo
  * coming soon...

## Advantages
* This Arduino Library makes an Arduino board play music in the background while running your program(Assuming your code doesn't have any delay).  
  * **So you can view the Serial Monitor, display stuff on an OLED, and read buttons while this library plays your music in the background**
  * This library doesn't use the delay function; it uses a similar technique to [Arduino's BlinkWithoutDelay sketch](https://www.arduino.cc/en/Tutorial/BlinkWithoutDelay).
* You do not need any shield or extra hardware for this library.
* **You can play 4 notes at the same time**
* Modulation effects added
* **Volume Control**
* Pause/resume Song
* Play song forward and backwards!! :D
* skipTo a favorite part in the song(The hardest to program, but it was worth it)
* Sustain effects

## Disadvantages(Soon to be fixed)
* ~~Can't play more than two notes on many arduino boards.~~
* ~~Tone library creates timer conflicts with Servo library~~
* ~~I am trying to move away from the Tone library because many common libraries hate it.~~ :D
* Not working on Arduino Mega 2560.
* ~~No volume control, yet~~.

## Quick Start
1. Install this library by downloading the zip folder.  Read [this Arduino Library Tutorial](https://www.arduino.cc/en/Guide/Libraries) to install the library.
2. Open the examples folder after installing the library.  
3. Choose the "Basics" sketch.
4. Attach a speaker as per the schematic below.
5. Upload the code.
6. The comments in the code should be helpful.  And try the other examples included with this library.

![alt text](https://raw.githubusercontent.com/nathanRamaNoodles/MusicWithoutDelay-LIbrary/master/MusicWithoutDelay.png "Schematic")

# How to write Music

You may be wondering how one can write songs for the Arduino without any knowledge of Music!  Its actually quite interesting and fun.  First you need to know the value of the notes in Music.
![alt text](http://ezstrummer.com/ezriffs/demo/notes_rests.gif "Note Values")

Triplets= 1/3

Now, remember these values.
You will need them in the next step.

## The Arduino Code

Now that you know the values of the basic notes in music, let's make some noise!
The Music file is stored in the `PROGMEM`.

![alt text](https://raw.githubusercontent.com/nathanRamaNoodles/MusicWithoutDelay-LIbrary/master/char%20song.PNG "storage Variable")

### Format(RTTL or Ring Tone Transfer Language)
`const char song[] PROGMEM =  " name of song : settings : notes";`
#### Name(Optional)

* The name of the Song can be set before the first semicolon.

#### The Settings(Optional)

* The settings for the song are set in-between the first and second semicolon(You may use a comma or white space between each setting option)
  * `d`= The default duration. So any note(letter) without a number in front will automatically be assigned with the default duration
    * By default, `d=1` which is a wholenote
  * `o`= The default Octave.
    * By default, `o=4` which is middle C on the Piano
  * `b`= the BPM(Beat per Minute) or the Tempo of the Song
    * By default, `b=100`, which is normal tempo in music
  * `s`= The sharps
    * Ex: `s=aeb`, means all a's, e's, and b's in the song are sharps
  * `f`= The Flats
    * Ex: `f=aeb`, means all a's, e's, and b's in the song are flats

#### Notes(Required)

* The song's notes are made after the second semicolon.
  * there are only 7 possible letters. `a,b,c,d,e,f,g`
  * A rest uses the letter `p`
* the duration of each note is made before the letter
  * ex: `4f` is a quarter note
* put a period `.` for dotted notes
  * ex: `2.f` is a dotted half note
* The duration can be determined by this formula.
  * **Formula: 4/(note value)= duration.  So an eight note would be 4/(1/2) = 8.**
* To name a letter a flat put an underscore `_` right after the letter
  * ex: `b_`
* To name a letter a sharp put a hashtag `#` right after the letter
  * ex: `c#`
* To raise a note up an octave, put the number of octaves to be raised after the Sharp/Flats/Letter
  * ex: `b1`
  * ex2: `c#1`
* To drop a note down an Octave, put a minus `-` sign and then the number of octaves to be dropped after the Sharp/Flats/Letter
  * ex: `b-1`
  * ex: `c#-2`
* Put a comma `,` to indicate the next note
  * ex: `b,c`  means that the notes will be played one after the other(taking a breath)
* Put a plus `+` sign to indicate the next note is a slur with the current note
  * ex: `b+c`  means that the notes will be played like a slur in music(without taking a breath)

So using these format rules, we can make this ordered outline for the definition of a Note.
1. duration   (optional)

2. period     (optional)

3. letter     **(required)**

4. sharp/flat (optional)

5. octave     (optional)

6. `,` or `+` **(required)**    

Let's use some examples to understand the format of the song file.  

## Examples
Assume `const char name[] PROGMEM`

1. Question: Lets try to play the letter C    
   * Answer:  `name = "::c";`

2. Question: Play the C major scale with wholenotes  
   * Answer:  `name = "::c,d,e,f,g,a,b,c1";`

3. Question: Play the C major scale with sixteenth notes
   * Answer:  `name = ":d=16:c,d,e,f,g,a,b,c1";`

4. Question: Play the C Major with different durations
   * Answer:  `name = "::c,4d,2e,16f,g,8a,32b,c1";`

5. Question: Play the C Major faster
   * Answer:  `name = ":b=160:c,d,e,f,g,a,b,c1";`

6. Question: Play some dotted quarter notes
   * Answer:  `name = "::4.c,8.d,2.e";`

7. Question: play a note every second
   * Answer:  `name = ":d=4,b=60:c,p";`  //bpm=60 means a quarter note = 1 second

8. Question: Slur every three notes
   * Answer:  `name = "::a+b+c,c+d+e";`

9. Question: Combine note with all possible options
   * Answer:  `name = "::4.a_-1";`

10. Question: Play notes with whitespaces(whitespaces make it easier to write music since it's easier on the eyes)
    * Answer:  `name = ":: 12c , 12b , 12a , 4a , 4b , 4c , 2g , 2a , d";`  //The library is user-friendly, it ignores spaces **(but don't put spaces in between the definitions of the notes)** So don't do this `":: 12 c # ,"`.

# Creating a MusicWithoutDelay Object

You may create a MusicWithoutDelay object two different ways:

```
MusicWithoutDelay buzzer(const char *song); // where song is a const char song[] PROGMEM
```
or
```
MusicWithoutDelay buzzer;
```

# Starting the speaker
In order to start our speaker, we need to call the `begin()` function.
You can call it two ways.
```
begin(int mode, int waveForm, int envelope, int mod)
```  
Where **mode** is the pin connected to the speaker.  It must be `CHA` or `CHB`.  **CHA is pin 11** on UNO, and **CHB is pin 3** on UNO.  
* `CHB` is more preferred

The **waveForm** can be `SINE`, `SQUARE`, `TRIANGLE`, `SAW`, `RAMP`, or `NOISE`
* `TRIANGLE` is recommended

The **envelope** can be `ENVELOPE0`, `ENVELOPE1`,`ENVELOPE2`, or `ENVELOPE3`.
* Default is `ENVELOPE0`


The **mod** can be any positive or negative number to create fancy musical effects.  **The further away you are from `0`, the fancier the song will sound.**
* Default is `0`.

```
begin(int waveForm, int envelope, int mod)
```
Use this format for more than one instrument.

# ✨Simple Continuous Frequency

If you don't want to play a song but want to play a simple continuous sine wave, all you have to do is call:
```
buzzer.setFrequency(440.0); // A4 is 440 hz
```
**However, if you don't know the frequency for a note, just call:**
```
float freq = buzzer.getNoteAsFrequency(int Note);
```
Where `Note` can be referenced by simply typing `NOTE_A4` for A4, or `NOTE_C4` for middle C.

# ✨**(New)** Volume Control

It's been forever, but I finally mastered the art of Timers, and with it I figured out how to control Volume.

Fortunately, setting the volume is a piece of Cake:
```
buzzer.setVolume(50);// 0-100
```
The **min volume is 0** and the **max is 100**.

# ✨**(New)** Sustain Override
So, you know how a piano's volume decreases overtime as soon as you play a note?  Well, I've applied the same principle to my library. As a bonus, I've also figured out how to make music sound like it's being played backwards!

As you know, your RTTL files contains commas `,` and plus `+` signs to seperate notes from each other; my library automatically adds sustain and removes it as your song plays.  **However, if you want the whole song to sound "backwards", you must Override the instrument!**

## How to Override Instruments:
```
buzzer.overrideSustain(true);
```
## Change Sustain type
After you have overridden the instrument, you now have the ability to choose a sustain effect.  If you choose to revert back to the original sustain, simply call `buzzer.overrideSustain(false);`
```
buzzer.setSustain(NONE);        // monotone sound
buzzer.setSustain(SUSTAIN);     // Normal sustain
buzzer.setSustain(REV_SUSTAIN); // Backwards effect
```
# Functions
```
begin(int mode, int waveForm, int envelope, int mod)//mode can be CHA or CHB  
begin(int waveForm, int envelope, int mod)
--------------------------------------------------------
update()          //update the instruments
play()            // plays the song infinitely
play(int numOfTimes)//repeat a song a certain amount
pause(bool b)           //pauses and resumes song
reverse(bool r)         //reverse direction of song being
played
overrideSustain(bool v); // must be called in order to use setSustain()
newSong(const char p[] PROGMEM)  //used to play a new song
skipTo(long index)//skips song to time suggested in milliseconds
mute(bool m)            // mutes instrument
--------------------------------------------------------
setBPM(int tempo)  //set speed of song
setOctave(int oct) //set octave
setWave(int waveShape) //set waveShape of instrument
setMod(int percent)    //modulate instrument for cool effects
setSustain(int sustain) //change how a note sounds at the front or end(but u must call overrideSustain(true))
setFrequency(float freq);//stops song, and plays a single frequency
--------------------------------------------------------
getTotalTime()    //gets totalTime of song in milliseconds
getCurrentTime()  //gets currentTime of song in milliseconds
getBPM()          //gets tempo of song
getOctave()       //gets the song's octave
getName()         //get name of song
getNoteAsFrequency(int n);//returns a Note as a frequency
--------------------------------------------------------
isRest()          //returns true if song is playing a rest
isMuted()         //returns true if muted
isStart()         //returns true if song has reached the beginning
isEnd()           //returns true if song has reached the end
isPaused()        //returns true if song is paused
isNote()          //returns true if song is playing a note
isBackwards()     //returns true if song is playing backwards
isSustainOverrided() // returns true if the current instrument's sustain is overridden.
isSingleNote();    //returns true if instrument is playing the frequency you set from "setFrequency(float freq)"
```
# Constants:
```
//Use these constants for setting wave shapes
SINE
SQUARE
SAW
RAMP
TRIANGLE
NOISE
--------------------------------------------------------
//Use these constants for setting the speaker pin
CHA  //pin 11 on Uno
CHB  //pin 3 on Uno
DIFF //Both pin 11 and 3 on Uno
--------------------------------------------------------
//Use these constants for setting the type of Envelope
ENVELOPE0
ENVELOPE1
ENVELOPE2
ENVELOPE3
--------------------------------------------------------
//Use these constants for setting the type of sustain for the instrument.
SUSTAIN     // Default, like how a piano plays
REV_SUSTAIN // Sounds like music is played backwards
NONE        // Boring, and classic Arduino monotone slur.
```
