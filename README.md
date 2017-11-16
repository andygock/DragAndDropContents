# DragAndDropContents.js

Small lightweight HTML Drag and Drop library (5kB uncompressed).

I wrote this with the aim of drag and dropping a text file into a `<textarea>` field, and filling the textarea with the file contents.

## Example use

First load the `DragAndDropContents.js` file:

```html
<script src="DragAndDropContents.js"></script>
```

The following will set up the textarea drop zone. Dropping text files into the textarea will make the browser read the file and fill the textarea with the file contents.

```html
<p>Drag a text file into the textarea below.</p>
<div>
    <textarea id="content1" rows="20" cols="80" value=""></textarea>
</div>

<script type="text/javascript">
    var content = document.getElementById("content1");
    DragAndDropContents.read({
        drop: 'textarea#content1',
        maxFileSize: 100000,
        onError: function(msg) {
            console.error('Error: ' + msg);
            content.value = "Errors occured, details were printed to browser console log.";
        },
        classOnDragOver: 'hover',
    }, function(result) {
        content.value = result.content;
    });
</script>
```

The main JavaScript syntax is:

    DragAndDropContents.read(options, callback)

`callback` Callback function when data is successfully read by FileReader. The parameter is an object with names:

- `file` File object
- `content` Contents of the file that was read (it may not be text if you have configured it otherwise)

`option`s available are:

- `drop` (string) Selector where the drag and drop is triggered.
- `maxFileSize` (number) Number in bytes
- `onError` (function) Called if error occurs, parameter is the error message a string
- `classOnDragOver` (string) Class applied when file is dragged over the drop area. Class is removed when taken out of the drop area, or the file is dropped.
- `acceptedTypes` (array) Array of accepted file types
- `readAs` (string) Sets the method used to read the `FileReader` object. Default is `Text` which will invoke [readAsText()](https://developer.mozilla.org/en-US/docs/Web/API/FileReader/readAsText). Other available options are: `ArrayBuffer`, `BinaryString` and `DataURL`.
- `formData` (function) Callback function with the [FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData) object as parameter. This may be useful if you wish to use the data to perform a POST request with XMLHttpRequest etc.
