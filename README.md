# Introduction

This is a taster session of the spatial stochastic modelling software MCell, which is used for modelling biochemical events. In MCell, every molecule is explicitly represented. It can move around in space and change or interact with other molecules according to rules specified by the user. We use an MCell plugin to Blender both to design the model and to visualise simulation results.

We will use MCell to model a simplified version of what happens in neurons when we form memories. When we learn, connections between neurons (synapses) get strengthened. It is said that "neurons that fire together, wire together". On a molecular level, this strengthening is mediated by molecular changes in the neuron at the receiving end of the synapse (the post-synaptic neuron): When a postsynaptic neuron gets activated, a set of transmembrane receptors called AMPA receptors (AMPARs) become activated and change the electrical properties of the cell. Then, if there is another activation event, another transmembrane receptor, the NMDA receptor (NMDAR) is activated and allows Calcium to enter the cell. Calcium activates a protein called calmodulin, which then activates another protein called CaMKII (this is short for Calcium-/calmodulin-activated kinase II). A kinase is a protein that catalyses the phosphorylation of other proteins. One of the proteins phosphorylated by CaMKII is the AMPA receptor itself. Phopshorylation causes AMPA receptors to be more active and also causes more AMPA receptors to be inserted into the cell membrane at this particular site. So, the next time there is an activation event, there are more receptors to receive it. This is what we mean when we say the synapse is "strengthened". (This is a simplified explanation, and there are many other molecular interactions in place that serve to fine-tune this system and to ensure that strengthened synapses can remain that way for a long period of time.)

The system we are modelling today is a very simple version of what happens in a synapse upon activation:

- Calcium enters the cell through activated NMDA receptors
- Calcium activates CaMKII 
- Activated CaMKII enzymatically activates AMPA receptors


# Getting started with MCell and CellBlender

