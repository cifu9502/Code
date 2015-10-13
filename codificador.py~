# -*- coding: utf-8 -*-
# <nbformat>3.0</nbformat>



# La primera linea de Codigo indica el texto que se va a tratar , si desea cambiarlo , cambie el el texto en book por la direccion del libro que desea leer. 

# <codecell>
from __future__ import division 
import numpy as np
import string

# La primera linea de Codigo indica el texto que se va a tratar , si desea cambiarlo , cambie el el texto en book por la direccion del libro que desea leer. 


book = 'hp.txt' #link de harry potter, se puede cambiar el libro cambiando el link

#A continuacion se presentan los archivos donde se escribiran los respectivos codigos y textos
huffCode = 'codigo_huffman_' + book
huffText = 'comp_huffman_' + book 

biCode = 'codigo_plano_'+ book
biText = 'comp_plano_' +book

writeHuff = "writeHuff_" + book
writeBin = "writeBin_" + book



infile = open(book, 'r')
#load the full text by lines
text = infile.readlines()

# <codecell>

n = 0
A = [] # Este sera el alfabeto
for x in text:
    #print(list(x))
    line = list(x)
    for y in line:
        if y not in A:
            A.append(y)

#ordeno el alfabeto y lo convierto en un array            
A = np.sort(A)  
A = np.array(A)

# <codecell>

freq1 = [] #Esta sera la distribucion de frequencias de los caracteres en A
#Se obtiene la frecuencia para cada letra del alfabeto 
for a in A:
    num = 0
    for x in text :
        line = list(x)
        num = num + line.count(a)
    freq1.append(num)    
freq1 = np.array(freq1)    


argF = freq1.argsort()
F = freq1[argF] # por comodidad hemos ordenado las frecuencias
Alph = A[argF] # alfabeto ordenado por frecuencias
index = np.linspace(0,len(freq1)-1, len(freq1)).astype(int) #correspondera al indice de las frecuencias
ind = 0

#Hago un pequeño cambio en las fecuencias para evitar que halla frecuencias iguales y el codigo no tenga ningun problema
#Este cambio es tan pequeño que no va a alterar de manera alguna el codigo de huffman definitivo

for i in range(0, len(freq1)):
    x = index[F == F[i]]
    if len(x)>1:
        a = (x-i)*(1/(500*len(x)))
        b = np.zeros((len(freq1)))
        b[x] = a
        F = F +b
        
#Tambien normalizamos F        
F = F/(sum(F))



# A continuacion se deduce el codigo de Huffman. Note que una vez impreso se nota como la dimensión del código decrece al aumentar la frecuencia. 

# <codecell>

freq = F #freq al igual que F guardara las frecuencias ordenadas
f = F  #nos mantendra informados de que valor de F corresponde el numero de f2
f2 = F  #ira sumando las frecuencias cuando se fusionen los elementos del grafo 

codeH =  [] #Aca pondremos el codigo para cada elemento
for i in range(0, len(freq)):
    codeH.append([])

sons =  np.identity(len(freq)) #se convertira en los hijos de cada elemento . Lo inicializamos en la identidad ya que cada uno es hijo de si mismo

#Define el grafo de Huffman y establece el codigo para cada letra
for i in range(0,len(A)-1):
    
    #Se eligen los dos menores y se liga el menor con el segundo menor
    m = np.argmin(f2)
    mini = f[m]
    mini2 = f2[m]
    m1 = np.min(index[F == mini])
    
    #le asigna a cada uno de sus hijos el codigo 0
    for x in range(0,len(freq)):
        if sons[m1,x] != 0 :
            codeH[x].append('0')
    
    #eliminamos el menor
    f = np.delete(f,m)
    f2 = np.delete(f2,m)
    
    n = np.argmin(f2)
    n1 = np.min(index[F == f[n]])
    f2[n] = mini2 +f2[n]
    
    #le asigna a cada uno de sus hijos el codigo 0
    for x in range(0,len(freq)):
        if sons[n1,x] != 0 :
            codeH[x].append('1')
            
    #Le transfiere sus hijos al  mas alto incluido el mismo
    sons[n1] = sons[m1] +  sons[n1]
    #print "( %s , %s , %s , %s)" % (m1, n1,  mini2 , f2[n])

