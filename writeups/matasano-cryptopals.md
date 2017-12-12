### 1.1

```py
import base64
import binascii
def hex_to_bytes(hexstr):
	return binascii.unhexlify(hexstr)
	
def bytes_to_hex(bytestr):
	return binascii.hexlify(bytestr)
	
def bytes_to_b64(bytestr):
	return(base64.b64encode(base64.b64encode))
		
#print bytes_to_b64(hex_to_bytes('49276d206b696c6c696e6720796f757220627261696e206c696b65206120706f69736f6e6f7573206d757368726f6f6d'))
```

### 1.2

```py
from Challenge1 import hex_to_bytes, bytes_to_b64, bytes_to_hex

input = '1c0111001f010100061a024b53535009181c'
xorkey = '686974207468652062756c6c277320657965'


def xor_encode(buff1, buff2):
	return [a^b for a, b in zip(buff1, buff2)]
	
if __name__=='__main__':	
	xor_res = [chr(x) for x in xor_encode(map(ord, hex_to_bytes(input)), map(ord, hex_to_bytes(xorkey)))]
	print bytes_to_hex(''.join(xor_res))

```

### 1.3

```py
from Challenge1 import hex_to_bytes, bytes_to_b64, bytes_to_hex

hex_encoded = '1b37373331363f78151b7f2b783431333d78397828372d363c78373e783a393b3736'
result = map(ord, hex_to_bytes(hex_encoded))

def xor_single(buff, char):
    return [x^char for x in buff]
    
def score(input):
    scoring = {69:2, 101:2}
    mass = 0
    for l in input:
        if l in scoring.iterkeys():
            mass += 4
        elif (l >= 65 and l <= 90):
                #lowercase
                mass += 3
        elif l >= 97 and l <= 122:
                mass +=3
        elif l == 32:
                #spaces
                mass += 3
        else:
                mass -=3
    return mass

if __name__=='__main__':
    
    xorlist = []
            
    for i in range(0, 255):
        decoded = xor_single(result, i)
        scored = score(decoded)
        written = ''.join(map(chr, decoded))
        xorlist.append([scored, i, written])

    print max(xorlist, key=lambda x:x[0])
```

### 1.4

```py
from Challenge1 import hex_to_bytes, bytes_to_b64, bytes_to_hex
from Challenge3 import score, xor_single

lines = []

with open('4.txt') as filefour:
    lines = filefour.readlines()

lines = [x[:-1] for x in lines if len(x)%2 !=0]

forcelist = []

for result in lines:
    result = map(ord, hex_to_bytes(result))
    
    xorlist = []
            
    for i in range(0, 255):
        decoded = xor_single(result, i)
        scored = score(decoded)
        written = ''.join(map(chr, decoded))
        xorlist.append([scored, i, written])
    forcelist.append(max(xorlist, key=lambda x:x[0]))

print max(forcelist, key=lambda x:x[0])
    

```



