##Handles


```
Static Variables

centerColor	Color to use for handles that represent the center of something.
color	Look up or set the Color of the handles.
inverseMatrix	The inverse of the matrix for all handle operations.
lighting	Are handles lit?
matrix	Matrix for all handle operations.
secondaryColor	Soft color to use for for less interactive UI, or handles that are used rarely (or not at all).
selectedColor	Color to use for the currently active handle.
xAxisColor	Color to use for handles that manipulate the X coordinate of something.
yAxisColor	Color to use for handles that manipulates the Y coordinate of something.
zAxisColor	Color to use for handles that manipulates the Z coordinate of something.
```

```
Variables

currentCamera	Setup viewport and stuff for a current camera.
Static Functions

ArrowCap	Draw an arrow like those used by the move tool.
ArrowHandleCap	Draw an arrow like those used by the move tool.
BeginGUI	Begin a 2D GUI block inside the 3D handle GUI.
Button	Make a 3D button.
CircleCap	Draw a camera-facing Circle. Pass this into handle functions.
CircleHandleCap	Draw a circle handle. Pass this into handle functions.
ClearCamera	Clears the camera.
ConeCap	Draw a Cone. Pass this into handle functions.
ConeHandleCap	Draw a cone handle. Pass this into handle functions.
CubeCap	Draw a cube. Pass this into handle functions.
CubeHandleCap	Draw a cube handle. Pass this into handle functions.
CylinderCap	Draw a Cylinder. Pass this into handle functions.
CylinderHandleCap	Draw a cylinder handle. Pass this into handle functions.
Disc	Make a 3D disc that can be dragged with the mouse.
DotCap	Draw a camera-facing dot. Pass this into handle functions.
DotHandleCap	Draw a dot handle. Pass this into handle functions.
DrawAAConvexPolygon	Draw anti-aliased convex polygon specified with point array.
DrawAAPolyLine	Draw anti-aliased line specified with point array and width.
DrawBezier	Draw textured bezier line through start and end points with the given tangents.
DrawCamera	Draws a camera inside a rectangle.
DrawDottedLine	Draw a dotted line from p1 to p2.
DrawDottedLines	Draw a list of dotted line segments.
DrawLine	Draw a line from p1 to p2.
DrawLines	Draw a list of line segments.
DrawPolyLine	Draw a line going through the list of all points.
DrawSelectionFrame	Draw a camera facing selection frame.
DrawSolidArc	Draw a circular sector (pie piece) in 3D space.
DrawSolidDisc	Draw a solid flat disc in 3D space.
DrawSolidRectangleWithOutline	Draw a solid outlined rectangle in 3D space.
DrawWireArc	Draw a circular arc in 3D space.
DrawWireCube	Draw a wireframe box with center and size.
DrawWireDisc	Draw the outline of a flat disc in 3D space.
EndGUI	End a 2D GUI block and get back to the 3D handle GUI.
FreeMoveHandle	Make an unconstrained movement handle.
FreeRotateHandle	Make an unconstrained rotation handle.
GetMainGameViewSize	Get the width and height of the main Game view.
Label	Make a text label positioned in 3D space.
MakeBezierPoints	Retuns an array of points to representing the bezier curve. See Handles.DrawBezier.
##PositionHandle	Make a 3D Scene view position handle.
RadiusHandle	Make a Scene view radius handle.
RectangleHandleCap	Draw a rectangle handle. Pass this into handle functions.
RotationHandle	Make a Scene view rotation handle.
ScaleHandle	Make a Scene view scale handle.
ScaleSlider	Make a directional scale slider.
ScaleValueHandle	Make a single-float draggable handle.
SelectionFrame	Draw a camera facing selection frame.
SetCamera	Set the current camera so all Handles and Gizmos are draw with its settings.
Slider	Make a 3D slider.
Slider2D	Slide a handle in a 2D plane.
SnapValue	Rounds the value val to the closest multiple of snap (snap can only be positive).
SphereCap	Draw a Sphere. Pass this into handle functions.
SphereHandleCap	Draw a sphere handle. Pass this into handle functions.
```

```
Delegates

CapFunction	The function to use for drawing the handle e.g. Handles.RectangleCap.
DrawCapFunction	The function to use for drawing the handle e.g. Handles.RectangleCap.
```