#recuerde que el algoritmo nos da el código escrito al reves así que lo invertimos
for x in codeH:
    x.reverse()

#Asi llamaremos la lista con el codigo de huffman    
codeH    

# <markdowncell>

# A continuacio se elabora el código binario plano, que tiene la misma dimensión para todas las frecuencias.

# <codecell>

#Se obtiene el código binario para el alfabeto ordenado Alph
binCode = [] #asi llamaremos la lista con el codigo plano
for x in index:
    binCode.append([])
    y = list(np.binary_repr(x))
    while len(y) < np.log2(len(freq)) :
        y.insert(0,'0')
    binCode[x] = y  
binCode    

# <markdowncell>

# Se escriben los codigos para cada letra en los archivos respectivos.

# <codecell>

#Se define el metodo codificarHuff, escribe el archivo en con el codigo para cada letra en huffCode
def codificarHuff():
    
    fh = open(huffCode, 'w')
    
    for i in range(0, len(freq)):
        a = Alph[i]
        if(a == '\n'):
            fh.write("'"+ "Enter(/n)" + "' ---->")
        elif(a == '\t'):
            fh.write("'"+ "Tab(/t)" + "' ---->" )
        elif(a == '\r'):
            fh.write("'"+ "Enter(/r)" + "' ---->" )
        else:    
            fh.write("'"+Alph[i] + "' ---->" )
            
        fh.write("".join(codeH[i]))
        
        fh.write('\n')
    fh.close() 
    
    # El siguente metodo codificarBin escribe el archivo en con el codigo para cada letra en bicode 
    
def codificarBin():
    
    fh = open(biCode , 'w')
    #fh2 = open(biText, 'w')
    for i in range(0, len(freq)):
        a = Alph[i]
        if(a == '\n'):
            fh.write("'"+ "Enter(/n)" + "' ---->")
        elif(a == '\t'):
            fh.write("'"+ "Tab(/t)" + "' ---->" )
        elif(a == '\r'):
            fh.write("'"+ "Enter(/r)" + "' ---->" )
        else:    
            fh.write("'"+Alph[i] + "' ---->" )
            
        fh.write("".join(binCode[i]))
            
        fh.write('\n')
    fh.close() 

codificarHuff()    
codificarBin()    

# <markdowncell>

# Se imprimen los libros en ambos codigos.
#     

# <codecell>

#El siguiente codigo escribe el texto en el codigo dado
#2 parametros --- texto : Nombre texto donde se escribe el archivo ----- code: tipo de codigo codeH , o binCode
def write(texto, code): 
    
    fh = open(texto, 'w')
    for x in text :
        line = list(x)
        for y in line:
            fh.write("".join(code[min(index[Alph == y])]))
            
    fh.close()        

write(huffText , codeH)    
write(biText , binCode)    


# <markdowncell>

# Se encuentran las entropias para ambos codigos. En Harry Potter se alcanza a apreciar una clara superioridad del codigo de Huffman

# <codecell>

np.array(freq1) #primero normalizamos las frecuencias, recuerde que no lo hicimos. 
freq1 = np.sort(freq1)
freq1 = freq1/(np.sum(freq1))


#se lee el texto y se obtiene el tamaño de este en bits
infile2 = open(huffText, 'r')
text2 = infile2.readlines()
a1 = len(text2[0])

#se halla la entropia
sum1 = 0
for i in range(0, len(codeH)): 
    sum1 = sum1 + freq1[i] * len(codeH[i])
print "\nLa entropia del codigo de Huffman es " + repr(sum1)    
print "El tamaño del archivo " + huffText + " es de " + repr(a1) + " bits" 


#se lee el texto y se obtiene el tamaño de este en bits
infile2 = open( biText , 'r')
text2 = infile2.readlines()
a2 = len(text2[0])

#se halla la entropia
sum2 = 0
for i in range(0, len(binCode)): 
    sum2 = sum2 + freq[i] * len(binCode[i])
       
print "\nLa entropia del codigo plano es " + repr(sum2) 
print "El tamaño del archivo " + biText + " es de " + repr(a2) + " bits" 

print "\nLa razon entre las dos entropias es: " + repr(sum1/sum2)
print "La razon entre el tamaño de ambos archivos es: " + repr(a1/a2)

print "\nLa diferencia entre ambas razones es: " + repr((sum1/sum2)-(a1/a2))




