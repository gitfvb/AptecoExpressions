# Osterformel

```
DateShift(MakeDate(1900;1;1);Round((DateDiff(MakeDate(1904;1;1);MakeDate(DateYear([Buchungsdatum]);4;DateDay(DateShift(MakeDate(1904;01;01);(60*24*(DateYear([Buchungsdatum])/38-RoundDown(DateYear([Buchungsdatum])/38;1))%60)/2+54;"days")));"days")+1462)/7)*7-8;"days")
```

# Postleitzahl

## International

Prüfen und vereinheitlichen internationaler Postleitzahlen, z.B. für die Verwendung in Karten. Kann danach als virtuelle Variable (Text) gespeichert werden und in Cubes/Trees verwendet werden, um daraus Selektor-Variablen zu erstellen

![grafik](https://user-images.githubusercontent.com/14135678/81716454-b103ee80-9479-11ea-9e8e-5d66d6016b3c.png)

```
Case(
	
	/* Länderspezifische PLZ */
	 {Query #1 - Deutschland+Valide};AddStr("DE-";Trim(StrClean([KU-PLZ];"ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz")))
	;{Query #2 - Niederlande+Valide};AddStr("NL--";Trim(StrClean([KU-PLZ];"ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz")))
	
	/* Unclassifieds */
	;1=1;"!"
	
)
```

# Demo

## Banding von "Personal Best" via Expression
```
Index(
	If(
		IsMissing([Personal Best - Letzer Trend])
		;1
		;BandUp(
			[Personal Best - Letzer Trend]
			;-1,6;-1,2;-0,8;-0,4;0;0,1;0,5;0,9;1,3;1,7
		)+2
	)
	;"!"
	;"-2,0 - -1,7"
	;"-1,6 - -1,3"
	;"-1,2 - -0,9"
	;"-0,8 - -0,5"
	;"-0,4 - -0,1"
	;" 0,0"
	;"+0,1 - +0,4"
	;"+0,5 - +0,8"
	;"+0,9 - +1,2"
	;"+1,3 - +1,6"
	;"+1,7 - +2,0"
)
```

## Durchschnittliche Anzahl Tage zwischen Käufen in Kategorien

```
Index(
	If(
		IsMissing([durchschnittliche Zeit zwischen zwei Käufe in Tagen])
		;1
		;BandUp(
			[durchschnittliche Zeit zwischen zwei Käufe in Tagen]
			;0;7;14;30;60;90;180;360;540;720
		)+2
	)
	;"!"
	;" 0"
	;"01 - 06"
	;"07 - 13"
	;"14 - 29"
	;"30 - 59"
	;"60 - 89"
	;"90 - 179"	
	;"180 - 359"
	;"360 - 539"	
	;"540 - 719"
	;">= 720"
)
```

## Normalverteilung 

Um eine Normalverteilung in die Expressions zu bringen, einfach folgende Formel benutzen. Damit sieht die Verteilung von Zufallswerten nicht immer gleich aus.

```
15*((Rand(1000)/1000)+(Rand(1000)/1000)+(Rand(1000)/1000)+(Rand(1000)/1000)+(Rand(1000)/1000)+(Rand(1000)/1000)+(Rand(1000)/1000)+(Rand(1000)/1000)+(Rand(1000)/1000)+(Rand(1000)/1000)+(Rand(1000)/1000)+(Rand(1000)/1000)-6)+(100/2)
```
15 stellt die Standardabweichung dar und 100/2 ist der Erwartungswert

## Zufällige Kaufkraft erstellen:

```
Index(BandUp(Rand(100);10;40;70;95)+1;
    "sehr unterdurchschnittlich";
    "unterdurchschnittlich";
    "durchschnittlich";
    "überdurchschnittlich";
    "sehr überdurchschnittlich")
```

## Always 10% Testgroup:

Markiert jede 10. Zeile mit einer "1". Wechselt jeden Tag durch und ergibt eine andere Markierung.

```
If(SequenceNumber(DayOfYear(Today(0)))%10,0,1)
```

## Convert YYYYMMDD into Date:

```
MakeDate(StrNumber(SubStr([XP BEG DAT];0;4));StrNumber(SubStr([XP BEG DAT];4;2));StrNumber(SubStr([XP BEG DAT];6 ;2)))
```

## Text to Selector as Virtual Variable:

The Case statement is returning a numeric value between 1 - 4. The Index function is then converting this numeric value to a description. Note the Index function can only use numerics!

```
    INDEX(CASE(
    CodeOf([Variable])="A",1,
    CodeOf([Variable])="B",2,
    CodeOf([Variable])="C",3,
    CodeOf([Variable])="D",4
    ),"Description A","Description B","Description C","Description D")
```

The first parameter must be numeric as it is the index to select from the list of descriptions e.g. 3 selects Wednesday. You can also use IndexOf to get the index of the selector code and use that directly.
e.g. INDEX(IndexOf([Variable]),"Description A","Description B","Description C","Description D")

```
Index(DayOfWeek([Booking Date]),"Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday")
Index(DayOfWeek([Buchungsdatum]);"Montag";"Dienstag";"Mittwoch";"Donnerstag";"Freitag";"Samstag";"Sonntag")
```

## Leere Flag Arrays vermeiden

Nur ausgeben, wenn Flag Array Werte enthält, sonst alternativen Flag Array nehmen:

```
If(Populated([test Agg])>0;DescOf([test Agg]);DescOf([Automarken]))
```

## Heutiges Datum nehmen, wenn Datum nicht valide:

```
If(And([Geburtsdatum]>19000101;[Geburtsdatum]<20500101);[Geburtsdatum]; today(0))
```


# Hinweise

* Angaben in `[]`sind Variablen aus dem System
* Angaben in `{}` sind Selektionen, die per Drag&Drop in den Ausdruck gezogen werden
