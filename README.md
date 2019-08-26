# stock-market-predictor-assistent-gui
using linear regression machine learning algorithm in python with the help of tkinter 
code-
from tkinter import *
from pandas_datareader import data as dt
import pandas as pd
import datetime
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split as tts
from sklearn.linear_model import LinearRegression
from sklearn.metrics import r2_score
import numpy as np

def fun1():
start = datetime.datetime(2019,1,1)
end = datetime.datetime.now()
if "Microsoft" == v1.get() :
f= dt.DataReader('MSFT', 'tiingo', start, end, access_key='4a368c25cb1695dd1b2fc229035f23df86349215')
if "Google" == v1.get() :
f= dt.DataReader('GOOGL', 'tiingo', start, end, access_key='4a368c25cb1695dd1b2fc229035f23df86349215')
if "Apple" == v1.get() :
f= dt.DataReader('AAPL', 'tiingo', start, end, access_key='4a368c25cb1695dd1b2fc229035f23df86349215')
if "Amazon" == v1.get() :
f= dt.DataReader('AMZN', 'tiingo', start, end, access_key='4a368c25cb1695dd1b2fc229035f23df86349215')
if "Tata Motors" == v1.get() :
f= dt.DataReader('TTM', 'tiingo', start, end, access_key='4a368c25cb1695dd1b2fc229035f23df86349215')

f.reset_index(inplace=True)
f.set_index('date', inplace=True)
f=f[['adjClose', 'adjHigh', 'adjLow', 'adjOpen', 'adjVolume',]]

n= e1.get()
no_days = int(n)
f['newclose']=f.adjClose.shift(no_days)
x=f.drop(['adjClose', 'newclose'],axis=1)
y=f['newclose'].dropna()
x1 = x[:-no_days]
x_pr = x[-no_days:]

t=v2.get()
ts = float(t)
x_tr, x_ts, y_tr, y_ts = tts(x1, y, test_size=ts)

alg = LinearRegression()
alg.fit(x_tr, y_tr)
aws1=alg.score(x_ts, y_ts)

r2_score(y_ts,alg.predict(x_ts))
aws2=alg.predict(x_pr)

global v5
global v6
v5.set(str(aws1))
v6.set(str(aws2))
def fun2():
start = datetime.datetime(2019,1,1)
end = datetime.datetime.now()

if "Microsoft" == v1.get() :
    f= dt.DataReader('MSFT', 'tiingo', start, end, access_key='4a368c25cb1695dd1b2fc229035f23df86349215')
if "Google" == v1.get() :
    f= dt.DataReader('GOOGL', 'tiingo', start, end, access_key='4a368c25cb1695dd1b2fc229035f23df86349215')
if "Apple" == v1.get() :
    f= dt.DataReader('AAPL', 'tiingo', start, end, access_key='4a368c25cb1695dd1b2fc229035f23df86349215')
if "Amazon" == v1.get() :
    f= dt.DataReader('AMZN', 'tiingo', start, end, access_key='4a368c25cb1695dd1b2fc229035f23df86349215')
if "Tata Motors" == v1.get() :
    f= dt.DataReader('TTM', 'tiingo', start, end, access_key='4a368c25cb1695dd1b2fc229035f23df86349215')

f.reset_index(inplace=True)
f.set_index('date', inplace=True)
f=f[['adjClose', 'adjHigh', 'adjLow', 'adjOpen', 'adjVolume',]]

n= e1.get()
no_days = int(n)
f['newclose']=f.adjClose.shift(no_days)
x=f.drop(['adjClose', 'newclose'],axis=1)
y=f['newclose'].dropna()
x1 = x[:-no_days]
x_pr = x[-no_days:]

t=v2.get()
ts = float(t)
x_tr, x_ts, y_tr, y_ts = tts(x1, y, test_size=ts)

alg = LinearRegression()
alg.fit(x_tr, y_tr)
alg.score(x_ts, y_ts)

r2_score(y_ts,alg.predict(x_ts))
alg.predict(x_pr)

f.iloc[-1].name+datetime.timedelta(1)
f['forecast'] = np.nan

prd=alg.predict(x_pr)
lastday=f.iloc[-1].name
for i in prd:
    lastday+=datetime.timedelta(1)
    f.loc[lastday]=[np.nan for _ in range(6)]+[i]
    
f['adjClose'].plot()
f['forecast'].plot()
plt.show()
root = Tk(className="*STOCK MARKET PREDICTOR ASSISTANT")
root.geometry("900x500")

v5 =StringVar(root)
v5.set("0")
v6=StringVar(root)
v6.set("0")

#label
l1=Label(root, font=('times',15,'bold'), text="Stock Name", width=30, height=2)
l1.grid(row=0, column=0)

#option menu
v1 = StringVar(root)
v1.set("Google") #default value
w1 = OptionMenu(root, v1, "Google", "Microsoft", "Apple", "Amazon", "Tata Motors")
w1.config(width=15)
w1.grid(row=0, column=1)

#label
l2=Label(root, font=('times',15,'bold'), text="Test Size", height=2)
l2.grid(row=1, column=0)

#option menu
v2 = StringVar(root)
v2.set("0.2") # default value
w2 = OptionMenu(root, v2, "0.1", "0.2", "0.3", "0.4", "0.5")
w2.config(width=15)
w2.grid(row=1, column=1)

#label
l2=Label(root, font=('times',15,'bold'), text="Predict Days", height=2)
l2.grid(row=2, column=0)

#Entry
v7 =IntVar(root, value=20)
e1=Entry(root, textvariable=v7)
e1.config(width=20)
e1.grid(row=2, column=1)

#button
b1=Button(root, font=('times',15,'bold'), text="Evaluate", bg="red", width=10, height=2, fg="white", command=fun1)
b1.grid(row=3, column=0)

#label
l2=Label(root, font=('times',15,'bold'), text="Accuracy", height=2)
l2.grid(row=4, column=0)

#label
l2=Label(root, font=('times',10,'bold'), textvariable=v5, height=2)
l2.grid(row=4, column=1)

#label
l2=Label(root, font=('times',15,'bold'), text="Last Days Prediction Matrix", height=2)
l2.grid(row=5, column=0)

#label
l2=Label(root, font=('times',10,'bold'), textvariable=v6,)
l2.grid(row=5, column=1)

#button
b2=Button(root, font=('times',15,'bold'), text="Plot Graph", bg="blue", width=10, height=2, fg="white", command=fun2)
b2.grid(row=6, column=0)

root.mainloop()

if 'name' == 'main':
main()
