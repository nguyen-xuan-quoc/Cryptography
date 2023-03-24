# Cyber Apocalypse 2023

# Ancient encoding

```python
from Crypto.Util.number import bytes_to_long, long_to_bytes
from base64 import b64encode, b64decode

FLAG = b"HTB{??????????}"

def encode(message):
    return hex(bytes_to_long(b64encode(message)))

def main():
    encoded_flag = encode(FLAG)
    with open("output.txt", "w") as f:
        f.write(encoded_flag)

if __name__ == "__main__":
    main()
```

Ciphertext:

```python
0x53465243657a467558336b7764584a66616a4231636d347a655639354d48566664326b786246397a5a544e66644767784e56396c626d4d775a4446755a334e665a58597a636e6c33614756794d33303d
```

Cơ chế mã hóa sẽ như sau:

flag → base64 → long → hex

Vậy ta chỉ cần giải mã ngược lại là xong.

```python
print(b64decode(long_to_bytes(int(0x53465243657a467558336b7764584a66616a4231636d347a655639354d48566664326b786246397a5a544e66644767784e56396c626d4d775a4446755a334e665a58597a636e6c33614756794d33303d)).decode()) )
```

Flag: HTB{1n_y0ur_j0urn3y_y0u_wi1l_se3_th15_enc0d1ngs_ev3rywher3}

# Small steps

```
from Crypto.Util.number import getPrime, bytes_to_long

FLAG = b"HTB{???????????????}"
assert len(FLAG) == 20

class RSA:

    def __init__(self):
        self.q = getPrime(256)
        self.p = getPrime(256)
        self.n = self.q * self.p
        self.e = 3

    def encrypt(self, plaintext):
        plaintext = bytes_to_long(plaintext)
        return pow(plaintext, self.e, self.n)

def menu():
    print('[E]ncrypt the flag.')
    print('[A]bort training.\n')
    return input('> ').upper()[0]

def main():
    print('This is the second level of training.\n')
    while True:
        rsa = RSA()
        choice = menu()

        if choice == 'E':
            encrypted_flag = rsa.encrypt(FLAG)
            print(f'\nThe public key is:\n\nN: {rsa.n}\ne: {rsa.e}\n')
            print(f'The encrypted flag is: {encrypted_flag}\n')
        elif choice == 'A':
            print('\nGoodbye\n')
            exit(-1)
        else:
            print('\nInvalid choice!\n')
            exit(-1)

if __name__ == '__main__':
    main()
```

Challenge này sử dụng RSA làm cơ chế mã hóa. Nhưng với e = 3 và p và q là 2 số nguyên tố 256 bit 

→ N rất lớn. 

Vậy nên phép mod sẽ không có tác dụng, ta chỉ cần căn mũ e của ciphertext sẽ thu được flag.

```python
import gmpy2
from Crypto.Util.number import long_to_bytes

N = 7695354928017344933404842600854399632783027664582903001401962817282024975704187988117581437997164424396254352110461605574379481601141346225785175679776343
e = 3

flag = 70407336670535933819674104208890254240063781538460394662998902860952366439176467447947737680952277637330523818962104685553250402512989897886053

print(long_to_bytes(gmpy2.iroot(flag, e)[0]))
#HTB{5ma1l_E-xp0n3nt}
```

Flag: HTB{5ma1l_E-xp0n3nt}

# Perfect synchroniztion

```python
from os import urandom
from Crypto.Cipher import AES
from string import ascii_uppercase, digits
#from secret import MESSAGE

#assert all([x.isupper() or x in '{_} ' for x in MESSAGE])

class Cipher:
    def __init__(self):
        self.salt = urandom(15)
        key = urandom(16)
        self.cipher = AES.new(key, AES.MODE_ECB)

    def encrypt(self, message):
        return [self.cipher.encrypt(c.encode() + self.salt) for c in message]

def main():
    cipher = Cipher()
    encrypted = cipher.encrypt((ascii_uppercase+digits+'{_} '))
#    encrypted = "\n".join([c.hex() for c in encrypted])
    print(((encrypted)))
    # with open("output.txt", 'w+') as f:
    #     f.write(encrypted)

if __name__ == "__main__":
    main()
```

