# Purpose
By employing this library, the user will be able to evaluate any transient or steady-state M/M/C/K queueing network that can be defined by an adjacency matrix. The model *does not* have to exist on product form. A weighted directed adjacency matrix is used by the class `create` to automatically construct the infinitesimal generator for the network at hand. The resulting object is then used as input in the class `evaluate` to retrieve the behavior of the network.

# Basic Overview

## Files

- `mc_math.jar`: The library file.

## Classes

- `create`: Automatically constructs the infinitesimal generator (the transition rate matrix) and various other parameters.

- `evaluate`: Uses an object defined with `create` to evaluate the queueing network.

# Getting Started
Firstly, download and add `mc_math.jar` to your Java-project.

Now import the `queueing` classes.

```
import queueing.*;
```

Define the weighted directed adjacency matrix using the structure: (1) Sources nodes, (2) queueing nodes, and (3) sink node. In this example, we have three queueing nodes. A single source node feeds all arriving customers into the first queueing node. The flow then splits into three parts sending 45% of the customers to queue 2, 30% to queue 3, and 25% to the sink node. Queue 2 and 3 send all customers to the sink after their service has been completed.

```
double[][] A = {{0,1,0,0,0},
                {0,0,0.45,0.30,0.25},
                {0,0,0,0,1},
                {0,0,0,0,1},
                {0,0,0,0,0}};
```

Define the remaining characteristics of the system, i.e. the arrival rate (`lambda`), service rates (`mu`), number of servers (`c`), capacity (`cap`), and how much of the capacity is occupied at time=0 (`occupiedCap`). If customers at queue 1 should be rejected when queue 2 and 3 are full, set `rejectWhenFull = true`; otherwise `rejectWhenFull = false`.  

```
double[] lambda = {2}; double[] mu = {1.5,4,2.5}; int[] c = {2,1,2}; int[] cap = {20,20,20};
int[] occupiedCap = {0,0,0}; boolean rejectWhenFull = false;
```

Create the model.

```
create network = new create(A,lambda,mu,c,cap,rejectWhenFull);
```

Prepare the evaluation calculations by plugging the `network` object into `evaluate`.

```
evaluate system = new evaluate(occupiedCap,network);
```

Evaluate the behavior of the system at time=5 with a precision of 1x10^-9.

```
system.uniformization(5,1e-9);
```

Get the marginal state distribution for each of the network queues.

```
double[][] dist = system.getMarginalDistributions();
```

Get the expected number of customers at each queue.

```
double[] expValue = system.expectedValue();
```

Evaluate the steady-state behavior of the system (i.e. at time=Inf) with a precision of 1x10^-6.

```
system.gauss_seidel(1e-6);
```



# License
Copyright 2019 Anders Reenberg Andersen, PhD

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
