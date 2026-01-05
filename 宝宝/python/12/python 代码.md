```python
import tkinter as tk
win=tk.TK()
win.title()
win.geometry()
win.resizable()
result=tk.StringVar()
result.set(0)
entry=tk.Entry(win,font=(),justify="",
               textvariable=result)
entry.place(x=25,y=20,width=190,height=50)
def calc(event):
    global num1,num2,opr
    char = event.widget["text"]
    if'()'<=char<="9":
        result.set(int(result.get())*10+int(char))
    if char in"+-*/":
        num1=result.get()
        opr=char
        result.set(0)
    if char=="=":
        if opr=="/":
            num2=result.get()
            try:
                result.set(eval(num1+opr+num2))
            except ZeroDivisionError:
                result.set("除数不能为0")
    if char=="C":
        result.set("0")
btnstr="789/456*123-C0=+"
for i in range(16):
    
```