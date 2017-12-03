###In this text, we study the electric potentials and fields of the capacitor problem. We use the symmetry of the capacitor problem to write a program that obtains the result by calculating the potential in only half od one quadrant of the x-y plane. Further more, we use both the Jacobi method and the SOR method to solve the problem, and compare the number of iterations for a fixed accuracy.
###
The space potential without any charge satisfies the Laplasse equation<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171203-130509%402x.png)<br>
come the conclusion
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171203-130518%402x.png)<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171203-130524%402x.png)<br>
###
```
from numpy import *
import mpl_toolkits.mplot3d
import matplotlib.pyplot as plt
from matplotlib import cm
from matplotlib.ticker 
import LinearLocator, FormatStrFormatter
class Electric():
    def __init__(self, V1=1, V2=1, V_boundary=0, n=30):
        self.V1=V1
        self.V2=V2
        self.V_boundary=V_boundary
        self.n=n
        self.V=[[0]*(self.n+1) for i in range(self.n+1)] 
        for j in range(0,self.n+1):
            self.V[0][j]=0
            self.V[self.n][j]=0
        for i in range(0,self.n+1):
            self.V[i][0]=0
            self.V[i][self.n]=0
        for i in range(int(self.n/3),int(self.n/3*2+1)):
            for j in range(int(self.n/3),int(self.n/3*2+1)):
                self.V[i][j]=self.V1
    def update_V(self):     
        self.counter=0
        self.delta_V=0
        self.V_2=self.V
        while True:
            for i in range(1,self.n):
                for j in range(1,self.n):
                    temp_V=(self.V[i-1][j]+self.V[i+1][j]+self.V[i][j-1]+self.V[i][j+1])/4
                    self.V[i][j]=temp_V
            for i in range(int(self.n/3),int(self.n/3*2+1)):
                for j in range(int(self.n/3),int(self.n/3*2+1)):
                    self.V[i][j]=self.V1
            for i in range(1,self.n):
                for j in range(1,self.n):
                    self.delta_V=self.delta_V+abs(self.V_2[i][j]-self.V[i][j])
            self.counter=self.counter+1
            self.V_2=self.V
            if self.delta_V < (961*(1.0E-5))and self.counter >1000:
                break
        return (self.counter,self.delta_V,self.V)
    def Electric_field(self,x1,x2,y1,y2):   
        self.dx=abs(x1-x2)/self.n
        self.dy=abs(y1-y2)/self.n
        self.Ex=[[0]*(self.n+1) for i in range(self.n+1)] 
        self.Ey=[[0]*(self.n+1) for i in range(self.n+1)] 
        for i in range(1,self.n):
            for j in range(1,self.n):
                self.Ex[i][j]=-(self.V[i][j+1]-self.V[i][j-1])/(2*self.dx)
                self.Ey[i][j]=(self.V[i+1][j]-self.V[i-1][j])/(2*self.dy)
        return (self.Ex,self.Ey)
    def Potential_picture(self,ax,x1,x2,y1,y2):  
        self.x=linspace(x1,x2,self.n+1)
        self.y=linspace(y2,y1,self.n+1)
        self.x,self.y=meshgrid(self.x,self.y)
        self.surf=ax.plot_surface(self.x,self.y,self.V, rstride=1, cstride=1, cmap=cm.coolwarm)
        ax.set_xlim(x1,x2)
        ax.set_ylim(y1,y2)
        ax.zaxis.set_major_locator(LinearLocator(10))
        ax.zaxis.set_major_formatter(FormatStrFormatter('%.01f'))
        ax.set_xlabel('x ',fontsize=14)
        ax.set_ylabel('y ',fontsize=14)
        ax.set_zlabel('V',fontsize=14)
        ax.set_title('Potential',fontsize=18)
    def Electric_picture(self,ax1,ax2,x1,x2,y1,y2):  
        self.x=linspace(x1,x2,self.n+1)
        self.y=linspace(y2,y1,self.n+1)
        self.x,self.y=meshgrid(self.x,self.y)
        cs=ax1.contour(self.x,self.y,self.V)
        plt.clabel(cs, inline=1, fontsize=10)
        ax1.set_title('Electric potential near two metal plates',fontsize=18)
        ax1.set_xlabel('x ',fontsize=14)
        ax1.set_ylabel('y ',fontsize=14)
        for i in range(1,self.n,1):
            for j in range(1,self.n,1):
                ax2.arrow(self.x[i][j],self.y[i][j],self.Ex[i][j]/40,self.Ey[i][j]/40)             
        ax2.set_xlim(-1,1)
        ax2.set_ylim(-1,1)
        ax2.set_title('Electric field near two metal plates',fontsize=18)
        ax2.set_xlabel('x ',fontsize=14)
        ax2.set_ylabel('y ',fontsize=14)
fig=plt.figure(figsize=(10,5))
ax1=plt.axes([0.1,0.1,0.35,0.8])
ax2=plt.axes([0.55,0.1,0.35,0.8])
cmp=Electric(1,1,0,20)
cmp.update_V()
cmp.Electric_field(-1,1,-1,1)
cmp.Electric_picture(ax1,ax2,-1,1,-1,1)
plt.show()     
fig=plt.figure(figsize=(14,7))
ax1= plt.subplot(1,2,1,projection='3d')
cmp=Electric()
cmp.update_V()
cmp.Potential_picture(ax1,-1,1,-1,1)
plt.show(fig)
```
###
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/3798441-79b33d046fadd8c1.png)<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/3798441-af0d45de8c23a78d.png)<br>  
中空的金属四方棱柱型的空间，棱柱上电势保持为0伏特，棱柱内部有两个对称摆放的电容盘，它们的电势分别保持为1v和-1v<be>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/3798441-b7ca2f9c1b08daad.png)<br>  
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/3798441-c456adbf4b380494.png)<br>      

