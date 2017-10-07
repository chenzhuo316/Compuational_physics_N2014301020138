### Consider again a decay problem with two type of nuclei A ande B, but now suppose that nuclei of type A decay into ones of type B.While nuclei of type B decay into ones of type A.Strictly speaking,this is not a decay process,since it is possible for the type B nuclei to turn back into type A nuclei. A better analogy would be a reasonance in which a system can tunnel or move back and forth between two states A and B which have equal energies. The corresponding rate equations are<br> ![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171007-212200%402x.png) where for simplicity we have assumed that two types of dacay are characterized by the same time constant,![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171007-224505%402x.png).Solve this system of equations for the numbers of nuclei ,Na and Nb,as functions of time.Consider different initial conditions ,such as Na=100,Nb=100,etc,and take ![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171007-224505%402x.png)=1s.Show that your numerical results are consistent with the idea that the system reaches a steady state in which Na and Nb are constant.In such a steady state,the time derivatives dNa/dt and dNb/dt should vanish. <br>
### answer 
Known equation<br> ![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171007-212200%402x.png)<br>
add equation1 to equation 2 <br>![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171007-224313%402x.png)<br>
taken equation1 from equation 2 ![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171007-223208%402x.png)<br>
we can get the conclusion ![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171007-223154%402x.png)<br>
### code
```import pylab as pl
class nuclei_decay:
    """
    A decay problem with two types of nuclei A and B.
    Simulation of the decay.
    Program to problem_1.5 on the book Computational Physics.
    """
   def __init__(self, number_of_A = 100, number_of_B = 0, time_constant = 1, time_of_duration = 5, time_step = 0.05):
        # unit of time is second
        self.N_A = [number_of_A]
        self.N_B = [number_of_B]
        self.t = [0]
        self.tau = time_constant
        self.dt = time_step
        self.time = time_of_duration
        self.nsteps = int(time_of_duration / time_step + 1)
        print("Initial number of nuclei A ->", number_of_A)
        print("Initial number of nuclei B ->", number_of_B)
        print("Time constant ->", time_constant)
        print("time step -> ", time_step)
        print("total time -> ", time_of_duration)

    def calculate(self):
         for i in range(self.nsteps):
            tmp_A = self.N_A[i] + (self.N_B[i] - self.N_A[i]) * self.dt / self.tau
            tmp_B = self.N_B[i] + (self.N_A[i] - self.N_B[i]) * self.dt / self.tau
            self.N_A.append(tmp_A)
            self.N_B.append(tmp_B)
            self.t.append(self.t[i] + self.dt)

    def show_results(self):
        plot1, = pl.plot(self.t, self.N_A, 'r')
        plot2, = pl.plot(self.t, self.N_B, 'g')
        pl.xlabel('time ($s$)')
        pl.ylabel('Number of Nuclei')
        pl.title("Number's change of nuclei A and nuclei B")
        pl.legend([plot1, plot2], ['nuclei A', 'nuclei B'], loc="best")
        pl.show()

    def store_results(self):
        myfile = open('nuclei_decay_data.txt', 'w')
        for i in range(len(self.t)):
            myfile.write(str(self.t[i]))
            myfile.write(str(self.N_A[i]))
        myfile.close()
        print(myfile)

a = nuclei_decay()
a.calculate()
a.show_results()
a.store_results()
```
### image ![](https://github.com/chenzhuo316/Compuational_physics_N2014301020138/blob/master/gif/QQ20171007-224803%402x.png)
