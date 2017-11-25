###Chaotic tumbling of Hyperion Exercise4.20   
###code
```python
from math import pi,sqrt,sin,cos
import matplotlib.pyplot as plt
def hyperion(theta0):
    # set the mass of saturn
    gm=4*pi**2
    # initialize position of center of mass, angle, angle velocity
    xc=[1]
    yc=[0]
    theta=[theta0]
    avelo=[0]
    vxc=[0]
    vyc=[2*pi]
    # get the trails
    dt=0.0001
    t=[0]
    while t[-1]<=15:
        r=sqrt((xc[-1])**2+(yc[-1])**2)
        vxc_new=vxc[-1]-gm*xc[-1]*dt/r**3
        vyc_new=vyc[-1]-gm*yc[-1]*dt/r**3
        avelo_new=avelo[-1]-12*(pi**2)*(xc[-1]*sin(theta[-1])-yc[-1]*cos(theta[-1]))*(xc[-1]*cos(theta[-1])+yc[-1]*sin(theta[-1]))*dt/r**5
        vxc.append(vxc_new)
        vyc.append(vyc_new)
        avelo.append(avelo_new)
        xc.append(xc[-1]+vxc[-1]*dt)
        yc.append(yc[-1]+vyc[-1]*dt)
        theta_new=theta[-1]+avelo[-1]*dt
        if theta_new<-pi:
            theta_new+=2*pi
        if theta_new>pi:
            theta_new-=2*pi
        theta.append(theta_new)
        t.append(t[-1]+dt)
    return theta,t
t=hyperion(0)[1]
the1=hyperion(0)[0]
the2=hyperion(0.01)[0]
dthe=[]
plt.plot(t,the1)
plt.title(' theta vs time')
plt.xlabel('time(yr)')
plt.ylabel('theta(radians)')
plt.xlim(0,15)
plt.show()
```
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171125-181753%402x.png)<br>
###
```python
from math import pi,sqrt,sin,cos
import matplotlib.pyplot as plt
def hyperion(theta0):
    # set the mass of saturn
    gm=4*pi**2
    # initialize position of center of mass, angle, angle velocity
    xc=[1]
    yc=[0]
    theta=[theta0]
    avelo=[0]
    vxc=[0]
    vyc=[5]
    # get the trails
    dt=0.0001
    t=[0]
    while t[-1]<=15:
        r=sqrt((xc[-1])**2+(yc[-1])**2)
        vxc_new=vxc[-1]-gm*xc[-1]*dt/r**3
        vyc_new=vyc[-1]-gm*yc[-1]*dt/r**3
        avelo_new=avelo[-1]-12*(pi**2)*(xc[-1]*sin(theta[-1])-yc[-1]*cos(theta[-1]))*(xc[-1]*cos(theta[-1])+yc[-1]*sin(theta[-1]))*dt/r**5
        vxc.append(vxc_new)
        vyc.append(vyc_new)
        avelo.append(avelo_new)
        xc.append(xc[-1]+vxc[-1]*dt)
        yc.append(yc[-1]+vyc[-1]*dt)
        theta_new=theta[-1]+avelo[-1]*dt
        theta.append(theta_new)
        t.append(t[-1]+dt)
    return theta,t
t=hyperion(0)[1]
the1=hyperion(0)[0]
the2=hyperion(0.01)[0]
dthe=[]
for i in range(len(the1)):
    dthe.append(abs(the2[i]-the1[i]))
plt.plot(t,dthe)
plt.semilogy(t,dthe)
plt.title('delta theta vs time')
plt.xlabel('time(yr)')
plt.ylabel('dtheta(radians)')
plt.xlim(0,15)
plt.show()
```
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171125-181907%402x.png)<br>
###

