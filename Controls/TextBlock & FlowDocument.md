# TextBlock & FlowDocument


```xml
<TextBlock Margin="10" Height="100" FontSize="14" Width="290" TextWrapping="Wrap">
    We <Bold>few</Bold>, we happy <Bold>few</Bold>, we band of <Underline>brothers</Underline>;
    For he to-day that sheds his <Italic>blood</Italic> with me
    Shall be my <Underline>brother</Underline>; be he ne'er so <Italic>vile</Italic>,
    This day shall <Bold>gentle</Bold> his <Italic>condition</Italic>;"
</TextBlock>
```

## Flow Document
The FlowDocument control provides richer functionality than Label and TextBlock for displaying text. A FlowDocument contains a collection of blocks, each of which is a separate paragraph, list, section or table.  There are five different types of blocks that you can include in a FlowDocument. A block can be one of the following:

- Paragraph – A paragraph of text
- List – A list of text items, typically displayed with a bullet character. https://wpf.2000things.com/2011/03/17/248-creating-a-list-in-a-flowdocument/
- Table – A table, with rows and columns. https://wpf.2000things.com/2011/03/18/249-creating-a-table-in-a-flowdocument/
- Section – A section of the document that can, in turn, contain other blocks. https://wpf.2000things.com/2011/03/19/250-including-a-section-block-in-a-flowdocument/
- BlockUIContainer – A placeholder that allows inserting any UIElement into the document. https://wpf.2000things.com/2011/03/20/251-embedding-an-uielement-into-a-flowdocument/
- We can also add Span – this tag can come inside paragraph and you can change fonts, color etc.



```xml
<FlowDocument FontFamily="Cambria" FontSize="16">
    <Paragraph FontFamily="Arial" FontSize="14">
        Excerpt from <Italic>White Fang</Italic>, by <Bold>Jack London</Bold>
    </Paragraph>

    <Paragraph>
        A second cry arose, piercing the silence with needle-like shrillness.  Both men located the sound.
        It was to the rear, somewhere in the snow expanse they had just traversed.  A third and answering
        cry arose, also to the rear and to the left of the second cry.
    </Paragraph>

    <Paragraph>
        “They’re after us, Bill,” said the man at the front.
    </Paragraph>

    <Paragraph>
        His voice sounded hoarse and unreal, and he had spoken with apparent effort.
    </Paragraph>
</FlowDocument>
```

Because `FlowDocument` does not derive from `UIElement`, you can’t include it in a Panel control. You can use a `FlowDocument as the main content in any ContentControl. If you do this, the FlowDocument will automatically be hosted in a FlowDocumentReader.

The FlowDocumentReader displays some portion of a FlowDocument and provides controls to either page or scroll through the rest of the document. It also supports displaying the document in multiple columns and provides an easy way to adjust the font size.

To add FlowDocument to a Panel control:


<Grid>
    <FlowDocumentScrollViewer>
        <FlowDocument>
            <Paragraph>
                <Run Text="{Binding MyProperty}"/>
            </Paragraph>
        </FlowDocument>
    </FlowDocumentScrollViewer>
</Grid>


https://wpf.2000things.com/2011/03/23/254-types-of-containers-for-hosting-flowdocument/
https://wpf.2000things.com/2011/03/24/255-flow-text-around-an-image-in-a-flowdocument/
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTcwNjkzMjMwMV19
-->