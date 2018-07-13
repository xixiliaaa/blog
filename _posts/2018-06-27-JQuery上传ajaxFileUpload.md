---
layout: post
title: "JQuery�ϴ�ajaxFileUpload"
description: JQuery�ϴ�ajaxFileUpload
date: 2018-06-27
categories: JQuery
tags: [JQuery, JS]
---

������
~~~php
UploadTravel.php�ļ�
<?php
class UploadTravel extends CI_Controller {

    public function index()
    {
       // echo 'Hello World!';
        $this->load->view("travel/up");
    }

    public function upload()
    {
    	$this->load->helper('url');
       $this->load->helper(array('form', 'url'));
        $config['upload_path']      = 'uploads';
        $config['allowed_types']    = 'xls|xlsx';
        $config['encrypt_name']       = TRUE;
        $config['overwrite']       = FALSE;
        echo $this->fileUpLoad('certimg',$config);
    }

    public function fileUpLoad($fileName, $config)
    {
        $this->load->library('upload', $config);
        $file = $this->upload->do_upload($fileName);
        if (!$file){
            $error=json_encode($this->upload->display_errors());
            return $error;
        }
        else{
            $data=$this->upload->data();
            $a=$data['file_name'];
            $data=json_encode(array('upload_data' => $data,'file_name' =>$config['upload_path'].$a));
            return $data;
        }
    }
}
~~~

view��ͼ�ļ�
up.php
~~~php
<script src="http://code.jquery.com/jquery-2.1.4.min.js"></script>
<script src="/CI_Study/static/ajaxfileupload.js"></script>



<button class="btn" onclick="addButton($(this))">�ϴ�</button>

<input style="display:none" url="url" type="text"/>

<span></span><br/><br/>


<script>
function addButton(obj)
{
	
    obj.after('<input class="formipt" style="display:none" name="certimg" id="certimgs" type="file" accept="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet" />')
    .next().change(function(){
        // show_loading();
        $.ajaxFileUpload({
            url: 'upload',
            secureuri: false,
            fileElementId: 'certimgs',
            type: 'POST',
            dataType: 'json',
            success: function(data,status)
            {  
            	console.log(data.file_name);
                alert("ok");
            },
            error: function (data)
            {
                console.log(data);
                alert("ʧ��");
            }
        });     

    }).click();
}
</script>
~~~

���ص��ϴ�js
ajaxfileupload.js
~~~javascript