Kèm theo source code ở trên là 1 file ciphertext với rất nhiều đoạn hex với 32 kí tự.

```python
dfc8a2232dc2487a5455bda9fa2d45a1
305d4649e3cb097fb094f8f45abbf0dc
c87a7eb9283e59571ad0cb0c89a74379
...
```

Phân tích sorce code ta hiểu được, Flag sẽ được mã hóa từng bytes và thêm vào 15 bytes bằng AES cipher nhưng 15bytes này sẽ không thay đổi trong suốt quá trình giải mã. Vậy, mặc dù dùng AES để mã hóa nhưng bản chất chỉ là 1 biến thể của subcipher và chúng ta có thể dùng tool phân tích kí tự để giải. 

Như vậy, mỗi đoạn trong ciphertext là một kí tự và ta sẽ tạo ta 1 alphabet sau đó gán từng kí tự cho 1 đoạn trong ciphertext. Sau đó, ta sẽ đoán 3 kí tự có thể xuất hiện ít nhất trong plaintetx và gán trực tiếp cho các đoạn tương ứng trong ciphertext.

```python
fbe86a428051747607a35b44b1a3e9e9 -> {
a94f49727cf771a85831bd03af1caaf5 -> _
c53ba24fbbe9e3dbdd6062b3aab7ed1a -> }
```

Sau khi, làm xong tất cả các bước ở trên ta sẽ có 1 đoạn subcipher như sau:

```python
CFTJFSIVNESEDVWKWNKWNBEWFPN3SNXLFN EIXNXLEXNKSNESVN2KHFSNWXCFXILN3 NGCKXXFSNDES2JE2FNIFCXEKSNDFXXFCWNESPNI31BKSEXK3SWN3 NDFXXFCWN3IIJCNGKXLNHECVKS2N CFTJFSIKFWN13CF3HFCNXLFCFNKWNENILECEIXFCKWXKINPKWXCKBJXK3SN3 NDFXXFCWNXLEXNKWNC3J2LDVNXLFNWE1FN 3CNED13WXNEDDNWE1YDFWN3 NXLEXNDES2JE2FNKSNICVYXESEDVWKWN CFTJFSIVNESEDVWKWNEDW3NMS3GSNEWNI3JSXKS2NDFXXFCWNKWNXLFNWXJPVN3 NXLFN CFTJFSIVN3 NDFXXFCWN3CN2C3JYWN3 NDFXXFCWNKSNENIKYLFCXFOXNXLFN1FXL3PNKWNJWFPNEWNESNEKPNX3NBCFEMKS2NIDEWWKIEDNIKYLFCWN CFTJFSIVNESEDVWKWNCFTJKCFWN3SDVNENBEWKINJSPFCWXESPKS2N3 NXLFNWXEXKWXKIWN3 NXLFNYDEKSXFOXNDES2JE2FNESPNW31FNYC3BDF1NW3DHKS2NWMKDDWNESPNK NYFC 3C1FPNBVNLESPNX3DFCESIFN 3CNFOXFSWKHFNDFXXFCNB33MMFFYKS2NPJCKS2NG3CDPNGECNKKNB3XLNXLFNBCKXKWLNESPNXLFNE1FCKIESWNCFICJKXFPNI3PFBCFEMFCWNBVNYDEIKS2NIC3WWG3CPNYJZZDFWNKSN1ER3CNSFGWYEYFCWNESPNCJSSKS2NI3SXFWXWN 3CNGL3NI3JDPNW3DHFNXLF1NXLFN EWXFWXNWFHFCEDN3 NXLFNIKYLFCWNJWFPNBVNXLFNEOKWNY3GFCWNGFCFNBCFEMEBDFNJWKS2N CFTJFSIVNESEDVWKWN 3CNFOE1YDFNW31FN3 NXLFNI3SWJDECNIKYLFCWNJWFPNBVNXLFNREYESFWFN1FILESKIEDN1FXL3PWN3 NDFXXFCNI3JSXKS2NESPNWXEXKWXKIEDNESEDVWKWN2FSFCEDDVNLXB{E_WK1YDF_WJBWXKXJXK3S_KW_GFEM}NIECPNXVYFN1EILKSFCVNGFCFN KCWXNJWFPNKSNG3CDPNGECNKKNY3WWKBDVNBVNXLFNJWNEC1VWNWKWNX3PEVNXLFNLECPNG3CMN3 NDFXXFCNI3JSXKS2NESPNESEDVWKWNLEWNBFFSNCFYDEIFPNBVNI31YJXFCNW3 XGECFNGLKILNIESNIECCVN3JXNWJILNESEDVWKWNKSNWFI3SPWNGKXLN13PFCSNI31YJXKS2NY3GFCNIDEWWKIEDNIKYLFCWNECFNJSDKMFDVNX3NYC3HKPFNESVNCFEDNYC3XFIXK3SN 3CNI3S KPFSXKEDNPEXENYJZZDFNYJZZDFNYJZZDFAAA
```

