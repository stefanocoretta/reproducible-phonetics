
# Measurements extraction script

This script extracts the formant values (in Hertz and Bark) and the duration of vowels from the file `sc.wav`.

#### get-measurements.praat
```praat
<<<read files>>>

<<<measurements loop>>>
```

The sound and TextGrid files are read, and the result file is initialised.
A Formant object is also created from the sound file.

#### "read files"
```praat
sound = Read from file: "../data/sc.wav"
formant = To Formant (burg): 0, 5, 5000, 0.025, 50
textgrid = Read from file: "../data/sc-palign.TextGrid"
createDirectory("../results")

header$ = "word,vowel,duration,F1,F2,F1.bark,F2.bark"
resultFile$ = "../results/vowels.csv"
writeFileLine: resultFile$, header$

selectObject: textgrid
intervals = Get number of intervals: 1
```

The following code is the main loop with extracts the measurements.
For each vowel, as indicated in the TextGrid, the start and end time of the interval are used to calculate duration and extract formant values from the Formant object.
The measurements are saved in `vowels.csv`.

#### "measurements loop"
```praat
for interval to intervals - 1
    label$ = Get label of interval: 1, interval
    if label$ == "#"
        start = Get start time of interval: 1, interval + 8
        end = Get end time of interval: 1, interval + 8
        duration = (end - start) * 1000
        vowel$ = Get label of interval: 1, interval + 8

        selectObject: formant
        f1 = Get mean: 1, start, end, "Hertz"
        f1Bark = Get mean: 1, start, end, "Bark"
        f2 = Get mean: 2, start, end, "Hertz"
        f2Bark = Get mean: 2, start, end, "Bark"

        selectObject: textgrid
        wordInterval = Get interval at time: 2, start
        word$ = Get label of interval: 2, wordInterval

        resultLine$ = "'word$','vowel$','duration','f1','f2',
            ...'f1Bark','f2Bark'"
        appendFileLine: resultFile$, resultLine$
    endif
endfor
```
