# BIM Specifications


Originally elaborated by [OpeningDesign](http://www.openingdesign.com) / Ryan Schultz / Yorik van Havre and licensed under [CreativeCommons](http://creativecommons.org/licenses/by/4.0/). Feel free to use, distribute or contribute to improve the process.


### Foreword


This BIM specification, unlike most other BIM specifications available, is based on real-life, tested methods to get the job done and guarantee as much as possible the primary aim of BIM--real collaboration between the different actors of a building project.


This specification consists of simple rules to guarantee a maximum of fidelity in data transfers using the [IFC](https://en.wikipedia.org/wiki/Industry_Foundation_Classes) file format. The following rules work with any version of the IFC schema.


The correct implementation of these rules implies a good understanding of the [IFC format](http://www.buildingsmart-tech.org/ifc/IFC4x1/html/), and ways to verify the contents of IFC files others than in the software where it was authored. Several [free](http://www.ifcwiki.org/index.php/Open_Source) or [open-source](http://www.ifcwiki.org/index.php/Open_Source) applications allow to open, visualize and inspect IFC files and make sure that the rules below were correctly implemented.


---


## Rules


---

### Model Lines: Lines in 3-dimensional space




| Platform                 |Native Functionality| Import | Export |
| ------------------------ | -----------------  | ------ | ------ |
| FreeCAD                  |Any [Part-based][FC-Part_Module] object which contains edges and no faces, such as [Draft][FC-Draft_Module] or [Sketch][FC-Sketcher_Module] objects| [IfcAnnotation][IfcAnnotation] objects are imported as simple, non-parametric Part objects. Color and line style currently not supported. | Part-based objects with edges and no faces are exported as [IfcAnnotation][IfcAnnotation] |
| Revit                    |[Model Lines][Revit-Model Lines] | :heavy_check_mark: - Name, Color, & Pattern, :x: - Weight|Had to use an MVD that supports the export of lines (Coordination View 2.0 does not)|
| ArchiCAD                 |         :x:        |   :x:  |   :x:  |

Test folder: [./Specifications test files/Model Lines](./Specifications%20test%20files/Model%20Lines/)

<!-- Start of Markdown URLs for 'Model Lines' section -->

[FC-Part_Module]: https://www.freecadweb.org/wiki/Part_Module
[FC-Draft_Module]: https://www.freecadweb.org/wiki/Draft_Module
[FC-Sketcher_Module]: https://www.freecadweb.org/wiki/Sketcher_Module
[IfcAnnotation]: http://www.buildingsmart-tech.org/ifc/IFC4/final/html/schema/ifcproductextension/lexical/ifcannotation.htm
[Revit-Model Lines]: https://knowledge.autodesk.com/support/revit-products/learn-explore/caas/CloudHelp/cloudhelp/2016/ENU/Revit-Model/files/GUID-24A8763F-D8DD-4579-9CF3-BBC02EF3A314-htm.html

<!-- End of Markdown URLs for 'Model Lines' section -->

---

### Walls

| Platform                 |Native Functionality| Import | Export |
| ------------------------ | -----------------  | ------ | ------ |
| FreeCAD                  |[Walls][FC-Arch_Wall]|[IfcWall][IfcWall] and IfcStandardWall are imported as [wall][FC-Arch_Wall] objects | all [walls][FC-Arch_Wall] and any other [Arch][FC-Arch_Module]-based object which Role property is set as Wall, is exported as an [IfcWall][IfcWall].|
| Revit                    |[Walls][Revit-Walls]|from FreeCAD, the wall imports as a in-place family, that is, not an intelligent wall| :heavy_check_mark: |
| ArchiCAD                 |         :x:        |   :x:  |   :x:  |

Test folder: [./Specifications test files/Wall](Specifications%20test%20files/Wall)


**To do:**


* Check if there is any possibility to recreate a working family from an IFC file

<!-- Start of Markdown URLs for 'Walls' section -->

[FC-Arch_Wall]: https://www.freecadweb.org/wiki/Arch_Wall
[FC-Arch_Module]: https://www.freecadweb.org/wiki/Arch_Module
[IfcWall]: https://encrypted.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwiekN3amJrRAhVFHZAKHVaZBrQQFgghMAE&url=http%3A%2F%2Fwww.buildingsmart-tech.org%2Fifc%2FIFC4%2Ffinal%2Fhtml%2Fschema%2Fifcsharedbldgelements%2Flexical%2Fifcwall.htm&usg=AFQjCNFehG4I0NTWy_EHVqfUV9skODan5w&sig2=qdyLzXHB0uERtQ2MUO-NBg
[Revit-Walls]: https://knowledge.autodesk.com/support/revit-products/learn-explore/caas/CloudHelp/cloudhelp/2016/ENU/Revit-Model/files/GUID-9A9F7D00-A6A1-4E17-8C74-0965FDA5E007-htm.html

<!-- End of Markdown URLs for 'Walls' section -->

---


### Materials

The notion of material in IFC is spread over different types: objects can have a [IfcStyledItem][IfcStyledItem] applied directly to them, that allows to set for example a color, an [IfcMaterial][IfcMaterial], that actually contains little more (it also contains an IfcStyledItem), and different [IfcProperties][IfcProperties] that is where the physical properties of a material can be stored.

Any Ifc object can have any of the above. For the material to work in Revit, it must have a IfcStyledItem directly applied on the object, and an IfcMaterial. Both the IfcStyledItem and the IfcMaterial contain an [IfcSurfaceStyle][IfcSurfaceStyle]. It is important that both these IfcSurfaceStyle have the same name as the material. So for any given object with a material, you need three IFC entities with the same name.


| Platform                 |Native Functionality| Import | Export |
| ------------------------ | -----------------  | ------ | ------ |
| FreeCAD                  |[Material][FC-Arch_SetMaterial]| For objects with an associated IfcMaterial, a FreeCAD material is created and linked to the object. For objects that only have a Surface Style directly applied, no material is created in FreeCAD, but the object takes the color of the Surface Style.| If FreeCAD objects have a material, it is exported as an IfcMaterial with corresponding Surface Style. Otherwise, only the shape color is exported as Surface Style. |
| Revit                    |         :x:        |   :x:  |   :x:  |
| ArchiCAD                 |         :x:        |   :x:  |   :x:  |

<!-- Start of Markdown URLs for 'Materials' section -->

[IfcStyledItem]: http://www.buildingsmart-tech.org/ifc/IFC2x4/rc1/html/ifcpresentationappearanceresource/lexical/ifcstyleditem.htm
[IfcMaterial]: http://www.buildingsmart-tech.org/ifc/IFC2x4/alpha/html/ifcmaterialresource/lexical/ifcmaterial.htm
[IfcProperties]: http://www.buildingsmart-tech.org/ifc/IFC2x4/rc1/html/ifcpropertyresource/lexical/ifcpropertysinglevalue.htm
[IfcSurfaceStyle]: http://www.buildingsmart-tech.org/ifc/IFC2x3/TC1/html/ifcpresentationappearanceresource/lexical/ifcsurfacestyle.htm
[FC-Arch_SetMaterial]: https://www.freecadweb.org/wiki/Arch_SetMaterial

<!-- End of Markdown URLs for 'Materials' section -->

---

### Name and Description: All objects and materials should have a human-readable name or description

Objects should have a name that allows a human being to understand what it is, in case the software that reads the IFC file fails to recognize or categorize it appropriately. For example, bad names are "Object00014", "Material43". Good names are "Kitchen chair", "Grey concrete", "East living room wall"

All [IFC objects][IfcRoot] have a **Name** and a **Description**. Any of them can be used for this purpose.

This rule mainly applies to [IfcProduct][IfcProduct] entities in IFC: They are the final, individual objects that compose a building. For example, a wall, a column, a chair, a window. It doesn't consider objects that are part of a final product, when they are composed of multiple objects, for example a leg of a chair or a brick of a wall. It is sufficient to have the chair correctly named, not necessarily each component of the chair.


| Platform                 |Native Functionality| Import | Export |
| ------------------------ | -----------------  | ------ | ------ |
| FreeCAD                  |[Label property][FC-Property_Editor] (all FreeCAD objects), Description property (only [Arch objects][FC-Arch_Module])| IFC name translated to FreeCAD object **label** `('My test wall')`, IFC **description** `('This is a test wall to check rule number one')` imported into FreeCAD object description if available | FreeCAD object label exported as IFC name, FreeCAD object description, if present, exported as IFC description |
| Revit                    |      :question:    |Upon import, **name** and **description** is not accessible to modify from Revit UI. (*See Screenshots [1][screenshot1] & [2][screenshot2])*      |  **Name** and **description** exports out correctly.      |
| ArchiCAD                 | :question: | :question: | :question: |

Test folder: [./Specifications test files/Name and Description](Specifications%20test%20files/Name%20and%20Description)


**To do:**


* Check where name and description are stored in Revit, maybe that could be accessed by dynamo or something. See if Revit doesn't provide any alternative (different ways to "label" objects)

<!-- Start of Markdown URLs for 'Name and Description' section -->

[IfcRoot]: http://www.buildingsmart-tech.org/ifc/IFC4x1/html/schema/ifckernel/lexical/ifcroot.htm
[IfcProduct]: http://www.buildingsmart-tech.org/ifc/IFC4x1/html/schema/ifckernel/lexical/ifcproduct.htm
[FC-Property_Editor]: https://www.freecadweb.org/wiki/Property_editor
[FC-Arch_Module]: https://www.freecadweb.org/wiki/Arch_Module
[screenshot1]: ./Specifications%20test%20files/wall_with_name_and_description/Revit%20Properties_1.png
[screenshot2]: ./Specifications%20test%20files/wall_with_name_and_description/Revit%20Properties_2.png

<!-- End of Markdown URLs for 'Name and Description' section -->

---

### Nested Groups: All objects should be grouped in meaningful ways


Grouping objects using [IfcGroups][IfcGroup] allows a human being to clearly recognize objects as being part of a same area, function or category. Groups can be nested inside other groups. A same object cannot be part of several groups.

| Platform                 |Native Functionality| Import | Export |
| ------------------------ | -----------------  | ------ | ------ |
| FreeCAD                  |[Groups][FC-Group]  | IFC groups translated to FreeCAD groups. Nesting is respected. | FreeCAD groups are exported to IFC groups, but groups are not part of IfcBuildingStoreys (**Problem**: IfcGroups cannot be nested into IfcBuildingStoreys) |
| Revit                    |[Groups of Elements][Revit-Group] | Does not import IFCgroups into Revit Groups      |  Yes, exports `#253= IFCGROUP('2wfBgyl9H71872FVeaZPs0',#41,'Model Group:Test Revit Group:149951',$,'Model Group:Test Revit Group');`                       |
| ArchiCAD                 | :question: | :question: | :question: |

Test folder: [./Specifications test files/Nested Groups](Specifications%20test%20files/Nested%20Groups)


**To do:**


* IFC offers two different "structures" to group and organize contents: [space-related](http://www.buildingsmart-tech.org/ifc/IFC4x1/html/schema/ifcproductextension/lexical/ifcrelcontainedinspatialstructure.htm) (Building->BuildingStorey->Zone->Space) and non-space-related ([Group](http://www.buildingsmart-tech.org/ifc/IFC4x1/html/schema/ifckernel/lexical/ifcgroup.htm) ). These two structures are fully independent and cannot be combined (you cannot add a group to a spatial structure element). Check if using IFC Groups is the most adequate form, and if it wouldn't be better to switch to a full space-related system.

<!-- Start of Markdown URLs for 'Nested Groups' section -->

[IfcGroup]: http://www.buildingsmart-tech.org/ifc/IFC4x1/html/schema/ifckernel/lexical/ifcgroup.htm
[FC-Group]: https://www.freecadweb.org/wiki/Group
[Revit-Group]: https://knowledge.autodesk.com/support/revit-products/learn-explore/caas/CloudHelp/cloudhelp/2016/ENU/Revit-Model/files/GUID-52612B0F-43AA-47AF-A76C-BB0E3DD24E34-htm.html

<!-- End of Markdown URLs for 'Nested Groups' section -->

---


### Duplicatable components


A way to be able to group a series of objects into a same structure, let's call it a component, and duplicate that component in the model. If one object is changed, added or removed from/to the base component, all instances of that component should update automatically. This is typically how "blocks" work in AutoCAD, or "components" in SketchUp, or "compounds" in FreeCAD. An example would be a restroom stall, with all its parts and accessories: door, wc basin, paper hanger, sink, etc.


| Platform                 |Native Functionality| Import | Export |
| ------------------------ | -----------------  | ------ | ------ |
| FreeCAD                  |[Compound][FC-Part_MakeCompound]| :question: | :question: |
| Revit                    |Groups              |   :x:  |   :heavy_check_mark: |
| Revit                    |Families            |   :x:  |   :x:  |
| Blender                  |Groups/Collections  |   :x:  |   :x:  |
| ArchiCAD                 | :question: | :question: | :question: |

<!-- Start of Markdown URLs for 'Duplicatable components' section -->

[FC-Part_MakeCompound]: https://www.freecadweb.org/wiki/Part_MakeCompound

<!-- End of Markdown URLs for 'Duplicatable components' section -->

---




---


### Generic components

IFC objects that are not of a specific type (wall, beam, etc) are usually saved into IFC as [BuildingElementProxy][BEP]. This is often used by applications that don't classify geometric objects by type (most non-BIM applications like Blender or Sketchup), but can be also used when you want a specific object to be rendered as less parametric as possible.

| Platform                 |Native Functionality| Import | Export |
| ------------------------ | -----------------  | ------ | ------ |
| FreeCAD                  |[Part shape][FC-Part_Module] | [`BuildingElementProxy`][BEP] objects are rendered either as simple, non-parametric Part shapes, or as generic [Arch components][FC-Arch_Component] depending on the import preference settings. The Arch component will retain the IFC properties and material attached to the object, while the Part shape not.| Generic [Arch components][FC-Arch_Component] and any other FreeCAD object that is not one of the [Arch][FC-Arch_Module] building elements (Wall, structure, etc) will be exported as `BuildingElementPRoxy`.|
| Revit                    |Generic component   |   :x:  |   :x:  |
| ArchiCAD                 |         :x:        |   :x:  |   :x:  |

<!-- Start of Markdown URLs for 'Generic components' section -->

[BEP]: http://www.buildingsmart-tech.org/ifc/IFC2x3/TC1/html/ifcproductextension/lexical/ifcbuildingelementproxy.htm
[FC-Part_Module]: https://www.freecadweb.org/wiki/Part_Module
[FC-Arch_Component]: https://www.freecadweb.org/wiki/Arch_Component
[FC-Arch_Module]: https://www.freecadweb.org/wiki/Arch_Module

<!-- End of Markdown URLs for 'Generic components' section -->

---


### Safe geometry types


**To be developed:**


Some geometry types, although they import correctly in all applications, are sometimes not editable (the concept of what editable means needs to be developed as well). This item should identify the geometry types that are "safe".


Note that in any application, safe geometry only means that: The geometry will be safely reproduced. All the semantic layers on top of the geometry (type, properties, materials, etc) is independent from the geometry itself. A faithfully rendered geometry doesn't necessarily mean that all these pieces of information will be reproduced correctly as well.


**Safe geometry for Revit**


* **Extrusions**: Revit will treat all extrusion (derived from [If ExtrudedAreaSolid](http://www.buildingsmart-tech.org/ifc/IFC2x3/TC1/html/ifcgeometricmodelresource/lexical/ifcextrudedareasolid.htm) ) as native Revit extusions, which is the basic condition to be editable. 
* **Faceted Breps**: Revit is also able sometimes to recognize extrusions from very simple and prismatic [IfcFacetedBrep](http://www.buildingsmart-tech.org/ifc/IFC2x4/rc1/html/ifcgeometricmodelresource/lexical/ifcfacetedbrep.htm) objects, and therefore consider them as editable too. All the rest will be non-editable (excluding untested types below).


**Safe geometry for FreeCAD**


* A priori, anything is safe in FreeCAD. The [IfcOpenShell](http://ifcopenshell.org/) engine, which is responsible for importing and exporting IFC files in FreeCAD, supports almost all the possible IFC geometry types and creates native FreeCAD geometry from them. However, FreeCAD doesn't feature the same "editability" concept as Revit. Objects are not editable or not by nature. However, the nature of FreeCAD objects, which are all breakable/reconstructable, allows in theory any object to be modified. However a couple of types will render in a more parametric way:
* **Extrusions**: all extrusion (derived from [If ExtrudedAreaSolid](http://www.buildingsmart-tech.org/ifc/IFC2x3/TC1/html/ifcgeometricmodelresource/lexical/ifcextrudedareasolid.htm) ) objects will be rendered as [Part Extrusions](https://www.freecadweb.org/wiki/Part_Extrude) or, if they are of a type whose corresponding [Arch](https://www.freecadweb.org/wiki/Arch_Module) equivalent is extrudable (Structure or Wall), as such.


**To be tested:**


* Vertical (Z-axis) [extrusions](http://www.buildingsmart-tech.org/ifc/IFC4/final/html/schema/ifcgeometricmodelresource/lexical/ifcextrudedareasolid.htm) of any profile (straight/only lines, complex/curved, and predefined types)
* Arbitrary (any direction) extrusions of any profile
* [Booleans](http://www.buildingsmart-tech.org/ifc/IFC4/final/html/schema/ifcgeometricmodelresource/lexical/ifcbooleanresult.htm) (unions, differences, intersections)
* [Faceted Brep](http://www.buildingsmart-tech.org/ifc/IFC4/final/html/schema/ifcgeometricmodelresource/lexical/ifcfacetedbrep.htm) shapes
* [Advanced Brep](http://www.buildingsmart-tech.org/ifc/IFC4/final/html/schema/ifcgeometricmodelresource/lexical/ifcadvancedbrep.htm) shapes


---


## Tools

Use free/open-source IFC applications to validate the data inside IFC files.

It is fundamental for the author of an IFC file to be fully aware of what has been included in that file. Therefore, it is essential to be able to verify the contents of the file in a neutral manner (independent of the application that exported it). It should also be possible for other people to easily open that file, and verify its contents, independently of the application used to import it.


**Items that should be checked on opening an IFC file for verification**:


* The application used for verification reports no error on opening the file
* The total number of objects informed by the application used for verification matches the number of objects informed by the application used for exporting
* All the geometry is there when inspecting the 3D model in the application used for verification
* All the geometry is at its correct location when inspecting the 3D model in the application used for verification


**To be developed:**


* For now the only reliable one I know that is open-source and cross-platform is [IfcPlusPlus](http://www.ifcplusplus.com/) which does a fairly good job. If it prints no error, and all objects appear in place, it generally means the data is of very good quality. [BimServer](http://bimserver.org/) might become a perfect option once it has good data validation plugins. IfcPlusPlus doesn't support IfcAdvancedBrep (Still not checked with BimServer).




<!-- Table Template


| Platform                 |Native Functionality| Import | Export |
| ------------------------ | -----------------  | ------ | ------ |
| FreeCAD                  |         :x:        |   :x:  |   :x:  |
| Revit                    |         :x:        |   :x:  |   :x:  |
| ArchiCAD                 |         :x:        |   :x:  |   :x:  |

LEGEND (emojis)
 :x:
 :heavy_check_mark:
 :question:
-->


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTExMzIxMDM4NTgsLTM4NjA5MDI5MF19
-->