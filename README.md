# sharepoint-list-jQuery-accordion 
# asher.sandler770@gmail.com
# How to create jQuery accordion on Sharepoint web Page

#I. create list Cities
#add new column Content

#add Values


#1. 	Title : London
#	Content : Capital of England
#2.  Title : Moscow
#	Content : Capital of Russia
#3.  Title : Berlin
#	Content : Capital of Germany
#4.  Title : Paris
#	Content : Capital of France
	
#II. 

#Create new page with web parts

#III.

#Add to page web part Content Editor Web Part (CEWP) and Script Editor Web Part (SEWP)

#to CEWP in Edit Source Code mode add 
<div id="accordion"></div>


#in SEWP add snippet


<script src="/_layouts/15/sp.runtime.js" type="text/javascript"></script>
<script src="/_layouts/15/sp.js" type="text/javascript"></script>

<link rel="stylesheet" href="//code.jquery.com/ui/1.12.1/themes/base/jquery-ui.css">
<script src="https://code.jquery.com/jquery-1.12.4.js"></script>
<script src="https://code.jquery.com/ui/1.12.1/jquery-ui.js"></script> 


<script type="text/javascript"> 
 
$(document).ready(function(){ 
 if (jQuery){
		console.log("jQuery loaded");
		var listName = 'Cities';
		getListItems(listName);
	}
	else
	{
		console.log("jQuery not loaded"); 
	}


});

function getListItems(listName){
	var siteURL = _spPageContextInfo.webServerRelativeUrl;
	
	var listURL = siteURL + "/_api/web/lists/GetByTitle('"+listName+"')/items";
	
	console.log(listURL);
	
	 $.ajax({
            url: listURL,   
            type: "GET",
            dataType: "json",
            headers: {
            Accept: "application/json;odata=verbose"
            },
        success: function(data) {
			console.log(data.d);
         if (data.d.results.length > 0) 
           {
			var reshima = genReshima(data.d.results);  
			console.log(reshima);
			$('#accordion').html(reshima); 
            
            $( "#accordion" ).accordion();
           }
         else{  
            $('#accordion').html("<span>No Records Found.</span>");  
            }
        },
        error: function(error){
            alert(JSON.stringify(error));
        }  
    });
	
	
}

function genReshima(jsonArr)
{
	var reshimaContent = "";
	
	var arrLen = jsonArr.length;
	for(var i=0; i < arrLen; i++){
		
		var title   = stripHtml(jsonArr[i].Title);
		var content = stripHtml(jsonArr[i].Content)
		reshimaContent += "<h3>"+title+"</h3>"
		reshimaContent += "<div></p>"+content+"</p></div>"
		
		
		//console.log (title);
		//console.log (content);
	} 
	
	//console.log(reshimaContent);
	return reshimaContent;
}

function stripHtml(html){
    
    var temporalDivElement = document.createElement("div");
    
    temporalDivElement.innerHTML = html;
    // Retrieve the text property of the element (cross-browser support)
    return temporalDivElement.textContent || temporalDivElement.innerText || "";
}


</script> 	



