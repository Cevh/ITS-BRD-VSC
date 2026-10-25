### Anw01: `LDR R0, =VariableA`

Die Adresse von `VariableA` wird in R0 geladen.

- R0: `0x2000000C` (Adresse von VariableA)
- R2: unverändert (`0x4`)
- R3: unverändert (`0x8`)
- Memory: unverändert. Die ersten Bytes sind `EF BE 34 12`.


### Anw02: `LDRB R2, [R0]`

Das Byte an der Speicheradresse in R0 wird in R2 geladen. Das erste Byte von VariableA ist `0xEF`.

- **R0:** `0x2000000C` (unverändert)
- **R2:** `0xEF`
- **R3:** `0x8` (unverändert)
- **Memory:** unverändert; die Bytes bleiben `EF BE 34 12`.

### Anw03: `LDRB R3, [R0,#1]`

Das Byte an der Adresse `R0 + 1` wird in R3 geladen. Es ist das zweite Byte von VariableA.

- **R0:** `0x2000000C` (unverändert)
- **R2:** `0xEF` (unverändert)
- **R3:** `0xBE`
- **Memory:** unverändert; die Bytes bleiben `EF BE 34 12`.


### Anw04: `LSL R2, #8`

R2 wird um 8 Bit nach links verschoben. Dadurch wird aus `0xEF` der Wert `0xEF00`.

- **R0:** `0x2000000C` (unverändert)
- **R2:** `0xEF00`
- **R3:** `0xBE` (unverändert)
- **Memory:** unverändert; die Bytes bleiben `EF BE 34 12`.

### Anw05: `ORR R2, R3`

Die Register R2 und R3 werden bitweise mit ODER verknüpft. Dadurch wird `0xBE` mit `0xEF00` kombiniert. Das Ergebnis ist `0xEFBE`.

- **R0:** `0x2000000C` (unverändert)
- **R2:** `0xEFBE`
- **R3:** `0xBE` (unverändert)
- **Memory:** unverändert; die Bytes bleiben `EF BE 34 12`.

### Anw06: `STRH R2, [R0]`

Der Wert der unteren 16 Bit von R2 wird an der Speicheradresse gespeichert, auf die R0 zeigt. Dadurch werden die ersten zwei Bytes von `VariableA` überschrieben.

- **R0:** `0x2000000C` (unverändert)
- **R2:** `0xEFBE` (unverändert)
- **Memory vorher:** `EF BE 34 12`
- **Memory nachher:** `BE EF 34 12`

Da das System Little Endian verwendet, werden die beiden Bytes des Halbworts `0xEFBE` in der Reihenfolge `BE EF` gespeichert.


### Konstante 0xAFFE in VariableC speichern

Die Konstante 0xAFFE wird in Register R5 geladen. Anschließend werden die beiden Bytes vertauscht, damit der Speicher aufgrund der Little-Endian-Darstellung die Bytes AF FE in dieser Reihenfolge anzeigt.

LSL und LSR verschieben die Bits, um die beiden Bytes an die richtige Position zu bringen.

ORR kombiniert die beiden Bytes zum Wert 0xFEAF.

STRH speichert die unteren 16 Bit in VariableC.

Ergebnis: R6 = 0xFEAF. Im Speicher stehen an aufeinanderfolgenden Adressen die Bytes AF FE.

### Anw09–Anw0D: Speicherreihenfolge von VariableB ändern

Der ursprüngliche Wert von VariableB ist 0x1234. Aufgrund der Little-Endian-Darstellung stehen die Bytes im Speicher zunächst in der Reihenfolge 34 12.

LDRH lädt den 16-Bit-Wert aus VariableB in R6.

MOV kopiert den Wert in R7, damit beide Bytes bearbeitet werden können.

LSL und LSR verschieben die Bits, um die beiden Bytes zu vertauschen.

ORR kombiniert die verschobenen Bytes. Dadurch enthält R6 den Wert 0x3412.

STRH speichert den neuen Wert in VariableB.

Ergebnis: Im Memory Browser stehen die Bytes 12 34 an aufeinanderfolgenden Adressen. Damit wird die geforderte Speicherreihenfolge erreicht.