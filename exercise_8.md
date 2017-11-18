###In this text, we compare the planetary orbits with different values of β in Fg=GMsMe/(r^β). Further more, we study orbits with the same value of β and compare the behavior with different values of the ellipticity of the orbit, then we would find the stability of the orbits depends on the ellipticity when is not equal to 2.
###answer
According to Newton's law of graviation the magnitude pf this force is given by<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171118-145857%402x.png)<br>
and Newton's second law given by<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171118-145930%402x.png)<br>
we can come the conclusion<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171118-145940%402x.png)<br>
###code
```
import math
import matplotlib.pyplot as pl
class planet_orbit:
    def __init__(self,time_step=0.001,init_x=1,init_y=0,init_vx=0,init_vy=1.3*math.pi,
        beta=2.0,T_a=0.998,eccentricity=0.017,total_time=2):
        self.Kep = 4*pow(math.pi,2)/T_a
        self.e = eccentricity
        # init_vy = 2*math.pi/(1+self.e)*math.sqrt((1-self.e)*init_x/T_a)
        self.dt = time_step
        self.r = []
        self.x = [init_x]
        self.y = [init_y]
        self.vx = [init_vx]
        self.vy = [init_vy]
        self.t = total_time
        self.power = beta + 1
    def run(self):
        loop = True
        i = 0
        while(loop):
            self.r.append(pow(((self.x[i])**2+(self.y[i])**2),0.5))
            self.vx.append(self.vx[i]-(self.Kep*self.x[i]*self.dt)/(self.r[i]**self.power))
            self.x.append(self.x[i]+self.vx[i+1]*self.dt)
            self.vy.append(self.vy[i]-(self.Kep*self.y[i]*self.dt)/(self.r[i]**self.power))
            self.y.append(self.y[i]+self.vy[i+1]*self.dt)
            i += 1
            if (i>=(self.t/self.dt-1)):
                loop = False
    def show_result(self):
        pl.plot(self.x,self.y,'.',label="beta = $self.beta$")
        pl.title('planet orbiting the Sun')
        pl.xlabel('x(AU)')
        pl.ylabel('y(AU)')
        pl.axis('equal' )
        pl.legend()
        pl.show()
a=planet_orbit()
a.run()
a.show_result()
```
###change initial velocity when β is fixed
```
import math
import matplotlib.pyplot as pl
class planet_orbit:
    def __init__(self,time_step=0.001,init_x=1,init_y=0,init_vx=0,init_vy=1.3*math.pi,
        beta=2.0,T_a=0.998,eccentricity=0.017,total_time=2):
        self.Kep = 4*pow(math.pi,2)/T_a
        self.e = eccentricity
        # init_vy = 2*math.pi/(1+self.e)*math.sqrt((1-self.e)*init_x/T_a)
        self.dt = time_step
        self.r = []
        self.x = [init_x]
        self.y = [init_y]
        self.vx = [init_vx]
        self.vy = [init_vy]
        self.t = total_time
        self.power = beta + 1
    def run(self):
        loop = True
        i = 0
        while(loop):
            self.r.append(pow(((self.x[i])**2+(self.y[i])**2),0.5))
            self.vx.append(self.vx[i]-(self.Kep*self.x[i]*self.dt)/(self.r[i]**self.power))
            self.x.append(self.x[i]+self.vx[i+1]*self.dt)
            self.vy.append(self.vy[i]-(self.Kep*self.y[i]*self.dt)/(self.r[i]**self.power))
            self.y.append(self.y[i]+self.vy[i+1]*self.dt)
            i += 1
            if (i>=(self.t/self.dt-1)):
                loop = False
    def show_result(self):
        pl.plot(self.x,self.y,'.',label="beta = $self.beta$")
        pl.title('planet orbiting the Sun')
        pl.xlabel('x(AU)')
        pl.ylabel('y(AU)')
        pl.axis('equal' )
        pl.legend()
        pl.show()
a=planet_orbit()
a.run()
a.show_result()
```
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171118-152430%402x.png)
