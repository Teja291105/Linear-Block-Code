# Linear-Block-Code

# Aim:
To implement and analyze Linear Block Code for error detection and correction in digital communication.

# Tools Required:
PYTHON COLAB
PERSONAL COMPUTER

# Program:
~~~
import numpy as np

pb = [] # Parity matrix
Ik = [] # I_K Matrix
p = []
m = []
h = []
h_dis = []
r_code = []
err = []
col = int(input("Enter the Parity bits : "))
row = int(input("Enter the Message bits : "))

# Generator matrix
for i in range (row):
    p = list(map(int, input(f"Enter the row values : {i+1} (Separated by space) : ").split()))  
    pb.append(p)
p_mat = np.array(pb, dtype=int)
Ik=np.eye(row, dtype=int) # Diagonal Matrix
g_mat = np.hstack((p_mat,Ik)) # Generator Matris

# Codeword length and parity bit length
n, k = g_mat.T.shape

# Possible Message Bits
m = np.array([[1 if (i >> (k - j - 1)) & 1 else 0 for j in range(k)] for i in range(2**k)])

# Codewords and Hamming weights
c = np.mod(np.dot(m, g_mat), 2)

for i, row in enumerate(c):
    h_dis1 = np.sum(row)  # Count number of 1's in the row
    h_dis.append(h_dis1)
h_mat = np.array(h_dis).reshape(1,-1)
#h_mat = np.hstack(h_mat)
d_min = np.min(np.sum(c[1:], axis=1))

# H matrix (Parity-check matrix)
h = p_mat[:, :3]
hp = np.hstack((np.eye(n-k, dtype=int), h.T))
ht = hp.T
zero_row = np.zeros((1, ht.shape[1]), dtype=int)  # Create a row of zeros
hp1 = np.vstack((zero_row, ht))

print('**********')
print('The Generator Matrix is: ')
#for r in p_mat: 
#    print(" ".join(map(str, r)))
#for r in Ik: 
#    print(" ".join(map(str, r)))
for r in g_mat: 
    print(" ".join(map(str, r)))

print('**********')
print(f'Message Bits  Codeword   Hamming Weight')
code_word = np.hstack((m, c, h_mat.T))
for r in range(code_word.shape[0]):
    format_row = " ".join(map(str, code_word[r, :k])) + '\t' + " ".join(map(str, code_word[r, k:n+k])) + '\t' + str(code_word[r, -1])
    print(format_row)

print('**********')
print(f'Minimum Hamming distance : {d_min}')

# Parity Check matrix
print('**********')
print(f'Parity Check Matrix')
for r in hp:
    print(" ".join(map(str, r)))
print('**********')
print(f'Parity Check Matrix Transpose')
for r in hp1:
    print(" ".join(map(str, r)))
#Receive codeword
rc = list(map(int, input(f"Enter the error codeword : ").split()))  
r_code.append(rc)
r_c = np.array(r_code)
#Syndrome Calculation
e = np.mod(np.dot(r_c, ht), 2)
#print('**********')
#print(f'Received codeword Matrix')
#for r in r_c:
#    print(" ".join(map(str, r)))
# Find the Error position
for i in range(n):
    if np.array_equal(e[0], ht[i, :]):
        err = np.eye(n, dtype=int)[i,:]
print(f"The error postion is : " + " ".join(map(str, err)))
n1 = hp1.shape[0]
print(f"The size is : {n1}")
print('**********')
print(f'Syndrome Matrix')
for i in range(n1):
    combined_row = np.concatenate((hp1[i, :], np.eye(n1, dtype=int)[i,:]))
    formatted_row = " ".join(map(str, combined_row[:3])) + '\t' + " ".join(map(str, combined_row[k:]))
    print(f'{formatted_row}')
print('**********')
print(f"Syndeome of given received codeword is : " + " ".join(map(str, e[0])))

# Correct the error in the received codeword
add = err + rc
add = np.array(add)
add1 = add % 2
print(f"The correct codeword is : " + " " .join(map(str,add1)))  
~~~
# Manual calculation :


![WhatsApp Image 2025-04-12 at 15 46 24_5fc9564d](https://github.com/user-attachments/assets/da757a27-d2ac-4f3e-9534-0774afcc7e46)
![WhatsApp Image 2025-04-12 at 15 47 18_b1476647](https://github.com/user-attachments/assets/b637a8d7-32d8-4124-bc14-ce69749d82b7)
# Output:
![image](https://github.com/user-attachments/assets/a6ce31c1-56e0-4735-86b2-8014a33603a4)
![image](https://github.com/user-attachments/assets/acc478a7-54e7-4900-85d2-00149d22a553)
# Results:
The Linear Block Code was successfully implemented using a (6,3) block code. A message of 3 bits was encoded into a 6-bit codeword using the generator matrix. An error was introduced manually, and the received codeword was checked using the parity check matrix.
