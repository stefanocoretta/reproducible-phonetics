
# Measurements extraction script

## get-measurements.praat
```praat
<<<read files>>>

<<<measurements loop>>>
```

## "read files"
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

## "measurements loop"
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

        resultLine$ = "'word$','vowel$','duration','f1','f2','f1Bark','f2Bark'"
        appendFileLine: resultFile$, resultLine$
    endif
endfor
```