jQuery.extend({

    createUploadIframe: function(id, uri)
	{
			//create frame
            var frameId = 'jUploadFrame' + id;
            
            if(window.ActiveXObject) {
                var io = document.createElement('<iframe id="' + frameId + '" name="' + frameId + '" />');
                if(typeof uri== 'boolean'){
                    io.src = 'javascript:false';
                }
                else if(typeof uri== 'string'){
                    io.src = uri;
                }
            }
            else {
                var io = document.createElement('iframe');
                io.id = frameId;
                io.name = frameId;
            }
            io.style.position = 'absolute';
            io.style.top = '-1000px';
            io.style.left = '-1000px';

            document.body.appendChild(io);

            return io			
    },
    createUploadForm: function(id, fileElementId)
	{
		//create form	
		var formId = 'jUploadForm' + id;
		var fileId = 'jUploadFile' + id;
		var form = $('<form  action="" method="POST" name="' + formId + '" id="' + formId + '" enctype="multipart/form-data"></form>');	
		var oldElement = $('#' + fileElementId);
		var newElement = $(oldElement).clone();
		$(oldElement).attr('id', fileId);
		$(oldElement).before(newElement);
		$(oldElement).appendTo(form);

        
        // for(var i in fileElementId){    
        //   var oldElement = jQuery('#' + fileElementId[i]);    
        //   var newElement = jQuery(oldElement).clone();    
        //   jQuery(oldElement).attr('id', fileId);    
        //   jQuery(oldElement).before(newElement);    
        //   jQuery(oldElement).appendTo(form);    
        // }  

        // if (typeof (fileElementId) == 'string') {
        //     fileElementId = [fileElementId];
        // }
        // for (var i in fileElementId) {
        //     //��nameȡֵ
        //     var oldElement = jQuery("input[name=" + fileElementId[i] + "]");
        //     oldElement.each(function () {
        //         var newElement = jQuery($(this)).clone();
        //         //jQuery(this).attr('id', fileId);
        //         jQuery(this).attr('name', fileId);
        //         jQuery(this).before(newElement);
        //         jQuery(this).appendTo(form);
        //     });
        // }

		//set attributes
		$(form).css('position', 'absolute');
		$(form).css('top', '-1200px');
		$(form).css('left', '-1200px');
		$(form).appendTo('body');		
		return form;
    },

    ajaxFileUpload: function(s) {
        // TODO introduce global settings, allowing the client to modify them for all requests, not only timeout		
        s = jQuery.extend({}, jQuery.ajaxSettings, s);
        var id = s.fileElementId;        
		var form = jQuery.createUploadForm(id, s.fileElementId);
		var io = jQuery.createUploadIframe(id, s.secureuri);
		var frameId = 'jUploadFrame' + id;
		var formId = 'jUploadForm' + id;		
        // Watch for a new set of requests
        if ( s.global && ! jQuery.active++ )
		{
			jQuery.event.trigger( "ajaxStart" );
		}            
        var requestDone = false;
        // Create the request object
        var xml = {}   
        if ( s.global )
            jQuery.event.trigger("ajaxSend", [xml, s]);
        // Wait for a response to come back
        var uploadCallback = function(isTimeout)
		{			
			var io = document.getElementById(frameId);
            try 
			{				
				if(io.contentWindow)
				{
					 xml.responseText = io.contentWindow.document.body?io.contentWindow.document.body.innerHTML:null;
                	 xml.responseXML = io.contentWindow.document.XMLDocument?io.contentWindow.document.XMLDocument:io.contentWindow.document;
					 
				}else if(io.contentDocument)
				{
					 xml.responseText = io.contentDocument.document.body?io.contentDocument.document.body.innerHTML:null;
                	xml.responseXML = io.contentDocument.document.XMLDocument?io.contentDocument.document.XMLDocument:io.contentDocument.document;
				}						
            }catch(e)
			{
				jQuery.handleError(s, xml, null, e);
			}
            if ( xml || isTimeout == "timeout") 
			{				
                requestDone = true;
                var status;
                try {
                    status = isTimeout != "timeout" ? "success" : "error";
                    // Make sure that the request was successful or notmodified
                    if ( status != "error" )
					{
                        // process the data (runs the xml through httpData regardless of callback)
                        var data = jQuery.uploadHttpData( xml, s.dataType );    
                        // If a local callback was specified, fire it and pass it the data
                        if ( s.success )
                            s.success( data, status );
    
                        // Fire the global callback
                        if( s.global )
                            jQuery.event.trigger( "ajaxSuccess", [xml, s] );
                    } else
                        jQuery.handleError(s, xml, status);
                } catch(e) 
				{
                    status = "error";
                    jQuery.handleError(s, xml, status, e);
                }

                // The request was completed
                if( s.global )
                    jQuery.event.trigger( "ajaxComplete", [xml, s] );

                // Handle the global AJAX counter
                if ( s.global && ! --jQuery.active )
                    jQuery.event.trigger( "ajaxStop" );

                // Process result
                if ( s.complete )
                    s.complete(xml, status);

                jQuery(io).unbind()

                setTimeout(function()
									{	try 
										{
											$(io).remove();
											$(form).remove();	
											
										} catch(e) 
										{
											jQuery.handleError(s, xml, null, e);
										}									

									}, 100)

                xml = null

            }
        }
        // Timeout checker
        if ( s.timeout > 0 ) 
		{
            setTimeout(function(){
                // Check to see if the request is still happening
                if( !requestDone ) uploadCallback( "timeout" );
            }, s.timeout);
        }
        try 
		{
           // var io = $('#' + frameId);
			var form = $('#' + formId);
			$(form).attr('action', s.url);
			$(form).attr('method', 'POST');
			$(form).attr('target', frameId);
            if(form.encoding)
			{
                form.encoding = 'multipart/form-data';				
            }
            else
			{				
                form.enctype = 'multipart/form-data';
            }			
            $(form).submit();

        } catch(e) 
		{			
            jQuery.handleError(s, xml, null, e);
        }
        if(window.attachEvent){
            document.getElementById(frameId).attachEvent('onload', uploadCallback);
        }
        else{
            document.getElementById(frameId).addEventListener('load', uploadCallback, false);
        } 		
        return {abort: function () {}};	

    },

    uploadHttpData: function( r, type ) {
        var data = !type;
        data = type == "xml" || data ? r.responseXML : r.responseText;
        // If the type is "script", eval it in global context
        if ( type == "script" )
            jQuery.globalEval( data );
        // Get the JavaScript object, if JSON is used.
        if ( type == "json" )
            eval( "data = " + data );
        // evaluate scripts within html
        if ( type == "html" )
            jQuery("<div>").html(data).evalScripts();
			//alert($('param', data).each(function(){alert($(this).attr('value'));}));
        return data;
    },

    handleError: function( s, xhr, status, e )      {  
        // If a local callback was specified, fire it  
        if ( s.error ) {  
            s.error.call( s.context || s, xhr, status, e );  
        }  

        // Fire the global callback  
        if ( s.global ) {
            (s.context ? jQuery(s.context) : jQuery.event).trigger( "ajaxError", [xhr, s, e] );  
        }  
    } 
})


~~~











