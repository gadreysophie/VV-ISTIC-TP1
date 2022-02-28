# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

### 1. BUG

#### Describe the bug. 
Example of the bug of the 2038 year. It is similar to the bug of the 2000 year. It is linked to the POSIX system

#### If possible, say whether the bug is local or global and describe the failure that manifested its presence. 
This bug is global for all system under POSIX

#### Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. 

On the 19th of January in 2038, 3 h 14 min 8 s, all systems will display the date of the 13th december in 1901, 20 h 45 min 52 s

#### Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

There is no solution for 32bits systems. The prefered way is to pass on a 64bits system.

### 2. Apache

#### Among those issues find one that corresponds to a bug that has been solved. 

I choose this bug:
![](https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-734?filter=doneissues)

Encountered an IllegalStateException while traversing with Flat3Map.entrySet()

#### Classify the bug as local or global. 
It is a local bug on the entrySet() function.

#### Explain the bug and the solution. 

There is a problem with the EntryIterator.remove() method in the Flat3Map java class

#### Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

I don't see tests to ensure the bug won't reappear. It was only a fix that have been done.

### 3. Netflix

#### What are the concrete experiments they perform? 

- Chaos Monkey: the principle is to select randomly a virtual machine instance and terminate it. Is is to prevent failures of VM instances. It is running only during working hours so that there is a good response delay -> run during weekdays
- Chaos Kong: the principle is to simulate the failure of a entire server on a region in the world -> run once a month

One of the principle of chaos engineering is to vary real word events.

The experiments that are designed focus on the availability of Netflix.

#### What are the requirements for these experiments? 

All tests, with the chaos engineering process, target the control plane of Netflix.
In the study, authors define the Chaos Engineering approach to designing experiments. There are 4 points to consider:
- build a hypothesis around steady state behavior
- vary real word events
- run experiments in production
- automate this experiments to run continuously.



#### What are the variables they observe and what are the main results they obtained? 

Variables that are observed are:
- SPS: stream/starts per second, it is a great metric to characterize the steady state behavior. If there is an outage on a region, all requests are redirect on an healthy region;
- the use of historical data

The authors give some inputs they use in their experiments : 
- terminate virtual machine instances (chaos monkey)
- inject latency into requests between services
- fail requests between services
- fail an internal service
- make an entire Amazon region unavailable (choas Kong)
Most of the time, the error is simulated, for example in the case of the chaos kong.

Netflix use fallbacks to ensure graceful degradation.

#### Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

Netflix runs thanks to a distributed system because it is not possible to use one single server.
Azure is using this system already as long as Netflix.

These experiments could be carried out on all streaming platforms like Disney+ or Prime Video that demand continuous availability. 

### 4. WebAssembly

#### Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. 

WebAssembly is safe, fast, portable and compact. It is the first solution for low level code on the web that delivers those capacities and services. It works on all browsers.
WebAssembly is a binary code format. 

#### In your opinion, does this mean that WebAssembly implementations should not be tested?

WebAssembly implementations should be tested.

### 5. WebAssembly using Isabelle

