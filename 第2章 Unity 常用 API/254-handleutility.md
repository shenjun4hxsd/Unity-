##HandleUtility

```
Static Variables

acceleration	Get standard acceleration for dragging values (Read Only).
niceMouseDelta	Get nice mouse delta to use for dragging a float value (Read Only).
niceMouseDeltaZoom	Get nice mouse delta to use for zooming (Read Only).

```

```
Static Functions

AddControl	Record a distance measurement from a handle.
AddDefaultControl	Add the ID for a default control. This will be picked if nothing else is.
CalcLineTranslation	Map a mouse drag onto a movement along a line in 3D space.
ClosestPointToArc	Get the point on an arc (in 3D space) which is closest to the current mouse position.
ClosestPointToDisc	Get the point on an disc (in 3D space) which is closest to the current mouse position.
ClosestPointToPolyLine	Get the point on a polyline (in 3D space) which is closest to the current mouse position.
DistancePointBezier	Calculate distance between a point and a Bezier curve.
DistancePointLine	Calculate distance between a point and a line.
DistancePointToLine	Distance from a point p in 2d to a line defined by two points a and b.
DistancePointToLineSegment	Distance from a point p in 2d to a line segment defined by two points a and b.
DistanceToArc	Pixel distance from mouse pointer to a 3D section of a disc.
DistanceToCircle	Pixel distance from mouse pointer to camera facing circle.
DistanceToDisc	Pixel distance from mouse pointer to a 3D disc.
DistanceToLine	Pixel distance from mouse pointer to line.
DistanceToPolyLine	Pixel distance from mouse pointer to a polyline.
DistanceToRectangle	Pixel distance from mouse pointer to a rectangle on screen.
GetHandleSize	Get world space size of a manipulator handle at given position.
##GUIPointToWorldRay	Convert 2D GUI position to a world space ray.
PickGameObject	Pick game object closest to specified position.
PickRectObjects	Pick GameObjects that lie within a specified screen rectangle.
PointOnLineParameter	Returns the parameter for the projection of the point on the given line.
PopCamera	Retrieve all camera settings.
ProjectPointLine	Project point onto a line.
PushCamera	Store all camera settings.
RaySnap	Casts ray against the scene and report if an object lies in its path.
Repaint	Repaint the current view.
WorldPointToSizedRect	Calculate a rectangle to display a 2D GUI element near a projected point in 3D space.
WorldToGUIPoint	Convert world space point to a 2D GUI position.

```