###Wave equation under  small interference
###question
Music is an essential part of our life. A large part of the stringed instrument, for example, a guitar
<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171230-013342%402x.png)<br>
The guitar is a fretted musical instrument that usually has six strings.[1] The sound is projected either acoustically, using a hollow wooden or plastic and wood box (for an acoustic guitar), or through electrical amplifier and a speaker (for an electric guitar). It is typically played by strumming or plucking the strings with the fingers, thumb or fingernails of the right hand or with a pick while fretting (or pressing against the frets) the strings with the fingers of the left hand. The guitar is a type of chordophone, traditionally constructed from wood and strung with either gut, nylon or steel strings and distinguished from other chordophones by its construction and tuning. The modern guitar was preceded by the gittern, the vihuela, the four-course Renaissance guitar, and the five-course baroque guitar, all of which contributed to the development of the modern six-string instrument.<br>
This task is to use numerical methods to solve wave functions for the wave propagation of strings, such as the vibration of a guitar string.<br>
###
we have the central equation of wave 
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171231-035703%402x.png)<br>
Suppose the interference is very small and the moving distance of string is very small
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171231-035708%402x.png)<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20180101-012446%402x.png)<br>
come the conclusion
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171231-035722%402x.png)<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20180101-012452%402x.png)<br>
For the convenience of computing in the numerical methods,  we can set up
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171231-035729%402x.png)<br>
### coed

###coed
```import numpy as np
import pylab as pl
import math 

class propagate():

    def __init__(self,c,dx,length,time,k):
        self.c=c
        self.dx=dx
        self.dt=dx/c
        self.steps=int(length/dx)
        self.time=int(time)
        self.k=k
        self.y=[[0]*(self.steps+1) for i in range(self.time)] 
        for i in range(int(self.steps/2)):
            self.y[0][i]=i*self.dx
            self.y[1][i]=i*self.dx
        for i in range(int(self.steps/2),self.steps+1):
            self.y[0][i]=(self.steps-i)*self.dx
            self.y[1][i]=(self.steps-i)*self.dx

        self.y[0][0]=0
        self.y[1][0]=0
        self.y[0][self.steps]=0
        self.y[1][self.steps]=0
        self.y1=[]
        self.y1.append(self.y[0][5])
        self.y1.append(self.y[1][5])
        self.t=[0]
        self.t.append(self.dt*1)
        
    def iteration(self):
        r=self.c*self.dt/self.dx
        for n in range(self.time-2):
            for i in range(1,self.steps):
                self.y[n+2][i]=2*(1-r**2)*self.y[n+1][i]-self.y[n][i]+r**2*(self.y[n+1][i+1]+self.y[n+1][i-1])
                self.y[n+2][0]=0
                self.y[n+2][self.steps]=0
            self.y1.append(self.y[n+2][5])
            self.t.append((n+2)*self.dt)
        return [self.y1,self.t,self.dt,self.y[1]]
    
a=propagate(300,0.01,1,1300,1000)
b=a.iteration()
pl.subplot(121)
pl.plot(b[1],b[0])
pl.xlabel("Time(s)")
pl.ylabel("Signal")
pl.title("String signal versus time")

pl.subplot(122)
y=abs(np.fft.rfft(b[0]))**2
x=np.linspace(0,int(1/b[2]/2),len(y))
pl.plot(x,y)
pl.xlabel("Frequency(Hz)")
pl.ylabel("Powre")
pl.xlim(0,3000)
pl.title("Power spectrum")


pl.plot(range(101),b[3])

pl.show()
```
iamge
![]()<br>
![]()<br>
