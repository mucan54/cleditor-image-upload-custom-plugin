#cleditor-imageupload-custom-plugin

This is simple plug-in for [CLEditor](http://premiumsoftware.net/cleditor) simple, lightweight jQuery plugin (WYSIWYG editor). 

This plugin allows to upload and use the uploaded pictures.

##Usage:

Add to your .html file next several lines.

to "&lt;head&gt;" path:
```html
	<link rel="stylesheet" media="screen" href="css/bootstrap.css"/>	
	<link rel="stylesheet" href="js/cleditor/jquery.cleditor.css" type="text/css">

	<script type="text/javascript" src="js/jquery-{version}.js"></script>	
	<script type="text/javascript" src="js/cleditor/jquery.cleditor.js"></script>
	<script type="text/javascript" src="js/cleditor-imageupload-plugin.js"></script>
	<script type="text/javascript">
		$(document).ready(function() {
			// Url for file upload 
			$.cleditor.buttons.image.uploadUrl = 'image-upload.php';
			// Url for images list
			$.cleditor.buttons.image.imageListUrl = 'image-list.php';
	
			$.cleditor.defaultOptions.width = 600;
			$.cleditor.defaultOptions.height = 300;
			$("#input").cleditor();
     });
   </script>
```


to "&lt;body&gt;" path:	
```html
	<textarea id="input" name="input"></textarea>	
	<script src="js/bootstrap.min.js"></script>	
```

**config.php:**	
```html
<?php
$imgDirectory = 'img/';
```

**image-upload.php:**	
```html
<?php
include_once 'config.php';
$fileName = $_FILES['imageName']['name'];
$ext = substr(strrchr(strtolower($fileName), '.'), 1);
$extArr = array('jpg', 'jpeg', 'gif', 'png', 'bmp');

if (in_array($ext, $extArr)) {
    $tmpFileName = $_FILES['imageName']['tmp_name'];
    $newName = 'chimay235-' . time() . '-' . $fileName . $ext;
    move_uploaded_file($tmpFileName, $imgDirectory . $newName);
    echo '<div id="image">' . $imgDirectory . $newName . '</div>';
} else {
    
}
?>	
```

**image-list.php:**	
```html
<?php
include_once 'config.php';
$images = glob($imgDirectory . "*.*"); //All images
//$images = glob($imgDirectory . "*.jpg"); //Only jpg images

//Show by date desc
usort($images, function ($a, $b) {
            return filemtime($b) - filemtime($a);
        });

$imgNames = array();
foreach ($images as $image) {
    $imgNames[] = $image;
}
echo json_encode($imgNames);
?>
```


	
