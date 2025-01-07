# Encoding
---

- <mark><strong>The process to convert User* Friendly data to Environment Friendly Data</strong></mark>

<small>*user isn't necessarily means humans, the term User simply means who consums the data and Environment means the place where data is stored/sent.</small>


### Some Examples:
	 - Encoding Text to bytes so that computer understand
	 - Encoding RGB to bytes so that image viewer can understand what to show
	 - Encoding bytes to base64 so that text based protocols can understand (here consumer is system and environment is HTTP protocol)

### Notes on utf-8 and base64 encoding:
- utf-8 is used to convert the human readable chars to system readable bytes
- base64 encoding however is used to encode system readable bytes to ascii chars, its used where only ascii allowed like html and css.

# Decoding:
---

- <mark><strong>The reverse process of encoding is decoding.</strong></mark> 

- Here you specify how to present system readable bytes to users, for example how to show image bytes in rgb values, or how to show binary text to user friendly text.

# Practical:
---

- when you use python's `open` function with 'wb'/'rb' mode, it directly works with system readable binary data.
- when you use base64.encode function and give input bytes, it convets tham in ascii, same way base64.decode returns original binary data.
- when you use str.encode with utf-8 encoding it covers tham to "b-string", and bytes.decode turns back to str. 