và ta đã có format flag trong đó, chỉ là các byte đã bị thay đổi.

```python
LXB{E_WK1YDF_WJBWXKXJXK3S_KW_GFEM}
```

Cuối cùng ta cho vào quip quip để giải → get flag.

Flag: HTB{A_Simple_Substitution_Is_Weak}

# Multipage recycling

```python
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad
import random, os

FLAG = b'HTB{??????????????????????}'

class CAES:

    def __init__(self):
        self.key = os.urandom(16)
        self.cipher = AES.new(self.key, AES.MODE_ECB)

    def blockify(self, message, size):
        return [message[i:i + size] for i in range(0, len(message), size)]

    def xor(self, a, b):
        return b''.join([bytes([_a ^ _b]) for _a, _b in zip(a, b)])

    def encrypt(self, message):
        iv = os.urandom(16)

        ciphertext = b''
        plaintext = iv

        blocks = self.blockify(message, 16)
        for block in blocks:
            ct = self.cipher.encrypt(plaintext)
            encrypted_block = self.xor(block, ct)
            ciphertext += encrypted_block
            plaintext = encrypted_block

        return ciphertext

    def leak(self, blocks):
        r = random.randint(0, len(blocks) - 2)
        leak = [self.cipher.encrypt(blocks[i]).hex() for i in [r, r + 1]]
        return r, leak

def main():
    aes = CAES()
    message = pad(FLAG * 4, 16)

    ciphertext = aes.encrypt(message)
    ciphertext_blocks = aes.blockify(ciphertext, 16)

    r, leak = aes.leak(ciphertext_blocks)

    with open('output.txt', 'w') as f:
        f.write(f'ct = {ciphertext.hex()}\nr = {r}\nphrases = {leak}\n')

if __name__ == "__main__":
    main()

'''
ct = bc9bc77a809b7f618522d36ef7765e1cad359eef39f0eaa5dc5d85f3ab249e788c9bc36e11d72eee281d1a645027bd96a363c0e24efc6b5caa552b2df4979a5ad41e405576d415a5272ba730e27c593eb2c725031a52b7aa92df4c4e26f116c631630b5d23f11775804a688e5e4d5624
r = 3
phrases = ['8b6973611d8b62941043f85cd1483244', 'cf8f71416111f1e8cdee791151c222ad']
'''
```

Theo như source code ở trên thì mã hóa AES sẽ chia `pad(FLAG * 4, 16)` thành từng block, mỗi `block[i]` 16 bytes sẽ được mã hóa thành `enc(block[i])` theo công thức như sau:

```python
enc(block[1]) = AES_ENC(iv) ^ block[1]
enc(block[i]) = AES_ENC(enc(block[i-1])) ^ block[i]
=> block[i] = enc(block[i]) ^ AES_ENC(enc(block[i-1]))
```

Điều đáng chú ý trong source code là:

```python
  def leak(self, blocks):
      r = random.randint(0, len(blocks) - 2)
      leak = [self.cipher.encrypt(blocks[i]).hex() for i in [r, r + 1]]
      return r, leak
```

Vậy ta sẽ leak được `AES_ENC(enc(block[r]))` và `AES_ENC(enc(block[r+1]))`

```python
ct = bc9bc77a809b7f618522d36ef7765e1cad359eef39f0eaa5dc5d85f3ab249e788c9bc36e11d72eee281d1a645027bd96a363c0e24efc6b5caa552b2df4979a5ad41e405576d415a5272ba730e27c593eb2c725031a52b7aa92df4c4e26f116c631630b5d23f11775804a688e5e4d5624
r = 3
phrases = ['8b6973611d8b62941043f85cd1483244', 'cf8f71416111f1e8cdee791151c222ad']
```

