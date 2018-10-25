> This project is still in its early stages. Most documentation is far from finished (or coherent), and the code might not work as expected.


# binarypaper
Creates printable (as in; on paper) text from a binary file, for backup purposes. It doesn't use bar or QR codes to make it more rugged and resilient to technology changes.

# Format
The print shall have to be readable through OCR without much problems and shall be somewhat ocr-error resilient.

The format is as follows:
String for hex values, with length of 'n', followed by a 3 character checksum (also in hex).
The hex values are to be stored without any prefix ("0x" or "h", or anything) and not contain any separators. The checksum is represented as hex just like the actual data, and is a sum of all values on that line (excluding the checksum) modulo 0xFFFFFF.
The length of the string ('n') can be anything, as long as it can be printed on one line (including its checksum).


# Pseudo code
The program does the following in pseudo code:
//ENCODE
```
SET checksum TO 0
SET charcount TO 0
SET line_length TO n
FOR ( character_to_encode )
    INCREASE charcount BY 1
    checksum = ( checksum + VALUE OF ( character_to_encode ) ) MOD 0xFFFFFF
    PRINT character_to_encode
    IF ( charcount EQUALS line_length )
        PRINT checksum
        LINEFEED
        SET charcount TO 0
        SET checksum TO 0
    END IF
END FOR
IF ( charcount DOES NOT EQUAL 0 )
    PRINT checksum
END IF
```

// DECODE
```
SET checksum TO 0
SET charcount TO 0
SET line_length TO n
FOR ( hex_char_to_decode ) // read 2 chars in hex as one hex_char_to_decode.
    INCREASE charcount BY 1
    IF ( line_length EQUALS charcount - 3 )
        .....
    checksum = ( checksum + VALUE OF ( hex_char_to_decode ) ) MOD 0xFFFFFF
```
