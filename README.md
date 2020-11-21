# SCOP Trial 
> SCOP (SOlver for Constraint Programming) trial 


## How to Install to Jupyter Notebook (Labo) and/or Google Colaboratory

1. Clone from github:
> `!git clone https://github.com/mikiokubo/scoptrial.git`
2. Move to scoptrial directry:
> `cd scoptrial`

3. Change mode of execution file

 - for linux (Google Colab.)  
 > `!chmod +x scop-linux`   

 - for Mac 
 > `!chmod +x scop-mac`  

4. Import package and write a code:> `from scoptrial.scop import *`
5. (Option) Install other packages if necessarily: 

> `!pip install plotly`



## How to use

See https://mikiokubo.github.io/scoptrial/  and  https://www.logopt.com/scop2/ 

Here is an example. 

```
from scoptrial.scop import *
```

```
'''
Example 1 (Assignment Problem):
Three jobs (0,1,2) must be assigned to three workers (A,B,C)
so that each job is assigned to exactly one worker.
The cost matrix is represented by the list of lists
Cost=[[15, 20, 30],
      [7, 15, 12],
      [25,10,13]],
where rows of the matrix are workers, and columns are jobs.
Find the minimum cost assignment of workers to jobs.
'''

workers=['A','B','C']
Jobs   =[0,1,2]
Cost={ ('A',0):15, ('A',1):20, ('A',2):30,
       ('B',0): 7, ('B',1):15, ('B',2):12,
       ('C',0):25, ('C',1):10, ('C',2):13 }

m=Model()
x={}
for i in workers:
    x[i]=m.addVariable(name=i,domain=Jobs)

xlist=[]
for i in x:
    xlist.append(x[i])

con1=Alldiff('AD',xlist,weight='inf')

con2=Linear('linear_constraint',weight=1,rhs=0,direction='<=')
for i in workers:
    for j in Jobs:
        con2.addTerms(Cost[i,j],x[i],j)

m.addConstraint(con1)
m.addConstraint(con2)

print(m)

m.Params.TimeLimit=1
sol,violated=m.optimize()

if m.Status==0:
    print('solution')
    for x in sol:
        print (x,sol[x])
    print ('violated constraint(s)')
    for v in violated:
        print (v,violated[v])
```
