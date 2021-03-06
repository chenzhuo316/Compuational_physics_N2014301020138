###Question 
Investigate the Lyapunov exponent of the stadium billiard for several values of α. You can do this qualitatively 
by examining the behavior for only one set of initial conditions for each value of α you consider, 
or more quantitatively by averaging over a range of initial conditions for each value of α <br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/370px-Chaos_Sensitive_Dependence.svg.png)<br>
###background
Chaos theory is the field of study in mathematics that studies the behavior of dynamical systems that are highly sensitive to initial conditions—a response popularly referred to as the butterfly effect.Small differences in initial conditions (such as those due to rounding errors in numerical computation) yield widely diverging outcomes for such dynamical systems, rendering long-term prediction impossible in general.This happens even though these systems are deterministic, meaning that their future behavior is fully determined by their initial conditions, with no random elements involved. In other words, the deterministic nature of these systems does not make them predictable. This behavior is known as deterministic chaos, or simply chaos. The theory was summarized by Edward Lorenz as: 
Chaos: When the present determines the future, but the approximate present does not approximately determine the future. <br>
###Round ball table
```
from numpy import * 
import matplotlib.pyplot as plt
# class: BILLIARD solves for a stadium-shaped boundary
# where:
#        x0, y0, vx0, vy0: initial position of billiard 
#        dt, time : time step and total time
#        alpha: the length cube region 
class BILLIARD(object):
    def __init__(self,_alpha=0.,_r=1.,_x0=0.2,_y0=0.,_vx0=0.6,_vy0=0.8,_dt=0.001,_time=300):
        self.alpha, self.r, self.dt, self.time, self.n = _alpha, _r, _dt, _time, int(_time/_dt)
        self.x, self.y, self.vx, self.vy = [_x0], [_y0], [_vx0], [_vy0]
    def cal(self):            # use Euler method to solve billiard motion
        for i in range(self.n):
            self.nextx = self.x[-1]+self.vx[-1]*self.dt
            self.nexty = self.y[-1]+self.vy[-1]*self.dt
            self.nextvx, self.nextvy = self.vx[-1], self.vy[-1]
            if (self.nexty>self.alpha*self.r and self.nextx**2+(self.nexty-self.alpha*self.r)**2>self.r**2) \
                    or (self.nexty<-self.alpha*self.r and self.nextx**2+(self.nexty+self.alpha*self.r)**2>self.r**2) \
                    or (self.nextx>self.r) \
                    or (self.nextx<-self.r):
                self.nextx=self.x[-1]
                self.nexty=self.y[-1]
                while not( \
                        (self.nexty>self.alpha*self.r and self.nextx**2+(self.nexty-self.alpha*self.r)**2>self.r**2) \
                        or (self.nexty<-self.alpha*self.r and self.nextx**2+(self.nexty+self.alpha*self.r)**2>self.r**2) \
                        or (self.nextx>self.r) \
                        or (self.nextx<-self.r)):
                    self.nextx=self.nextx+self.nextvx*self.dt/100
                    self.nexty=self.nexty+self.nextvy*self.dt/100
                if self.nexty>self.alpha*self.r:
                    self.v = array([self.nextvx,self.nextvy])
                    self.norm =  1/self.r*array([self.nextx, self.nexty-self.alpha*self.r])
                    self.v_perpendicular = dot(self.v, self.norm)*self.norm
                    self.v_parrallel = self.v-self.v_perpendicular
                    self.v_perpendicular= -self.v_perpendicular
                    self.nextvx, self.nextvy= (self.v_parrallel+self.v_perpendicular)[0],(self.v_parrallel+self.v_perpendicular)[1]
                elif self.nexty<-self.alpha*self.r:
                    self.v = array([self.nextvx,self.nextvy])
                    self.norm =  1/self.r*array([self.nextx, self.nexty+self.alpha*self.r])
                    self.v_perpendicular = dot(self.v, self.norm)*self.norm
                    self.v_parrallel = self.v-self.v_perpendicular
                    self.v_perpendicular= -self.v_perpendicular
                    self.nextvx, self.nextvy= (self.v_parrallel+self.v_perpendicular)[0],(self.v_parrallel+self.v_perpendicular)[1]
                else:
                    self.nextvx, self.nextvy= -self.nextvx, self.nextvy
            self.x.append(self.nextx)
            self.y.append(self.nexty)
            self.vx.append(self.nextvx)
            self.vy.append(self.nextvy)
    def plot_position(self,_ax):        # give trajectory plot
        _ax.plot(self.x,self.y,'-b',label=r'$\alpha=$'+'  %.2f'%self.alpha)
        _ax.plot([self.r]*10,linspace(-self.alpha*self.r,self.alpha*self.r,10),'-k',lw=5)
        _ax.plot([-self.r]*10,linspace(-self.alpha*self.r,self.alpha*self.r,10),'-k',lw=5)
        _ax.plot(self.r*cos(linspace(0,pi,100)),self.r*sin(linspace(0,pi,100))+self.alpha*self.r,'-k',lw=5)
        _ax.plot(self.r*cos(linspace(pi,2*pi,100)),self.r*sin(linspace(pi,2*pi,100))-self.alpha*self.r,'-k',lw=5)
    def plot_phase(self,_ax,_secy=0):        # give phase-space plot
        self.secy=_secy
        self.phase_x, self.phase_vx = [], []
        for i in range(len(self.x)):
            if abs(self.y[i]-self.secy)<1E-3 :
                self.phase_x.append(self.x[i])
                self.phase_vx.append(self.vx[i])
        _ax.plot(self.phase_x,self.phase_vx,'ob',markersize=2,label=r'$\alpha=$'+'  %.2f'%self.alpha)
# give a trajectory and phase space plot          
fig= plt.figure(figsize=(10,10))
ax1=plt.axes([0.1,0.55,0.35,0.35])
ax2=plt.axes([0.6,0.55,0.35,0.35])
ax3=plt.axes([0.1,0.1,0.35,0.35])
ax4=plt.axes([0.6,0.1,0.35,0.35])
ax1.set_xlim(-1.1,1.1)
ax2.set_xlim(-1.1,1.1)
ax3.set_xlim(-1.1,1.1)
ax4.set_xlim(-1.1,1.1)
ax1.set_ylim(-1.1,1.1)
ax2.set_ylim(-1.1,1.1)
ax3.set_ylim(-1.1,1.1)
ax4.set_ylim(-1.1,1.1)
ax1.set_xlabel(r'$x (m)$',fontsize=18)
ax1.set_ylabel(r'$y (m)$',fontsize=18)
ax1.set_title('Circular stadium: trajectory',fontsize=18)
ax2.set_xlabel(r'$x (m)$',fontsize=18)
ax2.set_ylabel(r'$v_x (m/s)$',fontsize=18)
ax2.set_title('Circular stadium: phase-space',fontsize=18)
ax3.set_xlabel(r'$x (m)$',fontsize=18)
ax3.set_ylabel(r'$y (m)$',fontsize=18)
ax3.set_title('Billiard'+r'$\alpha=0.065$'+': trajectory',fontsize=18)
ax4.set_xlabel(r'$x (m)$',fontsize=18)
ax4.set_ylabel(r'$v_x (m/s)$',fontsize=18)
ax4.set_title('Billiard '+r'$\alpha=0.065$'+': phase-space',fontsize=18)
cmp=BILLIARD()
cmp.cal()
cmp.plot_position(ax1)
cmp.plot_phase(ax2)
cmp=BILLIARD(0.065)
cmp.cal()
cmp.plot_position(ax3)
cmp.plot_phase(ax4)
plt.show()
```
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171112-165854%402x.png)<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171112-165906%402x.png)<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171112-170651%402x.png)<br>
###Ellipse ball table
```
from numpy import * 
import matplotlib.pyplot as plt
# class: BILLIARD solves for a elliptical-bounded boundary
# where:
#        x0, y0, vx0, vy0: initial position of billiard 
#        dt, time : time step and total time
class BILLIARD(object):
    def __init__(self,_x0=sqrt(2),_y0=0.,_vx0=0,_vy0=1,_dt=0.001,_time=500):
        self.dt, self.time, self.n = _dt, _time, int(_time/_dt)
        self.x, self.y, self.vx, self.vy = [_x0], [_y0], [_vx0], [_vy0]
    def cal(self):            # use Euler method to solve billiard motion
        for i in range(self.n):
            self.nextx = self.x[-1]+self.vx[-1]*self.dt
            self.nexty = self.y[-1]+self.vy[-1]*self.dt
            self.nextvx, self.nextvy = self.vx[-1], self.vy[-1]
            if self.nextx**2/3+self.nexty**2>1:
                self.nextx=self.x[-1]
                self.nexty=self.y[-1]
                while not(self.nextx**2/3+self.nexty**2>1):
                    self.nextx=self.nextx+self.nextvx*self.dt/100
                    self.nexty=self.nexty+self.nextvy*self.dt/100
                self.norm=array([self.nextx,3*self.nexty])
                self.norm=self.norm/sqrt(dot(self.norm,self.norm))
                self.v=array([self.nextvx,self.nextvy])
                self.v_perpendicular=dot(self.v,self.norm)*self.norm
                self.v_parrallel=self.v-self.v_perpendicular
                self.v_perpendicular=-self.v_perpendicular
                [self.nextvx,self.nextvy]=self.v_parrallel+self.v_perpendicular
            self.x.append(self.nextx)
            self.y.append(self.nexty)
            self.vx.append(self.nextvx)
            self.vy.append(self.nextvy)
    def plot_position(self,_ax):        # give trajectory plot
        _ax.plot(self.x,self.y,'-b',label='Ellipse'+r'$a=2,b=1$')
        _ax.plot(sqrt(3)*cos(linspace(0,2*pi,200)),sin(linspace(0,2*pi,200)),'-k',lw=5)
    def plot_phase(self,_ax,_secy=0):        # give phase-space plot
        self.secy=_secy
        self.phase_x, self.phase_vx = [], []
        for i in range(len(self.x)):
            if abs(self.y[i]-self.secy)<1E-3 and abs(self.vx[i])<0.95:
                self.phase_x.append(self.x[i])
                self.phase_vx.append(self.vx[i])
        _ax.plot(self.phase_x,self.phase_vx,'ob',markersize=2,label='Ellipse'+r'$a=2,b=1$')
# give a trajectory and phase space plot        
fig= plt.figure(figsize=(10,4))
ax1=plt.axes([0.1,0.15,0.35,0.7])
ax2=plt.axes([0.6,0.15,0.35,0.7])
ax1.set_xlim(-2,2)
ax1.set_ylim(-1.1,1.1)
ax2.set_xlim(-2,2)
ax2.set_ylim(-1.1,1.1)  
ax1.set_title('Elliptical boundary: '+r'$a^2 =3,b^2 = 1$',fontsize=18)
ax2.set_title('Phase-space: '+r'$a^2 =3,b^2 = 1$',fontsize=18)
ax1.set_xlabel(r'$x(m)$',fontsize=18)
ax1.set_ylabel(r'$y(m)$',fontsize=18)
ax2.set_xlabel(r'$x(m)$',fontsize=18)
ax2.set_ylabel(r'$v_x (m/s)$',fontsize=18)
cmp=BILLIARD()
cmp.cal()
cmp.plot_position(ax1)
cmp.plot_phase(ax2)
plt.show()
```
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171112-171432%402x.png)<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171112-171509%402x.png)<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171112-171518%402x.png)<br>
###Square ball table
```
import pylab as pl
import math
class billiard_on_a_square_table():
    """This is a continuation of the ttrajectory of a billiard
    on a square table
    """
    def __init__(self,time_step=0.01,x0=0.2,y0=0,vx0=2,vy0=4,steps=20000):
        self.x=[x0]
        self.y=[y0]
        self.vx=[vx0]
        self.vy=[vy0]
        self.dt=time_step
        self.t=[0]
        self.steps=steps
    def backtrack(self,condition,x,y,vx,vy,t):
        m=0
        while eval(condition):
            x=x+vx*self.dt/100
            y=y+vy*self.dt/100
            m+=1   
        return x,y,self.dt/100*m+t
    def calculate(self):
        for i in range(self.steps):
            temp_x=self.x[i]+self.vx[i]*self.dt
            temp_y=self.y[i]+self.vy[i]*self.dt
            temp_vx=self.vx[i]
            temp_vy=self.vy[i]
            self.x.append(temp_x)
            self.y.append(temp_y)
            self.vx.append(temp_vx)
            self.vy.append(temp_vy)
            self.t.append(self.t[i]+self.dt)
            if(self.x[i+1]>1):
                self.x[i+1],self.y[i+1],self.t[i+1]=self.backtrack("x<=1",self.x[i],self.y[i],self.vx[i],self.vy[i],self.t[i])
                self.vx[i+1]=-self.vx[i+1]
            if(self.x[i+1]<-1):
                self.x[i+1],self.y[i+1],self.t[i+1]=self.backtrack("x>=1",self.x[i],self.y[i],self.vx[i],self.vy[i],self.t[i])
                self.vx[i+1]=-self.vx[i+1]
            if(self.y[i+1]<-1):
                self.x[i+1],self.y[i+1],self.t[i+1]=self.backtrack("y>=-1",self.x[i],self.y[i],self.vx[i],self.vy[i],self.t[i])
                self.vy[i+1]=-self.vy[i+1]
            if(self.y[i+1]>1):
                self.x[i+1],self.y[i+1],self.t[i+1]=self.backtrack("y<=1",self.x[i],self.y[i],self.vx[i],self.vy[i],self.t[i])
                self.vy[i+1]=-self.vy[i+1]
        return self.x,self.y
    def show_result(self):
        pl.plot(self.x,self.y,"b")
        pl.xlabel('x')
        pl.ylabel('y')
        pl.xlim(-1,1)
        pl.ylim(-1,1)
        pl.title('trajectory of a billiard on a square table')
        pl.show()
a=billiard_on_a_square_table()
a.calculate()
a.show_result()
```
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171112-172559%402x.png)
###summary
桌球在方形边界的运动轨迹，不同初始的条件运动出来的轨迹图形也大不一样，具有对称性，但并不是所有的VxVy的比值能填满整个球桌。
桌球在ellipse—shaped 桌子上运动的轨迹图形，不同初始条件，不同α值对图形都有很大影响，并有， 
当α=0，即为圆形边界时，中心会有一个小圈空白，图形对称，取其他值时，α越大，图形不对称性越强，且中心的空白消失 
桌球在一般的ellipse-shaped 桌子上运动时的取不同α值的庞加莱相图，并可以看到，随着α变大，相图越来越杂乱，越来越混沌
