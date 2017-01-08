##Editor

|变量|说明|
|:--|:--|
|serializedObject|A SerializedObject representing the object or objects being inspected.|
|target|The object being inspected.|
|targets|An array of all the object being inspected.|


&emsp;

|公有函数|说明|
|:--|:--|
|DrawDefaultInspector|	Draw the built-in inspector.|
|DrawHeader|Call this function to draw the header of the editor.|
|DrawPreview|The first entry point for Preview Drawing.|
|GetInfoString|	Implement this method to show asset information on top of the asset preview.|
|GetPreviewTitle|Override this method if you want to change the label of the Preview area.|
|HasPreviewGUI|	Override this method in subclasses if you implement OnPreviewGUI.|
|OnInspectorGUI|Implement this function to make a custom inspector.|
|OnInteractivePreviewGUI|Implement to create your own interactive custom preview. Interactive custom previews are used in the preview area of the inspector and the object selector.|
|OnPreviewGUI|Implement to create your own custom preview for the preview area of the inspector, primary editor headers and the object selector.|
|OnPreviewSettings|Override this method if you want to show custom controls in the preview header.|
|RenderStaticPreview|Override this method if you want to render a static preview that shows.|
|Repaint|Repaint any inspectors that shows this editor.|
|RequiresConstantRepaint|Does this edit require to be repainted constantly in its current state?|
|UseDefaultMargins|Override this method in subclasses to return false if you don't want default margins.|


Static Functions

CreateCachedEditor	On return previousEditor is an editor for targetObject or targetObjects. The function either returns if the editor is already tracking the objects, or Destroys the previous editor and creates a new one.
CreateEditor	Make a custom editor for targetObject or targetObjects.
Messages

OnSceneGUI	Enables the Editor to handle an event in the scene view.