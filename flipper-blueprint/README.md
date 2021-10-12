<img src="https://via.placeholder.com/1000x4/45D7CA/45D7CA" alt="drawing" height="4px"/>

### Flipper Blueprint

<sub>[previous](../create-mesh/README.md) • [home](../README.md#user-content-ue4-flipper) • [next](../)</sub>

<img src="https://via.placeholder.com/1000x4/45D7CA/45D7CA" alt="drawing" height="4px"/>

Chapter introduction here.

<br>

---


##### `Step 1.`\|`ITA`|:small_blue_diamond:

![alt_text](images/.jpg)

<p>Drag a copy of the mesh from your content browser into your scene and place it wherever you’d like (ideally near our player). Now we can get to Blueprinting.

In the details panel of your Flipper mesh, click the “Blueprint/Add Script” button.</p>

<img src="images/2021-10-05 11_42_47-WalkthroughThings - Unreal Editor.png">

##### `Step 2.`\|`FHIU`|:small_blue_diamond: :small_blue_diamond: 

![alt_text](images/.jpg)

<p>Name your script <b>BP_Flipper</b> and hit <b>Select</b>
Let’s first just check for player input. Drag out a pin from Event Tick and create a <b>Cast to ThirdPerson Character</b> node. This will allow us to access the inputs from our player character. Now make a Get Player Character node, and connect its output to the Object of the Cast node.</p>

<img src="images/2021-10-07%2010_11_54-BP_Flipper.png">

##### `Step 3.`\|`ITA`|:small_blue_diamond: :small_blue_diamond: :small_blue_diamond:

<p>Now we can check the input of the ThirdPersonCharacter. Before we check for the jump, we need a Boolean that tells us if the Jump has been pressed. Let’s go back to the Content Browser and open up the ThirdPersonCharacter Blueprint (this should be in ThirdPersonBP>Blueprints).

Create a new variable called “bJumpPressed,” and make sure its type is set to Boolean.</p>

<img src="images/2021-10-07%2010_14_55-WalkthroughThings%20-%20Unreal%20Editor.png">

<p>Now go to the Jump section of your Character. Pull off a pin from the Jump node and type “Set Jump Pressed” to create a Set node for your new variable. Make sure the default value of this node is set to True. <b>Make sure you’re not confusing this with the Pressed Jump variable! This one does NOT have the functionality we’re looking for!</b></p>

<img src="images/2021-10-07%2010_17_53-WalkthroughThings%20-%20Unreal%20Editor.png">

<p>While we’re here we can reset the jump variable when it’s not being pressed. Move below the graph and make an Event Tick node. Next, make a Branch node. In the condition, connect an “==” (equals to) node and have it set to true. Drag in a copy of the Jump Pressed variable, and connect it to the first input of the equals node. Connect the Event Tick to the Branch. From the Branch’s True condition, create a “Set Jump Pressed” Node, and have this set to false. This is what you should end up with:</p>

<img src="images/2021-10-09%2014_22_51-WalkthroughThings%20-%20Unreal%20Editor.png">


![alt_text](images/.jpg)

<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

##### `Step 4.`\|`ITA`|:small_blue_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond:

![alt_text](images/.jpg)

<p>Let’s go back to the Flipper Blueprint. Pull out a pin from the <b>As Third Person Character</b> and create a “Get Jump Pressed” node.  For now we’ll just print out the output of the button press. Create a Print String Node and connect the <b>Get Jump Pressed</b> to its <b>In String</b>. </p>

<img src="images/2021-10-07%2010_25_49-BP_FlipObject.png" />

<p>Compile this, and let’s return to our scene. Hit play and see what happens when we jump. Notice the little blue “true” in the corner of our screen. This shows that our input is being detected.</p>


<video src="images\2021-10-07 10-29-02.mp4" width=600></video>

##### `Step 5.`\|`ITA`| :small_orange_diamond:

![alt_text](images/.jpg)

<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

<p>Now we can finish building out the main function of our Flipper. Return to the Blueprint and create a new variable called <b>bIsFlipping</b>. Make sure it is private and defaulted to false.</p>

<img src="images/2021-10-07%2010_32_18-BP_Flipper.png">

<p>We can disconnect our Print String (though it may be good to keep it around for debug purposes). From Jump Pressed, drag out a pin to create a new Branch node with the Jump Pressed as its condition. This is where we’ll check when the Flipper should start rotating. Also connect the output of the Cast to ThirdPerson to the Branch’s input.</p>

<img src="images/2021-10-07%2010_36_25-BP_FlipObject.png">

<p>From the True condition of the Jump Branch, connect it to a “Set isFlipping” node. Set this node to True. From the False condition, connect to another Branch node. Create a “Get IsFlipping” node, and connect it to the condition of this new Branch.</p>

<img src="images/2021-10-07%2010_44_05-BP_FlipObject.png">

##### `Step 6.`\|`ITA`| :small_orange_diamond: :small_blue_diamond:

![alt_text](images/.jpg)

<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

<p>Now we will need a function to actually do the rotation. Right click on the graph and type <b>Add Custom Event</b>. Name this node “RotateTo.” Drag this out of the way for now (I put mine underneath the current graph). This will be the entry point for our actual rotation function, which we will be writing very soon. </p>

<p>Now that this new event is created, we can add its function to our graph. Right click and type in “RotateTo.” Connect the True condition of the second Branch and the output of the “Set IsFlipping” to the function’s input.</p>

<img src="images/2021-10-07%2010_54_35-BP_FlipObject.png">

##### `Step 7.`\|`ITA`| :small_orange_diamond: :small_blue_diamond: :small_blue_diamond:

![alt_text](images/.jpg)

<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

<p>Now we can write the actual Rotate To function. Move over to wherever you placed the original event node (it should be a red node). Let’s start by adding a few more variables: <b>speed</b> (a float), <b>cumulative rotation</b> (a float), and <b>direction</b> (an integer). Set the default value of Speed to 75 and default value of Direction to 1.</p>

<img src="images/2021-10-07%2011_00_56-BP_Flipper.png">

##### `Step 8.`\|`ITA`| :small_orange_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond:

![alt_text](images/.jpg)

<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

<p>Let’s start by doing some basic math to change the flipper’s rotation. Drag a Get Speed and Get Direction node into the graph. Also create a Get World Delta Seconds node. Now create a <b>Float * Float</b> node and click its “add pin” button. Connect the three nodes to each of the multiply’s inputs.
Drag out a pin from the multiply node and create an addition node (you can simply type “+”). Drag in your Cumulative Rotation variable (select the Get option) and connect this to your addition node. This will add the speed (which is change in rotation over time) to the current rotation. This will be checked to see if our flipper has reached its target rotation. </p>

<img src="images/2021-10-07%2011_20_11-BP_FlipObject.png">

<p>Now drag out another Cumulative Rotation, this time set to Set. Connect the output of the add to this. From the Set Cumulative Rotation, drag and create an “abs” (Absolute Value) node. This will remove the negative sign on our rotational value, in case the Flipper is moving from a negative direction, which makes our math checks simpler. From the abs node, create a “>” (greater than) node and set it to 180. From this create a new Branch. Also connect the “Set Cumulative Rotation” to this branch.
</p>

<img src="images/2021-10-09%2014_20_45-BP_FlipObject.png">

<p>Let’s also connect the RotateTo to the Set node, so that our function actually runs when activated.</p>

<img src="images/2021-10-07%2011_40_39-BP_FlipObject.png">

##### `Step 9.`\|`ITA`| :small_orange_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond: :small_blue_diamond:

![alt_text](images/.jpg)

<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

<p>Let’s start with the True condition of the Branch. Drag in another <b>Set Cumulative Rotation</b> and connect that to <b>True</b>. Leave its default value as 0.0, as we want this to reset the total rotation to 0. Drag in a “Set IsFlipping” and connect its input to the <b>Set Cumulative Rotation</b> output. Leave its value at <b>False</b>. Now we need one last Set, this time for the Direction. Drag this in and connect to the <b>Set isFlipping</b>. For the input of the <b>Set Direction</b>, we will be inverting the current direction. Drag in the Direction variable (this time using Get). From its output, create a multiply node and set its value to <b>-1</b>. Drag the output of this multiply to the input of the <b>Set Direction</b>. Your chain so far should like look this:</p>

<img src="images/2021-10-07%2011_38_28-BP_FlipObject.png">

##### `Step 10.`\|`ITA`| :large_blue_diamond:

![alt_text](images/.jpg)

<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

<p>Now let’s finish up this RotateTo function. Create an <b>Add Actor Local Rotation</b> node. Right click on the Delta Rotation section and click <b>split struct pin</b>.</p>

<img src="images/2021-10-07%2011_43_40-.png">

<p>Connect out first multiply node (the one connecting direction, world delta seconds, and speed) to the Delta Y Rotation of this node. Also connect the False condition of the Branch to this node.</p>

<img src="images/2021-10-07%2011_45_24-BP_FlipObject.png">

##### `Step 11.`\|`ITA`| :large_blue_diamond: :small_blue_diamond: 

![alt_text](images/.jpg)

<img src="https://via.placeholder.com/500x2/45D7CA/45D7CA" alt="drawing" height="2px" alt = ""/>

<p>Back in the scene view, be sure that the mesh is set to Movable, otherwise none of the motions we programmed with actually take effect. Press Play and see the results of your new Flipper. Adjust Speed and sizing to your liking.</p>

<video src="images/2021-10-12 11-32-37.mp4" width="600"></video>

___


<img src="https://via.placeholder.com/1000x4/dba81a/dba81a" alt="drawing" height="4px" alt = ""/>

<img src="https://via.placeholder.com/1000x100/45D7CA/000000/?text=Next Up - ADD NEXT TITLE">

<img src="https://via.placeholder.com/1000x4/dba81a/dba81a" alt="drawing" height="4px" alt = ""/>

| [previous](../)| [home](../README.md#user-content-ue4-animations) | [next](../)|
|---|---|---|
