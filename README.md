### Course Project - Create and evaluate CloudSim Plus simulations that use different load balancing algorithms
### Submission By - 82_Arppit Madankar, 33_Sheetal Vishwakarma, 51_Utkal Junarkar


### How to run the code:
1. Clone the repository to your local machine using 

	``git clone  ``

2. Go to the cloned repository location on your terminal and test the program using
``sbt clean compile test``
3. Go to the cloned repository location on your terminal and compile the program using
``sbt clean compile run``
4. When prompted to enter a number to select main class. Enter the number of the desired load balancer class you want to run. 
5. The output prints a table of results for execution of 500 cloudlets. 



#### Structure of the project folder:
- /src/main/resources folder contains the files:
    - default.conf that contains the configuration settings for running the simulation  
    - PlanetLab trace file - used to test CPU utilization model using trace files
- /src/main/scala/com/cloudsimplus/app:
    - NetworkAbstract.scala contains an abstract class that defines network cloud simulation methods and must be extended to implement different load balancers
    - RandomLoadBalancer.scala contains the implementation of the Random algorithm (extend NetworkVM)
    - RoundRobinLoadBalancer.scala contains the implementation of the Round Robin algorithm  (extends NetworkVM)
    - HorizontalVmScalingLB.scala contains the implementation of the Horizontal VM Scaling algorithm  
- /src/main/java
    - org.cloudbus.cloudsim.utilzationmodels/UtilizationModelHalf.java creates a CPU utilization model that uses 70% of the CPU resources at a time
    - org.cloudsimplus.builders.tables/CloudletsTableBuilderWithCost.java extend the cloudsimplus CloudletsTableBuilder to print the results with total cost for each cloudlet


#### Approach towards designing the simulation:
The goal of this course project is to simulate cloud environments in CloudSim Plus using different load balancers in a networked cloud architecture. The evaluation of the load balancers is based on the time and cost of processing the cloudlets which are submitted dynamically in the simulation. 

We use three different load balancers in our simulation:
- Random: Random is a static algorithm that uses a random number generator to assign cloudlets to VMs. We use the Random algorithm as the baseline for evaluating the simulation.
- Round Robin: Round robin is another static load balancing algorithm where each cloudlet will be assigned to VM in a round - robin sequence.
- Horizontal VM Scaling: Horizontal VM Scaling is a dynamic load balancing algorithm that allows defining the condition to identify an overloaded VM, which in our case is based on current CPU utilization exceeding 70%, which will result in the creation of additional VMs.

As part of our evaluation, we assume the below Null and Alternate Hypotheses:

**Null Hypothesis:** Horizontal VM Scaling algorithm performs better than the Random and Round Robin algorithms  
**Alternate Hypothesis:** No improvement in performance can be seen with Horizontal VM Scaling algorithm as compared with the Random and Round Robin algorithms  

**Architecture of the Cloud Network**


![CloudSimPlusArchitecture](CloudSimPlusArchitecture.png)
(The file 'CloudSimPlusArchitecture' contains the diagram of the cloud network architecture.)

The above figure shows the high-level system architecture of our cloud network. The cloudlets arriving at the endpoint are distributed using load balancers. The load balancers are being simulated using Random, Round-Robin or Horizontal VM Scaling algorithms. Depending on the algorithm, the cloudlets are then assigned to the VMs. In case of horizontal VM scaling, there is auto-scaling of VMs at runtime depending on the current CPU utilization exceeding 70%. Each host is connected to the Edge Switches through a TOR (Top of Row) switch, which is in turn connected to a Router giving our datacenter access to world wide web.

The simulations are carried out on the Cloud Network Architecture with following characteristics:  

- Number of Datacenters:  1

- Number of VMs:  750

- Number of Hosts:  1000

- Number of Cloudlets:  500

**Evalution of the simulation:**

We did not apply regression analysis as the prediction of cost as well as execution time based on an unseen cloudlet's length and/or VM/Host characteristics would be redundant as Cloudsim Plus serves this purpose.

Consecutively, we evaluated our load balancers by keeping cloudlet execution time and cost of running of our total workload as our primary criterion

The results of the simulation are in the file [results.xlsx](./results.xlsx). We found a larger difference in the T-test value between Random vs Horizontal VM scaling and Round Robin vs Horizontal VM Scaling by running T-tests on the corresponding costs of the load balancers. This result is statistically significant as it proves the fact that Round Robin LB algorithm and Horizontal VM Scaling algorithm performs better than just randomly assigning these cloudlets to VMs in different datacenters.

Moreover, T-test score of 0.21 and 0.27 for running costs between Random vs Horizontal VM Scaling and Round Robin vs Horizontal VM Scaling signify that Horizontal VM scaling outperforms the latter two algorithms by a sufficient margin. However, the difference of 0.016 between T-test scores of Random and Round Robin give a notion that Random LB is as good as Round Robin LB. But, this is not the case as Randon LB runs for almost 3000 seconds for executing 500 cloudlets in the worst case when multiple cloudlets get assigned to the same VM whereas Round Robin finishes the simulation in less than 60 seconds. The trade-off between running cost and execution time should be considered and not just each parameter alone.

Furthermore, T-test score of 0 for execution time between Random vs Round Robin LB prove that the means of these two Load Balancers are same. The T-test score of 0.11 for execution times of Round Robin vs Horizontal VM Scaling LB is valid as Round Robin takes less time to execute for 500 cloudlets than Horizontal VM Scaling. Random vs Horizontal VM Scaling shows a T-test score 0f 0.09 for execution time. This result concludes that Horizontal VM Scaling takes less time to execute 500 cloudlets than Random Load Balancing.

We found that the p-values for Horizontal VM Scaling LB to be around ~ 0.40, when compared to RoundRobin and Random load balancers. This shows statistical significance as a significantly higher p-value (typically >= 0.05) indicates strong evidence for supporting the null hypothesis.


**Results using the three alogrithms:**

Random LB algorithm took higher time to execute when compared to Round Robin and Horizontal VM Scaling LB algorithms. It took > 3000 seconds to complete executing 500 cloudlets, while Round Robin LB took less than 60 seconds and Horizontal VM scaling took around 160 seconds.


**Challenges faced**

- We attempted scaling our basic architecture to assume higher number of Datacenters, VMs, Hosts along with higher number of dynamic cloudlets. While we were able to achieve scaling with the simple Cloudlet tasks designed 
on Clousim Plus, we faced dynamic VM allocation exceptions due to transfer of packets between the tasks designed for Network Cloudlets. As using Network Cloudlets was a pre-requisite for the project, we performed our simulation on a limited scale networked cloud environment.


**Conclusion:**  
Based on our performance evaluation, we see an improvement in performance in terms of cost and processing time when a dynamic algorithm like Horizontal VM Scaling is used vs other simple static algorithms for load balancing like Random LB and Round Robin LB.