Vậy ta se có `AES_ENC(enc(block[3]))` và `AES_ENC(enc(block[4]))`

Như vậy ta có thể tìm được block 4 và 5 bằng công thức đã phân tích ở trên:

 `block[i] = enc(block[i]) ^ AES_ENC(enc(block[i-1]))`

```python
import random
from pwn import *

ct = "bc9bc77a809b7f618522d36ef7765e1cad359eef39f0eaa5dc5d85f3ab249e788c9bc36e11d72eee281d1a645027bd96a363c0e24efc6b5caa552b2df4979a5ad41e405576d415a5272ba730e27c593eb2c725031a52b7aa92df4c4e26f116c631630b5d23f11775804a688e5e4d5624"
r = 3
phrases = ['8b6973611d8b62941043f85cd1483244', 'cf8f71416111f1e8cdee791151c222ad'] #[block[3], block[4]]

def blockify(message, size):
    return [message[i:i + size] for i in range(0, len(message), size)]

enc_blocks = blockify(ct, 32)
print(xor(bytes.fromhex(phrases[0]),bytes.fromhex(enc_blocks[4])))
print(xor(bytes.fromhex(phrases[1]),bytes.fromhex(enc_blocks[5])))
#HTB{CFB_15_w34k_w17h_l34kz}
```

Ta sẽ thu được:

```python
b'_w34k_w17h_l34kz'
b'}HTB{CFB_15_w34k'
```

và ghép lại ta sẽ có Flag: HTB{CFB_15_w34k_w17h_l34kz}

# Inside The Matrix

```python
from sage.all_cmdline import *
# from utils import ascii_print
import os

FLAG = b"HTB{????????????????????}"
assert len(FLAG) == 25

class Book:

    def __init__(self):
        self.size = 5
        self.prime = None

    def parse(self, pt: bytes):
        pt = [b for b in pt]
        return matrix(GF(self.prime), self.size, self.size, pt)

    def generate(self):
        key = os.urandom(self.size**2)
        return self.parse(key)

    def rotate(self):
        self.prime = random_prime(2**6, False, 2**4)

    def encrypt(self, message: bytes):
        self.rotate()
        key = self.generate()
        message = self.parse(message)
        ciphertext = message * key
        return ciphertext, key

def menu():
    print("Options:\n")
    print("[L]ook at page")
    print("[T]urn page")
    print("[C]heat\n")
    option = input("> ")
    return option

def main():
    book = Book()
    ciphertext, key = book.encrypt(FLAG)
    page_number = 1

    while True:
        option = menu()
        if option == "L":
            # ascii_print(ciphertext, key, page_number)
            print(ciphertext, key, page_number)
        elif option == "T":
            ciphertext, key = book.encrypt(FLAG)
            page_number += 2
            print()
        elif option == "C":
            print(f"\n{list(ciphertext)}\n{list(key)}\n")
        else:
            print("\nInvalid option!\n")

if __name__ == "__main__":
    try:
        main()
    except Exception as e:
        print(f"An error occurred: {e}")
```

Cơ chế mã hóa của challenge này là 25 bytes của flag sẽ được săp xếp thành một ma trận 5x5 trong một trường hữu hạn GF(prime) với prime là một số nguyên tố ngẫu nhiên trong khoảng 2^4 đến 2^6. Một cơ chế sinh khóa sẽ tạo ra 25 bytes ngẫu nhiên và sắp xếp thành ma trận như đã làm với flag. Sau cùng ta sẽ có:

```python
ciphertext = Matrix(GF(prime), flag) * Matrix(GF(prime), key)
```

Với `flag` và `key` là ma trận 5x5 thì tất nhiên `ciphertext` cũng là 1 ma trận 5x5:

Vậy ta có thể tìm flag theo phép nhân ma trận nghịch đảo : 

```python
Matrix(GF(prime), flag) = ciphertext * Matrix(GF(prime), key)^(-1)
```

