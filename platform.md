# Platform Decision Flow

The below flowchart is to show the platform selection based on the selection of the user. The user must answer the following questions:

1. Platform Type (Cloud or OnPrem) *([more on this further down](#Cloud) )*
2. Engine Size (S,M,L)
3. CPU Type (CPU, GPU)
4. Data Location (BDPaaS, RDBMS, Other)

These 4 questions will determine the platform the user docker will be send to. but there is still some uncertainty's about the limitations of the other platform. ( [link](#Other Platform Uncertainty's) )

````mermaid
graph TB

Start --> A{Platform Type?}
	   A -->|Cloud| B(Follow Azure Access Process)
	   A -->|On Prem| C{Data Location}
		  C -->|BDPaaS| D{Cpu Type?}
		  C -->|RDBMS| D{Cpu Type?} 
		  C -->|Other| D{Cpu Type?}
	 	 	 D -->|GPU| E(BDPaaS GPU)
	 		  D -->|CPU| F{Engine Size}
	  			 F -->|Small| G(OEA-AS)
	 			 F -->|Medium| H(BDPaaS CPU)
	  			 F -->|Large| I(OFSI)
````

1. Platform Type

   If **on premise** continue with flow to decide on which platform

   if **Cloud** this option will own show if uses belongs to the AD group or should it be controlled by database?

2. Data Location

   This may not be required, it will depend if the OFSI environment can connect to BDPaaS Cluster. For now it will continue to CPU Type and not be used in selection process.

3. CPU Type

   Either CPU or GPU. if GPU then its BDPaaS GPU else its everything else.

4. Engine Size

   We'll need some explanations here what this means to user. If user is submitting jobs to the BDPaaS cluster then there is no requirement to use a large engine size. For now until we understand the limitations of the other platforms. 

   - Small = OEA-AS Infrastructure
   - Medium = BDPaaS CPU
   - Large = OFSI

For our first release, the docker will not have restrictions of resources they can use. 

##  Cloud

Access to the Azure will be done by request. This is because before the user can use any services in azure there team / department will need to setup a subscription and associated GL codes. This is an external process.   

It will also simply our platform decision making as the user case for using azure will be different to anything on premises as they will need to copy there data into the cloud. User will get to decision platform and if they select "Cloud" then we have follow different set of rules and processes for this. 

## Other Platform Uncertainty's

### BDPaaS GPU & GPU

We need to understand the BDPaaS Kubernetes Cluster a little more and what BDPaaS Plans are for this cluster.

1. What are the available resources in both CPU and GPU environments?

2. How much can one docker container use in terms of resources?

3. How our resources shared in a multi user environment?

4. Do resources have relationship with our own cluster resources we pay for?

5. Is there a charge back?

   

### OFSI

Like the BDPaaS environment we need to understand more about how OFSI works.

1. What are the available resources in OFSI

2. How much resources can a docker consume?

3. How does the charge back mode work?

4. Can we submit deployments using API?

5. Is it connected with BDPaaS?

6. Provision Volume Types, (GlusterFS, NAS, Other)

   

### Container Scaling - DSW 3.0

With availability of other platforms like OFSI and Azure this opens up opportunities for us in creating an alternative to YARN / BDPaaS. Scaling multiple docker images and parallelizing its containers to run analytic jobs could be come a power alternative. The DSW will need have ability to setup and run multiple containers. This is a feature for DSW 3.0, we'll need to understand more about our new platforms and thier limitations. 





