# Osterformel

# Postleitzahl

## International

Prüfen und vereinheitlichen internationaler Postleitzahlen, z.B. für die Verwendung in Karten. Kann danach direkt in Cubes/Trees verwendet werden, um daraus Selektor-Variablen zu erstellen

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
