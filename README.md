CodeGist
========
Corona SDK object positioning

Example.
    local otherText = display.newText("Hello Me", 10, 10, nil, 14)
    local text = display.newText( "Hello world", 0, 0, nil, 14 )
    positionObject({object=text,x=display.contentWidth * .5,y=display.contentHeight * .5}) -- positions at the center
    positionObject({object=text,marginLeft=10,marginTop=10}) -- positions at the top left corner with a left margin and a top margin
    positionObject({object=text,other=otherText,margin=10}) -- positions at the right(default) of the other object with a margin of 10
    positionObject({object=text,other=otherText,margin=10,where="bottom"}) -- positions at the bottom of the other object with a margin of 10

Code Gist

--[[
{
object,
otherMargin,
where,
margin,
marginTop,
marginRight,
marginBottom,
marginLeft,
x,
y}
]]
function positionObject(params)
	local params=params or {}
	local object=params.object or nil
	local other=params.other or nil
	local where=params.where or "right"
	local otherMargin=params.otherMargin or 0
	local marginTop=params.marginTop or nil
	local marginRight=params.marginRight or nil
	local marginBottom=params.marginBottom or nil
	local marginLeft=params.marginLeft or nil			
	local xPos=params.x or nil
	local yPos=params.y or nil
	local objectWidthHalf
	local objectHeightHalf
	local otherWidthHalf
	local otherHeightHalf
	if object then
		objectWidthHalf=object.width/2
		objectHeightHalf=object.height/2
	else
		local alert=native.showAlert("Error","Need an object to position.",{"OK"})
	end
	if other then 
		otherWidthHalf=other.width/2
		otherHeightHalf=other.height/2
	end
	if other and object then
		if where=="top" then
			object.x=other.x
			object.y=other.y-objectHeightHalf-otherMargin-objectHeightHalf
		elseif where=="right" then
			object.x=other.x+otherWidthHalf+otherMargin+objectWidthHalf
			object.y=other.y
		elseif where=="bottom" then
			object.x=other.x
			object.y=other.y+objectHeightHalf+otherMargin+objectHeightHalf
		elseif where=="left" then
			object.x=other.x-otherWidthHalf-otherMargin-objectWidthHalf
			object.y=other.y
		end
	end
	if object then
		if marginTop and not marginBottom then 
			if where=="top" or where=="bottom" then
				local alert=native.showAlert("Warning","You can't use marginTop/marginBottom with otherMargin.",{"OK"}) 
			else
				object.y=marginTop+objectHeightHalf	
			end
		elseif marginBottom and not marginTop then
			if where=="top" or where=="bottom" then
				local alert=native.showAlert("Warning","You can't use marginTop/marginBottom with otherMargin.",{"OK"}) 
			else
				object.y=display.contentHeight-(marginBottom + objectHeightHalf) 
			end			
		elseif marginTop and marginBottom then 
			local alert=native.showAlert("Warning","You can't use marginTop or marginBottom at the same time.",{"OK"}) 
		end
		if marginLeft and not marginRight then
			if where=="left" or where=="right" then
				local alert=native.showAlert("Warning","You can't use marginLeft/marginRight with otherMargin.",{"OK"}) 
			else		 
				object.x=marginLeft+objectWidthHalf
			end
		elseif marginRight and not marginLeft then
			if where=="left" or where=="right" then
				local alert=native.showAlert("Warning","You can't use marginLeft/marginRight with otherMargin.",{"OK"}) 
			else		 
		 		object.x=display.contentWidth-(marginRight+objectWidthHalf)
		 	end
		elseif marginLeft and marginRight then 
			local alert=native.showAlert("Warning","You can't use marginLeft or marginRight at the same time.",{"OK"}) 
		end				
		if xPos then object.x=xPos end
		if yPos then object.y=yPos end
	end
end
