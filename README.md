# VIGENERE-CIPHER
## EX. NO: 4
## Name: HARISELVAN S
## IMPLEMETATION OF VIGENERE CIPHER
 

## AIM:

To implement the Vigenere Cipher substitution technique using C program.

## DESCRIPTION:

To encrypt, a table of alphabets can be used, termed a tabula recta, Vigenère square,or Vigenère table. It consists of the alphabet written out 26 times in differnt rows, each
 
alphabet shifted cyclically to the left compared to the previous alphabet, corresponding to the 26 possible Caesar ciphers. At different points in the encryption process, the cipher uses adifferent alphabet from one of the rows. The alphabet used at each point repeating keyword.depends on a Each row starts with a key letter. The remainder of the row holds the letters A to Z. Although there are 26 key rows shown, you will only use as many keys as there are unique letters in the key string, here just 5 keys, {L, E, M, O, N}. For successive letters of the message, we are going to take successive letters of the key string, and encipher each message letter using its corresponding key row. Choose the next letter of the key, go along that row to find the column heading that	atches the message character; the letter at the intersection of
[key-row, msg-col] is the enciphered letter.


## ALGORITHM:

STEP-1: Arrange the alphabets in row and column of a 26*26 matrix.
STEP-2: Circulate the alphabets in each row to position left such that the first letter is attached to last.
STEP-3: Repeat this process for all 26 rows and construct the final key matrix.
STEP-4: The keyword and the plain text is read from the user.
STEP-5: The characters in the keyword are repeated sequentially so as to match with that of the plain text.
STEP-6: Pick the first letter of the plain text and that of the keyword as the row indices and column indices respectively.
STEP-7: The junction character where these two meet forms the cipher character.
STEP-8: Repeat the above steps to generate the entire cipher text.


## PROGRAM
```
import math
ALPHABET_SIZE = 26
A_ORD = ord('A')
def _egcd(a, b):
    if b == 0:
        return (a, 1, 0)
    g, x1, y1 = _egcd(b, a % b)
    return (g, y1, x1 - (a // b) * y1)
def modinv(a, m):
    g, x, _ = _egcd(a % m, m)
    if g != 1:
        raise ValueError(f"No modular inverse for {a} mod {m}")
    return x % m
def _determinant(matrix):
    n = len(matrix)
    if n == 1: return matrix[0][0]
    if n == 2: return matrix[0][0]*matrix[1][1] - matrix[0][1]*matrix[1][0]
    det = 0
    for c in range(n):
        minor = [row[:c] + row[c+1:] for row in matrix[1:]]
        det += ((-1)**c) * matrix[0][c] * _determinant(minor)
    return det
def _matrix_of_minors(matrix):
    n = len(matrix)
    minors = [[0]*n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            minor = [row[:j] + row[j+1:] for idx,row in enumerate(matrix) if idx != i]
            minors[i][j] = _determinant(minor)
    return minors

def _cofactor_matrix(matrix):
    n = len(matrix)
    minors = _matrix_of_minors(matrix)
    return [[(((-1)**(i+j)) * minors[i][j]) for j in range(n)] for i in range(n)]

def _transpose(matrix):
    return [list(col) for col in zip(*matrix)]

def matrix_mod_inv(matrix, mod):
    det = _determinant(matrix) % mod
    det_inv = modinv(det, mod)
    cof = _cofactor_matrix(matrix)
    adj = _transpose(cof)
    return [[(det_inv * (adj[i][j] % mod)) % mod for j in range(len(matrix))] for i in range(len(matrix))]

def _chunkify(lst, size):
    chunks = []
    for i in range(0, len(lst), size):
        chunk = lst[i:i+size]
        if len(chunk) < size:
            chunk += [0] * (size - len(chunk)) 
        chunks.append(chunk)
    return chunks

def _mat_mul_vec(matrix, vec, mod):
    return [sum(matrix[i][j]*vec[j] for j in range(len(vec))) % mod for i in range(len(matrix))]

def _text_to_nums(text):
    cleaned = ''.join([c for c in text.upper() if c.isalpha()])
    return [ord(c) - A_ORD for c in cleaned]

def _nums_to_text(nums):
    return ''.join(chr((n % ALPHABET_SIZE) + A_ORD) for n in nums)

def key_to_matrix(key):
    k = ''.join([c for c in key.upper() if c.isalpha()])
    length = len(k)
    n = int(math.isqrt(length))
    if n*n != length:
        raise ValueError("Key length must be a perfect square (e.g. 4, 9, 16).")
    nums = [ord(c) - A_ORD for c in k]
    return [nums[i*n:(i+1)*n] for i in range(n)]

def encrypt(plaintext, key):
    K = key_to_matrix(key)
    n = len(K)
    nums = _text_to_nums(plaintext)
    chunks = _chunkify(nums, n)
    cipher_nums = []
    for chunk in chunks:
        cipher_nums.extend(_mat_mul_vec(K, chunk, ALPHABET_SIZE))
    return _nums_to_text(cipher_nums)

def decrypt(ciphertext, key):
    K = key_to_matrix(key)
    K_inv = matrix_mod_inv(K, ALPHABET_SIZE)
    n = len(K)
    nums = _text_to_nums(ciphertext)
    chunks = _chunkify(nums, n)
    plain_nums = []
    for chunk in chunks:
        plain_nums.extend(_mat_mul_vec(K_inv, chunk, ALPHABET_SIZE))
    return _nums_to_text(plain_nums)

if __name__ == "__main__":
    key = input("Enter the key (length must be square, e.g. 4, 9, 16 letters): ")
    choice = input("Encrypt or Decrypt? (E/D): ").strip().upper()
    if choice == "E":
        pt = input("Enter plaintext: ")
        print("Ciphertext:", encrypt(pt, key))
    elif choice == "D":
        ct = input("Enter ciphertext: ")
        print("Plaintext:", decrypt(ct, key))
    else:
        print("Invalid choice!")


```
## OUTPUT
<img width="1905" height="958" alt="image" src="https://github.com/user-attachments/assets/750533a4-a058-4f32-a1ad-b2a409affa66" />

## RESULT
Thus the implementation of Vigenere Cipher substitution technique using C program is executed successfully.
