###question 
   To solve the ideal wave equation(one dimension) 此处输入图片的描述, then set the different initial conditions to the whole string vary via time. Then choose one pointer on the string to observe its vibration via time. Finally, add some damp influence of the string's stiffness into the equation to make the model more realistic.  
   
###answer
   In order to match the variation of propagation of wave, we set the r=1,we set the point at the 0.05 length away from edge to see its vibration. then we do FFT to it signal and get its power spectrum. In this situation, we set the c=300m/s, the string is 2m and ![](),so that we can determine ![](). We have set two different conditions, one is Guassian wave packet,the other is that string is plucked at the middle. 
   we have 
![]()<br>
   come the conclusion
![]()<br>

###code
```
import numpy as np
import matplotlib.pyplot as plt
import math
def wave(x01,x02,k,t,tt):
    c1=[]
    c2=[]
    c3=[]
    ct=[]
    cc=[]
    ca=[]
    ca1=[]
    for i in range(tt):
        x=i*2.0/tt
        tr=math.exp(-k*(x-x01)**2)+0*math.exp(-5*k*(x-x02)**2)
        c1.append(tr)
        c2.append(tr)
        cc.append(x)
    c1[0]=0
    c1[-1]=0
    c2[0]=0
    c1[-1]=0
    ca.append(c2[tt/20])
    ca1.append(0)
    for j in range(t):
        c3.append(c1[0])
        for k in range(1,tt-1):
            c3.append(c2[k+1]+c2[k-1]-c1[k])
        c3.append(c1[0])
        c1=[]
        for k in range(tt):
            c1.append(c2[k])
        c2=[]
        for k in range(tt):
            c2.append(c3[k])
        c3=[]
        ca.append(c2[tt/20])
        ca1.append(j+1)
    return c2,cc,ca,ca1
def waved(x01,L,k,r,M,e,t,tt):
    c1=[]
    c2=[]
    c3=[]
    ct=[]
    cc=[]
    ca=[]
    ca1=[]
    for i in range(tt):
        x=i*L/tt
        tr=math.exp(-k*(x-x01)**2)
        c1.append(tr)
        c2.append(tr)
        cc.append(x)
    c1[0]=0
    c1[-1]=0
    c2[0]=0
    c1[-1]=0
    ca.append(c2[tt/20])
    ca1.append(0)
    for j in range(t):
        c3.append(c1[0])
        c3.append(c1[1])
        for k in range(2,tt-2):
            c3.append((2-2*r**2-6*e*r**2*M**2)*c2[k]-c1[k]+r**2*(1+4*e*M**2)*(c2[k+1]+c2[k-1])-e*r**2*M**2*(c2[k+2]+c2[k-2]))
        c3.append(c1[0])
        c3.append(-c3[-2])
        c1=[]
        for k in range(tt):
            c1.append(c2[k])
        c2=[]
        for k in range(tt):
            c2.append(c3[k])
        c3=[]
        ca.append(c2[tt/20])
        ca1.append(j+1)
    return c2,cc,ca,ca1
"""
#a,b,c,d=waved(1.0,200,0.25,200,0.000001,1000,200)
ax1=plt.subplot(211)
ax2=plt.subplot(212)
plt.sca(ax1)
a,b,c,d=wave(1.0,1.5,200,4000,200)
plt.plot(d,c)
plt.xlabel('time/s')
plt.ylabel('signal')
plt.title('string signal of Guassian packet')
plt.sca(ax2)
a1,b1,c1,d1=waved(1.0,200,0.25,200,0.000001,4000,200)
plt.plot(d1,c1)
plt.xlabel('time/s')
plt.ylabel('signal')
plt.title('damp string signal')
plt.show()
```
