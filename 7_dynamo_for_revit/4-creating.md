# Creating

You can create an array of Revit elements in Dynamo with full parametric control. The Revit nodes in Dynamo offer the ability to import elements from generic geometries to specific category types (like walls and floors). In this section, we'll focus on importing parametrically flexible elements with adaptive components.

![](images/4/creating-dynamonodes.jpg)

### Adaptive Components

An adaptive component is a flexible family category which lends itself well to generative applications. Upon instantiation, you can create a complex geometric element which is driven by the fundamental location of adaptive points.

Below is an example of a three-point adaptive component in the family editor. This generates a truss which is defined by the position of each adaptive point. In the exercise below, we'll use this component to generate a series of trusses across a facade.

![](images/4/ac.jpg)

### Principles of Interoperability

The adaptive component is a good example for best practices of interoperability. We can create an array of adaptive components by defining the fundamental adaptive points. And, when transferring this data to other programs, we have the ability to reduce the geometry to simple data. Importing and exporting with a program like Excel follows a similar logic.

Suppose a facade consultant wants to know the location of the truss elements without needing to parse through fully articulated geometry. In preparation for fabrication, the consultant can reference the location of adaptive points to regenerate geometry in a program like Inventor.

The workflow we'll setup in the exercise below allows us to access all of this data while creating the definition for Revit element creation. By this process, we can merge conceptualization, documentation, and fabrication into a seamless workflow. This creates a more intelligent and efficient process for interoperability.

### Multiple Elements and Lists

