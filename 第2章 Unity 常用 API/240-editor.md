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

&emsp;

|静态方法|说明|
|:--|:--|


|CreateCachedEditor|On return previousEditor is an editor for targetObject or targetObjects. The function either returns if the editor is already tracking the objects, or Destroys the previous editor and creates a new one.|
|CreateEditor|	Make a custom editor for targetObject or targetObjects.|

&emsp;

|Messages|说明|
|:--|:--|
|OnSceneGUI|Enables the Editor to handle an event in the scene view.|


####示例：

```javascript
    using UnityEngine;
    using System.Collections;
    
    // This is not an editor script.
    public class MyPlayer : MonoBehaviour {
        public int armor = 75;
        public int damage = 25;
        public GameObject gun;
    
        void Update () {
    	// Update logic here...
        }
    }
```

![](/assets/CustomEditor.png)


```javascript
	using UnityEditor;
	using UnityEngine;
	using System.Collections;
	
	// Custom Editor using SerializedProperties.
	// Automatic handling of multi-object editing, undo, and prefab overrides.
	[CustomEditor(typeof(MyPlayer))]
	[CanEditMultipleObjects]
	public class MyPlayerEditor : Editor {
		SerializedProperty damageProp;
		SerializedProperty armorProp;
		SerializedProperty gunProp;
		
		void OnEnable () {
			// Setup the SerializedProperties.
			damageProp = serializedObject.FindProperty ("damage");
			armorProp = serializedObject.FindProperty ("armor");
			gunProp = serializedObject.FindProperty ("gun");
		}
		
		public override void OnInspectorGUI() {
			// Update the serializedProperty - always do this in the beginning of OnInspectorGUI.
			serializedObject.Update ();
			
			// Show the custom GUI controls.
			EditorGUILayout.IntSlider (damageProp, 0, 100, new GUIContent ("Damage"));
	
			// Only show the damage progress bar if all the objects have the same damage value:
			if (!damageProp.hasMultipleDifferentValues)
				ProgressBar (damageProp.intValue / 100.0f, "Damage");
			
			EditorGUILayout.IntSlider (armorProp, 0, 100, new GUIContent ("Armor"));
	
			// Only show the armor progress bar if all the objects have the same armor value:
			if (!armorProp.hasMultipleDifferentValues)
				ProgressBar (armorProp.intValue / 100.0f, "Armor");
			
			EditorGUILayout.PropertyField (gunProp, new GUIContent ("Gun Object"));
			
			// Apply changes to the serializedProperty - always do this in the end of OnInspectorGUI.
			serializedObject.ApplyModifiedProperties ();
		}
		
		// Custom GUILayout progress bar.
		void ProgressBar (float value, string label) {
			// Get a rect for the progress bar using the same margins as a textfield:
			Rect rect = GUILayoutUtility.GetRect (18, 18, "TextField");
			EditorGUI.ProgressBar (rect, value, label);
			EditorGUILayout.Space ();
		}
	}
```



🔚