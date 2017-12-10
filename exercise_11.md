### 5.1 Question: Solve the potential in the prism geometry in Figure 5.4
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/12.10.png)<br>
###answer
the space electric potential  satisfies the Laplasse equation<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/12.1.png)<br>
at the point (i,j,k) the derivative with respect to x ca be written as<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/12.2.png)<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/12.3.png)<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/12.3.png)<br>


Thus, it is natural to write the second partial derivative as<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/12.5.png)<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/12.6.png)<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/12.7.png)<br>
we come the conclusion
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/12.8.png)<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/12.9.png)<br>

###code 1
```
import matplotlib.pyplot as plt
import numpy as np
from mpl_toolkits.mplot3d import Axes3D
from matplotlib import cm
from matplotlib.ticker import LinearLocator, FormatStrFormatter
class jacobi:
    def __init__(self):
        pass
    def initialization(self,initial_file):
        itxt= open(initial_file)
        self.lattice_in = []
        self.lattice_out = []
        for lines in itxt.readlines():
            lines=lines.replace("\n","").split(",")
            #print (lines[0].split(" ")
            self.lattice_in.append(lines[0].split(" "))
            self.lattice_out.append(lines[0].split(" "))
        itxt.close()
        #print (self.lattice_in)
        #print (self.initial_lattice)
        for i in range(len(self.lattice_in)):
            for j in range(len(self.lattice_in[i])):
                self.lattice_in[i][j] = float(self.lattice_in[i][j])
                self.lattice_out[i][j] = float(self.lattice_out[i][j])
        return 0
    def evolution(self):
        delta = 10
        self.N = 0
        delta_record = []
        while (delta > 0.00001*len(self.lattice_in)*len(self.lattice_in[0])):
            delta = 0
            for i in range(1,len(self.lattice_in) - 1):
                for j in range(1,len(self.lattice_in[i]) - 1):
                    if (self.lattice_in[i][j] != 1 and self.lattice_in[i][j] != -1):
                        self.lattice_out[i][j] = 0.25*(self.lattice_in[i - 1][j] + self.lattice_in[i + 1][j] + self.lattice_in[i][j - 1] + self.lattice_in[i][j + 1])
                        delta+= abs(self.lattice_out[i][j] - self.lattice_in[i][j])
            delta_record.append(delta)
            for i in range(len(self.lattice_in)):
                for j in range(len(self.lattice_in[i])):
                    self.lattice_in[i][j] = self.lattice_out[i][j]
            self.N+= 1
        plt.plot(delta_record,label = 'jacobi method')
        print (self.N)
        print (len(self.lattice_in))
        return 0
    def electric_field(self):
        self.ex = np.zeros([len(self.lattice_in),len(self.lattice_in)])
        self.ey = np.zeros([len(self.lattice_in),len(self.lattice_in)])
        deltax = 0.3333
        deltay = 0.3333
        for i in range(1,len(self.lattice_in) - 1):
            for j in range(1,len(self.lattice_in[0]) - 1):
                self.ey[i][j] = (-(self.lattice_in[i + 1][j] - self.lattice_in[i - 1][j])/(2*deltax))
                self.ex[i][j] = (-(self.lattice_in[i][j + 1] - self.lattice_in[i][j - 1])/(2*deltay))
        return 0
    def plot_field(self):
        self.electric_field()
        X, Y = np.meshgrid(np.arange(-1.00, 1.01, 2./(len(self.lattice_in) - 1)), np.arange(-1.00, 1.01, 2./(len(self.lattice_in) - 1)))
        #plt.figure(figsize = (8,8))
        plt.quiver(X, Y, self.ex, self.ey, pivot = 'mid', units='width')
        plt.title('Electric field between two metal plates')
        #plt.show()
        return 0
    def plot_contour(self): 
        X, Y = np.meshgrid(np.arange(-1.00, 1.01, 2./(len(self.lattice_in) - 1)), np.arange(-1.00, 1.01, 2./(len(self.lattice_in) - 1)))
        #plt.figure(figsize = (8,8))
        CS = plt.contour(X, Y, self.lattice_in, 13)
        plt.clabel(CS, inline=1, fontsize=10)
        plt.title('Equipotential lines')
        #plt.show()
        return 0
    def plot_surface(self):
        fig = plt.figure()
        ax = fig.gca(projection='3d')
        X, Y = np.meshgrid(np.arange(-1.00, 1.01, 2./(len(self.lattice_in) - 1)), np.arange(-1.00, 1.01, 2./(len(self.lattice_in) - 1)))
        surf = ax.plot_surface(X, Y, self.lattice_in, rstride=1, cstride=1,cmap = cm.coolwarm_r,
                       linewidth=0, antialiased=False)
        ax.set_zlim(-1.01, 1.01)
        ax.zaxis.set_major_locator(LinearLocator(10))
        ax.zaxis.set_major_formatter(FormatStrFormatter('%.02f'))
        fig.colorbar(surf, shrink=0.5, aspect=5)
#A = jacobi()
A = SOR()
A.initialization('initial_capacitor_26.txt')
A.evolution()
#plt.figure(figsize = (8,8))
plt.figure(figsize = (16,8))
plt.subplot(121)
A.plot_contour()
plt.subplot(122)
A.plot_field()
#A.plot_surface()
plt.savefig('chapter5_5.7_26.png', dpi = 256)
plt.show()
```
###code 2
```
class SOR(jacobi):
    def evolution(self):
        delta = 10
        self.N = 0
        delta_record = []
        alpha = 2/(1+np.pi/len(self.lattice_in))
        while (delta > 0.00001*len(self.lattice_in)*len(self.lattice_in[0])):
            delta = 0
            for i in range(1,len(self.lattice_in) - 1):
                for j in range(1,len(self.lattice_in[i]) - 1):
                    if (self.lattice_in[i][j] != 1 and self.lattice_in[i][j] != -1):
                        self.lattice_out[i][j] = 0.25*(self.lattice_in[i - 1][j] + self.lattice_in[i + 1][j] + self.lattice_in[i][j - 1] + self.lattice_in[i][j + 1])
                        delta+= abs(self.lattice_out[i][j] - self.lattice_in[i][j])
                        self.lattice_in[i][j] = alpha * (self.lattice_out[i][j] - self.lattice_in[i][j]) + self.lattice_in[i][j]
            #print (self.lattice_out)
            delta_record.append(delta)
            self.N+= 1
        plt.plot(delta_record, label ='SOR algorithm')
        print (self.N)
        print (len(self.lattice_in))
        #print (self.lattice_in)
        return 0    
class SOR_2(jacobi):
    def evolution(self,alpha):
        delta = 10
        self.N = 0
        delta_record = []
        #alpha = 2/(1+np.pi/len(self.lattice_in))
        while (delta > 0.00001*len(self.lattice_in)*len(self.lattice_in[0])):
            delta = 0
            for i in range(1,len(self.lattice_in) - 1):
                for j in range(1,len(self.lattice_in[i]) - 1):
                    if (self.lattice_in[i][j] != 1 and self.lattice_in[i][j] != -1):
                        self.lattice_out[i][j] = 0.25*(self.lattice_in[i - 1][j] + self.lattice_in[i + 1][j] + self.lattice_in[i][j - 1] + self.lattice_in[i][j + 1])
                        delta+= abs(self.lattice_out[i][j] - self.lattice_in[i][j])
                        self.lattice_in[i][j] = alpha * (self.lattice_out[i][j] - self.lattice_in[i][j]) + self.lattice_in[i][j]
            #print (self.lattice_out)
            delta_record.append(delta)
            self.N+= 1
        #plt.plot(delta_record, label ='SOR algorithm')
        print (self.N)
        #print (len(self.lattice_in))
        #print (self.lattice_in)
        return 0  
```
### image
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/12.12.png)<br>
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/12.13.png)<br>

We use initial guess V₁:
![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/12.11.png)<br>
