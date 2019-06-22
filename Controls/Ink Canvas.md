# Ink Canvas

You can use an `InkCanvas` to draw using a mouse, Paint style. You can add controls, but you can't interact with them. If you don’t give an `InkCanvas` an explicit `Height` or `Width`, but leave both to default to Auto, the InkCanvas automatically sets its `MinHeight` property to 250 and its `MinWidth` to 350.

```xml
<InkCanvas MinHeight="0" MinWidth="0">
    <Label Content="Draw on me" InkCanvas.Left="5" InkCanvas.Right="10"/>
    <Button Content="Me too" InkCanvas.Left="10" InkCanvas.Bottom="10" />
    <Image Source="http://www.gstatic.com/tv/thumb/movieposters/1312/p1312_p_v8_aa.jpg" Height="200" InkCanvas.Right="5" InkCanvas.Top="5"/>
</InkCanvas>
```

## Stroke Collection
When you draw on an `InkCanvas` control, you create a series of `Stroke` objects. One `Stroke` is created each time you hold the left mouse button down, drag the mouse to draw and then release the mouse button. All strokes are stored in the Strokes property of the `InkCanvas`.

Each `Stroke` stores information about its drawing attributes (e.g. color, width) as well as the collection of points that make up the `Stroke`, stored in its `StylusPoints` collection.


## Editing Modes
The `InkCanvas` control has an `EditingMode` property that allows you to change how you interact with the `InkCanvas`. You can draw on it, select strokes that you’ve already drawn, or erase strokes.

`EditingMode` can take on one of the following values:

- `None – You can’t drawn on the InkCanvas at all
- `Ink` – You can draw strokes, using a mouse or stylus
- `GestureOnly` – Responds to gestures, does not allow drawing. Used to let the user draw a shape and then the application tries to determine the shape they drew, e.g. circle, square. 
- `InkAndGesture` – Responds to gestures, or allows drawing
- `Select` – You can select elements that you previously drew
- `EraseByPoint` – You can use the mouse or stylus as an eraser
- `EraseByStroke` – You can erase entire strokes by clicking on them


## Style
You can specify a number of different drawing attributes that affect how new strokes appear when drawing on an InkCanvas control. You set the `DefaultDrawingAttributes` property of the `InkCanvas` to an instance of the `DrawingAttributes` class, which contains properties that you can set to change how new strokes appear.

You can modify the style of existing strokes by changing the instance of the `DrawingAttributes` class associated with the desired object in the `InkCanvas.Strokes` collection.

Properties of `DrawingAttributes` include:

- `Color` – the color of the new stroke
- `Height` – height of the brush used to draw a stroke
- `Width` – width of the brush used to draw a stroke
- `FitToCurve` – if true, smooths out the stroke
- `IsHighlighter` – if true, stroke is somewhat translucent, simulating a highlighter



## Gestures
https://wpf.2000things.com/2012/02/07/489-using-the-inkcanvas-to-recognize-gestures-part-i/
https://wpf.2000things.com/2012/02/08/490-using-the-inkcanvas-to-recognize-gestures-part-ii/
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1Nzk2MzcwMTVdfQ==
-->