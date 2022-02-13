---
layout: post
title: How to upload a files in php
date: 2018-02-20 00:21:04.000000000 +05:30
type: post
parent_id: '0'
published: true
password: ''
status: publish
categories:
- php
tags: []
meta:
  _edit_last: '1'
  _yoast_wpseo_content_score: '90'
  _yoast_wpseo_primary_category: '60'
  _yoast_wpseo_focuskw_text_input: upload multiple files in php
  _yoast_wpseo_focuskw: upload multiple files in php
  _yoast_wpseo_linkdex: '35'
author:
  email: ankit@bytefold.com
  display_name: Ankit Katiyar
  first_name: Ankit
  last_name: Katiyar
permalink: "/upload-files-php/"
---
At some point in time, we all had requirements to upload some file on a website whether it's for gallery image or CV for a career page.

Here we have created a simple page that will upload multiple files and place them in a directory. We will also have a look at how to manage files using DB so that you can show them in your gallery/front end.

## Upload some files to directory

First, you need to add _ **enctype&nbsp;** _in your form.

```
<form method="post" enctype="multipart/form-data">
	<input style="margin-top:30px" id="image1" name="image1" multiple="multiple" type="file" />
        <button type="submit" name="submit" class="btn btn-primary">Submit</button>
</form>
```

once this form is submitted it will set the value of&nbsp;_ **submit** _ as we have named our button as submit. You can use below code so your file gets uploaded and saved to _images&nbsp;_directory inside your server.

&nbsp;

```php
<?php
	$uploadedImages=false;
// check if form was submitted
if(isset($_POST['submit']))
{
	$imagesUpdated[]=array();
	$imageNamePrefix="new_";
	// directory where you want to keep files
	$fileDirectory="images/";
	// Look for files and upload them to images folder
	if(sizeof($_FILES)!=0){
		$counter=0;
		foreach($_FILES as $key => $image_data)
		{
			if($image_data['error']==0)
			{
				//echo "image $counter is valid temp path : ".$image_data['tmp_name']." actual path: images/".$image_data['name'];
				//generate a unique file name
				$fileName = $imageNamePrefix . ($counter+1).'_'.$image_data['name'];
				$uploaded = move_uploaded_file($image_data['tmp_name'], $fileDirectory.$fileName);
				if($uploaded){
					$imagesUpdated[$counter]=$fileName;
				}else{
					$imagesUpdated[$counter]=null;
				}
			}
			$counter++;
		}
	}
}
?>
```

&nbsp;

This code supports for&nbsp;multiple files upload. So you can have multiple file&nbsp;tags inside your form and all of them will be uploaded.

**Note:** Make sure you have directory _ **images** _ where you have your code or change the name in php code.

Get complete file from [GitHub](https://github.com/ankitkatiyar91/bytefold/blob/master/php/file-upload.php)

&nbsp;

&nbsp;

