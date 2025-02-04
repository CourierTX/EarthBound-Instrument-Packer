# EBMusEd tutorial ideas:
## Welcome!
Hello! vince94 here.

This tutorial series will come to you in multiple parts in this playlist. Click here to go to it!

As time goes on, I'd like to swap in new versions of the videos in here if need be. I'm imagining someone in the comments asking a really good question, or me from the future realizing that something could have been worded in a way that's easier to understand.

Also, EBMusEd itself is still being worked on by the community, and people have expressed interest in possibly even changing how it works for better integration with CoilSnake, the 

Well, I hope you enjoy!

## How Audio on the SNES works
The SNES has eight channels, which means that eight voices can be playing at once, including sound effects which game engines usually reserve for the last two.

The samples are small, carefully-made waveforms, compressed into BRR files, and loaded into audio memory.

The sound engine, pattern data, and the echo buffer shares the same block of memory, and if you use too much echo, it eats your instruments and sounds like this.

Making BRRs will be covered in a future video.

## How CoilSnake works
- What it is
- What Base ROMs are
- Decompiling
- Compiling a new ROM

## Enabling the debug menu

To play your song in an emulator, or on actual hardware if you have a flashcart, you'll want to enable the debug menu. To do this:

(ROM offsets to edit)

Alternately, if you're using CoilSnake, you can put the following text in any CCScript file and it'll get compiled into the game.

## Workflow

Here are two use cases I can think of for someone who wants to use EBMusEd.

1. Making a standalone song, resulting in an SPC file, or a WAV file recorded from the song playing back in-game
1. Making a song for a ROM hack, which will need to be inserted into a ROM generated by CoilSnake

If you're just interested in making a standalone song, then you probably won't need to worry about some of the stuff in this video. Here is the workflow for _that_ case.

> Header: Making a standalone song

1. If you're using a custom instrument pack, compile your CoilSnake project with the pack file in it. CoilSnake will output a ROM with the custom instruments in it. If you just want to use the instruments that are already in the game, use an unmodified ROM. 
1. Open the ROM in EBMusEd, and point it to a copy of the ROM called Reference.smc (TODO: Explain what reference ROMs are elsewhere)
1. Make the song.
1. Save your changes and open the ROM in an emulator. Go to the Sound Test, and use the +Directional Pad to go to the right Song Table entry. Press the A Button to play the song. Use trial and error until you're happy with how it sounds!
1. Once you're done, export the song to an .EBM file for backup, just in case. Put the instrument packs you use in the filename so you don't forget!

> Header: Making songs for an EarthBound ROM hack

- So this is where things get a bit more complicated. It works, though.
- ROM hacks are compiled via CoilSnake, and if you make your song in the ROM it exports, it'll get wiped if you compile the hack again. You need to insert the song in the Base ROM. Here's a good workflow for this.

1. Compile your CoilSnake project. It should spit out a ROM. If there are any custom instruments in it, then you'll be able to use them.
1. Make a copy of the ROM it spits out and call it "Composition ROM". This is to seperate **the place where you work on the song** and **the place that CoilSnake spits ROMs out** overwriting everything
1. Open it in EBMusEd and make the song, listening in emulator as usual to make sure it sounds good
1. Export the song to a file.
1. Open the Base ROM, and _import_ that file.
1. Compile your CoilSnake project again. With the song in the Base ROM, it will now be available for you to use in the hack! Be it via the debug menu, in a battle, or with the CCScript music() command in the middle of some dialog text.

## Note Lengths
One of the worst hurdles to learning EBMusEd is understanding the concept of note lengths.

In trackers, everything is the same length, and in MIDI it's very visual so you can just drag notes around without worrying about stuff like that.

Here, you have to specify note lengths. It's not too bad once you get used to it though.

Like most of the commands in EBMusEd (and in retro programming in general), note length commands are in hexadecimal. If you don't know what hex is, it's a special way of writing numbers that computers use. It goes like 1 2 3 4 5 6 7 8 9 A B C D E F 10 11 12 13 14 15 16 17 18 19 1A 1B 1C 1D 1E 1F 20 21 22, and so on. This may seem super weird already, but like a lot of things, you get used to it surprisingly quickly 'cause you have use it so often.

The format of a note length command is `[number of ticks] [optional release time & volume descriptor]`

One example is `[18]`

Another example is `[18 7F]`. This sets the note length to 18, and sets the release time and note volume to full.

Another example is `[18 0F]`. This sets the note length to 18, sets the release time to 0 (makes it really staccato), and sets the volume to full.

The first part is release time, and it goes from 0 to 7. The second part is the volume, and it goes from 0 to F.

Messing around with this optional part of a note length can make things sound a little more human, or interesting.

(Show an example from the vanilla game)

A big part of what I do is starting with a small value like 06, and then compressing as I go.

To compress note lengths, add them up with a hex calculator. Every windows computer comes with one, press start and type Calculator. Then click the menu and choose Programmer mode.

To compress these four notes, multiply the length of 06 by four, to get 18!

(continue with the rest of the channel)

As you can see, once you set the note length, each note afterwards will play at that length. To avoid this, I'm going to put an 06 after this stuff so it still looks the same.

## Inserting Notes

To insert notes, either use the keyboard or PK Piano.

(examples of both)

