 # Regex
 
 Notes from [this course](https://www.executeprogram.com/courses/regexes).
 
 ### LITERALS
 
 `/a/`
 Result: 
 (matches 'a') 
 
 ### WILDCARD
 
 `/./`
 
 Result: 
 (matches anything) 
 
 ### BOUNDARIES
 
 `/^/`
 
 Result: 
 (matches start of string) 
 
 `/$/`
 
 Result: 
 (matches end of string) 
 
 ### REPETITION
 
 `/a+/`
 
 Result: 
 (matches one or more 'a') 
 
 `/a*/`
 
 Result: 
 (matches zero or more 'a') 
 
 ### OR
 
 `/a|b/`
 
 Result: 
 (matches 'a' or 'b') 
 
 ### PARENS
 
 `/^(a|b)+$/`
 
 Result: 
 (matches a, b, ab, ba, ...) 
 
 ### ESCAPING
 
 `/\+/`
 
 Result: 
 (matches '+') 
 
 `/\$/`
 
 Result: 
 (matches '$') 
 
 ### BASIC CHARACTER SETS
 
 `/[ab]/`
 
 Result: 
 (matches 'a' or 'b') 
 
 `/[a-z]/`
 
 Result: 
 (matches lowercase letters) 
 
 ### BASIC CHARACTER CLASSES
 
 `/\s/`
 
 Result: 
 (matches whitespace) 
 
 `/\d/`
 
 Result: 
 (matches digits) 
 
 `/\D/`
 
 Result: 
 (matches not-digits) 
 
 ### CHARACTER SETS
 
 `/[$]/`
 
 Result: 
 (matches '$') 
 
 `/[a^]/`
 
 Result: 
 (matches 'a' and '^') 
 
 ### MAYBE
 
 `/a?/`
 
 Result: 
 (matches 'a' or '') 
 
 ### HEX CODES
 
 `/\xhh/`
 
 Result: 
 (matches hex codes) 