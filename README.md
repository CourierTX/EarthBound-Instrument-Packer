## A BRR import tool for use with the [EarthBound Music Editor](https://github.com/PKHackers/ebmused/releases)

Usage:

Create a folder with this structure:
```
config.txt   <-- take a look at the Examples folder to see what goes in here
Flute.brr    <-- AddMusicK-compliant BRR files
Strings.brr
asdfjkl.brr
```

Drop your folder on the EXE (or paste in the path), and it spits out a CCScript file.

Compile this into a CoilSnake project, and the ROM will have the custom instruments automatically inserted and repointed for you!

In addition to this, if there are any .ebm files present in the folder, it will also generate [SPC files](http://www.vgmpf.com/Wiki/index.php?title=SPC) for easy preview and playback in either [SPCPlay](https://github.com/dgrfactory/spcplay/releases) or a flashcart.
