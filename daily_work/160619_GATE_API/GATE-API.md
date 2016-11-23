## GATE—API

之前国外的那边ACM论文用的就是GATE

2.6.1 Using GATE with Maven/Ivy [#]

This section is based on contributions by Marin Nozhchev (Ontotext) and Benson Margulies (Basis Technology Corp).
Stable releases of GATE (since 5.2.1) are available in the standard central Maven repository, with group ID “uk.ac.gate” and artifact ID “gate-core”. To use GATE in a Maven-based project you can simply add a dependency:

	<dependency>  
 	 <groupId>uk.ac.gate</groupId>  
  	<artifactId>gate-core</artifactId>  
  	<version>6.0</version>  
	</dependency>
Similarly, with a project that uses Ivy for dependency management:

<dependency org="uk.ac.gate" name="gate-core" rev="6.0"/>
In addition you will require the matching versions of any GATE plugins you wish to use in your application – these are not managed by Maven or Ivy, but can be obtained from the standard GATE release download or downloaded using the GATE Developer plugin manager as appropriate.
Nightly snapshot builds of gate-core are available from our own Maven repository at http://repo.gate.ac.uk/content/groups/public.


## CREOLE

## ANNIE

In both cases this will include the GATE libraries only (and relevant dependencies) — you must separately obtain the appropriate versions of any GATE plugins your application requires (including ANNIE), typically by downloading the standard GATE release or by fetching them from the relevant place in subversion. For example if you are using GATE 5.2.1 via a Maven dependency then you can obtain the correct ANNIE plugin from https://gate.svn.sourceforge.net/svnroot/gate/gate/tags/release-5.2.1/.

## Quick Start

