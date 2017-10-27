###question<br>
In constructing the Poincaré section in Figure 3.9 we plotted points only at times that were in phase with the drive force; that is, at times t=2πn/Ωd , where n is an integer. At these values of t the driving force passed through zero [see(3.18)]. However, we could jusi as easily have chosen to make the plot at times corresponding to a maximum of the drive force, or at times π/4 out-of-phase with this force, etc. Construct the Poincaré sections for these cases and compare them with Figure 3.9.
###answer
we obtain the equation of motion<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171027-233052%402x.png)<br>
come the conclusion<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171027-233106%402x.png)<br>
Euler-Cromer method
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171027-233121%402x.png)<br>
###coed<br>
```
import pylab as pl
import math
class  Poincare_sections:
    """
    Program 3.12
    Construct the poincare sections for some different cases
    and compare them with Figure 3.9
    """
    def __init__(self,friction_coefficient=0.5,gravity=9.8,rope_length=9.8,angular_frequency_of_driving_force=2/3,ampulitude=1.2,time_of_step=0.04):
        self.q=friction_coefficient
        self.l=rope_length
        self.g=gravity
        self.Ω=angular_frequency_of_driving_force
        self.FD=ampulitude
        self.dt=time_of_step
        self.t=[0]
        self.ω=[0]
        self.Θ=[0.2]
        self.t2=[]
        self.ω2=[]
        self.Θ2=[]
                      




    def calculate(self):
        i=0
        while self.t[i]<=5000:     
            if self.Θ[i]>math.pi:   
                self.Θ[i]=self.Θ[i]-2*math.pi
            if self.Θ[i]<-math.pi:
                self.Θ[i]=self.Θ[i]+2*math.pi
            else:
                self.Θ[i]=self.Θ[i]
    
            temp_ω=self.ω[i]+(-self.g/self.l*math.sin(self.Θ[i])-self.q*self.ω[i]+self.FD*math.sin(self.Ω*self.t[i]))*self.dt
            self.ω.append(temp_ω)
            temp_Θ=self.Θ[i]+self.ω[i+1]*self.dt
            self.Θ.append(temp_Θ)
            self.t.append(self.t[i]+self.dt)
            
            x=int(self.Ω*self.t[i]/(2*math.pi))
            y=self.Ω*self.t[i]/(2*math.pi)
            if abs(x-y)<0.01:
                self.t2.append(self.t[i])  
                self.ω2.append(self.ω[i])
                self.Θ2.append(self.Θ[i])
            i+=1

            
        
            
            
    def show_result(self):      
        pl.plot(self.Θ2,self.ω2,".")
        pl.xlabel("Θ(radians)")
        pl.ylabel("ω(radians/s")
        pl.xlim(-4,4)
        pl.ylim(-2,1)
        pl.show()
a=Poincare_sections()
a.calculate()
a.show_result()
```
###iamge<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171027-234424%402x.png)<br>
![]()<br>
![]()<br>