Điều quan trọng trong phép tính này là ta phải tìm đúng `prime` , nhưng với giới hạn nhỏ như `2^4 → 2^6` ta có thể brute force và kiểm tra được `(bytes(H) mod prime = flag[0][0])`. Cụ thể là từ phần tử lớn nhất của key đến 2^6

Nhưng khi thu được `Matrix(GF(prime), flag)` thì các bytes trong flag đã bị thay đổi bằng phép `flag[i] mod prime`

Như vậy ta sẽ thu thập 3 ciphertext khác nhau với 3 prime khác nhau để giải định lý thằng dư trung hoa.

```python
flag[i] mod Prime1 = Flag1[i]
flag[i] mod Prime2 = Flag2[i]
flag[i] mod Prime3 = Flag3[i]
```

Sau khi giải xong 25 bytes của flag ta sắp xếp lại thì sẽ thu được flag gốc.

```python
from sage.all import *
from Crypto.Util.number import *
from sympy.ntheory.modular import crt

FLAG = b"HTB{????????????????????}"
def parse(prime, pt: bytes):
    pt = [b for b in pt]
    return matrix(GF(prime), 5, 5, pt)

C1 = [(10, 34, 32, 0, 12), (0, 26, 33, 9, 33), (7, 0, 6, 2, 25), (8, 14, 0, 1, 5), (21, 35, 35, 15, 9)] #ciphertext
K1 = [(20, 14, 8, 31, 30), (19, 20, 14, 28, 17), (23, 0, 16, 2, 29), (30, 35, 12, 16, 26), (19, 1, 36, 8, 10)] #key

C2 = [(22, 16, 22, 14, 16), (0, 20, 14, 4, 15), (15, 20, 5, 20, 11), (20, 20, 7, 5, 7), (18, 1, 22, 4, 2)]
K2 = [(4, 20, 0, 11, 16), (19, 12, 17, 21, 16), (20, 17, 11, 12, 0), (14, 15, 1, 10, 7), (4, 9, 10, 8, 14)]

C3 = [(11, 16, 1, 6, 21), (4, 3, 10, 24, 14), (1, 18, 10, 18, 18), (25, 10, 25, 14, 5), (9, 10, 3, 3, 2)]
K3 = [(12, 0, 7, 7, 6), (7, 28, 15, 8, 19), (11, 25, 5, 3, 2), (16, 19, 15, 22, 8), (13, 11, 21, 9, 17)]                  

C1 = Matrix(C1)
C2 = Matrix(C2)
C3 = Matrix(C3)

FLAG1 = []
FLAG2 = []
FLAG3 = []

p = []

#FLAG*KEY = CIPHERTEXT => FLAG = CIPHERTEXT*KEY^-1
#flag mod p = CIPHERTEXT * KEY^-1
#H = 72, T = 84, B = 66, 

for i in range(max(max(K1))+1, 2**6):
    if isPrime(i):
        K1_GF = Matrix(GF(i), K1)
        if K1_GF.is_invertible():
            K1_INV = K1_GF.inverse()        
            FLAG1 = C1 * K1_INV
            if (72 % i) == int(FLAG1[0][0]):
                p.append(i)
                break
        

for i in range(max(max(K2))+1, 2**6):
    if isPrime(i):
        K2_GF = Matrix(GF(i), K2)
        if K2_GF.is_invertible():
            K2_INV = K2_GF.inverse()          
            FLAG2 = C2 * K2_INV
            if (72 % i) == int(FLAG2[0][0]):
                p.append(i)
                break

for i in range(max(max(K3))+1, 2**6):
    if isPrime(i):
        K3_GF = Matrix(GF(i), K3)
        if K3_GF.is_invertible():
            K3_INV = K3_GF.inverse()      
            FLAG3 = C3 * K3_INV
            if (72 % i) == int(FLAG3[0][0]):
                p.append(i)
                break

FLAG1 = FLAG1.list()
FLAG2 = FLAG2.list()
FLAG3 = FLAG3.list()

for i in range(25):
    v = [int(FLAG1[i]),int(FLAG2[i]), int(FLAG3[i])]
    print(chr(crt(p, v)[0]), end='')
#HTB{l00k_@t_7h3_st4rs!!!}
```

Flag: HTB{l00k_@t_7h3_st4rs!!!}