PK Piano is a basic tool I made that gives you auditory feedback and copies the hex code into your clipboard so you can paste it in.

You can also use a MIDI keyboard if you own one.

C9 - Rest

C8 - Continue previous note

Don't use Ctrl-S - It saves, then thinks you're typing S and inserts a note. Oops!

### Subroutines
To create a subroutine, (show the menu dropdown)

Here's an example of the syntax you use when referring to subroutines in the hex box: *0,2

This means "repeat subroutine 0 two times"

Subroutines are extremely useful for compressing drum stuff that repeats a lot, or repeated melodies.

(make an echo channel)

Make sure each channel ends on the same tick, otherwise it glitches up in-game!

Whenever I have to deal with this problem, I usually copy all of the notes that are in the subroutine, paste them here, and just manually shorten the last note so it doesn't go over.

## Patterns
The `Add` button adds a new pattern.

The `Insert` button inserts a duplicate of the currently-selected pattern.

The `Delete` button deletes all instances of the currently-selected pattern - WATCH OUT!

- This is not how these kinds of programs behave, and it's easy to accidentally lose a lot of work if you don't save often (which of course you should).

`Loop Song` - this is where you put the number of times you want the song to loop. `255` is the value you usually want, it makes it play infinitely.

`Position` is the frame you want it to jump back to. Some of EarthBound's songs like to set global volume and instruments and such in an extremely short first frame, and then jump back to the second frame. The reason for this is that in-game, you can slowly fade out the volume (like at the start of a cutscene), and if one of these fades is happening and it hits the global volume command, it cancels out the fade, but the song still abruptly stops.

Make sure each channel in a pattern is exactly the same length, like I said in the subroutines video.

## Effects

Everything here can be found in the Code List. I reccomend making your own code list with the effects you use the most!

### Common effects
[E7 tempo] - Set tempo

[E5 volume] - Set Global Volume

[E0 XX] - Set current instrument

[E1 XX] - Panning

[ED volume] - Set channel volume

[E3 start speed range] - Vibrato

[E4] - Vibrato Off

[EB start speed range] - Tremolo

[EC] - Tremolo off

[FA instrument] - Set first drum instrument (this one probably deserves its own video)

### Portamento effects
[F1 start length range] - Portamento on (note -> note+range)

[F2 start length range] - Portamento on (note-range -> note)

[F3] - Portamento off

[F9 start length note] - Note-based portamento

### Delay effects
[F5 channels lvol rvol] - Delay 1

[F7 delay feedback filter] - Delay 2

[F6] - Delay off

### Sliding effects
[EE time volume] - Slide channel volume

[E2 time panning] - Slide channel panning

[E8 time tempo] - Slide tempo

[F8 time lvol rvol] - Slide echo volume

[E6 time volume] - Slide global volume

### Uncommon effects
[EA transpose] - Channel transpose

[E9 transpose] - Global transpose

[F4 finetune] - Set finetune - only useful for BRRs where you messed up the tuning by a tiny amount and don't want to go back and fix

[FB ?? ??] - Unused. It takes those two numbers and doesn't do anything.

[FC] - Mute channel (debug code, not implemented)

[FD] - Fast-forward on (debug code, not implemented)

[FE] - Fast-forward off (debug code, not implemented)

(show an example of each of these commands working)

## The Set First Drum effect
[FA instrument] - Set first drum

This effect controls what instruments the CA-DF notes are.

These are special notes that automatically set the instrument and play a C-4.

(Show an example)

## Exporting and re-importing your songs

File -> Export Song

- This saves your song as an .EBM file - the raw data that gets saved in the game
- Don't forget to put what instrument packs you used in the filename! It's a pain to import note data saved like this and then realize you've forgotten what instruments go with it.

File -> Import Song

Make sure the SPC Packs tab stuff and the BGM list tab stuff is correct

## SPC Packs tab

oh jeez

## Making Your Own BRRs
- Decide what you want to record, and record it in Audacity or something similar
- Save it as a WAV file
- Go into OpenMPT and load the WAV file as a sample
- Make sure it's tuned to C if it's pitched (not percussive)
- Downsample it to 32000 hz or 16000 hz, whatever the lowest is that you can get away with without sounding like total garbage.
- Set loop points
- Save it as a new WAV file
- Now we need to convert this WAV to a BRR file.
  - Open the [C700 VST](http://picopicose.com/software.html) and drop the WAV file into it
  - Play a few notes to make sure it doesn't sound glitchy
  - Click the Save Sample button
  - Alternately, you can use [BRRTools](https://www.smwcentral.net/?p=section&a=details&id=17670), but I've never used it before and you have to put the loop point in the command line arguments
  

## Making Custom Instrument Packs
[Pinci's tutorial](https://www.smwcentral.net/?p=viewthread&t=101960)

How to use the EarthBound Instrument Packer

1. Make custom instrument pack CCS files
   - Get all the BRRs you want
      - convert WAVs or drag over from a SMWCentral dump
   - Make your config.txt
      - Choose which vanilla pack you want to overwrite
      - tune with BRR player and Audacity's sine wave generator
      - alternately, tune with the sample playing back in OpenMPT
      - alternately, a lot of SMWCentral folks have the decency to include the configuration info in the BRR packs they make

The CCS file this spits out can then be compiled into the game via CoilSnake. Your instruments will automatically be inserted and repointed.
