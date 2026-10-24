# Aufgabe 1 - Einführung in die VSCode-IDE

## Was das Programm macht
Das Programm schreibt Bitmasken in die GPIOD-Register SET (einschalten)
und CLR (ausschalten). Nur Bits mit Wert 1 wirken, alle anderen LEDs
bleiben unverändert.

Schrittweise Auswertung des Originalprogramms:

| Anweisung        | Wirkung          | LEDs an        |
|------------------|------------------|----------------|
| STRB R2,[R6]     | D14 an           | D14            |
| STRB R3,[R6]     | D15 an           | D14, D15       |
| STRB R0,[R6]     | D08 an           | D08, D14, D15  |
| STRB R0,[R7]     | D08 aus          | D14, D15       |
| STRB R0,[R6]     | D08 an           | D08, D14, D15  |
| STRB R1,[R6]     | D09 an           | D08, D09, D14, D15 |
| STRB R2,[R7]     | D14 aus          | D08, D09, D15  |
| STRB R3,[R7]     | D15 aus          | D08, D09       |

Endzustand: nur D08 und D09 leuchten (0x03).

## Optimierung
Da nur der Endzustand zählt, reicht eine Anweisung:
`MOV R0, #0x03` und `STRB R0, [R6]`.
Die ursprünglichen Anweisungen sind als Kommentar erhalten.