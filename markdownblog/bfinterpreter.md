以下是我用Python写的BrainF**k执行器：
```python
#! /usr/bin/python
import sys

memory   = [0 for i in range(4208)]
code     = ""
cursor   = 0
usrinput = ""
def eachchar(char, codeindex):
    global usrinput
    global cursor
    if char == ',':
        if usrinput == "":
            usrinput = input()
        nextchar = usrinput[0]
        newinput = ""
        for index in range(1, len(usrinput)):
            newinput += usrinput[index]
        memory[cursor] = ord(nextchar) % 256
    elif char == '.':
        print(chr(memory[cursor]))
    elif char == '+':
        memory[cursor] += 1
        memory[cursor] %= 256
    elif char == '-':
        if memory[cursor] == 0:
            memory[cursor] = 255
        else:
            memory[cursor] -= 1
    elif char == '>':
        if cursor == 4207:
            cursor = 0
        else:
            cursor += 1;
    elif char == '<':
        if cursor == 0:
            cursor = 4207
        else:
            cursor -= 1
    elif char == '[':
        numpar = 0
        endindex = -1
        for index in range(codeindex, len(code)):
            if code[index] == '[':
                numpar += 1
            elif code[index] == ']':
                numpar -= 1
            if numpar == 0:
                endindex = index
                break
        if numpar == 0:
            innercode = ""
            for index in range(codeindex + 1, endindex):
                innercode += code[index]
            while memory[cursor] != 0:
                runcode(innercode, codeindex + 1)
def runcode(innercode, startindex):
    for index in range(len(innercode)):
        eachchar(innercode[index], startindex + index)
with open(sys.argv[1]) as file:
    code = file.read()
runcode(code, 0)
```