The [first exercise](4-creating.md#exercise-generate-elements-and-lists) below will walk through how Dynamo references data for Revit element creation. To generate multiple adaptive components, we define a list of lists, where each list has three points representing each point of the adaptive component. We'll keep this in mind as we manage the data structures in Dynamo.

![](images/4/creating-multipleelementsandlists01.jpg)

### DirectShape Elements

Another method for importing parametric Dynamo geometry into Revit is with DirectShape. In summary, the DirectShape element and related classes support the ability to store externally created geometric shapes in a Revit document. The geometry can include closed solids or meshes. DirectShape is primarily intended for importing shapes from other data formats such as IFC or STEP where not enough information is available to create a "real" Revit element. Like the IFC and STEP workflow, the DirectShape functionality works well with importing Dynamo created geometries into Revit projects as real elements.

Let's walk through [second exercise](4-creating.md#exercise-directshape-elements) for importing Dynamo geometry as a DirectShape into our Revit project. Using this method, we can assign an imported geometry's category, material, and name - all while maintaining a parametric link to our Dynamo graph.

## Exercise: Generate Elements and Lists

> Download the example file by clicking on the link below.
>
> A full list of example files can be found in the Appendix.

{% file src="datasets/4/Revit-Creating.zip" %}

Beginning with the example file from this section (or continuing with the Revit file from the previous session), we see the same Revit mass.

![](images/4/creating-exercise01.jpg)

> 1. This is the file as opened.
> 2. This is the truss system we created with Dynamo, linked intelligently to the Revit mass.

We've used the _"Select Model Element"_ and _"Select Face"_ nodes, now we're taking one step further down in the geometry hierarchy and using _"Select Edge"_. With the Dynamo solver set to run _"Automatic"_, the graph will continually update to changes in the Revit file. The edge we are selecting is tied dynamically to the Revit element topology. As long as the topology\* does not change, the connection remains linked between Revit and Dynamo.

![](images/4/creating-exercise02.jpg)

> 1. Select the top most curve of the glazing facade. This spans the full length of the building. If you're having trouble selecting the edge, remember to choose the selection in Revit by hovering over the edge and hitting _"Tab"_ until the desired edge is highlighted.
> 2. Using two _"Select Edge"_ nodes, select each edge representing the cant at the middle of the facade.
> 3. Do the same for the bottom edges of the facade in Revit.
> 4. The _Watch_ nodes reveal that we now have lines in Dynamo. This is automatically converted to Dynamo geometry since the edges themselves are not Revit elements. These curves are the references we'll use to instantiate adaptive trusses across the facade.

{% hint style="info" %}
\*To keep a consistent topology, we're referring to a model that does not have additional faces or edges added. While parameters can change its shape, the way in which it is built remains consistent.
{% endhint %}

We first need to join the curves and merge them into one list. This way we can _"group"_ the curves to perform geometry operations.

![](images/4/creating-exercise03.jpg)

> 1. Create a list for the two curves at the middle of the facade.
> 2. Join the two curves into a Polycurve by plugging the _List.Create_ component into a _Polycurve.ByJoinedCurves_ node.
> 3. Create a list for the two curves at the bottom of the facade.
> 4. Join the two curves into a Polycurve by plugging the _List.Create_ component into a _Polycurve.ByJoinedCurves_ node.
> 5. Finally, join the three main curves (one line and two polycurves) into one list.

We want to take advantage of the top curve, which is a line, and represents the full span of the facade. We'll create planes along this line to intersect with the set of curves we've grouped together in a list.

![](images/4/creating-exercise04.jpg)

> 1. With a _code block_, define a range using the syntax: `0..1..#numberOfTrusses;`
> 2. Plug an \*integer slider \*into the input for the code block. As you could have guessed, this will represent the number of trusses. Notice that the slider controls the number of items in the range defined from \*0 \*to _1_.
> 3. Plug the _code block_ into the _param_ input of a _"Curve.PlaneAtParameter"_ node, and plug the top edge into the _curve_ input. This will give us ten planes, evenly distributed across the span of the facade.

A plane is an abstract piece of geometry, representing a two dimensional space which is infinite. Planes are great for contouring and intersecting, as we are setting up in this step.

![](images/4/creating-exercise05.jpg)

> 1. Using the _Geometry.Intersect_ node (set lacing option to cross product), plug the _Curve.PlaneAtParameter_ into the _entity_ input of the _Geometry.Intersect_ node. Plug the main _List.Create_ node into the _geometry_ input. We now see points in the Dynamo viewport representing the intersection of each curve with the defined planes.

Notice the output is a list of lists of lists. Too many lists for our purposes. We want to do a partial flatten here. We need to take one step down on the list and flatten the result. To do this, we use the _List.Map_ operation, as discussed in the list chapter of the primer.

![](images/4/creating-exercise06.jpg)

> 1. Plug the _Geometry.Intersect_ node into the list input of _List.Map_.
> 2. Plug a _Flatten_ node into the f(x) input of _List.Map_. The results gives 3 list, each with a count equal to the number of trusses.
> 3. We need to change this data. If we want to instantiate the truss, we have to use the same number of adaptive points as defined in the family. This is a three point adaptive component, so instead of three lists with 10 items each (numberOfTrusses), we want 10 lists of three items each. This way we can create 10 adaptive components.
> 4. Plug the _List.Map_ into a _List.Transpose_ node. Now we have the desired data output.
> 5. To confirm that the data is correct, add a _Polygon.ByPoints_ node to the canvas and double check with the Dynamo preview.

In the same way we created the polygons, we array the adaptive components.

![](images/4/creating-exercise07.jpg)

> 1. Add an _AdaptiveComponent.ByPoints_ node to the canvas, plug the _List.Transpose_ node into the _points_ input.
> 2. Using a _Family Types_ node, select the _"AdaptiveTruss"_ family, and plug this into the _FamilyType_ input of the _AdaptiveComponent.ByPoints_ node.

In Revit, we now have the ten trusses evenly spaced across the facade!

"Flex" the graph, we turn up the numberOfTrusses to 30 by changing the slider. Lots of trusses, not very realistic, but the parametric link is working. Once verified, set the numberOfTrusses to 15.

![](images/4/creating-exercise08.gif)

And for the final test, by selecting the mass in Revit and editing instance parameters, we can change the form of the building and watch the truss follow suit. Remember, this Dynamo graph has to be open in order to see this update, and the link will be broken as soon as it's closed.

![](images/4/creating-exercise09.jpg)

## Exercise: DirectShape Elements

> Download the example file by clicking on the link below.
>
> A full list of example files can be found in the Appendix.

{% file src="datasets/4/Revit-Creating-DirectShape.zip" %}

Begin by opening the sample file for this lesson - ARCH-DirectShape-BaseFile.rvt.

![](images/4/creating-exerciseII-01.jpg)

> 1. In the 3D view, we see our building mass from the previous lesson.
> 2. Along the edge of the atrium is one reference curve, we'll use this as a curve to reference in Dynamo.
> 3. Along the opposing edge of the atrium is another reference curve which we'll reference in Dynamo as well.

![](images/4/creating-exerciseII-02.jpg)

> 1. To reference our geometry in Dynamo, we'll use _Select Model Element_ for each member in Revit. Select the mass in Revit and import the geometry into Dynamo by Using _Element.Faces_ - the mass should now be visible in your Dynamo preview.
> 2. Import one reference curve into Dynamo by using _Select Model Element_ and _CurveElement.Curve_.
> 3. Import the other reference curve into Dynamo by using _Select Model Element_ and _CurveElement.Curve_.

![](images/4/creating-exerciseII-03.jpg)

> 1. Zooming out and panning to the right in the sample graph, we see a large group of nodes - these are geometric operations which generate the trellis roof structure visible in the Dynamo preview. These nodes are generating using the _Node to Code_ functionality as discussed in the [code block section](../coding-in-dynamo/7\_code-blocks-and-design-script/7-2\_design-script-syntax.md#Node) of the primer.
> 2. The structure is driven by three major parameters - Diagonal Shift, Camber, and Radius.

Zooming a close-up look of the parameters for this graph. We can flex these to get different geometry outputs.

![](images/4/creating-exerciseII-04.jpg)

![](images/4/creating-exerciseII-05.jpg)

> 1. Dropping the _DirectShape.ByGeometry_ node onto the canvas, we see that it has four inputs: _geometry_**,** _category_**,** _material_, and _name_.
> 2. Geometry will be the solid created from the geometry creation portion of the graph
> 3. The category input is chosen using the dropdown _Categories_ node. In this case we'll use "Structural Framing".
> 4. The material input is selected through the array of nodes above - although it can be more simply defined as "Default" in this case.

After running Dynamo, back in Revit, we have the imported geometry on the roof in our project. This is a structural framing element, rather than a generic model. The parametric link to Dynamo remains intact.

![](images/4/creating-exerciseII-06.jpg)
