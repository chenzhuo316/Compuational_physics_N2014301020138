###Problem:
Use the Euler method to calculate cannon shell trajectories ignoring both air drag 
and the effect of air density(actually,ignoring the former authomatically rules out the latter).
Compare your results with those in Figure 2.4,and with the exact solution.<br>
###answer
we can get the equation when ignore air drag and air density <br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171015-175809%402x.png)<br>
find the solution of equation<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171015-150400%402x.png)<br>
###code
```import pylab as pl
import math as mt
class cannon_shells:
    def __init__(self,initial_velocity=0.7,g=0.0098,range=0,height=0,time_step=0.01,initial_angle=30.0,da=5.0):
        self.v=[initial_velocity]
        self.a=[initial_angle]
        self.g=g
        self.dt=time_step
        self.da=da
        self.x=[range]
        self.y=[height]

    def run(self):
        _a=self.a[0]
        while(_a<=60):    
            _a+=self.da
            self.a.append(self.a[-1]+self.da)
            t = 2 * mt.sin(_a / 180.0 * mt.pi) * self.v[0]/self.g
            vx=mt.cos(_a / 180.0 * mt.pi) * self.v[0]
            vy=mt.sin(_a / 180.0 * mt.pi) * self.v[0]
            _time=0
            xi = [0]
            yi = [0]
            while(_time<t):
                xi.append(xi[-1]+self.dt*vx)
                yi.append(yi[-1]+self.dt*vy)
                vy=vy - self.g * self.dt
                _time+=self.dt
            self.x = self.x + xi
            self.y = self.y + yi

    def show_results(self):
        # coding:utf-8
        font = {'family': 'yahei',
                'color':  'darkred',
                'weight': 'normal',
                'size': 16,
        }
        font2 = {'family': 'yahei',
                'color':  'darkred',
                'weight': 'normal',
                'size': 16,
        }
        pl.title('Trajectory of cannon shell')
        pl.text(0.95 * max(self.x), 0.95 * max(self.y),'No drag and density', fontdict=font)
        pl.plot(self.x,self.y)
        pl.xlabel('x ($km$)')
        pl.ylabel('y ($km$)')
        pl.show()

a = cannon_shells()
a.run()
a.show_results()
```
###image
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171015-180812%402x.png)<br>
range from 0 to 45
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171015-180531%402x.png)<br>
range from 45 to 90<br>
###conclusion
1炮弹在相同的初速度和抛射角抛射的情况下，不考虑空气阻力和空气质量运动的最大高度和最远距离明显是要大于考虑空气阻力下的情况。
2角度为45度时候，最远距离最大。
3一般情况下，空气阻力和物体运动的速度的平方成正比，所以在速度较小的情况下区别也少于高速运动。
        