This tutorial will assume you have MCell and CellBlender already installed. For instructions on how to do this, see [here](http://mcell.org/tutorials/software.html)

# Calcium binding to CaMKII

- Download the model file: [Calcium_signalling.blend](Calcium_signalling.blend) 

- Open Blender. In Blender, open the file `Calcium_signalling.blend`

- In the CellBlender tab, examine the `Molecules` and `Reactions` tabs. Those contain the molecules in the model and the reactions they undergo. At the moment, only the first reaction has a reaction rate that is not zero.

_What happens in this reaction? How would you expect the conentrations of CaMKII, Calcium, and active\_CaMKII to change over time?_

- Click on the `Run Simulation` tab. Click `Export & Run` to run the simulation. In the `MCell Processes` window below, a line will appear with a process ID and some more information. Left of it is a symbol. If it's a little running person, the simulation is still running. If it's a green checkmark, the simulation is done.

- If you receive a warning message about reaction proabilities, you can ignore it for now. If you want to know more about what it means, you can discuss it with a tutor.

- Click on `Reload Visualization Data`. Examine the image in the middle of the screen. We have a spherical reaction volume. Calcium and CaMKII are placed within that sphere, while the receptors (NMDA and AMPA receptors) are on the surface.

_Can you tell which is which? Go to the `Molecules` tab in order to see what molecule is associated with what colour._

- On the bottom of the Blender window is a timeline with a green bar in it. This allows you to look at how the simulation proceeds over time. Drag the green bar to 0. Click on the little white arrow to start the animation.

_What changes over the course of the simulation? Is this consistent with your expectations?_

# Where does the Calcium come from?

- In the simulation you just ran, we started with a number of CaMKII molecules and of Calcium molecules in our simulation compartment. How did we do that? Go to the `Molecule Placement` tab and examine `Release Site 2`. This specifies that at the beginning of the simulation, we release 100 molecules of Calcium into the simulation volume.

- But in an actual neuron, Calcium would come in through activated NMDA receptors. We have actually modelled this using a very simple biochemical reaction:

```
NMDAR' -> Calcium, + NMDAR'
```

Forget about the apostrophes and commas for now (if you want to learn more, there is an advanced tutorial [here](http://mcell.org/tutorials/surface_classes.html)). What happens in this reaction? Why are we using this as a way of representing Calcium moving into the cell?

- Set the reaction rate for this reaction to something other than 0. You could use 1e4 for instance, (which is a way of writing 10000, i.e. a one with four zeros. This would mean 10000 of those reactions happen in a second).

- Now that we have Calcium coming in through NMDA receptors, we don't actually need it around in the cell from the beginning. Change the number of Calcium molecules that are present at the beginning of the simulation to 0 in the `Molecule Placement` tab.

- Run the simulation again. When it is done running, click on `Reload Visualization Data` to reload your data. Set the timing pointer to 0 again and run the animation. You can stop and restart it if you want, you can go to specific times by sliding the green bar, you can advance or go back frame-by-frame with the arrows on your keyboard, so you can really examine in detail what goes on in the time course. 

_How much Calcium is there at the start of the simulation? How much is there at the end? How does this affect CaMKII activation?_ 

# AMPA receptor activation

- Finally, we need to activate AMPA receptors! AMPA receptors are activated enzymatically by active CaMKII. An enzyme is a protein that facilitates a reaction, but does not itself get used up in it. 

_How would you write this as a chemical reaction (or set of reactions)?_

- If you go to the `Reactions` tab, the last reaction represents AMPAR activation.

_Is this what you would have done? Does this make sense to you?_

- Again, we have set the reaction rate to 0. Set it to something greater than zero. We have found that something really quick works well. Try 1e12. (_What is that number called?_)

- Run the simulation again.

_What would you expect to be different from the last simulation?_

- Reload the visualization data and examine the outcome.

_Does this confirm what you expected?_

# Checking out diffusion

- You will have noticed that the molecules dance around in space. They are in fact diffusing randomly. A reaction can only happen when two molecules bump into each other. Being able to follow individual molecules as they move through space is one of the great advantages of the MCell simulation software.

- So, maybe you are curious and wonder: How much does a single molecule actually move around in space during the course of a simulation? This is not very easy to see with the naked eye.

_Do you have a suggestion as to how to make it visible?_

- We have tried to visualise diffusion by creating a molecule that is called `CaMKII_tracked`. It is similar to CaMKII (it diffuses at the same speed), but there is only one of it in the entire simulation. You cannot see it, because we have hidden it. (You can hide molecules from the visualisation by clicking on the "eye" symbol next to them in the panel on the right.) There is also a molecule called `CaMKII_trace`. If you go to the `Molecules` tab, you will notice that the other molecules inside the round reaction volume have a non-zero diffusion constant (i.e. they can move around in space). But it's different for `CaMKII_trace`.  Go to the `Reactions` tab and look at the second reaction.

_What happens here?_

- Actually, nothing happens, because that reaction rate is zero. Set it to something positive (1e8 works quite well) and run the simulation again.

- When done running, re-load the visualisation data. This time, in order to better see what's happening, you might want to hide all other molecules (close the little eye symbol next to them in the right pane), and only show `CaMKII_tracked` and `CaMKII_trace`.

_What do you see?_

# The Plot thickens

- Back to the main point of our simulation: Calcium entry into the cell and activation of CaMKII, and then AMPA receptors. You can hide the two molecules we just looked at again and show all the others. Now, looking at those animations is interesting, but it is really hard to quantify what happens. What we would like is a plot of how total numbers of a particular type of molecule (e.g. `active_CaMKII`) change over time.
In order to do this, click on `Plot Output Settings` and scroll down. There is a tab titled `Plot`, and to the left of it you can select the Plotter you want to use. (Some of them might work for you and some not, depending on other software installed on your computer. Try a few until you find one that's good).

_What does the plot show? What's on the x axis? What's on the y axis? What colour is what? How does this relate to what you have seen? Looking at the plot, do you notice something that might not be quite right?_

- We think there is a problem with Calcium. As more and more NMDA receptors get activated, Calcium levels just go up and up. In an actual cell, there would be limits to how much the Calcium concentration can rise.

_Why are there limits? How? How could you model something like that in the MCell model? You might have to add a reaction, or even another molecule._

